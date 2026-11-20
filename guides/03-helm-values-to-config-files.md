# 03. From Helm values to Trino files

The chart is a thin layer. Every Trino setting ends up as a line in a file under `/etc/trino`. This guide tells you which Helm value to use for a given Trino property. Chart 1.45.0.

The exhaustive template description is in [reference/chart-templates.md](reference/chart-templates.md). The value list is in [reference/helm-values-index-trino.md](reference/helm-values-index-trino.md).

## Decision table

Find the Trino file your property belongs in, then use the value in the right column.

| Trino file | Applies to | Helm value |
|---|---|---|
| `config.properties` | Coordinator and workers | `additionalConfigProperties` (list of `key=value` strings) |
| `config.properties` | Coordinator only | `server.coordinatorExtraConfig` (multi-line string) |
| `config.properties` | Workers only | `server.workerExtraConfig` (multi-line string) |
| `jvm.config` | Coordinator | `coordinator.jvm.*`, `coordinator.additionalJVMConfig` (list) |
| `jvm.config` | Workers | `worker.jvm.*`, `worker.additionalJVMConfig` (list) |
| `node.properties` | All | `server.node.*`, `additionalNodeProperties` (list) |
| `log.properties` | All | `server.log.trino.level`, `additionalLogProperties` (list) |
| `catalog/<name>.properties` | All | `catalogs.<name>` (multi-line string) |
| `exchange-manager.properties` | All | `server.exchangeManager.name`, `server.exchangeManager.baseDir`, `additionalExchangeManagerProperties` (list) |
| `resource-groups.properties` and its JSON | Coordinator | `resourceGroups` |
| `session-property-config.properties` and its JSON | Coordinator | `sessionProperties` |
| `access-control.properties` and rules | All, with role overrides | `accessControl`, or `opa` |
| `password-authenticator.properties`, password file | Coordinator | `server.config.authenticationType: PASSWORD` with `auth.passwordAuth` or `auth.passwordAuthSecret` |
| `group-provider.properties`, group file | Coordinator | `auth.groups` or `auth.groupsAuthSecret`, `auth.refreshPeriod` |
| `header-authenticator.properties` | Coordinator | `headerAuthenticator.properties` |
| `event-listener.properties` | All | `eventListenerProperties` (list) |
| Any other file in `/etc/trino` | One role | `coordinator.additionalConfigFiles.<filename>`, `worker.additionalConfigFiles.<filename>` |
| A file from an existing ConfigMap or Secret | All or one role | `configMounts`, `secretMounts`, `csiSecretMounts` at top level or under `coordinator` and `worker` |

## Properties the chart owns

These keys are written by the chart from dedicated values. Set them through the value, not through `additionalConfigProperties`.

| Property | Value | Chart default |
|---|---|---|
| `coordinator` | Fixed per role | |
| `node-scheduler.include-coordinator` | `coordinator.config.nodeScheduler.includeCoordinator` | `false` |
| `http-server.http.port` | `service.port` | `8080` |
| `query.max-memory` | `server.config.query.maxMemory` | `4GB` |
| `query.max-memory-per-node` | `coordinator.config.query.maxMemoryPerNode`, `worker.config.query.maxMemoryPerNode` | `1GB` |
| `memory.heap-headroom-per-node` | `coordinator.config.memory.heapHeadroomPerNode`, `worker.config.memory.heapHeadroomPerNode` | unset |
| `discovery.uri` | Derived from the Service name and port | |
| `http-server.authentication.type` | `server.config.authenticationType` (coordinator only) | unset |
| `http-server.https.enabled`, `http-server.https.port`, `http-server.https.keystore.path` | `server.config.https.*` (coordinator only) | disabled |
| `jmx.rmiregistry.port`, `jmx.rmiserver.port` | `jmx.registryPort`, `jmx.serverPort` when `jmx.enabled` | `9080`, `9081` |
| `shutdown.grace-period` | `worker.gracefulShutdown.gracePeriodSeconds` when enabled | `120` |
| `node.environment`, `node.data-dir`, `plugin.dir` | `server.node.environment`, `server.node.dataDir`, `server.node.pluginDir` | `production`, `/data/trino`, `/usr/lib/trino/plugin` |
| `-Xmx` | `coordinator.jvm.maxHeapSize`, `worker.jvm.maxHeapSize` | `8G` |

The chart defaults for memory are deliberately small. A production values file always sets `server.config.query.maxMemory`, both `maxMemoryPerNode` values and both heap sizes.

## Worked examples

### A property for every node

```yaml
additionalConfigProperties:
  - internal-communication.shared-secret=${ENV:TRINO_SHARED_SECRET}
  - query.max-execution-time=4h
  - retry-policy=QUERY
```

### A property for the coordinator only

```yaml
server:
  coordinatorExtraConfig: |
    query.max-queued-queries=2000
    web-ui.authentication.type=FORM
```

### A JVM flag

```yaml
worker:
  jvm:
    maxHeapSize: "48G"
  additionalJVMConfig:
    - -Djava.io.tmpdir=/data/trino/tmp
```

### Heap as a percentage of pod memory

```yaml
worker:
  jvm:
    maxHeapSize: ""
    maxHeapPercent: "75"
    initialHeapPercent: "75"
  resources:
    requests: {cpu: "15", memory: 64Gi}
    limits: {memory: 64Gi}
```

Clearing `maxHeapSize` matters. When it is set, the chart writes `-Xmx` and the percentage has no effect.

### A cache manager file plus the property that points at it

```yaml
additionalConfigProperties:
  - cache-manager.config-files=/etc/trino/cache-manager-alluxio.properties
coordinator:
  additionalConfigFiles:
    cache-manager-alluxio.properties: |
      cache-manager.name=alluxio
      fs.cache.directories=/data/trino/cache
      fs.cache.max-sizes=50GB
worker:
  additionalConfigFiles:
    cache-manager-alluxio.properties: |
      cache-manager.name=alluxio
      fs.cache.directories=/data/trino/cache
      fs.cache.max-sizes=200GB
```

### A secret value

Trino replaces `${ENV:NAME}` in any properties file at startup. Feed the variable from a Secret:

```yaml
env:
  - name: TRINO_SHARED_SECRET
    valueFrom:
      secretKeyRef:
        name: trino-internal
        key: shared-secret
```

`env` and `envFrom` apply to the coordinator and all workers.

### A file from a Secret

```yaml
coordinator:
  secretMounts:
    - name: tls-keystore
      secretName: trino-tls
      path: /etc/trino/tls
server:
  config:
    https:
      enabled: true
      port: 8443
      keystore:
        path: /etc/trino/tls/tls.pem
```

## Role overrides

`jmx`, `serviceMonitor`, `accessControl` and `service` take shared keys plus optional `coordinator` and `worker` blocks. Role keys override shared keys.

```yaml
jmx:
  enabled: true
  exporter:
    enabled: false
  coordinator:
    exporter:
      enabled: true
```

## Templating inside values

List entries and catalog texts pass through Helm `tpl`. You can reference other values or helpers:

```yaml
additionalConfigProperties:
  - 'http-server.log.enabled={{ .Values.custom.httpLog }}'
```

An entry that renders empty is dropped. Braces that must reach Trino untouched, such as `${ENV:NAME}`, are safe because they are not Go template syntax. Literal `{{` in a catalog needs escaping, or goes in `additionalCatalogs`, which is not templated.

## Common mistakes

| Mistake | Effect | Fix |
|---|---|---|
| `query.max-memory` added to `additionalConfigProperties` | Key appears twice in `config.properties` | Use `server.config.query.maxMemory` |
| Catalog property in `additionalConfigProperties` | Trino refuses to start: unused configuration property | Move it into `catalogs.<name>` |
| `config.properties` key in a catalog | Catalog fails to load | Move it to `additionalConfigProperties` |
| Session property name (underscores) in a properties file | Unused property error | Use the configuration property name, or a session property manager |
| Worker-only or coordinator-only key in `additionalConfigProperties` | Usually harmless, sometimes a startup error | Use `server.workerExtraConfig` or `server.coordinatorExtraConfig` |
| `maxHeapPercent` set while `maxHeapSize` keeps its default | Heap stays at 8G | Set `maxHeapSize: ""` |
| Exchange properties in `additionalConfigProperties` | Unused property error | Use `additionalExchangeManagerProperties` |

Trino fails fast on unknown properties. The error names the property and the file. Read the first `ERROR` block in the pod log.

## Verify what was rendered

```bash
kubectl -n trino get cm trino-coordinator -o jsonpath='{.data.config\.properties}'
kubectl -n trino get cm trino-worker -o jsonpath='{.data.jvm\.config}'
kubectl -n trino get cm trino-catalog -o yaml
kubectl -n trino exec deploy/trino-coordinator -- ls -R /etc/trino
```
