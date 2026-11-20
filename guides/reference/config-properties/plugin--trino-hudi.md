# Configuration properties: `plugin/trino-hudi`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [HudiConfig](#hudiconfig)

## HudiConfig

Source: `plugin/trino-hudi/src/main/java/io/trino/plugin/hudi/HudiConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hudi.columns-to-hide` | List<String> | `ImmutableList.of()` | List of column names that will be hidden from the query output. It can be used to hide Hudi meta fields. By default, no fields are hidden. |  |
| `hudi.ignore-absent-partitions` | boolean | `false` |  |  |
| `hudi.max-outstanding-splits` | int | `1000` | Maximum outstanding splits in a batch enqueued for processing. |  |
| `hudi.max-split-size` | DataSize | `DataSize.of(120, MEGABYTE)` | Maximum size for a single split. Defaults to 120MB. |  |
| `hudi.max-splits-per-second` | int | `Integer.MAX_VALUE` | Rate at which splits are enqueued for processing. The queue will throttle if this rate limit is breached. |  |
| `hudi.minimum-assigned-split-weight` | double | `0.05` | Minimum weight that a split can be assigned when size based split weights are enabled. |  |
| `hudi.parquet.use-column-names` | boolean | `true` | Access Parquet columns using names from the file. If disabled, then columns are accessed using index.Only applicable to Parquet file format. |  |
| `hudi.per-transaction-metastore-cache-maximum-size` | long | `2000` |  |  |
| `hudi.query-partition-filter-required` | boolean | `false` | Require a filter on at least one partition column |  |
| `hudi.size-based-split-weights-enabled` | boolean | `true` | Unlike uniform splitting, size-based splitting ensures that each batch of splits has enough data to process. By default, it is enabled to improve performance. |  |
| `hudi.split-generator-parallelism` | int | `4` | Number of threads to generate splits from partitions. |  |
| `hudi.split-loader-parallelism` | int | `4` | Number of threads to run background split loader. A single background split loader is needed per query. |  |
| `hudi.standard-split-weight-size` | DataSize | `DataSize.of(128, MEGABYTE)` | The split size corresponding to the standard weight (1.0) when size based split weights are enabled. |  |

Removed properties, rejected at startup if still set: `hudi.max-partition-batch-size`, `hudi.metadata-enabled`, `hudi.min-partition-batch-size`
