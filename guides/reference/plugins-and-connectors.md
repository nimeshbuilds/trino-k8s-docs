# Plugins and connectors from source

Every extension point implemented in Trino release 484, found by scanning the main Java sources for factory classes.

Use the **Name** as the value of the **Selected by** property. For a connector that is `connector.name` in `etc/catalog/<catalog>.properties`.

A name shown as "passed in by the plugin class" is assigned where the plugin registers the factory. Check the linked documentation page for the value.

| Module | Kind | Name | Selected by | Documentation | Config reference |
|---|---|---|---|---|---|
| `lib/trino-plugin-toolkit` | system access control | `allow-all` | `access-control.name` |  | [properties](config-properties/lib--trino-plugin-toolkit.md) |
| `lib/trino-plugin-toolkit` | system access control | `default` | `access-control.name` |  | [properties](config-properties/lib--trino-plugin-toolkit.md) |
| `lib/trino-plugin-toolkit` | system access control | `file` | `access-control.name` |  | [properties](config-properties/lib--trino-plugin-toolkit.md) |
| `lib/trino-plugin-toolkit` | system access control | `read-only` | `access-control.name` |  | [properties](config-properties/lib--trino-plugin-toolkit.md) |
| `plugin/trino-ai-functions` | connector | `ai` | `connector.name` |  | [properties](config-properties/plugin--trino-ai-functions.md) |
| `plugin/trino-bigquery` | connector | `bigquery` | `connector.name` | [connector/bigquery.md](../../upstream/trino/docs/connector/bigquery.md) | [properties](config-properties/plugin--trino-bigquery.md) |
| `plugin/trino-blackhole` | connector | `blackhole` | `connector.name` | [connector/blackhole.md](../../upstream/trino/docs/connector/blackhole.md) |  |
| `plugin/trino-blob-cache-alluxio` | cache manager | `alluxio` | `cache-manager.name` |  | [properties](config-properties/plugin--trino-blob-cache-alluxio.md) |
| `plugin/trino-blob-cache-memory` | cache manager | `memory` | `cache-manager.name` |  | [properties](config-properties/plugin--trino-blob-cache-memory.md) |
| `plugin/trino-cassandra` | connector | `cassandra` | `connector.name` | [connector/cassandra.md](../../upstream/trino/docs/connector/cassandra.md) | [properties](config-properties/plugin--trino-cassandra.md) |
| `plugin/trino-clickhouse` | connector | `clickhouse` | `connector.name` | [connector/clickhouse.md](../../upstream/trino/docs/connector/clickhouse.md) | [properties](config-properties/plugin--trino-clickhouse.md) |
| `plugin/trino-datasketches` | connector | `datasketches` | `connector.name` |  |  |
| `plugin/trino-delta-lake` | connector | `delta_lake` | `connector.name` | [connector/delta-lake.md](../../upstream/trino/docs/connector/delta-lake.md) | [properties](config-properties/plugin--trino-delta-lake.md) |
| `plugin/trino-druid` | connector | `druid` | `connector.name` | [connector/druid.md](../../upstream/trino/docs/connector/druid.md) | [properties](config-properties/plugin--trino-druid.md) |
| `plugin/trino-duckdb` | connector | `duckdb` | `connector.name` | [connector/duckdb.md](../../upstream/trino/docs/connector/duckdb.md) |  |
| `plugin/trino-elasticsearch` | connector | `elasticsearch` | `connector.name` | [connector/elasticsearch.md](../../upstream/trino/docs/connector/elasticsearch.md) | [properties](config-properties/plugin--trino-elasticsearch.md) |
| `plugin/trino-example-http` | connector | `example_http` | `connector.name` |  | [properties](config-properties/plugin--trino-example-http.md) |
| `plugin/trino-example-jdbc` | connector | `example_jdbc` | `connector.name` |  |  |
| `plugin/trino-exasol` | connector | `exasol` | `connector.name` | [connector/exasol.md](../../upstream/trino/docs/connector/exasol.md) |  |
| `plugin/trino-exchange-filesystem` | exchange manager | `filesystem` | `exchange-manager.name` |  | [properties](config-properties/plugin--trino-exchange-filesystem.md) |
| `plugin/trino-exchange-hdfs` | exchange manager | `hdfs` | `exchange-manager.name` |  | [properties](config-properties/plugin--trino-exchange-hdfs.md) |
| `plugin/trino-faker` | connector | `faker` | `connector.name` | [connector/faker.md](../../upstream/trino/docs/connector/faker.md) | [properties](config-properties/plugin--trino-faker.md) |
| `plugin/trino-google-sheets` | connector | `gsheets` | `connector.name` | [connector/googlesheets.md](../../upstream/trino/docs/connector/googlesheets.md) | [properties](config-properties/plugin--trino-google-sheets.md) |
| `plugin/trino-hive` | connector | `hive` | `connector.name` | [connector/hive.md](../../upstream/trino/docs/connector/hive.md) | [properties](config-properties/plugin--trino-hive.md) |
| `plugin/trino-http-event-listener` | event listener | `http` | `event-listener.name` |  | [properties](config-properties/plugin--trino-http-event-listener.md) |
| `plugin/trino-http-server-event-listener` | event listener | `http-server` | `event-listener.name` |  | [properties](config-properties/plugin--trino-http-server-event-listener.md) |
| `plugin/trino-hudi` | connector | `hudi` | `connector.name` | [connector/hudi.md](../../upstream/trino/docs/connector/hudi.md) | [properties](config-properties/plugin--trino-hudi.md) |
| `plugin/trino-iceberg` | connector | `iceberg` | `connector.name` | [connector/iceberg.md](../../upstream/trino/docs/connector/iceberg.md) | [properties](config-properties/plugin--trino-iceberg.md) |
| `plugin/trino-ignite` | connector | `ignite` | `connector.name` | [connector/ignite.md](../../upstream/trino/docs/connector/ignite.md) |  |
| `plugin/trino-jmx` | connector | `jmx` | `connector.name` | [connector/jmx.md](../../upstream/trino/docs/connector/jmx.md) | [properties](config-properties/plugin--trino-jmx.md) |
| `plugin/trino-kafka` | connector | `kafka` | `connector.name` | [connector/kafka.md](../../upstream/trino/docs/connector/kafka.md) | [properties](config-properties/plugin--trino-kafka.md) |
| `plugin/trino-kafka-event-listener` | event listener | `kafka` | `event-listener.name` |  | [properties](config-properties/plugin--trino-kafka-event-listener.md) |
| `plugin/trino-lakehouse` | connector | `lakehouse` | `connector.name` | [connector/lakehouse.md](../../upstream/trino/docs/connector/lakehouse.md) | [properties](config-properties/plugin--trino-lakehouse.md) |
| `plugin/trino-ldap-group-provider` | group provider | `ldap` | `group-provider.name` |  | [properties](config-properties/plugin--trino-ldap-group-provider.md) |
| `plugin/trino-loki` | connector | `loki` | `connector.name` | [connector/loki.md](../../upstream/trino/docs/connector/loki.md) | [properties](config-properties/plugin--trino-loki.md) |
| `plugin/trino-mariadb` | connector | `mariadb` | `connector.name` | [connector/mariadb.md](../../upstream/trino/docs/connector/mariadb.md) |  |
| `plugin/trino-memory` | connector | `memory` | `connector.name` | [connector/memory.md](../../upstream/trino/docs/connector/memory.md) | [properties](config-properties/plugin--trino-memory.md) |
| `plugin/trino-mongodb` | connector | `mongodb` | `connector.name` | [connector/mongodb.md](../../upstream/trino/docs/connector/mongodb.md) | [properties](config-properties/plugin--trino-mongodb.md) |
| `plugin/trino-mysql` | connector | `mysql` | `connector.name` | [connector/mysql.md](../../upstream/trino/docs/connector/mysql.md) | [properties](config-properties/plugin--trino-mysql.md) |
| `plugin/trino-mysql-event-listener` | event listener | `mysql` | `event-listener.name` |  | [properties](config-properties/plugin--trino-mysql-event-listener.md) |
| `plugin/trino-opa` | system access control | `opa` | `access-control.name` |  | [properties](config-properties/plugin--trino-opa.md) |
| `plugin/trino-openlineage` | event listener | `openlineage` | `event-listener.name` |  | [properties](config-properties/plugin--trino-openlineage.md) |
| `plugin/trino-opensearch` | connector | `opensearch` | `connector.name` | [connector/opensearch.md](../../upstream/trino/docs/connector/opensearch.md) | [properties](config-properties/plugin--trino-opensearch.md) |
| `plugin/trino-oracle` | connector | `oracle` | `connector.name` | [connector/oracle.md](../../upstream/trino/docs/connector/oracle.md) | [properties](config-properties/plugin--trino-oracle.md) |
| `plugin/trino-password-authenticators` | group provider | `file` | `group-provider.name` |  | [properties](config-properties/plugin--trino-password-authenticators.md) |
| `plugin/trino-password-authenticators` | password authenticator | `file` | `password-authenticator.name` |  | [properties](config-properties/plugin--trino-password-authenticators.md) |
| `plugin/trino-password-authenticators` | password authenticator | `ldap` | `password-authenticator.name` |  | [properties](config-properties/plugin--trino-password-authenticators.md) |
| `plugin/trino-password-authenticators` | password authenticator | `salesforce` | `password-authenticator.name` |  | [properties](config-properties/plugin--trino-password-authenticators.md) |
| `plugin/trino-pinot` | connector | `pinot` | `connector.name` | [connector/pinot.md](../../upstream/trino/docs/connector/pinot.md) | [properties](config-properties/plugin--trino-pinot.md) |
| `plugin/trino-postgresql` | connector | `postgresql` | `connector.name` | [connector/postgresql.md](../../upstream/trino/docs/connector/postgresql.md) | [properties](config-properties/plugin--trino-postgresql.md) |
| `plugin/trino-prometheus` | connector | `prometheus` | `connector.name` | [connector/prometheus.md](../../upstream/trino/docs/connector/prometheus.md) | [properties](config-properties/plugin--trino-prometheus.md) |
| `plugin/trino-ranger` | system access control | `ranger` | `access-control.name` |  | [properties](config-properties/plugin--trino-ranger.md) |
| `plugin/trino-redis` | connector | `redis` | `connector.name` | [connector/redis.md](../../upstream/trino/docs/connector/redis.md) | [properties](config-properties/plugin--trino-redis.md) |
| `plugin/trino-redshift` | connector | `redshift` | `connector.name` | [connector/redshift.md](../../upstream/trino/docs/connector/redshift.md) | [properties](config-properties/plugin--trino-redshift.md) |
| `plugin/trino-resource-group-managers` | resource group manager | `db` | `resource-groups.configuration-manager` |  | [properties](config-properties/plugin--trino-resource-group-managers.md) |
| `plugin/trino-resource-group-managers` | resource group manager | `file` | `resource-groups.configuration-manager` |  | [properties](config-properties/plugin--trino-resource-group-managers.md) |
| `plugin/trino-session-property-managers` | session property manager | `db` | `session-property-config.configuration-manager` |  | [properties](config-properties/plugin--trino-session-property-managers.md) |
| `plugin/trino-session-property-managers` | session property manager | `file` | `session-property-config.configuration-manager` |  | [properties](config-properties/plugin--trino-session-property-managers.md) |
| `plugin/trino-singlestore` | connector | `singlestore` | `connector.name` | [connector/singlestore.md](../../upstream/trino/docs/connector/singlestore.md) | [properties](config-properties/plugin--trino-singlestore.md) |
| `plugin/trino-snowflake` | connector | `snowflake` | `connector.name` | [connector/snowflake.md](../../upstream/trino/docs/connector/snowflake.md) | [properties](config-properties/plugin--trino-snowflake.md) |
| `plugin/trino-spooling-filesystem` | spooling manager | `filesystem` | `spooling-manager.name` |  | [properties](config-properties/plugin--trino-spooling-filesystem.md) |
| `plugin/trino-sqlserver` | connector | `sqlserver` | `connector.name` | [connector/sqlserver.md](../../upstream/trino/docs/connector/sqlserver.md) | [properties](config-properties/plugin--trino-sqlserver.md) |
| `plugin/trino-thrift` | connector | `trino_thrift` | `connector.name` | [connector/thrift.md](../../upstream/trino/docs/connector/thrift.md) | [properties](config-properties/plugin--trino-thrift.md) |
| `plugin/trino-tpcds` | connector | `tpcds` | `connector.name` | [connector/tpcds.md](../../upstream/trino/docs/connector/tpcds.md) | [properties](config-properties/plugin--trino-tpcds.md) |
| `plugin/trino-tpch` | connector | `tpch` | `connector.name` | [connector/tpch.md](../../upstream/trino/docs/connector/tpch.md) | [properties](config-properties/plugin--trino-tpch.md) |
