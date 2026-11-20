# Configuration properties: `plugin/trino-blob-cache-memory`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [MemoryBlobCacheConfig](#memoryblobcacheconfig)

## MemoryBlobCacheConfig

Source: `plugin/trino-blob-cache-memory/src/main/java/io/trino/blob/cache/memory/MemoryBlobCacheConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `fs.memory-cache.max-content-length` | DataSize | `DataSize.of(8, MEGABYTE)` | Maximum size of file that can be cached |  |
| `fs.memory-cache.max-size` | DataSize | `DEFAULT_CACHE_SIZE` | Maximum total size of the cache |  |
| `fs.memory-cache.ttl` | Duration | `new Duration(1, HOURS)` | Duration to keep files in the cache prior to eviction |  |
