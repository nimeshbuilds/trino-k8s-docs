# Configuration properties: `plugin/trino-mongodb`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [MongoClientConfig](#mongoclientconfig), [MongoTlsConfig](#mongotlsconfig)

## MongoClientConfig

Source: `plugin/trino-mongodb/src/main/java/io/trino/plugin/mongodb/MongoClientConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `mongodb.allow-local-scheduling` | boolean | `false` | Assign MongoDB splits to a specific host if worker and MongoDB share the same cluster |  |
| `mongodb.case-insensitive-name-matching` | boolean | `false` |  |  |
| `mongodb.connection-timeout` | int | `10_000` |  |  |
| `mongodb.connection-url` | String |  |  | sensitive |
| `mongodb.connections-per-host` | int | `100` |  |  |
| `mongodb.cursor-batch-size` | int | `0` |  |  |
| `mongodb.dynamic-filtering.wait-timeout` | Duration | `new Duration(5, SECONDS)` | Duration to wait for completion of dynamic filters during split generation |  |
| `mongodb.implicit-row-field-prefix` | String | `"_pos"` |  | hidden |
| `mongodb.max-connection-idle-time` | int | `0` |  |  |
| `mongodb.max-wait-time` | int | `120_000` |  |  |
| `mongodb.min-connections-per-host` | int | `0` |  |  |
| `mongodb.projection-pushdown-enabled` | boolean | `true` | Read only required fields from a row type |  |
| `mongodb.read-preference` | ReadPreferenceType | `ReadPreferenceType.PRIMARY` |  |  |
| `mongodb.required-replica-set` | String |  |  |  |
| `mongodb.schema-collection` | String | `"_schema"` |  |  |
| `mongodb.socket-timeout` | int | `0` |  |  |
| `mongodb.tls.enabled` | boolean | `false` |  | Legacy names: `mongodb.ssl.enabled` |
| `mongodb.write-concern` | WriteConcernType | `WriteConcernType.ACKNOWLEDGED` |  |  |

Removed properties, rejected at startup if still set: `mongodb.connection-per-host`, `mongodb.credentials`, `mongodb.seeds`, `mongodb.socket-keep-alive`

## MongoTlsConfig

Source: `plugin/trino-mongodb/src/main/java/io/trino/plugin/mongodb/MongoTlsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `mongodb.tls.keystore-password` | String |  |  | sensitive |
| `mongodb.tls.keystore-path` | File |  |  |  |
| `mongodb.tls.truststore-password` | String |  |  | sensitive |
| `mongodb.tls.truststore-path` | File |  |  |  |
