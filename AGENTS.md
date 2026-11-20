# Instructions for AI agents

You are working with a documentation-only repository about running Trino on Kubernetes. It contains no program source code. Use it to answer questions and to produce configuration for Trino 484 deployed with the official `trino` Helm chart 1.45.0, and Trino Gateway 22 with chart 1.22.0.

Read this file first. It tells you where each answer lives and which rules prevent the common mistakes.

## Versions this repository describes

| Component | Version | Source commit |
|---|---|---|
| Trino | 484 | `trinodb/trino` tag `484` |
| `trino` Helm chart | 1.45.0 | `trinodb/charts` main, 2026-10-20 |
| Trino Gateway | 22 | `trinodb/trino-gateway` tag `22` |
| `trino-gateway` Helm chart | 1.22.0 | `trinodb/charts` main, 2026-10-20 |
| AWS Data on EKS Trino stack | commit `150577d` | `awslabs/data-on-eks` main, 2026-11-19 |

If the user runs another version, say so, and check property names against the release notes in `upstream/trino/docs/release/`. Property names change between releases.

## Layout

| Path | What it is | Trust level |
|---|---|---|
| `guides/` | Task guides written for this repository | Derived from upstream docs, chart templates and source. Verified against them. |
| `guides/reference/` | Generated references: every config property, session property, Helm value, plugin, error code | Mechanically extracted from source or chart files |
| `guides/examples/` | Helm values and JSON files ready to adapt | Keys validated against chart defaults. Not rendered with Helm. |
| `upstream/` | Verbatim copies of upstream documentation and default configuration | Authoritative for the versions above |
| `PROVENANCE.md` | Exact sources, commits, what was copied and what was left out | |

When a guide and an upstream page disagree, the upstream page wins for behaviour and the source-derived reference wins for property names and defaults. Tell the user about the disagreement.

## Route a task to a file

| Task | Start here | Then |
|---|---|---|
| Understand the architecture | `guides/01-architecture.md` | `upstream/trino/docs/overview/concepts.md` |
| Install or change a cluster | `guides/02-install-with-helm.md` | `guides/examples/` |
| Decide which Helm value sets a Trino property | `guides/03-helm-values-to-config-files.md` | `guides/reference/chart-templates.md` |
| Find any Helm value | `guides/reference/helm-values-index-trino.md` | `upstream/charts/charts/trino/README.md` |
| Know which file a property belongs in | `guides/04-configuration-files.md` | |
| Look up a configuration property | `guides/reference/documented-properties.md` | `guides/reference/config-properties/README.md` (A to Z, from source) |
| Look up a session property | `guides/reference/session-properties-from-source.md` | |
| Size memory, heap and pods | `guides/05-memory-and-jvm-sizing.md` | |
| Configure Hive, Iceberg, Delta Lake on S3 and Glue | `guides/06-catalogs-glue-s3.md` | Connector page under `upstream/trino/docs/connector/` |
| Find a connector name or its properties | `guides/reference/plugins-and-connectors.md` | |
| Configure caching | `guides/07-caching.md` | |
| Scale workers, autoscale, shut down gracefully | `guides/08-scaling-and-graceful-shutdown.md` | |
| Survive worker loss, use spot capacity | `guides/09-fault-tolerant-execution.md` | |
| Expose the cluster, TLS termination, NetworkPolicy | `guides/10-networking-and-access.md` | |
| Authentication, access control, secrets | `guides/11-security.md` | `upstream/trino/docs/security/` |
| Resource groups, per-workload limits | `guides/12-resource-groups-and-session-properties.md` | |
| Metrics, logs, events, tracing | `guides/13-observability.md` | |
| Upgrade, restart, maintain | `guides/14-upgrades-and-day-2.md` | `upstream/trino/docs/release/` |
| Several clusters, routing, blue-green | `guides/15-trino-gateway.md` | `upstream/trino-gateway/docs/` |
| EKS reference deployment | `guides/16-eks-reference-stack.md` | `upstream/data-on-eks/` |
| Diagnose a failure | `guides/17-troubleshooting.md` | `guides/reference/error-codes.md` |
| Connect a client, call the REST API, constrain an agent | `guides/18-clients-and-rest-api.md` | |
| Find an upstream page by topic | `guides/19-upstream-doc-map.md` | |
| SQL syntax or a function | `upstream/trino/docs/sql/`, `upstream/trino/docs/functions/` | |

## How to look up a property

1. `grep -rn '<property>' guides/reference/config-properties/README.md` to confirm the name exists in 484 and find its module.
2. Open the module page it links to for type, default expression, description, legacy names and removed names.
3. `grep -rn '<property>' upstream/trino/docs --include=*.md -l | grep -v /release/` for the documentation.
4. If step 1 finds nothing, the property is one of: an Airlift framework property (`http-server.*` other than authentication, `node.*`, `log.*`, `discovery.*`, `openmetrics.*`, `tracing.*`, `otel.*`, most `http-client.*`), a plugin selector (`connector.name`, `access-control.name`, `event-listener.name` and similar, listed in `guides/reference/plugins-and-connectors.md`), a table or column property, misspelled, or removed. Check the upstream docs and the release notes before using it.

Never invent a property name. Trino refuses to start on an unknown property.

## Rules that prevent the common mistakes

1. **Three kinds of properties.** Configuration properties use dots and hyphens and go in files. Session properties use underscores and are set per session or by a session property manager. Catalog properties go in the catalog file only.
2. **Right file.** `config.properties` keys go in `additionalConfigProperties`. Catalog keys go in `catalogs.<name>`. Exchange keys go in `additionalExchangeManagerProperties`. A key in the wrong file stops the server from starting.
3. **Chart-owned keys.** Do not put `query.max-memory`, `query.max-memory-per-node`, `memory.heap-headroom-per-node`, `http-server.http.port`, `discovery.uri`, `coordinator` or `node-scheduler.include-coordinator` in `additionalConfigProperties`. Use the dedicated values. See `guides/03-helm-values-to-config-files.md`.
4. **Memory invariant.** `query.max-memory-per-node` plus `memory.heap-headroom-per-node` must not exceed the heap. Headroom defaults to 30 percent of the heap. Heap should be 70 to 85 percent of pod memory. Memory request equals limit.
5. **Chart memory defaults are small.** 8G heap, `query.max-memory` 4GB, per-node 1GB. Always set them explicitly for real workloads.
6. **Heap by percentage.** `maxHeapPercent` only works when `maxHeapSize` is set to an empty string.
7. **S3.** A lake catalog needs `fs.s3.enabled=true`. The older name `fs.native-s3.enabled` is a legacy alias. `s3.iam-role` requires `s3.auth-type=IAM_ROLE`.
8. **File system cache.** In 484 the cache directories and sizes live in a cache manager file named by `cache-manager.config-files`. A catalog only sets `fs.cache.enabled=true`.
9. **One coordinator.** No coordinator high availability. A coordinator restart fails all running queries. Any change to the coordinator ConfigMap or the catalogs restarts it.
10. **Workers are a Deployment** named `<fullname>-worker`. Autoscaling objects must target `kind: Deployment`.
11. **Graceful shutdown.** `worker.terminationGracePeriodSeconds` must be at least twice `worker.gracefulShutdown.gracePeriodSeconds`, and long enough for the longest query you want to protect.
12. **Autoscalers.** `server.autoscaling` and `server.keda` are mutually exclusive. KEDA needs at least one trigger.
13. **Metric names.** The built-in `/metrics` endpoint and the JMX exporter sidecar name the same value differently. Confirm against the live endpoint before writing a query.
14. **Task retries.** `retry-policy=TASK` needs an exchange manager, and the query memory limits do not apply under it.
15. **Authentication needs a secure channel.** An authentication type without HTTPS or `http-server.process-forwarded=true` lets plain HTTP requests in with a user name alone. With one of them, plain HTTP requests to the coordinator are refused, which affects in-cluster scrapes. See `guides/11-security.md`.
16. **Shared secret.** Required with any authentication, identical on every node.
17. **Access control files.** First matching rule wins. A present section denies what no rule allows. `system_information` denies everything when it has no rules.
18. **Secrets.** Never put credentials in values files. Use `${ENV:NAME}` with `env` or `envFrom`, or mounted Secrets.
19. **Hardened image.** The 484 image has no `grep`, `cut` or package manager. Keep probe and hook commands to shell built-ins and `curl`.
20. **Data on EKS files are templates** for an older chart. Do not copy their keys or property names without checking them. `guides/16-eks-reference-stack.md` lists the differences.

## Procedure for producing a values file

1. Start from the closest file in `guides/examples/`.
2. For every Trino property you add, do the lookup above and place it by rule 2.
3. For every Helm key you add, confirm it in `guides/reference/helm-values-index-trino.md`. Unknown keys are ignored silently by Helm.
4. Compute memory with `guides/05-memory-and-jvm-sizing.md` and check rule 4.
5. Check the render-time failure list in `guides/reference/chart-templates.md`.
6. Tell the user to run `helm template` and read the rendered ConfigMaps before applying.
7. State which changes restart the coordinator.

## Procedure for diagnosing a failure

1. Get the pod state, the first `ERROR` block of the log, or the query error name.
2. Match it in `guides/17-troubleshooting.md`. Look up error names in `guides/reference/error-codes.md`.
3. Confirm the cause with the command or query given there before proposing a change.
4. Propose the smallest change, and say what it restarts.

## Reading the upstream Trino pages

They are MyST Markdown written for Sphinx.

- `|trino_version|` means 484.
- `{doc}` and `{ref}` roles, and Markdown links with empty text and an absolute target, are cross-references. The target `/admin/properties` is the file `upstream/trino/docs/admin/properties.md`. A label such as `prop-type-duration` is defined somewhere as `(prop-type-duration)=`. Find it with `grep -rn '(label)=' upstream/trino/docs`.
- Blocks fenced with `:::` are notes, warnings and list tables. In a `list-table`, each `* -` starts a row and each following `  -` is the next cell.
- `{include} name.fragment` pulls in a `.fragment` file from the same directory.
- `{{breaking}}` in release notes marks a breaking change.

## What this repository does not contain

- Trino, chart or gateway source code, Helm templates, Terraform or scripts. Guides describe what they do.
- Client library documentation that lives in separate repositories.
- Grafana dashboards.
- Anything about a specific organisation's deployment.

When the answer is not here, say so. Do not guess.
