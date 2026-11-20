# Configuration properties: `plugin/trino-prometheus`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [PrometheusConnectorConfig](#prometheusconnectorconfig)

## PrometheusConnectorConfig

Source: `plugin/trino-prometheus/src/main/java/io/trino/plugin/prometheus/PrometheusConnectorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `prometheus.auth.http.header.name` | String | `HttpHeaders.AUTHORIZATION` | Name of the HTTP header to use for authorization |  |
| `prometheus.auth.password` | String |  |  | sensitive |
| `prometheus.auth.user` | String |  |  |  |
| `prometheus.bearer.token.file` | File |  | File holding bearer token if needed for access to Prometheus |  |
| `prometheus.cache.ttl` | Duration | `new Duration(30, TimeUnit.SECONDS)` | How long values from this config file are cached |  |
| `prometheus.case-insensitive-name-matching` | boolean | `false` | Where to match the prometheus metric name case insensitively |  |
| `prometheus.http.additional-headers` | String | `ImmutableMap.of()` | Comma separated key:value pairs to be sent with the HTTP request to Prometheus as additional headers |  |
| `prometheus.max.query.range.duration` | Duration | `new Duration(21, TimeUnit.DAYS)` | Width of overall query to Prometheus, will be divided into prometheus.query.chunk.size.duration queries |  |
| `prometheus.query.chunk.size.duration` | Duration | `new Duration(1, TimeUnit.DAYS)` | The duration of each query to Prometheus |  |
| `prometheus.read-timeout` | Duration | `new Duration(10, TimeUnit.SECONDS)` | How much time a query to Prometheus has before timing out |  |
| `prometheus.uri` | URI | `URI.create("http://localhost:9090")` | Where to find Prometheus coordinator host |  |
