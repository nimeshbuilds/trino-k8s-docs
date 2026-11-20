# Configuration properties: `plugin/trino-opensearch`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [AwsSecurityConfig](#awssecurityconfig), [OpenSearchConfig](#opensearchconfig), [PasswordConfig](#passwordconfig)

## AwsSecurityConfig

Source: `plugin/trino-opensearch/src/main/java/io/trino/plugin/opensearch/AwsSecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `opensearch.aws.access-key` | String |  |  |  |
| `opensearch.aws.deployment-type` | DeploymentType |  |  |  |
| `opensearch.aws.external-id` | String |  | Optional external id to pass to AWS STS while assuming a role |  |
| `opensearch.aws.iam-role` | String |  | Optional AWS IAM role to assume for authenticating. If set, this role will be used to get credentials to sign requests to ES. |  |
| `opensearch.aws.region` | String |  |  |  |
| `opensearch.aws.secret-key` | String |  |  | sensitive |

## OpenSearchConfig

Source: `plugin/trino-opensearch/src/main/java/io/trino/plugin/opensearch/OpenSearchConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `opensearch.backoff-init-delay` | Duration | `new Duration(500, MILLISECONDS)` | Initial delay to wait between backpressure retries |  |
| `opensearch.backoff-max-delay` | Duration | `new Duration(20, SECONDS)` | Maximum delay to wait between backpressure retries |  |
| `opensearch.connect-timeout` | Duration | `new Duration(1, SECONDS)` | OpenSearch connect timeout |  |
| `opensearch.default-schema-name` | String | `"default"` | Default schema name to use |  |
| `opensearch.host` | List<String> |  |  |  |
| `opensearch.http-thread-count` | int | `Runtime.getRuntime().availableProcessors()` | Number of threads handling HTTP connections to OpenSearch cluster |  |
| `opensearch.ignore-publish-address` | boolean | `false` |  |  |
| `opensearch.max-http-connections` | int | `25` | Maximum number of persistent HTTP connections to OpenSearch cluster |  |
| `opensearch.max-retry-time` | Duration | `new Duration(30, SECONDS)` | Maximum timeout in case of multiple retries |  |
| `opensearch.node-refresh-interval` | Duration | `new Duration(1, MINUTES)` | How often to refresh the list of available nodes in the OpenSearch cluster |  |
| `opensearch.port` | int | `9200` |  |  |
| `opensearch.projection-pushdown-enabled` | boolean | `true` | Read only required fields from a row type |  |
| `opensearch.request-timeout` | Duration | `new Duration(10, SECONDS)` | OpenSearch request timeout |  |
| `opensearch.scroll-size` | int | `1_000` | Scroll batch size |  |
| `opensearch.scroll-timeout` | Duration | `new Duration(1, MINUTES)` | Scroll timeout |  |
| `opensearch.security` | Security |  |  |  |
| `opensearch.tls.enabled` | boolean | `false` |  |  |
| `opensearch.tls.keystore-password` | String |  |  | sensitive |
| `opensearch.tls.keystore-path` | File |  |  |  |
| `opensearch.tls.truststore-password` | String |  |  | sensitive |
| `opensearch.tls.truststore-path` | File |  |  |  |
| `opensearch.tls.verify-hostnames` | boolean | `true` |  |  |

## PasswordConfig

Source: `plugin/trino-opensearch/src/main/java/io/trino/plugin/opensearch/PasswordConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `opensearch.auth.password` | String |  |  | sensitive |
| `opensearch.auth.user` | String |  |  |  |
