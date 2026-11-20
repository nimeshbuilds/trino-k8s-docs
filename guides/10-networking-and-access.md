# 10. Networking and access

Ports, Services, and the ways to expose a Trino coordinator on Kubernetes. Chart 1.45.0.

## Ports

| Port | Protocol | Purpose | Who connects |
|---|---|---|---|
| 8080 (`service.port`) | HTTP | Client API, Web UI, internal communication, `/metrics`, `/v1/info` | Clients, workers, coordinator, Prometheus, probes |
| 8443 (`server.config.https.port`) | HTTPS | Same API over TLS, when enabled | Clients |
| 9080, 9081 | JMX RMI | Remote JMX, when `jmx.enabled` | JMX tools, the exporter sidecar on localhost |
| 5556 | HTTP | JMX exporter sidecar metrics, when `jmx.exporter.enabled` | Prometheus |
| 8181, 8282 | HTTP | OPA sidecar API and diagnostics, when `opa.enabled` | Trino on localhost, kubelet |

Trino uses one HTTP port for everything. There is no separate RPC port between nodes.

## Traffic flows

```text
client  --> coordinator:8080   submit statements, poll results, Web UI
worker  --> coordinator:8080   announce, discovery
coordinator --> worker:8080    create and update tasks, fetch task status and results
worker  --> worker:8080        exchange data between stages
all     --> object storage, metastore, other data sources
all     --> exchange bucket    fault-tolerant execution only
client  --> spooling storage   spooling protocol in STORAGE mode only
```

Nodes address each other by pod IP. Workers are not behind a load-balancing Service. The headless Service `<fullname>-worker` exists for DNS and for ServiceMonitors.

## Reaching the coordinator

### Inside the cluster

`http://<fullname>.<namespace>.svc:8080`. For a release named `trino` in namespace `trino`: `http://trino.trino.svc:8080`.

### Port-forward

```bash
kubectl -n trino port-forward svc/trino 8080:8080
```

Fine for administration. Not for BI tools or shared use.

### Service type

```yaml
service:
  type: LoadBalancer
  port: 8080
  annotations:
    service.beta.kubernetes.io/aws-load-balancer-type: external
    service.beta.kubernetes.io/aws-load-balancer-scheme: internal
    service.beta.kubernetes.io/aws-load-balancer-nlb-target-type: ip
  loadBalancerSourceRanges:
    - 10.0.0.0/8
```

`service.coordinator` overrides keys for the coordinator Service only. `service.nodePort` sets a fixed node port for type `NodePort`. The annotations shown are those of the AWS Load Balancer Controller for an internal Network Load Balancer.

### Ingress

```yaml
ingress:
  enabled: true
  className: alb
  annotations:
    alb.ingress.kubernetes.io/scheme: internal
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTPS":443}]'
    alb.ingress.kubernetes.io/certificate-arn: arn:aws:acm:us-east-1:111122223333:certificate/example
    alb.ingress.kubernetes.io/healthcheck-path: /v1/info
  hosts:
    - host: trino.example.internal
      paths:
        - path: /
          pathType: Prefix
```

### Gateway API

```yaml
gateway:
  enabled: true
  parentRefs:
    - name: internal-gateway
      namespace: gateway-system
  hostnames:
    - trino.example.internal
```

The chart renders an `HTTPRoute` to the coordinator Service. The chart CI files [gateway-values.yaml](../upstream/charts/charts/trino/ci/gateway-values.yaml) and [gateway-https-values.yaml](../upstream/charts/charts/trino/ci/gateway-https-values.yaml) show complete examples. This `gateway` key is the Kubernetes Gateway API. It is unrelated to the Trino Gateway product in guide [15](15-trino-gateway.md).

## TLS termination in front of Trino

The common pattern: a load balancer or ingress terminates TLS and talks plain HTTP to the coordinator. Trino must then be told to trust the forwarded headers:

```yaml
additionalConfigProperties:
  - http-server.process-forwarded=true
```

Without it, Trino sees plain HTTP requests, rejects any request that carries a password, and generates `http://` links in responses, which breaks clients behind HTTPS. With it, Trino honours `X-Forwarded-Proto` and `X-Forwarded-Host`, and once an authentication type is configured it refuses requests that do not arrive as HTTPS. Guide [11](11-security.md) explains the exact rules.

Only set it when every path to port 8080 goes through a trusted proxy. Combine with a NetworkPolicy so nothing else can reach the port and forge the headers.

Load balancer settings that matter:

- Idle timeout above the longest gap between client polls. Clients poll continuously, so the default 60 seconds is usually fine. Raise it for clients that hold connections during long planning.
- Health check path `/v1/info`. It is public and answers without authentication.
- No sticky sessions needed. There is one coordinator.
- Request header size limits. Large session properties, prepared statements and Kerberos or JWT tokens travel in headers. Make sure the proxy accepts headers at least as large as Trino does. Trino's own limits are HTTP server properties of the Airlift framework, `http-server.max-request-header-size` and `http-server.max-response-header-size`. They are not described in the Trino documentation copied here, so verify them against your Trino version before relying on them.

TLS directly on Trino is covered in guide [11](11-security.md).

## NetworkPolicy

```yaml
networkPolicy:
  enabled: true
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              kubernetes.io/metadata.name: ingress-system
      ports:
        - port: 8080
          protocol: TCP
  egress: []
```

The chart always allows traffic between the Trino pods of the release. `networkPolicy.ingress` adds sources. Leaving `egress` empty leaves egress unrestricted. The chart refuses to render a NetworkPolicy together with a `NodePort` coordinator Service. The values file comments in [values.yaml](../upstream/charts/charts/trino/values.yaml) describe the exact semantics.

When you restrict egress, allow at least: DNS, S3 and Glue endpoints, STS, the EKS Pod Identity agent address when you use Pod Identity, and your other data sources. Trino does not call the Kubernetes API. On the ingress side, add a rule for Prometheus on port 8080, and 5556 when the JMX exporter runs.

## AWS specifics

- Use S3 gateway endpoints, and interface endpoints for Glue and STS in private subnets, so data traffic avoids NAT gateways.
- Keep the cluster in the same region as the buckets. Set `s3.region` explicitly.
- Workers exchange data with each other constantly. Spreading workers over availability zones costs cross-zone transfer. A single-zone worker pool is a deliberate trade of resilience for cost.
- With the VPC CNI, pods take VPC IP addresses. Large worker counts need subnets with room. Prefix delegation raises pod density per node. The Data on EKS best practices page covers this: [trino-best-practices.md](../upstream/data-on-eks/website/docs/bestpractices/analytics/trino-best-practices.md).

## Client connection strings

| Client | Connection |
|---|---|
| CLI | `trino --server https://trino.example.internal --user alice --password` |
| JDBC | `jdbc:trino://trino.example.internal:443/hive/default?SSL=true` |
| Python | `trino.dbapi.connect(host="trino.example.internal", port=443, http_scheme="https", auth=...)` |
| SQLAlchemy, used by many BI tools | `trino://user:password@trino.example.internal:443/hive` |

Details in guide [18](18-clients-and-rest-api.md).

## Checks

```bash
kubectl -n trino get svc,ingress,httproute,networkpolicy
kubectl -n trino get endpoints trino
curl -s http://trino.trino.svc:8080/v1/info           # from a pod in the cluster
curl -sk https://trino.example.internal/v1/info       # through the load balancer
```

`/v1/info` returns JSON with `"starting": false` once the coordinator is ready.
