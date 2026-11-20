# 16. EKS reference stack (AWS Data on EKS)

What the Trino stack in `awslabs/data-on-eks` deploys, how its values map to the official chart, and what to check before reusing it. Based on the repository at commit `150577d` (2026-11-19).

Copied files:

| File | Content |
|---|---|
| [website/docs/datastacks/databases/trino-on-eks/infra.md](../upstream/data-on-eks/website/docs/datastacks/databases/trino-on-eks/infra.md) | Deployment walkthrough, examples, troubleshooting |
| [website/docs/datastacks/databases/trino-on-eks/index.md](../upstream/data-on-eks/website/docs/datastacks/databases/trino-on-eks/index.md) | Stack landing page |
| [website/docs/bestpractices/analytics/trino-best-practices.md](../upstream/data-on-eks/website/docs/bestpractices/analytics/trino-best-practices.md) | Best practices for Trino on EKS |
| [infra/terraform/helm-values/trino.yaml](../upstream/data-on-eks/infra/terraform/helm-values/trino.yaml) | Helm values template |
| [infra/terraform/argocd-applications/trino.yaml](../upstream/data-on-eks/infra/terraform/argocd-applications/trino.yaml) | Argo CD Application |
| [infra/terraform/manifests/trino/keda-scaledobject.yaml](../upstream/data-on-eks/infra/terraform/manifests/trino/keda-scaledobject.yaml) | KEDA ScaledObject |
| [data-stacks/trino-on-eks/examples](../upstream/data-on-eks/data-stacks/trino-on-eks/examples) | Example SQL and an IAM trust policy |

The Terraform code and shell scripts of the stack are not copied. This page describes what they do.

The values and manifest files are Terraform templates. Placeholders like `${region}` and `%{ if enable_polaris ~}` are filled in by Terraform, not by Helm. Do not pass them to Helm as they are.

## What the stack builds

| Piece | Detail |
|---|---|
| Cluster | EKS with Karpenter, deployed by a shared Terraform base with per-stack feature flags |
| Namespace | `trino` |
| Trino | Official `trino` chart from `https://trinodb.github.io/charts`, installed by an Argo CD Application with automated sync, prune and self-heal |
| Chart version | `1.41.0` pinned in the Application. This repository documents 1.45.0. |
| S3 | Two buckets: table data (`...-trino-data-`) and exchange spooling (`...-trino-exchange-`), public access blocked |
| IAM | EKS Pod Identity association for the Trino ServiceAccount, with a bucket-scoped S3 policy for data, one for the exchange bucket, and the AWS managed policy `AWSGlueConsoleFullAccess` |
| Autoscaling | KEDA ScaledObject on Prometheus metrics, when `enable_trino_keda` is set |
| Monitoring | JMX exporter sidecar, ServiceMonitors for coordinator and workers |
| Optional | A Polaris REST catalog when `enable_polaris` is set |

The stack's feature flags also enable other platform components in the same cluster, such as Spark, Kafka, Superset and Prometheus. See the walkthrough for the list.

S3 actions granted on both buckets: `s3:GetObject`, `s3:GetObjectVersion`, `s3:PutObject`, `s3:DeleteObject`, `s3:ListBucket`, `s3:GetBucketLocation`, `s3:ListBucketMultipartUploads`, `s3:ListMultipartUploadParts`, `s3:AbortMultipartUpload`, plus `s3:ListAllMyBuckets`.

## Trino configuration it applies

| Area | Setting |
|---|---|
| Coordinator | Heap `32G`, pod memory 40Gi, 4 to 6 CPUs, R8g family, on-demand, `karpenter.sh/do-not-disrupt: "true"` |
| Workers | Heap `12G`, pod memory 40Gi, 4 to 6 CPUs, R8g family, on-demand, spread one per host |
| Query memory | `query.max-memory=280GB`, `query.max-memory-per-node=6GB`, `memory.heap-headroom-per-node=1.6GB` |
| Retry | `retry-policy=TASK` with the `filesystem` exchange manager on the exchange bucket |
| Exchange | `exchange.s3.region`, `exchange.s3.endpoint`, `exchange.s3.max-error-retries=10`, `exchange.s3.upload.part-size=64MB`, `exchange.compression-enabled=true` |
| Other | `query.max-execution-time=24h`, `query.client.timeout=30m`, `query.max-history=100`, `query.remote-task.max-error-duration=1m`, `query.max-hash-partition-count=100`, `sink.max-buffer-size=1GB`, `optimizer.join-reordering-strategy=AUTOMATIC`, spill disabled |
| Catalogs | `hive` on Glue and `iceberg` on Glue, both with the S3 file system enabled. Iceberg: Parquet, unique table locations, `register_table` enabled. |
| Metrics | JMX exporter with an object name allow-list for query manager, cluster size monitor, task manager, memory pools and JVM memory |

## Points to check before reuse

These are observations from comparing the files with chart 1.45.0 and Trino 484. Verify each in your environment.

1. **Chart and Trino version.** The Application pins chart 1.41.0, which ships an older Trino. Values keys and Trino property names differ between versions. Render with the chart version you actually deploy.
2. **KEDA target kind.** The ScaledObject targets `kind: StatefulSet` named `trino-worker`. Chart 1.45.0 creates a worker Deployment. Prefer `server.keda` in the chart, which generates a matching ScaledObject. See guide [08](08-scaling-and-graceful-shutdown.md).
3. **Metric names.** The KEDA queries use JMX exporter names such as `trino_execution_QueryManager_QueuedQueries`. The built-in OpenMetrics endpoint names the same value `trino_execution_name_QueryManager_QueuedQueries`.
4. **Legacy property name.** The catalogs use `fs.native-s3.enabled=true`. Trino 484 documents `fs.s3.enabled` and accepts the old name as legacy.
5. **Memory arithmetic.** Workers have a 12G heap inside a 40Gi pod. That leaves most of the pod memory unused by the heap. With 6GB per-node query memory and 1.6GB headroom, the settings are safe but conservative. Resize heap toward 70 to 85 percent of pod memory for production. See guide [05](05-memory-and-jvm-sizing.md).
6. **`query.max-memory=280GB` under `TASK`.** With the task retry policy the query memory limits do not apply.
7. **Values keys.** `server.config.query.initialHashPartitions`, `maxStageRetries`, `maxExecutionTime`, `clientTimeout`, `coordinator.config.query.minWorkers` and `coordinator.topologySpreadConstraints` appear in the template. They are not keys of chart 1.45.0, where only `server.config.query.maxMemory`, the per-role `maxMemoryPerNode` and `heapHeadroomPerNode`, and `worker.topologySpreadConstraints` exist. Helm ignores unknown keys silently, so those settings have no effect on 1.45.0. Put such properties in `additionalConfigProperties`.
8. **`exchange.compression-enabled`.** Release 437 replaced this property with `exchange.compression-codec`, marked as a breaking change in [release-437.md](../upstream/trino/docs/release/release-437.md). The 484 source has no property of the old name, so a 484 server given `exchange.compression-enabled=true` refuses to start with an unused property error. Use `exchange.compression-codec=LZ4` or `ZSTD`.
9. **JVM flags.** `coordinator.jvm.extraArguments` and `worker.jvm.extraArguments` are not chart 1.45.0 keys. The chart key is `additionalJVMConfig` at the role level, and it already writes the G1 and out-of-memory flags.
10. **Glue policy.** `AWSGlueConsoleFullAccess` is broad. Replace it with a policy scoped to the actions and databases Trino needs. See guide [06](06-catalogs-glue-s3.md#iam-policy-shape).
11. **JMX exporter image.** The values use an image tag `latest` from a third-party registry. Pin and mirror it.
12. **Graceful shutdown.** Not enabled in the values. Add `worker.gracefulShutdown` and a PodDisruptionBudget if queries should survive scale-in without relying only on task retries.
13. **Authentication.** None configured. The walkthrough reaches the UI by port-forward. Add the security steps of guide [11](11-security.md) before exposing the Service.

## Using the walkthrough

[infra.md](../upstream/data-on-eks/website/docs/datastacks/databases/trino-on-eks/infra.md) covers:

- Prerequisites and deployment with the repository's deploy script.
- Verification of pods, the KEDA object and Karpenter nodes.
- Example 1: Hive connector on a sample data set registered in Glue by a setup script.
- Example 2: Iceberg tables created from the TPC-DS connector with [trino_sf10000_tpcds_to_iceberg.sql](../upstream/data-on-eks/data-stacks/trino-on-eks/examples/trino_sf10000_tpcds_to_iceberg.sql), then queried with [trino_select_query_iceberg.sql](../upstream/data-on-eks/data-stacks/trino-on-eks/examples/trino_select_query_iceberg.sql), while watching KEDA scale workers.
- Example 3: fault-tolerant execution, by removing workers during a query.
- Best practices, troubleshooting and cleanup.

The shell commands in that page call scripts that are not part of this repository. Read them as a description of the steps.

## Best practices page

[trino-best-practices.md](../upstream/data-on-eks/website/docs/bestpractices/analytics/trino-best-practices.md) discusses cluster creation and add-ons, Karpenter node pools and mixed instances, coordinator and worker settings, the exchange manager on S3, requests against limits, KEDA, file caching, compute choices, VPC CNI configuration, S3 usage, and Hive and Iceberg connector settings.

Treat the property names in it with the same care as point 4 and point 8 above. It was written against earlier Trino versions, and its file caching section predates the cache manager model of 484. Guides [06](06-catalogs-glue-s3.md) and [07](07-caching.md) carry the 484 names.

## An equivalent values file for chart 1.45.0

[examples/values-eks-glue-s3.yaml](examples/values-eks-glue-s3.yaml) expresses the same design with 1.45.0 keys and 484 property names: Glue catalogs, Pod Identity through the ServiceAccount, explicit memory, graceful shutdown, a PodDisruptionBudget and OpenMetrics scraping. Add [examples/values-fault-tolerant.yaml](examples/values-fault-tolerant.yaml) for the exchange bucket and [examples/values-autoscaling-keda.yaml](examples/values-autoscaling-keda.yaml) for KEDA.
