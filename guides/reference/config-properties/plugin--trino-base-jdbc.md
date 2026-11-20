# Configuration properties: `plugin/trino-base-jdbc`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [BaseJdbcConfig](#basejdbcconfig), [ConfigFileBasedCredentialProviderConfig](#configfilebasedcredentialproviderconfig), [CredentialConfig](#credentialconfig), [CredentialProviderTypeConfig](#credentialprovidertypeconfig), [DeprecatedDecimalConfig](#deprecateddecimalconfig), [ExtraCredentialConfig](#extracredentialconfig), [FormatBasedRemoteQueryModifierConfig](#formatbasedremotequerymodifierconfig), [JdbcDynamicFilteringConfig](#jdbcdynamicfilteringconfig), [JdbcJoinPushdownConfig](#jdbcjoinpushdownconfig), [JdbcMetadataConfig](#jdbcmetadataconfig), [JdbcStatisticsConfig](#jdbcstatisticsconfig), [JdbcWriteConfig](#jdbcwriteconfig), [KeyStoreBasedCredentialProviderConfig](#keystorebasedcredentialproviderconfig), [LegacyDecimalConfig](#legacydecimalconfig), [QueryConfig](#queryconfig), [TypeHandlingJdbcConfig](#typehandlingjdbcconfig)

## BaseJdbcConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/BaseJdbcConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `connection-url` | String |  |  |  |
| `jdbc-types-mapped-to-varchar` | Set<String> | `ImmutableSet.of()` |  |  |
| `metadata.cache-maximum-size` | long | `Optional.empty()` | Maximum number of objects stored in the metadata cache |  |
| `metadata.cache-missing` | boolean | `false` | Determines if missing information will be cached |  |
| `metadata.cache-ttl` | Duration | `new Duration(0, SECONDS)` | Determines how long meta information will be cached |  |
| `metadata.schemas.cache-ttl` | Duration | `Optional.empty()` | Determines how long schema names list information will be cached |  |
| `metadata.statistics.cache-ttl` | Duration | `Optional.empty()` | Determines how long table statistics information will be cached |  |
| `metadata.tables.cache-ttl` | Duration | `Optional.empty()` | Determines how long table names list information will be cached |  |

Removed properties, rejected at startup if still set: `remote-query-async-cancellation.enabled`

## ConfigFileBasedCredentialProviderConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/credential/file/ConfigFileBasedCredentialProviderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `connection-credential-file` | String |  | Location of the file where credentials are present |  |

## CredentialConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/credential/CredentialConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `connection-password` | String |  | Password for JDBC client | sensitive |
| `connection-user` | String |  | user name for JDBC client |  |

## CredentialProviderTypeConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/credential/CredentialProviderTypeConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `credential-provider.type` | CredentialProviderType | `INLINE` |  |  |

## DeprecatedDecimalConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/DeprecatedDecimalConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `decimal-default-scale` | Integer |  | Default decimal scale for mapping unspecified and exceeding precision decimals. Not used when is set to STRICT or MAP_TO_NUMBER | deprecated |
| `decimal-mapping` | DecimalMapping | `DecimalMapping.MAP_TO_NUMBER` | Decimal mapping for unspecified and exceeding precision decimals. STRICT skips them. MAP_TO_NUMBER maps them to Trino NUMBER. ALLOW_OVERFLOW requires setting proper decimal scale and rounding mode | deprecated |
| `decimal-rounding-mode` | RoundingMode | `UNNECESSARY` | Rounding mode for mapping unspecified and exceeding precision decimals. Not used when is set to STRICT or MAP_TO_NUMBER | deprecated |

## ExtraCredentialConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/credential/ExtraCredentialConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `password-credential-name` | String |  |  |  |
| `user-credential-name` | String |  |  |  |

## FormatBasedRemoteQueryModifierConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/logging/FormatBasedRemoteQueryModifierConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `query.comment-format` | String | `""` | Format in which logs about query execution context should be added as comments sent through jdbc driver. |  |

## JdbcDynamicFilteringConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcDynamicFilteringConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `dynamic-filtering.enabled` | boolean | `true` | Wait for dynamic filters before starting JDBC query |  |
| `dynamic-filtering.wait-timeout` | Duration | `new Duration(20, SECONDS)` | Duration to wait for completion of dynamic filters |  |

## JdbcJoinPushdownConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcJoinPushdownConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `experimental.join-pushdown.automatic.max-join-to-tables-ratio` | double | `1.25` | If estimated join output size is greater than or equal to ratio * sum of table sizes, then join pushdown will not be performed |  |
| `experimental.join-pushdown.automatic.max-table-size` | DataSize | `Optional.empty()` | Maximum table size to be considered for join pushdown |  |
| `join-pushdown.strategy` | JoinPushdownStrategy | `JoinPushdownStrategy.AUTOMATIC` |  |  |

## JdbcMetadataConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcMetadataConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `aggregation-pushdown.enabled` | boolean | `true` | Enable aggregation pushdown | Legacy names: `allow-aggregation-pushdown` |
| `complex-expression-pushdown.enabled` | boolean | `true` |  |  |
| `deprecated.join-pushdown.with-expressions` | boolean | `true` | Enable join pushdown with complex expressions | deprecated |
| `domain-compaction-threshold` | int | `256` | Maximum ranges to allow in a tuple domain without compacting it |  |
| `jdbc.bulk-list-columns.enabled` | boolean | `false` |  | hidden |
| `join-pushdown.enabled` | boolean | `false` | Enable join pushdown | Legacy names: `experimental.join-pushdown.enabled` |
| `topn-pushdown.enabled` | boolean | `true` | Enable TopN pushdown |  |

Removed properties, rejected at startup if still set: `allow-drop-table`

## JdbcStatisticsConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcStatisticsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `statistics.enabled` | boolean | `true` |  |  |

## JdbcWriteConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcWriteConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `insert.non-transactional-insert.enabled` | boolean | `false` | Enables support for non-transactional INSERT. This means that the write operation can fail and leave the table in an inconsistent state. |  |
| `merge.non-transactional-merge.enabled` | boolean | `false` | Enables support for non-transactional MERGE. This means that the write operation can fail and leave the table in an inconsistent state. |  |
| `write.batch-size` | int | `1000` | Maximum number of rows to write in a single batch |  |
| `write.parallelism` | int | `DEFAULT_WRITE_PARALLELISM` | Maximum number of parallel write tasks |  |

## KeyStoreBasedCredentialProviderConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/credential/keystore/KeyStoreBasedCredentialProviderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `keystore-file-path` | String |  |  |  |
| `keystore-password` | String |  |  | sensitive |
| `keystore-password-credential-name` | String |  |  |  |
| `keystore-password-credential-password` | String |  |  |  |
| `keystore-type` | String |  |  |  |
| `keystore-user-credential-name` | String |  |  |  |
| `keystore-user-credential-password` | String |  |  |  |

## LegacyDecimalConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/LegacyDecimalConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `decimal-default-scale` | Integer |  | Default decimal scale for mapping unspecified and exceeding precision decimals. Not used when is set to STRICT or MAP_TO_NUMBER |  |
| `decimal-mapping` | DecimalMapping | `DecimalMapping.STRICT` | Decimal mapping for unspecified and exceeding precision decimals. STRICT skips them. MAP_TO_NUMBER maps them to Trino NUMBER. ALLOW_OVERFLOW requires setting proper decimal scale and rounding mode |  |
| `decimal-rounding-mode` | RoundingMode | `UNNECESSARY` | Rounding mode for mapping unspecified and exceeding precision decimals. Not used when is set to STRICT or MAP_TO_NUMBER |  |

## QueryConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/QueryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `query.reuse-connection` | boolean | `true` | Enables reusing JDBC connection for metadata queries to data source within a single Trino query |  |

## TypeHandlingJdbcConfig

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/TypeHandlingJdbcConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `unsupported-type-handling` | UnsupportedTypeHandling | `UnsupportedTypeHandling.IGNORE` | Unsupported type handling strategy | Legacy names: `unsupported-type.handling-strategy` |
