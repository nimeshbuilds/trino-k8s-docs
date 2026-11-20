# 17. Troubleshooting

Symptom, likely cause, how to confirm, fix. Trino 484, chart 1.45.0. Commands assume release `trino` in namespace `trino`.

## First five commands

```bash
kubectl -n trino get pods -o wide
kubectl -n trino describe pod <pod>                 # events, last state, exit code
kubectl -n trino logs <pod> --tail=200              # add --previous after a restart
kubectl -n trino get events --sort-by=.lastTimestamp | tail -30
kubectl -n trino exec deploy/trino-coordinator -- trino --execute \
  "SELECT node_id, coordinator, state FROM system.runtime.nodes"
```

For a failed query, start from its error name and type. All engine error names are listed in [reference/error-codes.md](reference/error-codes.md). The Web UI query page and the client both show them.

## Pods do not start

| Symptom | Cause | Confirm | Fix |
|---|---|---|---|
| `CrashLoopBackOff`, log ends with `Configuration property '...' was not used` | Unknown, misspelled, removed or misplaced property | First `ERROR` block in the log names the property | Look it up in [reference/config-properties/README.md](reference/config-properties/README.md). Move it to the right file or remove it. |
| Log shows `Defunct property` or `has been removed` | Property removed in this version | Same | Use the replacement named in the release notes |
| `Invalid configuration property` with a value error | Wrong type or unit | Log | Data sizes need units like `GB`. Durations need `s`, `m`, `h`. |
| `The sum of max query memory per node ... and heap headroom ... cannot be larger than the available heap memory` | Memory settings exceed the heap | Log | Lower `maxMemoryPerNode` or headroom, or raise the heap. Guide [05](05-memory-and-jvm-sizing.md). |
| `No factory for connector '...'. Available factories: [...]` | Wrong `connector.name` or plugin missing from the image | Log | Names are in [reference/plugins-and-connectors.md](reference/plugins-and-connectors.md) |
| `Error: Could not create the Java Virtual Machine` or unrecognised VM option | Bad flag in `additionalJVMConfig` | Log | Remove or fix the flag |
| Pod `Pending` | No node fits the requests, or taints | `kubectl describe pod` | Fix requests, node pool, tolerations |
| `OOMKilled` right at start | Heap larger than the pod limit | `kubectl describe pod`, exit code 137 | Heap at 70 to 85 percent of the limit |
| Startup probe fails, pod restarts while still loading | Many catalogs or slow metastore | Log shows steady progress, no error | Raise `coordinator.startupProbe.failureThreshold` |
| `helm upgrade` fails before anything changes | Chart validation | Helm error text | [reference/chart-templates.md](reference/chart-templates.md#render-time-failures) |
| `exec` probe or hook fails with `command not found` | Script uses a tool the hardened image lacks | Pod events | Use shell built-ins and `curl` only |

## Workers do not join

| Symptom | Cause | Fix |
|---|---|---|
| `system.runtime.nodes` shows only the coordinator | Workers cannot reach `discovery.uri` | Check the coordinator Service and endpoints, NetworkPolicy, DNS. From a worker: `curl -s http://trino:8080/v1/info`. |
| Workers start, never appear, coordinator logs authentication errors for internal requests | Shared secret differs or is missing on some nodes | Same `internal-communication.shared-secret` everywhere |
| Workers appear then vanish | Different `node.environment`, or different Trino version | Same values on all nodes. One image tag. |
| Workers flap between active and inactive | Long GC pauses, CPU throttling, network loss | GC logs, remove CPU limits, check node health |
| Queries fail with "Insufficient active worker nodes" | Fewer workers than `query-manager.required-workers` for longer than the max wait | Scale up, or fix autoscaling. Guide [08](08-scaling-and-graceful-shutdown.md). |

## Queries fail

| Error | Meaning | Fix |
|---|---|---|
| `EXCEEDED_LOCAL_MEMORY_LIMIT` | One query hit `query.max-memory-per-node` on a worker | Raise the limit within the heap budget, add memory, or fix the query: filter earlier, avoid cross joins, check join order and statistics |
| `EXCEEDED_GLOBAL_MEMORY_LIMIT` | Hit `query.max-memory` or `query.max-total-memory` | Raise it, or reduce the query |
| `CLUSTER_OUT_OF_MEMORY` | The low-memory killer chose this query | Limit concurrency of heavy queries with resource groups. Add workers. |
| `EXCEEDED_TIME_LIMIT` | `query.max-execution-time`, `query.max-run-time` or `query.max-planning-time` | Raise per session if justified |
| `EXCEEDED_CPU_LIMIT`, `EXCEEDED_SCAN_LIMIT` | CPU or scan cap | Same |
| `QUERY_QUEUE_FULL` | Resource group queue is full | Raise `maxQueued`, add capacity, or slow the client |
| `QUERY_REJECTED` | No resource group selector matched | Add a catch-all selector |
| `REMOTE_TASK_ERROR`, `REMOTE_HOST_GONE`, `PAGE_TRANSPORT_ERROR`, `PAGE_TRANSPORT_TIMEOUT`, `TOO_MANY_REQUESTS_FAILED` | A worker died, was removed, or stopped answering | Find which pod restarted and why. Enable graceful shutdown and a PodDisruptionBudget. Use fault-tolerant execution for long queries. |
| `SERVER_SHUTTING_DOWN` | The node was shutting down | Retry. Check why it shut down. |
| `NO_NODES_AVAILABLE` | No worker can take the task | Workers missing, or all excluded |
| `ADMINISTRATIVELY_KILLED` | Someone killed it | Query events show who |
| `ABANDONED_QUERY` | Client stopped polling for longer than `query.client.timeout` | Client or network problem between client and coordinator. Check proxy idle timeouts. |
| "This connector does not support query retries" | Retry policy with an unsupported connector | Set `retry_policy` to `NONE` for that session, or query through a supported connector |
| `PERMISSION_DENIED`, "Access Denied" | Trino access control, connector security, or the cloud provider | The message says which. An S3 or Glue denial names the AWS action. |

A query that works for one user and not another is almost always access control or a session property set by a rule.

## Catalogs and storage

| Symptom | Cause | Fix |
|---|---|---|
| Catalog missing from `SHOW CATALOGS`, or log line `Catalog '...' failed to initialize and is disabled` | File not rendered, or failed to load | Check the catalog ConfigMap and the coordinator log |
| `No factory for location: s3://...` | No file system enabled in the catalog | `fs.s3.enabled=true` |
| S3 `AccessDenied` or 403 | IAM role lacks the action, wrong bucket policy, KMS key policy, or the pod is not using the role you think | From a pod, check the identity. The AWS CLI is not in the image, so inspect the environment: `env \| grep AWS_` shows the web identity token file or container credentials URI. Then check CloudTrail for the denied call. |
| Works on the coordinator, fails on workers, or the reverse | Rare with one ServiceAccount. Happens with node-level instance profiles when Pod Identity or IRSA is not active on some nodes. | Confirm the ServiceAccount on both Deployments and the association or annotation |
| S3 301 or region errors | `s3.region` wrong or missing, bucket elsewhere | Set the right region, or `s3.cross-region-access=true` |
| Glue `ThrottlingException` | Too many metastore calls | Metastore cache. Guide [07](07-caching.md). |
| Glue `AccessDeniedException` naming Lake Formation | Lake Formation permissions on the catalog | Grant the Trino role in Lake Formation |
| Table exists in Glue, query returns no rows | Partitions not registered, wrong location, or files under subdirectories | `SELECT * FROM "t$partitions"`, `sync_partition_metadata`, `hive.recursive-directories=true` |
| New files not visible | Listing or metastore cache | `CALL <catalog>.system.flush_metadata_cache()`, shorter TTLs |
| `Query over table '...' can potentially read more than N partitions` | `hive.max-partitions-per-scan` | Add a partition filter. Raise the limit only with care. |
| Schema mismatch reading Parquet | Columns matched by name against a changed schema | Check `hive.parquet.use-column-names`, and the table definition against the files |
| Iceberg table from another engine not readable | Format version or feature not supported, or wrong catalog type | Check `iceberg.catalog.type` and the Iceberg page |
| "Please reduce your request rate" from S3 | Prefix hot spot, usually the exchange bucket | More buckets in `exchange.base-directories`. For data, spread prefixes. |

## Performance

| Symptom | Check | Typical cause |
|---|---|---|
| Long time in `PLANNING` | Query detail, planning time | Many partitions, cold metastore, missing statistics, huge `IN` lists |
| Long `QUEUED` | Resource group view | Concurrency limit reached. Expected behaviour. |
| Slow scans | `EXPLAIN ANALYZE`, input rate per stage | No partition pruning, tiny files, cross-region reads, too few workers |
| One stage slow, one task far behind | Stage view, per-task rows | Data skew in a join or aggregation key |
| Join runs out of memory or is slow | `EXPLAIN` join order and distribution | Missing statistics. Run `ANALYZE`. Try `SET SESSION join_distribution_type = 'PARTITIONED'` or `'BROADCAST'`. |
| Everything slow at peak | Cluster CPU, blocked nodes, GC | Under-provisioned. Scale out, or shape load with resource groups. |
| Coordinator CPU pinned | Coordinator metrics | Too many concurrent queries or huge result sets through the coordinator. Scale the coordinator up. Consider the spooling protocol. |
| Short queries slower after enabling `TASK` retries | Compare with `retry_policy=NONE` | Expected overhead. Use `QUERY` or a separate cluster. |

Tools:

```sql
EXPLAIN SELECT ...;                    -- plan with estimates
EXPLAIN (TYPE DISTRIBUTED) SELECT ...; -- fragments and exchanges
EXPLAIN ANALYZE SELECT ...;            -- runs the query, shows actual rows, CPU and memory per operator
EXPLAIN ANALYZE VERBOSE SELECT ...;
SHOW STATS FOR table_name;
SHOW SESSION;
```

Pages: [sql/explain.md](../upstream/trino/docs/sql/explain.md), [sql/explain-analyze.md](../upstream/trino/docs/sql/explain-analyze.md), [optimizer/statistics.md](../upstream/trino/docs/optimizer/statistics.md), [optimizer/cost-based-optimizations.md](../upstream/trino/docs/optimizer/cost-based-optimizations.md), [optimizer/pushdown.md](../upstream/trino/docs/optimizer/pushdown.md), [admin/dynamic-filtering.md](../upstream/trino/docs/admin/dynamic-filtering.md).

## Scaling and shutdown

| Symptom | Cause | Fix |
|---|---|---|
| Queries fail on every scale-in | No graceful shutdown, or termination grace period too short | Guide [08](08-scaling-and-graceful-shutdown.md) |
| Worker pods stay `Terminating` for a long time | Working as designed: waiting for tasks | Wait, or lower `query.max-execution-time` and the grace period together |
| `preStop` hook fails with 401 or 403 | Shutdown user not allowed to write system information, or workers require authentication | Fix the worker access control rule or the hook |
| HPA or KEDA never scales | No metrics, wrong metric name, wrong target kind, missing CPU requests | `kubectl describe hpa`, `kubectl describe scaledobject`, query Prometheus by hand |
| KEDA shows the metric but scales to max and stays | Threshold too low, or metric never falls, for example a memory target | Revisit the trigger |
| Scale from zero never happens | The metric only exists on running workers, or the coordinator is not scraped | Use a coordinator metric such as `RequiredWorkers` or queued queries |

## Security

| Symptom | Cause | Fix |
|---|---|---|
| `Password not allowed for insecure authentication` | Password sent over HTTP | HTTPS on Trino, or `http-server.process-forwarded=true` behind a TLS proxy |
| Clients get `http://` URLs in `nextUri` and hang | Proxy terminates TLS but Trino does not process forwarded headers | `http-server.process-forwarded=true` |
| Web UI login loop | Web UI authentication type does not match the server's | Set `web-ui.authentication.type` |
| 403 on `/metrics` | Scrape user lacks `read` on `system_information` | Add the rule on coordinator and workers |
| Everything denied after adding rules | A present section denies whatever no rule allows. First match wins. | Add explicit allow rules. Order them from specific to general. |
| Rules change not picked up | No `refreshPeriod`, so a restart is needed | Set `accessControl.refreshPeriod` |

## Collecting evidence for a bug report

1. Trino version and chart version: `SELECT node_version FROM system.runtime.nodes`, `helm list -n trino`.
2. Query ID, the full error with stack trace from the Web UI, and the query JSON (the "JSON" link on the query page).
3. `EXPLAIN ANALYZE VERBOSE` output if the query can run.
4. Coordinator and affected worker logs around the time, with `--previous` if the pod restarted.
5. The rendered ConfigMaps, with secrets removed.
