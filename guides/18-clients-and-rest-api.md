# 18. Clients and the REST API

How applications, BI tools, schedulers and AI agents talk to Trino 484, and how to make automated clients behave.

Upstream pages: [client.md](../upstream/trino/docs/client.md), [client/cli.md](../upstream/trino/docs/client/cli.md), [client/jdbc.md](../upstream/trino/docs/client/jdbc.md), [client/client-protocol.md](../upstream/trino/docs/client/client-protocol.md), [develop/client-protocol.md](../upstream/trino/docs/develop/client-protocol.md).

## Official clients

| Client | Repository | Notes |
|---|---|---|
| CLI | `trinodb/trino`, module `client/trino-cli` | In the server image at `/usr/bin/trino` |
| JDBC driver | `trinodb/trino`, module `client/trino-jdbc` | Used by most BI tools |
| Python | `trinodb/trino-python-client` | DB-API and SQLAlchemy dialect, used by many BI tools and orchestrators. |
| Go | `trinodb/trino-go-client` | `database/sql` driver |
| JavaScript | `trinodb/trino-js-client` | |
| C# | `trinodb/trino-csharp-client` | |

Only the coordinator is a client endpoint. Clients never connect to workers.

## CLI

```bash
trino --server https://trino.example.internal \
      --user alice --password \
      --catalog hive --schema logs \
      --source nightly-report --client-tags scheduled,finance \
      --session query_max_execution_time=30m \
      --output-format CSV_HEADER \
      --execute "SELECT count(*) FROM events WHERE day = DATE '2026-01-01'"
```

| Option | Purpose |
|---|---|
| `--server` | Coordinator URL |
| `--user`, `--password` | User name. `--password` prompts, or reads the `TRINO_PASSWORD` environment variable. |
| `--access-token` | Bearer token for JWT authentication |
| `--external-authentication` | Browser-based OAuth 2.0 flow |
| `--catalog`, `--schema` | Defaults for unqualified names |
| `--execute`, `--file` | Run and exit |
| `--output-format` | `CSV`, `CSV_HEADER`, `CSV_UNQUOTED`, `TSV`, `TSV_HEADER`, `JSON`, `ALIGNED`, `VERTICAL`, `MARKDOWN`, `NULL` and variants |
| `--source`, `--client-tags`, `--client-info` | Identification for resource groups, session rules and audit |
| `--session name=value` | Session property. Repeatable. |
| `--insecure` | Skip certificate verification. Test only. |
| `--debug` | Stack traces and protocol detail |

Exit status is non-zero when a statement fails, which makes the CLI usable in scripts. `--ignore-errors` continues a file after a failure.

## JDBC

```text
jdbc:trino://trino.example.internal:443/hive/logs?SSL=true&source=report-service&clientTags=scheduled
```

Driver class `io.trino.jdbc.TrinoDriver`. Common parameters: `user`, `password`, `SSL`, `SSLVerification`, `accessToken`, `source`, `clientTags`, `clientInfo`, `sessionProperties` (`name:value;name:value`), `sessionUser` for impersonation, `timezone`, `explicitPrepare`. The full list is in [client/jdbc.md](../upstream/trino/docs/client/jdbc.md).

## Python

```python
import trino

conn = trino.dbapi.connect(
    host="trino.example.internal",
    port=443,
    http_scheme="https",
    user="alice",
    auth=trino.auth.BasicAuthentication("alice", password),
    catalog="hive",
    schema="logs",
    source="notebook",
    client_tags=["interactive"],
    session_properties={"query_max_execution_time": "15m"},
)
cur = conn.cursor()
cur.execute("SELECT count(*) FROM events WHERE day = DATE '2026-01-01'")
print(cur.fetchall())
```

SQLAlchemy URL, as used by BI tools built on SQLAlchemy: `trino://alice:<password>@trino.example.internal:443/hive`. The client library is documented in its own repository, which is not copied here.

## REST API

Every client uses the same HTTP protocol.

1. `POST /v1/statement` with the SQL text as the body and identification headers.
2. The response is a JSON `QueryResults` document. If it contains `nextUri`, `GET` that URL.
3. Repeat until a response has no `nextUri`. The query is then finished, successfully or with an `error` attribute.
4. `DELETE` the current `nextUri` to cancel.

```bash
curl -s -X POST https://trino.example.internal/v1/statement \
  -u alice:secret \
  -H 'X-Trino-Catalog: hive' -H 'X-Trino-Schema: logs' \
  -H 'X-Trino-Source: my-service' -H 'X-Trino-Client-Tags: agent' \
  -H 'X-Trino-Session: query_max_execution_time=5m' \
  -d 'SELECT 1'
```

Request headers:

| Header | Meaning |
|---|---|
| `X-Trino-User` | User, when not taken from authentication |
| `X-Trino-Original-User` | The user before impersonation |
| `X-Trino-Catalog`, `X-Trino-Schema` | Defaults |
| `X-Trino-Source` | Free-text source name |
| `X-Trino-Client-Tags` | Comma-separated tags |
| `X-Trino-Client-Info` | Free-text client information |
| `X-Trino-Session` | `name=value`, comma separated or repeated |
| `X-Trino-Time-Zone`, `X-Trino-Language` | Session time zone and locale |
| `X-Trino-Trace-Token` | Correlation token that appears in logs and events |
| `X-Trino-Prepared-Statement`, `X-Trino-Transaction-Id`, `X-Trino-Role`, `X-Trino-Extra-Credential`, `X-Trino-Resource-Estimate`, `X-Trino-Client-Capabilities` | Protocol state |
| `X-Trino-Query-Data-Encoding` | Opt in to the spooling protocol |

Response attributes: `id`, `nextUri`, `columns`, `data`, `updateType`, `error`, plus diagnostic attributes such as `warnings`, `stats` and failure details. Response headers such as `X-Trino-Set-Session` and `X-Trino-Set-Catalog` tell the client to change its state for following requests.

Protocol rules a hand-written client must follow:

- Keep polling. A query whose client stops requesting `nextUri` for `query.client.timeout` (default `5m`) is abandoned.
- Rows arrive across many responses. A response without `data` is normal while the query runs.
- An HTTP 200 does not mean success. Check for `error` in the final document.
- HTTP 502, 503 or 504 means an intermittent problem, usually from a load balancer in front of Trino. Retry the same request after 50 to 100 ms. On 429, retry after the `Retry-After` header value. Any other status than 200 means the query failed.
- Follow `nextUri` exactly as returned. Behind a proxy that relies on Trino having `http-server.process-forwarded=true`.

Other endpoints:

| Endpoint | Access | Purpose |
|---|---|---|
| `GET /v1/info` | Public | Version, uptime, `starting` flag |
| `GET /v1/info/state` | Public | Node state: `ACTIVE`, `SHUTTING_DOWN` |
| `PUT /v1/info/state` | System information write | Graceful shutdown of a worker |
| `GET /v1/status` | Public | Node status summary |
| `GET /metrics` | Authenticated, system information read | OpenMetrics |
| `GET /v1/node`, `GET /v1/thread` | System information read | Node list, thread dump |
| `/ui/` | Web UI authentication | Web UI and the APIs behind it |

Prefer SQL over the UI's internal APIs for automation: `system.runtime.queries`, `system.runtime.tasks`, `system.runtime.nodes` and `CALL system.runtime.kill_query(...)`.

## Spooling protocol

With a spooling manager configured, capable clients can fetch large results as segments from object storage instead of through the coordinator. A client opts in by sending the `X-Trino-Query-Data-Encoding` header. The JDBC driver and CLI expose this as an `encoding` option, with `json+zstd` recommended, and fall back to the cluster default. Older clients keep using the direct protocol. Clients need network access to the storage location in `STORAGE` retrieval mode. See guide [04](04-configuration-files.md#spooling-managerproperties) and [client/client-protocol.md](../upstream/trino/docs/client/client-protocol.md).

## Rules for automated clients and AI agents

An agent that writes its own SQL is the client most likely to hurt a shared cluster. Put the controls on the server, where the agent cannot skip them.

### Identity

- One service identity per agent or application, authenticated with a JWT or a password. Never a shared human account.
- Always send `X-Trino-Source` with a stable name and `X-Trino-Client-Tags` with a class such as `agent`. Use `X-Trino-Trace-Token` to tie a query to the agent run that issued it.
- If the agent acts for end users, use impersonation with `X-Trino-User` set to the end user and impersonation rules that allow only that service to do it.

### Permissions

- A read-only role: catalog rules with `"allow": "read-only"` and table rules granting only `SELECT` on the schemas the agent may see.
- Deny `system_session_properties` changes for limit properties, so the agent cannot raise its own caps.
- Column masks and row filters for sensitive data, through file-based rules or OPA.

### Limits

- A dedicated resource group with a small `hardConcurrencyLimit`, a bounded `maxQueued` and a `softMemoryLimit`.
- Session property manager rules for that group: short `query_max_execution_time`, `query_max_planning_time`, a `query_max_scan_physical_bytes` cap, and `query_max_cpu_time`.
- `hive.max-partitions-per-scan` at the catalog level to stop unfiltered scans of large partitioned tables.

Guide [12](12-resource-groups-and-session-properties.md) has a complete example with an `automation` group.

### Agent-side habits

Give the agent these as instructions:

1. Discover before querying: `SHOW CATALOGS`, `SHOW SCHEMAS FROM <catalog>`, `SHOW TABLES FROM <catalog>.<schema>`, `DESCRIBE <table>`, `SHOW CREATE TABLE <table>` for partitioning.
2. Always filter on partition columns. Find them in `SHOW CREATE TABLE` or `SELECT * FROM "<table>$partitions" LIMIT 5`.
3. Run `EXPLAIN` first for anything non-trivial and read the estimated rows and scan size.
4. Add `LIMIT` when exploring. Select named columns, not `*`, on wide columnar tables.
5. Use `approx_distinct` and `approx_percentile` for exploration.
6. On `QUERY_QUEUE_FULL` or HTTP 503, back off and retry. On `EXCEEDED_*` errors, narrow the query. Do not retry unchanged.
7. Cancel what you no longer need with `DELETE` on `nextUri`.
8. Never issue DDL or DML unless the task calls for it.

### MCP

The official `trinodb` repositories reviewed for this documentation contain no Model Context Protocol server. Third-party MCP servers for Trino exist. Review one like any dependency before use: check that it can be forced read-only, how it passes identity to Trino, and whether it executes anything other than SQL. The server-side controls above apply whatever sits in front.

## SQL reference pointers

| Topic | Page |
|---|---|
| Statement syntax | [sql.md](../upstream/trino/docs/sql.md) and the `sql/` directory |
| Functions and operators | [functions.md](../upstream/trino/docs/functions.md) and the `functions/` directory |
| Data types | [language/types.md](../upstream/trino/docs/language/types.md) |
| Reserved words | [language/reserved.md](../upstream/trino/docs/language/reserved.md) |
| SQL support by statement | [language/sql-support.md](../upstream/trino/docs/language/sql-support.md) |
| User-defined functions | [udf.md](../upstream/trino/docs/udf.md) |
