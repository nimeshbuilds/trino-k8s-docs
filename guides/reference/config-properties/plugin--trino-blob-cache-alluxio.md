# Configuration properties: `plugin/trino-blob-cache-alluxio`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [AlluxioCacheConfig](#alluxiocacheconfig)

## AlluxioCacheConfig

Source: `plugin/trino-blob-cache-alluxio/src/main/java/io/trino/blob/cache/alluxio/AlluxioCacheConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `fs.cache.directories` | List<String> | `ImmutableList.of()` | Base directory to cache data. Use a comma-separated list to cache data in multiple directories. |  |
| `fs.cache.max-disk-usage-percentages` | List<Integer> | `ImmutableList.of()` | The maximum percentage (0-100) of total disk size the cache can use. Use a comma-separated list of percentage values if supplying several cache directories. |  |
| `fs.cache.max-sizes` | List<DataSize> | `ImmutableList.of()` | The maximum cache size for a cache directory. Use a comma-separated list of sizes to specify allowed maximum values for each directory. |  |
| `fs.cache.page-size` | DataSize | `DataSize.valueOf("1MB")` | Page size for cache |  |
| `fs.cache.ttl` | Duration | `Optional.of(Duration.valueOf("7d"))` | Duration to keep files in the cache prior to eviction |  |
