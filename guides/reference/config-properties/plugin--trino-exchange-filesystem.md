# Configuration properties: `plugin/trino-exchange-filesystem`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [ExchangeAzureConfig](#exchangeazureconfig), [ExchangeS3Config](#exchanges3config), [FileSystemExchangeConfig](#filesystemexchangeconfig)

## ExchangeAzureConfig

Source: `plugin/trino-exchange-filesystem/src/main/java/io/trino/plugin/exchange/filesystem/azure/ExchangeAzureConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `exchange.azure.block-size` | DataSize | `DataSize.of(4, MEGABYTE)` | Block size for Azure High-Throughput Block Blob |  |
| `exchange.azure.connection-acquisition-timeout` | Duration | `new Duration(1, TimeUnit.MINUTES)` | Maximum time to wait for a connection to be acquired from the Azure storage connection pool |  |
| `exchange.azure.connection-string` | String | `Optional.empty()` |  | sensitive |
| `exchange.azure.endpoint` | String | `Optional.empty()` |  | sensitive |
| `exchange.azure.max-connections` | int | `500` | Maximum number of concurrent HTTP connections in the Azure storage connection pool |  |
| `exchange.azure.max-error-retries` | int | `10` |  |  |
| `exchange.azure.pending-acquire-max-count` | int | `1000` | Maximum number of pending connection acquire requests in the Azure storage connection pool. Should be set to at least exchange.azure.max-connections |  |

## ExchangeS3Config

Source: `plugin/trino-exchange-filesystem/src/main/java/io/trino/plugin/exchange/filesystem/s3/ExchangeS3Config.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `exchange.gcs.json-key` | String | `Optional.empty()` | Your Google Cloud Platform service account key in JSON format. Not to be set together with `exchange.gcs.json-key-file-path` | sensitive |
| `exchange.gcs.json-key-file-path` | String | `Optional.empty()` | Path to the JSON file that contains your Google Cloud Platform service account key. Not to be set together with `exchange.gcs.json-key` |  |
| `exchange.s3.async-client-concurrency` | int | `100` |  |  |
| `exchange.s3.async-client-connection-acquisition-timeout` | Duration | `new Duration(1, MINUTES)` |  |  |
| `exchange.s3.async-client-max-pending-connection-acquires` | int | `10000` |  |  |
| `exchange.s3.aws-access-key` | String |  |  |  |
| `exchange.s3.aws-secret-key` | String |  |  | sensitive |
| `exchange.s3.endpoint` | String | `Optional.empty()` |  |  |
| `exchange.s3.external-id` | String | `Optional.empty()` | External ID for the IAM role trust policy when connecting to S3 |  |
| `exchange.s3.iam-role` | String | `Optional.empty()` | ARN of an IAM role to assume when connecting to S3 |  |
| `exchange.s3.max-error-retries` | int | `10` |  |  |
| `exchange.s3.path-style-access` | boolean | `false` | Use path-style access for all request to S3 |  |
| `exchange.s3.region` | String | `Optional.empty()` |  |  |
| `exchange.s3.retry-mode` | RetryMode | `RetryMode.ADAPTIVE_V2` |  |  |
| `exchange.s3.sse.kms-key-id` | String | `Optional.empty()` | KMS Key ID to use for S3 server-side encryption with KMS-managed key for FTE |  |
| `exchange.s3.sse.type` | S3SseType | `S3SseType.NONE` | Type of S3 server-side encryption for FTE |  |
| `exchange.s3.storage-class` | StorageClass | `StorageClass.STANDARD` |  |  |
| `exchange.s3.upload.part-size` | DataSize | `DataSize.of(5, MEGABYTE)` | Part size for S3 multi-part upload |  |

## FileSystemExchangeConfig

Source: `plugin/trino-exchange-filesystem/src/main/java/io/trino/plugin/exchange/filesystem/FileSystemExchangeConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `exchange.base-directories` | String | `ImmutableList.of()` | List of base directories separated by commas | Legacy names: `exchange.base-directory` |
| `exchange.file-listing-parallelism` | int | `50` | Max parallelism of file listing calls when enumerating spooling files. The actual parallelism will depend on implementation |  |
| `exchange.max-output-partition-count` | int | `50` |  |  |
| `exchange.max-page-storage-size` | DataSize | `DataSize.of(16, MEGABYTE)` | Max storage size of a page written to a sink, including the page itself and its size represented as an int |  |
| `exchange.sink-buffer-pool-min-size` | int | `10` |  |  |
| `exchange.sink-buffers-per-partition` | int | `2` |  |  |
| `exchange.sink-max-file-size` | DataSize | `DataSize.of(1, GIGABYTE)` | Max size of files written by exchange sinks |  |
| `exchange.source-concurrent-readers` | int | `4` |  |  |
| `exchange.source-handle-target-data-size` | DataSize | `DataSize.of(256, MEGABYTE)` | Target size of the data referenced by a single source handle |  |
| `exchange.source-max-files-per-reader` | int | `25` |  |  |

Removed properties, rejected at startup if still set: `exchange.encryption-enabled`
