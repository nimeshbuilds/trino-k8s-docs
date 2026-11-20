# 05. Memory and JVM sizing

How to turn a node size into pod resources, heap and Trino memory limits. Trino 484, chart 1.45.0.

## The layers

```text
Kubernetes node memory
  - kubelet and system reserved, DaemonSets
  = allocatable
      Trino pod memory limit (= request)
        JVM heap (-Xmx)
          memory.heap-headroom-per-node     untracked allocations
          general pool = heap - headroom    tracked query memory
            query.max-memory-per-node       cap for one query on this node
        non-heap: code cache, metaspace, thread stacks, direct buffers, native libraries
```

Three properties decide whether queries die or pods die:

| Property | Default | What happens at the limit |
|---|---|---|
| `query.max-memory-per-node` | 30% of heap | The query fails with `EXCEEDED_LOCAL_MEMORY_LIMIT` |
| `query.max-memory` | `20GB` (chart writes `4GB`) | The query fails with `EXCEEDED_GLOBAL_MEMORY_LIMIT` |
| `memory.heap-headroom-per-node` | 30% of heap | Not a limit. It shrinks the pool available to queries. |

Hard rule from upstream: `query.max-memory-per-node` plus `memory.heap-headroom-per-node` must be less than the heap. Trino refuses to start otherwise.

`query.max-total-memory` defaults to twice `query.max-memory` and includes revocable memory. It must be greater than `query.max-memory`.

## Step by step

1. **Pod memory.** Take node allocatable memory, subtract DaemonSet requests, and give the rest to one Trino pod. Set the memory request equal to the limit.
2. **Heap.** 70 to 85 percent of pod memory. Use the low end for small pods, where the fixed non-heap cost is a larger share, and for workloads with wide Parquet or ORC reads that use native buffers.
3. **Headroom.** Leave the default 30 percent unless you have measured. Lower it to 20 to 25 percent on large heaps (64GB and up) to give queries more room. If workers hit `OutOfMemoryError` while Trino reports free pool memory, headroom is too small.
4. **Per-node query limit.** At most heap minus headroom. Lower it if several large queries must coexist on a node.
5. **Cluster query limit.** `query.max-memory` is the most one query may use across all workers. A common starting point is workers x `query.max-memory-per-node` x 0.5 to 0.7. Setting it above workers x per-node limit has no effect.

### Worked example

Workers on nodes with 64 GiB, about 60 GiB allocatable after system pods.

| Setting | Value | Helm value |
|---|---|---|
| Pod memory request and limit | `58Gi` | `worker.resources` |
| Heap | `46G` (about 79%) | `worker.jvm.maxHeapSize` |
| Headroom | `12GB` (about 26%) | `worker.config.memory.heapHeadroomPerNode` |
| Per-node query limit | `24GB` | `worker.config.query.maxMemoryPerNode` |
| Cluster query limit, 10 workers | `120GB` | `server.config.query.maxMemory` |

```yaml
server:
  config:
    query:
      maxMemory: "120GB"
worker:
  jvm:
    maxHeapSize: "46G"
  config:
    memory:
      heapHeadroomPerNode: "12GB"
    query:
      maxMemoryPerNode: "24GB"
  resources:
    requests:
      cpu: "15"
      memory: 58Gi
    limits:
      memory: 58Gi
```

Check: 24 + 12 = 36 GB, below the 46 GB heap. The remaining 10 GB of pool lets a second query run next to one that sits at its per-node cap.

### Coordinator

The coordinator holds plans, statistics, query state and, without spooling, client result buffers. It does not run query tasks when `node-scheduler.include-coordinator=false`, which is the chart default.

- Start at 4 to 8 CPUs and 16 to 32 GiB for tens of concurrent queries. Raise heap when you see long GC pauses on the coordinator, slow planning, or `Query exceeded per-node memory limit` on the coordinator for metadata-heavy statements.
- `coordinator.config.query.maxMemoryPerNode` still has to satisfy the hard rule above against the coordinator heap.
- Thousands of partitions, very wide tables and large `IN` lists drive coordinator memory. So does `query.max-history`.

## Percentage-based heap

The chart can size the heap from the container limit instead of a fixed number:

```yaml
worker:
  jvm:
    maxHeapSize: ""
    maxHeapPercent: "78"
    initialHeapPercent: "78"
```

This writes `-XX:MaxRAMPercentage=78` and `-XX:InitialRAMPercentage=78`. It needs a memory limit on the container. Remember that the per-node limit and headroom values do not scale with it unless you leave them at their percentage defaults by not setting them. The chart always writes `query.max-memory-per-node`, so update `maxMemoryPerNode` whenever the pod size changes.

## CPU

- Requests decide scheduling and the share under contention. Set a CPU request close to what the node offers.
- A CPU limit throttles the JVM and stretches GC pauses. Many operators set no CPU limit on Trino pods and rely on one pod per node. If policy demands a limit, set it equal to the request and to a whole number.
- `task.concurrency` and `task.max-worker-threads` derive from the CPUs the JVM sees. With a CPU limit the JVM sees the limit. Without one it sees the node.

## Requests equal to limits

Memory request equal to limit is the important part. The pod then cannot be evicted for exceeding its request under node pressure, and the heap you configured is always backed by real memory. With CPU request equal to limit as well, the pod is in the Guaranteed QoS class.

## Out-of-memory, two different kinds

| Symptom | Meaning | Fix |
|---|---|---|
| Query fails with `EXCEEDED_LOCAL_MEMORY_LIMIT` or `EXCEEDED_GLOBAL_MEMORY_LIMIT` | Trino enforced a query limit | Raise the limit, add workers, or fix the query |
| Query fails with `CLUSTER_OUT_OF_MEMORY`, or "Query killed because the cluster is out of memory" | Pools are full and the low-memory killer picked a victim | Fewer concurrent large queries through resource groups, or more memory |
| Pod log shows `java.lang.OutOfMemoryError` and the pod restarts | Heap exhausted by untracked allocations | Raise headroom or heap |
| Pod status `OOMKilled`, exit code 137 | Container exceeded its memory limit, usually non-heap growth | Lower the heap percentage, or raise the pod limit |

## Memory under fault-tolerant execution

With `retry-policy=TASK`, `query.max-memory`, `query.max-total-memory` and `query.max-memory-per-node` do not apply. Tasks get memory from a bin-packing allocator that starts each task at `fault-tolerant-execution-task-memory` (default `5GB`) and grows the estimate when a task fails for lack of memory. See guide [09](09-fault-tolerant-execution.md).

## Spill to disk

Spill lets joins, aggregations and sorts write to local disk instead of failing. Upstream labels it legacy and points to fault-tolerant execution with the task retry policy instead. If you do use it:

```yaml
additionalConfigProperties:
  - spill-enabled=true
  - spiller-spill-path=/data/trino/spill
  - max-spill-per-node=100GB
  - query-max-spill-per-node=100GB
worker:
  additionalVolumes:
    - name: spill
      emptyDir: {}
  additionalVolumeMounts:
    - name: spill
      mountPath: /data/trino/spill
```

Size the volume, and count it against ephemeral storage limits. Details in [admin/spill.md](../upstream/trino/docs/admin/spill.md) and [admin/properties-spilling.md](../upstream/trino/docs/admin/properties-spilling.md).

## Garbage collection

The chart sets G1 with a 32M region size. To diagnose pauses, add GC logging:

```yaml
worker:
  additionalJVMConfig:
    - -Xlog:gc*,safepoint::time,level,tags,tid
```

Long pauses on workers show up as tasks that stop reporting and as `REMOTE_TASK_ERROR` or "Encountered too many errors talking to a worker node" on the coordinator.

## What to watch

| Signal | Where |
|---|---|
| Heap used after GC | `java.lang:type=Memory` |
| Pool reserved and free bytes | `trino.memory:type=MemoryPool,name=general` on each node, `trino.memory:type=ClusterMemoryPool,name=general` on the coordinator |
| Queries killed for memory | `trino.memory:name=ClusterMemoryManager` |
| Per-query peak memory | `system.runtime.queries`, the Web UI, query completed events |

```sql
SELECT query_id, state, user, resource_group_id, error_code
FROM system.runtime.queries
WHERE state = 'FAILED' AND error_code LIKE '%MEMORY%'
ORDER BY created DESC
LIMIT 20;
```

Metrics setup is in guide [13](13-observability.md).

## References

- [admin/properties-resource-management.md](../upstream/trino/docs/admin/properties-resource-management.md)
- [installation/deployment.md](../upstream/trino/docs/installation/deployment.md), JVM config section
- [admin/tuning.md](../upstream/trino/docs/admin/tuning.md)
- Source-derived: [MemoryManagerConfig and NodeMemoryConfig](reference/config-properties/core--trino-main.md)
