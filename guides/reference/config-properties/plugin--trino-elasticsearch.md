# Configuration properties: `plugin/trino-elasticsearch`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [AwsSecurityConfig](#awssecurityconfig), [ElasticsearchConfig](#elasticsearchconfig), [PasswordConfig](#passwordconfig)

## AwsSecurityConfig

Source: `plugin/trino-elasticsearch/src/main/java/io/trino/plugin/elasticsearch/AwsSecurityConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `elasticsearch.aws.access-key` | String |  |  |  |
| `elasticsearch.aws.external-id` | String |  | Optional external id to pass to AWS STS while assuming a role |  |
| `elasticsearch.aws.iam-role` | String |  | Optional AWS IAM role to assume for authenticating. If set, this role will be used to get credentials to sign requests to ES. |  |
| `elasticsearch.aws.region` | String |  |  |  |
| `elasticsearch.aws.secret-key` | String |  |  | sensitive |

Removed properties, rejected at startup if still set: `elasticsearch.aws.use-instance-credentials`

## ElasticsearchConfig

Source: `plugin/trino-elasticsearch/src/main/java/io/trino/plugin/elasticsearch/ElasticsearchConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `elasticsearch.aggregation-page-size` | int | `10_000` | Maximum number of buckets returned with each aggregation request |  |
| `elasticsearch.backoff-init-delay` | Duration | `new Duration(500, MILLISECONDS)` | Initial delay to wait between backpressure retries |  |
| `elasticsearch.backoff-max-delay` | Duration | `new Duration(20, SECONDS)` | Maximum delay to wait between backpressure retries |  |
| `elasticsearch.connect-timeout` | Duration | `new Duration(1, SECONDS)` | Elasticsearch connect timeout |  |
| `elasticsearch.default-schema-name` | String | `"default"` | Default schema name to use |  |
| `elasticsearch.host` | List<String> |  |  |  |
| `elasticsearch.http-thread-count` | int | `Runtime.getRuntime().availableProcessors()` | Number of threads handling HTTP connections to Elasticsearch |  |
| `elasticsearch.ignore-publish-address` | boolean | `false` |  |  |
| `elasticsearch.max-http-connections` | int | `25` | Maximum number of persistent HTTP connections to Elasticsearch |  |
| `elasticsearch.max-retry-time` | Duration | `new Duration(30, SECONDS)` | Maximum timeout in case of multiple retries |  |
| `elasticsearch.node-refresh-interval` | Duration | `new Duration(1, MINUTES)` | How often to refresh the list of available Elasticsearch nodes |  |
| `elasticsearch.port` | int | `9200` |  |  |
| `elasticsearch.request-timeout` | Duration | `new Duration(10, SECONDS)` | Elasticsearch request timeout |  |
| `elasticsearch.scroll-size` | int | `1_000` | Scroll batch size |  |
| `elasticsearch.scroll-timeout` | Duration | `new Duration(1, MINUTES)` | Scroll timeout |  |
| `elasticsearch.security` | Security |  |  |  |
| `elasticsearch.tls.enabled` | boolean | `false` |  |  |
| `elasticsearch.tls.keystore-password` | String |  |  | sensitive |
| `elasticsearch.tls.keystore-path` | File |  |  |  |
| `elasticsearch.tls.truststore-password` | String |  |  | sensitive |
| `elasticsearch.tls.truststore-path` | File |  |  |  |
| `elasticsearch.tls.verify-hostnames` | boolean | `true` |  |  |

Removed properties, rejected at startup if still set: `elasticsearch.cluster-name`, `elasticsearch.max-hits`, `elasticsearch.max-request-retries`, `elasticsearch.max-request-retry-time`, `elasticsearch.table-description-directory`, `searchguard.ssl.certificate-format`, `searchguard.ssl.keystore-filepath`, `searchguard.ssl.keystore-password`, `searchguard.ssl.pemcert-filepath`, `searchguard.ssl.pemkey-filepath`, `searchguard.ssl.pemkey-password`, `searchguard.ssl.pemtrustedcas-filepath`, `searchguard.ssl.truststore-filepath`, `searchguard.ssl.truststore-password`

## PasswordConfig

Source: `plugin/trino-elasticsearch/src/main/java/io/trino/plugin/elasticsearch/PasswordConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `elasticsearch.auth.password` | String |  |  | sensitive |
| `elasticsearch.auth.user` | String |  |  |  |
