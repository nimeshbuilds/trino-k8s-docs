# Configuration properties: `plugin/trino-redshift`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [RedshiftConfig](#redshiftconfig)

## RedshiftConfig

Source: `plugin/trino-redshift/src/main/java/io/trino/plugin/redshift/RedshiftConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `redshift.fetch-size` | Integer |  | Redshift fetch size, trino specific heuristic is applied if empty |  |
| `redshift.unload-iam-role` | String |  | Fully specified ARN of the IAM Role attached to the Redshift cluster and having access to S3 |  |
| `redshift.unload-location` | String |  | A writeable location in Amazon S3, to be used for unloading Redshift query results |  |

Removed properties, rejected at startup if still set: `redshift.disable-automatic-fetch-size`, `redshift.use-legacy-type-mapping`
