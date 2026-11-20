# Examples

Files to copy and adapt. All target the `trino` chart 1.45.0 with Trino 484.

| File | Purpose | Guide |
|---|---|---|
| [values-minimal.yaml](values-minimal.yaml) | Small cluster with explicit resources and memory | [02](../02-install-with-helm.md) |
| [values-eks-glue-s3.yaml](values-eks-glue-s3.yaml) | Hive and Iceberg catalogs on Glue and S3, IAM through the ServiceAccount, graceful shutdown, metrics | [06](../06-catalogs-glue-s3.md), [05](../05-memory-and-jvm-sizing.md) |
| [values-fault-tolerant.yaml](values-fault-tolerant.yaml) | Task retries with an S3 exchange. Layer on another file. | [09](../09-fault-tolerant-execution.md) |
| [values-autoscaling-keda.yaml](values-autoscaling-keda.yaml) | KEDA triggers on query metrics. Layer on another file. | [08](../08-scaling-and-graceful-shutdown.md) |
| [values-secured.yaml](values-secured.yaml) | Password authentication, shared secret, file-based access control, NetworkPolicy. Layer on another file. | [11](../11-security.md) |
| [resource-groups.json](resource-groups.json) | Resource groups for interactive, scheduled, automation and admin workloads | [12](../12-resource-groups-and-session-properties.md) |
| [session-property-config.json](session-property-config.json) | Default session properties per resource group | [12](../12-resource-groups-and-session-properties.md) |
| [access-control-rules.json](access-control-rules.json) | File-based system access control rules | [11](../11-security.md) |

## Layering

```bash
helm upgrade --install trino trino/trino -n trino --version 1.45.0 \
  -f values-eks-glue-s3.yaml \
  -f values-fault-tolerant.yaml \
  -f values-autoscaling-keda.yaml \
  -f values-secured.yaml
```

Helm merges maps and replaces lists. `additionalConfigProperties` is a list, so with several files only the last file's list survives. When you layer files that each set it, merge those lists by hand into one final file. The same applies to `env` and `additionalExchangeManagerProperties`.

## Using the JSON files

```yaml
resourceGroups:
  type: configmap
  resourceGroupsConfig: |-
    <contents of resource-groups.json>

sessionProperties:
  type: configmap
  sessionPropertiesConfig: |-
    <contents of session-property-config.json>

accessControl:
  type: configmap
  refreshPeriod: 60s
  configFile: rules.json
  rules:
    rules.json: |-
      <contents of access-control-rules.json>
```

Or keep the JSON as files and load them with `--set-file`:

```bash
helm upgrade trino trino/trino -n trino --version 1.45.0 -f values.yaml \
  --set resourceGroups.type=configmap \
  --set-file resourceGroups.resourceGroupsConfig=resource-groups.json
```

## What was checked

- Every key in the values files exists in the default values of chart 1.45.0.
- Every Trino property in them exists in the 484 source or in the upstream documentation.
- The JSON files parse, and their field names match the upstream documentation.
- The files were not rendered with `helm template` in the environment where they were written. Render them yourself before applying, and replace regions, account IDs, bucket names, node selectors and sizes.
