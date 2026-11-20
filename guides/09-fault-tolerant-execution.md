# 09. Fault-tolerant execution

Make queries survive worker loss by retrying queries or tasks, with intermediate data spooled to S3. Trino 484, chart 1.45.0. Upstream page: [admin/fault-tolerant-execution.md](../upstream/trino/docs/admin/fault-tolerant-execution.md).

## Choose a retry policy

| | `NONE` (default) | `QUERY` | `TASK` |
|---|---|---|---|
| On worker failure | Query fails | Whole query is retried | Only failed tasks are retried |
| Exchange manager | Not used | Optional. Needed for result sets above `exchange.deduplication-buffer-size` (default `32MB`). | Required |
| Best for | Lowest latency | Many short queries | Large batch queries, spot capacity |
| Latency cost | None | Small | Noticeable for short queries, because every stage boundary goes through storage |
| Memory limits | `query.max-memory*` apply | Apply | `query.max-memory*` do not apply. Tasks are bin-packed by estimated memory. |

Upstream recommends a dedicated cluster with `TASK` for batch work, separate from a cluster that serves short interactive queries. Route between them with [Trino Gateway](15-trino-gateway.md).

The policy is also a session property, `retry_policy`. The configuration property `retry-policy.allowed` (default `NONE`, `QUERY`, `TASK`) limits which values a session may choose. A cluster can default to `NONE` and let batch clients opt in per session, provided an exchange manager is configured.

## Connector support

Retries only work with connectors that implement them. In 484 the upstream page lists BigQuery, Delta Lake, Hive, Iceberg, MariaDB, MongoDB, MySQL, Oracle, PostgreSQL, Redshift and SQL Server. A query touching another connector under a retry policy fails with "This connector does not support query retries".

## Exchange manager on S3

`exchange-manager.properties` on the coordinator and all workers:

```properties
exchange-manager.name=filesystem
exchange.base-directories=s3://my-trino-exchange
exchange.s3.region=us-east-1
```

Helm values:

```yaml
server:
  exchangeManager:
    name: filesystem
    baseDir:
      - s3://my-trino-exchange
additionalExchangeManagerProperties:
  - exchange.s3.region=us-east-1
additionalConfigProperties:
  - retry-policy=TASK
```

`server.exchangeManager.baseDir` is a list. The chart joins it with commas.

| Property | Default | Meaning |
|---|---|---|
| `exchange.base-directories` | required | One or more locations. Several buckets spread the request load. |
| `exchange.s3.region` | unset | Bucket region |
| `exchange.s3.endpoint` | unset | Custom endpoint |
| `exchange.s3.iam-role` | unset | Role to assume |
| `exchange.s3.external-id` | unset | External ID for that role |
| `exchange.s3.max-error-retries` | `10` | |
| `exchange.s3.upload.part-size` | `5MB` | Multipart part size |
| `exchange.s3.path-style-access` | `false` | |
| `exchange.sink-buffer-pool-min-size` | `10` | |
| `exchange.sink-buffers-per-partition` | `2` | |
| `exchange.sink-max-file-size` | `1GB` | |
| `exchange.source-concurrent-readers` | `4` | |
| `exchange.max-page-storage-size` | `16MB` | |

All exchange properties from source: [plugin/trino-exchange-filesystem](reference/config-properties/plugin--trino-exchange-filesystem.md).

These `exchange.s3.*` keys are separate from the `s3.*` keys of catalogs. Catalog S3 settings do not apply to the exchange.

### Bucket setup

- A dedicated bucket, or several, in the same region as the cluster. Not shared with table data or with another cluster.
- IAM for the Trino ServiceAccount role on the bucket: `s3:GetObject`, `s3:PutObject`, `s3:DeleteObject`, `s3:ListBucket`, `s3:GetBucketLocation`, `s3:AbortMultipartUpload`, `s3:ListBucketMultipartUploads`, `s3:ListMultipartUploadParts`. Without static keys, the exchange uses the AWS default credentials chain, so the pod role applies. The upstream table words the access keys as required. The source falls back to the default chain when they are absent, and the Data on EKS stack runs that way.
- A lifecycle rule that expires objects after a day or two and aborts incomplete multipart uploads. Trino cleans up after finished queries. Crashes leave files behind.
- No versioning. It multiplies storage cost for data that lives minutes.
- If queries fail with "Please reduce your request rate", add buckets to `exchange.base-directories`.

Trino encrypts spooled data with a per-query key by default (`fault-tolerant-execution.exchange-encryption-enabled=true`).

A local directory also works as a base directory, for a single node or a shared file system. It is no use for surviving node loss on Kubernetes.

## Settings for `TASK`

```properties
retry-policy=TASK
task.low-memory-killer.policy=total-reservation-on-blocked-nodes
```

Tuning knobs, all in `config.properties`:

| Property | Default | Meaning |
|---|---|---|
| `task-retry-attempts-per-task` | `4` | Retries of one task before the query fails |
| `query-retry-attempts` | `4` | Retries under `QUERY` |
| `retry-initial-delay` | `10s` | |
| `retry-max-delay` | `1m` | |
| `retry-delay-scale-factor` | `2.0` | |
| `fault-tolerant-execution-task-memory` | `5GB` | Initial memory estimate per task for bin-packing. Session: `fault_tolerant_execution_task_memory`. |
| `fault-tolerant-execution-max-partition-count` | `50` | Partitions for distributed joins and aggregations |
| `fault-tolerant-execution-min-partition-count` | `4` | |
| `fault-tolerant-execution-standard-split-size` | `64MB` | |
| `fault-tolerant-execution-max-task-split-count` | `2048` | |
| `fault-tolerant-execution-arbitrary-distribution-compute-task-target-size-min` and `-max` | `512MB`, `50GB` | Task sizing for scans |
| `fault-tolerant-execution-hash-distribution-compute-task-target-size` | `512MB` | Task sizing for joins and aggregations |
| `max-tasks-waiting-for-node-per-query` | `50` | Back-pressure on scheduling |
| `exchange.deduplication-buffer-size` | `32MB` | Coordinator buffer for results |

More exist. The source-derived list for `QueryManagerConfig` has every `fault-tolerant-execution-*` key with its description: [core/trino-main](reference/config-properties/core--trino-main.md#querymanagerconfig).

### Memory under `TASK`

Each task starts with an estimate of `fault-tolerant-execution-task-memory`. A worker runs as many tasks as fit in its pool. A task that fails for lack of memory is retried with a larger reservation, up to a full node. Consequences:

- Workers need a general pool comfortably larger than the task memory estimate. A worker with a 20GB pool runs about four tasks at the 5GB default.
- Lower the estimate for clusters of small workers or for many light queries. Raise it when tasks routinely fail and retry on memory.
- A single task still cannot exceed one worker. Skewed joins need bigger workers or a better plan.

## Settings for `QUERY`

```properties
retry-policy=QUERY
```

Add the exchange manager as above so large result sets are covered too. Without it, a query whose final result exceeds `exchange.deduplication-buffer-size` is not retried.

## What retries do not cover

- Coordinator failure. All queries fail when the coordinator restarts.
- User errors: syntax, permissions, exceeded limits. These fail at once.
- Connectors without retry support.
- Side effects outside Trino's control. Writes through supported connectors are made safe by the connector.

## Interaction with graceful shutdown and autoscaling

Fault-tolerant execution and graceful shutdown complement each other. Graceful shutdown avoids the retry when Kubernetes gives enough notice. Retries cover the cases where it does not: spot reclaim, node failure, out-of-memory kills.

With `TASK` you can run workers on spot capacity and scale in aggressively. Keep the coordinator on on-demand capacity.

## Verify

```bash
kubectl -n trino exec deploy/trino-coordinator -- cat /etc/trino/exchange-manager.properties
kubectl -n trino exec deploy/trino-coordinator -- grep retry-policy /etc/trino/config.properties
```

```sql
SHOW SESSION LIKE 'retry_policy';
```

Then run a query that takes a few minutes and delete a worker pod while it runs:

```bash
kubectl -n trino delete pod <a-worker-pod> --grace-period=0 --force
```

The query should finish. During the run the exchange bucket fills with objects under a per-query prefix, and they disappear after the query ends. The Data on EKS walkthrough of the same test is in [infra.md, Example 3](../upstream/data-on-eks/website/docs/datastacks/databases/trino-on-eks/infra.md).

## Metrics

Expose exchange statistics through `openmetrics.jmx-object-names`:

```properties
openmetrics.jmx-object-names=trino.plugin.exchange.filesystem:name=FileSystemExchangeStats|trino.plugin.exchange.filesystem.s3:name=S3FileSystemExchangeStorageStats
```

## Example

[examples/values-fault-tolerant.yaml](examples/values-fault-tolerant.yaml)
