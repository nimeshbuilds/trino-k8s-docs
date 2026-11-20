# 13. Observability

Metrics, logs, traces, query events and the SQL views that show what a Trino 484 cluster is doing. Chart 1.45.0.

## What exists

| Source | Gives you | Enable with |
|---|---|---|
| OpenMetrics endpoint `/metrics` on every node | All Trino JMX beans in Prometheus format | Built in. Scrape it. |
| JMX exporter sidecar | JVM and Trino beans through the Prometheus JMX exporter | `jmx.enabled` and `jmx.exporter.enabled` |
| Web UI at `/ui/` | Live and recent queries, plans, stage and operator statistics, worker list | Built in, coordinator |
| `system.runtime.*` tables | Queries, tasks, nodes, transactions through SQL | Built in |
| `jmx` catalog | Any JMX bean of any node through SQL | Add a catalog with `connector.name=jmx` |
| Event listeners | One event per query created, completed and split completed, pushed to HTTP, Kafka, MySQL or OpenLineage | `eventListenerProperties` |
| OpenTelemetry traces | Spans for planning, scheduling, connector calls | `tracing.enabled=true` |
| Logs | Server log on standard output, HTTP request log in a file | Built in |

## Metrics with the chart ServiceMonitor

Needs the Prometheus Operator CRDs.

```yaml
serviceMonitor:
  enabled: true
  labels:
    release: kube-prometheus-stack     # whatever your Prometheus selects on
  interval: 30s
  openMetrics:
    enabled: true
    username: prometheus
```

What you get: one ServiceMonitor for the coordinator Service and one for the headless worker Service, each scraping `/metrics` on the `http` port with basic authentication. When a JMX exporter sidecar runs, its port is scraped too. `serviceMonitor.coordinator` and `serviceMonitor.worker` override per role, for example `serviceMonitor.worker.enabled: false`.

The chart default label is `prometheus: kube-prometheus`. If your Prometheus selects ServiceMonitors by another label, the objects exist and nothing is scraped. Check `kubectl get prometheus -A -o yaml | grep -A3 serviceMonitorSelector`.

### Authentication for the scrape

`/metrics` is a management endpoint. It is protected like the rest of the API.

| Cluster setup | What to configure |
|---|---|
| No authentication | Nothing. Any user name works, and the default `admin` is fine. |
| Authentication with TLS terminated at a proxy (`http-server.process-forwarded=true`) | Plain HTTP requests to the coordinator are refused, the scrape included. Set `management.user=prometheus` in `additionalConfigProperties`, which serves management endpoints over HTTP as that fixed user. |
| Authentication with HTTPS on the coordinator | The chart scrapes the `https` port. Put credentials in `serviceMonitor.coordinator.openMetrics.password`, or a Secret with `username` and `password` keys named in `serviceMonitor.coordinator.openMetrics.authSecret`. Add `serviceMonitor.tlsConfig` if Prometheus cannot verify the certificate. |
| Any, with file-based access control | The scrape user needs `read` on `system_information`, on the coordinator and on the workers. Without the rule the scrape gets a 403. |

Workers get no authentication type from the chart. They accept the user name alone and reject a password. For that reason the chart fails to render when `serviceMonitor.openMetrics.password` is set at the shared level. The background is in guide [11](11-security.md).

```yaml
accessControl:
  type: configmap
  rules:
    rules.json: |-
      {"system_information": [{"user": "prometheus", "allow": ["read"]}]}
```

That minimal file denies nothing else, because absent sections grant access. Merge the rule into your real rules file.

### Adding beans

Trino exports the beans it registers. JVM beans and some library beans need to be named:

```yaml
additionalConfigProperties:
  - openmetrics.jmx-object-names=java.lang:*|trino.plugin.exchange.filesystem:name=FileSystemExchangeStats|io.trino.blob.cache.alluxio:*
```

Separate object names with `|`. Use `:*` for a whole domain, `name=` or `type=` to select.

### Metric naming

OpenMetrics names are built from the JMX object name and attribute, with dots turned into underscores and key names kept:

| JMX | OpenMetrics name |
|---|---|
| `trino.execution:name=QueryManager`, attribute `RunningQueries` | `trino_execution_name_QueryManager_RunningQueries` |
| `trino.memory:type=ClusterMemoryPool,name=general`, attribute `FreeDistributedBytes` | `trino_memory_type_ClusterMemoryPool_name_general_FreeDistributedBytes` |
| `io.airlift.http.client:type=HttpClient,name=ForDiscoveryClient`, attribute `CurrentResponseProcessTime.Min` | `io_airlift_http_client_type_HttpClient_name_ForDiscoveryClient_CurrentResponseProcessTime_Min` |

The JMX exporter sidecar with a catch-all rule produces shorter names without the `name_` and `type_` segments, for example `trino_execution_QueryManager_RunningQueries`. Dashboards and autoscaling queries written for one do not work with the other. Always confirm a metric name against the live endpoint:

```bash
kubectl -n trino exec deploy/trino-coordinator -- \
  curl -s -H 'X-Trino-User: admin' http://localhost:8080/metrics | grep -i 'QueryManager' | head -40
```

### Metrics worth alerting on

Names are given as JMX bean and attribute. Translate as above.

| What | Bean : attribute | Alert idea |
|---|---|---|
| Running queries | `trino.execution:name=QueryManager` : `RunningQueries` | Capacity trend |
| Queued queries | `trino.execution:name=QueryManager` : `QueuedQueries` | Sustained queue |
| Failures by cause | `trino.execution:name=QueryManager` : `InternalFailures.FiveMinute.Count`, `ExternalFailures.FiveMinute.Count`, `UserErrorFailures.FiveMinute.Count`, `FailedQueries.FiveMinute.Count` | Internal failures above zero for long |
| Latency | `trino.execution:name=QueryManager` : `ExecutionTime.FiveMinutes.P50` and other percentiles | Regression against baseline |
| Queries waiting for resources | `trino.execution:name=QueryManager` : `WaitingForResourcesQueries`, `WaitingForResourcesMaxAgeInSeconds` | Under-provisioned cluster |
| Active workers | `trino.failuredetector:name=HeartbeatFailureDetector` : `ActiveCount` | Below expected |
| Required workers | `trino.execution:name=ClusterSizeMonitor` : `RequiredWorkers` | Autoscaling input |
| Cluster memory | `trino.memory:type=ClusterMemoryPool,name=general` : `FreeDistributedBytes`, `ReservedDistributedBytes`, `BlockedNodes` | Blocked nodes above zero |
| Memory kills | `trino.memory:name=ClusterMemoryManager` : `QueriesKilledDueToOutOfMemory` | Any increase |
| Task input | `trino.execution:name=SqlTaskManager` : `InputDataSize.FiveMinute.Count`, `InputPositions.FiveMinute.Count` | Throughput |
| JVM | `java.lang:type=Memory`, `java.lang:type=GarbageCollector,*` | Heap after GC, pause time |
| Resource groups | `trino.execution.resourcegroups:*`, when `jmxExport` is true | Queue per group |

Upstream list: [admin/jmx.md](../upstream/trino/docs/admin/jmx.md). Kubernetes-level signals to add: pod restarts, `OOMKilled`, CPU throttling, pending worker pods.

## JMX exporter sidecar

```yaml
jmx:
  enabled: true
  registryPort: 9080
  serverPort: 9081
  exporter:
    enabled: true
    port: 5556
    configProperties: |-
      hostPort: localhost:{{- .Values.jmx.registryPort }}
      startDelaySeconds: 0
      ssl: false
      lowercaseOutputName: false
      lowercaseOutputLabelNames: false
      includeObjectNames: ["trino.execution:name=QueryManager", "trino.execution:name=ClusterSizeMonitor", "trino.memory:*", "java.lang:type=Memory"]
      rules:
        - pattern: ".*"
```

Use it when you already have dashboards built on exporter names, or need exporter rules for relabelling. Otherwise the built-in endpoint is simpler and needs no sidecar. The chart default image for the exporter comes from a third-party registry. Mirror it and set `jmx.exporter.image`.

## SQL views

```sql
-- nodes
SELECT * FROM system.runtime.nodes;

-- what is running
SELECT query_id, state, user, source, resource_group_id, created, queued_time_ms, planning_time_ms
FROM system.runtime.queries
WHERE state IN ('QUEUED', 'PLANNING', 'STARTING', 'RUNNING', 'FINISHING')
ORDER BY created;

-- recent failures
SELECT query_id, user, error_type, error_code, "end"
FROM system.runtime.queries
WHERE state = 'FAILED'
ORDER BY "end" DESC
LIMIT 50;

-- tasks of one query
SELECT node_id, stage_id, state, splits, physical_input_bytes, output_bytes
FROM system.runtime.tasks
WHERE query_id = '20260101_000000_00000_abcde';

-- kill a query
CALL system.runtime.kill_query(query_id => '20260101_000000_00000_abcde', message => 'Killed by operator');
```

`system.runtime.queries` only holds what the coordinator still remembers: running queries plus up to `query.max-history` finished ones, for at least `query.min-expire-age`. It is not a history store. Use an event listener for history.

Tables available: `system.runtime.nodes`, `queries`, `tasks`, `transactions`, `optimizer_rule_stats`, and `system.metadata.*`. Page: [connector/system.md](../upstream/trino/docs/connector/system.md).

With a `jmx` catalog you can read beans through SQL on all nodes at once:

```sql
SELECT node, "freebytes", "reservedbytes"
FROM jmx.current."trino.memory:type=memorypool,name=general";
```

Page: [connector/jmx.md](../upstream/trino/docs/connector/jmx.md).

## Query history with event listeners

```yaml
eventListenerProperties:
  - event-listener.name=http
  - http-event-listener.connect-ingest-uri=http://collector.observability.svc:8080/trino
  - http-event-listener.log-created=false
  - http-event-listener.log-completed=true
```

The completed event carries the SQL text, user, source, client tags, resource group, timings, CPU, peak memory, bytes and rows read and written, tables and columns touched, and the failure. It is the right feed for cost attribution, audit and slow-query analysis.

| Listener | `event-listener.name` | Page |
|---|---|---|
| HTTP | `http` | [event-listeners-http.md](../upstream/trino/docs/admin/event-listeners-http.md) |
| Kafka | `kafka` | [event-listeners-kafka.md](../upstream/trino/docs/admin/event-listeners-kafka.md) |
| MySQL | `mysql` | [event-listeners-mysql.md](../upstream/trino/docs/admin/event-listeners-mysql.md) |
| OpenLineage | `openlineage` | [event-listeners-openlineage.md](../upstream/trino/docs/admin/event-listeners-openlineage.md) |

For more than one listener, create one file per listener with `coordinator.additionalConfigFiles` and name them in `event-listener.config-files` through `server.coordinatorExtraConfig`. Query events are produced by the coordinator.

Source-derived properties: [http](reference/config-properties/plugin--trino-http-event-listener.md), [kafka](reference/config-properties/plugin--trino-kafka-event-listener.md), [mysql](reference/config-properties/plugin--trino-mysql-event-listener.md), [openlineage](reference/config-properties/plugin--trino-openlineage.md).

## Tracing

```yaml
additionalConfigProperties:
  - tracing.enabled=true
  - otel.exporter.endpoint=http://otel-collector.observability.svc:4317
```

Trino sends OTLP over gRPC from the coordinator and workers. Trace IDs supplied by clients are kept, so a trace can span the calling application and Trino. Page: [admin/opentelemetry.md](../upstream/trino/docs/admin/opentelemetry.md).

## Logs

- Server log: standard output of each pod. `kubectl logs`. Set `log.console-format=JSON` for structured output.
- Levels: `server.log.trino.level` and `additionalLogProperties`, for example `io.trino.plugin.hive=DEBUG`.
- HTTP request log: a file under the data directory, `var/log/http-request.log`, rotated. Disable with `http-server.log.enabled=false` or ship it with a sidecar.
- GC log: add `-Xlog:gc*` through `additionalJVMConfig` when chasing pauses.

Lines to search for:

| Pattern | Meaning |
|---|---|
| `SERVER STARTED` | Node finished starting |
| `Shutdown requested` | Graceful shutdown began |
| `Configuration property ... was not used` or `Error: Configuration property` | Bad or misplaced property. The node will not start. |
| `Cannot connect to discovery server` | Worker cannot reach the coordinator |
| `OutOfMemoryError` | Heap exhausted |
| `Query ... failed` with `REMOTE_TASK_ERROR`, `PAGE_TRANSPORT_TIMEOUT` | A worker is gone, slow, or unreachable |

## Web UI

`http://<coordinator>/ui/`. The query detail page has the plan, a stage view with per-operator CPU, input and wall time, and live worker status. The legacy UI is at `/ui/legacy`. Turn the UI off with `web-ui.enabled=false`. Properties: [admin/properties-web-interface.md](../upstream/trino/docs/admin/properties-web-interface.md).

## Dashboards

The Trino project publishes Grafana material in the `trinodb/grafana-trino` repository. It is not copied here. The Data on EKS stack wires Prometheus and Grafana for Trino. See guide [16](16-eks-reference-stack.md).
