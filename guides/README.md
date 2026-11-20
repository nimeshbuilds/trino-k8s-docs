# Guides

Task-oriented documentation for Trino 484 on Kubernetes with the official Helm chart 1.45.0. Read in order for a full picture, or jump to what you need.

| # | Guide | Covers |
|---|---|---|
| 01 | [Architecture](01-architecture.md) | Coordinator, workers, catalogs, discovery, configuration model, the three kinds of properties |
| 02 | [Install with Helm](02-install-with-helm.md) | Install, verify, change, test, uninstall, production checklist |
| 03 | [From Helm values to Trino files](03-helm-values-to-config-files.md) | Which value writes which line of which file, worked examples, common mistakes |
| 04 | [Configuration files](04-configuration-files.md) | Every file under `/etc/trino`, the main properties and their defaults |
| 05 | [Memory and JVM sizing](05-memory-and-jvm-sizing.md) | Pod, heap, headroom and query limits, with a worked example |
| 06 | [Catalogs on Glue and S3](06-catalogs-glue-s3.md) | Hive, Iceberg and Lakehouse catalogs, S3 file system, IAM on EKS |
| 07 | [Caching](07-caching.md) | Metastore, listing, memory and file system caches, the 484 cache manager model |
| 08 | [Scaling and graceful shutdown](08-scaling-and-graceful-shutdown.md) | Manual scaling, HPA, KEDA, graceful shutdown, disruption budgets, node autoscaling |
| 09 | [Fault-tolerant execution](09-fault-tolerant-execution.md) | Retry policies, the S3 exchange manager, task memory, testing |
| 10 | [Networking and access](10-networking-and-access.md) | Ports, Services, Ingress, Gateway API, TLS termination, NetworkPolicy |
| 11 | [Security](11-security.md) | TLS, shared secret, authentication, access control, OPA, secrets |
| 12 | [Resource groups and session properties](12-resource-groups-and-session-properties.md) | Admission control and per-workload defaults |
| 13 | [Observability](13-observability.md) | Metrics, JMX, SQL views, event listeners, tracing, logs |
| 14 | [Upgrades and day-2](14-upgrades-and-day-2.md) | What restarts what, upgrade procedures, breaking changes in 483 and 484, maintenance |
| 15 | [Trino Gateway](15-trino-gateway.md) | Routing across clusters, blue-green upgrades |
| 16 | [EKS reference stack](16-eks-reference-stack.md) | The AWS Data on EKS Trino stack, and what to check before reusing it |
| 17 | [Troubleshooting](17-troubleshooting.md) | Symptom tables for startup, workers, queries, storage, performance, scaling, security |
| 18 | [Clients and the REST API](18-clients-and-rest-api.md) | CLI, JDBC, Python, the HTTP protocol, rules for automated clients and AI agents |
| 19 | [Map of the upstream documentation](19-upstream-doc-map.md) | Every copied upstream page with its title |

Also:

- [reference/](reference/README.md): generated references for every configuration property, session property, Helm value, plugin and error code.
- [examples/](examples/README.md): Helm values and JSON files.

## Conventions

- Release name `trino` in namespace `trino`, so objects are `trino`, `trino-coordinator` and `trino-worker`. Adjust for your release name.
- Regions, account IDs, bucket names and host names are placeholders: `us-east-1`, `111122223333`, `my-trino-exchange`, `trino.example.internal`.
- Property tables give the documented default when the upstream documentation has one, otherwise the default from source.
- "Chart default" means the default in chart 1.45.0. "Default" alone means the Trino default.

## How these guides were written

1. The upstream documentation, chart values, chart templates, image build files and the Trino Java sources at release 484 were read.
2. References were generated mechanically from the sources and checked for completeness: the number of extracted configuration properties equals the number of `@Config` annotations in the main sources.
3. Each guide was checked against those references and the upstream pages: property names, defaults, error names, log messages, SQL system tables, chart keys and template behaviour.
4. Relative links were checked. Example values were validated key by key against the chart defaults.

What was not done: nothing was run against a live cluster, and the example values were not rendered with Helm. Sizing numbers and tuning advice are starting points from the upstream documentation and general practice, not measurements. Verify in a test environment.

Where a guide states behaviour that the upstream documentation does not cover, it says that the statement comes from the source or from the chart templates.
