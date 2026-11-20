# Configuration properties: `plugin/trino-spooling-filesystem`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [FileSystemSpoolingConfig](#filesystemspoolingconfig), [PartitionedLayoutConfig](#partitionedlayoutconfig)

## FileSystemSpoolingConfig

Source: `plugin/trino-spooling-filesystem/src/main/java/io/trino/spooling/filesystem/FileSystemSpoolingConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `fs.azure.enabled` | boolean | `false` |  |  |
| `fs.gcs.enabled` | boolean | `false` |  |  |
| `fs.layout` | Layout | `SIMPLE` | File system layout for spooled segments storage |  |
| `fs.location` | String |  |  |  |
| `fs.s3.enabled` | boolean | `false` |  |  |
| `fs.segment.direct.ttl` | Duration | `new Duration(1, HOURS)` | Maximum duration for the client to retrieve spooled segment from the direct URI |  |
| `fs.segment.encryption` | boolean | `true` | Encrypt segments with ephemeral keys |  |
| `fs.segment.explicit-ack` | boolean | `true` | Enables deletion of segments on client acknowledgment |  |
| `fs.segment.pruning.batch-size` | long | `250` | Prune expired segments in batches of provided size |  |
| `fs.segment.pruning.enabled` | boolean | `true` | Prune expired segments periodically |  |
| `fs.segment.pruning.interval` | Duration | `new Duration(5, MINUTES)` | Interval to prune expired segments |  |
| `fs.segment.ttl` | Duration | `new Duration(12, HOURS)` | Maximum duration for the client to retrieve spooled segment before it expires |  |

## PartitionedLayoutConfig

Source: `plugin/trino-spooling-filesystem/src/main/java/io/trino/spooling/filesystem/PartitionedLayoutConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `fs.layout.partitions` | int | `32` | Number of file system partitions to use |  |
