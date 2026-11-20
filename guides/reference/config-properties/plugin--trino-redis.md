# Configuration properties: `plugin/trino-redis`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [RedisConnectorConfig](#redisconnectorconfig), [RedisTlsConfig](#redistlsconfig)

## RedisConnectorConfig

Source: `plugin/trino-redis/src/main/java/io/trino/plugin/redis/RedisConnectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `redis.connect-timeout` | String | `new Duration(2000, MILLISECONDS)` | Timeout to connect to Redis |  |
| `redis.database-index` | int | `0` | Index of the Redis DB to connect to |  |
| `redis.default-schema` | String | `"default"` | The schema name to use in the connector |  |
| `redis.hide-internal-columns` | boolean | `true` | Whether internal columns are shown in table metadata or not. Default is no |  |
| `redis.key-delimiter` | String | `':'` | Delimiter for separating schema name and table name in the KEY prefix |  |
| `redis.key-prefix-schema-table` | boolean | `false` | Whether Redis key string follows "schema:table:*" format |  |
| `redis.max-keys-per-fetch` | int | `100` | Get values associated with the specified number of keys in the command such as MGET(key...) |  |
| `redis.nodes` | List<String> | `ImmutableSet.of()` | Seed nodes for Redis cluster. At least one must exist |  |
| `redis.password` | String |  | Password for a password-protected Redis server | sensitive |
| `redis.scan-count` | int | `100` | Count parameter for Redis scan command |  |
| `redis.table-description-cache-ttl` | Duration | `new Duration(5, MINUTES)` | The cache time for redis table description files |  |
| `redis.table-description-dir` | File | `new File("etc/redis/")` | Folder holding the JSON description files for Redis values |  |
| `redis.table-names` | Set<String> | `ImmutableSet.of()` | Set of tables known to this connector. For each table, a description file may be present in the catalog folder which describes columns for the given table |  |
| `redis.tls.enabled` | boolean | `false` | Whether TLS security is enabled |  |
| `redis.user` | String |  | Username for a Redis server |  |

## RedisTlsConfig

Source: `plugin/trino-redis/src/main/java/io/trino/plugin/redis/tls/RedisTlsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `redis.tls.keystore-password` | String |  | Password for the key store | sensitive |
| `redis.tls.keystore-path` | File |  | Path to the JKS or PKCS12 key store file |  |
| `redis.tls.truststore-password` | String |  | Password for the trust store | sensitive |
| `redis.tls.truststore-path` | File |  | Path to the JKS or PKCS12 trust store file |  |
