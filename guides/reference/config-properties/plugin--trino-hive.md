# Configuration properties: `plugin/trino-hive`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [FileHiveMetastoreConfig](#filehivemetastoreconfig), [GlueHiveMetastoreConfig](#gluehivemetastoreconfig), [HiveConfig](#hiveconfig), [HiveMetastoreConfig](#hivemetastoreconfig), [MetastoreKerberosConfig](#metastorekerberosconfig), [MetastoreTypeConfig](#metastoretypeconfig), [OrcReaderConfig](#orcreaderconfig), [OrcWriterConfig](#orcwriterconfig), [ParquetEncryptionConfig](#parquetencryptionconfig), [ParquetReaderConfig](#parquetreaderconfig), [ParquetWriterConfig](#parquetwriterconfig), [SecurityConfig](#securityconfig), [SortingFileWriterConfig](#sortingfilewriterconfig), [StaticMetastoreConfig](#staticmetastoreconfig), [ThriftMetastoreAuthenticationConfig](#thriftmetastoreauthenticationconfig), [ThriftMetastoreConfig](#thriftmetastoreconfig)

## FileHiveMetastoreConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/file/FileHiveMetastoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore.catalog.dir` | String |  | Hive file-based metastore catalog directory |  |
| `hive.metastore.disable-location-checks` | boolean | `false` |  |  |
| `hive.metastore.user` | String | `"presto"` | Hive file-based metastore username for file access |  |
| `hive.metastore.version-compatibility` | VersionCompatibility | `NOT_SUPPORTED` |  |  |

Removed properties, rejected at startup if still set: `hive.metastore.assume-canonical-partition-keys`

## GlueHiveMetastoreConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/glue/GlueHiveMetastoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore.glue.assume-canonical-partition-keys` | boolean | `false` | Allow conversion of non-char types (eg BIGINT, date) to canonical string formats |  |
| `hive.metastore.glue.aws-access-key` | String | `Optional.empty()` | Hive Glue metastore AWS access key |  |
| `hive.metastore.glue.aws-secret-key` | String | `Optional.empty()` | Hive Glue metastore AWS secret key | sensitive |
| `hive.metastore.glue.catalogid` | String | `Optional.empty()` | Hive Glue metastore catalog id |  |
| `hive.metastore.glue.default-warehouse-dir` | String | `Optional.empty()` | Hive Glue metastore default warehouse directory |  |
| `hive.metastore.glue.endpoint-url` | URI | `Optional.empty()` | Glue API endpoint URL |  |
| `hive.metastore.glue.external-id` | String | `Optional.empty()` | External ID for the IAM role trust policy when connecting to Glue |  |
| `hive.metastore.glue.iam-role` | String | `Optional.empty()` | ARN of an IAM role to assume when connecting to Glue |  |
| `hive.metastore.glue.max-connections` | int | `30` | Max number of concurrent connections to Glue |  |
| `hive.metastore.glue.max-error-retries` | int | `10` | Maximum number of error retries for the Glue client |  |
| `hive.metastore.glue.partitions-segments` | int | `5` | Number of segments for partitioned Glue tables |  |
| `hive.metastore.glue.pin-client-to-current-region` | boolean | `false` | Should the Glue client be pinned to the current EC2 region |  |
| `hive.metastore.glue.region` | String | `Optional.empty()` | AWS Region for Glue Data Catalog |  |
| `hive.metastore.glue.skip-archive` | boolean | `false` | Skip archiving an old table version when updating a table in the Glue metastore |  |
| `hive.metastore.glue.sts.endpoint` | URI | `Optional.empty()` | AWS STS endpoint for Glue authentication |  |
| `hive.metastore.glue.sts.region` | String | `Optional.empty()` | AWS STS signing region for Glue authentication |  |
| `hive.metastore.glue.threads` | int | `40` | Number of threads for parallel operations |  |
| `hive.metastore.glue.use-web-identity-token-credentials-provider` | boolean | `false` | If true, explicitly use the WebIdentityTokenCredentialsProvider instead of the default credential provider chain. |  |

Removed properties, rejected at startup if still set: `hive.metastore.glue.aws-credentials-provider`, `hive.metastore.glue.get-partition-threads`, `hive.metastore.glue.proxy-api-id`, `hive.metastore.glue.read-statistics-threads`, `hive.metastore.glue.use-instance-credentials`, `hive.metastore.glue.write-statistics-threads`

## HiveConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/HiveConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.allow-register-partition-procedure` | boolean | `false` |  |  |
| `hive.auto-purge` | boolean | `false` |  |  |
| `hive.bucket-execution` | boolean | `true` | Enable bucket-aware execution: use physical bucketing information to optimize queries |  |
| `hive.collect-column-statistics-on-write` | boolean | `true` | Enables automatic column level statistics collection on write |  |
| `hive.compression-codec` | HiveCompressionOption | `HiveCompressionOption.GZIP` |  |  |
| `hive.create-empty-bucket-files` | boolean | `false` | Create empty files for buckets that have no data |  |
| `hive.delegate-transactional-managed-table-location-to-metastore` | boolean | `false` | When transactional, managed table is created via Trino the location will not be set in request sent to HMS and location will be determined by metastore; if this value is set to true, CREATE TABLE AS queries are not supported. |  |
| `hive.delete-schema-locations-fallback` | boolean | `false` | Whether schema locations should be deleted when Trino can't determine whether they contain external files. |  |
| `hive.delta-lake-catalog-name` | String | `Optional.empty()` | Catalog to redirect to when a Delta Lake table is referenced |  |
| `hive.domain-compaction-threshold` | int | `1000` | Maximum ranges to allow in a tuple domain without compacting it |  |
| `hive.dynamic-filtering.wait-timeout` | Duration | `new Duration(0, MINUTES)` | Duration to wait for completion of dynamic filters during split generation | Legacy names: `hive.dynamic-filtering-probe-blocking-timeout` |
| `hive.file-status-cache-expire-time` | Duration | `new Duration(1, MINUTES)` |  |  |
| `hive.file-status-cache-tables` | List<String> | `ImmutableList.of()` |  |  |
| `hive.file-status-cache.excluded-tables` | List<String> | `ImmutableList.of()` | List of tables that should be excluded from file status caching |  |
| `hive.file-status-cache.max-retained-size` | DataSize | `DataSize.of(1, GIGABYTE)` |  |  |
| `hive.force-local-scheduling` | boolean | `false` |  |  |
| `hive.hive-views.enabled` | boolean | `false` | Experimental: Allow translation of Hive views into Trino views | Legacy names: `hive.views-execution.enabled`, `hive.translate-hive-views` |
| `hive.hive-views.legacy-translation` | boolean | `false` | Use legacy Hive view translation mechanism | Legacy names: `hive.legacy-hive-view-translation` |
| `hive.hive-views.run-as-invoker` | boolean | `false` | Execute Hive views with permissions of invoker |  |
| `hive.hudi-catalog-name` | String | `Optional.empty()` | Catalog to redirect to when a Hudi table is referenced |  |
| `hive.iceberg-catalog-name` | String | `Optional.empty()` | The catalog to redirect iceberg tables to |  |
| `hive.idle-writer-min-file-size` | DataSize | `DataSize.of(16, MEGABYTE)` | Minimum data written by a single partition writer before it can be consider as 'idle' and could be closed by the engine |  |
| `hive.ignore-absent-partitions` | boolean | `false` |  |  |
| `hive.ignore-corrupted-statistics` | boolean | `false` | Ignore corrupted statistics rather than failing |  |
| `hive.immutable-partitions` | boolean | `false` | Can new data be inserted into existing partitions or existing unpartitioned tables |  |
| `hive.insert-existing-partitions-behavior` | InsertExistingPartitionsBehavior | `Optional.empty()` | Default value for insert existing partitions behavior |  |
| `hive.max-concurrent-file-system-operations` | int | `20` |  | Legacy names: `hive.max-concurrent-file-renames` |
| `hive.max-concurrent-metastore-drops` | int | `20` |  |  |
| `hive.max-concurrent-metastore-updates` | int | `20` |  |  |
| `hive.max-outstanding-splits` | int | `3_000` | Target number of buffered splits for each table scan in a query, before the scheduler tries to pause itself |  |
| `hive.max-outstanding-splits-size` | DataSize | `DataSize.of(256, MEGABYTE)` | Maximum amount of memory allowed for split buffering for each table scan in a query, before the query is failed |  |
| `hive.max-partition-drops-per-query` | int | `100_000` |  |  |
| `hive.max-partitions-for-eager-load` | int | `100_000` | Maximum allowed partitions for a single table scan to be loaded eagerly on coordinator. Certain optimizations are not possible without eager loading. |  |
| `hive.max-partitions-per-scan` | int | `1_000_000` | Maximum allowed partitions for a single table scan |  |
| `hive.max-partitions-per-writers` | int | `100` | Maximum number of partitions per writer |  |
| `hive.max-split-iterator-threads` | int | `1_000` |  |  |
| `hive.max-split-size` | DataSize | `DataSize.of(64, MEGABYTE)` |  |  |
| `hive.max-splits-per-second` | Integer |  | Throttles the maximum number of splits that can be assigned to tasks per second |  |
| `hive.metadata.parallelism` | int | `8` | Limits metadata enumeration calls parallelism |  |
| `hive.metadata.virtual-threads-enabled` | boolean | `true` | Run blocking metadata enumeration I/O on virtual threads |  |
| `hive.metastore.partition-batch-size.max` | int | `100` |  |  |
| `hive.metastore.partition-batch-size.min` | int | `10` |  |  |
| `hive.minimum-assigned-split-weight` | double | `0.05` | Minimum weight that a split can be assigned when size based split weights are enabled |  |
| `hive.non-managed-table-creates-enabled` | boolean | `true` | Enable non-managed (external) table creates |  |
| `hive.non-managed-table-writes-enabled` | boolean | `false` | Enable writes to non-managed (external) tables |  |
| `hive.optimize-mismatched-bucket-count` | boolean | `true` |  |  |
| `hive.orc.time-zone` | String | `TimeZone.getDefault().getID()` | Time zone for legacy ORC files that do not contain a time zone |  |
| `hive.parallel-partitioned-bucketed-writes` | boolean | `true` | Improve parallelism of partitioned and bucketed table writes | Legacy names: `hive.parallel-partitioned-bucketed-inserts` |
| `hive.parquet.max-split-size` | DataSize | `DataSize.of(120, MEGABYTE)` | Largest size of a single file section assigned to a worker for Parquet files |  |
| `hive.parquet.time-zone` | String | `TimeZone.getDefault().getID()` | Time zone for Parquet read and write |  |
| `hive.parquet.use-column-names` | boolean | `true` | Access Parquet columns using names from the file |  |
| `hive.partition-projection-enabled` | boolean | `true` | Enables AWS Athena partition projection |  |
| `hive.partition-statistics-sample-size` | int | `100` | Maximum sample size of the partitions column statistics |  |
| `hive.per-transaction-file-status-cache.max-retained-size` | DataSize | `DataSize.of(100, MEGABYTE)` | Maximum retained size of file statuses cached by transactional file status cache |  |
| `hive.per-transaction-metastore-cache-maximum-size` | long | `1000` |  |  |
| `hive.projection-pushdown-enabled` | boolean | `true` | Projection pushdown into hive is enabled through applyProjection |  |
| `hive.propagate-table-scan-sorting-properties` | boolean | `false` | Use sorted table layout to generate more efficient execution plans. May lead to incorrect results if files are not sorted as per table definition. |  |
| `hive.protobuf.descriptors.cache.max-size` | long | `64` | The maximum amount of protobuf descriptors to keep in memory |  |
| `hive.protobuf.descriptors.cache.refresh-interval` | Duration | `new Duration(1, TimeUnit.DAYS)` | Interval on when loaded descriptors should be refreshed |  |
| `hive.protobuf.descriptors.location` | Path |  | Directory where binary protobuf descriptors are stored to use for deserializing protobufs |  |
| `hive.query-partition-filter-required` | boolean | `false` | Require filter on at least one partition column |  |
| `hive.query-partition-filter-required-schemas` | List<String> | `ImmutableSet.of()` | List of schemas for which filter on partition column is enforced |  |
| `hive.rcfile.time-zone` | String | `TimeZone.getDefault().getID()` | Time zone for RCFile binary read and write |  |
| `hive.rcfile.writer.validate` | boolean | `false` | Validate RCFile after write by re-reading the whole file |  |
| `hive.recursive-directories` | boolean | `false` |  |  |
| `hive.respect-table-format` | boolean | `true` | Should new partitions be written using the existing table format or the default Trino format |  |
| `hive.s3-glacier-filter` | S3GlacierFilter | `S3GlacierFilter.READ_ALL` | Filter based on storage class of S3 object | Legacy names: `hive.s3.storage-class-filter` |
| `hive.single-statement-writes` | boolean | `false` | Require transaction to be in auto-commit mode for writes |  |
| `hive.size-based-split-weights-enabled` | boolean | `true` |  |  |
| `hive.skip-deletion-for-alter` | boolean | `false` | Skip deletion of old partition data when a partition is deleted and then inserted in the same transaction |  |
| `hive.skip-target-cleanup-on-rollback` | boolean | `false` | Skip deletion of target directories when a metastore operation fails |  |
| `hive.sorted-writing` | boolean | `true` | Enable writing to bucketed sorted tables |  |
| `hive.split-loader-concurrency` | int | `64` |  |  |
| `hive.storage-format` | HiveStorageFormat | `HiveStorageFormat.PARQUET` |  |  |
| `hive.table-statistics-enabled` | boolean | `true` | Enable use of table statistics |  |
| `hive.target-max-file-size` | DataSize | `DataSize.of(1, GIGABYTE)` | Target maximum size of written files; the actual size may be larger |  |
| `hive.temporary-staging-directory-enabled` | boolean | `true` | Should use (if possible) temporary staging directory for write operations |  |
| `hive.temporary-staging-directory-path` | String | `"/tmp/presto-${USER}"` | Location of temporary staging directory for write operations. Use ${USER} placeholder to use different location for each user. |  |
| `hive.text.max-line-length` | DataSize | `DataSize.of(100, MEGABYTE)` | Maximum line length for text files |  |
| `hive.timestamp-precision` | HiveTimestampPrecision | `HiveTimestampPrecision.DEFAULT_PRECISION` | Precision used to represent timestamps |  |
| `hive.transaction-heartbeat-interval` | Duration | `Optional.empty()` | Interval after which heartbeat is sent for open Hive transaction |  |
| `hive.transaction-heartbeat-threads` | int | `5` | Number of threads to run in the Hive transaction heartbeat service |  |
| `hive.validate-bucketing` | boolean | `true` | Verify that data is bucketed correctly when reading |  |

Removed properties, rejected at startup if still set: `dfs.domain-socket-path`, `hive.allow-corrupt-writes-for-testing`, `hive.assume-canonical-partition-keys`, `hive.bucket-writing`, `hive.file-system-cache-ttl`, `hive.max-global-split-iterator-threads`, `hive.max-initial-split-size`, `hive.max-initial-splits`, `hive.max-sort-files-per-bucket`, `hive.optimize-symlink-listing`, `hive.optimized-reader.enabled`, `hive.partition-use-column-names`, `hive.rcfile-optimized-writer.enabled`, `hive.s3select-pushdown.enabled`, `hive.s3select-pushdown.experimental-textfile-pushdown-enabled`, `hive.s3select-pushdown.max-connections`, `hive.time-zone`, `hive.write-validation-threads`

## HiveMetastoreConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/HiveMetastoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.hide-delta-lake-tables` | boolean | `false` | Hide Delta Lake tables in table listings |  |

## MetastoreKerberosConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/thrift/MetastoreKerberosConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore.client.credential-cache.location` | String |  | Hive Metastore client credential cache location |  |
| `hive.metastore.client.keytab` | String |  | Hive Metastore client keytab location |  |
| `hive.metastore.client.principal` | String |  | Hive Metastore client principal |  |
| `hive.metastore.service.principal` | String |  | Hive Metastore service principal |  |

## MetastoreTypeConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/MetastoreTypeConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore` | MetastoreType | `THRIFT` |  |  |

## OrcReaderConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/orc/OrcReaderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.orc.bloom-filters.enabled` | boolean |  |  |  |
| `hive.orc.lazy-read-small-ranges` | boolean |  | ORC read small disk ranges lazily | deprecated |
| `hive.orc.max-buffer-size` | DataSize |  |  |  |
| `hive.orc.max-merge-distance` | DataSize |  |  |  |
| `hive.orc.max-read-block-size` | DataSize |  |  |  |
| `hive.orc.nested-lazy` | boolean |  | ORC lazily read nested data | deprecated |
| `hive.orc.read-legacy-short-zone-id` | boolean |  | Allow reads on ORC files with short zone ID in the stripe footer |  |
| `hive.orc.stream-buffer-size` | DataSize |  |  |  |
| `hive.orc.tiny-stripe-threshold` | DataSize |  |  |  |
| `hive.orc.use-column-names` | boolean | `false` | Access ORC columns using names from the file |  |

## OrcWriterConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/orc/OrcWriterConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.orc.default-bloom-filter-fpp` | double |  | ORC Bloom filter false positive probability |  |
| `hive.orc.writer.dictionary-max-memory` | DataSize |  |  |  |
| `hive.orc.writer.max-compression-buffer-size` | DataSize |  |  |  |
| `hive.orc.writer.row-group-max-rows` | int |  |  |  |
| `hive.orc.writer.string-statistics-limit` | DataSize |  |  |  |
| `hive.orc.writer.stripe-max-rows` | int |  |  |  |
| `hive.orc.writer.stripe-max-size` | DataSize |  |  |  |
| `hive.orc.writer.stripe-min-size` | DataSize |  |  |  |
| `hive.orc.writer.validation-mode` | OrcWriteValidationMode | `OrcWriteValidationMode.BOTH` | Level of detail in ORC validation. Lower levels require more memory. |  |
| `hive.orc.writer.validation-percentage` | double | `0.0` | Percentage of ORC files to validate after write by re-reading the whole file |  |
| `hive.orc.writer.writer-identification` | WriterIdentification |  |  |  |

Removed properties, rejected at startup if still set: `hive.orc.optimized-writer.enabled`

## ParquetEncryptionConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/crypto/ParquetEncryptionConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `pme.aad-prefix` | String | `Optional.empty()` | AAD prefix used to decode Parquet files |  |
| `pme.check-footer-integrity` | boolean | `true` | Validate signature for plaintext footer files |  |
| `pme.environment-key-retriever.enabled` | boolean | `false` | Enable the key retriever that retrieves keys from the environment variable |  |

## ParquetReaderConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/parquet/ParquetReaderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `parquet.experimental.vectorized-decoding.enabled` | boolean |  | Enable using Java Vector API for faster decoding of parquet files |  |
| `parquet.footer-read-size` | DataSize |  | Expected size of the Parquet footer to read before falling back to a second read |  |
| `parquet.ignore-statistics` | boolean |  | Ignore statistics from Parquet to allow querying files with corrupted or incorrect statistics |  |
| `parquet.max-buffer-size` | DataSize |  |  |  |
| `parquet.max-footer-read-size` | DataSize |  | Maximum allowed size of the parquet footer. Files with footers larger than this will generate an exception on read |  |
| `parquet.max-merge-distance` | DataSize |  |  |  |
| `parquet.max-page-read-size` | DataSize |  | Maximum allowed size of a parquet page during reads. Files with parquet pages larger than this will generate an exception on read |  |
| `parquet.max-read-block-row-count` | int |  | Maximum number of rows read in a batch |  |
| `parquet.max-read-block-size` | DataSize |  |  |  |
| `parquet.selected-positions-pushdown-enabled` | boolean |  | Enable pushing selected positions into Parquet column readers |  |
| `parquet.small-file-threshold` | DataSize |  | Size below which a parquet file will be read entirely |  |
| `parquet.use-bloom-filter` | boolean |  | Use Parquet Bloom filters |  |
| `parquet.use-column-index` | boolean |  | Enable using Parquet column indexes |  |

Removed properties, rejected at startup if still set: `hive.parquet.fail-on-corrupted-statistics`, `hive.parquet.max-read-block-size`, `parquet.fail-on-corrupted-statistics`, `parquet.optimized-nested-reader.enabled`, `parquet.optimized-reader.enabled`

## ParquetWriterConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/parquet/ParquetWriterConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `parquet.writer.batch-size` | int | `ParquetWriterOptions.DEFAULT_BATCH_SIZE` | Maximum number of rows passed to the writer in each batch |  |
| `parquet.writer.delta-length-byte-array-encoding-enabled` | boolean | `true` | Use DELTA_LENGTH_BYTE_ARRAY encoding instead of PLAIN as the non-dictionary fallback for BYTE_ARRAY columns |  |
| `parquet.writer.page-size` | DataSize | `DataSize.ofBytes(ParquetProperties.DEFAULT_PAGE_SIZE)` |  |  |
| `parquet.writer.page-value-count` | int | `ParquetWriterOptions.DEFAULT_MAX_PAGE_VALUE_COUNT` |  |  |
| `parquet.writer.row-group-max-row-count` | int | `ParquetWriterOptions.DEFAULT_MAX_ROW_GROUP_ROW_COUNT` | Maximum row count of row groups created by the Parquet writer |  |
| `parquet.writer.row-group-size` | DataSize | `DataSize.of(128, MEGABYTE)` |  | Legacy names: `parquet.writer.block-size` |
| `parquet.writer.validation-percentage` | double | `5` | Percentage of parquet files to validate after write by re-reading the whole file |  |

Removed properties, rejected at startup if still set: `hive.parquet.optimized-writer.enabled`, `hive.parquet.writer.block-size`, `hive.parquet.writer.page-size`, `parquet.experimental-optimized-writer.enabled`, `parquet.optimized-writer.enabled`, `parquet.optimized-writer.validation-percentage`

## SecurityConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/security/SecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.security` | HiveSecurity | `ALLOW_ALL` |  |  |

## SortingFileWriterConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/SortingFileWriterConfig.java`

Bound with prefix: `hive`, `iceberg`, `lakehouse`. Prepend the prefix and a dot to each property below where this binding applies.

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `max-open-sort-files` | int | `50` | Maximum number of writer temporary files to read in one pass |  |
| `writer-sort-buffer-size` | DataSize | `DataSize.of(64, MEGABYTE)` |  |  |

## StaticMetastoreConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/thrift/StaticMetastoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore.uri` | List<String> |  | Hive metastore URIs (comma separated) |  |
| `hive.metastore.username` | String |  | Optional username for accessing the Hive metastore |  |

## ThriftMetastoreAuthenticationConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/thrift/ThriftMetastoreAuthenticationConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore.authentication.type` | ThriftMetastoreAuthenticationType | `NONE` | Thrift metastore authentication type |  |

## ThriftMetastoreConfig

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/metastore/thrift/ThriftMetastoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `hive.metastore.thrift.assume-canonical-partition-keys` | boolean | `false` |  |  |
| `hive.metastore.thrift.catalog-name` | String |  | Hive metastore thrift catalog name |  |
| `hive.metastore.thrift.client.backoff-scale-factor` | double | `RetryDriver.DEFAULT_SCALE_FACTOR` | Scale factor for metastore request retry delay |  |
| `hive.metastore.thrift.client.connect-timeout` | Duration | `new Duration(10, TimeUnit.SECONDS)` | Socket connect timeout for metastore client | Legacy names: `hive.metastore-timeout` |
| `hive.metastore.thrift.client.max-backoff-delay` | Duration | `RetryDriver.DEFAULT_MAX_BACKOFF_DELAY` | Maximum delay between metastore request retries |  |
| `hive.metastore.thrift.client.max-retries` | int | `RetryDriver.DEFAULT_MAX_ATTEMPTS - 1` | Maximum number of retry attempts for metastore requests |  |
| `hive.metastore.thrift.client.max-retry-time` | Duration | `RetryDriver.DEFAULT_MAX_RETRY_TIME` | Total time limit for a metastore request to be retried |  |
| `hive.metastore.thrift.client.min-backoff-delay` | Duration | `RetryDriver.DEFAULT_MIN_BACKOFF_DELAY` | Minimum delay between metastore request retries |  |
| `hive.metastore.thrift.client.read-timeout` | Duration | `new Duration(10, TimeUnit.SECONDS)` | Socket read timeout for metastore client | Legacy names: `hive.metastore-timeout` |
| `hive.metastore.thrift.client.socks-proxy` | HostAndPort |  |  |  |
| `hive.metastore.thrift.client.ssl.enabled` | boolean | `false` | Whether TLS security is enabled |  |
| `hive.metastore.thrift.client.ssl.key` | File |  | Path to the key store |  |
| `hive.metastore.thrift.client.ssl.key-password` | String |  | Password for the key store | sensitive |
| `hive.metastore.thrift.client.ssl.trust-certificate` | File |  | Path to the trust store |  |
| `hive.metastore.thrift.client.ssl.trust-certificate-password` | String |  | Password for the trust store | sensitive |
| `hive.metastore.thrift.delegation-token.cache-maximum-size` | long | `1000` | Delegation token cache maximum size |  |
| `hive.metastore.thrift.delegation-token.cache-ttl` | Duration | `new Duration(1, TimeUnit.HOURS)` | Time to live delegation token cache for metastore |  |
| `hive.metastore.thrift.delete-files-on-drop` | boolean | `false` | Delete files on drop in case the metastore doesn't do it |  |
| `hive.metastore.thrift.impersonation.enabled` | boolean | `false` | Should end user be impersonated when communicating with metastore | Legacy names: `hive.metastore.impersonation-enabled` |
| `hive.metastore.thrift.txn-lock-max-wait` | Duration | `new Duration(10, TimeUnit.MINUTES)` | Maximum time to wait to acquire hive transaction lock |  |
| `hive.metastore.thrift.use-spark-table-statistics-fallback` | boolean | `true` | Enable usage of table statistics generated by Apache Spark when hive table statistics are not available |  |
| `hive.metastore.thrift.write-statistics-threads` | int | `20` | Number of threads for parallel statistics writes |  |

Removed properties, rejected at startup if still set: `hive.metastore.thrift.batch-fetch.enabled`
