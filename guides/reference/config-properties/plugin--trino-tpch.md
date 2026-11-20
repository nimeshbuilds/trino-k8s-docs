# Configuration properties: `plugin/trino-tpch`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [TpchConfig](#tpchconfig)

## TpchConfig

Source: `plugin/trino-tpch/src/main/java/io/trino/plugin/tpch/TpchConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `tpch.column-naming` | ColumnNaming | `ColumnNaming.SIMPLIFIED` |  |  |
| `tpch.double-type-mapping` | DecimalTypeMapping | `DecimalTypeMapping.DOUBLE` |  |  |
| `tpch.max-rows-per-page` | int | `1_000_000` |  |  |
| `tpch.partitioning-enabled` | boolean | `true` |  |  |
| `tpch.predicate-pushdown-enabled` | boolean | `true` |  |  |
| `tpch.splits-per-node` | int | `0` |  |  |
| `tpch.table-scan-redirection-catalog` | String |  |  |  |
| `tpch.table-scan-redirection-schema` | String |  |  |  |

Removed properties, rejected at startup if still set: `tpch.produce-pages`
