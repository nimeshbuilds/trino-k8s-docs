# Configuration properties: `plugin/trino-http-event-listener`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [HttpEventListenerConfig](#httpeventlistenerconfig)

## HttpEventListenerConfig

Source: `plugin/trino-http-event-listener/src/main/java/io/trino/plugin/httpquery/HttpEventListenerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `http-event-listener.connect-backoff-base` | double | `2.0` | Base used for exponential backoff when retying on server error. Formula is attemptDelay = retryDelay * backoffBase ^ attemptCount. Attempt counting starts from 0. Leave empty or set to 1 to disable. |  |
| `http-event-listener.connect-http-headers` | List<String> | `ImmutableMap.of()` | List of custom custom HTTP headers provided as: "Header-Name-1: header value 1, Header-Value-2: header value 2, ..." |  |
| `http-event-listener.connect-http-method` | HttpEventListenerHttpMethod | `HttpEventListenerHttpMethod.POST` | Specifies the HTTP method to use for the request. Supported values are POST (default) and PUT. |  |
| `http-event-listener.connect-ingest-uri` | String |  | URL of receiving server. Explicitly set the scheme https:// to use symmetric encryption |  |
| `http-event-listener.connect-max-delay` | Duration | `Duration.valueOf("1m")` | Maximum delay between retries. This should be used with exponential backoff. |  |
| `http-event-listener.connect-retry-count` | int | `0` | Number of retries on server error |  |
| `http-event-listener.connect-retry-delay` | Duration | `Duration.valueOf("1s")` | Delay in seconds between retries |  |
| `http-event-listener.log-completed` | boolean |  | Will log io.trino.spi.eventlistener.QueryCompletedEvent |  |
| `http-event-listener.log-created` | boolean |  | Will log io.trino.spi.eventlistener.QueryCreatedEvent |  |

Removed properties, rejected at startup if still set: `http-event-listener.log-split`
