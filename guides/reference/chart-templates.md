# What the trino Helm chart renders

This page describes every template of the `trino` chart at version 1.45.0 (Trino 484). It was written by reading the chart templates. The templates are not copied into this repository.

Use it to answer two questions: "which Kubernetes object does this value create?" and "which line of which Trino file does this value write?".

## Names

`<fullname>` is the release name when the release is called `trino`. Otherwise it is `<release>-trino`. `fullnameOverride` replaces it. Names are cut at 63 characters.

| Object | Name |
|---|---|
| Coordinator Deployment, ConfigMap | `<fullname>-coordinator`, or `coordinatorNameOverride` |
| Worker Deployment, ConfigMap, HPA, ScaledObject, PodDisruptionBudget | `<fullname>-worker`, or `workerNameOverride` |
| Coordinator Service | `<fullname>` |
| Worker Service (headless, `clusterIP: None`) | `<fullname>-worker` |
| Catalog ConfigMap | `<fullname>-catalog` |
| Resource groups ConfigMap | `<fullname>-resource-groups-volume-coordinator` |
| Access control ConfigMaps | `<fullname>-access-control-volume-coordinator`, `<fullname>-access-control-volume-worker` |
| Kafka table description ConfigMaps | `<fullname>-schemas-volume-coordinator`, `<fullname>-schemas-volume-worker` |

Labels on every object: `app.kubernetes.io/name`, `app.kubernetes.io/instance`, `app.kubernetes.io/version`, `app.kubernetes.io/managed-by`, `helm.sh/chart`, plus `app.kubernetes.io/component` set to `coordinator`, `worker` or `catalogs`. `commonLabels` is merged over them.

## Templates

| Template | Kind | Created when |
|---|---|---|
| `deployment-coordinator.yaml` | Deployment | Always. One coordinator. |
| `deployment-worker.yaml` | Deployment | `server.workers` is greater than 0, or `server.keda.enabled` is true |
| `configmap-coordinator.yaml` | ConfigMap (up to 3) | Always. The resource groups ConfigMap is added when `resourceGroups.type` is `configmap`. The schemas ConfigMap is always added. |
| `configmap-worker.yaml` | ConfigMap (2) | Same condition as the worker Deployment |
| `configmap-catalog.yaml` | ConfigMap | `catalogs` or `additionalCatalogs` is not empty |
| `configmap-access-control-coordinator.yaml` | ConfigMap | Coordinator access control type is `configmap` |
| `configmap-access-control-worker.yaml` | ConfigMap | Worker access control type is `configmap` |
| `configmap-opa-policy-coordinator.yaml` | ConfigMap | `opa.enabled` is true and `opa.policy` has entries. Fails if `opa.policyConfigMap` is also set. |
| `configmap-jmx-exporter.yaml` | ConfigMap (per role) | `jmx.exporter.enabled` is true for that role |
| `secret.yaml` | Secret | `auth.passwordAuth` is set without `auth.passwordAuthSecret`. A second Secret holds `auth.groups` when set without `auth.groupsAuthSecret`. |
| `secret-servicemonitor.yaml` | Secret | A ServiceMonitor scrapes the OpenMetrics endpoint and `serviceMonitor.openMetrics.authSecret` is empty |
| `service-coordinator.yaml` | Service | Always |
| `service-worker.yaml` | Service (headless) | Always |
| `serviceaccount.yaml` | ServiceAccount | `serviceAccount.create` is true |
| `autoscaler.yaml` | HorizontalPodAutoscaler | `server.autoscaling.enabled` is true |
| `keda-scaledobject.yaml` | ScaledObject (`keda.sh/v1alpha1`) | `server.keda.enabled` is true. Fails if `server.autoscaling.enabled` is also true. |
| `poddisruptionbudget-worker.yaml` | PodDisruptionBudget | `worker.podDisruptionBudget` is set and workers exist |
| `ingress.yaml` | Ingress | `ingress.enabled` is true |
| `httproute.yaml` | HTTPRoute (Gateway API) | `gateway.enabled` is true |
| `networkpolicy.yaml` | NetworkPolicy | `networkPolicy.enabled` is true |
| `servicemonitor-coordinator.yaml` | ServiceMonitor | Coordinator ServiceMonitor is enabled |
| `servicemonitor-worker.yaml` | ServiceMonitor | Worker ServiceMonitor is enabled |
| `extra-manifests.yaml` | Anything | One object per entry of `extraManifests`. String entries are passed through `tpl`. |
| `tests/` | Pods | Only run by `helm test` |

## Role overrides

`jmx`, `serviceMonitor`, `accessControl` and `service` accept a `coordinator` block and a `worker` block. The chart copies the shared keys, then writes the role block over them. A key set in `jmx.worker` therefore wins over the same key in `jmx` for workers only.

## Files written into the coordinator ConfigMap

All files are mounted at `server.config.path`, which defaults to `/etc/trino`.

### node.properties

```text
node.environment=<server.node.environment>
node.data-dir=<server.node.dataDir>
plugin.dir=<server.node.pluginDir>
<each entry of additionalNodeProperties>
```

The chart does not set `node.id`. Trino generates one at startup.

### jvm.config

```text
-server
-agentpath:/usr/lib/trino/bin/libjvmkill.so
-Xmx<coordinator.jvm.maxHeapSize>                       only when set
-Xms<coordinator.jvm.minHeapSize>                       only when set
-XX:MaxRAMPercentage=<coordinator.jvm.maxHeapPercent>   only when set
-XX:InitialRAMPercentage=<coordinator.jvm.initialHeapPercent>   only when set
-XX:+<coordinator.jvm.gcMethod.type>
-XX:G1HeapRegionSize=<coordinator.jvm.gcMethod.g1.heapRegionSize>
-XX:+ExplicitGCInvokesConcurrent
-XX:+HeapDumpOnOutOfMemoryError
-XX:+ExitOnOutOfMemoryError
-XX:-OmitStackTraceInFastThrow
-XX:ReservedCodeCacheSize=512M
-XX:PerMethodRecompilationCutoff=10000
-XX:PerBytecodeRecompilationCutoff=10000
-Djdk.attach.allowAttachSelf=true
-Djdk.nio.maxCachedBufferSize=2000000
-XX:+EnableDynamicAgentLoading
<each entry of coordinator.additionalJVMConfig>
-Dcom.sun.management.jmxremote.rmi.port=<jmx.serverPort>   only when jmx is enabled for the role
```

For the stock `trinodb/trino` image with a version above 447 the chart also adds `-XX:+UnlockDiagnosticVMOptions` and `-XX:G1NumCollectionsKeepPinned=10000000`. It skips them when the image repository, registry or digest is overridden, so add them through `additionalJVMConfig` for a mirrored image if you want the same flags.

To size the heap as a share of the container memory, set `maxHeapSize` to an empty string and set `maxHeapPercent`. Setting both writes both flags, and `-Xmx` wins in the JVM.

### config.properties on the coordinator

```text
coordinator=true
node-scheduler.include-coordinator=<coordinator.config.nodeScheduler.includeCoordinator>
http-server.http.port=<service.port>
query.max-memory=<server.config.query.maxMemory>
query.max-memory-per-node=<coordinator.config.query.maxMemoryPerNode>
memory.heap-headroom-per-node=<coordinator.config.memory.heapHeadroomPerNode>   only when set
discovery.uri=http://localhost:<service.port>
http-server.authentication.type=<server.config.authenticationType>   only when set
<each entry of additionalConfigProperties>
http-server.https.enabled=true                                       only when server.config.https.enabled
http-server.https.port=<server.config.https.port>
http-server.https.keystore.path=<server.config.https.keystore.path>
jmx.rmiregistry.port=<jmx.registryPort>                              only when jmx is enabled
jmx.rmiserver.port=<jmx.serverPort>
shutdown.grace-period=<worker.gracefulShutdown.gracePeriodSeconds>s  only when graceful shutdown is enabled
<server.coordinatorExtraConfig, verbatim>
```

### config.properties on workers

```text
coordinator=false
http-server.http.port=<service.port>
query.max-memory=<server.config.query.maxMemory>
query.max-memory-per-node=<worker.config.query.maxMemoryPerNode>
memory.heap-headroom-per-node=<worker.config.memory.heapHeadroomPerNode>   only when set
discovery.uri=http://<fullname>:<service.port>
<each entry of additionalConfigProperties>
jmx.rmiregistry.port=<jmx.registryPort>                              only when jmx is enabled
jmx.rmiserver.port=<jmx.serverPort>
shutdown.grace-period=<worker.gracefulShutdown.gracePeriodSeconds>s  only when graceful shutdown is enabled
<server.workerExtraConfig, verbatim>
```

Things to notice:

- `additionalConfigProperties` goes to the coordinator and to every worker. Use `server.coordinatorExtraConfig` or `server.workerExtraConfig` for a key that belongs on one role only.
- Workers do not get `http-server.authentication.type` or the HTTPS keys from `server.config`. Add them through `server.workerExtraConfig` when workers need them, for example for internal TLS.
- The chart writes `query.max-memory`, `query.max-memory-per-node`, `http-server.http.port` and `discovery.uri` itself. Change them through the values shown above. Repeating one of these keys in `additionalConfigProperties` puts the key in the file twice.

### Other files

| File | Written when | Content |
|---|---|---|
| `log.properties` | Always | `io.trino=<server.log.trino.level>` plus `additionalLogProperties` |
| `exchange-manager.properties` | `server.exchangeManager` is not empty | `exchange-manager.name=<server.exchangeManager.name>`, `exchange.base-directories=<server.exchangeManager.baseDir joined with commas>`, then `additionalExchangeManagerProperties`. Written on coordinator and workers. |
| `access-control.properties` | Access control is configured, or `opa.enabled` | Type `configmap`: `access-control.name=file`, `security.refresh-period`, `security.config-file=<path>/access-control/<configFile or rules.json>`. Type `properties`: the `properties` text verbatim. OPA: `access-control.name=opa` and the `opa.policy.*` URIs pointing at `http://127.0.0.1:<opa.port>`. |
| `resource-groups.properties` | `resourceGroups` is set. Coordinator only. | Type `configmap`: `resource-groups.configuration-manager=file` and `resource-groups.config-file=<path>/resource-groups/resource-groups.json`. Type `properties`: verbatim. |
| `session-property-config.properties` and `session-property-config.json` | `sessionProperties` is set. Coordinator only. | Type `configmap`: the file manager pointing at `<path>/session-property-config.json`, and the JSON from `sessionProperties.sessionPropertiesConfig`. Type `properties`: verbatim. |
| `password-authenticator.properties` | `server.config.authenticationType` contains `PASSWORD` and the file is not given in `coordinator.additionalConfigFiles` | `password-authenticator.name=file`, `file.password-file=<path>/auth/password/password.db` |
| `header-authenticator.properties` | Authentication type contains `HEADER` | `headerAuthenticator.properties` verbatim. Rendering fails when it is missing. |
| `group-provider.properties` | `auth.groups` or `auth.groupsAuthSecret` is set | `group-provider.name=file`, `file.group-file=<path>/auth/group/group.db`, `file.refresh-period=<auth.refreshPeriod>` |
| `event-listener.properties` | `eventListenerProperties` has an entry that renders non-empty | The entries. Written on coordinator and workers. |
| Any name | Each key of `coordinator.additionalConfigFiles` or `worker.additionalConfigFiles` | The value, passed through `tpl` |

`additionalNodeProperties`, `additionalConfigProperties`, `additionalLogProperties`, `additionalExchangeManagerProperties`, `eventListenerProperties`, `catalogs` and the `additionalConfigFiles` values support Helm templating. An entry that renders to an empty string is left out.

### Catalog ConfigMap

One `<name>.properties` file per key of `catalogs` and `additionalCatalogs`, mounted at `<path>/catalog` on the coordinator and on every worker. `catalogs` values go through `tpl`. `additionalCatalogs` values are used verbatim. When both define the same name, the entry from `catalogs` wins.

The chart default for `catalogs` defines `tpch` and `tpcds`. Set those keys to `null` to remove them.

## Pods

### Volumes and mounts

| Mount path | Source |
|---|---|
| `<server.config.path>` | Role ConfigMap |
| `<server.config.path>/catalog` | Catalog ConfigMap |
| `<kafka.mountPath>` | Schemas ConfigMap |
| `<server.config.path>/access-control` | Access control ConfigMap |
| `<server.config.path>/resource-groups` | Resource groups ConfigMap (coordinator) |
| `<server.config.path>/auth/password`, `<server.config.path>/auth/group` | Password and group Secrets (coordinator) |
| Paths you choose | `configMounts`, `secretMounts`, `csiSecretMounts`, at top level for all nodes or under `coordinator` and `worker` for one role, and `additionalVolumes` with `additionalVolumeMounts` |

Because the ConfigMap is mounted over the whole of `/etc/trino`, the default files baked into the image are hidden. Only the files the chart renders exist.

### Rollouts

Pod templates carry `checksum/coordinator-config` or `checksum/worker-config`, and `checksum/catalog-config` when catalogs exist. A `helm upgrade` that changes rendered configuration therefore restarts the pods. The access control checksum is added only when no `refreshPeriod` is set, because Trino reloads the rules itself when one is.

`replicas` is written on the worker Deployment only when both autoscalers are off.

### Ports

| Port name | Value | On |
|---|---|---|
| `http` | `service.port`, default 8080 | Coordinator and workers |
| `https` | `server.config.https.port`, default 8443 | When HTTPS is enabled |
| `jmx-registry`, `jmx-server` | `jmx.registryPort` 9080, `jmx.serverPort` 9081 | When JMX is enabled |
| `jmx-exporter` | `jmx.exporter.port`, default 5556 | Exporter sidecar |
| Custom | `coordinator.additionalExposedPorts`, `worker.additionalExposedPorts` | As configured |

### Probes

| Probe | Check | Defaults |
|---|---|---|
| Liveness | HTTP GET `/v1/info` on `http` | initialDelay 30s, period 10s, timeout 5s, failureThreshold 6 |
| Readiness | Exec `/usr/lib/trino/bin/health-check` | initialDelay 10s, period 10s, timeout 5s, failureThreshold 6 |
| Startup | Exec `/usr/lib/trino/bin/health-check` | initialDelay 10s, period 2s, timeout 2s, failureThreshold 60 |

Override single fields under `coordinator.livenessProbe`, `coordinator.readinessProbe`, `coordinator.startupProbe` and the same keys under `worker`.

### Worker graceful shutdown

With `worker.gracefulShutdown.enabled: true` the chart:

1. Writes `shutdown.grace-period=<gracePeriodSeconds>s` on the coordinator and on workers.
2. Adds a `preStop` hook that sends `PUT /v1/info/state` with body `"SHUTTING_DOWN"` and header `X-Trino-User: <worker.gracefulShutdown.user>` to `http://localhost:<service.port>`, then waits for process 1 to exit.
3. Merges a `system_information` write rule for that user into the worker access control rules.

Rendering fails when:

- `worker.terminationGracePeriodSeconds` is less than two times `gracePeriodSeconds`.
- `worker.lifecycle` is also set.
- Worker access control is of type `properties`. The chart cannot merge the rule into opaque properties.

### Sidecars

- `jmx-exporter`, when `jmx.exporter.enabled` is true for the role.
- `opa`, on the coordinator pod, when `opa.enabled` is true.
- Anything in `sidecarContainers.coordinator` and `sidecarContainers.worker`. Init containers go in `initContainers.coordinator` and `initContainers.worker`.

## Render-time failures

`helm template` and `helm upgrade` stop with an error in these cases:

| Condition | Fix |
|---|---|
| `server.keda.enabled` and `server.autoscaling.enabled` are both true | Enable one autoscaler |
| `server.keda.enabled` is true and `server.keda.triggers` is empty | Add at least one trigger |
| Graceful shutdown is on and `worker.terminationGracePeriodSeconds` is under two times `gracePeriodSeconds` | Raise the termination grace period |
| Graceful shutdown is on and `worker.lifecycle` is set | Remove one of them |
| Graceful shutdown is on and worker access control has type `properties` | Use type `configmap` for workers, or disable graceful shutdown and wire it by hand |
| `accessControl`, `resourceGroups` or `sessionProperties` has a `type` other than `configmap` or `properties` | Fix the type |
| Type `properties` without a `properties` text | Add the text |
| Authentication type contains `HEADER` and `headerAuthenticator.properties` is missing | Add it |
| `opa.enabled` with an image tag below 438 | Use Trino 438 or later |
| `opa.policy` and `opa.policyConfigMap` both set, or neither set while `opa.enabled` | Set exactly one |
| `networkPolicy.enabled` with a `NodePort` coordinator Service | Use another Service type |

## Services

The coordinator Service has type `service.type` (default `ClusterIP`) and exposes `http`, `https` when enabled, `jmx-exporter` when enabled, and any additional exposed ports. `service.coordinator` overrides shared keys for the coordinator Service.

The worker Service is always headless. Workers register with the coordinator through `discovery.uri`, so nothing needs to load balance to them.

## Related pages

- [Helm values index](helm-values-index-trino.md)
- [Guide 03: from Helm values to Trino files](../03-helm-values-to-config-files.md)
- [Full chart README](../../upstream/charts/charts/trino/README.md)
