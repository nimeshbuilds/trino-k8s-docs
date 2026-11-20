# Trino on Kubernetes: documentation

Documentation only. No program source code.

A single place for everything needed to install, configure, secure, scale and operate Trino on Kubernetes, written so that a person or an AI agent can work from it alone.

| Component | Version documented |
|---|---|
| Trino | 484 |
| `trino` Helm chart | 1.45.0 |
| Trino Gateway | 22 |
| `trino-gateway` Helm chart | 1.22.0 |
| AWS Data on EKS Trino stack | commit `150577d`, 2026-11-19 |

## What is inside

| Path | Content | Size |
|---|---|---|
| [AGENTS.md](AGENTS.md) | Entry point for AI agents: routing table, lookup procedure, rules | |
| [guides/](guides/README.md) | 19 task guides written for this repository | |
| [guides/reference/](guides/reference/README.md) | Generated references: 1,402 configuration properties and 374 session properties extracted from the 484 source, 222 chart values, every plugin, 188 error codes, what each chart template renders | 60+ pages |
| [guides/examples/](guides/examples/README.md) | Helm values and JSON files | 8 files |
| [upstream/trino/](upstream/trino) | The complete Trino 484 documentation: 650 pages including all release notes, plus the container image defaults | 4.8 MB |
| [upstream/charts/](upstream/charts) | Chart READMEs, default values and example values for the `trino` and `trino-gateway` charts | |
| [upstream/trino-gateway/](upstream/trino-gateway) | The complete Trino Gateway 22 documentation | |
| [upstream/data-on-eks/](upstream/data-on-eks) | The Trino pages, Helm values and manifests of AWS Data on EKS | |
| [PROVENANCE.md](PROVENANCE.md) | Sources, commits, what was copied, what was left out, how to verify | |

## Start here

- New to Trino on Kubernetes: [guides/01-architecture.md](guides/01-architecture.md), then [guides/02-install-with-helm.md](guides/02-install-with-helm.md).
- Need a property: [guides/reference/config-properties/README.md](guides/reference/config-properties/README.md).
- Need a Helm value: [guides/reference/helm-values-index-trino.md](guides/reference/helm-values-index-trino.md).
- Something is broken: [guides/17-troubleshooting.md](guides/17-troubleshooting.md).
- Giving this to an agent: point it at [AGENTS.md](AGENTS.md).

## What this is not

- Not a fork of Trino. There is no Java, no Helm templates, no Terraform and no scripts here.
- Not an official publication of the Trino project, the Trino Software Foundation, or Amazon Web Services.
- Not tested against a live cluster. See "How these guides were written" in [guides/README.md](guides/README.md).

Files under `upstream/` are unmodified copies. For current documentation go to the sources: <https://trino.io/docs/current/>, <https://trinodb.github.io/charts>, <https://trinodb.github.io/trino-gateway/>, <https://awslabs.github.io/data-on-eks/>.

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).

The upstream projects are each licensed under the Apache License 2.0. Their license files are kept next to the copied files. Trino is a trademark of its owner. This repository is an independent documentation collection and claims no affiliation.
