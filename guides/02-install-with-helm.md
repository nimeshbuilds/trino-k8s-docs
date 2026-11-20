# 02. Install with Helm

Install, verify, change and remove a Trino cluster with the official chart. Chart 1.45.0 deploys Trino 484.

## Requirements

- A Kubernetes cluster on a supported Kubernetes version.
- `kubectl` and `helm` within the version skew policies of your cluster.
- Nodes for both CPU architectures are fine. The image is built for `amd64` and `arm64`.
- Outbound access from pods to your data sources.
- Optional: Prometheus Operator for ServiceMonitors, KEDA for event-driven autoscaling, an ingress controller or Gateway API implementation for external access.

## Install

```bash
helm repo add trino https://trinodb.github.io/charts
helm repo update
helm search repo trino/trino --versions | head

kubectl create namespace trino
helm install trino trino/trino \
  --namespace trino \
  --version 1.45.0 \
  --values values.yaml
```

Naming the release `trino` gives short object names: `trino`, `trino-coordinator`, `trino-worker`. Any other release name produces `<release>-trino-...`. See [reference/chart-templates.md](reference/chart-templates.md#names).

Pin `--version`. A chart upgrade can change the Trino version, because `image.tag` defaults to the chart `appVersion`.

Start from one of the example files:

| File | Use |
|---|---|
| [examples/values-minimal.yaml](examples/values-minimal.yaml) | Smallest sensible cluster with explicit resources |
| [examples/values-eks-glue-s3.yaml](examples/values-eks-glue-s3.yaml) | Data lake on S3 with Glue, Hive and Iceberg catalogs |
| [examples/values-fault-tolerant.yaml](examples/values-fault-tolerant.yaml) | Task retries with an S3 exchange |
| [examples/values-autoscaling-keda.yaml](examples/values-autoscaling-keda.yaml) | KEDA autoscaling with graceful shutdown |
| [examples/values-secured.yaml](examples/values-secured.yaml) | Password authentication, shared secret, file-based access control |

Combine several with repeated `--values` flags. Later files win. Helm merges maps but replaces lists, so list values such as `additionalConfigProperties` and `env` must be merged by hand into one file. See [examples/README.md](examples/README.md#layering).

## Render before you apply

```bash
helm template trino trino/trino --version 1.45.0 -n trino -f values.yaml > rendered.yaml
```

Read the rendered ConfigMaps. They show the exact `config.properties`, `jvm.config` and catalog files Trino will start with. This is the fastest way to catch a property in the wrong place. The chart also stops with a clear error for invalid combinations, listed in [reference/chart-templates.md](reference/chart-templates.md#render-time-failures).

## Verify

```bash
kubectl -n trino get pods -o wide
kubectl -n trino rollout status deploy/trino-coordinator
kubectl -n trino rollout status deploy/trino-worker
kubectl -n trino logs deploy/trino-coordinator | grep -E 'SERVER STARTED|ERROR'
```

A healthy start ends with `======== SERVER STARTED ========` in the log of every pod.

Run a query from inside the coordinator pod. The image ships the CLI at `/usr/bin/trino`.

```bash
kubectl -n trino exec -it deploy/trino-coordinator -- trino
```

```sql
SELECT node_id, http_uri, node_version, coordinator, state FROM system.runtime.nodes;
SELECT count(*) FROM tpch.tiny.nation;
SHOW CATALOGS;
```

`system.runtime.nodes` must list the coordinator and every worker with state `active`. The count returns 25 while the default `tpch` catalog is present.

From your machine:

```bash
kubectl -n trino port-forward svc/trino 8080:8080
trino --server http://localhost:8080
```

The Web UI is at `http://localhost:8080/ui/`. Without authentication it accepts any user name.

## Change configuration

Edit the values file and run:

```bash
helm upgrade trino trino/trino --namespace trino --version 1.45.0 --values values.yaml
```

The pod templates carry checksums of the rendered configuration. A change to any Trino file restarts the affected pods. A coordinator restart fails all running queries. Workers restart as a rolling update, and queries on a restarted worker fail unless graceful shutdown or fault-tolerant execution is configured. See guide [14](14-upgrades-and-day-2.md) for safe procedures.

Check what a change will do first:

```bash
helm diff upgrade trino trino/trino -n trino --version 1.45.0 -f values.yaml   # needs the helm-diff plugin
helm get values trino -n trino          # values in effect
helm get manifest trino -n trino        # objects in effect
helm history trino -n trino
helm rollback trino <revision> -n trino
```

## Add a catalog

Catalogs are properties text under `catalogs`:

```yaml
catalogs:
  lake: |
    connector.name=hive
    hive.metastore=glue
    hive.metastore.glue.region=us-east-1
    fs.s3.enabled=true
    s3.region=us-east-1
```

The key is the catalog name users type in SQL. The file lands on every node as `/etc/trino/catalog/lake.properties`. With the default `catalog.management=static`, adding or changing a catalog restarts the cluster. Guide [06](06-catalogs-glue-s3.md) covers the data lake setup.

Never put a credential in `catalogs`. Reference an environment variable instead, and supply it from a Secret:

```yaml
envFrom:
  - secretRef:
      name: trino-catalog-secrets
catalogs:
  pg: |
    connector.name=postgresql
    connection-url=jdbc:postgresql://db.example.internal:5432/app
    connection-user=${ENV:PG_USER}
    connection-password=${ENV:PG_PASSWORD}
```

## Run the chart tests

```bash
helm test trino -n trino
```

The tests start short-lived pods that query the cluster. They pull images listed under `testImages`.

## Uninstall

```bash
helm uninstall trino -n trino
kubectl delete namespace trino
```

Nothing persists in the cluster. Data in object storage, Glue, and any exchange bucket is untouched. Clean the exchange bucket with a lifecycle rule.

## Local test cluster

The upstream page [installation/kubernetes.md](../upstream/trino/docs/installation/kubernetes.md) walks through a `kind` cluster. Use [examples/values-minimal.yaml](examples/values-minimal.yaml) with smaller memory numbers on a laptop.

## Checklist for a production install

1. Explicit `image.tag` and chart `--version`.
2. Memory requests equal to limits. Heap sized from the pod memory. See guide [05](05-memory-and-jvm-sizing.md).
3. `server.config.query.maxMemory` and both `maxMemoryPerNode` values set on purpose. The chart defaults are 4GB and 1GB, which are small.
4. A ServiceAccount with cloud permissions for the metastore and object storage. See guide [06](06-catalogs-glue-s3.md).
5. Coordinator on on-demand capacity with disruption protection. Workers spread across nodes.
6. Graceful shutdown for workers, and a PodDisruptionBudget. See guide [08](08-scaling-and-graceful-shutdown.md).
7. Authentication, a shared secret and access control before anyone outside the namespace can reach the Service. See guide [11](11-security.md).
8. Resource groups so one user cannot fill the cluster. See guide [12](12-resource-groups-and-session-properties.md).
9. Metrics scraped and alerts defined. See guide [13](13-observability.md).
