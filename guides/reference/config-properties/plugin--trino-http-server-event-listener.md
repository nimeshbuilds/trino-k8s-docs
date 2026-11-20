# Configuration properties: `plugin/trino-http-server-event-listener`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [HttpServerEventListenerConfig](#httpservereventlistenerconfig)

## HttpServerEventListenerConfig

Source: `plugin/trino-http-server-event-listener/src/main/java/io/trino/plugin/httpquery/HttpServerEventListenerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-server-event-listener.event-buffer-size` | int | `50` | Event buffer size |  |
| `http-server-event-listener.event-ttl` | Duration | `new Duration(10, TimeUnit.MINUTES)` | Event TTL |  |
