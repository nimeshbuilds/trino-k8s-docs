# The trinodb/trino container image

Facts about the official image for Trino 484, taken from the image build files in `core/docker` of the Trino repository. The default configuration files are copied in [`upstream/trino/core/docker/default/etc`](../../upstream/trino/core/docker/default/etc). The upstream usage notes are in [`upstream/trino/core/docker/README.md`](../../upstream/trino/core/docker/README.md).

## Layout

| Path | Content |
|---|---|
| `/usr/lib/trino` | Trino server installation |
| `/usr/lib/trino/bin/launcher` | Launcher. The image command is `/usr/lib/trino/bin/launcher run --etc-dir /etc/trino`. |
| `/usr/lib/trino/bin/health-check` | Health check script used by the image and by the Helm chart probes |
| `/usr/lib/trino/bin/libjvmkill.so` | JVM agent that kills the process on resource exhaustion. Loaded with `-agentpath`. |
| `/usr/lib/trino/plugin` | Plugins, one directory each |
| `/etc/trino` | Configuration directory |
| `/etc/trino/catalog` | Catalog properties files |
| `/data/trino` | Data directory (`node.data-dir`), logs included |
| `/usr/bin/trino` | Trino CLI |

## Runtime facts

- Runs as user `trino`, group `trino`.
- Exposes port 8080.
- Bundles the JDK. Trino 484 needs Java 25.
- The base image is a minimal hardened image. `curl`, `tar` and `less` are present. Common tools such as `grep` and `cut` are not, so keep `exec` scripts and lifecycle hooks to shell built-ins and `curl`.
- Environment variable `CATALOG_MANAGEMENT` defaults to `static` and feeds `catalog.management` in the default `config.properties`.
- Builds exist for `amd64` and `arm64`.

## Changes in release 484

From the 484 release notes: the image moved to a hardened minimal base that lacks utilities such as `grep`, `dnf` and `useradd`. The `run-trino` wrapper script was removed, so the launcher is the entry point and a custom `node.properties` must set `node.id=${ENV:HOSTNAME}` to keep using the container hostname as the node ID. The ppc64le image is no longer published.

## Health check

The script reads `/etc/trino/config.properties`, picks HTTP unless `http-server.http.enabled=false`, reads the port from `http-server.http.port` (default 8080) or `http-server.https.port` (default 8443), and calls `/v1/info` on `localhost`. It fails when the server does not answer, and it reports unhealthy while the server is still starting.

The script expects the configuration at `/etc/trino`. If you change `server.config.path` in the Helm chart, the health check still reads `/etc/trino/config.properties` and falls back to port 8080.

## Default configuration baked into the image

These files apply when you run the image without mounting anything over `/etc/trino`. The Helm chart mounts a ConfigMap over the whole directory, so none of them apply to a chart deployment.

`config.properties`:

```properties
coordinator=true
node-scheduler.include-coordinator=true
discovery.uri=http://localhost:8080
catalog.management=${ENV:CATALOG_MANAGEMENT}
cache-manager.config-files=etc/cache-manager-memory.properties
```

`node.properties`:

```properties
node.environment=docker
node.data-dir=/data/trino
node.id=${ENV:HOSTNAME}
```

`jvm.config`:

```text
-server
-agentpath:/usr/lib/trino/bin/libjvmkill.so
-XX:InitialRAMPercentage=80
-XX:MaxRAMPercentage=80
-XX:G1HeapRegionSize=32M
-XX:+ExplicitGCInvokesConcurrent
-XX:+HeapDumpOnOutOfMemoryError
-XX:+ExitOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-XX:ReservedCodeCacheSize=256M
-XX:PerMethodRecompilationCutoff=10000
-XX:PerBytecodeRecompilationCutoff=10000
-Djdk.attach.allowAttachSelf=true
-Djdk.nio.maxCachedBufferSize=2000000
```

`cache-manager-memory.properties`:

```properties
cache-manager.name=memory
fs.memory-cache.max-size=2GB
fs.memory-cache.ttl=24h
```

Default catalogs: `jmx`, `memory`, `tpcds` (`tpcds.splits-per-node=4`) and `tpch` (`tpch.splits-per-node=4`).

## Differences between the image defaults and the Helm chart

| Topic | Image default | Helm chart 1.45.0 default |
|---|---|---|
| Heap | 80 percent of container memory | Fixed `-Xmx8G` on coordinator and workers |
| Code cache | 256M | 512M |
| Topology | Single node, coordinator also runs queries | One coordinator that does not run queries, two workers |
| `node.environment` | `docker` | `production` |
| `node.id` | Pod hostname | Not set, generated |
| Catalogs | `jmx`, `memory`, `tpcds`, `tpch` | `tpcds`, `tpch` |
| Cache manager file | `memory` manager with 2GB and 24h | None. Trino still loads the `memory` manager with its own defaults. |

## Custom images

Build your own image when you need extra plugins, JDBC drivers or CA certificates. Start `FROM trinodb/trino:<version>`, copy files into `/usr/lib/trino/plugin/<name>/` or the trust store, and keep the `trino` user. Point the chart at it with `image.registry`, `image.repository` and `image.tag`, or with `image.useRepositoryAsSoleImageReference` and a full reference in `image.repository`.

The chart skips its version-dependent JVM flags for a non-default image. See [chart-templates.md](chart-templates.md#jvmconfig).
