# 07. Caching

The caches Trino 484 offers for a data lake, what each one saves, and how to configure them on Kubernetes without local SSDs or with them.

## Overview

| Cache | Saves | Lives in | Default |
|---|---|---|---|
| Metastore cache | Glue or Hive Metastore API calls | Coordinator heap, per catalog | Off (`hive.metastore-cache-ttl=0s`) |
| Statistics cache | Statistics lookups | Coordinator heap | `5m` |
| Directory listing cache (Hive) | S3 `LIST` calls | Coordinator heap | Off |
| Iceberg metadata file cache | Reads of manifests and metadata JSON | Heap, through the `memory` cache manager | On |
| File system cache for table data | S3 `GET` of data files | Local disk on workers, through the `alluxio` cache manager | Off |

None of these cache query results. Trino has no result cache.

## Metastore cache

Set in the catalog file of Hive, Delta Lake, Hudi or Lakehouse catalogs, and in Iceberg catalogs that use a Hive metastore:

```properties
hive.metastore-cache-ttl=5m
hive.metastore-refresh-interval=1m
hive.metastore-cache-maximum-size=20000
```

| Property | Default | Meaning |
|---|---|---|
| `hive.metastore-cache-ttl` | `0s` | How long entries are served. `0s` disables the cache. |
| `hive.metastore-refresh-interval` | unset | Refresh entries in the background after this age, so hot entries do not expire under load |
| `hive.metastore-cache-maximum-size` | `20000` | Entries |
| `hive.metastore-stats-cache-ttl` | `5m` | Statistics entries |
| `hive.metastore-cache.cache-partitions` | `true` | Cache partition metadata too |
| `hive.metastore-cache.cache-missing` | `true` | Remember that a table does not exist |
| `hive.user-metastore-cache-ttl` | `0s` | Per-user cache, for impersonation setups |

Trade-off: tables and partitions created outside Trino show up after the TTL, not at once. Writes through Trino invalidate the entries they touch. Flush by hand with `CALL <catalog>.system.flush_metadata_cache()`.

For Iceberg on Glue, `iceberg.glue.cache-table-metadata=true` is the default and covers the table metadata pointer.

## Directory listing cache

Hive connector only. Caches the list of files per partition directory.

```properties
hive.file-status-cache-tables=logs.*,ref.dim_accounts
hive.file-status-cache-expire-time=5m
hive.file-status-cache.max-retained-size=1GB
```

Only enable it for tables whose directories are immutable once written, or where a few minutes of staleness is acceptable. New files in a cached directory are invisible until the entry expires.

## File system cache

Caches the bytes of data files on worker-local storage. Useful when the same files are read again and again and the local disk is faster than the network path to S3. On a cluster in the same region as its buckets, with a gateway endpoint, the gain can be small. Measure before committing storage to it.

### What changed in recent releases

Cache settings moved out of catalog files. A catalog now only says `fs.cache.enabled=true`. Directories, sizes and TTL belong to a cache manager configured once per node. The upstream page has a migration section: [object-storage/file-system-cache.md](../upstream/trino/docs/object-storage/file-system-cache.md).

Old layout, no longer valid on 484:

```properties
# catalog file
fs.cache.enabled=true
fs.cache.directories=/data/cache
fs.cache.max-sizes=100GB
```

### Configuration on 484

1. A cache manager properties file on every node, for example `/etc/trino/cache-manager-alluxio.properties`:

   ```properties
   cache-manager.name=alluxio
   fs.cache.directories=/data/trino/cache
   fs.cache.max-sizes=200GB
   fs.cache.ttl=7d
   ```

2. In `config.properties` on every node:

   ```properties
   cache-manager.config-files=/etc/trino/cache-manager-alluxio.properties
   ```

3. In each catalog that should cache table data:

   ```properties
   fs.cache.enabled=true
   ```

| Property | Default | Meaning |
|---|---|---|
| `fs.cache.directories` | required | Absolute paths, comma separated. Must exist on the coordinator and all workers. |
| `fs.cache.max-sizes` | one of the two is required | Size per directory, same order |
| `fs.cache.max-disk-usage-percentages` | one of the two is required | Percent of the disk per directory, 1 to 100 |
| `fs.cache.ttl` | `7d` | Maximum age. `0s` effectively turns caching off. |
| `fs.cache.page-size` | `1MB` | Between `64kB` and `15MB` |
| `node-scheduler.cache-preferred-hosts-count` | `2` | Coordinator `config.properties`. How many workers are preferred for a given file. |

A catalog with `fs.cache.enabled=true` fails to start on a node that has no `alluxio` cache manager. That includes the coordinator, so the directory must exist there too, even though it stays almost empty.

### Helm values

```yaml
additionalConfigProperties:
  - cache-manager.config-files=/etc/trino/cache-manager-alluxio.properties

coordinator:
  additionalConfigFiles:
    cache-manager-alluxio.properties: |
      cache-manager.name=alluxio
      fs.cache.directories=/data/trino/cache
      fs.cache.max-sizes=10GB
  additionalVolumes:
    - name: cache
      emptyDir:
        sizeLimit: 12Gi
  additionalVolumeMounts:
    - name: cache
      mountPath: /data/trino/cache

worker:
  additionalConfigFiles:
    cache-manager-alluxio.properties: |
      cache-manager.name=alluxio
      fs.cache.directories=/data/trino/cache
      fs.cache.max-sizes=200GB
  additionalVolumes:
    - name: cache
      emptyDir: {}
  additionalVolumeMounts:
    - name: cache
      mountPath: /data/trino/cache

catalogs:
  hive: |
    connector.name=hive
    hive.metastore=glue
    fs.s3.enabled=true
    s3.region=us-east-1
    fs.cache.enabled=true
```

### Where the cache disk comes from

| Option | Notes |
|---|---|
| Instance store NVMe | Fastest. Needs the node to format and mount the disks, and `emptyDir` or a host path on that mount. Lost on node replacement, which is fine for a cache. |
| `emptyDir` on the node root volume | Works anywhere. Speed is that of the root EBS volume. Upstream advises against using the root disk. Give the node a large, fast volume if you go this way. |
| Generic ephemeral volume backed by EBS | A dedicated gp3 volume per pod with the IOPS and throughput you provision. No instance store needed. |
| `emptyDir` with `medium: Memory` | A RAM disk. Counts against the pod memory limit. |

The cache is optional. A cluster without local SSDs runs fine without it, and reads go to S3 every time.

### Cache and autoscaling

Each worker keeps its own cache. A new worker starts cold. A cluster that scales in and out often gets less from the cache than a steady one. Soft affinity sends splits for a file to the same few workers, so stable worker identity helps too.

## Memory cache manager

Trino loads the `memory` cache manager with defaults even when you configure nothing. The coordinator uses it for metadata files read during planning, for example Iceberg manifests.

| Property | Default |
|---|---|
| `fs.memory-cache.max-size` | 2 percent of the heap |
| `fs.memory-cache.max-content-length` | `8MB`, at most `15MB` |
| `fs.memory-cache.ttl` | `1h` |

To change it, add a second file with `cache-manager.name=memory` and list both files in `cache-manager.config-files`. A catalog that enables table data caching serves its metadata reads from `alluxio` instead.

The Iceberg connector also has `iceberg.metadata-cache.enabled`, default `true`.

## Monitoring

| Bean | Content |
|---|---|
| `io.trino.blob.cache.alluxio:type=AlluxioCacheStats,name=<catalog>` | External reads against cache reads per catalog |
| `org.alluxio:*` | Alluxio client metrics |
| `io.trino.blob.cache.memory:type=MemoryBlobCacheStats,name=<catalog>` | Memory cache per catalog |
| `io.trino.blob.cache.memory:name=MemoryBlobCache` | Shared memory cache |

Expose them through `openmetrics.jmx-object-names` or the JMX exporter. See guide [13](13-observability.md). Dashboards built for releases before the move use the old bean names and need updating.

## Choosing

| Situation | Do this |
|---|---|
| Interactive queries feel slow in planning, many partitions | Metastore cache, then listing cache for immutable tables |
| Glue throttling errors | Metastore cache with a refresh interval |
| Same hot files scanned repeatedly, network to S3 is the bottleneck | File system cache on fast local disk |
| Mostly one-off scans of cold data | No file cache. Spend on more workers. |
| Data changes outside Trino and readers need it at once | Keep TTLs short or caches off for those tables |
