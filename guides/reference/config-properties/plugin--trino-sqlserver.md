# Configuration properties: `plugin/trino-sqlserver`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [SqlServerConfig](#sqlserverconfig)

## SqlServerConfig

Source: `plugin/trino-sqlserver/src/main/java/io/trino/plugin/sqlserver/SqlServerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `sqlserver.bulk-copy-for-write.enabled` | boolean | `false` | Use SQL Server Bulk Copy API for writes |  |
| `sqlserver.bulk-copy-for-write.lock-destination-table` | boolean | `false` | Obtain a Bulk Update lock on destination table on write |  |
| `sqlserver.snapshot-isolation.disabled` | boolean | `false` | Disables automatic use of snapshot isolation for transactions issued by Trino in SQL Server |  |
| `sqlserver.stored-procedure-table-function-enabled` | boolean | `false` | Allows accessing Stored procedure as a table function | Legacy names: `sqlserver.experimental.stored-procedure-table-function-enabled` |
