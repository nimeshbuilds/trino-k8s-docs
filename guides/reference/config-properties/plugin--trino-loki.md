# Configuration properties: `plugin/trino-loki`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [LokiConfig](#lokiconfig)

## LokiConfig

Source: `plugin/trino-loki/src/main/java/io/trino/plugin/loki/LokiConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `loki.query-timeout` | Duration | `new Duration(10, SECONDS)` | How much time a query to Loki has before timing out |  |
| `loki.uri` | URI |  | Loki API endpoint base |  |
