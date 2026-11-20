# Configuration properties: `lib/trino-filesystem-gcs`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [GcsFileSystemConfig](#gcsfilesystemconfig), [GcsServiceAccountAuthConfig](#gcsserviceaccountauthconfig)

## GcsFileSystemConfig

Source: `lib/trino-filesystem-gcs/src/main/java/io/trino/filesystem/gcs/GcsFileSystemConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `gcs.application-id` | String | `"Trino"` | Suffix that will be added to HTTP User-Agent header to identify the application |  |
| `gcs.auth-type` | AuthType | `AuthType.SERVICE_ACCOUNT` |  |  |
| `gcs.batch-size` | int | `100` | Number of blobs to delete per batch. Recommended batch size is 100: https://cloud.google.com/storage/docs/batch |  |
| `gcs.client.backoff-scale-factor` | double | `3.0` | Scale factor for RPC retry delay |  |
| `gcs.client.max-backoff-delay` | Duration | `new Duration(2000, TimeUnit.MILLISECONDS)` | Maximum delay between RPC retries. |  |
| `gcs.client.max-retries` | int | `20` | Maximum number of RPC attempts |  |
| `gcs.client.max-retry-time` | Duration | `new Duration(25, TimeUnit.SECONDS)` | Total time limit for an RPC to be retried |  |
| `gcs.client.min-backoff-delay` | Duration | `new Duration(10, TimeUnit.MILLISECONDS)` | Minimum delay between RPC retries |  |
| `gcs.endpoint` | Optional<String> | `Optional.empty()` | Endpoint to use for GCS requests |  |
| `gcs.page-size` | int | `100` | The maximum number of blobs to return per page. |  |
| `gcs.project-id` | String |  |  |  |
| `gcs.read-block-size` | DataSize | `DataSize.of(2, MEGABYTE)` | Minimum size that will be read in one RPC. The default size is 2MiB, see com.google.cloud.BaseStorageReadChannel. |  |
| `gcs.write-block-size` | DataSize | `DataSize.of(16, MEGABYTE)` | Minimum size that will be written in one RPC. The default size is 16MiB, see com.google.cloud.BaseStorageWriteChannel. |  |

Removed properties, rejected at startup if still set: `gcs.use-access-token`

## GcsServiceAccountAuthConfig

Source: `lib/trino-filesystem-gcs/src/main/java/io/trino/filesystem/gcs/GcsServiceAccountAuthConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `gcs.json-key` | String |  |  | sensitive |
| `gcs.json-key-file-path` | String |  | JSON key file used to access Google Cloud Storage |  |
