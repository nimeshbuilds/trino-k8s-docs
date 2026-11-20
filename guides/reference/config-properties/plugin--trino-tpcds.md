# Configuration properties: `plugin/trino-tpcds`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [TpcdsConfig](#tpcdsconfig)

## TpcdsConfig

Source: `plugin/trino-tpcds/src/main/java/io/trino/plugin/tpcds/TpcdsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `tpcds.split-count` | Integer |  | Number of split to be created. If not specified the number of splits is computed as 'tpcds.splits-per-node * <number of active nodes>' |  |
| `tpcds.splits-per-node` | int | `Runtime.getRuntime().availableProcessors()` |  |  |
| `tpcds.with-no-sexism` | boolean | `false` |  |  |
