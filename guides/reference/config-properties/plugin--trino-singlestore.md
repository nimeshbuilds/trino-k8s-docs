# Configuration properties: `plugin/trino-singlestore`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [SingleStoreConfig](#singlestoreconfig)

## SingleStoreConfig

Source: `plugin/trino-singlestore/src/main/java/io/trino/plugin/singlestore/SingleStoreConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `singlestore.auto-reconnect` | boolean | `true` |  | Legacy names: `memsql.auto-reconnect` |
| `singlestore.connection-timeout` | Duration | `new Duration(10, TimeUnit.SECONDS)` |  | Legacy names: `memsql.connection-timeout` |
