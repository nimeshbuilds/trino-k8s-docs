# 11. Security

TLS, the shared secret, authentication, access control and secrets for Trino 484 on Kubernetes with chart 1.45.0.

Upstream recommends this order, one step at a time, verifying each: TLS, shared secret, authentication, access control. See [security/overview.md](../upstream/trino/docs/security/overview.md).

## Default state

A fresh chart install has no TLS, no authentication and no access control. Anyone who can reach port 8080 can run any query as any user name, read every catalog, and kill queries. Keep the Service as `ClusterIP` and restrict the namespace until the steps below are done.

## Step 1: TLS

Pick one.

### A. Terminate TLS at a load balancer or ingress

```yaml
additionalConfigProperties:
  - http-server.process-forwarded=true
```

Trino then treats requests arriving with `X-Forwarded-Proto: https` as secure. Traffic between the proxy and the coordinator pod is plain HTTP inside the cluster. See guide [10](10-networking-and-access.md).

### B. TLS on the coordinator

```yaml
server:
  config:
    https:
      enabled: true
      port: 8443
      keystore:
        path: /etc/trino/tls/tls.pem
coordinator:
  secretMounts:
    - name: tls
      secretName: trino-tls
      path: /etc/trino/tls
```

The keystore is a PEM file with the certificate chain and the private key, or a JKS or PKCS12 file. For a password-protected keystore add `http-server.https.keystore.key=${ENV:KEYSTORE_PASSWORD}` to `server.coordinatorExtraConfig`. A cert-manager Secret has `tls.crt` and `tls.key` in separate keys. Trino wants one PEM file, so concatenate them into one key, or use an init container that does.

Keep HTTP on 8080 open for the probes and for workers unless you also configure internal TLS. Page: [security/tls.md](../upstream/trino/docs/security/tls.md).

## Step 2: Shared secret

Required as soon as any authentication is enabled. Same value on every node.

```bash
kubectl -n trino create secret generic trino-internal \
  --from-literal=shared-secret="$(openssl rand 512 | base64 -w 0)"
```

```yaml
env:
  - name: TRINO_SHARED_SECRET
    valueFrom:
      secretKeyRef:
        name: trino-internal
        key: shared-secret
additionalConfigProperties:
  - internal-communication.shared-secret=${ENV:TRINO_SHARED_SECRET}
```

Verify: the Web UI shows the expected number of active workers. A worker with the wrong secret does not register.

Internal TLS between nodes is optional and separate. Properties `internal-communication.https.required`, `internal-communication.https.keystore.path` and related are in [security/internal-communication.md](../upstream/trino/docs/security/internal-communication.md). It costs CPU on every exchange. Many deployments rely on network isolation or a service mesh instead.

## Step 3: Authentication

`http-server.authentication.type` takes one or more of `PASSWORD`, `OAUTH2`, `KERBEROS`, `CERTIFICATE`, `JWT`, `HEADER`, comma separated and tried in order. The chart value is `server.config.authenticationType`, and it is written on the coordinator only.

Authentication only applies to requests Trino considers secure: HTTPS on Trino itself, or requests carrying `X-Forwarded-Proto: https` when `http-server.process-forwarded=true`. What happens to plain HTTP requests depends on `http-server.authentication.allow-insecure-over-http`. This is from the source (`AuthenticationFilter`, `InsecureAuthenticator`, `ServerSecurityModule`):

| Setup | Default of `allow-insecure-over-http` | Plain HTTP requests |
|---|---|---|
| An authentication type, but neither HTTPS nor `process-forwarded` | `true` | Accepted with a user name alone, from `X-Trino-User` or basic auth without a password. A request that sends a password is rejected with "Password not allowed for insecure authentication". |
| An authentication type plus HTTPS or `process-forwarded=true` | `false` | Refused with "Authentication over HTTP is not enabled" |
| Same, with `allow-insecure-over-http=true` set by hand | `true` | Accepted with a user name alone |

Read the first row twice. Setting `authenticationType: PASSWORD` without HTTPS or `process-forwarded` protects nothing: anyone who can reach port 8080 gets in by naming a user. Always pair an authentication type with one of the two.

The second row is the safe state, and it has side effects inside the cluster. Anything that talks plain HTTP to the coordinator pod is refused, the metrics scrape included. Probes are unaffected because `/v1/info` is public. See the table further down.

### Password file

```bash
htpasswd -B -C 10 -c password.db alice
kubectl -n trino create secret generic trino-password-authentication --from-file=password.db
```

```yaml
server:
  config:
    authenticationType: PASSWORD
auth:
  passwordAuthSecret: trino-password-authentication
```

The chart writes `password-authenticator.properties` with `password-authenticator.name=file` and mounts the Secret at `/etc/trino/auth/password/password.db`. Hashes must be bcrypt with cost 8 or higher, or PBKDF2. The file is reloaded every `file.refresh-period`, default `5s`.

`auth.passwordAuth: "user:hash"` inlines the file into a chart-managed Secret. Prefer the external Secret, so hashes stay out of values files.

### Groups

```yaml
auth:
  groupsAuthSecret: trino-groups-authentication   # key group.db, lines "group:user1,user2"
  refreshPeriod: 30s
```

Groups feed access control rules and resource group selectors.

### LDAP

```yaml
server:
  config:
    authenticationType: PASSWORD
coordinator:
  additionalConfigFiles:
    password-authenticator.properties: |
      password-authenticator.name=ldap
      ldap.url=ldaps://ldap.example.internal:636
      ldap.user-bind-pattern=uid=${USER},ou=people,dc=example,dc=internal
```

Supplying `password-authenticator.properties` yourself stops the chart from writing its file-based one. Page: [security/ldap.md](../upstream/trino/docs/security/ldap.md).

### OAuth 2.0 and JWT

```yaml
server:
  config:
    authenticationType: OAUTH2
  coordinatorExtraConfig: |
    http-server.authentication.oauth2.issuer=https://idp.example.com/realms/data
    http-server.authentication.oauth2.client-id=trino
    http-server.authentication.oauth2.client-secret=${ENV:OAUTH_CLIENT_SECRET}
    web-ui.authentication.type=OAUTH2
```

OAuth 2.0 suits browsers and interactive clients. Services and agents usually present a JWT: `authenticationType: JWT` with `http-server.authentication.jwt.key-file` pointing at the JWKS URL. `OAUTH2,JWT` together is common. Pages: [security/oauth2.md](../upstream/trino/docs/security/oauth2.md), [security/jwt.md](../upstream/trino/docs/security/jwt.md).

### Others

[Kerberos](../upstream/trino/docs/security/kerberos.md), [client certificates](../upstream/trino/docs/security/certificate.md), [Salesforce](../upstream/trino/docs/security/salesforce.md), and custom [header authentication](../upstream/trino/docs/develop/header-authenticator.md) through `headerAuthenticator.properties`.

User names can be rewritten after authentication with [user mapping](../upstream/trino/docs/security/user-mapping.md).

### What changes inside the cluster when authentication goes on

| Thing | Effect | Fix |
|---|---|---|
| Probes and the image health check | None. `/v1/info` is public. | |
| Worker graceful shutdown hook | None with chart defaults. The chart gives workers no authentication type, so they keep accepting the `X-Trino-User` header on HTTP. | Only needs work if you add authentication to workers yourself |
| ServiceMonitor scrape of the coordinator over HTTP | Refused, once HTTPS or `process-forwarded` is on | Set `management.user`, see below. Or enable HTTPS on the coordinator, in which case the chart scrapes the `https` port with basic authentication from `serviceMonitor.coordinator.openMetrics.password` or an `authSecret`. |
| ServiceMonitor scrape of workers | None. Workers reject a password, so the chart refuses to render a shared `serviceMonitor.openMetrics.password`. | Put a password under `serviceMonitor.coordinator.openMetrics` only |
| Web UI | Switches to a login form with `PASSWORD` | Set `web-ui.authentication.type` for other types |
| Clients on plain HTTP | Refused | Use the HTTPS endpoint |

`management.user` names a fixed user for management endpoints, `/metrics` among them. Requests to those endpoints over plain HTTP are then served as that user without credentials. `management.user.https-enabled=true` extends that to HTTPS requests.

```yaml
additionalConfigProperties:
  - management.user=prometheus
```

Give that user nothing beyond `read` on `system_information`, and restrict who can reach port 8080 with a NetworkPolicy, because the fixed user needs no credentials.

## Step 4: Access control

System access control decides what an authenticated user may do. Options: `default`, `allow-all`, `read-only`, `file`, `opa`, `ranger`. Page: [security/built-in-system-access-control.md](../upstream/trino/docs/security/built-in-system-access-control.md).

`default` allows everything except user impersonation and the graceful shutdown call.

### File-based rules

```yaml
accessControl:
  type: configmap
  refreshPeriod: 60s
  configFile: rules.json
  rules:
    rules.json: |-
      {
        "catalogs": [
          {"group": "admins", "catalog": ".*", "allow": "all"},
          {"group": "analysts", "catalog": "(hive|iceberg)", "allow": "read-only"},
          {"catalog": "system", "allow": "read-only"}
        ],
        "queries": [
          {"group": "admins", "allow": ["execute", "kill", "view"]},
          {"allow": ["execute"]}
        ],
        "system_information": [
          {"user": "prometheus", "allow": ["read"]},
          {"group": "admins", "allow": ["read", "write"]}
        ]
      }
```

How the rules behave:

- Each section is evaluated top to bottom. The first matching rule decides.
- A missing `catalogs`, `schemas` or `tables` section grants access. A present section denies whatever no rule allows.
- `system_information` is different: with no rules, all access is denied. That blocks `/metrics` scrapes and graceful shutdown until you add rules.
- Users can always view and kill their own queries.
- Sections available: `catalogs`, `schemas`, `tables` (with column masks and row filters), `functions`, `procedures`, `system_session_properties`, `catalog_session_properties`, `queries`, `impersonation`, `principals`, `system_information`, `authorization`.

The chart applies `accessControl` to coordinator and workers. Role blocks override:

```yaml
accessControl:
  type: configmap
  rules: {...}
  worker:
    type: configmap
    rules:
      rules.json: |-
        {"system_information": [{"user": "prometheus", "allow": ["read"]}]}
```

With `worker.gracefulShutdown.enabled`, the chart merges the write permission for the shutdown user into the worker rules.

Full rule reference with a permission table per SQL statement: [security/file-system-access-control.md](../upstream/trino/docs/security/file-system-access-control.md). Example file: [examples/access-control-rules.json](examples/access-control-rules.json).

### Open Policy Agent

```yaml
opa:
  enabled: true
  policyPath: /v1/data/trino/allow
  batchedPath: /v1/data/trino/batch
  policy:
    trino.rego: |
      package trino
      default allow := false
      allow if input.context.identity.user == "admin"
```

The chart runs OPA as a sidecar in the coordinator pod, bound to `127.0.0.1:8181`, and writes `access-control.name=opa` with the policy URIs. Supply policies inline through `opa.policy` or from an existing ConfigMap through `opa.policyConfigMap`. Exactly one of the two. Row filters and column masks use `opa.rowFiltersPath` and `opa.columnMaskingPath`. Example values: [opa-values.yaml](../upstream/charts/charts/trino/ci/opa-values.yaml). Page: [security/opa-access-control.md](../upstream/trino/docs/security/opa-access-control.md). To use an OPA server elsewhere, configure it through `accessControl.type: properties` instead.

### Apache Ranger and others

```yaml
accessControl:
  type: properties
  properties: |
    access-control.name=ranger
    ranger.service.name=trino
    ranger.plugin.config.resource=/etc/trino/ranger/ranger-trino-security.xml,/etc/trino/ranger/ranger-trino-audit.xml
```

Page: [security/ranger-access-control.md](../upstream/trino/docs/security/ranger-access-control.md). Type `properties` cannot be combined with chart-managed graceful shutdown on workers.

### Connector-level authorization

Separate from system access control. Hive has `hive.security` (`allow-all`, `read-only`, `file`, `sql-standard`), Iceberg has `iceberg.security`. For a lake on Glue, system-level rules plus IAM are usually enough, and `allow-all` at the connector keeps the model simple.

## Securing access to data

- Trino reaches S3 and Glue with the pod's IAM role. Every user shares it unless you configure S3 security mapping, which picks a role per user, group or path. See [object-storage/file-system-s3.md](../upstream/trino/docs/object-storage/file-system-s3.md).
- Enforce data access in Trino's access control, and keep the pod role as narrow as the union of what users may read.
- Lake Formation permissions apply to the role Trino uses, not to the Trino end user.

## Secrets

Never commit credentials in values files. Options, from simplest:

1. `${ENV:NAME}` in properties, with `env` or `envFrom` reading Kubernetes Secrets.
2. Files from Secrets with `secretMounts`.
3. External stores through the Secrets Store CSI driver with `csiSecretMounts`.
4. Trino secrets providers, for example the keystore provider. See [security/secrets.md](../upstream/trino/docs/security/secrets.md).

Environment variables are visible to anyone who can `exec` into the pod or read the pod spec's referenced Secrets. Restrict both with RBAC.

## Pod security

The image runs as the non-root user `trino`. The chart sets `containerSecurityContext.allowPrivilegeEscalation: false` and drops all capabilities by default. `securityContext` sets the pod-level context. A read-only root file system needs writable volumes for `/data/trino` and the JVM temporary directory.

## Verification queries

```sql
SELECT current_user;
SELECT * FROM system.runtime.nodes;          -- needs system catalog access
SHOW CATALOGS;                               -- only catalogs the user may see
```

```bash
trino --server https://trino.example.internal --user alice --password --execute 'SELECT 1'
curl -s -u alice:secret https://trino.example.internal/v1/statement -d 'SELECT 1'
```

## Example

[examples/values-secured.yaml](examples/values-secured.yaml)
