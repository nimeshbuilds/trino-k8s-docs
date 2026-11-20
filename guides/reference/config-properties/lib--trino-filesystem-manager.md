# Configuration properties: `lib/trino-filesystem-manager`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [FileSystemConfig](#filesystemconfig)

## FileSystemConfig

Source: `lib/trino-filesystem-manager/src/main/java/io/trino/filesystem/manager/FileSystemConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `fs.azure.enabled` | boolean | `false` |  | Legacy names: `fs.native-azure.enabled` |
| `fs.cache.enabled` | boolean | `false` |  |  |
| `fs.gcs.enabled` | boolean | `false` |  | Legacy names: `fs.native-gcs.enabled` |
| `fs.hadoop.enabled` | boolean | `false` |  |  |
| `fs.local.enabled` | boolean | `false` |  | Legacy names: `fs.native-local.enabled` |
| `fs.s3.enabled` | boolean | `false` |  | Legacy names: `fs.native-s3.enabled` |
| `fs.tracking.enabled` | boolean | `getenv("CONTINUOUS_INTEGRATION") != null` | Enable input/output stream tracking to detect resource leaks |  |

Removed properties, rejected at startup if still set: `fs.alluxio.enabled`
