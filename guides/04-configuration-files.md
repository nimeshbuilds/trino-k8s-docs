# 04. Configuration files

Every file Trino reads from `/etc/trino`, what belongs in it, and where the complete property list lives. Trino 484.

For the Helm value that writes each file, see guide [03](03-helm-values-to-config-files.md).

## Rules that apply to every file

- Format is Java properties: `key=value`, one per line, `#` for comments.
- Trino rejects unknown keys at startup. The error names the key.
- `${ENV:NAME}` is replaced with the environment variable `NAME` in any properties file. Other secret providers are described in [security/secrets.md](../upstream/trino/docs/security/secrets.md).
- Relative paths resolve against the data directory, where the launcher links `etc` and `plugin`. Use absolute paths such as `/etc/trino/rules.json` to avoid surprises.
- Value types: data sizes like `512MB`, `10GB`. Durations like `30s`, `5m`, `2h`, `1d`. Booleans `true` and `false`. Types are defined in [admin/properties.md](../upstream/trino/docs/admin/properties.md).
- Changes need a restart unless a file has its own refresh period.

## Lookup order for any property

1. [reference/documented-properties.md](reference/documented-properties.md) for the documented default, type and session property.
2. The upstream page for the feature, linked from [19-upstream-doc-map.md](19-upstream-doc-map.md), for meaning and advice.
3. [reference/config-properties/README.md](reference/config-properties/README.md) for the A to Z index generated from source, including undocumented properties, legacy names and removed names.

## node.properties

| Property | Meaning |
|---|---|
| `node.environment` | Cluster name. Must match on all nodes. Lowercase alphanumerics and `_`. |
| `node.id` | Unique per node. Optional. Generated when missing. |
| `node.data-dir` | Data directory. Logs go under it unless redirected. |
| `plugin.dir` | Plugin directory. The chart sets `/usr/lib/trino/plugin`. |
| `node.internal-address` | Address other nodes use to reach this node. Rarely needed on Kubernetes, where the pod IP is detected. |

## jvm.config

One JVM flag per line. No shell quoting. The chart writes the list shown in [reference/chart-templates.md](reference/chart-templates.md#jvmconfig).

Rules:

- Trino 484 requires Java 25. The image bundles it.
- Use G1. Keep `-XX:+ExitOnOutOfMemoryError` so Kubernetes restarts a node that ran out of heap.
- Heap is `-Xmx`, or a percentage through `-XX:MaxRAMPercentage`. Guide [05](05-memory-and-jvm-sizing.md) has the numbers.
- The JVM temporary directory must allow code execution. If `/tmp` is mounted `noexec`, add `-Djava.io.tmpdir=<path>`.
- Heap dumps on out-of-memory go to the working directory by default. Set `-XX:HeapDumpPath=<path>` to a volume with room, or replace the flag with `-XX:-HeapDumpOnOutOfMemoryError` when the pod has little ephemeral storage.

## config.properties

The main server file. Minimal content:

```properties
# coordinator
coordinator=true
node-scheduler.include-coordinator=false
http-server.http.port=8080
discovery.uri=http://trino:8080

# worker
coordinator=false
http-server.http.port=8080
discovery.uri=http://trino:8080
```

The properties most deployments touch, with documented defaults:

| Area | Property | Default | Notes |
|---|---|---|---|
| Memory | `query.max-memory` | `20GB` | User memory per query across the cluster. Chart default is `4GB`. |
| Memory | `query.max-total-memory` | 2 x `query.max-memory` | Includes revocable memory |
| Memory | `query.max-memory-per-node` | 30% of heap | Chart default is `1GB` |
| Memory | `memory.heap-headroom-per-node` | 30% of heap | Untracked allocations |
| Memory | `query.low-memory-killer.policy` | `total-reservation-on-blocked-nodes` | What to kill when nodes run out of memory |
| Limits | `query.max-execution-time` | `100d` | Wall time executing. Session: `query_max_execution_time`. |
| Limits | `query.max-run-time` | `100d` | Includes queueing and planning |
| Limits | `query.max-planning-time` | `10m` | |
| Limits | `query.max-cpu-time` | effectively unlimited | |
| Limits | `query.max-scan-physical-bytes` | unset | Cap on bytes read per query |
| Limits | `query.max-stage-count` | `150` | |
| Limits | `query.max-length` | `1,000,000` | Characters of SQL text |
| Clients | `query.client.timeout` | `5m` | Abandon a query when the client stops polling |
| History | `query.max-history` | `100` | Finished queries kept for the UI |
| History | `query.min-expire-age` | `15m` | |
| Scheduling | `node-scheduler.include-coordinator` | `true` | Chart sets `false` |
| Scheduling | `node-scheduler.max-splits-per-node` | `256` | |
| Tasks | `task.concurrency` | Physical CPUs, 2 to 32 | Local parallelism for joins and aggregations |
| Tasks | `task.max-worker-threads` | CPUs x 2 | |
| Optimizer | `join-distribution-type` | `AUTOMATIC` | Session: `join_distribution_type` |
| Exchange | `exchange.compression-codec` | `NONE` | `LZ4` or `ZSTD` trade CPU for network |
| Resilience | `retry-policy` | `NONE` | `QUERY` or `TASK`. See guide [09](09-fault-tolerant-execution.md). |
| Shutdown | `shutdown.grace-period` | `2m` | See guide [08](08-scaling-and-graceful-shutdown.md) |
| Catalogs | `catalog.management` | `static` | `dynamic` enables `CREATE CATALOG` and `DROP CATALOG` |
| Security | `internal-communication.shared-secret` | unset | Required with any authentication |
| Security | `http-server.authentication.type` | unset | See guide [11](11-security.md) |
| Network | `http-server.process-forwarded` | `false` | Trust `X-Forwarded-*` from a TLS-terminating proxy |
| Protocol | `protocol.spooling.enabled` | `true` | Needs a spooling manager to take effect |
| Metrics | `openmetrics.jmx-object-names` | unset | Extra JMX beans for `/metrics` |

The full documented set is split across the upstream `admin/properties-*.md` pages, indexed in [reference/documented-properties.md](reference/documented-properties.md).

## log.properties

Logger name, equals sign, level. Levels are `DEBUG`, `INFO`, `WARN`, `ERROR`.

```properties
io.trino=INFO
io.trino.plugin.iceberg=DEBUG
io.trino.filesystem.s3=DEBUG
```

Log destination and format are `config.properties` keys, not `log.properties` keys:

| Property | Default | Meaning |
|---|---|---|
| `log.console-format` | `TEXT` | Format of standard output. `TEXT` or `JSON`. |
| `log.path` | unset | Extra file or TCP destination |
| `log.format` | `TEXT` | Format for the `log.path` destination |
| `log.max-size`, `log.max-total-size`, `log.compression` | `100MB`, `1GB`, `GZIP` | Rotation of the `log.path` file |
| `http-server.log.enabled` | `true` | HTTP request log |
| `http-server.log.path` | `var/log/http-request.log` | Request log file under the data directory |
| `http-server.log.max-history` | `15` | Rotated request log files kept |

See [admin/properties-logging.md](../upstream/trino/docs/admin/properties-logging.md) and [admin/logging.md](../upstream/trino/docs/admin/logging.md).

On Kubernetes the launcher runs in the foreground and the server logs to standard output, which is what you want. Set `log.console-format=JSON` when your log pipeline parses JSON. The HTTP request log is written to a file inside the pod, which counts against ephemeral storage. Set `http-server.log.enabled=false` if you do not collect it.

## catalog/&lt;name&gt;.properties

One file per catalog. The file name without `.properties` is the catalog name. The first line picks the connector:

```properties
connector.name=iceberg
```

Everything else is specific to the connector and to the libraries it uses (file system, metastore, file formats). Identical files must exist on every node.

- Connector names and their documentation and source-derived property pages: [reference/plugins-and-connectors.md](reference/plugins-and-connectors.md).
- Data lake catalogs: guide [06](06-catalogs-glue-s3.md).
- Dynamic catalog management: [admin/properties-catalog.md](../upstream/trino/docs/admin/properties-catalog.md). With `catalog.management=dynamic` and `catalog.store=file`, Trino writes catalog files into `catalog.config-dir`. On Kubernetes that directory is a read-only ConfigMap mount, so dynamic catalogs need a writable directory and are lost on restart unless you persist it. Static catalogs from Helm values are the simpler model.

## exchange-manager.properties

Storage for intermediate data under fault-tolerant execution. Required for `retry-policy=TASK`.

```properties
exchange-manager.name=filesystem
exchange.base-directories=s3://my-exchange-bucket
exchange.s3.region=us-east-1
```

Guide [09](09-fault-tolerant-execution.md). Source-derived properties: [plugin/trino-exchange-filesystem](reference/config-properties/plugin--trino-exchange-filesystem.md).

## spooling-manager.properties

Storage for client result sets under the spooling protocol.

```properties
spooling-manager.name=filesystem
fs.s3.enabled=true
fs.location=s3://my-spooling-bucket/trino
s3.region=us-east-1
```

Plus, in `config.properties`:

```properties
protocol.spooling.enabled=true
protocol.spooling.shared-secret-key=${ENV:SPOOLING_KEY}
```

The key is a base64-encoded 256-bit secret. With the default `protocol.spooling.retrieval-mode=STORAGE`, clients fetch segments straight from the storage location and must be able to reach it. Other modes route the download through the cluster. Read [client/client-protocol.md](../upstream/trino/docs/client/client-protocol.md) and [admin/properties-client-protocol.md](../upstream/trino/docs/admin/properties-client-protocol.md) before enabling it. With the chart, supply this file through `coordinator.additionalConfigFiles` and `worker.additionalConfigFiles`.

## resource-groups.properties

Coordinator only.

```properties
resource-groups.configuration-manager=file
resource-groups.config-file=/etc/trino/resource-groups/resource-groups.json
```

Or `resource-groups.configuration-manager=db` with `resource-groups.config-db-url`, `resource-groups.config-db-user`, `resource-groups.config-db-password`. Guide [12](12-resource-groups-and-session-properties.md).

## session-property-config.properties

Coordinator only.

```properties
session-property-config.configuration-manager=file
session-property-manager.config-file=/etc/trino/session-property-config.json
```

Guide [12](12-resource-groups-and-session-properties.md).

## access-control.properties

System access control. Put it on every node when workers must authorize requests, for example the graceful shutdown call or metrics scrapes on workers.

```properties
access-control.name=file
security.config-file=/etc/trino/access-control/rules.json
security.refresh-period=60s
```

Available names: `default`, `allow-all`, `read-only`, `file`, `opa`, `ranger`. Use `access-control.config-files` in `config.properties` to chain several. Guide [11](11-security.md).

## Authentication helper files

| File | First line | Purpose |
|---|---|---|
| `password-authenticator.properties` | `password-authenticator.name=file`, `ldap` or `salesforce` | Backing store for `PASSWORD` authentication |
| `group-provider.properties` | `group-provider.name=file` or `ldap` | Maps users to groups for access control and resource groups |
| `header-authenticator.properties` | `header-authenticator.name=<plugin>` | Custom header authentication |
| `certificate-authenticator.properties` | `certificate-authenticator.name=<plugin>` | Custom certificate mapping |

## event-listener.properties

```properties
event-listener.name=http
http-event-listener.connect-ingest-uri=https://collector.example.internal/trino
http-event-listener.log-completed=true
```

Bundled listeners: `http`, `kafka`, `mysql`, `openlineage`, and `http-server`. For several listeners at once, write one file each and list them in `event-listener.config-files` in `config.properties`. Pages: [event-listeners-http](../upstream/trino/docs/admin/event-listeners-http.md), [event-listeners-kafka](../upstream/trino/docs/admin/event-listeners-kafka.md), [event-listeners-mysql](../upstream/trino/docs/admin/event-listeners-mysql.md), [event-listeners-openlineage](../upstream/trino/docs/admin/event-listeners-openlineage.md).

## Cache manager files

Named by `cache-manager.config-files` in `config.properties`. One file per cache manager, each starting with `cache-manager.name=alluxio` or `cache-manager.name=memory`. Guide [07](07-caching.md).

## Files that are data, not properties

| File | Format | Used by |
|---|---|---|
| `resource-groups.json` | JSON | File resource group manager |
| `session-property-config.json` | JSON | File session property manager |
| `rules.json` | JSON | File-based system access control |
| `password.db` | `user:bcrypt-or-pbkdf2-hash` per line | File password authenticator |
| `group.db` | `group:user1,user2` per line | File group provider |
| Catalog-level rule files | JSON | Connector authorization, for example `hive.security=file` with `security.config-file` in the catalog |

Examples: [examples/resource-groups.json](examples/resource-groups.json), [examples/session-property-config.json](examples/session-property-config.json), [examples/access-control-rules.json](examples/access-control-rules.json).
