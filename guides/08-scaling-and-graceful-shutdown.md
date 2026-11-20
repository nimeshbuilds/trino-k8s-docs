# 08. Scaling and graceful shutdown

How to change worker count by hand or automatically without failing queries. Trino 484, chart 1.45.0.

## The problem

Trino workers hold query state in memory. A worker that disappears takes every query with a task on it down, unless fault-tolerant execution is on. Kubernetes removes pods all the time: scale-in, node consolidation, spot interruption, rolling updates. So every scaling setup needs two things:

1. A way to remove workers gently: graceful shutdown.
2. A scaling signal that reflects query load: CPU, or queue and concurrency metrics.

## Graceful shutdown

### What Trino does

A worker that receives `PUT /v1/info/state` with body `"SHUTTING_DOWN"`:

1. Enters state `SHUTTING_DOWN`.
2. Sleeps for `shutdown.grace-period` (default `2m`). During this time the coordinator notices and stops sending new tasks.
3. Waits until all its running tasks finish.
4. Sleeps for the grace period again so the coordinator can collect the results.
5. Exits.

Only workers support this. The call needs a user who may write system information. The `default` system access control does not allow it. `allow-all`, file-based rules with a `system_information` write rule, or Ranger do. Upstream page: [admin/graceful-shutdown.md](../upstream/trino/docs/admin/graceful-shutdown.md).

### What the chart does

```yaml
worker:
  gracefulShutdown:
    enabled: true
    gracePeriodSeconds: 60
    user: admin
  terminationGracePeriodSeconds: 3600
```

With this, the chart writes `shutdown.grace-period=60s` on all nodes, adds a `preStop` hook that sends the shutdown request as `user` and waits for the Trino process to exit, and merges a `system_information` write rule for that user into the worker access control rules.

Rules the chart enforces at render time:

- `worker.terminationGracePeriodSeconds` must be at least two times `gracePeriodSeconds`.
- `worker.lifecycle` must not be set at the same time.
- Worker access control must not be of type `properties`.

### Picking the numbers

Total shutdown time is two grace periods plus the longest task still running. Kubernetes kills the pod when `terminationGracePeriodSeconds` runs out, whatever Trino is doing.

| Workload | `gracePeriodSeconds` | `terminationGracePeriodSeconds` |
|---|---|---|
| Interactive, queries under a few minutes | 30 to 60 | 600 to 900 |
| Mixed, queries up to an hour | 60 | 3600 to 4500 |
| Long batch without fault tolerance | 60 to 120 | The longest query you are willing to protect, plus margin |

Set `query.max-execution-time` to a value below the termination grace period, so no query can outlive the window.

Spot instances give two minutes of notice. Graceful shutdown cannot protect a query longer than that on spot capacity. Use fault-tolerant execution there. See guide [09](09-fault-tolerant-execution.md).

### Authentication and TLS

The hook calls `http://localhost:<service.port>` with an `X-Trino-User` header. That works while workers accept plain HTTP without authentication, which is the chart default, because the chart only writes authentication settings on the coordinator. If you add authentication or HTTPS-only settings to workers, write your own `worker.lifecycle` hook with credentials and turn `worker.gracefulShutdown.enabled` off. In that case add `shutdown.grace-period` and the access control rule yourself.

### PodDisruptionBudget

```yaml
worker:
  podDisruptionBudget:
    maxUnavailable: 1
```

The budget limits voluntary evictions, for example node drains, to one worker at a time. Combined with graceful shutdown, a drain walks through the workers one by one.

### Rolling updates

```yaml
worker:
  deployment:
    strategy:
      type: RollingUpdate
      rollingUpdate:
        maxSurge: 1
        maxUnavailable: 0
```

Each old pod goes through graceful shutdown before it exits. A full rollout takes roughly the number of workers times the shutdown time, divided by the parallelism you allow.

## Manual scaling

```yaml
server:
  workers: 10
```

```bash
helm upgrade trino trino/trino -n trino --version 1.45.0 -f values.yaml
# or, temporarily:
kubectl -n trino scale deploy/trino-worker --replicas=10
```

New workers join in under a minute once the pod is running. Queries already running do not use them for stages that have started. New queries do.

`server.workers: 0` with both autoscalers off removes the worker Deployment. Set `coordinator.config.nodeScheduler.includeCoordinator: true` for a single-node cluster.

## Horizontal Pod Autoscaler

```yaml
server:
  workers: 3
  autoscaling:
    enabled: true
    maxReplicas: 20
    targetCPUUtilizationPercentage: 60
    targetMemoryUtilizationPercentage: ""
    behavior:
      scaleUp:
        stabilizationWindowSeconds: 0
        policies:
          - type: Percent
            value: 100
            periodSeconds: 30
      scaleDown:
        stabilizationWindowSeconds: 600
        policies:
          - type: Pods
            value: 1
            periodSeconds: 120
```

- `server.workers` becomes `minReplicas`.
- Needs CPU requests on workers and a metrics server.
- Disable the memory target with an empty string. JVM memory does not fall when load falls, so a memory target keeps the cluster scaled out.
- Scale down slowly. Each removed worker needs its graceful shutdown window.

CPU is a late signal for Trino. Queries queue before CPU averages move. Prefer KEDA with query metrics when you can.

## KEDA

```yaml
server:
  workers: 0
  keda:
    enabled: true
    pollingInterval: 30
    cooldownPeriod: 600
    minReplicaCount: 2
    maxReplicaCount: 30
    triggers:
      - type: prometheus
        metricType: Value
        metadata:
          serverAddress: http://prometheus-operated.monitoring.svc:9090
          threshold: "1"
          metricName: required_workers
          query: >-
            sum by (service)
            (avg_over_time(trino_execution_name_ClusterSizeMonitor_RequiredWorkers{service={{ include "trino.fullname" . | quote }}}[1m]))
```

- KEDA must be installed in the cluster. The chart creates a `ScaledObject` that targets the worker Deployment.
- `server.keda.enabled` and `server.autoscaling.enabled` are mutually exclusive.
- At least one trigger is required.
- `minReplicaCount: 0` allows scale to zero. A query submitted while fewer than `query-manager.required-workers` workers (default `1`) are active waits up to `query-manager.required-workers-max-wait` (default `5m`), then fails with `GENERIC_INSUFFICIENT_RESOURCES` and the message "Insufficient active worker nodes". The waiting query raises the `RequiredWorkers` metric, which is what the trigger above scales on. Raise the wait if nodes take longer than five minutes to arrive.

### Metric names

The same JMX attribute has two names depending on who exports it:

| Source | Example name |
|---|---|
| Trino OpenMetrics endpoint `/metrics`, scraped by the chart ServiceMonitor by default | `trino_execution_name_QueryManager_QueuedQueries` |
| `jmx.exporter` sidecar with a catch-all rule | `trino_execution_QueryManager_QueuedQueries` |

The OpenMetrics name carries a `name_` segment. Pick the form that matches what your Prometheus scrapes. Confirm with:

```bash
kubectl -n trino exec deploy/trino-coordinator -- \
  curl -s -H 'X-Trino-User: admin' http://localhost:8080/metrics | grep -E 'QueryManager_(Queued|Running)Queries'
```

Useful signals:

| Signal | Bean and attribute | Use |
|---|---|---|
| Queued queries | `trino.execution:name=QueryManager`, `QueuedQueries` | Scale out when queries wait |
| Running queries | `trino.execution:name=QueryManager`, `RunningQueries` | Scale with concurrency |
| Required workers | `trino.execution:name=ClusterSizeMonitor`, `RequiredWorkers` | Wake a cluster scaled to zero |
| Cluster memory | `trino.memory:type=ClusterMemoryPool,name=general`, `ReservedDistributedBytes`, `FreeDistributedBytes`, `BlockedNodes` | Scale on memory pressure |

A trigger on queued queries alone never fires when resource groups admit everything. A trigger on running queries per worker is a sturdier default. Example with both: [examples/values-autoscaling-keda.yaml](examples/values-autoscaling-keda.yaml).

### The Data on EKS ScaledObject

The Data on EKS stack ships a hand-written ScaledObject, copied at [upstream/data-on-eks/infra/terraform/manifests/trino/keda-scaledobject.yaml](../upstream/data-on-eks/infra/terraform/manifests/trino/keda-scaledobject.yaml). It uses the JMX exporter metric names and targets `kind: StatefulSet`. Chart 1.45.0 creates a worker Deployment, so check the target kind before reusing that manifest. Using `server.keda` in the chart avoids the mismatch.

## Node autoscaling

Pod autoscaling adds pods. Something has to add nodes: Karpenter or Cluster Autoscaler.

- Give workers a dedicated node pool with a taint, and matching `worker.tolerations` and `worker.nodeSelector`.
- Spread workers with `worker.topologySpreadConstraints` or anti-affinity, one per node.
- Protect the coordinator from consolidation, for example with the annotation `karpenter.sh/do-not-disrupt: "true"` under `coordinator.annotations`, and run it on on-demand capacity.
- Node consolidation evicts worker pods. Graceful shutdown and the PodDisruptionBudget make that safe. Make sure the node autoscaler respects the pod termination grace period.
- Keep worker nodes in one availability zone when cross-zone data transfer cost matters more than zone resilience. Workers exchange a lot of data with each other.

## Checklist

1. `worker.gracefulShutdown.enabled: true`, with a termination grace period that covers your longest protected query.
2. `worker.podDisruptionBudget` set.
3. Slow scale-down behaviour.
4. Scaling signal based on query metrics.
5. Coordinator excluded from disruption.
6. `query.max-execution-time` below the termination grace period.
7. Fault-tolerant execution for anything on spot capacity or longer than the window.
