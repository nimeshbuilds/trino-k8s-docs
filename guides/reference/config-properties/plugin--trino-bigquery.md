# Configuration properties: `plugin/trino-bigquery`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [BigQueryArrowConfig](#bigqueryarrowconfig), [BigQueryConfig](#bigqueryconfig), [BigQueryProxyConfig](#bigqueryproxyconfig), [BigQueryRpcConfig](#bigqueryrpcconfig), [StaticCredentialsConfig](#staticcredentialsconfig)

## BigQueryArrowConfig

Source: `plugin/trino-bigquery/src/main/java/io/trino/plugin/bigquery/BigQueryArrowConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `bigquery.arrow-serialization.max-allocation` | DataSize | `DataSize.of(100, MEGABYTE)` | Maximum memory allocation allowed for Arrow library |  |

## BigQueryConfig

Source: `plugin/trino-bigquery/src/main/java/io/trino/plugin/bigquery/BigQueryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `bigquery.arrow-serialization.enabled` | boolean | `true` | Enables Arrow serialization while reading data | Legacy names: `bigquery.experimental.arrow-serialization.enabled` |
| `bigquery.case-insensitive-name-matching` | boolean | `false` | Match dataset and table names case-insensitively |  |
| `bigquery.case-insensitive-name-matching.cache-ttl` | Duration | `new Duration(0, MILLISECONDS)` | Duration for which case insensitive schema and table names are cached. Set to 0ms to disable the cache. |  |
| `bigquery.job.label-format` | String |  | Adds `bigquery.job.label-name` label to the BigQuery job with provided value format |  |
| `bigquery.job.label-name` | String |  | Adds label with the given name to the BigQuery job |  |
| `bigquery.legacy-metadata-listing` | boolean |  | Call BigQuery REST API per table when listing metadata | hidden |
| `bigquery.max-parallelism` | Integer | `Optional.empty()` | The max number of partitions to split the data into |  |
| `bigquery.max-read-rows-retries` | int | `DEFAULT_MAX_READ_ROWS_RETRIES` | The number of retries in case of retryable server issues |  |
| `bigquery.metadata-page-size` | int | `1000` | The number of metadata entries retrieved per API request |  |
| `bigquery.metadata.cache-ttl` | Duration | `new Duration(0, MILLISECONDS)` | Duration for which BigQuery client metadata is cached after listing |  |
| `bigquery.metadata.parallelism` | int | `Math.min(Runtime.getRuntime().availableProcessors(), MAX_METADATA_PARALLELISM)` | Limits metadata enumeration calls parallelism |  |
| `bigquery.parent-project-id` | String | `Optional.empty()` | The Google Cloud Project ID to bill for the export |  |
| `bigquery.project-id` | String | `Optional.empty()` | The Google Cloud Project ID where the data reside |  |
| `bigquery.projection-pushdown-enabled` | boolean | `true` | Dereference push down for ROW type |  |
| `bigquery.query-results-cache.enabled` | boolean | `false` |  |  |
| `bigquery.rpc-proxy.enabled` | boolean | `false` | Enables proxying of RPC and gRPC requests to BigQuery APIs |  |
| `bigquery.service-cache-ttl` | Duration | `new Duration(3, MINUTES)` | Duration for which BigQuery client service instances are cached | hidden |
| `bigquery.skip-view-materialization` | boolean | `false` | Skip materializing views |  |
| `bigquery.view-expire-duration` | Duration | `new Duration(24, HOURS)` |  |  |
| `bigquery.view-materialization-dataset` | String | `Optional.empty()` | The dataset where the materialized view is going to be created |  |
| `bigquery.view-materialization-project` | String | `Optional.empty()` | The project where the materialized view is going to be created |  |
| `bigquery.view-materialization-with-filter` | boolean | `false` | Use filter when materializing views |  |
| `bigquery.views-cache-ttl` | Duration | `new Duration(15, MINUTES)` | Duration for which the materialization of a view will be cached and reused |  |
| `bigquery.views-enabled` | boolean | `false` | Enables the connector to read from views and not only tables |  |
| `bigquery.write-retry-initial-delay` | Duration | `new Duration(500, MILLISECONDS)` | The initial backoff delay before retrying a failed Storage Write API append request |  |
| `bigquery.write-retry-max-attempts` | int | `5` | The maximum number of attempts for a Storage Write API append request, including the initial attempt |  |
| `bigquery.write-retry-max-delay` | Duration | `new Duration(30, SECONDS)` | The maximum backoff delay between retries of a failed Storage Write API append request |  |

Removed properties, rejected at startup if still set: `bigquery.parallelism`

## BigQueryProxyConfig

Source: `plugin/trino-bigquery/src/main/java/io/trino/plugin/bigquery/BigQueryProxyConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `bigquery.rpc-proxy.keystore-password` | String |  | Password to a Java keystore file | sensitive |
| `bigquery.rpc-proxy.keystore-path` | File |  | Path to a Java keystore file |  |
| `bigquery.rpc-proxy.password` | String | `Optional.empty()` | Password used to authenticate against proxy | sensitive |
| `bigquery.rpc-proxy.truststore-password` | String |  | Password to a Java truststore file | sensitive |
| `bigquery.rpc-proxy.truststore-path` | File |  | Path to a Java truststore file |  |
| `bigquery.rpc-proxy.uri` | URI |  | Proxy URI (host and port only) |  |
| `bigquery.rpc-proxy.username` | String | `Optional.empty()` | Username used to authenticate against proxy |  |

## BigQueryRpcConfig

Source: `plugin/trino-bigquery/src/main/java/io/trino/plugin/bigquery/BigQueryRpcConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `bigquery.channel-pool.initial-size` | int | `1` |  |  |
| `bigquery.channel-pool.max-rpc-per-channel` | int | `Integer.MAX_VALUE` |  |  |
| `bigquery.channel-pool.max-size` | int | `1` |  |  |
| `bigquery.channel-pool.min-rpc-per-channel` | int | `0` |  |  |
| `bigquery.channel-pool.min-size` | int | `1` |  |  |
| `bigquery.rpc-retries` | int | `0` |  |  |
| `bigquery.rpc-retry-delay` | Duration | `Duration.valueOf("0s")` |  |  |
| `bigquery.rpc-retry-delay-multiplier` | double | `1.0` |  |  |
| `bigquery.rpc-timeout` | Duration | `Duration.valueOf("0s")` |  |  |

## StaticCredentialsConfig

Source: `plugin/trino-bigquery/src/main/java/io/trino/plugin/bigquery/StaticCredentialsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `bigquery.credentials-file` | String | `Optional.empty()` | The path to the JSON credentials file |  |
| `bigquery.credentials-key` | String | `Optional.empty()` | The base64 encoded credentials key | sensitive |
