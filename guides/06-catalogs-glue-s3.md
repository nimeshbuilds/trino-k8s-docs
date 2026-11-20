# 06. Catalogs for a data lake on S3 with AWS Glue

Configure Trino 484 to query Parquet, ORC, Iceberg, Delta Lake and Hudi tables in Amazon S3, with AWS Glue as the metastore, on EKS.

## Pick a connector

| Tables in Glue | Connector | `connector.name` |
|---|---|---|
| Hive-style tables (Parquet, ORC, JSON, CSV, Avro files in partition directories) | Hive | `hive` |
| Iceberg tables | Iceberg | `iceberg` |
| Delta Lake tables | Delta Lake | `delta_lake` |
| Hudi tables | Hudi | `hudi` |
| A mix, through one catalog | Lakehouse | `lakehouse` |

One Glue database can hold several table formats. Either create one catalog per connector and use table redirection, or use the Lakehouse connector, which combines the four.

Pages: [connector/hive.md](../upstream/trino/docs/connector/hive.md), [connector/iceberg.md](../upstream/trino/docs/connector/iceberg.md), [connector/delta-lake.md](../upstream/trino/docs/connector/delta-lake.md), [connector/hudi.md](../upstream/trino/docs/connector/hudi.md), [connector/lakehouse.md](../upstream/trino/docs/connector/lakehouse.md), [object-storage/metastores.md](../upstream/trino/docs/object-storage/metastores.md), [object-storage/file-system-s3.md](../upstream/trino/docs/object-storage/file-system-s3.md).

## A property name that changed

In Trino 484 the S3 switch is `fs.s3.enabled=true`. The older name `fs.native-s3.enabled` still works as a legacy name. Much third-party material, including the Data on EKS values copied in this repository, still uses the old name. Both are fine on 484. New files should use `fs.s3.enabled`.

No file system is enabled by default. A lake catalog without `fs.s3.enabled=true` fails when it first touches an `s3://` path.

## Hive catalog on Glue

```properties
connector.name=hive
hive.metastore=glue
hive.metastore.glue.region=us-east-1
fs.s3.enabled=true
s3.region=us-east-1
```

Useful additions for a read-mostly lake:

```properties
# Glue is a different account or catalog
hive.metastore.glue.catalogid=111122223333

# Metastore cache, off by default (ttl 0s)
hive.metastore-cache-ttl=5m
hive.metastore-refresh-interval=1m

# Directory listing cache for selected tables, off by default
hive.file-status-cache-tables=logs.*
hive.file-status-cache-expire-time=5m

# Guard rails
hive.max-partitions-per-scan=100000

# Allow INSERT and CREATE TABLE AS into external tables
hive.non-managed-table-writes-enabled=true
```

Facts from source and upstream docs:

| Property | Default | Meaning |
|---|---|---|
| `hive.metastore` | `thrift` | Set `glue` for AWS Glue |
| `hive.metastore.glue.region` | unset | Glue region. Falls back to the SDK region chain. |
| `hive.metastore.glue.catalogid` | unset | Glue catalog ID, an AWS account ID, for cross-account access |
| `hive.metastore.glue.iam-role` | unset | Role to assume for Glue calls |
| `hive.metastore.glue.max-connections` | `30` | HTTP connections to Glue |
| `hive.metastore.glue.max-error-retries` | `10` | |
| `hive.metastore.glue.partitions-segments` | `5` | Parallel segments for partition listing |
| `hive.metastore.glue.threads` | `40` | Threads for parallel Glue calls. In source, not in the docs page. |
| `hive.metastore.glue.default-warehouse-dir` | unset | Default location for new schemas |
| `hive.metastore.glue.skip-archive` | `false` | Skip archiving the old table version on update |
| `hive.metastore-cache-ttl` | `0s` | Cache for metastore reads |
| `hive.metastore-stats-cache-ttl` | `5m` | Cache for statistics |
| `hive.max-partitions-per-scan` | `1000000` | Fail queries that touch more partitions |
| `hive.max-partitions-for-eager-load` | `100000` | |
| `hive.max-split-size` | `64MB` | |
| `hive.parquet.use-column-names` | `true` | Match Parquet columns by name, not position |
| `hive.security` | `allow-all` | Connector-level authorization |

All Hive properties: [upstream page](../upstream/trino/docs/connector/hive.md) and [source-derived list](reference/config-properties/plugin--trino-hive.md). Glue and metastore cache properties: [lib/trino-metastore](reference/config-properties/lib--trino-metastore.md).

### Partitions

Trino reads partitions from Glue. Files that exist in S3 but have no partition entry in Glue are invisible. Options:

- Register them where they are written, or with a Glue crawler.
- `CALL <catalog>.system.sync_partition_metadata('<schema>', '<table>', 'ADD')`.
- Partition projection. Trino honours the Athena-style partition projection table properties while `hive.partition-projection-enabled` is `true`, which is the default.

After changing data outside Trino with caches on, flush:

```sql
CALL hive.system.flush_metadata_cache();
CALL hive.system.flush_metadata_cache(schema_name => 'logs', table_name => 'events');
```

## Iceberg catalog on Glue

```properties
connector.name=iceberg
iceberg.catalog.type=glue
hive.metastore.glue.region=us-east-1
fs.s3.enabled=true
s3.region=us-east-1
```

The Glue properties keep the `hive.metastore.glue.` prefix in the Iceberg connector. `iceberg.catalog.type` defaults to `hive_metastore`, so it must be set. `iceberg.glue.cache-table-metadata` defaults to `true`.

Routine maintenance runs through SQL:

```sql
ALTER TABLE iceberg.logs.events EXECUTE optimize(file_size_threshold => '128MB');
ALTER TABLE iceberg.logs.events EXECUTE expire_snapshots(retention_threshold => '7d');
ALTER TABLE iceberg.logs.events EXECUTE remove_orphan_files(retention_threshold => '7d');
```

All Iceberg properties: [upstream page](../upstream/trino/docs/connector/iceberg.md) and [source-derived list](reference/config-properties/plugin--trino-iceberg.md). Other Iceberg catalog types (REST, JDBC, Nessie, Snowflake) are in [object-storage/metastores.md](../upstream/trino/docs/object-storage/metastores.md).

## Lakehouse catalog

```properties
connector.name=lakehouse
hive.metastore=glue
hive.metastore.glue.region=us-east-1
fs.s3.enabled=true
s3.region=us-east-1
```

Do not set `iceberg.catalog.type` here. `hive.metastore` configures the Iceberg catalog as well. `lakehouse.table-type` picks the format for new tables.

## S3 file system

| Property | Default | Meaning |
|---|---|---|
| `fs.s3.enabled` | `false` | Turn on S3 support in this catalog |
| `s3.region` | unset | Bucket region. Set it. |
| `s3.endpoint` | unset | Custom or VPC endpoint URL |
| `s3.cross-region-access` | `false` | Allow buckets outside `s3.region` |
| `s3.path-style-access` | `false` | For S3-compatible stores |
| `s3.auth-type` | `DEFAULT` | `DEFAULT`, `IAM_ROLE`, `WEB_IDENTITY`, `ANONYMOUS` |
| `s3.iam-role` | unset | Role to assume. Needs `s3.auth-type=IAM_ROLE`. |
| `s3.external-id` | unset | External ID for the role trust policy |
| `s3.sse.type` | `NONE` | `S3`, `KMS`, `CUSTOMER` |
| `s3.sse.kms-key-id` | unset | KMS key for writes |
| `s3.max-connections` | `500` | Connection pool per catalog per node |
| `s3.max-error-retries` | `20` | In source. The docs page describes retry settings. |
| `s3.retry-mode` | `LEGACY` | `STANDARD` and `ADAPTIVE` also exist |
| `s3.streaming.part-size` | `32MB` | Multipart upload part size |
| `s3.requester-pays` | `false` | |
| `s3.storage-class` | `STANDARD` | For writes |

All S3 properties: [upstream page](../upstream/trino/docs/object-storage/file-system-s3.md) and [source-derived list](reference/config-properties/lib--trino-filesystem-s3.md).

## Credentials on EKS

Give the Trino ServiceAccount an IAM role. Do not put access keys in catalog files.

```yaml
serviceAccount:
  create: true
  name: trino
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::111122223333:role/trino-lake   # IRSA only
```

| Mechanism | Setup | Trino side |
|---|---|---|
| EKS Pod Identity | Pod Identity Agent add-on, plus a pod identity association for namespace and ServiceAccount. No annotation needed. | Leave `s3.auth-type` at `DEFAULT`. The AWS default credentials chain picks up the container credentials. |
| IAM roles for service accounts (IRSA) | OIDC provider, role trust policy on the ServiceAccount, annotation above | `DEFAULT` works through the chain. `s3.auth-type=WEB_IDENTITY` forces the web identity provider. For Glue, `hive.metastore.glue.use-web-identity-token-credentials-provider=true` does the same. |
| Assume another role | Base role as above, plus permission to assume the target | `s3.auth-type=IAM_ROLE` with `s3.iam-role`, and `hive.metastore.glue.iam-role` for Glue |
| Different roles per user, bucket or prefix | A mapping file | S3 security mapping, `s3.security-mapping.*`. See the S3 page. |

The same role is used by the coordinator and by every worker, because `serviceAccount` applies to both. The coordinator needs Glue access and S3 list and read for planning. Workers need S3 read, and write if you write tables.

### IAM policy shape

Read-only lake access needs, at minimum:

- S3: `s3:GetObject` on the data prefixes, `s3:ListBucket` and `s3:GetBucketLocation` on the buckets.
- Glue: `glue:GetDatabase`, `glue:GetDatabases`, `glue:GetTable`, `glue:GetTables`, `glue:GetPartition`, `glue:GetPartitions`, `glue:BatchGetPartition`, and the column statistics read actions `glue:GetColumnStatisticsForTable` and `glue:GetColumnStatisticsForPartition`.
- KMS: `kms:Decrypt` on the keys when buckets use SSE-KMS.

Writes add `s3:PutObject`, `s3:DeleteObject`, `s3:AbortMultipartUpload`, `s3:ListBucketMultipartUploads`, `s3:ListMultipartUploadParts`, the Glue create, update and delete actions for databases, tables and partitions, and `kms:GenerateDataKey`.

Treat these lists as a starting point and confirm against CloudTrail for your workload. With Lake Formation enabled on the Glue catalog, the role also needs Lake Formation grants. The Data on EKS stack attaches a broad managed Glue policy and a bucket-scoped S3 policy. See guide [16](16-eks-reference-stack.md).

## Catalogs in Helm values

```yaml
catalogs:
  tpch: null
  tpcds: null
  hive: |
    connector.name=hive
    hive.metastore=glue
    hive.metastore.glue.region=us-east-1
    fs.s3.enabled=true
    s3.region=us-east-1
  iceberg: |
    connector.name=iceberg
    iceberg.catalog.type=glue
    hive.metastore.glue.region=us-east-1
    fs.s3.enabled=true
    s3.region=us-east-1
```

Full example: [examples/values-eks-glue-s3.yaml](examples/values-eks-glue-s3.yaml).

## Table redirection between catalogs

When one Glue database holds Hive and Iceberg tables and you run separate catalogs, tell each where to send the other format:

```properties
# in the hive catalog
hive.iceberg-catalog-name=iceberg
# in the iceberg catalog
iceberg.hive-catalog-name=hive
```

Users can then query any table through either catalog. Redirection covers reads and most statements. Details are in the "Table redirection" sections of the Hive and Iceberg pages.

## Reading performance checklist

1. Partition and filter. Trino prunes partitions from predicates on partition columns. A query without one lists and plans every partition.
2. Keep statistics fresh. `ANALYZE` for Hive tables. Iceberg keeps file-level statistics itself. The cost-based optimizer picks join order and distribution from them.
3. Right-size files. Many small files cost listing time and task overhead. Aim for files of roughly 100MB to 1GB.
4. Let dynamic filtering work. It is on by default. `hive.dynamic-filtering.wait-timeout` defaults to `0s` and `iceberg.dynamic-filtering.wait-timeout` to `1s`. Raising them lets selective joins prune more of the probe side before the scan starts.
5. Turn on the metastore cache for interactive workloads, and the file system cache when the same files are read repeatedly. See guide [07](07-caching.md).
6. Keep Trino in the same region as the buckets, and use an S3 gateway VPC endpoint so traffic stays off NAT gateways.
7. Watch Glue API throttling on tables with very many partitions. Caching, `hive.metastore.glue.partitions-segments` and partition projection all reduce calls.

## Verify a catalog

```sql
SHOW CATALOGS;
SHOW SCHEMAS FROM hive;
SHOW TABLES FROM hive.logs;
SELECT * FROM hive.logs."events$partitions" LIMIT 10;
SELECT count(*) FROM hive.logs.events WHERE day = DATE '2026-01-01';
EXPLAIN SELECT count(*) FROM hive.logs.events WHERE day = DATE '2026-01-01';
```

A catalog that fails to load is missing from `SHOW CATALOGS` or errors on first use. The coordinator log has the reason. Common causes are in guide [17](17-troubleshooting.md#catalogs-and-storage).
