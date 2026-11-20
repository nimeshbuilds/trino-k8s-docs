# 14. Upgrades and day-2 operations

Changing configuration, upgrading Trino and the chart, restarting safely, and routine tasks. Trino 484, chart 1.45.0.

## What a change costs

| Change | Restarts | Running queries |
|---|---|---|
| Worker-only value (worker resources, JVM, `server.workerExtraConfig`) | Workers, rolling | Survive with graceful shutdown or fault-tolerant execution |
| Anything in the coordinator ConfigMap, catalogs included | Coordinator, and workers when their files change too | All fail when the coordinator restarts |
| File-based access control rules with `refreshPeriod` | None | Unaffected. Reloaded in place. |
| Password or group Secret content | None | Reloaded by the file authenticator and group provider |
| Resource groups with the `db` manager | None | Unaffected. Polled. |
| Resource groups or session property JSON with the `file` manager | Coordinator | All fail |
| Worker replica count | None | Unaffected on scale-out. Scale-in needs graceful shutdown. |
| Trino version | Everything | All fail, unless you switch clusters behind a gateway |

Trino has no rolling coordinator restart and no mixed-version clusters. The coordinator and all workers must run the same version.

## Before any restart of the coordinator

```sql
SELECT state, count(*) FROM system.runtime.queries
WHERE state IN ('QUEUED','PLANNING','STARTING','RUNNING','FINISHING') GROUP BY 1;
```

Pick a quiet window, or drain first: stop schedulers, and stop admitting new queries by routing clients elsewhere. There is no built-in "stop accepting queries" switch on a coordinator. With Trino Gateway you deactivate the backend and wait for running queries to end.

## Upgrade procedure, single cluster

1. Read the release notes for every version between yours and the target. They are copied under [upstream/trino/docs/release](../upstream/trino/docs/release). Search them for `Breaking`, `removed`, `renamed`, `deprecated`, and for the connectors you use.
2. Check your properties against the target version. Removed properties stop the server from starting. The source-derived pages list removed names per class under "Removed properties", and legacy names in the Notes column: [reference/config-properties/README.md](reference/config-properties/README.md).
3. Check the Java requirement. Trino 484 needs Java 25. The image brings its own JDK, so this only matters for custom images.
4. Check client compatibility. Old JDBC drivers and CLIs generally work with newer servers, but new protocol features such as spooling need new clients.
5. Render and diff:

   ```bash
   helm repo update
   helm template trino trino/trino --version <new-chart> -n trino -f values.yaml > new.yaml
   helm get manifest trino -n trino > old.yaml
   diff -u old.yaml new.yaml | less
   ```

6. Apply in a test cluster with the same values and run a smoke suite: `SHOW CATALOGS`, a scan per catalog, a join, an insert if you write.
7. Upgrade production in a window:

   ```bash
   helm upgrade trino trino/trino -n trino --version <new-chart> -f values.yaml --wait --timeout 30m
   ```

8. Verify versions and health:

   ```sql
   SELECT node_version, coordinator, state, count(*) FROM system.runtime.nodes GROUP BY 1,2,3;
   ```

9. Roll back if needed with `helm rollback trino <revision> -n trino`. Trino keeps no state of its own, so a rollback is safe as long as nothing wrote table metadata in a format the old version cannot read. That matters for Iceberg and Delta Lake format upgrades, which are explicit table operations and never happen as a side effect of a server upgrade.

Chart version and Trino version are coupled through `appVersion`. Pin `image.tag` if you want to move them separately. Chart changes are listed in the `trinodb/charts` release notes on GitHub, which are not copied here.

## Upgrade procedure, blue-green behind a gateway

1. Install the new version as a second release, for example `trino-blue` and `trino-green`, with identical catalogs.
2. Register it in Trino Gateway as inactive. Test it directly.
3. Activate the new backend and deactivate the old one. New queries go to the new cluster. Queries already running stay on the old one, because the gateway remembers which backend owns each query ID.
4. When the old cluster has no running queries, uninstall it or scale its workers to zero as a standby.

This is the only way to upgrade without failing queries. See guide [15](15-trino-gateway.md).

## Changes in 483 and 484 that break older setups

Taken from the release notes copied in this repository: [release-483.md](../upstream/trino/docs/release/release-483.md), [release-484.md](../upstream/trino/docs/release/release-484.md). Read both in full before upgrading across them.

| Release | Change | Action |
|---|---|---|
| 483 | `s3.auth-type` added, with `DEFAULT`, `IAM_ROLE`, `WEB_IDENTITY`, `ANONYMOUS` | None by itself |
| 483 | Breaking: `s3.iam-role` requires `s3.auth-type=IAM_ROLE` | Add the auth type to every catalog that sets `s3.iam-role` |
| 483 | Breaking: `s3.use-web-identity-token-credentials-provider` removed | Use `s3.auth-type=WEB_IDENTITY` |
| 484 | One file system cache shared by all catalogs on a node, configured with `cache-manager.config-files` | See below |
| 484 | Breaking: `fs.cache.directories`, `fs.cache.max-sizes`, `fs.cache.max-disk-usage-percentages`, `fs.cache.ttl` and `fs.cache.page-size` must move from catalog files into the `alluxio` cache manager file | Follow guide [07](07-caching.md). Catalogs keep only `fs.cache.enabled`. Dashboards on the old cache JMX beans need new names. |
| 484 | Breaking: the Docker image uses a hardened minimal base without tools such as `grep` | Review custom `exec` probes, lifecycle hooks, init logic and derived images |
| 484 | Breaking: the `run-trino` script is gone from the image | Custom `node.properties` must set `node.id=${ENV:HOSTNAME}` to keep the hostname as node ID |
| 484 | Breaking: the ppc64le image is no longer published | Use `amd64` or `arm64` |
| 484 | Breaking: user names in `GRANT`, `REVOKE`, `SET AUTHORIZATION` and `CREATE SCHEMA` keep their case | Review authorization statements that relied on lowercasing |
| 484 | Breaking: several JSON function and interval behaviours changed | Read the General section of the 484 notes if queries use SQL/JSON functions or interval arithmetic |
| 484 | `query.max-output-data-size` added | Optional cap on result size |

Other naming facts for 484, from the documentation and source: the S3 switch is documented as `fs.s3.enabled`, with `fs.native-s3.enabled` accepted as a legacy name, and tracing is configured with `tracing.enabled` and `otel.exporter.endpoint`.

The chart writes its own `node.properties` without `node.id`, so each pod start generates a new node ID. To tie the ID to the pod name, add:

```yaml
additionalNodeProperties:
  - node.id=${ENV:HOSTNAME}
```

## Restart commands

```bash
kubectl -n trino rollout restart deploy/trino-worker        # rolling, honours graceful shutdown
kubectl -n trino rollout status deploy/trino-worker
kubectl -n trino rollout restart deploy/trino-coordinator   # fails running queries
kubectl -n trino delete pod <pod>                           # one node
```

A single stuck worker is best removed with a plain `kubectl delete pod`, which runs the `preStop` hook. `--force --grace-period=0` skips it.

## Take one worker out of service by hand

```bash
kubectl -n trino exec <worker-pod> -- \
  curl -s -X PUT -d '"SHUTTING_DOWN"' -H 'Content-type: application/json' \
  -H 'X-Trino-User: admin' http://localhost:8080/v1/info/state
```

The worker finishes its tasks and exits. The Deployment then replaces the pod. To shrink instead, lower the replica count first.

## Kill and inspect queries

```sql
CALL system.runtime.kill_query(query_id => '<id>', message => 'reason');
```

```bash
trino --server http://localhost:8080 --execute "SELECT query_id, user, state FROM system.runtime.queries WHERE state='RUNNING'"
```

The Web UI has a kill button on each query for users allowed to kill it.

## Catalog changes

Static catalogs come from the `catalogs` value. Adding, changing or removing one changes the catalog ConfigMap checksum and restarts coordinator and workers. Batch catalog changes into one upgrade.

With `catalog.management=dynamic`, `CREATE CATALOG` and `DROP CATALOG` work at runtime. The catalog store then has to be writable and durable, which the read-only ConfigMap mount is not. Treat dynamic catalogs on Kubernetes as an advanced setup. Page: [admin/properties-catalog.md](../upstream/trino/docs/admin/properties-catalog.md).

## Routine maintenance

| Task | How | Frequency |
|---|---|---|
| Iceberg compaction | `ALTER TABLE t EXECUTE optimize` | By write volume, often daily |
| Iceberg snapshot expiry | `ALTER TABLE t EXECUTE expire_snapshots(retention_threshold => '7d')` | Daily to weekly |
| Iceberg orphan file removal | `ALTER TABLE t EXECUTE remove_orphan_files(retention_threshold => '7d')` | Weekly |
| Hive statistics | `ANALYZE t` or with a partition filter | After large loads |
| Hive partition sync | `CALL system.sync_partition_metadata(...)` | When writers do not register partitions |
| Exchange bucket cleanup | S3 lifecycle rule | Always on |
| Certificate rotation | Replace the Secret, restart the coordinator if Trino terminates TLS | Before expiry |
| Shared secret rotation | Replace on all nodes at once. A mismatch splits the cluster. | Per policy, in a window |
| Password file | Update the Secret. Reloaded automatically. | As needed |

Run maintenance statements from your scheduler under a dedicated user and resource group.

## GitOps

The chart works with Argo CD and Flux as a plain Helm release. Notes:

- Pin chart version and image tag in Git.
- The checksum annotations make config changes roll pods, so a merged change restarts the coordinator. Gate merges accordingly.
- With autoscaling, `replicas` is absent from the worker Deployment, so the controller does not fight the autoscaler.
- Keep secrets out of the values file. Reference Secrets managed by your secret tooling.
- Argo CD example from Data on EKS: [argocd-applications/trino.yaml](../upstream/data-on-eks/infra/terraform/argocd-applications/trino.yaml). It pins chart 1.41.0, older than the chart documented here.

## Backups

Nothing in Trino needs backing up. Back up what defines it: the values file, Secrets, resource group database if you use the `db` manager, the gateway database, and the Glue catalog and S3 data through their own means.

## Capacity reviews

Review monthly from query events: peak concurrent queries, queue time percentiles per resource group, peak memory per query, bytes scanned per user and source, failure counts by error code. Those five numbers tell you whether to add workers, raise limits, fix queries, or split workloads onto separate clusters.
