# Helm values index: trino-gateway chart

Every value of the chart at version 1.22.0: 64 keys in 32 top-level groups.

This is a compact index generated from the chart README. The README has the full descriptions and the YAML examples: [`upstream/charts/charts/gateway/README.md`](../../upstream/charts/charts/gateway/README.md).

The Description column holds the first paragraph only. "Example" means the README shows a YAML example for that key.

## Groups

[`replicaCount`](#replicacount) (1), [`image`](#image) (3), [`imagePullSecrets`](#imagepullsecrets) (1), [`envFrom`](#envfrom) (1), [`initContainers`](#initcontainers) (1), [`config`](#config) (7), [`command`](#command) (1), [`service`](#service) (1), [`serviceName`](#servicename) (1), [`ingress`](#ingress) (5), [`resources`](#resources) (4), [`autoscaling`](#autoscaling) (5), [`livenessProbe`](#livenessprobe) (5), [`readinessProbe`](#readinessprobe) (5), [`volumes`](#volumes) (1), [`volumeMounts`](#volumemounts) (1), [`lifecycle`](#lifecycle) (1), [`terminationGracePeriodSeconds`](#terminationgraceperiodseconds) (1), [`nodeSelector`](#nodeselector) (1), [`tolerations`](#tolerations) (1), [`topologySpreadConstraints`](#topologyspreadconstraints) (1), [`affinity`](#affinity) (1), [`commonLabels`](#commonlabels) (1), [`podAnnotations`](#podannotations) (1), [`podLabels`](#podlabels) (1), [`podDisruptionBudget`](#poddisruptionbudget) (1), [`podSecurityContext`](#podsecuritycontext) (1), [`securityContext`](#securitycontext) (1), [`serviceAccount`](#serviceaccount) (4), [`strategy`](#strategy) (1), [`serviceMonitor`](#servicemonitor) (3), [`extraManifests`](#extramanifests) (1)

## replicaCount

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `replicaCount` | int | `1` |  |  |

## image

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `image.repository` | string | `"trinodb/trino-gateway"` | Repository location of the Trino Gateway image, typically `organization/imagename` |  |
| `image.pullPolicy` | string | `"IfNotPresent"` |  |  |
| `image.tag` | string | `""` | Override the image tag whose default is the chart appVersion. |  |

## imagePullSecrets

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `imagePullSecrets` | list | `[]` | An optional list of references to secrets in the same namespace to use for pulling images. Example: | Example |

## envFrom

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `envFrom` | list | `[]` | A list of secrets and configmaps to mount into the init container as environment variables. Example: | Example |

## initContainers

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `initContainers` | object | `{}` | Additional [containers that run to completion](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) during pod initialization. Example: | Example |

## config

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `config.serverConfig."node.environment"` | string | `"test"` |  |  |
| `config.serverConfig."http-server.http.enabled"` | bool | `true` |  |  |
| `config.dataStore.jdbcUrl` | string | `"jdbc:postgresql://localhost:5432/gateway"` | The connection details for the backend database for Trino Gateway and Trino query history |  |
| `config.dataStore.user` | string | `"postgres"` |  |  |
| `config.dataStore.password` | string | `"mysecretpassword"` |  |  |
| `config.dataStore.driver` | string | `"org.postgresql.Driver"` |  |  |
| `config.clusterStatsConfiguration.monitorType` | string | `"INFO_API"` |  |  |

## command

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `command` | list | `["java","-XX:MinRAMPercentage=80.0","-XX:MaxRAMPercentage=80.0","-jar","/usr/lib/trino...` | Startup command for Trino Gateway process. Add additional Java options and other modifications as desired. |  |

## service

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `service` | object | `{"annotations":{},"ports":[{"name":"gateway","protocol":"TCP"}],"type":"ClusterIP"}` | Service for accessing the gateway. The contents of this dictionary are used for the [service spec](https://kubernetes.io/docs/concepts/services-networking/service/#type-nodeport). The `port` and `targetPort` of the first element of the ports list will automatically be set to the value of `config.serverConfig."http-server.http[s].port"`. If both https and http ports are defined the https port is used. In this case, an additional service for the http port must be configured manually. Additional ports, such as for JMX or a Java Agent can be configured by adding elements to the ports list. The selector is also automatically configured. All other values are passed through as is. Example configuration for exposing both https and http: | Example |

## serviceName

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `serviceName` | string | `"trino-gateway"` | Set a custom name for the gateway service |  |

## ingress

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `ingress.enabled` | bool | `false` |  |  |
| `ingress.className` | string | `""` |  |  |
| `ingress.annotations` | object | `{}` |  |  |
| `ingress.hosts` | list | `[{"host":"chart-example.local","paths":[{"path":"/","pathType":"ImplementationSpecific...` | [Ingress rules](https://kubernetes.io/docs/concepts/services-networking/ingress/#ingress-rules). Example: | Example |
| `ingress.tls` | list | `[]` | Ingress [TLS](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) configuration. Example: | Example |

## resources

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `resources.limits.cpu` | int | `2` |  |  |
| `resources.limits.memory` | string | `"4Gi"` |  |  |
| `resources.requests.cpu` | int | `2` |  |  |
| `resources.requests.memory` | string | `"4Gi"` |  |  |

## autoscaling

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `autoscaling.enabled` | bool | `false` |  |  |
| `autoscaling.minReplicas` | int | `1` |  |  |
| `autoscaling.maxReplicas` | int | `100` |  |  |
| `autoscaling.targetCPUUtilizationPercentage` | int | `80` | Target average CPU utilization, represented as a percentage of requested CPU. To disable scaling based on CPU, set to an empty string. |  |
| `autoscaling.targetMemoryUtilizationPercentage` | string | `""` | Target average memory utilization, represented as a percentage of requested memory. To disable scaling based on memory, set to an empty string. |  |

## livenessProbe

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `livenessProbe.initialDelaySeconds` | int | `30` |  |  |
| `livenessProbe.periodSeconds` | int | `10` |  |  |
| `livenessProbe.failureThreshold` | int | `3` |  |  |
| `livenessProbe.timeoutSeconds` | int | `1` |  |  |
| `livenessProbe.scheme` | string | `"HTTP"` |  |  |

## readinessProbe

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `readinessProbe.initialDelaySeconds` | int | `5` |  |  |
| `readinessProbe.periodSeconds` | int | `5` |  |  |
| `readinessProbe.failureThreshold` | int | `12` |  |  |
| `readinessProbe.timeoutSeconds` | int | `1` |  |  |
| `readinessProbe.scheme` | string | `"HTTP"` |  |  |

## volumes

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `volumes` | list | `[]` |  |  |

## volumeMounts

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `volumeMounts` | list | `[]` |  |  |

## lifecycle

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `lifecycle` | object | `{}` | Container lifecycle events. Example: | Example |

## terminationGracePeriodSeconds

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `terminationGracePeriodSeconds` | int | `30` |  |  |

## nodeSelector

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `nodeSelector` | object | `{}` |  |  |

## tolerations

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `tolerations` | list | `[]` |  |  |

## topologySpreadConstraints

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `topologySpreadConstraints` | list | `[]` | [Deployment Topology Spread Constraints](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/) configuration. Useful to control how Pods are spread across domains such as regions, zones, nodes etc. Example: | Example |

## affinity

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `affinity` | object | `{}` |  |  |

## commonLabels

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `commonLabels` | object | `{}` | Labels that get applied to every resource's metadata |  |

## podAnnotations

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `podAnnotations` | object | `{}` | Annotations to add to the Gateway pods. By default, all pods will have the `checksum/trino-gateway-config` annotation with the checksum of the current configuration file. This is used to trigger a rolling update of the deployment when the configuration changes. This behaviour can be disabled by manually setting this annotation to a fixed constant. Example: | Example |

## podLabels

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `podLabels` | object | `{}` |  |  |

## podDisruptionBudget

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `podDisruptionBudget` | object | `{"minAvailable":1}` | [Pod Disruption Budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/#pod-disruption-budgets) configuration. |  |

## podSecurityContext

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `podSecurityContext` | object | `{}` | [Pod security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod) configuration. To remove the default, set it to null (or `~`). |  |

## securityContext

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `securityContext` | object | `{}` | [Container security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container) configuration. Example: | Example |

## serviceAccount

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `serviceAccount.create` | bool | `true` | Specifies whether a service account should be created |  |
| `serviceAccount.automount` | bool | `true` | Automatically mount a ServiceAccount's API credentials? |  |
| `serviceAccount.annotations` | object | `{}` | Annotations to add to the service account |  |
| `serviceAccount.name` | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |  |

## strategy

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `strategy` | object | `{"rollingUpdate":{"maxSurge":"25%","maxUnavailable":"25%"},"type":"RollingUpdate"}` | The deployment strategy to use to replace existing pods with new ones. |  |

## serviceMonitor

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `serviceMonitor.enabled` | bool | `false` | Set to true to create resources for the [prometheus-operator](https://github.com/prometheus-operator/prometheus-operator). |  |
| `serviceMonitor.labels` | object | `{"prometheus":"kube-prometheus"}` | Labels for serviceMonitor, so that Prometheus can select it |  |
| `serviceMonitor.interval` | string | `"30s"` | The serviceMonitor web endpoint interval |  |

## extraManifests

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `extraManifests` | list | `[]` | Array of extra kubernetes manifests to deploy with the release. Strings are evaluated as a template and can use template expansions and functions. All other objects are used as yaml. |  |
