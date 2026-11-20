# Configuration properties: `plugin/trino-kafka-event-listener`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [KafkaEventListenerConfig](#kafkaeventlistenerconfig)

## KafkaEventListenerConfig

Source: `plugin/trino-kafka-event-listener/src/main/java/io/trino/plugin/eventlistener/kafka/KafkaEventListenerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `kafka-event-listener.anonymization.enabled` | boolean | `false` |  |  |
| `kafka-event-listener.batch-size` | DataSize | `DataSize.of(16, KILOBYTE)` | Value that specifies the size to batch before sending records to Kafka |  |
| `kafka-event-listener.broker-endpoints` | String |  |  |  |
| `kafka-event-listener.client-id` | String | `Optional.empty()` |  |  |
| `kafka-event-listener.completed-event.topic` | String | `Optional.empty()` |  |  |
| `kafka-event-listener.config.resources` | List<String> | `ImmutableList.of()` | Optional config files |  |
| `kafka-event-listener.created-event.topic` | String | `Optional.empty()` |  |  |
| `kafka-event-listener.env-var-prefix` | String | `Optional.empty()` | When set, Kafka events will be sent with additional metadata populated from environment variables. E.g. if env-var-prefix is set to 'TRINO_INSIGHTS_' and there is an env var TRINO_INSIGHTS_CLUSTER_ID=foo, then Kafka payload metadata will contain CLUSTER_ID=foo. |  |
| `kafka-event-listener.excluded-fields` | Set<String> | `Collections.emptySet()` | Comma-separated list of field names to be excluded from the Kafka event (their value will be replaced with null). E.g.: 'payload,user' |  |
| `kafka-event-listener.max-request-size` | DataSize | `DataSize.of(5, MEGABYTE)` | The maximum size of a request/message in bytes |  |
| `kafka-event-listener.publish-completed-event` | boolean | `true` | Whether to publish io.trino.spi.eventlistener.QueryCompletedEvent |  |
| `kafka-event-listener.publish-created-event` | boolean | `true` | Whether to publish io.trino.spi.eventlistener.QueryCreatedEvent |  |
| `kafka-event-listener.request-timeout` | Duration | `new Duration(10, SECONDS)` | Timeout value to complete a kafka request. |  |
| `kafka-event-listener.terminate-on-initialization-failure` | boolean | `true` | Kafka publisher initialization might fail due to network issues reaching the Kafka brokers. This flag controls whether to throw an exception in such cases. |  |

Removed properties, rejected at startup if still set: `kafka-event-listener.client-config-overrides`, `kafka-event-listener.publish-split-completed-event`, `kafka-event-listener.split-completed-event.topic`
