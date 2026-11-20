# 19. Map of the upstream documentation

Every upstream page copied into this repository, with its title, so you can jump straight to the authority on a topic. Generated from the files under `upstream/`.

In the Trino pages, `|trino_version|` stands for 484. Cross-references written with the `{doc}` and `{ref}` roles, or as Markdown links with empty text and an absolute target, are Sphinx and MyST syntax. Read the target `/admin/properties` as the file `admin/properties.md` under `upstream/trino/docs`. Blocks fenced with `:::` are notes, warnings and tables.

## Trino 484

Root: [`upstream/trino/docs`](../upstream/trino/docs). Section index pages: [overview.md](../upstream/trino/docs/overview.md), [installation.md](../upstream/trino/docs/installation.md), [client.md](../upstream/trino/docs/client.md), [security.md](../upstream/trino/docs/security.md), [admin.md](../upstream/trino/docs/admin.md), [optimizer.md](../upstream/trino/docs/optimizer.md), [object-storage.md](../upstream/trino/docs/object-storage.md), [connector.md](../upstream/trino/docs/connector.md), [functions.md](../upstream/trino/docs/functions.md), [udf.md](../upstream/trino/docs/udf.md), [language.md](../upstream/trino/docs/language.md), [sql.md](../upstream/trino/docs/sql.md), [develop.md](../upstream/trino/docs/develop.md), [appendix.md](../upstream/trino/docs/appendix.md), [index.md](../upstream/trino/docs/index.md), [glossary.md](../upstream/trino/docs/glossary.md).

### Overview and concepts

2 pages in `overview/`.

| Page | Title |
|---|---|
| [overview/concepts.md](../upstream/trino/docs/overview/concepts.md) | Trino concepts |
| [overview/use-cases.md](../upstream/trino/docs/overview/use-cases.md) | Use cases |

### Installation

5 pages in `installation/`.

| Page | Title |
|---|---|
| [installation/containers.md](../upstream/trino/docs/installation/containers.md) | Trino in a Docker container |
| [installation/deployment.md](../upstream/trino/docs/installation/deployment.md) | Deploying Trino |
| [installation/kubernetes.md](../upstream/trino/docs/installation/kubernetes.md) | Trino on Kubernetes with Helm |
| [installation/plugins.md](../upstream/trino/docs/installation/plugins.md) | Plugins |
| [installation/query-resiliency.md](../upstream/trino/docs/installation/query-resiliency.md) | Improve query processing resilience |

### Clients

3 pages in `client/`.

| Page | Title |
|---|---|
| [client/cli.md](../upstream/trino/docs/client/cli.md) | Command line interface |
| [client/client-protocol.md](../upstream/trino/docs/client/client-protocol.md) | Client protocol |
| [client/jdbc.md](../upstream/trino/docs/client/jdbc.md) | JDBC driver |

### Security

20 pages in `security/`.

| Page | Title |
|---|---|
| [security/authentication-types.md](../upstream/trino/docs/security/authentication-types.md) | Authentication types |
| [security/built-in-system-access-control.md](../upstream/trino/docs/security/built-in-system-access-control.md) | System access control |
| [security/certificate.md](../upstream/trino/docs/security/certificate.md) | Certificate authentication |
| [security/file-system-access-control.md](../upstream/trino/docs/security/file-system-access-control.md) | File-based access control |
| [security/group-mapping.md](../upstream/trino/docs/security/group-mapping.md) | Group mapping |
| [security/inspect-jks.md](../upstream/trino/docs/security/inspect-jks.md) | JKS files |
| [security/inspect-pem.md](../upstream/trino/docs/security/inspect-pem.md) | PEM files |
| [security/internal-communication.md](../upstream/trino/docs/security/internal-communication.md) | Secure internal communication |
| [security/jwt.md](../upstream/trino/docs/security/jwt.md) | JWT authentication |
| [security/kerberos.md](../upstream/trino/docs/security/kerberos.md) | Kerberos authentication |
| [security/ldap.md](../upstream/trino/docs/security/ldap.md) | LDAP authentication |
| [security/oauth2.md](../upstream/trino/docs/security/oauth2.md) | OAuth 2.0 authentication |
| [security/opa-access-control.md](../upstream/trino/docs/security/opa-access-control.md) | Open Policy Agent access control |
| [security/overview.md](../upstream/trino/docs/security/overview.md) | Security overview |
| [security/password-file.md](../upstream/trino/docs/security/password-file.md) | Password file authentication |
| [security/ranger-access-control.md](../upstream/trino/docs/security/ranger-access-control.md) | Ranger access control |
| [security/salesforce.md](../upstream/trino/docs/security/salesforce.md) | Salesforce authentication |
| [security/secrets.md](../upstream/trino/docs/security/secrets.md) | Secrets |
| [security/tls.md](../upstream/trino/docs/security/tls.md) | TLS and HTTPS |
| [security/user-mapping.md](../upstream/trino/docs/security/user-mapping.md) | User mapping |

### Administration

36 pages in `admin/`.

| Page | Title |
|---|---|
| [admin/dist-sort.md](../upstream/trino/docs/admin/dist-sort.md) | Distributed sort |
| [admin/dynamic-filtering.md](../upstream/trino/docs/admin/dynamic-filtering.md) | Dynamic filtering |
| [admin/event-listeners-http.md](../upstream/trino/docs/admin/event-listeners-http.md) | HTTP event listener |
| [admin/event-listeners-kafka.md](../upstream/trino/docs/admin/event-listeners-kafka.md) | Kafka event listener |
| [admin/event-listeners-mysql.md](../upstream/trino/docs/admin/event-listeners-mysql.md) | MySQL event listener |
| [admin/event-listeners-openlineage.md](../upstream/trino/docs/admin/event-listeners-openlineage.md) | OpenLineage event listener |
| [admin/fault-tolerant-execution.md](../upstream/trino/docs/admin/fault-tolerant-execution.md) | Fault-tolerant execution |
| [admin/graceful-shutdown.md](../upstream/trino/docs/admin/graceful-shutdown.md) | Graceful shutdown |
| [admin/jmx.md](../upstream/trino/docs/admin/jmx.md) | Monitoring with JMX |
| [admin/logging.md](../upstream/trino/docs/admin/logging.md) | Logging |
| [admin/openmetrics.md](../upstream/trino/docs/admin/openmetrics.md) | Trino metrics with OpenMetrics |
| [admin/opentelemetry.md](../upstream/trino/docs/admin/opentelemetry.md) | Observability with OpenTelemetry |
| [admin/properties-catalog.md](../upstream/trino/docs/admin/properties-catalog.md) | Catalog management properties |
| [admin/properties-client-protocol.md](../upstream/trino/docs/admin/properties-client-protocol.md) | Client protocol properties |
| [admin/properties-exchange.md](../upstream/trino/docs/admin/properties-exchange.md) | Exchange properties |
| [admin/properties-general.md](../upstream/trino/docs/admin/properties-general.md) | General properties |
| [admin/properties-http-client.md](../upstream/trino/docs/admin/properties-http-client.md) | HTTP client properties |
| [admin/properties-http-server.md](../upstream/trino/docs/admin/properties-http-server.md) | HTTP server properties |
| [admin/properties-logging.md](../upstream/trino/docs/admin/properties-logging.md) | Logging properties |
| [admin/properties-node-scheduler.md](../upstream/trino/docs/admin/properties-node-scheduler.md) | Node scheduler properties |
| [admin/properties-optimizer.md](../upstream/trino/docs/admin/properties-optimizer.md) | Optimizer properties |
| [admin/properties-query-management.md](../upstream/trino/docs/admin/properties-query-management.md) | Query management properties |
| [admin/properties-regexp-function.md](../upstream/trino/docs/admin/properties-regexp-function.md) | Regular expression function properties |
| [admin/properties-resource-management.md](../upstream/trino/docs/admin/properties-resource-management.md) | Resource management properties |
| [admin/properties-spilling.md](../upstream/trino/docs/admin/properties-spilling.md) | Spilling properties |
| [admin/properties-sql-environment.md](../upstream/trino/docs/admin/properties-sql-environment.md) | SQL environment properties |
| [admin/properties-task.md](../upstream/trino/docs/admin/properties-task.md) | Task properties |
| [admin/properties-web-interface.md](../upstream/trino/docs/admin/properties-web-interface.md) | Web UI properties |
| [admin/properties-write-partitioning.md](../upstream/trino/docs/admin/properties-write-partitioning.md) | Write partitioning properties |
| [admin/properties-writer-scaling.md](../upstream/trino/docs/admin/properties-writer-scaling.md) | Writer scaling properties |
| [admin/properties.md](../upstream/trino/docs/admin/properties.md) | Properties reference |
| [admin/resource-groups.md](../upstream/trino/docs/admin/resource-groups.md) | Resource groups |
| [admin/session-property-managers.md](../upstream/trino/docs/admin/session-property-managers.md) | Session property managers |
| [admin/spill.md](../upstream/trino/docs/admin/spill.md) | Spill to disk |
| [admin/tuning.md](../upstream/trino/docs/admin/tuning.md) | Tuning Trino |
| [admin/web-interface.md](../upstream/trino/docs/admin/web-interface.md) | Web UI |

### Query optimizer

5 pages in `optimizer/`.

| Page | Title |
|---|---|
| [optimizer/adaptive-plan-optimizations.md](../upstream/trino/docs/optimizer/adaptive-plan-optimizations.md) | Adaptive plan optimizations |
| [optimizer/cost-based-optimizations.md](../upstream/trino/docs/optimizer/cost-based-optimizations.md) | Cost-based optimizations |
| [optimizer/cost-in-explain.md](../upstream/trino/docs/optimizer/cost-in-explain.md) | Cost in EXPLAIN |
| [optimizer/pushdown.md](../upstream/trino/docs/optimizer/pushdown.md) | Pushdown |
| [optimizer/statistics.md](../upstream/trino/docs/optimizer/statistics.md) | Table statistics |

### Object storage, file systems and metastores

9 pages in `object-storage/`.

| Page | Title |
|---|---|
| [object-storage/file-formats.md](../upstream/trino/docs/object-storage/file-formats.md) | Object storage file formats |
| [object-storage/file-system-alluxio.md](../upstream/trino/docs/object-storage/file-system-alluxio.md) | Alluxio file system support |
| [object-storage/file-system-azure.md](../upstream/trino/docs/object-storage/file-system-azure.md) | Azure Storage file system support |
| [object-storage/file-system-cache.md](../upstream/trino/docs/object-storage/file-system-cache.md) | File system cache |
| [object-storage/file-system-gcs.md](../upstream/trino/docs/object-storage/file-system-gcs.md) | Google Cloud Storage file system support |
| [object-storage/file-system-hdfs.md](../upstream/trino/docs/object-storage/file-system-hdfs.md) | HDFS file system support |
| [object-storage/file-system-local.md](../upstream/trino/docs/object-storage/file-system-local.md) | Local file system support |
| [object-storage/file-system-s3.md](../upstream/trino/docs/object-storage/file-system-s3.md) | S3 file system support |
| [object-storage/metastores.md](../upstream/trino/docs/object-storage/metastores.md) | Metastores |

### Connectors

39 pages in `connector/`.

| Page | Title |
|---|---|
| [connector/bigquery.md](../upstream/trino/docs/connector/bigquery.md) | BigQuery connector |
| [connector/blackhole.md](../upstream/trino/docs/connector/blackhole.md) | Black Hole connector |
| [connector/cassandra.md](../upstream/trino/docs/connector/cassandra.md) | Cassandra connector |
| [connector/clickhouse.md](../upstream/trino/docs/connector/clickhouse.md) | ClickHouse connector |
| [connector/delta-lake.md](../upstream/trino/docs/connector/delta-lake.md) | Delta Lake connector |
| [connector/druid.md](../upstream/trino/docs/connector/druid.md) | Druid connector |
| [connector/duckdb.md](../upstream/trino/docs/connector/duckdb.md) | DuckDB connector |
| [connector/elasticsearch.md](../upstream/trino/docs/connector/elasticsearch.md) | Elasticsearch connector |
| [connector/exasol.md](../upstream/trino/docs/connector/exasol.md) | Exasol connector |
| [connector/faker.md](../upstream/trino/docs/connector/faker.md) | Faker connector |
| [connector/googlesheets.md](../upstream/trino/docs/connector/googlesheets.md) | Google Sheets connector |
| [connector/hive.md](../upstream/trino/docs/connector/hive.md) | Hive connector |
| [connector/hudi.md](../upstream/trino/docs/connector/hudi.md) | Hudi connector |
| [connector/iceberg.md](../upstream/trino/docs/connector/iceberg.md) | Iceberg connector |
| [connector/ignite.md](../upstream/trino/docs/connector/ignite.md) | Ignite connector |
| [connector/jmx.md](../upstream/trino/docs/connector/jmx.md) | JMX connector |
| [connector/kafka-tutorial.md](../upstream/trino/docs/connector/kafka-tutorial.md) | Kafka connector tutorial |
| [connector/kafka.md](../upstream/trino/docs/connector/kafka.md) | Kafka connector |
| [connector/lakehouse.md](../upstream/trino/docs/connector/lakehouse.md) | Lakehouse connector |
| [connector/loki.md](../upstream/trino/docs/connector/loki.md) | Loki connector |
| [connector/mariadb.md](../upstream/trino/docs/connector/mariadb.md) | MariaDB connector |
| [connector/memory.md](../upstream/trino/docs/connector/memory.md) | Memory connector |
| [connector/mongodb.md](../upstream/trino/docs/connector/mongodb.md) | MongoDB connector |
| [connector/mysql.md](../upstream/trino/docs/connector/mysql.md) | MySQL connector |
| [connector/opensearch.md](../upstream/trino/docs/connector/opensearch.md) | OpenSearch connector |
| [connector/oracle.md](../upstream/trino/docs/connector/oracle.md) | Oracle connector |
| [connector/pinot.md](../upstream/trino/docs/connector/pinot.md) | Pinot connector |
| [connector/postgresql.md](../upstream/trino/docs/connector/postgresql.md) | PostgreSQL connector |
| [connector/prometheus.md](../upstream/trino/docs/connector/prometheus.md) | Prometheus connector |
| [connector/redis.md](../upstream/trino/docs/connector/redis.md) | Redis connector |
| [connector/redshift.md](../upstream/trino/docs/connector/redshift.md) | Redshift connector |
| [connector/removed.md](../upstream/trino/docs/connector/removed.md) | 404 - Connector removed |
| [connector/singlestore.md](../upstream/trino/docs/connector/singlestore.md) | SingleStore connector |
| [connector/snowflake.md](../upstream/trino/docs/connector/snowflake.md) | Snowflake connector |
| [connector/sqlserver.md](../upstream/trino/docs/connector/sqlserver.md) | SQL Server connector |
| [connector/system.md](../upstream/trino/docs/connector/system.md) | System connector |
| [connector/thrift.md](../upstream/trino/docs/connector/thrift.md) | Thrift connector |
| [connector/tpcds.md](../upstream/trino/docs/connector/tpcds.md) | TPC-DS connector |
| [connector/tpch.md](../upstream/trino/docs/connector/tpch.md) | TPC-H connector |

### Functions and operators

37 pages in `functions/`.

| Page | Title |
|---|---|
| [functions/aggregate.md](../upstream/trino/docs/functions/aggregate.md) | Aggregate functions |
| [functions/ai.md](../upstream/trino/docs/functions/ai.md) | AI functions |
| [functions/array.md](../upstream/trino/docs/functions/array.md) | Array functions and operators |
| [functions/binary.md](../upstream/trino/docs/functions/binary.md) | Binary functions and operators |
| [functions/bitwise.md](../upstream/trino/docs/functions/bitwise.md) | Bitwise functions |
| [functions/color.md](../upstream/trino/docs/functions/color.md) | Color functions |
| [functions/comparison.md](../upstream/trino/docs/functions/comparison.md) | Comparison functions and operators |
| [functions/conditional.md](../upstream/trino/docs/functions/conditional.md) | Conditional expressions |
| [functions/conversion.md](../upstream/trino/docs/functions/conversion.md) | Conversion functions |
| [functions/datasketches.md](../upstream/trino/docs/functions/datasketches.md) | DataSketches functions |
| [functions/datetime.md](../upstream/trino/docs/functions/datetime.md) | Date and time functions and operators |
| [functions/decimal.md](../upstream/trino/docs/functions/decimal.md) | Decimal functions and operators |
| [functions/geospatial.md](../upstream/trino/docs/functions/geospatial.md) | Geospatial functions |
| [functions/hyperloglog.md](../upstream/trino/docs/functions/hyperloglog.md) | HyperLogLog functions |
| [functions/ipaddress.md](../upstream/trino/docs/functions/ipaddress.md) | IP Address Functions |
| [functions/json.md](../upstream/trino/docs/functions/json.md) | JSON functions and operators |
| [functions/lambda.md](../upstream/trino/docs/functions/lambda.md) | Lambda expressions |
| [functions/list-by-topic.md](../upstream/trino/docs/functions/list-by-topic.md) | List of functions by topic |
| [functions/list.md](../upstream/trino/docs/functions/list.md) | List of functions and operators |
| [functions/logical.md](../upstream/trino/docs/functions/logical.md) | Logical operators |
| [functions/map.md](../upstream/trino/docs/functions/map.md) | Map functions and operators |
| [functions/math.md](../upstream/trino/docs/functions/math.md) | Mathematical functions and operators |
| [functions/ml.md](../upstream/trino/docs/functions/ml.md) | Machine learning functions |
| [functions/qdigest.md](../upstream/trino/docs/functions/qdigest.md) | Quantile digest functions |
| [functions/regexp.md](../upstream/trino/docs/functions/regexp.md) | Regular expression functions |
| [functions/row.md](../upstream/trino/docs/functions/row.md) | Row functions |
| [functions/session.md](../upstream/trino/docs/functions/session.md) | Session information |
| [functions/setdigest.md](../upstream/trino/docs/functions/setdigest.md) | Set Digest functions |
| [functions/string.md](../upstream/trino/docs/functions/string.md) | String functions and operators |
| [functions/system.md](../upstream/trino/docs/functions/system.md) | System information |
| [functions/table.md](../upstream/trino/docs/functions/table.md) | Table functions |
| [functions/tdigest.md](../upstream/trino/docs/functions/tdigest.md) | T-Digest functions |
| [functions/teradata.md](../upstream/trino/docs/functions/teradata.md) | Teradata functions |
| [functions/url.md](../upstream/trino/docs/functions/url.md) | URL functions |
| [functions/uuid.md](../upstream/trino/docs/functions/uuid.md) | UUID functions |
| [functions/variant.md](../upstream/trino/docs/functions/variant.md) | VARIANT functions and operators |
| [functions/window.md](../upstream/trino/docs/functions/window.md) | Window functions |

### User-defined functions

4 pages in `udf/`.

| Page | Title |
|---|---|
| [udf/function.md](../upstream/trino/docs/udf/function.md) | FUNCTION |
| [udf/introduction.md](../upstream/trino/docs/udf/introduction.md) | Introduction to UDFs |
| [udf/python.md](../upstream/trino/docs/udf/python.md) | Python user-defined functions |
| [udf/sql.md](../upstream/trino/docs/udf/sql.md) | SQL user-defined functions |

### SQL language

4 pages in `language/`.

| Page | Title |
|---|---|
| [language/comments.md](../upstream/trino/docs/language/comments.md) | Comments |
| [language/reserved.md](../upstream/trino/docs/language/reserved.md) | Keywords and identifiers |
| [language/sql-support.md](../upstream/trino/docs/language/sql-support.md) | SQL statement support |
| [language/types.md](../upstream/trino/docs/language/types.md) | Data types |

### SQL statements

77 pages in `sql/`.

| Page | Title |
|---|---|
| [sql/alter-branch.md](../upstream/trino/docs/sql/alter-branch.md) | ALTER BRANCH |
| [sql/alter-materialized-view.md](../upstream/trino/docs/sql/alter-materialized-view.md) | ALTER MATERIALIZED VIEW |
| [sql/alter-schema.md](../upstream/trino/docs/sql/alter-schema.md) | ALTER SCHEMA |
| [sql/alter-table.md](../upstream/trino/docs/sql/alter-table.md) | ALTER TABLE |
| [sql/alter-view.md](../upstream/trino/docs/sql/alter-view.md) | ALTER VIEW |
| [sql/analyze.md](../upstream/trino/docs/sql/analyze.md) | ANALYZE |
| [sql/call.md](../upstream/trino/docs/sql/call.md) | CALL |
| [sql/comment.md](../upstream/trino/docs/sql/comment.md) | COMMENT |
| [sql/commit.md](../upstream/trino/docs/sql/commit.md) | COMMIT |
| [sql/create-branch.md](../upstream/trino/docs/sql/create-branch.md) | CREATE BRANCH |
| [sql/create-catalog.md](../upstream/trino/docs/sql/create-catalog.md) | CREATE CATALOG |
| [sql/create-function.md](../upstream/trino/docs/sql/create-function.md) | CREATE FUNCTION |
| [sql/create-materialized-view.md](../upstream/trino/docs/sql/create-materialized-view.md) | CREATE MATERIALIZED VIEW |
| [sql/create-role.md](../upstream/trino/docs/sql/create-role.md) | CREATE ROLE |
| [sql/create-schema.md](../upstream/trino/docs/sql/create-schema.md) | CREATE SCHEMA |
| [sql/create-table-as.md](../upstream/trino/docs/sql/create-table-as.md) | CREATE TABLE AS |
| [sql/create-table.md](../upstream/trino/docs/sql/create-table.md) | CREATE TABLE |
| [sql/create-view.md](../upstream/trino/docs/sql/create-view.md) | CREATE VIEW |
| [sql/deallocate-prepare.md](../upstream/trino/docs/sql/deallocate-prepare.md) | DEALLOCATE PREPARE |
| [sql/delete.md](../upstream/trino/docs/sql/delete.md) | DELETE |
| [sql/deny.md](../upstream/trino/docs/sql/deny.md) | DENY |
| [sql/describe-input.md](../upstream/trino/docs/sql/describe-input.md) | DESCRIBE INPUT |
| [sql/describe-output.md](../upstream/trino/docs/sql/describe-output.md) | DESCRIBE OUTPUT |
| [sql/describe.md](../upstream/trino/docs/sql/describe.md) | DESCRIBE |
| [sql/drop-branch.md](../upstream/trino/docs/sql/drop-branch.md) | DROP BRANCH |
| [sql/drop-catalog.md](../upstream/trino/docs/sql/drop-catalog.md) | DROP CATALOG |
| [sql/drop-function.md](../upstream/trino/docs/sql/drop-function.md) | DROP FUNCTION |
| [sql/drop-materialized-view.md](../upstream/trino/docs/sql/drop-materialized-view.md) | DROP MATERIALIZED VIEW |
| [sql/drop-role.md](../upstream/trino/docs/sql/drop-role.md) | DROP ROLE |
| [sql/drop-schema.md](../upstream/trino/docs/sql/drop-schema.md) | DROP SCHEMA |
| [sql/drop-table.md](../upstream/trino/docs/sql/drop-table.md) | DROP TABLE |
| [sql/drop-view.md](../upstream/trino/docs/sql/drop-view.md) | DROP VIEW |
| [sql/execute-immediate.md](../upstream/trino/docs/sql/execute-immediate.md) | EXECUTE IMMEDIATE |
| [sql/execute.md](../upstream/trino/docs/sql/execute.md) | EXECUTE |
| [sql/explain-analyze.md](../upstream/trino/docs/sql/explain-analyze.md) | EXPLAIN ANALYZE |
| [sql/explain.md](../upstream/trino/docs/sql/explain.md) | EXPLAIN |
| [sql/grant-roles.md](../upstream/trino/docs/sql/grant-roles.md) | GRANT role |
| [sql/grant.md](../upstream/trino/docs/sql/grant.md) | GRANT privilege |
| [sql/insert.md](../upstream/trino/docs/sql/insert.md) | INSERT |
| [sql/match-recognize.md](../upstream/trino/docs/sql/match-recognize.md) | MATCH_RECOGNIZE |
| [sql/merge.md](../upstream/trino/docs/sql/merge.md) | MERGE |
| [sql/pattern-recognition-in-window.md](../upstream/trino/docs/sql/pattern-recognition-in-window.md) | Row pattern recognition in window structures |
| [sql/pivot.md](../upstream/trino/docs/sql/pivot.md) | PIVOT |
| [sql/prepare.md](../upstream/trino/docs/sql/prepare.md) | PREPARE |
| [sql/refresh-materialized-view.md](../upstream/trino/docs/sql/refresh-materialized-view.md) | REFRESH MATERIALIZED VIEW |
| [sql/reset-session-authorization.md](../upstream/trino/docs/sql/reset-session-authorization.md) | RESET SESSION AUTHORIZATION |
| [sql/reset-session.md](../upstream/trino/docs/sql/reset-session.md) | RESET SESSION |
| [sql/revoke-roles.md](../upstream/trino/docs/sql/revoke-roles.md) | REVOKE role |
| [sql/revoke.md](../upstream/trino/docs/sql/revoke.md) | REVOKE privilege |
| [sql/rollback.md](../upstream/trino/docs/sql/rollback.md) | ROLLBACK |
| [sql/select.md](../upstream/trino/docs/sql/select.md) | SELECT |
| [sql/set-path.md](../upstream/trino/docs/sql/set-path.md) | SET PATH |
| [sql/set-role.md](../upstream/trino/docs/sql/set-role.md) | SET ROLE |
| [sql/set-session-authorization.md](../upstream/trino/docs/sql/set-session-authorization.md) | SET SESSION AUTHORIZATION |
| [sql/set-session.md](../upstream/trino/docs/sql/set-session.md) | SET SESSION |
| [sql/set-time-zone.md](../upstream/trino/docs/sql/set-time-zone.md) | SET TIME ZONE |
| [sql/show-branches.md](../upstream/trino/docs/sql/show-branches.md) | SHOW BRANCHES |
| [sql/show-catalogs.md](../upstream/trino/docs/sql/show-catalogs.md) | SHOW CATALOGS |
| [sql/show-columns.md](../upstream/trino/docs/sql/show-columns.md) | SHOW COLUMNS |
| [sql/show-create-function.md](../upstream/trino/docs/sql/show-create-function.md) | SHOW CREATE FUNCTION |
| [sql/show-create-materialized-view.md](../upstream/trino/docs/sql/show-create-materialized-view.md) | SHOW CREATE MATERIALIZED VIEW |
| [sql/show-create-schema.md](../upstream/trino/docs/sql/show-create-schema.md) | SHOW CREATE SCHEMA |
| [sql/show-create-table.md](../upstream/trino/docs/sql/show-create-table.md) | SHOW CREATE TABLE |
| [sql/show-create-view.md](../upstream/trino/docs/sql/show-create-view.md) | SHOW CREATE VIEW |
| [sql/show-functions.md](../upstream/trino/docs/sql/show-functions.md) | SHOW FUNCTIONS |
| [sql/show-grants.md](../upstream/trino/docs/sql/show-grants.md) | SHOW GRANTS |
| [sql/show-role-grants.md](../upstream/trino/docs/sql/show-role-grants.md) | SHOW ROLE GRANTS |
| [sql/show-roles.md](../upstream/trino/docs/sql/show-roles.md) | SHOW ROLES |
| [sql/show-schemas.md](../upstream/trino/docs/sql/show-schemas.md) | SHOW SCHEMAS |
| [sql/show-session.md](../upstream/trino/docs/sql/show-session.md) | SHOW SESSION |
| [sql/show-stats.md](../upstream/trino/docs/sql/show-stats.md) | SHOW STATS |
| [sql/show-tables.md](../upstream/trino/docs/sql/show-tables.md) | SHOW TABLES |
| [sql/start-transaction.md](../upstream/trino/docs/sql/start-transaction.md) | START TRANSACTION |
| [sql/truncate.md](../upstream/trino/docs/sql/truncate.md) | TRUNCATE |
| [sql/update.md](../upstream/trino/docs/sql/update.md) | UPDATE |
| [sql/use.md](../upstream/trino/docs/sql/use.md) | USE |
| [sql/values.md](../upstream/trino/docs/sql/values.md) | VALUES |

### Developer guide

18 pages in `develop/`.

| Page | Title |
|---|---|
| [develop/certificate-authenticator.md](../upstream/trino/docs/develop/certificate-authenticator.md) | Certificate authenticator |
| [develop/client-protocol.md](../upstream/trino/docs/develop/client-protocol.md) | Trino client REST API |
| [develop/connectors.md](../upstream/trino/docs/develop/connectors.md) | Connectors |
| [develop/event-listener.md](../upstream/trino/docs/develop/event-listener.md) | Event listener |
| [develop/example-http.md](../upstream/trino/docs/develop/example-http.md) | Example HTTP connector |
| [develop/example-jdbc.md](../upstream/trino/docs/develop/example-jdbc.md) | Example JDBC connector |
| [develop/functions.md](../upstream/trino/docs/develop/functions.md) | Functions |
| [develop/group-provider.md](../upstream/trino/docs/develop/group-provider.md) | Group provider |
| [develop/header-authenticator.md](../upstream/trino/docs/develop/header-authenticator.md) | Header authenticator |
| [develop/insert.md](../upstream/trino/docs/develop/insert.md) | Supporting `INSERT` and `CREATE TABLE AS` |
| [develop/password-authenticator.md](../upstream/trino/docs/develop/password-authenticator.md) | Password authenticator |
| [develop/secrets-provider.md](../upstream/trino/docs/develop/secrets-provider.md) | Secrets provider |
| [develop/spi-overview.md](../upstream/trino/docs/develop/spi-overview.md) | SPI overview |
| [develop/supporting-merge.md](../upstream/trino/docs/develop/supporting-merge.md) | Supporting `MERGE` |
| [develop/system-access-control.md](../upstream/trino/docs/develop/system-access-control.md) | System access control |
| [develop/table-functions.md](../upstream/trino/docs/develop/table-functions.md) | Table functions |
| [develop/tests.md](../upstream/trino/docs/develop/tests.md) | Test writing guidelines |
| [develop/types.md](../upstream/trino/docs/develop/types.md) | Types |

### Appendix

2 pages in `appendix/`.

| Page | Title |
|---|---|
| [appendix/from-hive.md](../upstream/trino/docs/appendix/from-hive.md) | Migrating from Hive |
| [appendix/legal-notices.md](../upstream/trino/docs/appendix/legal-notices.md) | Legal notices |

### Release notes

358 pages in [`release/`](../upstream/trino/docs/release), one per release, named `release-<version>.md`. The newest is [release-484.md](../upstream/trino/docs/release/release-484.md). The index page is [release.md](../upstream/trino/docs/release.md). Breaking changes are marked `{breaking}`. Search across them with `grep -l "<property>" upstream/trino/docs/release/*.md`.

### Other Trino files

| File | Content |
|---|---|
| [README.md](../upstream/trino/README.md) | Repository README |
| [docs/README-docs-build.md](../upstream/trino/docs/README-docs-build.md) | How the documentation is written and built upstream |
| [core/docker/README.md](../upstream/trino/core/docker/README.md) | Container image usage |
| [core/docker/default/etc](../upstream/trino/core/docker/default/etc) | Default configuration baked into the image |
| [LICENSE](../upstream/trino/LICENSE) | Apache License 2.0 |

## Trino Gateway 22

| Page | Title |
|---|---|
| [backend-tags.md](../upstream/trino-gateway/docs/backend-tags.md) | Backend tags |
| [design.md](../upstream/trino-gateway/docs/design.md) | Design |
| [development.md](../upstream/trino-gateway/docs/development.md) | Development |
| [docker.md](../upstream/trino-gateway/docs/docker.md) | Trino Gateway Docker image |
| [docs.md](../upstream/trino-gateway/docs/docs.md) | Documentation development |
| [gateway-api.md](../upstream/trino-gateway/docs/gateway-api.md) | Gateway API |
| [index.md](../upstream/trino-gateway/docs/index.md) | Overview |
| [installation.md](../upstream/trino-gateway/docs/installation.md) | Installation |
| [migration-to-airlift.md](../upstream/trino-gateway/docs/migration-to-airlift.md) | Migration to Airlift |
| [operation.md](../upstream/trino-gateway/docs/operation.md) | Operation |
| [quickstart.md](../upstream/trino-gateway/docs/quickstart.md) | Quickstart |
| [release-notes.md](../upstream/trino-gateway/docs/release-notes.md) | Release notes |
| [resources.md](../upstream/trino-gateway/docs/resources.md) | Resources |
| [routers.md](../upstream/trino-gateway/docs/routers.md) | Routers |
| [routing-logic.md](../upstream/trino-gateway/docs/routing-logic.md) | Routing Logic |
| [routing-rules.md](../upstream/trino-gateway/docs/routing-rules.md) | Routing rules |
| [security.md](../upstream/trino-gateway/docs/security.md) | Security |
| [users.md](../upstream/trino-gateway/docs/users.md) | Overview |
| [config.yaml](../upstream/trino-gateway/docs/config.yaml) | Minimal example configuration |

## Helm charts

| File | Content |
|---|---|
| [charts/trino/README.md](../upstream/charts/charts/trino/README.md) | Every value of the `trino` chart 1.45.0 with descriptions and examples |
| [charts/trino/values.yaml](../upstream/charts/charts/trino/values.yaml) | Default values with comments |
| [charts/trino/Chart.yaml](../upstream/charts/charts/trino/Chart.yaml) | Chart metadata |
| [charts/trino/ci/access-control-properties-values.yaml](../upstream/charts/charts/trino/ci/access-control-properties-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/complete-values.yaml](../upstream/charts/charts/trino/ci/complete-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/default-values.yaml](../upstream/charts/charts/trino/ci/default-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/exchange-manager-values.yaml](../upstream/charts/charts/trino/ci/exchange-manager-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/gateway-https-values.yaml](../upstream/charts/charts/trino/ci/gateway-https-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/gateway-values.yaml](../upstream/charts/charts/trino/ci/gateway-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/graceful-shutdown-values.yaml](../upstream/charts/charts/trino/ci/graceful-shutdown-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/header-authenticator-values.yaml](../upstream/charts/charts/trino/ci/header-authenticator-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/opa-values.yaml](../upstream/charts/charts/trino/ci/opa-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/overrides-values.yaml](../upstream/charts/charts/trino/ci/overrides-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/resource-groups-properties-values.yaml](../upstream/charts/charts/trino/ci/resource-groups-properties-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/single-node-values.yaml](../upstream/charts/charts/trino/ci/single-node-values.yaml) | Example values used by the chart tests |
| [charts/trino/ci/worker-access-control-values.yaml](../upstream/charts/charts/trino/ci/worker-access-control-values.yaml) | Example values used by the chart tests |
| [charts/gateway/README.md](../upstream/charts/charts/gateway/README.md) | Every value of the `trino-gateway` chart 1.22.0 |
| [charts/gateway/values.yaml](../upstream/charts/charts/gateway/values.yaml) | Default values with comments |
| [charts/gateway/ci/complete-values.yaml](../upstream/charts/charts/gateway/ci/complete-values.yaml) | Example values used by the chart tests |
| [charts/gateway/ci/env-from-values.yaml](../upstream/charts/charts/gateway/ci/env-from-values.yaml) | Example values used by the chart tests |
| [charts/gateway/ci/https-values.yaml](../upstream/charts/charts/gateway/ci/https-values.yaml) | Example values used by the chart tests |
| [charts/gateway/ci/nodeport-values.yaml](../upstream/charts/charts/gateway/ci/nodeport-values.yaml) | Example values used by the chart tests |
| [README.md](../upstream/charts/README.md) | Chart repository README |

## AWS Data on EKS

See guide [16](16-eks-reference-stack.md) for the file list and commentary.
