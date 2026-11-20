# Configuration properties: `plugin/trino-thrift`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [ThriftConnectorConfig](#thriftconnectorconfig)

## ThriftConnectorConfig

Source: `plugin/trino-thrift/src/main/java/io/trino/plugin/thrift/ThriftConnectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `trino-thrift.lookup-requests-concurrency` | int | `1` |  |  |
| `trino-thrift.max-response-size` | DataSize | `DataSize.of(16, MEGABYTE)` |  |  |
| `trino-thrift.metadata-refresh-threads` | int | `1` |  |  |
