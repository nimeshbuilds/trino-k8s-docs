# Configuration properties: `plugin/trino-postgresql`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [PostgreSqlConfig](#postgresqlconfig)

## PostgreSqlConfig

Source: `plugin/trino-postgresql/src/main/java/io/trino/plugin/postgresql/PostgreSqlConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `postgresql.array-mapping` | ArrayMapping | `ArrayMapping.DISABLED` |  | Legacy names: `postgresql.experimental.array-mapping` |
| `postgresql.experimental.enable-string-pushdown-with-collate` | boolean | `false` |  |  |
| `postgresql.fetch-size` | Integer |  | Postgresql fetch size, trino specific heuristic is applied if empty |  |
| `postgresql.include-system-tables` | boolean | `false` |  |  |

Removed properties, rejected at startup if still set: `postgresql.disable-automatic-fetch-size`
