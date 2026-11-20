# Configuration properties: `plugin/trino-exchange-hdfs`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [ExchangeHdfsConfig](#exchangehdfsconfig)

## ExchangeHdfsConfig

Source: `plugin/trino-exchange-hdfs/src/main/java/io/trino/plugin/exchange/hdfs/ExchangeHdfsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `exchange.hdfs.block-size` | DataSize | `DataSize.of(4, MEGABYTE)` | Block size for HDFS storage |  |
| `exchange.hdfs.skip-directory-scheme-validation` | boolean | `false` | Skip directory scheme validation to support hadoop compatible file system |  |
| `hdfs.config.resources` | List<String> | `ImmutableList.of()` |  |  |
