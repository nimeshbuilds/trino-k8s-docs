# 15. Trino Gateway

A proxy and load balancer in front of several Trino clusters. Use it for workload routing, blue-green upgrades and coordinator failover. Trino Gateway 22, `trino-gateway` chart 1.22.0.

Upstream docs are copied in [upstream/trino-gateway/docs](../upstream/trino-gateway/docs). Start with [index.md](../upstream/trino-gateway/docs/index.md), [installation.md](../upstream/trino-gateway/docs/installation.md) and [routing-rules.md](../upstream/trino-gateway/docs/routing-rules.md).

## What it solves

A Trino cluster has one coordinator and one configuration. The gateway gives clients a single URL and decides per query which cluster handles it.

| Need | With the gateway |
|---|---|
| Separate interactive and batch workloads | Two clusters, routed by source, user or client tags |
| Upgrade without failing queries | Add the new cluster, switch traffic, drain the old one |
| Survive a coordinator outage | A second cluster takes new queries |
| Different settings per workload, such as retry policy | One cluster per profile |
| Maintenance windows | Deactivate a backend, wait, work, reactivate |

What it does not do: it does not move a running query between clusters, and it does not merge results. A query runs on exactly one backend.

## How it works

1. The client posts a statement to the gateway.
2. The gateway picks a routing group, from rules or the default `adhoc`, then a healthy active backend in that group.
3. The request is proxied. The gateway records which backend owns the returned query ID.
4. Follow-up requests for that query ID go to the same backend.

Requirements:

- Java 25 runtime. The container image includes it.
- A MySQL, PostgreSQL or Oracle database for backend definitions and query history. Schema migrations run at startup.
- Every Trino cluster behind it sets `http-server.process-forwarded=true`, so the URLs Trino returns to clients point at the gateway.

## Install with Helm

```bash
helm repo add trino https://trinodb.github.io/charts
helm install trino-gateway trino/trino-gateway -n trino-gateway --create-namespace \
  --version 1.22.0 -f gateway-values.yaml
```

```yaml
replicaCount: 2

config:
  serverConfig:
    node.environment: production
    http-server.http.enabled: true
    http-server.http.port: 8080
  dataStore:
    jdbcUrl: jdbc:postgresql://gateway-db.example.internal:5432/gateway
    user: ${ENV:DB_USER}
    password: ${ENV:DB_PASSWORD}
    driver: org.postgresql.Driver
  clusterStatsConfiguration:
    monitorType: INFO_API
  routingRules:
    rulesEngineEnabled: true
    rulesType: FILE
    rulesConfigPath: /etc/routing-rules/rules.yaml

envFrom:
  - secretRef:
      name: trino-gateway-db

volumes:
  - name: routing-rules
    configMap:
      name: trino-gateway-routing-rules
volumeMounts:
  - name: routing-rules
    mountPath: /etc/routing-rules

service:
  type: ClusterIP
```

The whole gateway configuration is the `config` value, rendered as the gateway's YAML configuration file. Every chart value is indexed in [reference/helm-values-index-gateway.md](reference/helm-values-index-gateway.md), and described in the [chart README](../upstream/charts/charts/gateway/README.md). Secrets handling in the configuration file is described in the installation page under "Secrets in configuration file".

Run at least two replicas. Gateway instances are stateless apart from the database, so they scale horizontally behind a Service.

## Register backends

Through the UI, or the REST API:

```bash
curl -X POST http://trino-gateway:8080/gateway/backend/modify/add \
  -H 'Content-Type: application/json' \
  -d '{"name": "interactive-1", "proxyTo": "http://trino.trino-interactive.svc:8080", "active": true, "routingGroup": "adhoc"}'

curl -X POST http://trino-gateway:8080/gateway/backend/modify/add \
  -H 'Content-Type: application/json' \
  -d '{"name": "batch-1", "proxyTo": "http://trino.trino-batch.svc:8080", "active": true, "routingGroup": "etl"}'
```

`externalUrl` overrides the link shown in the UI when the internal and external addresses differ. All endpoints, including activate, deactivate, update and delete, are in [gateway-api.md](../upstream/trino-gateway/docs/gateway-api.md).

## Routing rules

A rules file is a multi-document YAML. Each rule has a `name`, a `condition` and `actions`:

```yaml
---
name: "scheduled"
description: "scheduler traffic goes to the batch clusters"
condition: 'request.getHeader("X-Trino-Source") == "airflow"'
actions:
  - 'result.put("routingGroup", "etl")'
---
name: "tagged batch"
description: "anything tagged batch goes to the batch clusters"
condition: 'request.getHeader("X-Trino-Client-Tags") contains "batch"'
actions:
  - 'result.put("routingGroup", "etl")'
```

- A rule must put a `routingGroup` into `result`. Requests matching no rule go to the default routing group, `adhoc` unless configured otherwise.
- The file is re-read every minute by default (`rulesRefreshPeriod`).
- Beyond request headers, the rules engine exposes `trinoStatus`, `trinoRequestUser` and `trinoQueryProperties`, which allow routing on the authenticated user or on the tables a query touches. Rule priorities control evaluation order. An external HTTP service can decide instead of a file (`rulesType: EXTERNAL`).

Full syntax: [routing-rules.md](../upstream/trino-gateway/docs/routing-rules.md). Within a group, the default router picks a backend at random among healthy ones. `QueryCountBasedRouterProvider` picks the least loaded. See [routers.md](../upstream/trino-gateway/docs/routers.md).

## Health checks

`clusterStatsConfiguration.monitorType` chooses how the gateway judges a backend: `INFO_API` calls `/v1/info`. Other types read the UI API, JDBC, JMX or metrics for load-aware routing. Unhealthy backends receive no new queries. Details and the interplay between database state and the health cache: [operation.md](../upstream/trino-gateway/docs/operation.md).

## Blue-green upgrade with the gateway

1. Deploy the new Trino release next to the old one.
2. Add it as a backend with `"active": false`. Test it directly.
3. Activate it: `POST /gateway/backend/activate/<name>`.
4. Deactivate the old one: `POST /gateway/backend/deactivate/<name>`.
5. Watch the old cluster until `system.runtime.queries` shows nothing running, then remove it.

## Security

The gateway has its own authentication (OAuth 2.0 or OIDC, form with preset users or LDAP, JWT), role-based authorization for its UI and API, TLS settings and user mapping. It forwards client credentials to the backend, so Trino clusters still authenticate users themselves. Page: [security.md](../upstream/trino-gateway/docs/security.md).

Protect the backend API. Anyone who can call `/gateway/backend/modify/*` can redirect all queries.

## Operations

- Graceful shutdown, monitoring endpoints and the query history UI: [operation.md](../upstream/trino-gateway/docs/operation.md).
- Release notes, with configuration changes per version: [release-notes.md](../upstream/trino-gateway/docs/release-notes.md).
- The chart can create a ServiceMonitor (`serviceMonitor` values), an Ingress, an HPA and a PodDisruptionBudget.
- Clients need no changes other than the URL. Spooled results in `STORAGE` mode still flow straight from object storage to the client.

## When not to use it

One cluster, one workload profile, and tolerance for a short outage during upgrades: skip the gateway. It adds a database, a deployment and a routing layer to operate.
