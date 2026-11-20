# Configuration properties: `plugin/trino-kafka`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [ConfluentSchemaRegistryConfig](#confluentschemaregistryconfig), [FileTableDescriptionSupplierConfig](#filetabledescriptionsupplierconfig), [KafkaConfig](#kafkaconfig), [KafkaSecurityConfig](#kafkasecurityconfig), [KafkaSslConfig](#kafkasslconfig), [ProtobufAnySupportConfig](#protobufanysupportconfig)

## ConfluentSchemaRegistryConfig

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/schema/confluent/ConfluentSchemaRegistryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `kafka.confluent-schema-registry-client-cache-size` | int | `1000` | The maximum number of subjects that can be stored in the Confluent Schema Registry client cache |  |
| `kafka.confluent-schema-registry-url` | Set<String> | `ImmutableSet.of()` | The url of the Confluent Schema Registry |  |
| `kafka.confluent-subjects-cache-refresh-interval` | Duration | `new Duration(1, SECONDS)` | The interval that the topic to subjects cache will be refreshed |  |
| `kafka.empty-field-strategy` | EmptyFieldStrategy | `IGNORE` | How to handle struct types with no fields: ignore, add a marker field named '$empty_field_marker' or fail the query |  |

## FileTableDescriptionSupplierConfig

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/schema/file/FileTableDescriptionSupplierConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `kafka.table-description-dir` | File | `new File("etc/kafka/")` | Folder holding JSON description files for Kafka topics |  |
| `kafka.table-names` | Set<String> | `ImmutableSet.of()` | Set of tables known to this connector |  |

## KafkaConfig

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/KafkaConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `kafka.buffer-size` | String | `DataSize.of(64, Unit.KILOBYTE)` | Kafka message consumer buffer size |  |
| `kafka.config.resources` | List<String> | `ImmutableList.of()` | Optional config files |  |
| `kafka.default-schema` | String | `"default"` | Schema name to use in the connector |  |
| `kafka.hide-internal-columns` | boolean | `true` | Whether internal columns are shown in table metadata or not. Default is no |  |
| `kafka.internal-column-prefix` | String | `"_"` | Prefix for internal columns |  |
| `kafka.messages-per-split` | int | `100_000` | Count of Kafka messages to be processed by single Trino Kafka connector split |  |
| `kafka.nodes` | Set<String> | `ImmutableSet.of()` | Seed nodes for Kafka cluster. At least one must exist |  |
| `kafka.table-description-supplier` | String | `FileTableDescriptionSupplier.NAME` | The table description supplier to use, default is FILE |  |
| `kafka.timestamp-upper-bound-force-push-down-enabled` | boolean | `false` | timestamp upper bound force pushing down enabled |  |

Removed properties, rejected at startup if still set: `kafka.connect-timeout`

## KafkaSecurityConfig

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/KafkaSecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `kafka.security-protocol` | SecurityProtocol |  | Kafka communication security protocol |  |

## KafkaSslConfig

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/security/KafkaSslConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `kafka.ssl.endpoint-identification-algorithm` | KafkaEndpointIdentificationAlgorithm | `HTTPS` | The endpoint identification algorithm to validate server hostname using server certificate |  |
| `kafka.ssl.key.password` | String |  | The password of the private key in the key store file | sensitive |
| `kafka.ssl.keystore.location` | String |  | The location of the key store file. This can be used for two-way authentication for client |  |
| `kafka.ssl.keystore.password` | String |  | The store password for the key store file | sensitive |
| `kafka.ssl.keystore.type` | KafkaKeystoreTruststoreType | `JKS` | The file format of the key store file |  |
| `kafka.ssl.truststore.location` | String |  | The location of the trust store file |  |
| `kafka.ssl.truststore.password` | String |  | The password for the trust store file | sensitive |
| `kafka.ssl.truststore.type` | KafkaKeystoreTruststoreType | `JKS` | The file format of the trust store file |  |

## ProtobufAnySupportConfig

Source: `plugin/trino-kafka/src/main/java/io/trino/plugin/kafka/schema/ProtobufAnySupportConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `kafka.protobuf-any-support-enabled` | boolean | `false` | True to enable supporting encoding google.protobuf.Any types as JSON |  |
