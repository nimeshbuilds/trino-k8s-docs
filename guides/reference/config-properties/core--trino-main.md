# Configuration properties: `core/trino-main`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [AccessControlConfig](#accesscontrolconfig), [AnnounceNodeAnnouncerConfig](#announcenodeannouncerconfig), [CacheManagerConfig](#cachemanagerconfig), [CatalogManagerConfig](#catalogmanagerconfig), [CatalogPruneTaskConfig](#catalogprunetaskconfig), [CatalogStoreConfig](#catalogstoreconfig), [CertificateConfig](#certificateconfig), [CompilerConfig](#compilerconfig), [DirectExchangeClientConfig](#directexchangeclientconfig), [DnsNodeInventoryConfig](#dnsnodeinventoryconfig), [DynamicFilterConfig](#dynamicfilterconfig), [EmbeddedDiscoveryConfig](#embeddeddiscoveryconfig), [EventListenerConfig](#eventlistenerconfig), [ExchangeManagerConfig](#exchangemanagerconfig), [FailureDetectorConfig](#failuredetectorconfig), [FeaturesConfig](#featuresconfig), [FileCatalogStoreConfig](#filecatalogstoreconfig), [FixedUserWebUiConfig](#fixeduserwebuiconfig), [FormWebUiConfig](#formwebuiconfig), [HeaderAuthenticatorConfig](#headerauthenticatorconfig), [InsecureAuthenticatorConfig](#insecureauthenticatorconfig), [InternalCommunicationConfig](#internalcommunicationconfig), [JmxNamingConfig](#jmxnamingconfig), [JwtAuthenticatorConfig](#jwtauthenticatorconfig), [KerberosConfig](#kerberosconfig), [MemoryManagerConfig](#memorymanagerconfig), [NodeInventoryConfig](#nodeinventoryconfig), [NodeMemoryConfig](#nodememoryconfig), [NodeSchedulerConfig](#nodeschedulerconfig), [NodeSpillConfig](#nodespillconfig), [OAuth2Config](#oauth2config), [OidcDiscoveryConfig](#oidcdiscoveryconfig), [OptimizerConfig](#optimizerconfig), [PasswordAuthenticatorConfig](#passwordauthenticatorconfig), [ProtocolConfig](#protocolconfig), [QueryDataEncodingConfig](#querydataencodingconfig), [QueryManagerConfig](#querymanagerconfig), [QueryMonitorConfig](#querymonitorconfig), [RefreshTokensConfig](#refreshtokensconfig), [SecurityConfig](#securityconfig), [ServerConfig](#serverconfig), [ServerPluginsProviderConfig](#serverpluginsproviderconfig), [SpoolingConfig](#spoolingconfig), [SpoolingEnabledConfig](#spoolingenabledconfig), [SqlEnvironmentConfig](#sqlenvironmentconfig), [StableHostAddressProviderConfig](#stablehostaddressproviderconfig), [StaticCatalogManagerConfig](#staticcatalogmanagerconfig), [StaticOAuth2ServerConfig](#staticoauth2serverconfig), [SubnetTopologyConfig](#subnettopologyconfig), [TaskManagerConfig](#taskmanagerconfig), [TestingWarningCollectorConfig](#testingwarningcollectorconfig), [TopologyAwareNodeSelectorConfig](#topologyawarenodeselectorconfig), [TopologyFileConfig](#topologyfileconfig), [TransactionManagerConfig](#transactionmanagerconfig), [WarningCollectorConfig](#warningcollectorconfig), [WebUiAuthenticationConfig](#webuiauthenticationconfig), [WebUiConfig](#webuiconfig)

## AccessControlConfig

Source: `core/trino-main/src/main/java/io/trino/security/AccessControlConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `access-control.config-files` | List<String> | `ImmutableList.of()` |  |  |

## AnnounceNodeAnnouncerConfig

Source: `core/trino-main/src/main/java/io/trino/node/AnnounceNodeAnnouncerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `discovery.uri` | List<URI> | `List.of()` |  |  |

## CacheManagerConfig

Source: `core/trino-main/src/main/java/io/trino/cache/CacheManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `cache-manager.config-files` | List<String> | `ImmutableList.of()` |  |  |

## CatalogManagerConfig

Source: `core/trino-main/src/main/java/io/trino/connector/CatalogManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `catalog.management` | CatalogMangerKind | `CatalogMangerKind.STATIC` |  |  |

## CatalogPruneTaskConfig

Source: `core/trino-main/src/main/java/io/trino/connector/CatalogPruneTaskConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `catalog.prune.enabled` | boolean | `true` |  |  |
| `catalog.prune.update-interval` | Duration | `new Duration(5, SECONDS)` |  |  |

## CatalogStoreConfig

Source: `core/trino-main/src/main/java/io/trino/connector/CatalogStoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `catalog.store` | String | `"file"` |  |  |

## CertificateConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/CertificateConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.certificate.user-mapping.file` | File | `Optional.empty()` |  |  |
| `http-server.authentication.certificate.user-mapping.pattern` | String | `Optional.empty()` |  |  |

## CompilerConfig

Source: `core/trino-main/src/main/java/io/trino/sql/planner/CompilerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `compiler.expression-cache-size` | int | `10_000` | Reuse compiled expressions across multiple queries |  |
| `compiler.specialized-aggregation-loops` | boolean | `true` |  |  |

Removed properties, rejected at startup if still set: `compiler.interpreter-enabled`

## DirectExchangeClientConfig

Source: `core/trino-main/src/main/java/io/trino/operator/DirectExchangeClientConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `exchange.acknowledge-pages` | boolean | `true` |  |  |
| `exchange.client-threads` | String | `25` |  |  |
| `exchange.concurrent-request-multiplier` | int | `3` |  |  |
| `exchange.deduplication-buffer-size` | DataSize | `DataSize.of(32, Unit.MEGABYTE)` |  |  |
| `exchange.max-buffer-size` | DataSize | `DataSize.of(32, Unit.MEGABYTE)` |  |  |
| `exchange.max-error-duration` | Duration | `new Duration(1, TimeUnit.MINUTES)` |  |  |
| `exchange.max-response-size` | DataSize | `new HttpClientConfig().getMaxResponseContentLength()` |  |  |
| `exchange.page-buffer-client.max-callback-threads` | String | `25` |  |  |

Removed properties, rejected at startup if still set: `exchange.min-error-duration`

## DnsNodeInventoryConfig

Source: `core/trino-main/src/main/java/io/trino/node/DnsNodeInventoryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `cluster.hosts` | Set<String> | `ImmutableSet.of()` | List of host names to resolve for Trino nodes. All host addresses are used, including IPv4 and IPv6. |  |

## DynamicFilterConfig

Source: `core/trino-main/src/main/java/io/trino/execution/DynamicFilterConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `dynamic-filtering.max-distinct-values-per-driver` | int | `50_000` |  | Legacy names: `dynamic-filtering.large.max-distinct-values-per-driver` |
| `dynamic-filtering.max-size-per-driver` | DataSize | `DataSize.of(4, MEGABYTE)` |  | Legacy names: `dynamic-filtering.large.max-size-per-driver` |
| `dynamic-filtering.max-size-per-filter` | DataSize | `DataSize.of(10, MEGABYTE)` |  | Legacy names: `dynamic-filtering.large.max-size-per-filter` |
| `dynamic-filtering.max-size-per-operator` | DataSize | `DataSize.of(5, MEGABYTE)` |  | Legacy names: `dynamic-filtering.large.max-size-per-operator` |
| `dynamic-filtering.partitioned.max-distinct-values-per-driver` | int | `20_000` |  | Legacy names: `dynamic-filtering.large-partitioned.max-distinct-values-per-driver` |
| `dynamic-filtering.partitioned.max-size-per-driver` | DataSize | `DataSize.of(200, KILOBYTE)` |  | Legacy names: `dynamic-filtering.large-partitioned.max-size-per-driver` |
| `dynamic-filtering.partitioned.max-size-per-operator` | DataSize | `DataSize.of(5, MEGABYTE)` |  | Legacy names: `dynamic-filtering.large-partitioned.max-size-per-operator` |
| `dynamic-filtering.partitioned.range-row-limit-per-driver` | int | `30_000` |  | Legacy names: `dynamic-filtering.large-partitioned.range-row-limit-per-driver` |
| `dynamic-filtering.range-row-limit-per-driver` | int | `100_000` |  | Legacy names: `dynamic-filtering.large.range-row-limit-per-driver` |
| `dynamic-row-filtering.selectivity-threshold` | double | `0.7` | Avoid using dynamic row filters when fraction of rows selected is above threshold |  |
| `enable-dynamic-filtering` | boolean | `true` |  |  |
| `enable-dynamic-row-filtering` | boolean | `true` | Enable fine-grained filtering of rows in the scan operator using dynamic filters |  |

Removed properties, rejected at startup if still set: `dynamic-filtering-max-per-driver-row-count`, `dynamic-filtering-max-per-driver-size`, `dynamic-filtering-range-row-limit-per-driver`, `dynamic-filtering.large-broadcast.max-distinct-values-per-driver`, `dynamic-filtering.large-broadcast.max-size-per-driver`, `dynamic-filtering.large-broadcast.max-size-per-operator`, `dynamic-filtering.large-broadcast.range-row-limit-per-driver`, `dynamic-filtering.service-thread-count`, `dynamic-filtering.small-broadcast.max-distinct-values-per-driver`, `dynamic-filtering.small-broadcast.max-size-per-driver`, `dynamic-filtering.small-broadcast.max-size-per-operator`, `dynamic-filtering.small-broadcast.range-row-limit-per-driver`, `dynamic-filtering.small-partitioned.max-distinct-values-per-driver`, `dynamic-filtering.small-partitioned.max-size-per-driver`, `dynamic-filtering.small-partitioned.max-size-per-operator`, `dynamic-filtering.small-partitioned.range-row-limit-per-driver`, `dynamic-filtering.small.max-distinct-values-per-driver`, `dynamic-filtering.small.max-size-per-driver`, `dynamic-filtering.small.max-size-per-filter`, `dynamic-filtering.small.max-size-per-operator`, `dynamic-filtering.small.range-row-limit-per-driver`, `enable-coordinator-dynamic-filters-distribution`, `enable-large-dynamic-filters`, `experimental.dynamic-filtering-max-per-driver-row-count`, `experimental.dynamic-filtering-max-per-driver-size`, `experimental.dynamic-filtering-refresh-interval`, `experimental.enable-dynamic-filtering`

## EmbeddedDiscoveryConfig

Source: `core/trino-main/src/main/java/io/trino/node/EmbeddedDiscoveryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `discovery-server.enabled` | boolean | `true` |  | deprecated |

## EventListenerConfig

Source: `core/trino-main/src/main/java/io/trino/eventlistener/EventListenerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `event-listener.config-files` | List<String> | `ImmutableList.of()` |  |  |

## ExchangeManagerConfig

Source: `core/trino-main/src/main/java/io/trino/exchange/ExchangeManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `exchange-manager.config-file` | File | `Optional.empty()` |  |  |

## FailureDetectorConfig

Source: `core/trino-main/src/main/java/io/trino/failuredetector/FailureDetectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `failure-detector.enabled` | boolean | `true` |  |  |
| `failure-detector.expiration-grace-interval` | Duration | `new Duration(10, TimeUnit.MINUTES)` | How long to wait before 'forgetting' a service after it disappears from discovery |  |
| `failure-detector.heartbeat-interval` | Duration | `new Duration(500, TimeUnit.MILLISECONDS)` |  |  |
| `failure-detector.threshold` | double | `0.1` |  |  |
| `failure-detector.warmup-interval` | Duration | `new Duration(5, TimeUnit.SECONDS)` | How long to wait after transitioning to success before considering a service alive |  |

## FeaturesConfig

Source: `core/trino-main/src/main/java/io/trino/FeaturesConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `aggregation-operator-unspill-memory-limit` | DataSize | `DataSize.of(4, MEGABYTE)` |  |  |
| `analyzer.max-grouping-sets` | int | `2048` |  |  |
| `deprecated.legacy-arithmetic-decimal-operators` | boolean | `false` |  |  |
| `deprecated.legacy-catalog-roles` | boolean | `false` | Enable legacy role management syntax that assumed all roles are catalog scoped |  |
| `deprecated.legacy-varchar-to-char-coercion` | boolean | `false` | Implicitly coerce varchar to char, instead of char to varchar |  |
| `deprecated.regex-library` | RegexLibrary | `JONI` |  | Legacy names: `regex-library`; deprecated |
| `exchange.compression-codec` | CompressionCodec | `NONE` | Compression codec used for data in exchanges |  |
| `exchange.data-integrity-verification` | DataIntegrityVerification | `DataIntegrityVerification.ABORT` |  |  |
| `exchange.experimental.vectorized-serde.enabled` | boolean | `true` | Enable using Java Vector API for faster serialization and deserialization of exchange data |  |
| `experimental.adaptive-filter-reordering.enabled` | boolean | `true` | Reorder conjunctive/disjunctive filter terms at runtime based on observed selectivity and performance |  |
| `experimental.columnar-filter-evaluation.enabled` | boolean | `true` | Enables columnar evaluation of filters |  |
| `external-exchange-encryption-enabled` | boolean | `true` |  | Legacy names: `fault-tolerant-execution-exchange-encryption-enabled`, `fault-tolerant-execution.exchange-encryption-enabled` |
| `filter-and-project-min-output-page-row-count` | int | `256` |  |  |
| `filter-and-project-min-output-page-size` | DataSize | `DataSize.of(500, KILOBYTE)` |  |  |
| `force-spilling-join-operator` | boolean | `false` | Force spilling join operator in favour of the non-spilling one even when there is no spill |  |
| `hide-inaccessible-columns` | boolean | `false` | When enabled non-accessible columns are silently filtered from results from SELECT * statements |  |
| `incremental-hash-array-load-factor.enabled` | boolean | `true` | Use smaller load factor for small hash arrays in order to improve performance | deprecated |
| `max-memory-per-partition-writer` | DataSize | `DataSize.of(256, MEGABYTE)` | Estimated maximum memory required per partition writer in a single thread |  |
| `max-recursion-depth` | int | `10` | Maximum recursion depth for recursive common table expression |  |
| `memory-revoking-target` | double | `0.5` | When revoking memory, try to revoke so much that pool is filled below target at the end |  |
| `memory-revoking-threshold` | double | `0.9` | Revoke memory when memory pool is filled over threshold |  |
| `pages-index.eager-compaction-enabled` | boolean | `false` |  |  |
| `re2j.dfa-retries` | int | `5` |  |  |
| `re2j.dfa-states-limit` | int | `Integer.MAX_VALUE` |  |  |
| `redistribute-writes` | boolean | `true` |  |  |
| `scale-writers` | boolean | `true` |  |  |
| `spill-enabled` | boolean | `false` |  |  |
| `spiller-max-used-space-threshold` | double | `0.9` |  |  |
| `spiller-spill-path` | List<String> | `ImmutableList.of()` |  |  |
| `spiller-threads` | String |  |  |  |
| `writer-scaling-min-data-processed` | DataSize | `DataSize.of(120, MEGABYTE)` | Minimum amount of uncompressed output data processed by writers before writer scaling can happen |  |

Removed properties, rejected at startup if still set: `analyzer.experimental-syntax-enabled`, `arrayagg.implementation`, `deprecated.disable-set-properties-security-check-for-create-ddl`, `deprecated.group-by-uses-equal`, `deprecated.legacy-char-to-varchar-coercion`, `deprecated.legacy-join-using`, `deprecated.legacy-map-subscript`, `deprecated.legacy-order-by`, `deprecated.legacy-row-field-ordinal-access`, `deprecated.legacy-row-to-json-cast`, `deprecated.legacy-timestamp`, `deprecated.legacy-unnest-array-rows`, `deprecated.legacy-update-delete-implementation`, `deprecated.omit-datetime-type-precision`, `experimental-syntax-enabled`, `experimental.aggregation-operator-unspill-memory-limit`, `experimental.filter-and-project-min-output-page-row-count`, `experimental.filter-and-project-min-output-page-size`, `experimental.late-materialization.enabled`, `experimental.memory-revoking-target`, `experimental.memory-revoking-threshold`, `experimental.resource-groups-enabled`, `experimental.spill-enabled`, `experimental.spill-order-by`, `experimental.spill-window-operator`, `experimental.spiller-max-used-space-threshold`, `experimental.spiller-spill-path`, `experimental.spiller-threads`, `fast-inequality-joins`, `histogram.implementation`, `legacy.allow-set-view-authorization`, `legacy.materialized-view-grace-period`, `multimapagg.implementation`, `optimizer.iterative-rule-based-column-pruning`, `optimizer.processing-optimization`, `parse-decimal-literals-as-double`, `resource-group-manager`, `spill-order-by`, `spill-window-operator`

## FileCatalogStoreConfig

Source: `core/trino-main/src/main/java/io/trino/connector/FileCatalogStoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `catalog.config-dir` | File | `new File("etc/catalog/")` |  | Legacy names: `plugin.config-dir` |
| `catalog.disabled-catalogs` | List<String> | `ImmutableList.of()` |  |  |
| `catalog.read-only` | boolean | `false` |  |  |

## FixedUserWebUiConfig

Source: `core/trino-main/src/main/java/io/trino/server/ui/FixedUserWebUiConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `web-ui.user` | String |  |  |  |

## FormWebUiConfig

Source: `core/trino-main/src/main/java/io/trino/server/ui/FormWebUiConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `web-ui.session-timeout` | Duration | `new Duration(1, DAYS)` |  |  |
| `web-ui.shared-secret` | String | `Optional.empty()` |  | sensitive |

## HeaderAuthenticatorConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/HeaderAuthenticatorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `header-authenticator.config-files` | List<String> | `ImmutableList.of(new File("etc/header-authenticator.properties"))` | Ordered list of header authenticator configuration files |  |
| `http-server.authentication.header.user-mapping.file` | File | `Optional.empty()` | An optional user mapping file containing the mapping rules to be applied to the authenticated principal |  |
| `http-server.authentication.header.user-mapping.pattern` | String | `Optional.empty()` | An optional user mapping pattern to be applied to the authenticated principal |  |

## InsecureAuthenticatorConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/InsecureAuthenticatorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.insecure.user-mapping.file` | File | `Optional.empty()` |  |  |
| `http-server.authentication.insecure.user-mapping.pattern` | String | `Optional.empty()` |  |  |

## InternalCommunicationConfig

Source: `core/trino-main/src/main/java/io/trino/server/InternalCommunicationConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.https.enabled` | boolean | `false` |  |  |
| `internal-communication.http2.enabled` | boolean | `true` | Enable the HTTP/2 transport |  |
| `internal-communication.https.keystore.key` | String |  |  | sensitive |
| `internal-communication.https.keystore.path` | String |  |  |  |
| `internal-communication.https.required` | boolean | `false` |  |  |
| `internal-communication.https.truststore.key` | String |  |  | sensitive |
| `internal-communication.https.truststore.path` | String |  |  |  |
| `internal-communication.shared-secret` | String |  |  | sensitive |

Removed properties, rejected at startup if still set: `internal-communication.jwt.enabled`, `internal-communication.kerberos.enabled`, `internal-communication.kerberos.use-canonical-hostname`

## JmxNamingConfig

Source: `core/trino-main/src/main/java/io/trino/server/JmxNamingConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `jmx.base-name` | String | `"trino"` |  |  |

## JwtAuthenticatorConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/jwt/JwtAuthenticatorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.jwt.key-file` | String |  |  | Legacy names: `http.authentication.jwt.key-file` |
| `http-server.authentication.jwt.principal-field` | String | `"sub"` |  |  |
| `http-server.authentication.jwt.required-audience` | String |  |  | Legacy names: `http.authentication.jwt.required-audience` |
| `http-server.authentication.jwt.required-issuer` | String |  |  | Legacy names: `http.authentication.jwt.required-issuer` |
| `http-server.authentication.jwt.user-mapping.file` | File | `Optional.empty()` |  |  |
| `http-server.authentication.jwt.user-mapping.pattern` | String | `Optional.empty()` |  |  |

## KerberosConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/KerberosConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.krb5.keytab` | File |  |  | Legacy names: `http.server.authentication.krb5.keytab` |
| `http-server.authentication.krb5.name-type` | KerberosNameType | `HOSTBASED_SERVICE` |  | Legacy names: `http.server.authentication.krb5.name-type` |
| `http-server.authentication.krb5.principal-hostname` | String |  |  | Legacy names: `http.server.authentication.krb5.principal-hostname` |
| `http-server.authentication.krb5.service-name` | String |  |  | Legacy names: `http.server.authentication.krb5.service-name` |
| `http-server.authentication.krb5.user-mapping.file` | File | `Optional.empty()` |  |  |
| `http-server.authentication.krb5.user-mapping.pattern` | String | `Optional.empty()` |  |  |
| `http.authentication.krb5.config` | File |  |  |  |

## MemoryManagerConfig

Source: `core/trino-main/src/main/java/io/trino/memory/MemoryManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `fault-tolerant-execution-coordinator-task-memory` | DataSize | `DataSize.of(2, GIGABYTE)` | Estimated amount of memory a single coordinator task will use when task level retries are used; value is used when allocating nodes for tasks execution |  |
| `fault-tolerant-execution-eager-speculative-tasks-node-memory-overcommit` | DataSize | `DataSize.of(20, GIGABYTE)` |  | Legacy names: `fault-tolerant-execution-eager-speculative-tasks-node_memory-overcommit` |
| `fault-tolerant-execution-memory-requirement-increase-on-worker-crash-enabled` | boolean | `true` | Increase memory requirement for tasks failed due to a suspected worker crash | Legacy names: `fault-tolerant-execution.memory-requirement-increase-on-worker-crash-enabled` |
| `fault-tolerant-execution-task-memory` | DataSize | `DataSize.of(5, GIGABYTE)` | Estimated amount of memory a single task will use when task level retries are used; value is used when allocating nodes for tasks execution |  |
| `fault-tolerant-execution-task-memory-estimation-quantile` | double | `0.9` | What quantile of memory usage of completed tasks to look at when estimating memory usage for upcoming tasks |  |
| `fault-tolerant-execution-task-memory-growth-factor` | double | `3.0` | Factor by which estimated task memory is increased if task execution runs out of memory; value is used allocating nodes for tasks execution |  |
| `fault-tolerant-execution-task-runtime-memory-estimation-overhead` | DataSize | `DataSize.of(1, GIGABYTE)` | Extra memory to account for when estimating actual task runtime memory consumption |  |
| `query.low-memory-killer.policy` | LowMemoryQueryKillerPolicy | `LowMemoryQueryKillerPolicy.TOTAL_RESERVATION_ON_BLOCKED_NODES` |  |  |
| `query.max-memory` | DataSize | `DataSize.of(20, GIGABYTE)` |  |  |
| `query.max-total-memory` | DataSize |  |  |  |
| `task.low-memory-killer.policy` | LowMemoryTaskKillerPolicy | `LowMemoryTaskKillerPolicy.TOTAL_RESERVATION_ON_BLOCKED_NODES` |  |  |

Removed properties, rejected at startup if still set: `experimental.cluster-memory-manager-enabled`, `query.low-memory-killer.delay`, `query.low-memory-killer.enabled`, `resources.reserved-system-memory`

## NodeInventoryConfig

Source: `core/trino-main/src/main/java/io/trino/node/NodeInventoryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `discovery.type` | NodeInventoryType | `NodeInventoryType.ANNOUNCE` |  |  |

## NodeMemoryConfig

Source: `core/trino-main/src/main/java/io/trino/memory/NodeMemoryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `memory.heap-headroom-per-node` | String | `HeapSizeParser.DEFAULT.parse("30%")` | The amount of heap memory to set aside as headroom/buffer (e.g., for untracked allocations) |  |
| `query.max-memory-per-node` | String | `HeapSizeParser.DEFAULT.parse("30%")` |  |  |

Removed properties, rejected at startup if still set: `deprecated.legacy-system-pool-enabled`, `experimental.reserved-pool-disabled`, `experimental.reserved-pool-enabled`, `query.max-memory-per-task`, `query.max-total-memory-per-node`

## NodeSchedulerConfig

Source: `core/trino-main/src/main/java/io/trino/execution/scheduler/NodeSchedulerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `node-scheduler.allowed-no-matching-node-period` | Duration | `new Duration(10, TimeUnit.MINUTES)` | How long scheduler should wait before failing a query for which hard task requirements (e.g. node exposing specific catalog) cannot be satisfied. Relevant for TASK retry policy only. |  |
| `node-scheduler.exhausted-node-wait-period` | Duration | `new Duration(2, TimeUnit.MINUTES)` | Maximum time to wait for resource availability on preferred nodes before scheduling a remotely accessible split on other nodes. Relevant for TASK retry policy only. |  |
| `node-scheduler.include-coordinator` | boolean | `true` |  |  |
| `node-scheduler.max-adjusted-pending-splits-per-task` | int | `2000` |  |  |
| `node-scheduler.max-splits-per-node` | int | `256` |  |  |
| `node-scheduler.max-unacknowledged-splits-per-task` | int | `2000` | Maximum number of leaf splits not yet delivered to a given task |  |
| `node-scheduler.min-candidates` | int | `10` |  |  |
| `node-scheduler.min-pending-splits-per-task` | int | `16` |  | Legacy names: `node-scheduler.max-pending-splits-per-task`, `node-scheduler.max-pending-splits-per-node-per-task`, `node-scheduler.max-pending-splits-per-node-per-stage` |
| `node-scheduler.optimized-local-scheduling` | boolean | `true` |  |  |
| `node-scheduler.policy` | String | `NodeSchedulerPolicy.UNIFORM` |  | Legacy names: `node-scheduler.network-topology` |
| `node-scheduler.splits-balancing-policy` | SplitsBalancingPolicy | `SplitsBalancingPolicy.STAGE` | Strategy for balancing new splits on worker nodes |  |

Removed properties, rejected at startup if still set: `node-scheduler.allocator-type`, `node-scheduler.location-aware-scheduling-enabled`, `node-scheduler.max-absolute-full-nodes-per-query`, `node-scheduler.max-fraction-full-nodes-per-query`, `node-scheduler.multiple-tasks-per-node-enabled`

## NodeSpillConfig

Source: `core/trino-main/src/main/java/io/trino/spiller/NodeSpillConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `max-spill-per-node` | DataSize | `DataSize.of(100, DataSize.Unit.GIGABYTE)` |  | Legacy names: `experimental.max-spill-per-node` |
| `query-max-spill-per-node` | DataSize | `DataSize.of(100, DataSize.Unit.GIGABYTE)` |  | Legacy names: `experimental.query-max-spill-per-node` |
| `spill-compression-codec` | CompressionCodec | `NONE` | Compression codec used for data in spills |  |
| `spill-encryption-enabled` | boolean | `false` |  | Legacy names: `experimental.spill-encryption-enabled` |

Removed properties, rejected at startup if still set: `experimental.spill-compression-enabled`

## OAuth2Config

Source: `core/trino-main/src/main/java/io/trino/server/security/oauth2/OAuth2Config.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.oauth2.additional-audiences` | List<String> | `Collections.emptyList()` | Additional audiences to trust in addition to the Client ID | Legacy names: `http-server.authentication.oauth2.audience` |
| `http-server.authentication.oauth2.challenge-timeout` | Duration | `new Duration(15, TimeUnit.MINUTES)` | Maximum duration of OAuth2 authorization challenge |  |
| `http-server.authentication.oauth2.client-id` | String |  | Client ID |  |
| `http-server.authentication.oauth2.client-secret` | String |  | Client secret | sensitive |
| `http-server.authentication.oauth2.domain-hint` | String | `Optional.empty()` | Domain hint to restrict SSO account selection to a specific domain (e.g. for Azure AD) |  |
| `http-server.authentication.oauth2.issuer` | String |  | The required issuer of a token |  |
| `http-server.authentication.oauth2.jwt-type` | String | `Optional.empty()` | Custom JWT type for server to use |  |
| `http-server.authentication.oauth2.max-clock-skew` | Duration | `new Duration(1, TimeUnit.MINUTES)` | Max clock skew between the Authorization Server and the coordinator |  |
| `http-server.authentication.oauth2.oidc.discovery` | boolean | `true` | Enable OpenID Provider Issuer discovery |  |
| `http-server.authentication.oauth2.principal-field` | String | `"sub"` | The claim to use as the principal |  |
| `http-server.authentication.oauth2.refresh-tokens` | boolean | `false` | Enables OpenID refresh tokens usage |  |
| `http-server.authentication.oauth2.scopes` | Set<String> | `ImmutableSet.of(OPENID_SCOPE)` | Scopes requested by the server during OAuth2 authorization challenge |  |
| `http-server.authentication.oauth2.state-key` | String | `Optional.empty()` | A secret key used by HMAC algorithm to sign the state parameter |  |
| `http-server.authentication.oauth2.user-mapping.file` | File | `Optional.empty()` | File containing rules for mapping user |  |
| `http-server.authentication.oauth2.user-mapping.pattern` | String | `Optional.empty()` | Regex to match against user name |  |

## OidcDiscoveryConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/oauth2/OidcDiscoveryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.oauth2.access-token-issuer` | String | `Optional.empty()` | The required issuer for access tokens |  |
| `http-server.authentication.oauth2.auth-url` | String | `Optional.empty()` | URL of the authorization server's authorization endpoint |  |
| `http-server.authentication.oauth2.jwks-url` | String | `Optional.empty()` | URL of the authorization server's JWKS (JSON Web Key Set) endpoint |  |
| `http-server.authentication.oauth2.oidc.discovery.timeout` | Duration | `new Duration(30, SECONDS)` | OpenID Connect discovery timeout |  |
| `http-server.authentication.oauth2.oidc.use-userinfo-endpoint` | boolean | `true` | Use userinfo endpoint from OpenID connect metadata document |  |
| `http-server.authentication.oauth2.token-url` | String | `Optional.empty()` | URL of the authorization server's token endpoint |  |
| `http-server.authentication.oauth2.userinfo-url` | String | `Optional.empty()` | URL of the userinfo endpoint |  |

## OptimizerConfig

Source: `core/trino-main/src/main/java/io/trino/sql/planner/OptimizerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `adaptive-partial-aggregation.enabled` | boolean | `true` |  |  |
| `adaptive-partial-aggregation.unique-rows-ratio-threshold` | double | `0.8` | Ratio between aggregation output and input rows above which partial aggregation might be adaptively turned off |  |
| `collect-plan-statistics-for-all-queries` | boolean | `false` | Collect plan statistics for non-EXPLAIN queries |  |
| `colocated-joins-enabled` | boolean | `true` | Use a colocated join when possible |  |
| `cpu-cost-weight` | double | `75` |  |  |
| `distributed-sort` | boolean |  |  |  |
| `enable-forced-exchange-below-group-id` | boolean | `true` |  |  |
| `enable-stats-calculator` | boolean | `true` |  | Legacy names: `experimental.enable-stats-calculator` |
| `iterative-optimizer-timeout` | Duration | `new Duration(3, MINUTES)` |  | Legacy names: `experimental.iterative-optimizer-timeout` |
| `join-distribution-type` | JoinDistributionType | `JoinDistributionType.AUTOMATIC` |  |  |
| `join-max-broadcast-table-size` | DataSize | `DataSize.of(100, MEGABYTE)` | Maximum estimated size of a table that can be broadcast when using automatic join type selection |  |
| `memory-cost-weight` | double | `10` |  |  |
| `network-cost-weight` | double | `15` |  |  |
| `optimizer.allow-unsafe-pushdown` | boolean | `false` | Allow pushing down expressions that mail fail for some inputs |  |
| `optimizer.complex-expression-pushdown.enabled` | boolean | `true` |  |  |
| `optimizer.default-filter-factor-enabled` | boolean | `false` |  |  |
| `optimizer.dictionary-aggregation` | boolean | `false` |  |  |
| `optimizer.distinct-aggregations-strategy` | DistinctAggregationsStrategy |  | Strategy to use for distinct aggregations |  |
| `optimizer.enable-intermediate-aggregations` | boolean | `false` |  |  |
| `optimizer.experimental-max-prefetched-information-schema-prefixes` | int | `100` | Experimental: maximum number of internal "prefixes" to be prefetched when optimizing information_schema queries | hidden |
| `optimizer.filter-conjunction-independence-factor` | double | `0.75` | Scales the strength of independence assumption for selectivity estimates of the conjunction of multiple filters |  |
| `optimizer.force-single-node-output` | boolean | `false` |  |  |
| `optimizer.ignore-downstream-preferences` | boolean | `false` |  |  |
| `optimizer.ignore-stats-calculator-failures` | boolean | `true` | Ignore statistics calculator failures |  |
| `optimizer.join-multi-clause-independence-factor` | double | `0.25` | Scales the strength of independence assumption for selectivity estimates of multi-clause joins |  |
| `optimizer.join-partitioned-build-min-row-count` | long | `1_000_000L` | Minimum number of join build side rows required to use partitioned join lookup |  |
| `optimizer.join-reordering-strategy` | JoinReorderingStrategy | `JoinReorderingStrategy.AUTOMATIC` | The strategy to use for reordering joins |  |
| `optimizer.max-reordered-joins` | int | `8` | The maximum number of tables to reorder in cost-based join reordering |  |
| `optimizer.merge-project-with-values` | boolean | `true` |  |  |
| `optimizer.min-input-rows-per-task` | long | `10_000_000L` | Minimum input rows required per task. This will help optimizer determine hash partition count for joins and aggregations |  |
| `optimizer.min-input-size-per-task` | DataSize | `DataSize.of(5, GIGABYTE)` | Minimum input data size required per task. This will help optimizer determine hash partition count for joins and aggregations |  |
| `optimizer.non-estimatable-predicate-approximation.enabled` | boolean | `true` | Approximate the cost of filters which cannot be accurately estimated even with complete statistics |  |
| `optimizer.optimize-duplicate-insensitive-joins` | boolean | `true` |  |  |
| `optimizer.optimize-metadata-queries` | boolean | `false` |  |  |
| `optimizer.optimize-top-n-ranking` | boolean | `true` |  | Legacy names: `optimizer.optimize-top-n-row-number` |
| `optimizer.pre-aggregate-case-aggregations.enabled` | boolean | `true` | Pre-aggregate rows before GROUP BY with multiple CASE aggregations on same column |  |
| `optimizer.predicate-pushdown-use-table-properties` | boolean | `true` |  |  |
| `optimizer.prefer-partial-aggregation` | boolean | `true` |  |  |
| `optimizer.push-aggregation-through-outer-join` | boolean | `true` |  | Legacy names: `optimizer.push-aggregation-through-join` |
| `optimizer.push-filter-into-values-max-row-count` | int | `100` | Maximum number of rows in values for which filter is pushed down into values |  |
| `optimizer.push-partial-aggregation-through-join` | boolean | `true` |  |  |
| `optimizer.push-table-write-through-union` | boolean | `true` |  |  |
| `optimizer.rewrite-filtering-semi-join-to-inner-join` | boolean | `true` |  |  |
| `optimizer.skip-redundant-sort` | boolean | `true` |  |  |
| `optimizer.table-scan-node-partitioning-min-bucket-to-task-ratio` | double | `0.5` | Min table scan bucket to task ratio for which plan will be adopted to node pre-partitioned tables |  |
| `optimizer.use-cost-based-partitioning` | boolean | `true` | When enabled the cost based optimizer is used to determine if repartitioning the output of an already partitioned stage is necessary |  |
| `optimizer.use-exact-partitioning` | boolean | `false` | When enabled this forces data repartitioning unless the partitioning of upstream stage matches exactly what downstream stage expects |  |
| `optimizer.use-legacy-window-filter-pushdown` | boolean | `false` |  |  |
| `optimizer.use-table-scan-node-partitioning` | boolean | `true` | Adapt plan to node pre-partitioned tables | Legacy names: `optimizer.plan-with-table-node-partitioning` |
| `spatial-joins-enabled` | boolean | `true` | Use spatial index for spatial joins when possible |  |
| `statistics-precalculation-for-pushdown.enabled` | boolean | `true` |  |  |
| `use-preferred-write-partitioning` | boolean | `true` |  |  |

Removed properties, rejected at startup if still set: `adaptive-partial-aggregation.min-rows`, `optimizer.optimize-hash-generation`, `optimizer.optimize-mixed-distinct-aggregations`, `optimizer.use-mark-distinct`, `preferred-write-partitioning-min-number-of-partitions`

## PasswordAuthenticatorConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/PasswordAuthenticatorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.password.user-mapping.file` | File | `Optional.empty()` |  |  |
| `http-server.authentication.password.user-mapping.pattern` | String | `Optional.empty()` |  |  |
| `password-authenticator.config-files` | List<String> | `ImmutableList.of(new File("etc/password-authenticator.properties"))` | Ordered list of password authenticator config files |  |

## ProtocolConfig

Source: `core/trino-main/src/main/java/io/trino/server/ProtocolConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `protocol.v1.alternate-header-name` | String |  | Alternate header name for V1 protocol | deprecated |
| `protocol.v1.prepared-statement-compression.length-threshold` | int | `2 * 1024` | Compression is applied to prepared statements longer than the configured value |  |
| `protocol.v1.prepared-statement-compression.min-gain` | int | `512` | Prepared statement compression is not applied if the size gain is less than the configured value |  |

## QueryDataEncodingConfig

Source: `core/trino-main/src/main/java/io/trino/server/protocol/spooling/QueryDataEncodingConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `protocol.spooling.encoding.compression.threshold` | DataSize | `DataSize.of(8, KILOBYTE)` | Do not compress segments smaller than threshold |  |
| `protocol.spooling.encoding.json+lz4.enabled` | boolean | `true` | Enable LZ4 compressed json spooled encoding |  |
| `protocol.spooling.encoding.json+zstd.enabled` | boolean | `true` | Enable Zstd compressed json spooled encoding |  |
| `protocol.spooling.encoding.json.enabled` | boolean | `true` | Enable uncompressed json spooled encoding |  |

## QueryManagerConfig

Source: `core/trino-main/src/main/java/io/trino/execution/QueryManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `direct-trino-client.fault-tolerant-execution-enabled` | boolean | `true` | Allow DirectTrinoClient to consume results of queries running under fault-tolerant execution; when disabled such queries are forced to retry_policy=NONE |  |
| `fault-tolerant-execution-adaptive-join-reordering-enabled` | boolean | `false` | Reorder partitioned join based on run time stats in fault tolerant execution |  |
| `fault-tolerant-execution-adaptive-join-reordering-min-size-threshold` | DataSize | `DataSize.of(5, GIGABYTE)` | The minimum size of the right side of join to consider reordering |  |
| `fault-tolerant-execution-adaptive-join-reordering-size-difference-ratio` | double | `1.5` | The ratio of difference in estimated size of right and left side of join to consider reordering |  |
| `fault-tolerant-execution-adaptive-query-planning-enabled` | boolean | `true` | Enable adaptive query planning for the fault tolerant execution |  |
| `fault-tolerant-execution-arbitrary-distribution-compute-task-target-size-growth-factor` | double | `1.26` | Growth factor for adaptive sizing of non-writer tasks of arbitrary distribution for fault-tolerant execution |  |
| `fault-tolerant-execution-arbitrary-distribution-compute-task-target-size-growth-period` | int | `64` | The number of tasks created for any given non-writer stage of arbitrary distribution before task size is increased |  |
| `fault-tolerant-execution-arbitrary-distribution-compute-task-target-size-max` | DataSize | `DataSize.of(50, GIGABYTE)` | Max target input size for non-writer task of arbitrary distribution of fault-tolerant execution |  |
| `fault-tolerant-execution-arbitrary-distribution-compute-task-target-size-min` | DataSize | `DataSize.of(512, MEGABYTE)` | Initial/min target input size for non-writer tasks of arbitrary distribution of fault-tolerant execution |  |
| `fault-tolerant-execution-arbitrary-distribution-write-task-target-size-growth-factor` | double | `1.26` | Growth factor for adaptive sizing of writer tasks of arbitrary distribution for fault-tolerant execution |  |
| `fault-tolerant-execution-arbitrary-distribution-write-task-target-size-growth-period` | int | `64` | The number of tasks created for any given writer stage of arbitrary distribution before task size is increased |  |
| `fault-tolerant-execution-arbitrary-distribution-write-task-target-size-max` | DataSize | `DataSize.of(50, GIGABYTE)` | Max target input size for writer tasks of arbitrary distribution of fault-tolerant execution |  |
| `fault-tolerant-execution-arbitrary-distribution-write-task-target-size-min` | DataSize | `DataSize.of(4, GIGABYTE)` | Initial/min target input size for writer tasks of arbitrary distribution of fault-tolerant execution |  |
| `fault-tolerant-execution-hash-distribution-compute-task-target-size` | DataSize | `DataSize.of(512, MEGABYTE)` | Target input size for non-writer tasks of hash distribution of fault-tolerant execution |  |
| `fault-tolerant-execution-hash-distribution-compute-task-to-node-min-ratio` | double | `2.0` | Minimal ratio of tasks count vs cluster nodes count for hash distributed compute stage in fault-tolerant execution |  |
| `fault-tolerant-execution-hash-distribution-write-task-target-max-count` | int | `2000` | Soft upper bound on number of writer tasks in a stage of hash distribution of fault-tolerant execution |  |
| `fault-tolerant-execution-hash-distribution-write-task-target-size` | DataSize | `DataSize.of(4, GIGABYTE)` | Target input size of writer tasks of hash distribution of fault-tolerant execution |  |
| `fault-tolerant-execution-hash-distribution-write-task-to-node-min-ratio` | double | `2.0` | Minimal ratio of tasks count vs cluster nodes count for hash distributed writer stage in fault-tolerant execution |  |
| `fault-tolerant-execution-max-partition-count` | int | `50` | Maximum number of partitions for distributed joins and aggregations executed with fault tolerant execution enabled | Legacy names: `fault-tolerant-execution-partition-count` |
| `fault-tolerant-execution-max-task-split-count` | int | `2048` | Maximal number of splits for a single fault tolerant task (count based) |  |
| `fault-tolerant-execution-min-partition-count` | int | `4` | Minimum number of partitions for distributed joins and aggregations executed with fault tolerant execution enabled |  |
| `fault-tolerant-execution-min-partition-count-for-write` | int | `50` | Minimum number of partitions for distributed joins and aggregations in write queries executed with fault tolerant execution enabled |  |
| `fault-tolerant-execution-min-source-stage-progress` | double | `0.2` | Minimal progress of source stage to consider scheduling of parent stage |  |
| `fault-tolerant-execution-runtime-adaptive-partitioning-enabled` | boolean | `false` |  |  |
| `fault-tolerant-execution-runtime-adaptive-partitioning-max-task-size` | DataSize | `DataSize.of(12, GIGABYTE)` | Max average task input size when deciding runtime adaptive partitioning |  |
| `fault-tolerant-execution-runtime-adaptive-partitioning-partition-count` | int | `FAULT_TOLERANT_EXECUTION_MAX_PARTITION_COUNT_LIMIT` | The partition count to use for runtime adaptive partitioning when enabled |  |
| `fault-tolerant-execution-small-stage-estimation-enabled` | boolean | `true` | Enable small stage estimation heuristic, used for more aggresive speculative stage scheduling |  |
| `fault-tolerant-execution-small-stage-estimation-threshold` | DataSize | `DataSize.of(20, GIGABYTE)` | Threshold until which stage is considered small |  |
| `fault-tolerant-execution-small-stage-require-no-more-partitions` | boolean | `false` | Is it required for all stage partitions (tasks) to be enumerated for stage to be used in heuristic to determine if parent stage is small |  |
| `fault-tolerant-execution-small-stage-source-size-multiplier` | double | `1.2` | Multiplier used for heuristic estimation is stage is small; the bigger the more conservative estimation is |  |
| `fault-tolerant-execution-stage-estimation-for-eager-parent-enabled` | boolean | `true` | Enable aggressive stage output size estimation heuristic for children of stages to be executed eagerly |  |
| `fault-tolerant-execution-standard-split-size` | DataSize | `DataSize.of(64, MEGABYTE)` | Standard split size for a single fault tolerant task (split weight aware) |  |
| `fault-tolerant-execution-task-descriptor-storage-high-water-mark` | DataSize | `DataSize.ofBytes((long) (DEFAULT_TASK_DESCRIPTOR_STORAGE_MAX_MEMORY.toBytes() * 0.6))` | Compress the storage when task descriptors in memory are above given data size |  |
| `fault-tolerant-execution-task-descriptor-storage-low-water-mark` | DataSize | `DataSize.ofBytes((long) (DEFAULT_TASK_DESCRIPTOR_STORAGE_MAX_MEMORY.toBytes() * 0.4))` | Do not compress the storage when tasks descriptors in memory are below given data size |  |
| `fault-tolerant-execution-task-descriptor-storage-max-memory` | DataSize | `DEFAULT_TASK_DESCRIPTOR_STORAGE_MAX_MEMORY` | Maximum amount of memory to be used to store task descriptors for fault tolerant queries on coordinator |  |
| `max-tasks-waiting-for-execution-per-query` | int | `10` | Maximum number of tasks waiting to be scheduled per query. Split enumeration is paused by the scheduler when this threshold is crossed |  |
| `max-tasks-waiting-for-node-per-query` | int | `50` | Maximum possible number of tasks waiting for node allocation per query before scheduling of new tasks for query is paused | Legacy names: `max-tasks-waiting-for-node-per-stage` |
| `query-manager.required-workers` | int | `1` | Minimum number of active workers that must be available before a query will start |  |
| `query-manager.required-workers-max-wait` | Duration | `new Duration(5, MINUTES)` | Maximum time to wait for minimum number of workers before the query is failed |  |
| `query-retry-attempts` | int | `4` |  |  |
| `query.client.timeout` | Duration | `new Duration(5, MINUTES)` |  |  |
| `query.determine-partition-count-for-write-enabled` | boolean | `false` | Determine the number of partitions based on amount of data read and processed by the query for write queries |  |
| `query.dispatcher-query-pool-size` | String | `DISPATCHER_THREADPOOL_MAX_SIZE` |  |  |
| `query.execution-policy` | String | `"phased"` |  |  |
| `query.executor-pool-size` | String | `1000` |  |  |
| `query.manager-executor-pool-size` | String | `5` |  |  |
| `query.max-concurrent-queries` | int | `1000` |  | deprecated |
| `query.max-cpu-time` | Duration | `new Duration(1_000_000_000, TimeUnit.DAYS)` |  |  |
| `query.max-execution-time` | Duration | `new Duration(100, TimeUnit.DAYS)` |  |  |
| `query.max-hash-partition-count` | int | `100` | Maximum number of partitions for distributed joins and aggregations | Legacy names: `query.hash-partition-count` |
| `query.max-history` | int | `100` |  |  |
| `query.max-length` | int | `1_000_000` |  |  |
| `query.max-output-data-size` | DataSize | `Optional.empty()` |  |  |
| `query.max-planning-time` | Duration | `new Duration(10, MINUTES)` |  |  |
| `query.max-queued-queries` | int | `5000` |  | deprecated |
| `query.max-run-time` | Duration | `new Duration(100, TimeUnit.DAYS)` |  |  |
| `query.max-scan-physical-bytes` | DataSize | `Optional.empty()` |  |  |
| `query.max-split-manager-callback-threads` | String | `100` | The maximum number of threads allowed to run splits generation callbacks concurrently |  |
| `query.max-stage-count` | int | `150` |  |  |
| `query.max-state-machine-callback-threads` | String | `5` | The maximum number of threads allowed to run query and stage state machine listener callbacks concurrently for each query |  |
| `query.max-write-physical-size` | DataSize | `Optional.empty()` |  |  |
| `query.max-writer-task-count` | int | `100` | Maximum number of tasks that will participate in writing data |  |
| `query.min-expire-age` | Duration | `new Duration(15, MINUTES)` |  |  |
| `query.min-hash-partition-count` | int | `4` | Minimum number of partitions for distributed joins and aggregations |  |
| `query.min-hash-partition-count-for-write` | int | `50` | Minimum number of partitions for distributed joins and aggregations in write queries |  |
| `query.min-schedule-split-batch-size` | int | `100` |  |  |
| `query.remote-task.enable-adaptive-request-size` | boolean | `true` |  |  |
| `query.remote-task.guaranteed-splits-per-task` | int | `3` |  |  |
| `query.remote-task.max-callback-threads` | String | `1000` |  |  |
| `query.remote-task.max-error-duration` | Duration | `new Duration(1, MINUTES)` |  |  |
| `query.remote-task.max-request-size` | DataSize | `DataSize.of(8, MEGABYTE)` |  |  |
| `query.remote-task.request-size-headroom` | DataSize | `DataSize.of(2, MEGABYTE)` |  |  |
| `query.reported-rule-stats-limit` | int | `10` |  |  |
| `query.schedule-split-batch-size` | int | `1000` |  |  |
| `query.stage-count-warning-threshold` | int | `50` | Emit a warning when stage count exceeds this threshold |  |
| `retry-delay-scale-factor` | double | `2.0` | Factor by which retry delay is scaled on subsequent failures |  |
| `retry-initial-delay` | Duration | `new Duration(10, SECONDS)` | Initial delay before initiating a retry attempt. Delay increases exponentially for each subsequent attempt up to 'retry_max_delay' |  |
| `retry-max-delay` | Duration | `new Duration(1, MINUTES)` | Maximum delay before initiating a retry attempt. Delay increases exponentially for each subsequent attempt starting from 'retry_initial_delay' |  |
| `retry-policy` | RetryPolicy | `RetryPolicy.NONE` |  |  |
| `retry-policy.allowed` | Set<RetryPolicy> | `EnumSet.allOf(RetryPolicy.class)` | Retry policies that are allowed to be used |  |
| `source-pages-validation-enabled` | boolean | `true` | Runtime validation of blocks in source pages |  |
| `task-retry-attempts-per-task` | int | `4` |  |  |

Removed properties, rejected at startup if still set: `experimental.big-query-initial-hash-partitions`, `experimental.fault-tolerant-execution-force-preferred-write-partitioning-enabled`, `experimental.max-concurrent-big-queries`, `experimental.max-queued-big-queries`, `fault-tolerant-execution-target-task-input-size`, `fault-tolerant-execution-target-task-split-count`, `query-manager.initialization-required-workers`, `query-manager.initialization-timeout`, `query.initial-hash-partitions`, `query.max-age`, `query.max-pending-splits-per-node`, `query.queue-config-file`, `query.remote-task.max-consecutive-error-count`, `query.remote-task.min-error-duration`, `retry-attempts`

## QueryMonitorConfig

Source: `core/trino-main/src/main/java/io/trino/event/QueryMonitorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `event.max-output-stage-size` | DataSize | `DataSize.of(16, Unit.MEGABYTE)` |  |  |

## RefreshTokensConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/oauth2/RefreshTokensConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.oauth2.refresh-tokens.issued-token.audience` | String | `"Trino_coordinator"` | Audience representing this coordinator instance, that will be used in issued token |  |
| `http-server.authentication.oauth2.refresh-tokens.issued-token.issuer` | String | `"Trino_coordinator"` | Issuer representing this coordinator instance, that will be used in issued token. In addition current Version will be added to it |  |
| `http-server.authentication.oauth2.refresh-tokens.issued-token.timeout` | Duration | `Duration.succinctDuration(1, HOURS)` | Expiration time for issued token. It needs to be equal or lower than duration of refresh token issued by IdP |  |
| `http-server.authentication.oauth2.refresh-tokens.secret-key` | String |  | Base64 encoded secret key used to encrypt generated token | sensitive |

## SecurityConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/SecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.allow-insecure-over-http` | boolean | `true` | Insecure authentication over HTTP (non-secure) enabled |  |
| `http-server.authentication.type` | List<String> |  | Ordered list of authentication types |  |
| `management.user` | String | `Optional.empty()` | Optional fixed user for all requests to management endpoints |  |
| `management.user.https-enabled` | boolean | `false` | Use fixed management user for secure HTTPS requests |  |

Removed properties, rejected at startup if still set: `dispatcher.forwarded-header`, `http-server.authentication.allow-forwarded-https`, `http.server.authentication.enabled`

## ServerConfig

Source: `core/trino-main/src/main/java/io/trino/server/ServerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `coordinator` | boolean | `true` |  |  |
| `experimental.concurrent-startup` | boolean | `false` | Parallelize work during server startup |  |
| `http.include-exception-in-response` | boolean | `true` |  |  |
| `query-results.compression-enabled` | boolean | `true` |  |  |
| `query.info-url-template` | String | `Optional.empty()` |  |  |
| `shutdown.grace-period` | Duration | `new Duration(2, MINUTES)` |  |  |

## ServerPluginsProviderConfig

Source: `core/trino-main/src/main/java/io/trino/server/ServerPluginsProviderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `plugin.dir` | List<Path> | `ImmutableList.of(Path.of("plugin"))` | Comma separated list of root directories where the plugins are located |  |

## SpoolingConfig

Source: `core/trino-main/src/main/java/io/trino/server/protocol/spooling/SpoolingConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `protocol.spooling.initial-segment-size` | DataSize | `DataSize.of(8, MEGABYTE)` | Initial size of the spooled segments in bytes |  |
| `protocol.spooling.inlining.enabled` | boolean | `true` | Allow spooling protocol to inline data |  |
| `protocol.spooling.inlining.max-rows` | long | `50_000` | Maximum number of rows that are allowed to be inlined per worker |  |
| `protocol.spooling.inlining.max-size` | DataSize | `DataSize.of(3, MEGABYTE)` | Maximum size of rows that are allowed to be inlined per worker |  |
| `protocol.spooling.max-segment-size` | DataSize | `DataSize.of(16, MEGABYTE)` | Maximum size of the spooled segments in bytes | Legacy names: `protocol.spooling.maximum-segment-size` |
| `protocol.spooling.retrieval-mode` | SegmentRetrievalMode | `SegmentRetrievalMode.STORAGE` | Determines how the client will retrieve the segment |  |
| `protocol.spooling.shared-secret-key` | String |  | 256 bit, base64-encoded secret key used to secure segment identifiers | sensitive |

## SpoolingEnabledConfig

Source: `core/trino-main/src/main/java/io/trino/server/protocol/spooling/SpoolingEnabledConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `protocol.spooling.enabled` | boolean | `false` | Enable spooling protocol |  |
| `protocol.spooling.unsupported-warning.enabled` | boolean | `false` | Generates a warning when a client lacks spooling support and connects to a spooling-enabled server |  |

## SqlEnvironmentConfig

Source: `core/trino-main/src/main/java/io/trino/sql/SqlEnvironmentConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `sql.default-catalog` | String | `Optional.empty()` |  |  |
| `sql.default-function-catalog` | String | `Optional.empty()` |  |  |
| `sql.default-function-schema` | String | `Optional.empty()` |  |  |
| `sql.default-schema` | String | `Optional.empty()` |  |  |
| `sql.forced-session-time-zone` | String | `Optional.empty()` | User session time zone overriding value sent by client |  |
| `sql.path` | String | `""` |  |  |

## StableHostAddressProviderConfig

Source: `core/trino-main/src/main/java/io/trino/execution/scheduler/StableHostAddressProviderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `node-scheduler.cache-preferred-hosts-count` | int | `2` | Number of preferred worker hosts the scheduler derives from a split's cache key |  |

## StaticCatalogManagerConfig

Source: `core/trino-main/src/main/java/io/trino/connector/StaticCatalogManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `catalog.config-dir` | File | `new File("etc/catalog/")` |  | Legacy names: `plugin.config-dir` |
| `catalog.disabled-catalogs` | List<String> | `ImmutableList.of()` |  |  |

## StaticOAuth2ServerConfig

Source: `core/trino-main/src/main/java/io/trino/server/security/oauth2/StaticOAuth2ServerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server.authentication.oauth2.access-token-issuer` | String | `Optional.empty()` | The required issuer for access tokens |  |
| `http-server.authentication.oauth2.auth-url` | URI |  | URL of the authorization server's authorization endpoint |  |
| `http-server.authentication.oauth2.end-session-url` | URI | `Optional.empty()` | URL of the end session endpoint |  |
| `http-server.authentication.oauth2.jwks-url` | URI |  | URL of the authorization server's JWKS (JSON Web Key Set) endpoint |  |
| `http-server.authentication.oauth2.token-url` | URI |  | URL of the authorization server's token endpoint |  |
| `http-server.authentication.oauth2.userinfo-url` | URI | `Optional.empty()` | URL of the userinfo endpoint |  |

## SubnetTopologyConfig

Source: `core/trino-main/src/main/java/io/trino/execution/scheduler/SubnetTopologyConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `node-scheduler.network-topology.subnet.cidr-prefix-lengths` | List<Integer> | `ImmutableList.of()` |  |  |
| `node-scheduler.network-topology.subnet.ip-address-protocol` | SubnetBasedTopology.AddressProtocol | `IPv4` |  |  |

## TaskManagerConfig

Source: `core/trino-main/src/main/java/io/trino/execution/TaskManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `driver.max-page-partitioning-buffer-size` | DataSize | `DataSize.of(32, Unit.MEGABYTE)` |  |  |
| `driver.page-partitioning-buffer-pool-size` | int | `8` | Maximum number of free buffers in the per task partitioned page buffer pool. Setting this to zero effectively disables the pool |  |
| `experimental.thread-per-driver-scheduler-enabled` | boolean | `true` |  |  |
| `sink.max-broadcast-buffer-size` | DataSize | `DataSize.of(200, Unit.MEGABYTE)` |  |  |
| `sink.max-buffer-size` | DataSize | `DataSize.of(32, Unit.MEGABYTE)` |  |  |
| `task.client.timeout` | Duration | `new Duration(2, TimeUnit.MINUTES)` |  |  |
| `task.concurrency` | int | `clamp(nextPowerOfTwo(getAvailablePhysicalProcessorCount()), 2, 32)` | Default number of local parallel jobs per worker |  |
| `task.cpu-timer-enabled` | boolean | `true` |  |  |
| `task.driver-timeout-threads` | String | `5` | Number of threads used for timing out blocked drivers if the timeout is set |  |
| `task.http-response-threads` | String | `100` |  |  |
| `task.http-timeout-threads` | String | `3` |  |  |
| `task.info-update-interval` | Duration | `new Duration(3, TimeUnit.SECONDS)` | Interval between updating task data |  |
| `task.info.max-age` | Duration | `new Duration(5, TimeUnit.MINUTES)` |  |  |
| `task.initial-splits-per-node` | int | `maxWorkerThreads` |  |  |
| `task.interrupt-stuck-split-tasks-detection-interval` | Duration | `new Duration(2, TimeUnit.MINUTES)` | Interval between detecting stuck split |  |
| `task.interrupt-stuck-split-tasks-enabled` | boolean | `true` |  |  |
| `task.interrupt-stuck-split-tasks-timeout` | Duration | `new Duration(15, TimeUnit.MINUTES)` | Interrupt task processing thread after this timeout if the thread is stuck in certain external libraries used by Trino functions |  |
| `task.interrupt-stuck-split-tasks-warning-threshold` | Duration | `new Duration(10, TimeUnit.MINUTES)` | Print out call stacks and generate JMX metrics for splits running longer than the threshold |  |
| `task.level-time-multiplier` | BigDecimal | `TWO` | Factor that determines the target scheduled time for a level relative to the next |  |
| `task.max-drivers-per-task` | int | `Integer.MAX_VALUE` | Maximum number of drivers a task can run |  |
| `task.max-index-memory` | DataSize | `DataSize.of(64, Unit.MEGABYTE)` |  |  |
| `task.max-local-exchange-buffer-size` | DataSize | `DataSize.of(128, Unit.MEGABYTE)` |  |  |
| `task.max-partial-aggregation-memory` | DataSize | `DataSize.of(16, Unit.MEGABYTE)` |  |  |
| `task.max-partial-top-n-memory` | DataSize | `DataSize.of(16, Unit.MEGABYTE)` |  |  |
| `task.max-worker-threads` | String | `Runtime.getRuntime().availableProcessors() * 2` |  |  |
| `task.max-writer-count` | int | `clamp(nextPowerOfTwo(getAvailablePhysicalProcessorCount() * 2), 2, MAX_WRITER_COUNT)` | Maximum number of local parallel table writers per task when either task writer scaling or preferred partitioning is used |  |
| `task.min-drivers` | int | `0` |  |  |
| `task.min-drivers-per-task` | int | `3` | Minimum number of drivers guaranteed to run per task given there is sufficient work to do |  |
| `task.min-writer-count` | int | `1` | Minimum number of local parallel table writers per task when preferred partitioning and task writer scaling are not used |  |
| `task.per-operator-cpu-timer-enabled` | boolean | `true` |  |  |
| `task.scale-writers.enabled` | boolean | `true` | Scale the number of concurrent table writers per task based on throughput |  |
| `task.scale-writers.max-writer-memory-percentage` | double | `70.0` | Maximum percentage of memory per node that can be used by task scale writers before stopping writer scaling |  |
| `task.share-index-loading` | boolean | `false` |  |  |
| `task.split-concurrency-adjustment-interval` | Duration | `new Duration(100, TimeUnit.MILLISECONDS)` |  |  |
| `task.status-refresh-max-wait` | Duration | `new Duration(1, TimeUnit.SECONDS)` |  |  |
| `task.task-notification-threads` | String | `5` | Number of threads used for internal task event notifications |  |
| `task.task-yield-threads` | String | `3` | Number of threads used for setting yield signals |  |
| `task.termination-timeout` | Duration | `new Duration(1, TimeUnit.MINUTES)` | Maximum duration to wait for a task to complete termination before failing the task on the coordinator |  |

Removed properties, rejected at startup if still set: `experimental.big-query-max-task-memory`, `sink.new-implementation`, `task.http-notification-threads`, `task.info-refresh-max-wait`, `task.legacy-scheduling-behavior`, `task.level-absolute-priority`, `task.max-memory`, `task.operator-pre-allocated-memory`, `task.shard.max-threads`, `task.statistics-cpu-timer-enabled`, `task.verbose-stats`

## TestingWarningCollectorConfig

Source: `core/trino-main/src/main/java/io/trino/testing/TestingWarningCollectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `testing-warning-collector.add-warnings` | boolean | `false` | Adds a warning each time getWarnings is called |  |
| `testing-warning-collector.preloaded-warnings` | int | `0` | Preloads warning collector with test warnings |  |

## TopologyAwareNodeSelectorConfig

Source: `core/trino-main/src/main/java/io/trino/execution/scheduler/TopologyAwareNodeSelectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `node-scheduler.network-topology.segments` | List<String> | `ImmutableList.of("machine")` |  |  |
| `node-scheduler.network-topology.type` | TopologyType | `TopologyType.FLAT` |  |  |

## TopologyFileConfig

Source: `core/trino-main/src/main/java/io/trino/execution/scheduler/TopologyFileConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `node-scheduler.network-topology.file` | File |  |  |  |
| `node-scheduler.network-topology.refresh-period` | Duration | `new Duration(5, MINUTES)` |  |  |

## TransactionManagerConfig

Source: `core/trino-main/src/main/java/io/trino/transaction/TransactionManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `transaction.idle-check-interval` | Duration | `new Duration(1, TimeUnit.MINUTES)` | Time interval between idle transactions checks |  |
| `transaction.idle-timeout` | Duration | `new Duration(5, TimeUnit.MINUTES)` | Amount of time before an inactive transaction is considered expired |  |
| `transaction.max-finishing-concurrency` | int | `1` | Maximum parallelism for committing or aborting a transaction |  |

## WarningCollectorConfig

Source: `core/trino-main/src/main/java/io/trino/execution/warnings/WarningCollectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `warning-collector.max-warnings` | int | `Integer.MAX_VALUE` |  |  |

## WebUiAuthenticationConfig

Source: `core/trino-main/src/main/java/io/trino/server/ui/WebUiAuthenticationConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `web-ui.authentication.type` | String |  | Authentication type for the web ui |  |

## WebUiConfig

Source: `core/trino-main/src/main/java/io/trino/server/ui/WebUiConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `web-ui.enabled` | boolean | `true` |  |  |
| `web-ui.legacy.enabled` | boolean | `false` |  |  |
| `web-ui.preview.enabled` | boolean | `true` |  | deprecated |
