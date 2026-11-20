# Configuration properties: `plugin/trino-iceberg`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [GoogleSecurityConfig](#googlesecurityconfig), [IcebergConfig](#icebergconfig), [IcebergEncryptionConfig](#icebergencryptionconfig), [IcebergGlueCatalogConfig](#iceberggluecatalogconfig), [IcebergHiveCatalogConfig](#iceberghivecatalogconfig), [IcebergJdbcCatalogConfig](#icebergjdbccatalogconfig), [IcebergNessieCatalogConfig](#icebergnessiecatalogconfig), [IcebergRestCatalogConfig](#icebergrestcatalogconfig), [IcebergRestCatalogSigV4Config](#icebergrestcatalogsigv4config), [IcebergSecurityConfig](#icebergsecurityconfig), [IcebergSnowflakeCatalogConfig](#icebergsnowflakecatalogconfig), [OAuth2SecurityConfig](#oauth2securityconfig)

## GoogleSecurityConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/rest/GoogleSecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.rest-catalog.google-json-key` | String |  | Google Cloud service account key in JSON format, used to authenticate with the Iceberg REST catalog | sensitive |
| `iceberg.rest-catalog.google-json-key-file-path` | String |  | Path to a Google Cloud service account key file in JSON format, used to authenticate with the Iceberg REST catalog |  |
| `iceberg.rest-catalog.google-project-id` | String |  | Google project ID |  |

## IcebergConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/IcebergConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.add-files-procedure.enabled` | boolean | `false` | Allow users to call the add_files procedure | Legacy names: `iceberg.add_files-procedure.enabled` |
| `iceberg.allowed-extra-properties` | List<String> | `ImmutableList.of()` | List of extra properties that are allowed to be set on Iceberg tables |  |
| `iceberg.bucket-execution` | boolean | `true` | Enable bucket-aware execution: use physical bucketing information to optimize queries |  |
| `iceberg.catalog.type` | CatalogType | `HIVE_METASTORE` |  |  |
| `iceberg.compression-codec` | HiveCompressionOption | `HiveCompressionOption.ZSTD` |  |  |
| `iceberg.delete-after-commit-enabled` | Boolean | `Optional.empty()` | Whether to delete old tracked metadata files after each table commit |  |
| `iceberg.delete-schema-locations-fallback` | boolean | `false` | Whether schema locations should be deleted when Trino can't determine whether they contain external files. | Legacy names: `hive.delete-schema-locations-fallback` |
| `iceberg.domain-compaction-threshold` | int | `1000` | Maximum ranges to allow in a tuple domain without compacting it |  |
| `iceberg.dynamic-filtering.wait-timeout` | Duration | `new Duration(1, SECONDS)` | Duration to wait for completion of dynamic filters during split generation |  |
| `iceberg.expire-snapshots.min-retention` | Duration | `new Duration(7, DAYS)` | Minimal retention period for expire_snapshot procedure | Legacy names: `iceberg.expire_snapshots.min-retention` |
| `iceberg.extended-statistics.collect-on-write` | boolean | `true` |  |  |
| `iceberg.file-delete-threads` | String | `Runtime.getRuntime().availableProcessors() * 2` | Number of threads to use for deleting files when running the `expire_snapshots` or `remove_orphan_files` procedure, or when executing `DROP TABLE` queries |  |
| `iceberg.file-format` | IcebergFileFormat | `PARQUET` |  |  |
| `iceberg.format-version` | int | `2` | Default Iceberg table format version |  |
| `iceberg.hive-catalog-name` | String | `Optional.empty()` | Catalog to redirect to when a Hive table is referenced |  |
| `iceberg.idle-writer-min-file-size` | DataSize | `DataSize.of(16, MEGABYTE)` | Minimum data written by a single partition writer before it can be consider as 'idle' and could be closed by the engine |  |
| `iceberg.incremental-refresh-enabled` | boolean | `true` | Enable Incremental refresh for MVs backed by Iceberg tables, when possible |  |
| `iceberg.materialized-views.hide-storage-table` | boolean | `true` | Hide materialized view storage tables in metastore | deprecated |
| `iceberg.materialized-views.refresh-max-snapshots-to-expire` | int | `200` | Maximum number of snapshots to remove during materialized view refresh |  |
| `iceberg.materialized-views.refresh-snapshot-retention-period` | Duration | `new Duration(4, HOURS)` | Retention threshold to use when expiring snapshots during materialized view refresh |  |
| `iceberg.materialized-views.storage-schema` | String | `Optional.empty()` | Schema for creating materialized views storage tables | deprecated |
| `iceberg.max-commit-retry` | Integer | `Optional.empty()` | Number of times to retry a commit before failing |  |
| `iceberg.max-partitions-per-writer` | int | `100` | Maximum number of partitions per writer |  |
| `iceberg.max-previous-versions` | Integer | `Optional.empty()` | The number of old metadata files to keep |  |
| `iceberg.max-split-size` | DataSize | `Optional.empty()` | Target maximum split size for Iceberg tables |  |
| `iceberg.metadata-cache.enabled` | boolean | `true` | Enables in-memory caching of metadata files on coordinator if fs.cache.enabled is not set to true |  |
| `iceberg.metadata.parallelism` | int | `8` | Limits metadata enumeration calls parallelism |  |
| `iceberg.metadata.virtual-threads-enabled` | boolean | `true` | Run blocking metadata enumeration I/O on virtual threads |  |
| `iceberg.minimum-assigned-split-weight` | double | `0.05` | Minimum weight that a split can be assigned |  |
| `iceberg.object-store-layout.enabled` | boolean | `false` | Enable the Iceberg object store file layout |  |
| `iceberg.parquet-footer-cache.memory.max-size` | DataSize | `DataSize.of(10, MEGABYTE)` | Maximum size of the in-memory Parquet footer cache |  |
| `iceberg.parquet-footer-cache.type` | ParquetFooterCacheType | `NONE` | Type of cache to use for Parquet footer metadata |  |
| `iceberg.planning-threads` | String | `Runtime.getRuntime().availableProcessors() * 2` | Number of threads to use for metadata scans in planning |  |
| `iceberg.projection-pushdown-enabled` | boolean | `true` | Read only required fields from a row type |  |
| `iceberg.query-partition-filter-required` | boolean | `false` | Require a filter on at least one partition column |  |
| `iceberg.query-partition-filter-required-schemas` | Set<String> | `ImmutableSet.of()` | List of schemas for which filter on partition column is enforced |  |
| `iceberg.register-table-procedure.enabled` | boolean | `false` | Allow users to call the register_table procedure |  |
| `iceberg.remove-orphan-files.min-retention` | Duration | `new Duration(7, DAYS)` | Minimal retention period for remove_orphan_files procedure | Legacy names: `iceberg.remove_orphan_files.min-retention` |
| `iceberg.sorted-writing-enabled` | boolean | `true` | Enable sorted writing to tables with a specified sort order |  |
| `iceberg.sorted-writing.local-staging-path` | String | `Optional.empty()` | Use provided local directory for staging writes to sorted tables. Use ${USER} placeholder to use different location for each user |  |
| `iceberg.split-manager-threads` | String | `Math.min(Runtime.getRuntime().availableProcessors() * 2, 32)` | Number of threads to use for generating splits |  |
| `iceberg.table-statistics-enabled` | boolean | `true` | Enable use of table statistics |  |
| `iceberg.target-max-file-size` | DataSize | `DataSize.of(1, GIGABYTE)` | Target maximum size of written files; the actual size may be larger | Legacy names: `hive.target-max-file-size` |
| `iceberg.unique-table-location` | boolean | `true` | Use randomized, unique table locations |  |
| `iceberg.use-file-size-from-metadata` | boolean | `true` |  | deprecated |

Removed properties, rejected at startup if still set: `iceberg.allow-legacy-snapshot-syntax`, `iceberg.equality-deletes-blocks-hash-enabled`, `iceberg.experimental.extended-statistics.enabled`, `iceberg.extended-statistics.enabled`, `iceberg.file-based-conflict-detection`

## IcebergEncryptionConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/encryption/IcebergEncryptionConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.encryption.kms-type` | KmsType | `Optional.empty()` | Key Management Service type for Iceberg table encryption |  |
| `iceberg.encryption.plaintext-files-allowed-for-encrypted-tables` | boolean | `false` | Allow reading unencrypted files in tables with encryption enabled |  |

## IcebergGlueCatalogConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/glue/IcebergGlueCatalogConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.glue.cache-table-metadata` | boolean | `true` |  |  |

## IcebergHiveCatalogConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/hms/IcebergHiveCatalogConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.hive-catalog.locking-enabled` | boolean | `true` | Acquire locks when updating tables |  |

## IcebergJdbcCatalogConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/jdbc/IcebergJdbcCatalogConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.jdbc-catalog.catalog-name` | String |  | Iceberg JDBC metastore catalog name |  |
| `iceberg.jdbc-catalog.connection-password` | String |  | Password for JDBC client | sensitive |
| `iceberg.jdbc-catalog.connection-url` | String |  | The URI to connect to the JDBC server | sensitive |
| `iceberg.jdbc-catalog.connection-user` | String |  | User name for JDBC client |  |
| `iceberg.jdbc-catalog.default-warehouse-dir` | String |  | The default warehouse directory to use for JDBC |  |
| `iceberg.jdbc-catalog.driver-class` | String |  | JDBC driver class name |  |
| `iceberg.jdbc-catalog.retryable-status-codes` | String |  | On connection error to JDBC metastore, retry if it is one of these JDBC status codes |  |
| `iceberg.jdbc-catalog.schema-version` | SchemaVersion | `SchemaVersion.V1` | JDBC catalog schema version |  |

## IcebergNessieCatalogConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/nessie/IcebergNessieCatalogConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.nessie-catalog.authentication.token` | String | `Optional.empty()` | The token to use with BEARER authentication | sensitive |
| `iceberg.nessie-catalog.authentication.type` | Security |  | The authentication type to use |  |
| `iceberg.nessie-catalog.client-api-version` | ClientApiVersion | `Optional.empty()` | Client API version to use |  |
| `iceberg.nessie-catalog.connection-timeout` | Duration | `new Duration(DEFAULT_CONNECT_TIMEOUT_MILLIS, MILLISECONDS)` | The connection timeout for the client. |  |
| `iceberg.nessie-catalog.default-warehouse-dir` | String |  | The default warehouse to use for Nessie |  |
| `iceberg.nessie-catalog.enable-compression` | boolean | `true` | Configure whether compression should be enabled or not. |  |
| `iceberg.nessie-catalog.read-timeout` | Duration | `new Duration(DEFAULT_READ_TIMEOUT_MILLIS, MILLISECONDS)` | The read timeout for the client. |  |
| `iceberg.nessie-catalog.ref` | String | `"main"` | The default Nessie reference to work on |  |
| `iceberg.nessie-catalog.uri` | URI |  | The URI to connect to the Nessie server |  |

## IcebergRestCatalogConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/rest/IcebergRestCatalogConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.rest-catalog.case-insensitive-name-matching` | boolean | `false` | Match object names case-insensitively |  |
| `iceberg.rest-catalog.case-insensitive-name-matching.cache-max-size` | long | `10_000` | Maximum number of entries in the case insensitive object mapping cache |  |
| `iceberg.rest-catalog.case-insensitive-name-matching.cache-ttl` | Duration | `new Duration(1, MINUTES)` | Duration to keep case insensitive object mapping prior to eviction |  |
| `iceberg.rest-catalog.case-insensitive-name-matching.namespace-cache.enabled` | boolean | `true` | Cache the full list of tables/views per namespace to avoid repeated listings during case insensitive resolution |  |
| `iceberg.rest-catalog.case-insensitive-name-matching.namespace-cache.max-size` | long | `10_000` | Maximum total number of table or view identifiers retained across namespaces in the case insensitive listing cache |  |
| `iceberg.rest-catalog.connection-timeout` | Duration |  | Maximum time allowed for socket connect to complete before timing out |  |
| `iceberg.rest-catalog.http-headers` | List<String> | `ImmutableMap.of()` | Additional HTTP headers to attach to REST catalog requests |  |
| `iceberg.rest-catalog.max-retries` | int | `5` | Maximum number of retry attempts for failed REST catalog HTTP requests |  |
| `iceberg.rest-catalog.metrics-reporting-enabled` | boolean | `true` | Report table scan and commit metrics to the REST catalog server |  |
| `iceberg.rest-catalog.nested-namespace-enabled` | boolean | `false` | Support querying objects under nested namespace |  |
| `iceberg.rest-catalog.prefix` | String | `Optional.empty()` | The prefix for the resource path to use with the REST catalog server |  |
| `iceberg.rest-catalog.security` | Security | `Security.NONE` | Authorization protocol to use when communicating with the REST catalog server |  |
| `iceberg.rest-catalog.server-assigned-table-location-enabled` | boolean | `false` | Let the REST catalog server assign locations for created tables instead of computing a default location from the namespace location |  |
| `iceberg.rest-catalog.session` | SessionType | `SessionType.NONE` | Type of REST catalog sessionType to use when communicating with REST catalog Server |  |
| `iceberg.rest-catalog.session-timeout` | Duration | `new Duration(CatalogProperties.AUTH_SESSION_TIMEOUT_MS_DEFAULT, MILLISECONDS)` | Duration to keep authentication session in cache |  |
| `iceberg.rest-catalog.snapshot-loading-mode` | SnapshotLoadingMode | `SnapshotLoadingMode.valueOf(SNAPSHOT_LOADING_MODE_DEFAULT.name())` | Load all table snapshots eagerly (ALL), or only branch/tag-referenced snapshots with lazy loading of the remaining ones (REFS) |  |
| `iceberg.rest-catalog.socket-timeout` | Duration |  | Maximum time allowed for socket reads/writes before timing out |  |
| `iceberg.rest-catalog.uri` | String |  | The URI to the REST server |  |
| `iceberg.rest-catalog.vended-credentials-enabled` | boolean | `false` | Use credentials provided by the REST backend for file system access |  |
| `iceberg.rest-catalog.view-endpoints-enabled` | boolean | `true` | Enable view endpoints |  |
| `iceberg.rest-catalog.warehouse` | String | `Optional.empty()` | The warehouse location/identifier to use with the REST catalog server |  |

Removed properties, rejected at startup if still set: `iceberg.rest-catalog.parent-namespace`, `iceberg.rest-catalog.sigv4-enabled`

## IcebergRestCatalogSigV4Config

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/rest/IcebergRestCatalogSigV4Config.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.rest-catalog.signing-name` | String | `AwsProperties.REST_SIGNING_NAME_DEFAULT` | AWS SigV4 signing service name |  |

## IcebergSecurityConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/IcebergSecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.security` | IcebergSecurity | `IcebergSecurity.ALLOW_ALL` |  |  |

## IcebergSnowflakeCatalogConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/snowflake/IcebergSnowflakeCatalogConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.snowflake-catalog.account-uri` | URI |  | Snowflake JDBC URI |  |
| `iceberg.snowflake-catalog.database` | String |  | Snowflake database |  |
| `iceberg.snowflake-catalog.password` | String |  | Password for Snowflake | sensitive |
| `iceberg.snowflake-catalog.role` | String | `Optional.empty()` | Name of Snowflake role to use |  |
| `iceberg.snowflake-catalog.user` | String |  | Username for Snowflake |  |

## OAuth2SecurityConfig

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/catalog/rest/OAuth2SecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `iceberg.rest-catalog.oauth2.credential` | String |  | The credential to exchange for a token in the OAuth2 client credentials flow with the server | sensitive |
| `iceberg.rest-catalog.oauth2.scope` | String |  | The scope which will be used for interactions with the server |  |
| `iceberg.rest-catalog.oauth2.server-uri` | URI |  | The endpoint to retrieve access token from OAuth2 Server |  |
| `iceberg.rest-catalog.oauth2.token` | String |  | The Bearer token which will be used for interactions with the server | sensitive |
| `iceberg.rest-catalog.oauth2.token-exchange-enabled` | boolean | `OAuth2Properties.TOKEN_EXCHANGE_ENABLED_DEFAULT` | Controls whether to use the token exchange flow to acquire new tokens |  |
| `iceberg.rest-catalog.oauth2.token-refresh-enabled` | boolean | `OAuth2Properties.TOKEN_REFRESH_ENABLED_DEFAULT` | Controls whether a token should be refreshed if information about its expiration time is available |  |
