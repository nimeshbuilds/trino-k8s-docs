# Configuration properties: `plugin/trino-cassandra`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [CassandraClientConfig](#cassandraclientconfig), [CassandraPasswordConfig](#cassandrapasswordconfig), [CassandraTlsConfig](#cassandratlsconfig)

## CassandraClientConfig

Source: `plugin/trino-cassandra/src/main/java/io/trino/plugin/cassandra/CassandraClientConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `cassandra.allow-drop-table` | boolean | `false` | Allow Cassandra connector to drop table |  |
| `cassandra.batch-size` | int | `100` |  |  |
| `cassandra.client.connect-timeout` | Duration | `new Duration(5_000, MILLISECONDS)` |  |  |
| `cassandra.client.read-timeout` | Duration | `new Duration(12_000, MILLISECONDS)` |  |  |
| `cassandra.client.so-linger` | Integer |  |  |  |
| `cassandra.consistency-level` | DefaultConsistencyLevel | `ConsistencyLevel.ONE` |  |  |
| `cassandra.contact-points` | List<String> | `ImmutableList.of()` |  |  |
| `cassandra.fetch-size` | int | `5_000` |  |  |
| `cassandra.load-policy.dc-aware.allow-remote-dc-for-local` | boolean | `false` |  |  |
| `cassandra.load-policy.dc-aware.local-dc` | String |  |  |  |
| `cassandra.load-policy.dc-aware.used-hosts-per-remote-dc` | Integer |  |  |  |
| `cassandra.load-policy.use-dc-aware` | boolean | `true` |  |  |
| `cassandra.native-protocol-port` | int | `9042` |  |  |
| `cassandra.no-host-available-retry-timeout` | Duration | `new Duration(1, MINUTES)` |  |  |
| `cassandra.partition-size-for-batch-select` | int | `100` |  |  |
| `cassandra.protocol-version` | DefaultProtocolVersion |  |  |  |
| `cassandra.retry-policy` | RetryPolicyType | `RetryPolicyType.DEFAULT` |  |  |
| `cassandra.security` | CassandraAuthenticationType | `NONE` |  |  |
| `cassandra.speculative-execution.delay` | Duration | `new Duration(500, MILLISECONDS)` |  |  |
| `cassandra.speculative-execution.limit` | Integer | `Optional.empty()` |  |  |
| `cassandra.split-size` | int | `1_024` |  |  |
| `cassandra.splits-per-node` | Long |  |  |  |
| `cassandra.tls.enabled` | boolean | `false` |  |  |

Removed properties, rejected at startup if still set: `cassandra.load-policy.allowed-addresses`, `cassandra.load-policy.token-aware.shuffle-replicas`, `cassandra.load-policy.use-token-aware`, `cassandra.load-policy.use-white-list`, `cassandra.load-policy.white-list.addresses`, `cassandra.max-schema-refresh-threads`, `cassandra.no-host-available-retry-count`, `cassandra.partitioner`, `cassandra.schema-cache-ttl`, `cassandra.schema-refresh-interval`, `cassandra.thrift-connection-factory-class`, `cassandra.thrift-port`, `cassandra.transport-factory-options`

## CassandraPasswordConfig

Source: `plugin/trino-cassandra/src/main/java/io/trino/plugin/cassandra/CassandraPasswordConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `cassandra.password` | String |  |  | sensitive |
| `cassandra.username` | String |  |  |  |

## CassandraTlsConfig

Source: `plugin/trino-cassandra/src/main/java/io/trino/plugin/cassandra/tls/CassandraTlsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `cassandra.tls.keystore-password` | String |  |  | sensitive |
| `cassandra.tls.keystore-path` | File |  |  |  |
| `cassandra.tls.truststore-password` | String |  |  | sensitive |
| `cassandra.tls.truststore-path` | File |  |  |  |
