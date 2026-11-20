# Configuration properties: `plugin/trino-mysql-event-listener`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [MysqlEventListenerConfig](#mysqleventlistenerconfig)

## MysqlEventListenerConfig

Source: `plugin/trino-mysql-event-listener/src/main/java/io/trino/plugin/eventlistener/mysql/MysqlEventListenerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `mysql-event-listener.db.url` | String |  |  | sensitive |
| `mysql-event-listener.terminate-on-initialization-failure` | boolean | `true` | The MySQL event listener initialization may fail if the database is unavailable. This flag determines whether an exception should be thrown in such cases. |  |
