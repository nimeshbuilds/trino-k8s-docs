# Documented server properties with defaults

Every property described in the upstream `admin/properties-*.md` pages of Trino 484, with the documented type, default and matching session property.

This index is generated from the documentation. For properties the documentation does not cover, see [config-properties/README.md](config-properties/README.md), which is generated from the source code.

All of these go in `config.properties` unless the upstream page says otherwise.

## Catalog management properties

Upstream page: [admin/properties-catalog.md](../../upstream/trino/docs/admin/properties-catalog.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `catalog.management` | string | `static` | `static`, `dynamic` |  |
| `catalog.prune.update-interval` | duration | `5s` |  |  |
| `catalog.store` | string | `file` | `file`, `memory` |  |
| `catalog.config-dir` | string | `etc/catalog/` |  |  |
| `catalog.disabled-catalogs` | string |  |  |  |
| `catalog.read-only` | string | `false` |  |  |

## Client protocol properties

Upstream page: [admin/properties-client-protocol.md](../../upstream/trino/docs/admin/properties-client-protocol.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `protocol.spooling.enabled` | boolean | `true` |  | `spooling_enabled` |
| `protocol.spooling.shared-secret-key` | string |  |  |  |
| `protocol.spooling.retrieval-mode` | string | `STORAGE` |  |  |
| `protocol.spooling.encoding.json.enabled` | boolean | `true` |  |  |
| `protocol.spooling.encoding.json+zstd.enabled` | boolean | `true` |  |  |
| `protocol.spooling.encoding.json+lz4.enabled` | boolean | `true` |  |  |
| `protocol.spooling.encoding.compression.threshold` | data-size | `8kB` |  |  |
| `protocol.spooling.initial-segment-size` | data-size | `8MB` |  | `spooling_initial_segment_size` |
| `protocol.spooling.max-segment-size` | data-size | `16MB` |  | `spooling_max_segment_size` |
| `protocol.spooling.inlining.enabled` | boolean | `true` |  | `spooling_inlining_enabled` |
| `protocol.spooling.inlining.max-rows` | integer | `1000` |  | `spooling_inlining_max_rows` |
| `protocol.spooling.inlining.max-size` | data-size | `128kB` |  | `spooling_inlining_max_size` |
| `fs.azure.enabled` | boolean | `false` |  |  |
| `fs.s3.enabled` | boolean | `false` |  |  |
| `fs.gcs.enabled` | boolean | `false` |  |  |
| `fs.location` | string |  |  |  |
| `fs.segment.ttl` | duration | `12h` |  |  |
| `fs.segment.direct.ttl` | duration | `1h` |  |  |
| `fs.segment.encryption` | boolean | `true` |  |  |
| `fs.segment.explicit-ack` | boolean | `true` |  |  |
| `fs.segment.pruning.enabled` | boolean | `true` |  |  |
| `fs.segment.pruning.interval` | duration | `5m` |  |  |
| `fs.segment.pruning.batch-size` | integer | `250` |  |  |
| `protocol.v1.prepared-statement-compression.length-threshold` | integer | `2048` |  |  |
| `protocol.v1.prepared-statement-compression.min-gain` | integer | `512` |  |  |

## Exchange properties

Upstream page: [admin/properties-exchange.md](../../upstream/trino/docs/admin/properties-exchange.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `exchange.client-threads` | integer | `25` |  |  |
| `exchange.concurrent-request-multiplier` | integer | `3` |  |  |
| `exchange.compression-codec` | string | `NONE` | `NONE`, `LZ4`, `ZSTD` |  |
| `exchange.data-integrity-verification` | string | `ABORT` | `NONE`, `ABORT`, `RETRY` |  |
| `exchange.max-buffer-size` | data-size | `32MB` |  |  |
| `exchange.max-response-size` | data-size | `16MB` |  |  |
| `sink.max-buffer-size` | data-size | `32MB` |  |  |
| `sink.max-broadcast-buffer-size` | `data size` | `200MB` |  |  |

## General properties

Upstream page: [admin/properties-general.md](../../upstream/trino/docs/admin/properties-general.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `join-distribution-type` | string | `AUTOMATIC` | `AUTOMATIC`, `PARTITIONED`, `BROADCAST` | `join_distribution_type` |
| `redistribute-writes` | boolean | `true` |  | `redistribute_writes` |

## HTTP client properties

Upstream page: [admin/properties-http-client.md](../../upstream/trino/docs/admin/properties-http-client.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `http-client.connect-timeout` | duration | `5s` |  |  |
| `max-content-length` | data-size | `16MB` |  |  |
| `http-client.request-timeout` | duration | `5m` |  |  |
| `http-client.https.excluded-cipher` | string |  |  |  |
| `http-client.https.included-cipher` | string |  |  |  |
| `http-client.https.hostname-verification` | boolean | `true` |  |  |
| `http-client.key-store-password` | string |  |  |  |
| `http-client.key-store-path` | string |  |  |  |
| `http-client.secure-random-algorithm` | string |  |  |  |
| `http-client.trust-store-password` | string |  |  |  |
| `http-client.trust-store-path` | string |  |  |  |
| `http-client.http-proxy` | string |  |  |  |
| `http-client.http-proxy.user` | string |  |  |  |
| `http-client.http-proxy.password` | string |  |  |  |
| `http-client.http-proxy.secure` | boolean | `false` |  |  |
| `http-client.socks-proxy` | string |  |  |  |
| `http-client.log.compression.enabled` | boolean | `true` |  |  |
| `http-client.log.enabled` | boolean | `false` |  |  |
| `http-client.log.flush-interval` | duration | `10s` |  |  |
| `http-client.log.max-history` | integer | `15` |  |  |
| `http-client.log.max-size` | data-size | `1GB` |  |  |
| `http-client.log.path` | string | `var/log/` |  |  |
| `http-client.log.queue-size` | integer | `10000` |  |  |

## HTTP server properties

Upstream page: [admin/properties-http-server.md](../../upstream/trino/docs/admin/properties-http-server.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `http-server.process-forwarded` | boolean | `false` |  |  |
| `http-server.http.port` | integer | `8080` |  |  |
| `http-server.https.enabled` | boolean | `false` |  |  |
| `http-server.https.port` | integer | `8443` |  |  |
| `http-server.https.keystore.path` | string |  |  |  |
| `http-server.https.keystore.key` | string |  |  |  |
| `http-server.https.truststore.path` | boolean | `false` |  |  |
| `http-server.https.truststore.key` | boolean | `false` |  |  |
| `http-server.https.keymanager.password` | string |  |  |  |
| `http-server.https.secure-random-algorithm` | string |  |  |  |
| `http-server.https.ssl-session-timeout` | duration | `4h` |  |  |
| `http-server.https.ssl-session-cache-size` | integer | `10000` |  |  |
| `http-server.https.ssl-context.refresh-time` | duration | `1m` |  |  |
| `http-server.authentication.type` | string |  |  |  |
| `http-server.authentication.allow-insecure-over-http` | boolean |  |  |  |
| `http-server.authentication.certificate.*` |  |  |  |  |
| `http-server.authentication.jwt.*` |  |  |  |  |
| `http-server.authentication.krb5.*` |  |  |  |  |
| `http-server.authentication.oauth2.*` |  |  |  |  |
| `http-server.authentication.password.*` |  |  |  |  |
| `http-server.log.*` |  |  |  |  |
| `internal-communication.shared-secret` | string |  |  |  |
| `internal-communication.http2.enabled` | boolean | `true` |  |  |
| `internal-communication.https.required` | boolean | `false` |  |  |

## Logging properties

Upstream page: [admin/properties-logging.md](../../upstream/trino/docs/admin/properties-logging.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `log.annotation-file` | string |  |  |  |
| `log.format` | string | `TEXT` |  |  |
| `log.console-format` | string | `TEXT` |  |  |
| `log.path` | string |  |  |  |
| `log.max-size` | data-size | `100MB` |  |  |
| `log.max-total-size` | data-size | `1GB` |  |  |
| `log.compression` | string | `GZIP` |  |  |
| `http-server.log.enabled` | boolean | `true` |  |  |
| `http-server.log.compression.enabled` | boolean | `true` |  |  |
| `http-server.log.path` | string | `var/log/http-request.log` |  |  |
| `http-server.log.max-history` | integer | `15` |  |  |
| `http-server.log.max-size` | data-size | `unlimited` |  |  |

## Node scheduler properties

Upstream page: [admin/properties-node-scheduler.md](../../upstream/trino/docs/admin/properties-node-scheduler.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `node-scheduler.include-coordinator` | boolean | `true` |  |  |
| `node-scheduler.max-splits-per-node` | integer | `256` |  |  |
| `node-scheduler.min-pending-splits-per-task` | integer | `16` |  |  |
| `node-scheduler.max-adjusted-pending-splits-per-task` | integer | `2000` |  |  |
| `node-scheduler.max-unacknowledged-splits-per-task` | integer | `2000` |  |  |
| `node-scheduler.min-candidates` | integer | `10` |  |  |
| `node-scheduler.policy` | string | `uniform` | `uniform`, `topology` |  |
| `node-scheduler.network-topology.segments` | string | `machine` |  |  |
| `node-scheduler.network-topology.type` | string | `flat` | `flat`, `file`, `subnet` |  |
| `node-scheduler.network-topology.file` | string |  |  |  |
| `node-scheduler.network-topology.refresh-period` | duration | `5m` |  |  |
| `node-scheduler.network-topology.subnet.ip-address-protocol` | string | `IPv4` | `IPv4`, `IPv6` |  |
| `node-scheduler.network-topology.subnet.cidr-prefix-lengths` |  |  |  |  |

## Optimizer properties

Upstream page: [admin/properties-optimizer.md](../../upstream/trino/docs/admin/properties-optimizer.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `optimizer.dictionary-aggregation` | boolean | `false` |  | `dictionary_aggregation` |
| `optimizer.optimize-metadata-queries` | boolean | `false` |  | `optimize_metadata_queries` |
| `optimizer.distinct-aggregations-strategy` | string | `AUTOMATIC` | `AUTOMATIC`, `MARK_DISTINCT`, `SINGLE_STEP`, `PRE_AGGREGATE`, `SPLIT_TO_SUBQUERIES` | `distinct_aggregations_strategy` |
| `optimizer.push-aggregation-through-outer-join` | boolean | `true` |  | `push_aggregation_through_outer_join` |
| `optimizer.push-table-write-through-union` | boolean | `true` |  | `push_table_write_through_union` |
| `optimizer.push-filter-into-values-max-row-count` | integer | `100` |  | `push_filter_into_values_max_row_count` |
| `optimizer.join-reordering-strategy` | string | `AUTOMATIC` | `AUTOMATIC`, `ELIMINATE_CROSS_JOINS`, `NONE` | `join_reordering_strategy` |
| `optimizer.max-reordered-joins` | integer | `8` |  | `max_reordered_joins` |
| `optimizer.optimize-duplicate-insensitive-joins` | boolean | `true` |  | `optimize_duplicate_insensitive_joins` |
| `optimizer.use-exact-partitioning` | boolean | `false` |  | `use_exact_partitioning` |
| `optimizer.use-table-scan-node-partitioning` | boolean | `true` |  | `use_table_scan_node_partitioning` |
| `optimizer.table-scan-node-partitioning-min-bucket-to-task-ratio` | double | `0.5` |  | `table_scan_node_partitioning_min_bucket_to_task_ratio` |
| `optimizer.colocated-joins-enabled` | boolean | `true` |  | `colocated_join` |
| `optimizer.filter-conjunction-independence-factor` | double | `0.75` |  | `filter_conjunction_independence_factor` |
| `optimizer.join-multi-clause-independence-factor` | double | `0.25` |  | `join_multi_clause_independence_factor` |
| `optimizer.non-estimatable-predicate-approximation.enabled` | boolean | `true` |  | `non_estimatable_predicate_approximation_enabled` |
| `optimizer.join-partitioned-build-min-row-count` | integer | `1000000` |  | `join_partitioned_build_min_row_count` |
| `optimizer.min-input-size-per-task` | data-size | `5GB` |  | `min_input_size_per_task` |
| `optimizer.min-input-rows-per-task` | integer | `10000000` |  | `min_input_rows_per_task` |
| `optimizer.use-cost-based-partitioning` | boolean | `true` |  | `use_cost_based_partitioning` |

## Query management properties

Upstream page: [admin/properties-query-management.md](../../upstream/trino/docs/admin/properties-query-management.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `query.client.timeout` | duration | `5m` |  |  |
| `query.execution-policy` | string | `phased` |  | `execution_policy` |
| `query.determine-partition-count-for-write-enabled` | boolean | `false` |  | `determine_partition_count_for_write_enabled` |
| `query.max-hash-partition-count` | integer | `100` |  | `max_hash_partition_count` |
| `query.min-hash-partition-count` | integer | `4` |  | `min_hash_partition_count` |
| `query.min-hash-partition-count-for-write` | integer | `50` |  | `min_hash_partition_count_for_write` |
| `query.max-writer-task-count` | integer | `100` |  | `max_writer_task_count` |
| `query.low-memory-killer.policy` | string | `total-reservation-on-blocked-nodes` |  |  |
| `task.low-memory-killer.policy` | string | `total-reservation-on-blocked-nodes` |  |  |
| `query.max-execution-time` | duration | `100d` |  | `query_max_execution_time` |
| `query.max-length` | integer | `1,000,000` |  |  |
| `query.max-planning-time` | duration | `10m` |  | `query_max_planning_time` |
| `query.max-run-time` | duration | `100d` |  | `query_max_run_time` |
| `query.max-scan-physical-bytes` | data-size |  |  | `query_max_scan_physical_bytes` |
| `query.max-write-physical-size` | data-size |  |  | `query_max_write_physical_size` |
| `query.max-output-data-size` | data-size |  |  |  |
| `query.max-stage-count` | integer | `150` |  |  |
| `query.max-history` | integer | `100` |  |  |
| `query.min-expire-age` | duration | `15m` |  |  |
| `query.remote-task.enable-adaptive-request-size` | boolean | `true` |  | `remote_task_adaptive_update_request_size_enabled` |
| `query.remote-task.guaranteed-splits-per-task` | integer | `3` |  | `remote_task_guaranteed_splits_per_request` |
| `query.remote-task.max-error-duration` | duration | `1m` |  |  |
| `query.remote-task.max-request-size` | data-size | `8MB` |  | `remote_task_max_request_size` |
| `query.remote-task.request-size-headroom` | data-size | `2MB` |  | `remote_task_request_size_headroom` |
| `query.info-url-template` | string | `(URL of the query info page on the coordinator)` |  |  |
| `retry-policy` | string | `NONE` |  |  |

## Regular expression function properties

Upstream page: [admin/properties-regexp-function.md](../../upstream/trino/docs/admin/properties-regexp-function.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `regex-library` | string | `JONI` | `JONI`, `RE2J` |  |
| `re2j.dfa-states-limit` | integer | `2147483647` |  |  |
| `re2j.dfa-retries` | integer | `5` |  |  |

## Resource management properties

Upstream page: [admin/properties-resource-management.md](../../upstream/trino/docs/admin/properties-resource-management.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `query.max-cpu-time` | duration | `1_000_000_000d` |  |  |
| `query.max-memory-per-node` | heap-size | (30% of maximum heap size on the node) |  |  |
| `query.max-memory` | data-size | `20GB` |  |  |
| `query.max-total-memory` | data-size | (`query.max-memory` * 2) |  |  |
| `memory.heap-headroom-per-node` | heap-size | (30% of maximum heap size on the node) |  |  |
| `exchange.deduplication-buffer-size` | data-size | `32MB` |  |  |

## Spilling properties

Upstream page: [admin/properties-spilling.md](../../upstream/trino/docs/admin/properties-spilling.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `spill-enabled` | boolean | `false` |  | `spill_enabled` |
| `spiller-spill-path` | string |  |  |  |
| `spiller-max-used-space-threshold` | double | `0.9` |  |  |
| `spiller-threads` | integer | The number of spill directories multiplied by 2, with a minimum value of 4. |  |  |
| `max-spill-per-node` | data-size | `100GB` |  |  |
| `query-max-spill-per-node` | data-size | `100GB` |  |  |
| `aggregation-operator-unspill-memory-limit` | data-size | `4MB` |  |  |
| `spill-compression-codec` | string | `NONE` | `NONE`, `LZ4`, `ZSTD` |  |
| `spill-encryption-enabled` | boolean | `false` |  |  |

## SQL environment properties

Upstream page: [admin/properties-sql-environment.md](../../upstream/trino/docs/admin/properties-sql-environment.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `sql.forced-session-time-zone` | string |  |  |  |
| `sql.default-catalog` | string |  |  |  |
| `sql.default-schema` | string |  |  |  |
| `sql.default-function-catalog` | string |  |  |  |
| `sql.default-function-schema` | string |  |  |  |
| `sql.path` | string |  |  |  |

## Task properties

Upstream page: [admin/properties-task.md](../../upstream/trino/docs/admin/properties-task.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `task.concurrency` | integer | The number of physical CPUs of the node, with a minimum value of 2 and a maximum of 32. Defaults to 8 in /admin/fault-tolerant-execution mode. |  | `task_concurrency` |
| `task.http-response-threads` | integer | `100` |  |  |
| `task.http-timeout-threads` | integer | `3` |  |  |
| `task.info-update-interval` | duration | `3s` |  |  |
| `task.max-drivers-per-task` | integer |  |  |  |
| `task.max-partial-aggregation-memory` | data-size | `16MB` |  |  |
| `task.max-worker-threads` | integer | (Node CPUs * 2) |  |  |
| `task.min-drivers` | integer | (`task.max-worker-threads` * 2) |  |  |
| `task.min-drivers-per-task` | integer |  |  |  |
| `task.scale-writers.enabled` |  |  |  |  |
| `task.min-writer-count` | integer | `1` |  | `task_min_writer_count` |
| `task.max-writer-count` | integer | The number of physical CPUs of the node, with a minimum value of 2 and a maximum of 64 |  | `task_max_writer_count` |
| `task.interrupt-stuck-split-tasks-enabled` | boolean | `true` |  |  |
| `task.interrupt-stuck-split-tasks-warning-threshold` | duration | `10m` |  |  |
| `task.interrupt-stuck-split-tasks-timeout` | duration | `10m` |  |  |
| `task.interrupt-stuck-split-tasks-detection-interval` | duration | `2m` |  |  |

## Web UI properties

Upstream page: [admin/properties-web-interface.md](../../upstream/trino/docs/admin/properties-web-interface.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `web-ui.authentication.type` | string | `FORM` | `FORM`, `FIXED`, `CERTIFICATE`, `KERBEROS`, `JWT`, `OAUTH2` |  |
| `web-ui.enabled` | boolean | `true` |  |  |
| `web-ui.shared-secret` | string | randomly generated unless set |  |  |
| `web-ui.session-timeout` | duration | `1d` |  |  |
| `web-ui.user` | string | None |  |  |

## Write partitioning properties

Upstream page: [admin/properties-write-partitioning.md](../../upstream/trino/docs/admin/properties-write-partitioning.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `use-preferred-write-partitioning` | boolean | `true` |  | `use_preferred_write_partitioning` |

## Writer scaling properties

Upstream page: [admin/properties-writer-scaling.md](../../upstream/trino/docs/admin/properties-writer-scaling.md)

| Property | Type | Default | Allowed values | Session property |
|---|---|---|---|---|
| `scale-writers` | boolean | `true` |  | `scale_writers` |
| `task.scale-writers.enabled` | boolean | `true` |  | `task_scale_writers_enabled` |
| `task.scale-writers.max-writer-memory-percentage` | double | `70` |  | `task_scale_writers_max_writer_memory_percentage` |
| `writer-scaling-min-data-processed` | data-size | `120MB` |  | `writer_scaling_min_data_processed` |
