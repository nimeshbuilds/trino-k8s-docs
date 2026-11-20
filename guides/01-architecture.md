# 01. Architecture

How a Trino cluster is put together, and what that means on Kubernetes. Versions covered: Trino 484, `trino` Helm chart 1.45.0.

## The moving parts

| Part | What it does | On Kubernetes |
|---|---|---|
| Coordinator | Parses SQL, plans queries, schedules tasks on workers, tracks worker health, serves the client REST API and the Web UI | One pod, a Deployment with a single replica |
| Worker | Runs tasks, reads from data sources, exchanges intermediate data with other workers | A Deployment with N replicas |
| Catalog | A named configuration of one connector, for example `hive` or `iceberg` | One `<name>.properties` file in `/etc/trino/catalog` on every node |
| Connector | Plugin that talks to one kind of data source | Shipped in the image under `/usr/lib/trino/plugin` |
| Client | CLI, JDBC driver, language clients, BI tools | Talks HTTP to the coordinator Service only |

A cluster has exactly one coordinator. Trino has no built-in coordinator high availability. If the coordinator pod restarts, running queries fail and clients have to resubmit. Plan for that in three ways:

- Keep the coordinator on stable, on-demand capacity and protect it from voluntary disruption.
- Let Kubernetes restart it quickly. Startup takes seconds to a few minutes depending on the number of catalogs.
- Put [Trino Gateway](15-trino-gateway.md) in front of two or more clusters when you need failover or zero-downtime upgrades.

## Query execution in one paragraph

A client posts a statement to the coordinator. The coordinator plans it into stages. Each stage runs as tasks on workers. Tasks process splits, which are chunks of the underlying data, for example a range of a Parquet file. Workers pull intermediate data from each other over HTTP. The coordinator collects the final stage and serves the result to the client in pages, or hands out links to spooled result segments when the spooling protocol is enabled.

Consequences:

- Every worker must reach every other worker and the coordinator on the HTTP port. The coordinator must reach every worker.
- Memory matters more than disk. By default all intermediate data stays in memory.
- A lost worker fails every query with a task on it, unless [fault-tolerant execution](09-fault-tolerant-execution.md) is on.

Concepts are defined in full in the upstream page [overview/concepts.md](../upstream/trino/docs/overview/concepts.md).

## Discovery

Workers find the coordinator through `discovery.uri` in `config.properties`. The chart sets it to the coordinator Service, `http://<fullname>:<service.port>`. The coordinator points at itself on `localhost`. Workers announce themselves, and the coordinator starts scheduling on a worker once it is announced and healthy.

There is no ZooKeeper, no etcd and no persistent state. A Trino cluster is stateless apart from what you configure externally: the metastore, object storage, an exchange bucket for fault-tolerant execution, and optional databases for resource groups or event listeners.

## Configuration model

Trino reads static files from its `etc` directory at startup. On the official image that directory is `/etc/trino`. Almost every change needs a restart of the affected nodes. The exceptions are files with their own refresh period: file-based access control rules, the file group provider, the password file and database-backed resource groups.

| File | Scope | Purpose |
|---|---|---|
| `node.properties` | Each node | Environment name, data directory, node ID |
| `jvm.config` | Each node | JVM flags, one per line |
| `config.properties` | Each node, with role differences | Server configuration |
| `log.properties` | Each node | Log levels |
| `catalog/<name>.properties` | All nodes, identical | One catalog each |
| `exchange-manager.properties` | All nodes | Spooling storage for fault-tolerant execution |
| `resource-groups.properties` | Coordinator | Admission control |
| `session-property-config.properties` | Coordinator | Default session properties by rule |
| `access-control.properties` | All nodes | System access control |
| `password-authenticator.properties`, `group-provider.properties`, `header-authenticator.properties` | Coordinator | Authentication helpers |
| `event-listener.properties` | Coordinator, and workers for some listeners | Query event sinks |

Guide [04](04-configuration-files.md) covers each file. Guide [03](03-helm-values-to-config-files.md) shows which Helm value writes which line.

## Three kinds of properties

Agents and humans mix these up. Keep them apart.

| Kind | Where it is set | Example |
|---|---|---|
| Configuration property | A properties file, read at startup | `query.max-memory=50GB` in `config.properties` |
| Catalog configuration property | `catalog/<name>.properties` | `hive.metastore=glue` |
| Session property | Per session with `SET SESSION`, per client connection, or by a session property manager rule | `SET SESSION query_max_execution_time = '2h'`, `SET SESSION hive.insert_existing_partitions_behavior = 'OVERWRITE'` |

Configuration properties use dots and hyphens. Session properties use underscores. Many configuration properties have a session property twin that overrides them for one session.

Complete lists generated from the source code:

- [Every configuration property](reference/config-properties/README.md)
- [Every session property](reference/session-properties-from-source.md)

## What the Helm chart deploys

```text
                 clients
                    |
        Service <fullname> (ClusterIP by default, port 8080)
                    |
        Deployment <fullname>-coordinator (1 replica)
            ConfigMap <fullname>-coordinator   -> /etc/trino
            ConfigMap <fullname>-catalog       -> /etc/trino/catalog
                    |
        Deployment <fullname>-worker (N replicas)
            ConfigMap <fullname>-worker        -> /etc/trino
            ConfigMap <fullname>-catalog       -> /etc/trino/catalog
        Service <fullname>-worker (headless)
        optional: HorizontalPodAutoscaler or KEDA ScaledObject, PodDisruptionBudget
```

There is no official Trino operator. The Helm chart in `trinodb/charts` is the only Kubernetes path maintained by the Trino project. Details of every rendered object are in [reference/chart-templates.md](reference/chart-templates.md).

## Workers are a Deployment

Workers hold no durable state, so the chart uses a Deployment. Two things follow:

- Local disk for spill or for the file system cache is `emptyDir` or a host path, added with `worker.additionalVolumes`. It is lost when the pod goes away, which is fine for a cache.
- Tooling that expects a worker StatefulSet does not match this chart. Check the `kind` in any autoscaling object you write by hand.

## Sizing rules of thumb

These are starting points. Measure and adjust.

- Run one Trino pod per node, with requests equal to limits for memory, so the JVM heap is never squeezed by neighbours.
- Give workers identical shapes. The scheduler assumes workers are alike, and per-node memory limits are one setting for all workers.
- Size the coordinator by concurrency and plan complexity, not by data volume. It needs heap for query plans, statistics, and the metadata of every running query.
- Prefer fewer, larger workers over many small ones for joins and aggregations that need memory. Prefer more workers for scan-heavy work that parallelises well.

Memory math is in guide [05](05-memory-and-jvm-sizing.md).

## Where to go next

| Goal | Guide |
|---|---|
| Install a first cluster | [02](02-install-with-helm.md) |
| Query a data lake on S3 with Glue | [06](06-catalogs-glue-s3.md) |
| Stop queries failing when workers leave | [08](08-scaling-and-graceful-shutdown.md), [09](09-fault-tolerant-execution.md) |
| Lock the cluster down | [11](11-security.md) |
| Keep one tenant from starving another | [12](12-resource-groups-and-session-properties.md) |
