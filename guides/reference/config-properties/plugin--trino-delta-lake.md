# Configuration properties: `plugin/trino-delta-lake`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [DeltaLakeConfig](#deltalakeconfig), [DeltaLakeGlueMetastoreConfig](#deltalakegluemetastoreconfig), [DeltaLakeSecurityConfig](#deltalakesecurityconfig)

## DeltaLakeConfig

Source: `plugin/trino-delta-lake/src/main/java/io/trino/plugin/deltalake/DeltaLakeConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `delta.checkpoint-processing.parallelism` | int | `4` | Limits per table scan checkpoint files processing parallelism |  |
| `delta.checkpoint-row-statistics-writing.enabled` | boolean | `true` |  |  |
| `delta.compression-codec` | HiveCompressionOption | `HiveCompressionOption.ZSTD` | Compression codec to use when writing new data files |  |
| `delta.default-checkpoint-writing-interval` | long | `10` | How often (in number of transactions) to write checkpoint of transaction log |  |
| `delta.delete-schema-locations-fallback` | boolean | `false` | Whether schema locations should be deleted when Trino can't determine whether they contain external files. |  |
| `delta.deletion-vectors-enabled` | boolean | `false` | Enable deletion vectors by default for new tables |  |
| `delta.domain-compaction-threshold` | int | `1000` | Maximum ranges to allow in a tuple domain without compacting it |  |
| `delta.dynamic-filtering.wait-timeout` | Duration | `new Duration(0, SECONDS)` | Duration to wait for completion of dynamic filters during split generation |  |
| `delta.enable-non-concurrent-writes` | boolean | `false` |  |  |
| `delta.extended-statistics.collect-on-write` | boolean | `true` | Enables automatic column level extended statistics collection on write |  |
| `delta.extended-statistics.enabled` | boolean | `true` | Enable collection (ANALYZE) and use of extended statistics. |  |
| `delta.fs.cache.disable-transaction-log-caching` | boolean | `false` | Disable blob caching of the _delta_log directory (effective only when fs.cache.enabled=true) |  |
| `delta.hive-catalog-name` | String | `Optional.empty()` | Catalog to redirect to when a Hive table is referenced |  |
| `delta.idle-writer-min-file-size` | DataSize | `DataSize.of(16, MEGABYTE)` | Minimum data written by a single partition writer before it can be considered as 'idle' and could be closed by the engine |  |
| `delta.load-metadata-from-checksum-file` | boolean | `true` | Read table metadata and protocol from the Delta version checksum file when available, falling back to the transaction log |  |
| `delta.max-outstanding-splits` | int | `1_000` | Target number of buffered splits for each table scan in a query, before the scheduler tries to pause itself |  |
| `delta.max-partitions-per-writer` | int | `100` | Maximum number of partitions per writer |  |
| `delta.max-split-size` | DataSize | `DataSize.of(128, MEGABYTE)` |  |  |
| `delta.max-splits-per-second` | int | `Integer.MAX_VALUE` | Throttles the maximum number of splits that can be assigned to tasks per second |  |
| `delta.metadata.cache-max-retained-size` | DataSize | `DEFAULT_METADATA_CACHE_MAX_RETAINED_SIZE` | Maximum retained size of Delta table metadata stored in cache |  |
| `delta.metadata.cache-ttl` | Duration | `new Duration(30, TimeUnit.MINUTES)` | Caching duration for Delta table metadata (e.g. table schema, partition info) |  |
| `delta.metadata.parallelism` | int | `8` | Limits metadata enumeration calls parallelism |  |
| `delta.metadata.virtual-threads-enabled` | boolean | `true` | Run blocking metadata enumeration I/O on virtual threads |  |
| `delta.metastore.store-table-metadata` | boolean | `false` | Store table metadata in metastore |  |
| `delta.metastore.store-table-metadata-interval` | Duration | `new Duration(1, SECONDS)` | Interval to store table metadata in metastore | hidden |
| `delta.metastore.store-table-metadata-threads` | String | `5` | Number of threads used for storing table metadata in metastore |  |
| `delta.minimum-assigned-split-weight` | double | `0.05` | Minimum weight that a split can be assigned |  |
| `delta.object-store-layout.enabled` | boolean | `false` | Enable the Delta Lake object store file layout by default for new tables |  |
| `delta.parquet.time-zone` | String | `TimeZone.getDefault().getID()` | Time zone for Parquet read and write | Legacy names: `hive.parquet.time-zone` |
| `delta.per-transaction-metastore-cache-maximum-size` | long | `1000` |  | Legacy names: `hive.per-transaction-metastore-cache-maximum-size` |
| `delta.projection-pushdown-enabled` | boolean | `true` | Read only required fields from a row type |  |
| `delta.query-partition-filter-required` | boolean | `false` | Require filter on at least one partition column |  |
| `delta.register-table-procedure.enabled` | boolean | `false` | Allow users to call the register_table procedure |  |
| `delta.s3.transaction-log-conditional-writes.enabled` | boolean | `true` | Whether to use conditional writes when writing Delta table transaction log files on S3. If false, lock-file based synchronization will be used. | Legacy names: `s3.exclusive-create` |
| `delta.table-statistics-enabled` | boolean | `true` | Expose table statistics |  |
| `delta.target-max-file-size` | DataSize | `DataSize.of(1, GIGABYTE)` | Target maximum size of written files; the actual size may be larger |  |
| `delta.transaction-log.max-cached-file-size` | DataSize | `DEFAULT_TRANSACTION_LOG_MAX_CACHED_SIZE` | Maximum size of delta transaction log file that will be cached in memory |  |
| `delta.unique-table-location` | boolean | `true` | Use randomized, unique table locations |  |
| `delta.vacuum.min-retention` | Duration | `new Duration(7, DAYS)` | Minimal retention period for vacuum procedure |  |

Removed properties, rejected at startup if still set: `delta.checkpoint-filtering.enabled`, `delta.experimental.ignore-checkpoint-write-failures`, `delta.legacy-create-table-with-existing-location.enabled`, `delta.max-initial-split-size`, `delta.max-initial-splits`, `delta.metadata.cache-size`, `delta.metadata.live-files.cache-size`, `delta.metadata.live-files.cache-ttl`

## DeltaLakeGlueMetastoreConfig

Source: `plugin/trino-delta-lake/src/main/java/io/trino/plugin/deltalake/metastore/glue/DeltaLakeGlueMetastoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `delta.hide-non-delta-lake-tables` | boolean | `false` | Hide non-Delta Lake tables in table listings |  |

## DeltaLakeSecurityConfig

Source: `plugin/trino-delta-lake/src/main/java/io/trino/plugin/deltalake/DeltaLakeSecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `delta.security` | DeltaLakeSecurity | `ALLOW_ALL` | Authorization checks for Delta Lake connector |  |
