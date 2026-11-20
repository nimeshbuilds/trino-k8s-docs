# Configuration properties: `plugin/trino-openlineage`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [OpenLineageHttpTransportConfig](#openlineagehttptransportconfig), [OpenLineageListenerConfig](#openlineagelistenerconfig), [OpenLineageTransportConfig](#openlineagetransportconfig)

## OpenLineageHttpTransportConfig

Source: `plugin/trino-openlineage/src/main/java/io/trino/plugin/openlineage/transport/http/OpenLineageHttpTransportConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `openlineage-event-listener.transport.api-key` | String | `Optional.empty()` | API Key to use when authenticating against OpenLineage API | sensitive |
| `openlineage-event-listener.transport.compression` | Compression | `Compression.NONE` | Compression codec using for HTTP body |  |
| `openlineage-event-listener.transport.endpoint` | String |  | Custom path for API receiving the events. |  |
| `openlineage-event-listener.transport.headers` | List<String> | `new HashMap<>()` | List of custom custom HTTP headers provided as: "Header-Name-1: header value 1, Header-Value-2: header value 2, ..." |  |
| `openlineage-event-listener.transport.timeout` | Duration | `new Duration(5000, TimeUnit.MILLISECONDS)` | Timeout when making HTTP Requests. |  |
| `openlineage-event-listener.transport.url` | URI |  | URL of receiving server. Explicitly set the scheme https:// to use symmetric encryption |  |
| `openlineage-event-listener.transport.url-params` | List<String> | `new HashMap<>()` | List of custom custom url params provided as: "url-param-1: url param value 1, ..." |  |

## OpenLineageListenerConfig

Source: `plugin/trino-openlineage/src/main/java/io/trino/plugin/openlineage/OpenLineageListenerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `openlineage-event-listener.disabled-facets` | Set<OpenLineageTrinoFacet> | `ImmutableSet.of()` | Which facets should be removed from OpenLineage events |  |
| `openlineage-event-listener.job.name-format` | String | `"$QUERY_ID"` | Set name format for OpenLineage job |  |
| `openlineage-event-listener.namespace` | String | `Optional.empty()` | Override default namespace for OpenLineage job |  |
| `openlineage-event-listener.trino.include-query-types` | Set<QueryType> | `ImmutableSet.<QueryType>builder() .add(QueryType.ALTER_TABLE_EXECUTE) .add(QueryType.DELETE) .add(QueryType.INSERT) .add(QueryType.MERGE) .add(QueryType.UPDATE) .add(QueryType.DATA_DEFINITION) .build()` | Which query types emitted by Trino should generate OpenLineage events. Other query types will be filtered out |  |
| `openlineage-event-listener.trino.uri` | URI |  | URI of trino server. Used for namespace rendering |  |

## OpenLineageTransportConfig

Source: `plugin/trino-openlineage/src/main/java/io/trino/plugin/openlineage/transport/OpenLineageTransportConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `openlineage-event-listener.transport.type` | OpenLineageTransport | `OpenLineageTransport.CONSOLE` | Type of transport used to emit lineage information |  |
