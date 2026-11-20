# Configuration properties: `plugin/trino-jmx`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [JmxConnectorConfig](#jmxconnectorconfig)

## JmxConnectorConfig

Source: `plugin/trino-jmx/src/main/java/io/trino/plugin/jmx/JmxConnectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `jmx.dump-period` | Duration | `new Duration(10, SECONDS)` |  |  |
| `jmx.dump-tables` | String | `ImmutableSet.of()` |  |  |
| `jmx.max-entries` | int | `24 * 60 * 60` |  |  |
