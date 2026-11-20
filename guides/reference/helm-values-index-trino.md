# Helm values index: trino chart

Every value of the chart at version 1.45.0: 222 keys in 43 top-level groups.

This is a compact index generated from the chart README. The README has the full descriptions and the YAML examples: [`upstream/charts/charts/trino/README.md`](../../upstream/charts/charts/trino/README.md).

The Description column holds the first paragraph only. "Example" means the README shows a YAML example for that key.

## Groups

[`nameOverride`](#nameoverride) (1), [`fullnameOverride`](#fullnameoverride) (1), [`coordinatorNameOverride`](#coordinatornameoverride) (1), [`workerNameOverride`](#workernameoverride) (1), [`image`](#image) (6), [`imagePullSecrets`](#imagepullsecrets) (1), [`testImages`](#testimages) (4), [`server`](#server) (27), [`accessControl`](#accesscontrol) (1), [`opa`](#opa) (29), [`headerAuthenticator`](#headerauthenticator) (1), [`resourceGroups`](#resourcegroups) (1), [`additionalNodeProperties`](#additionalnodeproperties) (1), [`additionalConfigProperties`](#additionalconfigproperties) (1), [`additionalLogProperties`](#additionallogproperties) (1), [`additionalExchangeManagerProperties`](#additionalexchangemanagerproperties) (1), [`sessionProperties`](#sessionproperties) (1), [`eventListenerProperties`](#eventlistenerproperties) (1), [`catalogs`](#catalogs) (1), [`additionalCatalogs`](#additionalcatalogs) (1), [`env`](#env) (1), [`envFrom`](#envfrom) (1), [`initContainers`](#initcontainers) (1), [`sidecarContainers`](#sidecarcontainers) (1), [`securityContext`](#securitycontext) (1), [`containerSecurityContext`](#containersecuritycontext) (3), [`shareProcessNamespace`](#shareprocessnamespace) (2), [`service`](#service) (8), [`auth`](#auth) (1), [`serviceAccount`](#serviceaccount) (3), [`configMounts`](#configmounts) (1), [`secretMounts`](#secretmounts) (1), [`csiSecretMounts`](#csisecretmounts) (1), [`coordinator`](#coordinator) (34), [`worker`](#worker) (36), [`kafka`](#kafka) (2), [`jmx`](#jmx) (14), [`serviceMonitor`](#servicemonitor) (14), [`commonLabels`](#commonlabels) (1), [`ingress`](#ingress) (5), [`gateway`](#gateway) (5), [`networkPolicy`](#networkpolicy) (3), [`extraManifests`](#extramanifests) (1)

## nameOverride

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `nameOverride` | string | `nil` | Override resource names to avoid name conflicts when deploying multiple releases in the same namespace. Example: | Example |

## fullnameOverride

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `fullnameOverride` | string | `nil` |  |  |

## coordinatorNameOverride

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `coordinatorNameOverride` | string | `nil` |  |  |

## workerNameOverride

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `workerNameOverride` | string | `nil` |  |  |

## image

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `image.registry` | string | `""` | Image registry, defaults to empty, which results in DockerHub usage |  |
| `image.repository` | string | `"trinodb/trino"` | Repository location of the Trino image, typically `organization/imagename` |  |
| `image.tag` | string | `""` | Image tag, defaults to the Trino release version specified as `appVersion` from Chart.yaml |  |
| `image.digest` | string | `""` | Optional digest value of the image specified as `sha256:abcd...`. A specified value overrides `tag`. |  |
| `image.useRepositoryAsSoleImageReference` | bool | `false` | When true, only the content in `repository` is used as image reference |  |
| `image.pullPolicy` | string | `"IfNotPresent"` |  |  |

## imagePullSecrets

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `imagePullSecrets` | list | `[]` | An optional list of references to secrets in the same namespace to use for pulling images. Example: | Example |

## testImages

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `testImages` | object | `{"kubectl":{"pullPolicy":"IfNotPresent","repository":"alpine/kubectl","tag":"1.37.1"},...` | Images of the containers the chart's [Helm tests](https://helm.sh/docs/topics/chart_tests/) run, so a cluster that pulls through a mirror can run them too. The registry prefix is shared with the Trino image and comes from `image.registry`. |  |
| `testImages.kubectl` | object | `{"pullPolicy":"IfNotPresent","repository":"alpine/kubectl","tag":"1.37.1"}` | Used by the graceful shutdown test to watch and delete a worker pod. It needs a shell, so a distroless image such as `registry.k8s.io/kubectl` does not work here. |  |
| `testImages.postgresql` | object | `{"pullPolicy":"IfNotPresent","repository":"postgres","tag":"18-alpine"}` | Used by the connection test to seed the resource groups database, so it is only pulled when `resourceGroups.type` is `properties`. |  |
| `testImages.python` | object | `{"pullPolicy":"IfNotPresent","repository":"python","tag":"3-slim"}` | Used by the JMX test to query the Prometheus targets the ServiceMonitor produced. |  |

## server

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `server.workers` | int | `2` |  |  |
| `server.node.environment` | string | `"production"` | Supports templating with `tpl`. |  |
| `server.node.dataDir` | string | `"/data/trino"` |  |  |
| `server.node.pluginDir` | string | `"/usr/lib/trino/plugin"` |  |  |
| `server.log.trino.level` | string | `"INFO"` |  |  |
| `server.config.path` | string | `"/etc/trino"` |  |  |
| `server.config.https.enabled` | bool | `false` |  |  |
| `server.config.https.port` | int | `8443` |  |  |
| `server.config.https.keystore.path` | string | `""` |  |  |
| `server.config.authenticationType` | string | `""` | Trino supports multiple [authentication types](https://trino.io/docs/current/security/authentication-types.html): PASSWORD, CERTIFICATE, OAUTH2, JWT, KERBEROS, HEADER. |  |
| `server.config.query.maxMemory` | string | `"4GB"` |  |  |
| `server.exchangeManager` | object | `{}` | Mandatory [exchange manager configuration](https://trino.io/docs/current/admin/fault-tolerant-execution.html#id1). Used to set the name and location(s) of spooling data storage. For multiple destinations use a list or a comma separated URI locations. To enable fault-tolerant execution, set the `retry-policy` property in `additionalConfigProperties`. Additional exchange manager configurations can be added to `additionalExchangeManagerProperties`. Example: | Example |
| `server.workerExtraConfig` | string | `""` |  |  |
| `server.coordinatorExtraConfig` | string | `""` |  |  |
| `server.autoscaling` | object | `{"behavior":{},"enabled":false,"maxReplicas":5,"targetCPUUtilizationPercentage":50,"ta...` | Configure [Horizontal Pod Autoscaling](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) for workers (`server.keda.enabled` must be `false`). |  |
| `server.autoscaling.targetCPUUtilizationPercentage` | int | `50` | Target average CPU utilization, represented as a percentage of requested CPU. To disable scaling based on CPU, set to an empty string. |  |
| `server.autoscaling.targetMemoryUtilizationPercentage` | int | `80` | Target average memory utilization, represented as a percentage of requested memory. To disable scaling based on memory, set to an empty string. |  |
| `server.autoscaling.behavior` | object | `{}` | Configuration for scaling up and down. Example: | Example |
| `server.keda` | object | `{"advanced":{},"annotations":{},"cooldownPeriod":300,"enabled":false,"fallback":{},"in...` | Configure [Kubernetes Event-driven Autoscaling](https://keda.sh/) for workers (`server.autoscaling.enabled` must be `false`). |  |
| `server.keda.cooldownPeriod` | int | `300` | Period (in seconds) to wait after the last trigger reported active before scaling the resource back to 0 |  |
| `server.keda.initialCooldownPeriod` | int | `0` | The delay (in seconds) before the `cooldownPeriod` starts after the initial creation of the `ScaledObject`. |  |
| `server.keda.minReplicaCount` | int | `0` | Minimum number of replicas KEDA will scale the resource down to. By default, it’s scale to zero, but you can use it with some other value as well. |  |
| `server.keda.maxReplicaCount` | int | `5` | This setting is passed to the HPA definition that KEDA will create for a given resource and holds the maximum number of replicas of the target resource. |  |
| `server.keda.fallback` | object | `{}` | Defines a number of replicas to fall back to if a scaler is in an error state. Example: | Example |
| `server.keda.advanced` | object | `{}` | Specifies HPA related options Example: | Example |
| `server.keda.triggers` | list | `[]` | List of triggers to activate scaling of the target resource Example: | Example |
| `server.keda.annotations` | object | `{}` | Annotations to apply to the ScaledObject CRD. Example: | Example |

## accessControl

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `accessControl` | object | `{}` | [System access control](https://trino.io/docs/current/security/built-in-system-access-control.html) configuration. These settings apply to the coordinator and to the workers. Settings meant for one role only go in a `coordinator` or a `worker` block, where they override the shared ones. A rules document denies whatever it does not allow, and both roles serve the management endpoints, so a document without a `system_information` section denies every management read, the metrics endpoint among them, on the coordinator and on the workers alike. The rules are evaluated in order and the first one matching a user decides, so a rule that matches a user without granting `read` denies that user as surely as no rule at all. Granting a user read access to system information on workers takes nothing more than: | Example |

## opa

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `opa.enabled` | bool | `false` | Set to true to deploy an [Open Policy Agent](https://www.openpolicyagent.org/) sidecar container on the Trino coordinator pod. When `opa.enabled` is true and `accessControl` is left at its default (empty `{}`), the chart automatically wires Trino's access-control plugin to this sidecar using `opa.policyPath` and the optional `opa.*Path` / `opa.log*` / `opa.allowPermissionManagementOperations` values. To customize further, set `accessControl.type: properties` and provide the full `accessControl.properties` block; any `accessControl` value disables auto-wiring. |  |
| `opa.image` | string | `"openpolicyagent/opa:1.21.2-static"` | OPA container image. The `-static` variant is recommended: it ships without a shell which keeps the attack surface small. Check for newer versions at https://hub.docker.com/r/openpolicyagent/opa/tags |  |
| `opa.pullPolicy` | string | `"IfNotPresent"` |  |  |
| `opa.listenAddress` | string | `"127.0.0.1"` | Address OPA binds its API to inside the coordinator pod. The default keeps it on loopback, where Trino reaches it and nothing outside the pod can: OPA's API is writable and unauthenticated, so a request that arrives there can replace the policy. The probes use `opa.diagnosticPort` rather than this address, so there is no need to widen it for them. |  |
| `opa.port` | int | `8181` | Port OPA listens on inside the coordinator pod. The URIs Trino is given are built from it, so there is nothing else to keep in step. |  |
| `opa.diagnosticPort` | int | `8282` | Port of OPA's read-only diagnostic listener, which serves `/health` and `/metrics` and nothing else. It binds to all addresses so the kubelet can probe it, which is why the API itself does not have to. |  |
| `opa.policyPath` | string | `"/v1/data/trino/allow"` | Path of the document Trino asks for an authorization decision. The sidecar shares the coordinator's network, so the chart addresses it over loopback on `opa.port` and only the path is configured here. |  |
| `opa.columnMaskingPath` | string | `""` | Path of the document for column-masking decisions. When set, the auto-wired access control adds `opa.policy.column-masking-uri`. Leave empty to omit. |  |
| `opa.rowFiltersPath` | string | `""` | Path of the document for row-filter decisions. When set, the auto-wired access control adds `opa.policy.row-filters-uri`. Leave empty to omit. |  |
| `opa.batchedPath` | string | `""` | Path of the document for batched authorization decisions. When set, the auto-wired access control adds `opa.policy.batched-uri`. Leave empty to omit. |  |
| `opa.logRequests` | bool | `false` | When true, the auto-wired access-control adds `opa.log-requests=true`, logging every request Trino sends to OPA. |  |
| `opa.logResponses` | bool | `false` | When true, the auto-wired access-control adds `opa.log-responses=true`, logging every response Trino receives from OPA. |  |
| `opa.allowPermissionManagementOperations` | bool | `false` | When true, the auto-wired access-control adds `opa.allow-permission-management-operations=true`, allowing CREATE/DROP ROLE, GRANT, REVOKE, and similar statements without OPA evaluation. |  |
| `opa.policy` | object | `{}` | Rego policy files to render into a ConfigMap and mount into the OPA container at `opa.policyMountPath`. Provide one or more entries, each keyed by file name (e.g. `policy.rego`, `data.json`). Mutually exclusive with `opa.policyConfigMap`. Exactly one of the two must be set when `opa.enabled` is true. Example: | Example |
| `opa.policyConfigMap` | string | `""` | Name of an existing ConfigMap holding the OPA policy files. Mutually exclusive with `opa.policy`. |  |
| `opa.policyMountPath` | string | `"/policies"` | Directory inside the OPA container where the policy files are mounted. |  |
| `opa.extraArgs` | list | `[]` | Additional flags appended to the `opa run` command, before the policy directory positional argument. Example: | Example |
| `opa.securityContext` | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"readOnlyRootFilesys...` | [Security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container) for the OPA container. Defaults to a hardened, non-root, read-only-root configuration. Override fields explicitly if your environment requires it. |  |
| `opa.resources` | object | `{"limits":{"memory":"512Mi"},"requests":{"cpu":"250m","memory":"256Mi"}}` | Resource requests and limits for the OPA container. Set to `{}` to opt out of any resource defaults. |  |
| `opa.livenessProbe.initialDelaySeconds` | int | `5` |  |  |
| `opa.livenessProbe.periodSeconds` | int | `10` |  |  |
| `opa.livenessProbe.timeoutSeconds` | int | `5` |  |  |
| `opa.livenessProbe.failureThreshold` | int | `3` |  |  |
| `opa.livenessProbe.successThreshold` | int | `1` |  |  |
| `opa.readinessProbe.initialDelaySeconds` | int | `5` |  |  |
| `opa.readinessProbe.periodSeconds` | int | `10` |  |  |
| `opa.readinessProbe.timeoutSeconds` | int | `5` |  |  |
| `opa.readinessProbe.failureThreshold` | int | `3` |  |  |
| `opa.readinessProbe.successThreshold` | int | `1` |  |  |

## headerAuthenticator

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `headerAuthenticator` | object | `{}` | [Header authenticator](https://trino.io/docs/current/develop/header-authenticator.html) configuration. Required when `server.config.authenticationType` contains `HEADER`. Provide the plugin configuration properties. The `header-authenticator.name` value must match the name of the installed authenticator plugin. | Example |

## resourceGroups

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `resourceGroups` | object | `{}` | [Resource groups control](https://trino.io/docs/current/admin/resource-groups.html) Set the type property to either: * `configmap`, and provide the Resource groups file contents in `resourceGroupsConfig`, * `properties`, and provide configuration properties in `properties`. Properties example: | Example |

## additionalNodeProperties

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `additionalNodeProperties` | list | `[]` | [Additional node properties](https://trino.io/docs/current/installation/deployment.html#log-levels). Supports templating the entries with `tpl`; an entry that renders empty is omitted. Example, assuming the NODE_ID environment variable has been set: | Example |

## additionalConfigProperties

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `additionalConfigProperties` | list | `[]` | [Additional config properties](https://trino.io/docs/current/admin/properties.html). Supports templating the entries with `tpl`; an entry that renders empty is omitted. Example: | Example |

## additionalLogProperties

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `additionalLogProperties` | list | `[]` | [Additional log properties](https://trino.io/docs/current/installation/deployment.html#log-levels). Supports templating the entries with `tpl`; an entry that renders empty is omitted. Example: | Example |

## additionalExchangeManagerProperties

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `additionalExchangeManagerProperties` | list | `[]` | [Exchange manager properties](https://trino.io/docs/current/admin/fault-tolerant-execution.html#exchange-manager). Supports templating the entries with `tpl`; an entry that renders empty is omitted. Example: | Example |

## sessionProperties

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `sessionProperties` | object | `{}` | [Session properties manager config file](https://trino.io/docs/current/admin/session-property-managers.html) is mounted to {{ .Values.server.config.path }}/session-property-config.json Set the type property to either: * `configmap`, and provide the session properties manager file contents in `sessionPropertiesConfig`, * `properties`, and provide configuration properties in `properties`. |  |

## eventListenerProperties

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `eventListenerProperties` | list | `[]` | [Event listener](https://trino.io/docs/current/develop/event-listener.html#event-listener) properties. To configure multiple event listeners, add them in `coordinator.additionalConfigFiles` and `worker.additionalConfigFiles`, and set the `event-listener.config-files` property in `additionalConfigProperties` to their locations. Supports templating the entries with `tpl`; an entry that renders empty is omitted, and no event listener file is written when every entry does. Example: | Example |

## catalogs

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `catalogs` | object | `{"tpcds":"connector.name=tpcds\ntpcds.splits-per-node=4\n","tpch":"connector.name=tpch...` | Configure [catalogs](https://trino.io/docs/current/installation/deployment.html#catalog-properties). Example: | Example |

## additionalCatalogs

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `additionalCatalogs` | object | `{}` | Deprecated, use `catalogs` instead. Configure additional [catalogs](https://trino.io/docs/current/installation/deployment.html#catalog-properties). |  |

## env

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `env` | list | `[]` | additional environment variables added to every pod, specified as a list with explicit values Example: | Example |

## envFrom

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `envFrom` | list | `[]` | additional environment variables added to every pod, specified as a list of either `ConfigMap` or `Secret` references Example: | Example |

## initContainers

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `initContainers` | object | `{}` | Additional [containers that run to completion](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) during pod initialization. Example: | Example |

## sidecarContainers

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `sidecarContainers` | object | `{}` | Additional [containers that starts before](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/) the Trino container and continues to run. Example: | Example |

## securityContext

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `securityContext` | object | `{"runAsGroup":1000,"runAsUser":1000}` | [Pod security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod) configuration. To remove the default, set it to null (or `~`). |  |

## containerSecurityContext

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `containerSecurityContext` | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]}}` | [Container security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container) configuration. |  |
| `containerSecurityContext.allowPrivilegeEscalation` | bool | `false` | Control whether a process can gain more privileges than its parent process. |  |
| `containerSecurityContext.capabilities.drop` | list | `["ALL"]` | A list of the Linux kernel capabilities that are dropped from every container. Valid values are listed in [the capabilities manual page](https://man7.org/linux/man-pages/man7/capabilities.7.html). Ensure # to remove the "CAP_" prefix which the kernel attaches to the names of permissions. |  |

## shareProcessNamespace

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `shareProcessNamespace.coordinator` | bool | `false` |  |  |
| `shareProcessNamespace.worker` | bool | `false` |  |  |

## service

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `service.annotations` | object | `{}` |  |  |
| `service.type` | string | `"ClusterIP"` |  |  |
| `service.port` | int | `8080` |  |  |
| `service.nodePort` | string | `""` | The port the service listens on the host, for the `NodePort` type. If not set, Kubernetes will [allocate a port automatically](https://kubernetes.io/docs/concepts/services-networking/service/#nodeport-custom-port). |  |
| `service.appProtocols` | object | `{}` | The [application protocol](https://kubernetes.io/docs/concepts/services-networking/service/#application-protocol) of a service port, keyed by the name of the port. A service mesh reads it to choose the protocol it speaks to the pods, which is what keeps an HTTP/2 client from being downgraded to HTTP/1.1. The names of the ports are `http`, `https`, `jmx-exporter`, and the `name` of every entry in `additionalExposedPorts`. There is no default, because the right value differs per port: `kubernetes.io/h2c` means cleartext HTTP/2, so it is wrong for a TLS port and for a port that does not speak HTTP at all. Example | Example |
| `service.loadBalancerSourceRanges` | list | `[]` | The client address ranges allowed to reach the service, for the `LoadBalancer` type. The cloud provider turns them into the load balancer's own access rules, such as the security group of an AWS NLB. If empty or not set, the load balancer accepts traffic from anywhere. Example | Example |
| `service.coordinator` | object | `{}` | Override service configurations for the coordinator service. Example | Example |
| `service.worker` | object | `{}` | Override service configurations for the worker service. The worker service is headless, so only the settings that apply to a headless service can be overridden. Setting any other one fails the rendering instead of being silently ignored. Example | Example |

## auth

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `auth` | object | `{}` | Available authentication methods. Use username and password provided as a [password file](https://trino.io/docs/current/security/password-file.html#file-format): | Example |

## serviceAccount

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `serviceAccount.create` | bool | `false` | Specifies whether a service account should be created |  |
| `serviceAccount.name` | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |  |
| `serviceAccount.annotations` | object | `{}` | Annotations to add to the service account |  |

## configMounts

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `configMounts` | list | `[]` | Allows mounting additional Trino configuration files from Kubernetes config maps on all nodes. Example: | Example |

## secretMounts

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `secretMounts` | list | `[]` | Allows mounting additional Trino configuration files from Kubernetes secrets on all nodes. Example: | Example |

## csiSecretMounts

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `csiSecretMounts` | list | `[]` | Allows mounting secrets on all nodes using the [Secrets Store CSI driver](https://secrets-store-csi-driver.sigs.k8s.io/). Each entry creates a volume of type `secrets-store.csi.k8s.io` referencing a `SecretProviderClass` and a matching volume mount on the Trino container. This enables consuming secrets from external stores (AWS Secrets Manager, HashiCorp Vault, Azure Key Vault, etc.) without first materializing a Kubernetes `Secret`. Requires the driver and its provider to be installed in the cluster, and the `SecretProviderClass` to exist in the release namespace. Example: | Example |

## coordinator

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `coordinator.deployment.annotations` | object | `{}` |  |  |
| `coordinator.deployment.progressDeadlineSeconds` | int | `600` | The maximum time in seconds for a deployment to make progress before it is considered failed. The deployment controller continues to process failed deployments and a condition with a ProgressDeadlineExceeded reason is surfaced in the deployment status. |  |
| `coordinator.deployment.revisionHistoryLimit` | int | `10` | The number of old ReplicaSets to retain to allow rollback. |  |
| `coordinator.deployment.strategy` | object | `{}` | The deployment strategy to use to replace existing pods with new ones. |  |
| `coordinator.jvm.maxHeapSize` | string | `"8G"` |  |  |
| `coordinator.jvm.minHeapSize` | string | `nil` |  |  |
| `coordinator.jvm.maxHeapPercent` | string | `nil` | Alternative method of setting heap size as a percentage of container memory limits, which is recommended for better JVM ergonomics. `maxHeapSize` must be unset for this to work. |  |
| `coordinator.jvm.initialHeapPercent` | string | `nil` | sets the starting heap size as a percentage of container memory limits. NOTE: to disable dynamic heap resizing when setting the size as a percent, add `-XX:MaxHeapFreeRatio=100` or `-XX:-UseAdaptiveSizePolicy` to `additionalJVMConfig`. |  |
| `coordinator.jvm.gcMethod.type` | string | `"UseG1GC"` |  |  |
| `coordinator.jvm.gcMethod.g1.heapRegionSize` | string | `"32M"` |  |  |
| `coordinator.config.memory.heapHeadroomPerNode` | string | `""` |  |  |
| `coordinator.config.nodeScheduler.includeCoordinator` | bool | `false` | Allows scheduling work on the coordinator so that a single machine can function as both coordinator and worker. For large clusters, processing work on the coordinator can negatively impact query performance because the machine's resources are not available for the critical coordinator tasks of scheduling, managing, and monitoring query execution. |  |
| `coordinator.config.query.maxMemoryPerNode` | string | `"1GB"` |  |  |
| `coordinator.additionalJVMConfig` | list | `[]` |  |  |
| `coordinator.additionalExposedPorts` | object | `{}` | Additional ports configured in the coordinator container and the service. Example: | Example |
| `coordinator.resources` | object | `{}` | It is recommended not to specify default resources and to leave this as a conscious choice for the user. This also increases chances charts run on environments with little resources, such as Minikube. If you do want to specify resources, use the following example, and adjust it as necessary. Example: | Example |
| `coordinator.livenessProbe` | object | `{}` | [Liveness probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes) options Example: | Example |
| `coordinator.readinessProbe` | object | `{}` | [Readiness probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes) Example: | Example |
| `coordinator.startupProbe` | object | `{}` | [Startup probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes) Example: | Example |
| `coordinator.lifecycle` | object | `{}` | Coordinator container [lifecycle events](https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/) Example: | Example |
| `coordinator.terminationGracePeriodSeconds` | int | `30` |  |  |
| `coordinator.nodeSelector` | object | `{}` |  |  |
| `coordinator.tolerations` | list | `[]` |  |  |
| `coordinator.affinity` | object | `{}` |  |  |
| `coordinator.hostAliases` | list | `[]` | [Adding entries to Pod /etc/hosts with HostAliases] (https://kubernetes.io/docs/tasks/network/customize-hosts-file-for-pods/). Example: | Example |
| `coordinator.additionalConfigFiles` | object | `{}` | Additional config files placed in the default configuration directory. Supports templating the files' contents with `tpl`. Example: | Example |
| `coordinator.additionalVolumes` | list | `[]` | One or more additional volumes to add to the coordinator. Example: | Example |
| `coordinator.additionalVolumeMounts` | list | `[]` | One or more additional volume mounts to add to the coordinator. Example: - name: extras mountPath: /usr/share/extras readOnly: true |  |
| `coordinator.priorityClassName` | string | `nil` |  |  |
| `coordinator.annotations` | object | `{}` | Annotations to add to the coordinator pod. By default, the following annotations are added to the coordinator pod: - `checksum/access-control-config` - checksum of the coordinator access control config file; - `checksum/catalog-config` - checksum of the catalog config file; - `checksum/coordinator-config` - checksum of the coordinator config file. This allows for automatic rolling updates on configuration changes. This behaviour can be disabled by manually setting these annotations to fixed constants in the `coordinator.annotations` section. Example: | Example |
| `coordinator.labels` | object | `{}` |  |  |
| `coordinator.configMounts` | list | `[]` | Allows mounting additional Trino configuration files from Kubernetes config maps on the coordinator node. Example: | Example |
| `coordinator.secretMounts` | list | `[]` | Allows mounting additional Trino configuration files from Kubernetes secrets on the coordinator node. Example: | Example |
| `coordinator.csiSecretMounts` | list | `[]` | Allows mounting secrets on the coordinator node using the [Secrets Store CSI driver](https://secrets-store-csi-driver.sigs.k8s.io/). Each entry creates a volume of type `secrets-store.csi.k8s.io` referencing a `SecretProviderClass` and a matching volume mount on the Trino container. This enables consuming secrets from external stores (AWS Secrets Manager, HashiCorp Vault, Azure Key Vault, etc.) without first materializing a Kubernetes `Secret`. Requires the driver and its provider to be installed in the cluster, and the `SecretProviderClass` to exist in the release namespace. Example: | Example |

## worker

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `worker.deployment.annotations` | object | `{}` |  |  |
| `worker.deployment.progressDeadlineSeconds` | int | `600` | The maximum time in seconds for a deployment to make progress before it is considered failed. The deployment controller continues to process failed deployments and a condition with a ProgressDeadlineExceeded reason is surfaced in the deployment status. |  |
| `worker.deployment.revisionHistoryLimit` | int | `10` | The number of old ReplicaSets to retain to allow rollback. |  |
| `worker.deployment.strategy` | object | `{}` | The deployment strategy to use to replace existing pods with new ones. |  |
| `worker.podDisruptionBudget` | object | `{}` | Worker [Pod Disruption Budget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/#pod-disruption-budgets) configuration. The contents are rendered into the budget's `spec`, so any field the API supports can be set here. Nothing is created while this is empty. | Example |
| `worker.jvm.maxHeapSize` | string | `"8G"` |  |  |
| `worker.jvm.minHeapSize` | string | `nil` |  |  |
| `worker.jvm.maxHeapPercent` | string | `nil` |  |  |
| `worker.jvm.initialHeapPercent` | string | `nil` |  |  |
| `worker.jvm.gcMethod.type` | string | `"UseG1GC"` |  |  |
| `worker.jvm.gcMethod.g1.heapRegionSize` | string | `"32M"` |  |  |
| `worker.config.memory.heapHeadroomPerNode` | string | `""` |  |  |
| `worker.config.query.maxMemoryPerNode` | string | `"1GB"` |  |  |
| `worker.additionalJVMConfig` | list | `[]` |  |  |
| `worker.additionalExposedPorts` | object | `{}` | Additional container ports configured in all worker pods and the worker service. Example: | Example |
| `worker.resources` | object | `{}` | It is recommended not to specify default resources and to leave this as a conscious choice for the user. This also increases chances charts run on environments with little resources, such as Minikube. If you do want to specify resources, use the following example, and adjust it as necessary. Example: | Example |
| `worker.livenessProbe` | object | `{}` | [Liveness probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes) Example: | Example |
| `worker.readinessProbe` | object | `{}` | [Readiness probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes) Example: | Example |
| `worker.startupProbe` | object | `{}` | [Startup probe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#configure-probes) Example: | Example |
| `worker.lifecycle` | object | `{}` | Worker container [lifecycle events](https://kubernetes.io/docs/tasks/configure-pod-container/attach-handler-lifecycle-event/) Setting `worker.lifecycle` conflicts with `worker.gracefulShutdown`. Example: | Example |
| `worker.gracefulShutdown` | object | `{"enabled":false,"gracePeriodSeconds":120,"user":"admin"}` | Configure [graceful shutdown](https://trino.io/docs/current/admin/graceful-shutdown.html) in order to ensure that workers terminate without affecting running queries, given a sufficient grace period. When enabled, the value of `worker.terminationGracePeriodSeconds` must be at least two times greater than the configured `gracePeriodSeconds`. Enabling `worker.gracefulShutdown` conflicts with `worker.lifecycle`. When a custom `worker.lifecycle` configuration needs to be used, graceful shutdown must be configured manually. The shutdown request is sent as `user`, and the chart permits that user to read and write system information in the worker access control rules, merging the permissions into the rules set in `accessControl`. Read access is granted as well because the first rule matching a user decides, so a rule granting only write would deny that user every management read. Example: | Example |
| `worker.terminationGracePeriodSeconds` | int | `30` |  |  |
| `worker.nodeSelector` | object | `{}` |  |  |
| `worker.tolerations` | list | `[]` |  |  |
| `worker.affinity` | object | `{}` |  |  |
| `worker.hostAliases` | list | `[]` | [Adding entries to Pod /etc/hosts with HostAliases] (https://kubernetes.io/docs/tasks/network/customize-hosts-file-for-pods/). Example: | Example |
| `worker.topologySpreadConstraints` | list | `[]` | Configure [topology spread constraints](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/) to control how worker pods are spread across your cluster among failure-domains such as nodes, zones, and regions. This is a best practice for achieving high availability and preventing resource hotspots. Example of spreading workers across hostnames and zones: | Example |
| `worker.additionalConfigFiles` | object | `{}` | Additional config files placed in the default configuration directory. Supports templating the files' contents with `tpl`. Example: | Example |
| `worker.additionalVolumes` | list | `[]` | One or more additional volume mounts to add to all workers. Example: | Example |
| `worker.additionalVolumeMounts` | list | `[]` | One or more additional volume mounts to add to all workers. Example: | Example |
| `worker.priorityClassName` | string | `nil` |  |  |
| `worker.annotations` | object | `{}` | Annotations to add to the worker pods. By default, the following annotations are added to the worker pods: - `checksum/access-control-config` - checksum of the worker access control config file; - `checksum/catalog-config` - checksum of the catalog config file; - `checksum/worker-config` - checksum of the worker config file. This allows for automatic rolling updates on configuration changes. This behaviour can be disabled by manually setting these annotations to fixed constants in the `worker.annotations` section. Example: | Example |
| `worker.labels` | object | `{}` |  |  |
| `worker.configMounts` | list | `[]` | Allows mounting additional Trino configuration files from Kubernetes config maps on all worker nodes. Example: | Example |
| `worker.secretMounts` | list | `[]` | Allows mounting additional Trino configuration files from Kubernetes secrets on all worker nodes. Example: | Example |
| `worker.csiSecretMounts` | list | `[]` | Allows mounting secrets on all worker nodes using the [Secrets Store CSI driver](https://secrets-store-csi-driver.sigs.k8s.io/). Each entry creates a volume of type `secrets-store.csi.k8s.io` referencing a `SecretProviderClass` and a matching volume mount on the Trino container. This enables consuming secrets from external stores (AWS Secrets Manager, HashiCorp Vault, Azure Key Vault, etc.) without first materializing a Kubernetes `Secret`. Requires the driver and its provider to be installed in the cluster, and the `SecretProviderClass` to exist in the release namespace. Example: | Example |

## kafka

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `kafka.mountPath` | string | `"/etc/trino/schemas"` |  |  |
| `kafka.tableDescriptions` | object | `{}` | Custom kafka table descriptions that will be mounted in mountPath. Example: | Example |

## jmx

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `jmx.enabled` | bool | `false` | Set to true to enable the RMI server to expose Trino's [JMX metrics](https://trino.io/docs/current/admin/jmx.html). |  |
| `jmx.registryPort` | int | `9080` |  |  |
| `jmx.serverPort` | int | `9081` |  |  |
| `jmx.exporter.enabled` | bool | `false` | Set to true to run the [JMX exporter](https://github.com/prometheus/jmx_exporter) as a sidecar and export JMX metrics over its own HTTP port. Every Trino node already serves its metrics itself, see `serviceMonitor.openMetrics`, so this is only needed to keep the exporter's metric names or to reach MBeans the OpenMetrics endpoint does not expose. |  |
| `jmx.exporter.image` | string | `"bitnamilegacy/jmx-exporter:1.4.0"` | The upstream project publishes no runnable image, and `bitnamilegacy` is Bitnami's archived namespace, so this tag receives no further updates or CVE patches. It stays the default only because a default has to pull without credentials. Prefer `serviceMonitor.openMetrics`, which needs no sidecar at all. If you do need the sidecar, Docker Hardened Images publishes a maintained `jmx-exporter` under Apache-2.0 at no cost, and pulling it needs a Docker account, so it takes an `imagePullSecrets` entry: | Example |
| `jmx.exporter.pullPolicy` | string | `"IfNotPresent"` | The default tag is pinned to an immutable image in an archived namespace, so pulling it again on every start would only make that registry a startup dependency. |  |
| `jmx.exporter.port` | int | `5556` |  |  |
| `jmx.exporter.env` | list | `[]` | environment variables specifically for the JMX exporter container, specified as a list with explicit values |  |
| `jmx.exporter.envFrom` | list | `[]` | environment variables specifically for the JMX exporter container, specified as a list of either `ConfigMap` or `Secret` references |  |
| `jmx.exporter.configProperties` | string | `""` | The string value is templated using `tpl`. The JMX config properties file is mounted to `/etc/jmx-exporter/jmx-exporter-config.yaml`. Example: | Example |
| `jmx.exporter.securityContext` | object | `{}` |  |  |
| `jmx.exporter.resources` | object | `{}` | It is recommended not to specify default resources and to leave this as a conscious choice for the user. This also increases chances charts run on environments with little resources, such as Minikube. If you do want to specify resources, use the following example, and adjust it as necessary. Example: | Example |
| `jmx.coordinator` | object | `{}` | Override JMX configurations for the Trino coordinator. Example | Example |
| `jmx.worker` | object | `{}` | Override JMX configurations for the Trino workers. Example | Example |

## serviceMonitor

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `serviceMonitor.enabled` | bool | `false` | Set to true to create resources for the [prometheus-operator](https://github.com/prometheus-operator/prometheus-operator). |  |
| `serviceMonitor.apiVersion` | string | `"monitoring.coreos.com/v1"` |  |  |
| `serviceMonitor.labels` | object | `{"prometheus":"kube-prometheus"}` | Labels for serviceMonitor, so that Prometheus can select it |  |
| `serviceMonitor.interval` | string | `"30s"` | The serviceMonitor web endpoint interval |  |
| `serviceMonitor.scrapeTimeout` | string | `""` | Maximum time to wait for a response. If empty or not set, uses the Prometheus global default. |  |
| `serviceMonitor.openMetrics.enabled` | bool | `true` | Scrape the [OpenMetrics](https://trino.io/docs/current/admin/openmetrics.html) endpoint every Trino node serves on its own HTTP port. This needs no sidecar, and it is the recommended metrics source. The `jmx.exporter` sidecar is scraped as well whenever a role runs one, so both metric sets can be collected side by side. Trino exports every MBean it registers itself. Add JVM level beans, which it does not register, with the `openmetrics.jmx-object-names` property in `additionalConfigProperties`: | Example |
| `serviceMonitor.openMetrics.path` | string | `"/metrics"` |  |  |
| `serviceMonitor.openMetrics.username` | string | `"admin"` | The Trino user the scrape authenticates as. Reading `/metrics` is a management read, so a file-based `accessControl` rule set has to allow this user `read` on `system_information` or the scrape gets a 403. | Example |
| `serviceMonitor.openMetrics.password` | string | `""` | The password sent with the username, on the coordinator alone: only it is given an authentication type, so a worker rejects a password and rendering fails rather than leaving you a scrape that cannot authenticate. An empty value sends no password, which is what an unsecured cluster wants. An authentication type Prometheus cannot speak, such as `OAUTH2`, has one more way out: `management.user` lets management endpoints through without credentials. It applies to every management endpoint, not just `/metrics`. | Example |
| `serviceMonitor.openMetrics.authSecret` | string | `""` | Name of an existing Secret in the release namespace holding `username` and `password` keys. The chart creates that Secret from the two values above when this is empty. |  |
| `serviceMonitor.tlsConfig` | object | `{}` | TLS settings for the scrape, needed when `server.config.https.enabled` is set and Prometheus cannot verify the coordinator's certificate. Only the coordinator serves https, so this reaches its endpoint alone. |  |
| `serviceMonitor.endpoints` | list | `[]` | Describe the scrape yourself instead of letting the chart derive it. This replaces the generated endpoints entirely, so nothing else in this block applies to a role that sets it. Example: | Example |
| `serviceMonitor.coordinator` | object | `{}` | Override ServiceMonitor configurations for the Trino coordinator. Example | Example |
| `serviceMonitor.worker` | object | `{}` | Override ServiceMonitor configurations for the Trino workers. Example | Example |

## commonLabels

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `commonLabels` | object | `{}` | Labels that get applied to every resource's metadata |  |

## ingress

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `ingress.enabled` | bool | `false` |  |  |
| `ingress.className` | string | `""` |  |  |
| `ingress.annotations` | object | `{}` |  |  |
| `ingress.hosts` | list | `[]` | [Ingress rules](https://kubernetes.io/docs/concepts/services-networking/ingress/#ingress-rules). Supports templating the `host` values with `tpl`. Example: | Example |
| `ingress.tls` | list | `[]` | Ingress [TLS](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) configuration. Supports templating the `hosts` and `secretName` values with `tpl`. Example: | Example |

## gateway

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `gateway.enabled` | bool | `false` | Set to true to create HTTPRoute resources for [Kubernetes Gateway API](https://gateway-api.sigs.k8s.io/). The Gateway API is the successor to the Ingress API and provides more advanced routing capabilities. > [!NOTE] > - Requires Gateway API CRDs to be installed in the cluster > - Not recommended to use together with `ingress.enabled` (choose one or the other) > - Requires a Gateway resource to be configured separately |  |
| `gateway.annotations` | object | `{}` | Annotations to add to the HTTPRoute resource. Example: | Example |
| `gateway.parentRefs` | list | `[]` | References to the Gateway resources that this HTTPRoute should attach to. Example: | Example |
| `gateway.hostnames` | list | `[]` | Hostnames to match for routing traffic. Example: | Example |
| `gateway.rules` | list | `[]` | HTTPRoute rules for routing traffic to Trino. Each rule can use either the simplified `path` format for basic routing, or the full `matches` format for advanced use cases. Simple path-based routing example: | Example |

## networkPolicy

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `networkPolicy.enabled` | bool | `false` | Set to true to enable Trino pod protection with a [NetworkPolicy](https://kubernetes.io/docs/concepts/services-networking/network-policies/). By default, the NetworkPolicy will only allow Trino pods to communicate with each other. > [!NOTE] > - NetworkPolicies cannot block the ingress traffic coming directly > from the Kubernetes node on which the Pod is running, > and are thus incompatible with services of type `NodePort`. > - When using NetworkPolicies together with JMX metrics export, > additional ingress rules might be required to allow metric scraping. |  |
| `networkPolicy.ingress` | list | `[]` | Additional ingress rules to apply to the Trino pods. Example: | Example |
| `networkPolicy.egress` | list | `[]` | Egress rules to apply to the Trino pods. Example: | Example |

## extraManifests

| Key | Type | Default | Description | |
|---|---|---|---|---|
| `extraManifests` | list | `[]` | Array of extra kubernetes manifests to deploy with the release. Strings are evaluated as a template and can use template expansions and functions. All other objects are used as yaml. |  |
