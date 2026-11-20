# Configuration properties: `plugin/trino-pinot`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [PinotAuthenticationTypeConfig](#pinotauthenticationtypeconfig), [PinotConfig](#pinotconfig), [PinotGrpcServerQueryClientConfig](#pinotgrpcserverqueryclientconfig), [PinotGrpcServerQueryClientTlsConfig](#pinotgrpcserverqueryclienttlsconfig), [PinotPasswordBrokerAuthenticationConfig](#pinotpasswordbrokerauthenticationconfig), [PinotPasswordControllerAuthenticationConfig](#pinotpasswordcontrollerauthenticationconfig)

## PinotAuthenticationTypeConfig

Source: `plugin/trino-pinot/src/main/java/io/trino/plugin/pinot/auth/PinotAuthenticationTypeConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `pinot.broker.authentication.type` | PinotAuthenticationType | `PinotAuthenticationType.NONE` |  |  |
| `pinot.controller.authentication.type` | PinotAuthenticationType | `PinotAuthenticationType.NONE` |  |  |

## PinotConfig

Source: `plugin/trino-pinot/src/main/java/io/trino/plugin/pinot/PinotConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `pinot.aggregation-pushdown.enabled` | boolean | `true` |  |  |
| `pinot.broker-url` | HostAndPort | `Optional.empty()` | Provide global broker host and port. Setting this property will disable broker discovery mechanism. |  |
| `pinot.connection-timeout` | Duration | `new Duration(1, TimeUnit.MINUTES)` |  |  |
| `pinot.controller-urls` | List<String> | `ImmutableList.of()` |  |  |
| `pinot.count-distinct-pushdown.enabled` | boolean | `true` | Controls whether distinct count is pushed down to Pinot. Distinct count pushdown can cause Pinot to do a full scan. Aggregation pushdown must also be enabled in addition to this parameter otherwise no pushdowns will be enabled. |  |
| `pinot.fetch-retry-count` | int | `2` |  |  |
| `pinot.forbid-segment-queries` | boolean | `false` |  |  |
| `pinot.max-rows-for-broker-queries` | int | `50_000` |  |  |
| `pinot.metadata-expiry` | Duration | `new Duration(2, TimeUnit.MINUTES)` |  |  |
| `pinot.non-aggregate-limit-for-broker-queries` | int | `25_000` |  |  |
| `pinot.prefer-broker-queries` | boolean | `false` |  |  |
| `pinot.proxy.enabled` | boolean | `false` |  |  |
| `pinot.segments-per-split` | int | `1` |  |  |
| `pinot.target-segment-page-size` | DataSize | `DataSize.of(1, MEGABYTE)` |  |  |

Removed properties, rejected at startup if still set: `pinot.estimated-size-in-bytes-for-non-numeric-column`, `pinot.grpc.enabled`, `pinot.idle-timeout`, `pinot.max-backlog-per-server`, `pinot.max-connections-per-server`, `pinot.min-connections-per-server`, `pinot.request-timeout`, `pinot.thread-pool-size`

## PinotGrpcServerQueryClientConfig

Source: `plugin/trino-pinot/src/main/java/io/trino/plugin/pinot/client/PinotGrpcServerQueryClientConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `pinot.grpc.max-inbound-message-size` | DataSize | `DataSize.ofBytes(DEFAULT_MAX_INBOUND_MESSAGE_BYTES_SIZE)` |  |  |
| `pinot.grpc.port` | int | `8090` |  |  |
| `pinot.grpc.proxy-uri` | String | `Optional.empty()` |  |  |
| `pinot.grpc.use-plain-text` | boolean | `true` |  |  |
| `pinot.max-rows-per-split-for-segment-queries` | int | `Integer.MAX_VALUE - 1` |  |  |

## PinotGrpcServerQueryClientTlsConfig

Source: `plugin/trino-pinot/src/main/java/io/trino/plugin/pinot/client/PinotGrpcServerQueryClientTlsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `pinot.grpc.tls.keystore-password` | String |  |  | sensitive |
| `pinot.grpc.tls.keystore-path` | File |  |  |  |
| `pinot.grpc.tls.keystore-type` | PinotKeystoreTrustStoreType | `JKS` |  |  |
| `pinot.grpc.tls.ssl-provider` | String | `"JDK"` |  |  |
| `pinot.grpc.tls.truststore-password` | String |  |  | sensitive |
| `pinot.grpc.tls.truststore-path` | File |  |  |  |
| `pinot.grpc.tls.truststore-type` | PinotKeystoreTrustStoreType | `JKS` |  |  |

## PinotPasswordBrokerAuthenticationConfig

Source: `plugin/trino-pinot/src/main/java/io/trino/plugin/pinot/auth/password/inline/PinotPasswordBrokerAuthenticationConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `pinot.broker.authentication.password` | String |  |  | sensitive |
| `pinot.broker.authentication.user` | String |  |  |  |

## PinotPasswordControllerAuthenticationConfig

Source: `plugin/trino-pinot/src/main/java/io/trino/plugin/pinot/auth/password/inline/PinotPasswordControllerAuthenticationConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `pinot.controller.authentication.password` | String |  |  | sensitive |
| `pinot.controller.authentication.user` | String |  |  |  |
