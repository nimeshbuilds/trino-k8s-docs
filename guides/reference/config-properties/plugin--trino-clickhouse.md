# Configuration properties: `plugin/trino-clickhouse`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [ClickHouseConfig](#clickhouseconfig)

## ClickHouseConfig

Source: `plugin/trino-clickhouse/src/main/java/io/trino/plugin/clickhouse/ClickHouseConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `clickhouse.cluster-name` | String | `Optional.empty()` | Name of the ClickHouse cluster on which DDL statements are executed |  |
| `clickhouse.map-string-as-varchar` | boolean | `false` | Map ClickHouse String and FixedString as varchar instead of varbinary |  |

Removed properties, rejected at startup if still set: `clickhouse.legacy-driver`
