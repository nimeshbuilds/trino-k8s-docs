# Configuration properties: `lib/trino-filesystem-azure`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [AzureAuthAccessKeyConfig](#azureauthaccesskeyconfig), [AzureAuthManagedIdentityConfig](#azureauthmanagedidentityconfig), [AzureAuthOAuthConfig](#azureauthoauthconfig), [AzureFileSystemConfig](#azurefilesystemconfig)

## AzureAuthAccessKeyConfig

Source: `lib/trino-filesystem-azure/src/main/java/io/trino/filesystem/azure/AzureAuthAccessKeyConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `azure.access-key` | String |  |  | sensitive |

## AzureAuthManagedIdentityConfig

Source: `lib/trino-filesystem-azure/src/main/java/io/trino/filesystem/azure/AzureAuthManagedIdentityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `azure.user-assigned-managed-identity.client-id` | String | `Optional.empty()` |  | sensitive |
| `azure.user-assigned-managed-identity.resource-id` | String | `Optional.empty()` |  | sensitive |

## AzureAuthOAuthConfig

Source: `lib/trino-filesystem-azure/src/main/java/io/trino/filesystem/azure/AzureAuthOAuthConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `azure.oauth.client-id` | String |  |  | sensitive |
| `azure.oauth.endpoint` | String |  |  | sensitive |
| `azure.oauth.secret` | String |  |  | sensitive |
| `azure.oauth.tenant-id` | String |  |  |  |

## AzureFileSystemConfig

Source: `lib/trino-filesystem-azure/src/main/java/io/trino/filesystem/azure/AzureFileSystemConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `azure.application-id` | String | `"Trino"` | Suffix that will be added to HTTP User-Agent header to identify the application |  |
| `azure.auth-type` | AuthType | `AuthType.DEFAULT` |  |  |
| `azure.connection-pool-max-idle-time` | Duration | `new Duration(5, TimeUnit.MINUTES)` | Maximum idle time for pooled HTTP connections |  |
| `azure.endpoint` | String | `"core.windows.net"` |  |  |
| `azure.http-request-timeout` | Duration | `new Duration(10, TimeUnit.MINUTES)` | Maximum time for an HTTP request to complete |  |
| `azure.max-http-connections` | int | `2 * max(Runtime.getRuntime().availableProcessors(), 8)` | Maximum number of pooled HTTP connections |  |
| `azure.max-http-requests` | int | `2 * Runtime.getRuntime().availableProcessors()` | Maximum number of concurrent HTTP requests to Azure on every node |  |
| `azure.max-single-upload-size` | DataSize | `DataSize.of(4, Unit.MEGABYTE)` |  |  |
| `azure.max-write-concurrency` | int | `8` |  |  |
| `azure.multipart-write-enabled` | boolean | `false` | Enable multipart writes for large files |  |
| `azure.read-block-size` | DataSize | `DataSize.of(4, Unit.MEGABYTE)` |  |  |
| `azure.write-block-size` | DataSize | `DataSize.of(4, Unit.MEGABYTE)` |  |  |
