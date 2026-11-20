# Configuration properties: `plugin/trino-mysql`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [MySqlConfig](#mysqlconfig)

## MySqlConfig

Source: `plugin/trino-mysql/src/main/java/io/trino/plugin/mysql/MySqlConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `mysql.auto-reconnect` | boolean | `true` |  |  |
| `mysql.connection-timeout` | Duration | `new Duration(10, TimeUnit.SECONDS)` |  |  |
| `mysql.jdbc.use-information-schema` | boolean | `true` | Value of useInformationSchema MySQL JDBC driver connection property |  |
| `mysql.max-reconnects` | int | `3` |  |  |
