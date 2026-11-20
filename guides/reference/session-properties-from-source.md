# Session properties from source

Extracted from the `*SessionProperties.java` classes of Trino release 484.

System session properties are set with `SET SESSION name = value`. Catalog session properties are set with `SET SESSION <catalog>.name = value`, where `<catalog>` is the name of a catalog that uses that connector.

The Type column is the kind of property builder used in the source. `custom` means the type is defined in code.

The Lakehouse connector defines no session properties of its own. It exposes those of the Hive, Iceberg, Delta Lake and Hudi connectors.

Defaults usually come from the matching configuration property. Check a live value with `SHOW SESSION`.

## `core/trino-main`

### SystemSessionProperties

Source: `core/trino-main/src/main/java/io/trino/SystemSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `adaptive_filter_reordering_enabled` | boolean | Reorder conjunctive/disjunctive filter terms at runtime based on observed selectivity and performance |
| `adaptive_partial_aggregation_enabled` | boolean | When enabled, partial aggregation might be adaptively turned off when it does not provide any performance gain |
| `adaptive_partial_aggregation_unique_rows_ratio_threshold` | double | Ratio between aggregation output and input rows above which partial aggregation might be adaptively turned off |
| `aggregation_operator_unspill_memory_limit` | dataSize | How much memory should be allocated per aggregation operator in unspilling process |
| `allow_pushdown_into_connectors` | boolean | Allow pushdown into connectors |
| `allow_unsafe_pushdown` | boolean | Allow pushing down expressions that may fail for some inputs |
| `close_idle_writers_trigger_duration` | duration | The duration after which the writer operator tries to close the idle writers |
| `collect_plan_statistics_for_all_queries` | boolean | Collect plan statistics for non-EXPLAIN queries |
| `colocated_join` | boolean | Use a colocated join when possible |
| `columnar_filter_evaluation_enabled` | boolean | Enables columnar evaluation of filters |
| `complex_expression_pushdown` | boolean | Allow complex expression pushdown into connectors |
| `cost_estimation_worker_count` | integer | Set the estimate count of workers while planning |
| `debug_adaptive_planner` | boolean | Enable debug information for the adaptive planner |
| `default_filter_factor_enabled` | boolean | use a default filter factor for unknown filters in a filter node |
| `determine_partition_count_for_write_enabled` | boolean | Determine the number of partitions based on amount of data read and processed by the query for write queries |
| `dictionary_aggregation` | boolean | Enable optimization for aggregations on dictionaries |
| `direct_trino_client_fault_tolerant_execution_enabled` | boolean | Allow DirectTrinoClient to consume results of queries running under fault-tolerant execution; when disabled such queries are forced to retry_policy=NONE |
| `distinct_aggregations_strategy` | enum | Strategy to use for distinct aggregations. |
| `distributed_sort` | boolean | Parallelize sort across multiple nodes |
| `dynamic_row_filtering_selectivity_threshold` | double | Avoid using dynamic row filters when fraction of rows selected is above threshold |
| `enable_dynamic_filtering` | boolean | Enable dynamic filtering |
| `enable_dynamic_row_filtering` | boolean | Enable fine-grained filtering of rows in the scan operator using dynamic filters |
| `enable_forced_exchange_below_group_id` | boolean | Enable a stats-based rule adding exchanges below GroupId |
| `enable_intermediate_aggregations` | boolean | Enable the use of intermediate aggregations |
| `enable_stats_calculator` | boolean | Enable statistics calculator |
| `exchange_compression_codec` | enum | Compression codec used for data in exchanges, supports NONE, LZ4, ZSTD |
| `execution_policy` | string | Policy used for scheduling query tasks |
| `fault_tolerant_execution_adaptive_join_reordering_enabled` | boolean | Reorder partitioned join based on run time stats in fault tolerant execution |
| `fault_tolerant_execution_adaptive_join_reordering_min_size_threshold` | dataSize | The minimum size of the right side of join to consider reordering |
| `fault_tolerant_execution_adaptive_join_reordering_size_difference_ratio` | double | The ratio of difference in estimated size of right and left side of join to consider reordering |
| `fault_tolerant_execution_adaptive_query_planning_enabled` | boolean | Enable adaptive query planning for the fault tolerant execution |
| `fault_tolerant_execution_arbitrary_distribution_compute_task_target_size_growth_factor` | double | Growth factor for adaptive sizing of non-writer tasks of arbitrary distribution for fault-tolerant execution |
| `fault_tolerant_execution_arbitrary_distribution_compute_task_target_size_growth_period` | integer | The number of tasks we create for given non-writer stage of arbitrary distribution before we increase task size |
| `fault_tolerant_execution_arbitrary_distribution_compute_task_target_size_max` | dataSize | Max target input size for non-writer task of arbitrary distribution of fault-tolerant execution |
| `fault_tolerant_execution_arbitrary_distribution_compute_task_target_size_min` | dataSize | Initial/min target input size for non-writer tasks of arbitrary distribution of fault-tolerant execution |
| `fault_tolerant_execution_arbitrary_distribution_write_task_target_size_growth_factor` | double | Growth factor for adaptive sizing of writer tasks of arbitrary distribution for fault-tolerant execution |
| `fault_tolerant_execution_arbitrary_distribution_write_task_target_size_growth_period` | integer | The number of tasks we create for given writer stage of arbitrary distribution before we increase task size |
| `fault_tolerant_execution_arbitrary_distribution_write_task_target_size_max` | dataSize | Max target input size for writer tasks of arbitrary distribution of fault-tolerant execution |
| `fault_tolerant_execution_arbitrary_distribution_write_task_target_size_min` | dataSize | Initial/min target input size for writer tasks of arbitrary distribution of fault-tolerant execution |
| `fault_tolerant_execution_coordinator_task_memory` | dataSize | Estimated amount of memory a single coordinator task will use when task level retries are used; value is used when allocating nodes for tasks execution |
| `fault_tolerant_execution_hash_distribution_compute_task_target_size` | dataSize | Target input size for non-writer tasks of hash distribution of fault-tolerant execution |
| `fault_tolerant_execution_hash_distribution_compute_task_to_node_min_ratio` | double | Minimal ratio of tasks count vs cluster nodes count for hash distributed compute stage in fault-tolerant execution |
| `fault_tolerant_execution_hash_distribution_write_task_target_max_count` | integer | Soft upper bound on number of writer tasks in a stage of hash distribution of fault-tolerant execution |
| `fault_tolerant_execution_hash_distribution_write_task_target_size` | dataSize | Target input size of writer tasks of hash distribution of fault-tolerant execution |
| `fault_tolerant_execution_hash_distribution_write_task_to_node_min_ratio` | double | Minimal ratio of tasks count vs cluster nodes count for hash distributed writer stage in fault-tolerant execution |
| `fault_tolerant_execution_max_partition_count` | integer | Maximum number of partitions for distributed joins and aggregations executed with fault tolerant execution enabled |
| `fault_tolerant_execution_max_task_split_count` | integer | Maximal number of splits for a single fault tolerant task (count based) |
| `fault_tolerant_execution_min_partition_count` | integer | Minimum number of partitions for distributed joins and aggregations executed with fault tolerant execution enabled |
| `fault_tolerant_execution_min_partition_count_for_write` | integer | Minimum number of partitions for distributed joins and aggregations in write queries executed with fault tolerant execution enabled |
| `fault_tolerant_execution_min_source_stage_progress` | double | Minimal progress of source stage to consider scheduling of parent stage |
| `fault_tolerant_execution_runtime_adaptive_partitioning_enabled` | boolean | Enables change of number of partitions at runtime when intermediate data size is large |
| `fault_tolerant_execution_runtime_adaptive_partitioning_max_task_size` | dataSize | Max average task input size when deciding runtime adaptive partitioning |
| `fault_tolerant_execution_runtime_adaptive_partitioning_partition_count` | integer | The partition count to use for runtime adaptive partitioning when enabled |
| `fault_tolerant_execution_small_stage_estimation_enabled` | boolean | Enable small stage estimation heuristic, used for more aggresive speculative stage scheduling |
| `fault_tolerant_execution_small_stage_estimation_threshold` | dataSize | Threshold until which stage is considered small |
| `fault_tolerant_execution_small_stage_require_no_more_partitions` | boolean | Is it required for all stage partitions (tasks) to be enumerated for stage to be used in heuristic to determine if parent stage is small |
| `fault_tolerant_execution_small_stage_source_size_multiplier` | double | Multiplier used for heuristic estimation is stage is small; the bigger the more conservative estimation is |
| `fault_tolerant_execution_stage_estimation_for_eager_parent_enabled` | boolean | Enable aggressive stage output size estimation heuristic for children of stages to be executed eagerly |
| `fault_tolerant_execution_standard_split_size` | dataSize | Standard split size for a single fault tolerant task (split weight aware) |
| `fault_tolerant_execution_task_memory` | dataSize | Estimated amount of memory a single task will use when task level retries are used; value is used when allocating nodes for tasks execution |
| `fault_tolerant_execution_task_memory_estimation_quantile` | double | What quantile of memory usage of completed tasks to look at when estimating memory usage for upcoming tasks |
| `fault_tolerant_execution_task_memory_growth_factor` | double | Factor by which estimated task memory is increased if task execution runs out of memory; value is used allocating nodes for tasks execution |
| `filter_and_project_min_output_page_row_count` | integer | Experimental: Minimum output page row count for filter and project operators |
| `filter_and_project_min_output_page_size` | dataSize | Experimental: Minimum output page size for filter and project operators |
| `filter_conjunction_independence_factor` | custom | Scales the strength of independence assumption for selectivity estimates of the conjunction of multiple filters |
| `force_single_node_output` | boolean | Force single node output |
| `force_spilling_join` | boolean | Force the usage of spliing join operator in favor of the non-spilling one, even if spill is not enabled |
| `hide_inaccessible_columns` | boolean | When enabled non-accessible columns are silently filtered from results from SELECT * statements |
| `idle_writer_min_data_size_threshold` | dataSize | Minimum amount of data written by a writer operator on average before it tries to close the idle writers |
| `ignore_downstream_preferences` | boolean | Ignore Parent's PreferredProperties in AddExchange optimizer |
| `ignore_stats_calculator_failures` | boolean | Ignore statistics calculator failures |
| `incremental_hash_array_load_factor_enabled` | boolean | Use smaller load factor for small hash arrays in order to improve performance |
| `initial_splits_per_node` | integer | The number of splits each node will run per task, initially |
| `iterative_optimizer_timeout` | duration | Timeout for plan optimization in iterative optimizer |
| `join_distribution_type` | enum | Join distribution type |
| `join_max_broadcast_table_size` | dataSize | Maximum estimated size of a table that can be broadcast when using automatic join type selection |
| `join_multi_clause_independence_factor` | custom | Scales the strength of independence assumption for selectivity estimates of multi-clause joins |
| `join_partitioned_build_min_row_count` | long | Minimum number of join build side rows required to use partitioned join lookup |
| `join_reordering_strategy` | enum | Join reordering strategy |
| `legacy_catalog_roles` | boolean | Enable legacy role management syntax that assumed all roles are catalog scoped |
| `legacy_varchar_to_char_coercion` | boolean | Implicitly coerce varchar to char, instead of char to varchar |
| `mark_distinct_strategy` | enum | Deprecated. Strategy to use for distinct aggregations. |
| `max_drivers_per_task` | custom | Maximum number of drivers per task |
| `max_grouping_sets` | integer | Maximum number of grouping sets in a GROUP BY |
| `max_hash_partition_count` | integer | Maximum number of partitions for distributed joins and aggregations |
| `max_memory_per_partition_writer` | dataSize | Estimated maximum memory required per partition writer in a single thread |
| `max_partial_top_n_memory` | dataSize | Max memory size for partial Top N aggregations. This can be turned off by setting it with '0'. |
| `max_recursion_depth` | custom | Maximum recursion depth for recursive common table expression |
| `max_reordered_joins` | custom | The maximum number of joins to reorder as one group in cost-based join reordering |
| `max_tasks_waiting_for_execution_per_query` | integer | Maximum number of tasks waiting to be scheduled per query. Split enumeration is paused by the scheduler when this threshold is crossed |
| `max_tasks_waiting_for_node_per_query` | integer | Maximum possible number of tasks waiting for node allocation per query before scheduling of new tasks is paused |
| `max_unacknowledged_splits_per_task` | custom | Maximum number of leaf splits awaiting delivery to a given task |
| `max_writer_task_count` | integer | Maximum number of tasks that will participate in writing data |
| `merge_project_with_values` | boolean | Inline project expressions into values |
| `min_hash_partition_count` | integer | Minimum number of partitions for distributed joins and aggregations |
| `min_hash_partition_count_for_write` | integer | Minimum number of partitions for distributed joins and aggregations in write queries |
| `min_input_rows_per_task` | long | Minimum input rows required per task. This will help optimizer determine hash partition count for joins and aggregations |
| `min_input_size_per_task` | dataSize | Minimum input data size required per task. This will help optimizer determine hash partition count for joins and aggregations |
| `non_estimatable_predicate_approximation_enabled` | boolean | Approximate the cost of filters which cannot be accurately estimated even with complete statistics |
| `optimize_duplicate_insensitive_joins` | boolean | Optimize duplicate insensitive joins |
| `optimize_metadata_queries` | boolean | Enable optimization for metadata queries |
| `optimize_top_n_ranking` | boolean | Use top N ranking optimization |
| `output_pages_validation_enabled` | boolean | Runtime validation of blocks in source pages |
| `page_partitioning_buffer_pool_size` | integer | Maximum number of free buffers in the per task partitioned page buffer pool. Setting this to zero effectively disables the pool |
| `pre_aggregate_case_aggregations_enabled` | boolean | Pre-aggregate rows before GROUP BY with multiple CASE aggregations on same column |
| `predicate_pushdown_use_table_properties` | boolean | Use table properties in predicate pushdown |
| `prefer_partial_aggregation` | boolean | Prefer splitting aggregations into partial and final stages |
| `push_aggregation_through_outer_join` | boolean | Allow pushing aggregations below joins |
| `push_filter_into_values_max_row_count` | integer | Maximum number of rows in values for which filter is pushed down into values |
| `push_partial_aggregation_through_join` | boolean | Push partial aggregations below joins |
| `push_table_write_through_union` | boolean | Parallelize writes when using UNION ALL in queries that write data |
| `query_max_cpu_time` | duration | Maximum CPU time of a query |
| `query_max_execution_time` | duration | Maximum execution time of a query |
| `query_max_memory` | dataSize | Maximum amount of distributed memory a query can use |
| `query_max_memory_per_node` | dataSize | Maximum amount of memory a query can use per node |
| `query_max_planning_time` | duration | Maximum planning time of a query |
| `query_max_run_time` | duration | Maximum run time of a query (includes the queueing time) |
| `query_max_scan_physical_bytes` | dataSize | Maximum scan physical bytes of a query |
| `query_max_stage_count` | integer | Temporary: Maximum number of stages a query can have |
| `query_max_total_memory` | dataSize | Maximum amount of distributed total memory a query can use |
| `query_max_write_physical_size` | dataSize | Maximum write physical size of a query |
| `query_priority` | integer | The priority of queries. Larger numbers are higher priority |
| `query_retry_attempts` | integer | Maximum number of query retry attempts |
| `redistribute_writes` | boolean | Force parallel distributed writes |
| `remote_task_adaptive_update_request_size_enabled` | boolean | Experimental: Enable adaptive adjustment for size of remote task update request |
| `remote_task_guaranteed_splits_per_request` | integer | Guaranteed splits per remote task request |
| `remote_task_max_request_size` | dataSize | Experimental: Max size of remote task update request |
| `remote_task_request_size_headroom` | dataSize | Experimental: Headroom for size of remote task update request |
| `required_workers_count` | integer | Minimum number of active workers that must be available before the query will start |
| `required_workers_max_wait_time` | duration | Maximum time to wait for minimum number of workers before the query is failed |
| `resource_overcommit` | boolean | Use resources which are not guaranteed to be available to the query |
| `retry_delay_scale_factor` | double | Maximum delay before initiating a retry attempt. Delay increases exponentially for each subsequent attempt starting from 'retry_initial_delay' |
| `retry_initial_delay` | duration | Initial delay before initiating a retry attempt. Delay increases exponentially for each subsequent attempt up to 'retry_max_delay' |
| `retry_max_delay` | duration | Maximum delay before initiating a retry attempt. Delay increases exponentially for each subsequent attempt starting from 'retry_initial_delay' |
| `retry_policy` | enum | Retry policy |
| `rewrite_filtering_semi_join_to_inner_join` | boolean | Rewrite semi join in filtering context to inner join |
| `scale_writers` | boolean | Scale out writers based on throughput (use minimum necessary) |
| `skewed_partition_min_data_processed_rebalance_threshold` | dataSize | Minimum data processed to trigger skewed partition rebalancing in local and remote exchange |
| `skip_redundant_sort` | boolean | Skip redundant sort operations |
| `spatial_join` | boolean | Use spatial index for spatial join when possible |
| `spatial_partitioning_table_name` | string | Name of the table containing spatial partitioning scheme |
| `spill_enabled` | boolean | Enable spilling |
| `split_concurrency_adjustment_interval` | duration | Experimental: Interval between changes to the number of concurrent splits per node |
| `spooling_enabled` | boolean | Enable client spooling protocol |
| `spooling_unsupported_warning` | boolean | Generate warning when client lacks support for spooling protocol |
| `statistics_precalculation_for_pushdown_enabled` | boolean | Enable statistics precalculation for pushdown |
| `table_scan_node_partitioning_min_bucket_to_task_ratio` | double | Min table scan bucket to task ratio for which plan will be adopted to node pre-partitioned tables |
| `task_concurrency` | integer | Default number of local parallel jobs per worker |
| `task_max_writer_count` | integer | Maximum number of local parallel table writers per task when either task writer scaling or preferred partitioning is used |
| `task_min_writer_count` | integer | Minimum number of local parallel table writers per task when preferred partitioning and task writer scaling are not used |
| `task_retry_attempts_per_task` | integer | Maximum number of task retry attempts per single task |
| `task_scale_writers_enabled` | boolean | Scale the number of concurrent table writers per task based on throughput |
| `task_scale_writers_max_writer_memory_percentage` | double | Maximum percentage of memory per node that can be used by concurrent writers within a task before stopping writer scaling |
| `task_share_index_loading` | boolean | Share index join lookups and caching within a task |
| `time_zone_id` | string | Time Zone Id for the current session |
| `use_cost_based_partitioning` | boolean | When enabled the cost based optimizer is used to determine if repartitioning the output of an already partitioned stage is necessary |
| `use_exact_partitioning` | boolean | When enabled this forces data repartitioning unless the partitioning of upstream stage matches exactly what downstream stage expects |
| `use_legacy_window_filter_pushdown` | boolean | Use legacy window filter pushdown optimizer |
| `use_partial_distinct_limit` | boolean | Use partial Distinct Limit |
| `use_partial_topn` | boolean | Use partial TopN |
| `use_preferred_write_partitioning` | boolean | Use preferred write partitioning |
| `use_table_scan_node_partitioning` | boolean | Adapt plan to node pre-partitioned tables |
| `writer_scaling_min_data_processed` | dataSize | Minimum amount of uncompressed output data processed by writers before writer scaling can happen |

### SpoolingSessionProperties

Source: `core/trino-main/src/main/java/io/trino/server/protocol/spooling/SpoolingSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `spooling_initial_segment_size` | dataSize | Initial size of a spooled segment |
| `spooling_inlining_enabled` | boolean | Allow inlining initial rows |
| `spooling_inlining_max_rows` | long | Maximum number of rows that are allowed to be inlined per worker |
| `spooling_inlining_max_size` | dataSize | Maximum size of inlined data |
| `spooling_max_segment_size` | dataSize | Maximum size of a spooled segment |

## `plugin/trino-base-jdbc`

### DecimalSessionSessionProperties

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/DecimalSessionSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `decimal_default_scale` | integer | Default decimal scale for mapping unspecified and exceeding precision decimals. Not used when |
| `decimal_mapping` | enum | Decimal mapping for unspecified and exceeding precision decimals. STRICT skips them. MAP_TO_NUMBER maps them to Trino NUMBER. ALLOW_OVERFLOW requires setting proper decimal scale and rounding mode |
| `decimal_rounding_mode` | enum | Rounding mode for mapping unspecified and exceeding precision decimals. Not used when |

### JdbcDynamicFilteringSessionProperties

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcDynamicFilteringSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `dynamic_filtering_enabled` | boolean | Wait for dynamic filters before starting JDBC query |
| `dynamic_filtering_wait_timeout` | duration | Duration to wait for completion of dynamic filters |

### JdbcJoinPushdownSessionProperties

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcJoinPushdownSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `join_pushdown_automatic_max_join_to_tables_ratio` | double | If estimated join output size is greater than or equal to ratio * sum of table sizes, then join pushdown will not be performed |
| `join_pushdown_automatic_max_table_size` | dataSize | Maximum table size to be considered for join pushdown |
| `join_pushdown_strategy` | enum | Join pushdown strategy |

### JdbcMetadataSessionProperties

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcMetadataSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `aggregation_pushdown_enabled` | boolean | Enable aggregation pushdown |
| `bulk_list_columns` | boolean | Listing tables' columns in bulk |
| `complex_expression_pushdown` | boolean | Allow complex expression pushdown into connectors |
| `complex_join_pushdown_enabled` | boolean | Enable join pushdown with non-comparison expressions |
| `domain_compaction_threshold` | integer | Maximum ranges to allow in a tuple domain without simplifying it |
| `join_pushdown_enabled` | boolean | Enable join pushdown |
| `topn_pushdown_enabled` | boolean | Enable TopN pushdown |

### JdbcWriteSessionProperties

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/JdbcWriteSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `non_transactional_insert` | boolean | Enables support for non-transactional INSERT |
| `non_transactional_merge` | boolean | Enables support for non-transactional MERGE |
| `write_batch_size` | integer | Maximum number of rows to write in a single batch |
| `write_parallelism` | integer | Maximum number of parallel write tasks |

### TypeHandlingJdbcSessionProperties

Source: `plugin/trino-base-jdbc/src/main/java/io/trino/plugin/jdbc/TypeHandlingJdbcSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `unsupported_type_handling` | enum | Unsupported type handling strategy |

## `plugin/trino-bigquery`

### BigQuerySessionProperties

Source: `plugin/trino-bigquery/src/main/java/io/trino/plugin/bigquery/BigQuerySessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `create_disposition_type` | enum | Create disposition type |
| `max_parallelism` | integer | The max number of partitions to split the data into. |
| `projection_pushdown_enabled` | boolean | Dereference push down for STRUCT type |
| `query_results_cache_enabled` | boolean | Enable query results cache |
| `skip_view_materialization` | boolean | Skip materializing views |
| `view_materialization_with_filter` | boolean | Materialize views with filters |

## `plugin/trino-cassandra`

### CassandraSessionProperties

Source: `plugin/trino-cassandra/src/main/java/io/trino/plugin/cassandra/CassandraSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `splits_per_node` | long | Number of splits per node. By default, the values from the system.size_estimates table are used. |

## `plugin/trino-clickhouse`

### ClickHouseSessionProperties

Source: `plugin/trino-clickhouse/src/main/java/io/trino/plugin/clickhouse/ClickHouseSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `map_string_as_varchar` | boolean | Map ClickHouse String and FixedString as varchar instead of varbinary |

## `plugin/trino-delta-lake`

### DeltaLakeSessionProperties

Source: `plugin/trino-delta-lake/src/main/java/io/trino/plugin/deltalake/DeltaLakeSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `compression_codec` | enum | Compression codec to use when writing new data files |
| `dynamic_filtering_wait_timeout` | duration | Duration to wait for completion of dynamic filters during split generation |
| `extended_statistics_collect_on_write` | boolean | Enables automatic column level extended statistics collection on write |
| `extended_statistics_enabled` | boolean | Enable collection (ANALYZE) and use of extended statistics. |
| `hive_catalog_name` | string | Catalog to redirect to when a Hive table is referenced |
| `idle_writer_min_file_size` | dataSize | Minimum data written by a single partition writer before it can be consider as 'idle' and could be closed by the engine |
| `load_metadata_from_checksum_file` | boolean | Read table metadata and protocol from the Delta version checksum file when available, falling back to the transaction log |
| `max_split_size` | dataSize | Max split size |
| `parquet_ignore_statistics` | boolean | Ignore statistics from Parquet to allow querying files with corrupted or incorrect statistics |
| `parquet_max_read_block_row_count` | integer | Parquet: Maximum number of rows read in a batch |
| `parquet_max_read_block_size` | dataSize | Parquet: Maximum size of a block to read |
| `parquet_small_file_threshold` | dataSize | Parquet: Size below which a parquet file will be read entirely |
| `parquet_use_column_index` | boolean | Use Parquet column index |
| `parquet_vectorized_decoding_enabled` | boolean | Enable using Java Vector API for faster decoding of parquet files |
| `parquet_writer_block_size` | dataSize | Deprecated. Use parquet_writer_row_group_size instead. |
| `parquet_writer_page_size` | dataSize | Parquet: Writer page size |
| `parquet_writer_page_value_count` | integer | Parquet: Writer page row count |
| `parquet_writer_row_group_max_row_count` | integer | Parquet: The maximum row count of row groups created by the Parquet writer |
| `parquet_writer_row_group_size` | dataSize | Parquet: Writer row group size |
| `projection_pushdown_enabled` | boolean | Read only required fields from a row type |
| `query_partition_filter_required` | boolean | Require filter on partition column |
| `statistics_enabled` | boolean | Expose table statistics |
| `store_table_metadata` | boolean | Store table metadata in metastore |
| `target_max_file_size` | dataSize | Target maximum size of written files; the actual size may be larger |
| `timestamp_precision` | enum | Internal Delta Lake connector property |
| `vacuum_min_retention` | duration | Minimal retention period for vacuum procedure |

## `plugin/trino-hive`

### HiveSessionProperties

Source: `plugin/trino-hive/src/main/java/io/trino/plugin/hive/HiveSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `bucket_execution_enabled` | boolean | Enable bucket-aware execution: use physical bucketing information to optimize queries |
| `collect_column_statistics_on_write` | boolean | Enables automatic column level statistics collection on write |
| `compression_codec` | enum | Compression codec to use when writing files |
| `create_empty_bucket_files` | boolean | Create empty files for buckets that have no data |
| `delegate_transactional_managed_table_location_to_metastore` | boolean | When transactional managed table is created via Trino the location will not be set in request sent to HMS and location will be determined by metastore; if this property is set to true CREATE TABLE AS queries are not supported. |
| `delta_lake_catalog_name` | string | Catalog to redirect to when a Delta Lake table is referenced |
| `dynamic_filtering_wait_timeout` | duration | Duration to wait for completion of dynamic filters during split generation |
| `force_local_scheduling` | boolean | Only schedule splits on workers colocated with data node |
| `hive_storage_format` | enum | Default storage format for new tables or partitions |
| `hive_views_legacy_translation` | boolean | Use legacy Hive view translation mechanism |
| `hudi_catalog_name` | string | Catalog to redirect to when a Hudi table is referenced |
| `iceberg_catalog_name` | string | Catalog to redirect to when an Iceberg table is referenced |
| `idle_writer_min_file_size` | dataSize | Minimum data written by a single partition writer before it can be consider as 'idle' and could be closed by the engine |
| `ignore_absent_partitions` | boolean | Ignore partitions when the file system location does not exist rather than failing the query. |
| `ignore_corrupted_statistics` | boolean | Experimental: Ignore corrupted statistics rather than failing |
| `insert_existing_partitions_behavior` | custom | Behavior on insert existing partitions; this session property doesn't control behavior on insert existing unpartitioned table |
| `max_split_size` | dataSize | Max split size |
| `minimum_assigned_split_weight` | double | Minimum assigned split weight when size based split weighting is enabled |
| `non_transactional_optimize_enabled` | boolean | Enable OPTIMIZE table procedure |
| `optimize_mismatched_bucket_count` | boolean | Experimental: Enable optimization to avoid shuffle when bucket count is compatible but not the same |
| `orc_bloom_filters_enabled` | boolean | ORC: Enable bloom filters for predicate pushdown |
| `orc_lazy_read_small_ranges` | boolean | Experimental: ORC: Read small file segments lazily |
| `orc_max_buffer_size` | dataSize | ORC: Maximum size of a single read |
| `orc_max_merge_distance` | dataSize | ORC: Maximum size of gap between two reads to merge into a single read |
| `orc_max_read_block_size` | dataSize | ORC: Soft max size of Trino blocks produced by ORC reader |
| `orc_nested_lazy_enabled` | boolean | Experimental: ORC: Lazily read nested data |
| `orc_optimized_writer_max_dictionary_memory` | dataSize | ORC: Max dictionary memory |
| `orc_optimized_writer_max_stripe_rows` | integer | ORC: Max stripe row count |
| `orc_optimized_writer_max_stripe_size` | dataSize | ORC: Max stripe size |
| `orc_optimized_writer_min_stripe_size` | dataSize | ORC: Min stripe size |
| `orc_optimized_writer_validate` | boolean | ORC: Force all validation for files |
| `orc_optimized_writer_validate_mode` | enum | ORC: Level of detail in ORC validation |
| `orc_optimized_writer_validate_percentage` | custom | ORC: sample percentage for validation for files |
| `orc_stream_buffer_size` | dataSize | ORC: Size of buffer for streaming reads |
| `orc_string_statistics_limit` | dataSize | ORC: Maximum size of string statistics; drop if exceeding |
| `orc_tiny_stripe_threshold` | dataSize | ORC: Threshold below which an ORC stripe or file will read in its entirety |
| `orc_use_column_names` | boolean | ORC: Access ORC columns using names from the file |
| `parallel_partitioned_bucketed_writes` | boolean | Improve parallelism of partitioned and bucketed table writes |
| `parquet_ignore_statistics` | boolean | Ignore statistics from Parquet to allow querying files with corrupted or incorrect statistics |
| `parquet_max_read_block_row_count` | integer | Parquet: Maximum number of rows read in a batch |
| `parquet_max_read_block_size` | dataSize | Parquet: Maximum size of a block to read |
| `parquet_max_split_size` | dataSize | Max split size for Parquet files |
| `parquet_optimized_writer_validation_percentage` | custom | Parquet: sample percentage for validation of written files |
| `parquet_small_file_threshold` | dataSize | Parquet: Size below which a parquet file will be read entirely |
| `parquet_use_bloom_filter` | boolean | Use Parquet Bloom filters |
| `parquet_use_column_index` | boolean | Use Parquet column index |
| `parquet_use_column_names` | boolean | Parquet: Access Parquet columns using names from the file |
| `parquet_vectorized_decoding_enabled` | boolean | Enable using Java Vector API for faster decoding of parquet files |
| `parquet_writer_batch_size` | integer | Parquet: Maximum number of rows passed to the writer in each batch |
| `parquet_writer_block_size` | dataSize | Deprecated. Use parquet_writer_row_group_size instead. |
| `parquet_writer_page_size` | dataSize | Parquet: Writer page size |
| `parquet_writer_page_value_count` | integer | Parquet: Writer page row count |
| `parquet_writer_row_group_max_row_count` | integer | Parquet: The maximum row count of row groups created by the Parquet writer |
| `parquet_writer_row_group_size` | dataSize | Parquet: Writer row group size |
| `partition_statistics_sample_size` | integer | Maximum sample size of the partitions column statistics |
| `projection_pushdown_enabled` | boolean | Read only required fields from a row type |
| `propagate_table_scan_sorting_properties` | boolean | Use sorted table layout to generate more efficient execution plans. May lead to incorrect results if files are not sorted as per table definition. |
| `query_partition_filter_required` | boolean | Require filter on partition column |
| `query_partition_filter_required_schemas` | custom | List of schemas for which filter on partition column is enforced. |
| `rcfile_optimized_writer_validate` | boolean | RCFile: Validate writer files |
| `respect_table_format` | boolean | Write new partitions using table format rather than default storage format |
| `size_based_split_weights_enabled` | boolean | Enable estimating split weights based on size in bytes |
| `sorted_writing_enabled` | boolean | Enable writing to bucketed sorted tables |
| `statistics_enabled` | boolean | Expose table statistics |
| `target_max_file_size` | dataSize | Target maximum size of written files; the actual size may be larger |
| `timestamp_precision` | enum | Precision for timestamp columns in Hive tables |
| `validate_bucketing` | boolean | Verify that data is bucketed correctly when reading |

## `plugin/trino-hudi`

### HudiSessionProperties

Source: `plugin/trino-hudi/src/main/java/io/trino/plugin/hudi/HudiSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `columns_to_hide` | custom | List of column names that will be hidden |
| `ignore_absent_partitions` | boolean | Ignore absent partitions |
| `max_outstanding_splits` | integer | Maximum outstanding splits in a batch enqueued for processing |
| `max_splits_per_second` | integer | Rate at which splits are enqueued for processing. The queue will throttle if this rate limit is breached. |
| `minimum_assigned_split_weight` | double | Minimum assigned split weight when size-based split weights are enabled |
| `parquet_max_read_block_row_count` | integer | Parquet: Maximum number of rows read in a batch |
| `parquet_small_file_threshold` | dataSize | Parquet: Size below which a parquet file will be read entirely |
| `parquet_vectorized_decoding_enabled` | boolean | Enable using Java Vector API for faster decoding of parquet files |
| `query_partition_filter_required` | boolean | Require a filter on at least one partition column |
| `size_based_split_weights_enabled` | get |  |
| `split_generator_parallelism` | integer | Number of threads to generate splits from partitions |
| `standard_split_weight_size` | dataSize | The split size corresponding to the standard weight (1.0) when size-based split weights are enabled |
| `use_parquet_column_names` | boolean | Access parquet columns using names from the file. If disabled, then columns are accessed using index. |

## `plugin/trino-iceberg`

### IcebergSessionProperties

Source: `plugin/trino-iceberg/src/main/java/io/trino/plugin/iceberg/IcebergSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `bucket_execution_enabled` | boolean | Enable bucket-aware execution: use physical bucketing information to optimize queries |
| `collect_extended_statistics_on_write` | boolean | Collect extended statistics during writes |
| `dynamic_filtering_wait_timeout` | duration | Duration to wait for completion of dynamic filters during split generation |
| `expire_snapshots_min_retention` | duration | Minimal retention period for expire_snapshot procedure |
| `hive_catalog_name` | string | Catalog to redirect to when a Hive table is referenced |
| `idle_writer_min_file_size` | dataSize | Minimum data written by a single partition writer before it can be consider as 'idle' and could be closed by the engine |
| `incremental_refresh_enabled` | boolean | Enable Incremental refresh for MVs backed by Iceberg tables, when possible. |
| `max_partitions_per_writer` | integer | Maximum number of partitions per writer |
| `max_split_size` | dataSize | Max split size |
| `merge_manifests_on_write` | boolean | Compact manifest files when performing write operations |
| `minimum_assigned_split_weight` | double | Minimum assigned split weight |
| `orc_bloom_filters_enabled` | boolean | ORC: Enable bloom filters for predicate pushdown |
| `orc_lazy_read_small_ranges` | boolean | Experimental: ORC: Read small file segments lazily |
| `orc_max_buffer_size` | dataSize | ORC: Maximum size of a single read |
| `orc_max_merge_distance` | dataSize | ORC: Maximum size of gap between two reads to merge into a single read |
| `orc_max_read_block_size` | dataSize | ORC: Soft max size of Trino blocks produced by ORC reader |
| `orc_nested_lazy_enabled` | boolean | Experimental: ORC: Lazily read nested data |
| `orc_stream_buffer_size` | dataSize | ORC: Size of buffer for streaming reads |
| `orc_string_statistics_limit` | dataSize | ORC: Maximum size of string statistics; drop if exceeding |
| `orc_tiny_stripe_threshold` | dataSize | ORC: Threshold below which an ORC stripe or file will read in its entirety |
| `orc_writer_max_dictionary_memory` | dataSize | ORC: Max dictionary memory |
| `orc_writer_max_row_group_rows` | integer | ORC: Max number of rows in a row group |
| `orc_writer_max_stripe_rows` | integer | ORC: Max stripe row count |
| `orc_writer_max_stripe_size` | dataSize | ORC: Max stripe size |
| `orc_writer_min_stripe_size` | dataSize | ORC: Min stripe size |
| `orc_writer_validate_mode` | enum | ORC: Level of detail in ORC validation |
| `orc_writer_validate_percentage` | double | ORC: Percentage of written files to validate by re-reading them |
| `parquet_ignore_statistics` | boolean | Ignore statistics from Parquet to allow querying files with corrupted or incorrect statistics |
| `parquet_max_read_block_row_count` | integer | Parquet: Maximum number of rows read in a batch |
| `parquet_max_read_block_size` | dataSize | Parquet: Maximum size of a block to read |
| `parquet_small_file_threshold` | dataSize | Parquet: Size below which a parquet file will be read entirely |
| `parquet_use_bloom_filter` | boolean | Use Parquet Bloom filters |
| `parquet_use_column_index` | boolean | Use Parquet column index |
| `parquet_vectorized_decoding_enabled` | boolean | Enable using Java Vector API for faster decoding of parquet files |
| `parquet_writer_batch_size` | integer | Parquet: Maximum number of rows passed to the writer in each batch |
| `parquet_writer_delta_length_byte_array_encoding_enabled` | boolean | Parquet: Use DELTA_LENGTH_BYTE_ARRAY encoding for binary and string columns |
| `parquet_writer_page_size` | dataSize | Parquet: Writer page size |
| `parquet_writer_page_value_count` | integer | Parquet: Writer page row count |
| `parquet_writer_row_group_max_row_count` | integer | Parquet: The maximum row count of row groups created by the Parquet writer |
| `plaintext_files_allowed_for_encrypted_tables` | boolean | Allow reading unencrypted files in tables with encryption enabled |
| `projection_pushdown_enabled` | boolean | Read only required fields from a row type |
| `query_partition_filter_required` | boolean | Require filter on partition column |
| `query_partition_filter_required_schemas` | custom | List of schemas for which filter on partition column is enforced. |
| `remove_orphan_files_min_retention` | duration | Minimal retention period for remove_orphan_files procedure |
| `sorted_writing_enabled` | boolean | Enable sorted writing to tables with a specified sort order |
| `statistics_enabled` | boolean | Expose table statistics |
| `use_file_size_from_metadata` | boolean | Use file size stored in Iceberg metadata |

## `plugin/trino-kafka`

### KafkaSessionProperties

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/KafkaSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `timestamp_upper_bound_force_push_down_enabled` | boolean | Enable or disable timestamp upper bound push down for topic createTime mode |

### ConfluentSessionProperties

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/schema/confluent/ConfluentSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `empty_field_strategy` | enum | Strategy for handling struct types with no fields: IGNORE (default), FAIL, and MARK to add a boolean field named '$empty_field_marker' |

## `plugin/trino-mongodb`

### MongoSessionProperties

Source: `plugin/trino-mongodb/src/main/java/io/trino/plugin/mongodb/MongoSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `dynamic_filtering_wait_timeout` | duration | Duration to wait for completion of dynamic filters |
| `projection_pushdown_enabled` | boolean | Read only required fields from a row type |

## `plugin/trino-opensearch`

### OpenSearchSessionProperties

Source: `plugin/trino-opensearch/src/main/java/io/trino/plugin/opensearch/OpenSearchSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `projection_pushdown_enabled` | boolean | Read only required fields from a row type |

## `plugin/trino-oracle`

### OracleSessionProperties

Source: `plugin/trino-oracle/src/main/java/io/trino/plugin/oracle/OracleSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `number_default_scale` | integer | Deprecated. Default scale for Oracle Number data type |
| `number_rounding_mode` | enum | Deprecated. Rounding mode for Oracle NUMBER data type. You can set the value to 'UNNECESSARY' (old default) to disable mapping Oracle NUMBER to Trino NUMBER |

## `plugin/trino-pinot`

### PinotSessionProperties

Source: `plugin/trino-pinot/src/main/java/io/trino/plugin/pinot/PinotSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `aggregation_pushdown_enabled` | boolean | Enable aggregation pushdown |
| `connection_timeout` | duration | Connection Timeout to talk to Pinot servers |
| `count_distinct_pushdown_enabled` | boolean | Enable count distinct pushdown |
| `forbid_segment_queries` | boolean | Forbid segment queries |
| `non_aggregate_limit_for_broker_queries` | integer | Max limit for non aggregate queries to the pinot broker |
| `prefer_broker_queries` | boolean | Prefer queries to broker even when parallel scan is enabled for aggregation queries |
| `retry_count` | integer | Retry count for retriable pinot data fetch calls |
| `segments_per_split` | integer | Number of segments of the same host per split |

## `plugin/trino-postgresql`

### PostgreSqlSessionProperties

Source: `plugin/trino-postgresql/src/main/java/io/trino/plugin/postgresql/PostgreSqlSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `array_mapping` | enum | Handling of PostgreSql arrays |
| `enable_string_pushdown_with_collate` | boolean | Enable string pushdown with collate (experimental) |

## `plugin/trino-prometheus`

### PrometheusSessionProperties

Source: `plugin/trino-prometheus/src/main/java/io/trino/plugin/prometheus/PrometheusSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `max_query_range_duration` | duration | Width of overall query to Prometheus, will be divided into query_chunk_size_duration queries |
| `query_chunk_size_duration` | duration | The duration of each query to Prometheus |

## `plugin/trino-redshift`

### RedshiftSessionProperties

Source: `plugin/trino-redshift/src/main/java/io/trino/plugin/redshift/RedshiftSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `unload_enabled` | boolean | Use UNLOAD for reading query results |

## `plugin/trino-sqlserver`

### SqlServerSessionProperties

Source: `plugin/trino-sqlserver/src/main/java/io/trino/plugin/sqlserver/SqlServerSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `bulk_copy_for_write` | boolean | Use SQL Server Bulk Copy API for writes |
| `bulk_copy_for_write_lock_destination_table` | boolean | Obtain a Bulk Update lock on destination table on write |

## `plugin/trino-tpcds`

### TpcdsSessionProperties

Source: `plugin/trino-tpcds/src/main/java/io/trino/plugin/tpcds/TpcdsSessionProperties.java`

| Session property | Type | Description |
|---|---|---|
| `split_count` | integer | Number of split to be created. If not specified the number of splits is computed as 'splits_per_node * <number of active nodes>' |
| `splits_per_node` | integer | Number of splits created for each worker node |
| `with_no_sexism` | boolean | With no sexism |
