# Configuration properties: `lib/trino-metastore`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [CachingHiveMetastoreConfig](#cachinghivemetastoreconfig), [ImpersonationCachingConfig](#impersonationcachingconfig)

## CachingHiveMetastoreConfig

Source: `lib/trino-metastore/src/main/java/io/trino/metastore/cache/CachingHiveMetastoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore-cache-maximum-size` | long | `20000` |  |  |
| `hive.metastore-cache-ttl` | Duration | `new Duration(0, SECONDS)` |  |  |
| `hive.metastore-cache.cache-missing` | boolean | `true` |  |  |
| `hive.metastore-cache.cache-missing-partitions` | boolean | `false` |  |  |
| `hive.metastore-cache.cache-missing-stats` | boolean | `false` |  |  |
| `hive.metastore-cache.cache-partitions` | boolean | `true` |  |  |
| `hive.metastore-refresh-interval` | Duration | `Optional.empty()` |  |  |
| `hive.metastore-refresh-max-threads` | int | `10` |  |  |
| `hive.metastore-stats-cache-ttl` | Duration | `Optional.empty()` |  |  |

## ImpersonationCachingConfig

Source: `lib/trino-metastore/src/main/java/io/trino/metastore/cache/ImpersonationCachingConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.user-metastore-cache-maximum-size` | long | `1000` |  |  |
| `hive.user-metastore-cache-ttl` | Duration | `new Duration(0, TimeUnit.SECONDS)` |  |  |
