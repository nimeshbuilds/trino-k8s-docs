# Configuration properties: `lib/trino-filesystem-s3`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [S3FileSystemConfig](#s3filesystemconfig), [S3SecurityMappingConfig](#s3securitymappingconfig), [S3SecurityMappingEnabledConfig](#s3securitymappingenabledconfig)

## S3FileSystemConfig

Source: `lib/trino-filesystem-s3/src/main/java/io/trino/filesystem/s3/S3FileSystemConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `s3.application-id` | String | `"Trino"` | Suffix that will be added to HTTP User-Agent header to identify the application |  |
| `s3.auth-type` | S3AuthType | `S3AuthType.DEFAULT` | Authentication mode for accessing S3 |  |
| `s3.aws-access-key` | String |  |  |  |
| `s3.aws-secret-key` | String |  |  | sensitive |
| `s3.canned-acl` | ObjectCannedAcl | `ObjectCannedAcl.NONE` | Canned ACL (predefined grants) to manage access to objects |  |
| `s3.checksum-algorithm` | S3ChecksumAlgorithm | `S3ChecksumAlgorithm.DEFAULT` | Checksum algorithm to use for S3 upload and delete requests; DEFAULT keeps the AWS SDK default behavior |  |
| `s3.chunked-encoding-enabled` | boolean | `true` | Enable chunked encoding for S3 upload requests; disable for S3-compatible storage systems that do not support it |  |
| `s3.connection-max-idle-time` | Duration |  | Maximum time allowed for connections to remain idle in the connection pool before being closed |  |
| `s3.connection-ttl` | Duration |  | Maximum time allowed for connections to be reused before being replaced in the connection pool |  |
| `s3.cross-region-access` | boolean | `false` | Enable S3 cross region access |  |
| `s3.endpoint` | String |  |  |  |
| `s3.expect-continue-enabled` | boolean | `true` | Enable HTTP expect-continue handshake for S3 PUT requests; disable for S3-compatible gateways that reject it |  |
| `s3.external-id` | String |  | External ID for the IAM role trust policy when connecting to S3 |  |
| `s3.http-proxy` | HostAndPort |  |  |  |
| `s3.http-proxy.non-proxy-hosts` | String | `ImmutableSet.of()` |  |  |
| `s3.http-proxy.password` | String |  |  | sensitive |
| `s3.http-proxy.preemptive-basic-auth` | boolean | `false` |  |  |
| `s3.http-proxy.secure` | boolean | `false` |  |  |
| `s3.http-proxy.username` | String |  |  |  |
| `s3.iam-role` | String |  | ARN of an IAM role to assume when connecting to S3 |  |
| `s3.max-connections` | Integer | `500` |  |  |
| `s3.max-error-retries` | int | `20` |  |  |
| `s3.path-style-access` | boolean | `false` | Use path-style access for all requests to S3 |  |
| `s3.region` | String |  |  |  |
| `s3.requester-pays` | boolean | `false` |  |  |
| `s3.retry-mode` | RetryMode | `RetryMode.LEGACY` | Specifies how the AWS SDK attempts retries, default is LEGACY |  |
| `s3.role-session-name` | String | `"trino-filesystem"` | Role session name to use when connecting to S3 |  |
| `s3.signer-type` | SignerType |  | AWS signing protocol to use while authenticating S3 requests |  |
| `s3.socket-connect-timeout` | Duration |  | Maximum time allowed for socket connect to complete before timing out |  |
| `s3.socket-timeout` | Duration |  | Maximum time allowed for socket reads/writes before timing out | Legacy names: `s3.socket-read-timeout` |
| `s3.sse.customer-key` | String |  | Customer Key to use for S3 server-side encryption with Customer key (SSE-C) | sensitive |
| `s3.sse.kms-key-id` | String |  | KMS Key ID to use for S3 server-side encryption with KMS-managed key |  |
| `s3.sse.type` | S3SseType | `S3SseType.NONE` |  |  |
| `s3.storage-class` | StorageClassType | `StorageClassType.STANDARD` | The S3 storage class to use when writing the data |  |
| `s3.streaming.part-size` | DataSize | `DataSize.of(32, MEGABYTE)` | Part size for S3 streaming upload |  |
| `s3.sts.endpoint` | String |  |  |  |
| `s3.sts.region` | String |  |  |  |
| `s3.tcp-keep-alive` | boolean | `false` | Enable TCP keep alive on created connections |  |

Removed properties, rejected at startup if still set: `s3.exclusive-create`, `s3.use-web-identity-token-credentials-provider`

## S3SecurityMappingConfig

Source: `lib/trino-filesystem-s3/src/main/java/io/trino/filesystem/s3/S3SecurityMappingConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `s3.security-mapping.colon-replacement` | String |  | Value used in place of colon for IAM role name in extra credentials |  |
| `s3.security-mapping.config-file` | File |  | Path to the JSON security mappings file |  |
| `s3.security-mapping.config-uri` | URI |  | HTTP URI of the JSON security mappings |  |
| `s3.security-mapping.iam-role-credential-name` | String |  | Name of the extra credential used to provide IAM role |  |
| `s3.security-mapping.json-pointer` | String | `""` | JSON pointer (RFC 6901) to mappings inside JSON config |  |
| `s3.security-mapping.kms-key-id-credential-name` | String |  | Name of the extra credential used to provide KMS Key ID |  |
| `s3.security-mapping.refresh-period` | Duration |  | How often to refresh the security mapping configuration |  |
| `s3.security-mapping.sse-customer-key-credential-name` | String |  | Name of the extra credential used to provide SSE Customer key |  |

## S3SecurityMappingEnabledConfig

Source: `lib/trino-filesystem-s3/src/main/java/io/trino/filesystem/s3/S3SecurityMappingEnabledConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `s3.security-mapping.enabled` | boolean | `false` |  |  |
