# Configuration properties: `plugin/trino-oracle`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [OracleConfig](#oracleconfig)

## OracleConfig

Source: `plugin/trino-oracle/src/main/java/io/trino/plugin/oracle/OracleConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `oracle.connection-pool.enabled` | boolean | `true` |  |  |
| `oracle.connection-pool.inactive-timeout` | Duration | `new Duration(20, MINUTES)` | How long a connection in the pool can remain idle before it is closed |  |
| `oracle.connection-pool.max-size` | int | `30` |  |  |
| `oracle.connection-pool.min-size` | int | `1` |  |  |
| `oracle.connection-pool.wait-duration` | Duration | `new Duration(3, SECONDS)` | Maximum amount of time a request will wait to obtain an available connection from the pool if all connections are currently in use |  |
| `oracle.fetch-size` | Integer |  | Oracle fetch size, trino specific heuristic is applied if empty |  |
| `oracle.number.default-scale` | Integer |  | Default Trino DECIMAL scale for Oracle NUMBER data type | deprecated |
| `oracle.number.rounding-mode` | RoundingMode | `Optional.empty()` |  | deprecated |
| `oracle.remarks-reporting.enabled` | boolean | `false` |  |  |
| `oracle.synonyms.enabled` | boolean | `false` |  |  |

Removed properties, rejected at startup if still set: `oracle.disable-automatic-fetch-size`
