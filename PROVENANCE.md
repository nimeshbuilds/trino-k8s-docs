# Provenance

Where every file in this repository comes from, what was left out, and how to check it.

## Sources

| Project | Repository | Reference | Commit | Commit date | License |
|---|---|---|---|---|---|
| Trino | <https://github.com/trinodb/trino> | tag `484` | `e2d50bb733e193607c44be84ad80ce2430ba1d01` | 2026-10-15 | Apache 2.0 |
| Trino Helm charts | <https://github.com/trinodb/charts> | branch `main` | `19a890d26f990df745b0f920d1d6dd07fae2bdd1` | 2026-10-20 | Apache 2.0 |
| Trino Gateway | <https://github.com/trinodb/trino-gateway> | tag `22` | `cb116ce465a91b94018db0215d246e80a85bd816` | | Apache 2.0 |
| Data on EKS | <https://github.com/awslabs/data-on-eks> | branch `main` | `150577d1ca616b527a5bae857e6540c4d285250a` | 2026-11-19 | Apache 2.0 |

Chart versions at that charts commit: `trino` 1.45.0 (appVersion 484), `trino-gateway` 1.22.0 (appVersion 22).

## Copied files

All files under `upstream/` are byte-for-byte copies. Nothing was edited.

### `upstream/trino/`

| Destination | Source path in `trinodb/trino` | What |
|---|---|---|
| `LICENSE`, `README.md` | `LICENSE`, `README.md` | |
| `docs/**` | `docs/src/main/sphinx/**` | Files with extensions `.md`, `.fragment`, `.png`, `.svg`, `.json`. 650 Markdown pages, 27 include fragments, example JSON files, images. |
| `docs/README-docs-build.md` | `docs/README.md` | Renamed to avoid a clash with the documentation tree. Content unchanged. |
| `core/docker/README.md` | `core/docker/README.md` | |
| `core/docker/default/etc/**` | `core/docker/default/etc/**` | Default properties files of the container image |

The directory level `docs/src/main/sphinx` is flattened to `docs`.

Left out of the Trino documentation tree:

| Path | Reason |
|---|---|
| `docs/src/main/sphinx/static/` | Web fonts under the SIL Open Font License, JavaScript and CSS for the website |
| `docs/src/main/sphinx/ext/`, `conf.py` | Python code for the documentation build |
| `docs/src/main/sphinx/templates/`, `docutils.conf`, `redirects.txt` | Website build configuration |
| `docs/src/main/sphinx/include/TrinoThriftService.thrift` | An interface definition file, which is source code. The Thrift connector page refers to it. |
| `docs/pom.xml`, `docs/build`, `docs/release-template.md`, `docs/sitemap.xslt` | Build files |

Left out of `core/docker`: `Dockerfile`, `build.sh`, `container-test.sh`, `bin/health-check`. These are code. [guides/reference/docker-image.md](guides/reference/docker-image.md) describes what they do.

### `upstream/charts/`

| Destination | Source path in `trinodb/charts` |
|---|---|
| `LICENSE`, `README.md` | same |
| `charts/trino/Chart.yaml`, `README.md`, `values.yaml`, `ci/*.yaml` | same |
| `charts/gateway/Chart.yaml`, `README.md`, `values.yaml`, `ci/*.yaml` | same |

Left out: `charts/*/templates/` (Helm template code), `charts/*/tests/`, `charts/trino/README.md.gotmpl`, repository tooling. [guides/reference/chart-templates.md](guides/reference/chart-templates.md) describes what the `trino` templates render.

### `upstream/trino-gateway/`

| Destination | Source path in `trinodb/trino-gateway` |
|---|---|
| `LICENSE`, `README.md` | same |
| `docs/**` | `docs/**`, files with extensions `.md`, `.png`, `.svg`, `.yaml` |

Left out: all Java modules and build files, `mkdocs.yml`, `docs/assets/**/*.pdf` (four conference slide decks), CSS, JavaScript and icon files of the documentation site.

### `upstream/data-on-eks/`

| Destination | Source path in `awslabs/data-on-eks` |
|---|---|
| `LICENSE`, `NOTICE` | same |
| `website/docs/datastacks/databases/trino-on-eks/**` | same. Two Markdown pages and their images. |
| `website/docs/bestpractices/analytics/trino-best-practices.md` | same |
| `infra/terraform/helm-values/trino.yaml` | same. A Terraform template of Helm values. |
| `infra/terraform/argocd-applications/trino.yaml` | same |
| `infra/terraform/manifests/trino/keda-scaledobject.yaml` | same |
| `data-stacks/trino-on-eks/examples/*.sql`, `glue-role-trust.json` | same |

Left out: `infra/terraform/trino.tf` and all other Terraform, `data-stacks/trino-on-eks/deploy.sh`, `examples/hive-setup.sh`, `examples/hive-cleanup.sh`, `terraform/data-stack.tfvars`. [guides/16-eks-reference-stack.md](guides/16-eks-reference-stack.md) describes what the Terraform creates.

The example SQL file `trino_sf10000_tpcds_to_iceberg.sql` creates tables from Trino's built-in TPC-DS connector. It contains table names of the TPC-DS schema and no benchmark query text.

## Original files

Everything outside `upstream/` was written for this repository: `README.md`, `AGENTS.md`, `NOTICE`, `PROVENANCE.md`, and `guides/**`. `LICENSE` is the Apache License 2.0 text.

### Generated reference pages

These pages under `guides/reference/` were produced by scripts that read upstream files. The scripts are not part of this repository. No source code was copied. What was extracted is listed below.

| Page | Input | Extracted |
|---|---|---|
| `config-properties/*.md` | Java files under `core/`, `lib/`, `plugin/`, `client/` of `trinodb/trino` at tag `484`, `src/main/java` only | From `@Config`, `@ConfigDescription`, `@LegacyConfig`, `@DefunctConfig`, `@ConfigSecuritySensitive`, `@ConfigHidden` and `@Deprecated` annotations on setter methods: the property name, the Java type of the setter parameter, the initialiser expression of the backing field, the description string, legacy names, removed names and flags |
| `session-properties-from-source.md` | `*SessionProperties.java` in the same tree | Property name, kind of property, description string |
| `plugins-and-connectors.md` | Factory classes in the same tree, connector pages in the docs | The name each factory registers, and links |
| `error-codes.md` | `core/trino-spi/src/main/java/io/trino/spi/StandardErrorCode.java` | Error name, numeric code, error type |
| `documented-properties.md` | `docs/src/main/sphinx/admin/properties-*.md` | Property name, type, default, allowed values, session property |
| `helm-values-index-trino.md`, `helm-values-index-gateway.md` | The chart READMEs | Key, type, default, first paragraph of the description |
| `guides/19-upstream-doc-map.md` | The copied Markdown files | Path and first heading of each page |

`chart-templates.md` and `docker-image.md` were written by hand after reading the chart templates and the image build files.

### Completeness checks that were run

| Check | Result |
|---|---|
| `@Config` annotations in the main sources against extracted property rows | 1,402 and 1,402 |
| Session property name constants in every `*SessionProperties` class against extracted rows | All present, 374 rows |
| Relative links and heading anchors in all Markdown outside `upstream/` | All resolve |
| Keys of every example values file against the default values of chart 1.45.0 | All known |
| Example JSON files | Parse |

## Verify a copied file

```bash
git clone --depth 1 --branch 484 https://github.com/trinodb/trino.git /tmp/trino
diff -r <(cd /tmp/trino/docs/src/main/sphinx && find admin -name '*.md' | sort | xargs sha256sum) \
        <(cd upstream/trino/docs && find admin -name '*.md' | sort | xargs sha256sum)

git clone https://github.com/trinodb/charts.git /tmp/charts
git -C /tmp/charts checkout 19a890d26f990df745b0f920d1d6dd07fae2bdd1
diff /tmp/charts/charts/trino/values.yaml upstream/charts/charts/trino/values.yaml

git clone --depth 1 --branch 22 https://github.com/trinodb/trino-gateway.git /tmp/gw
diff -r /tmp/gw/docs/installation.md upstream/trino-gateway/docs/installation.md
```

## File types in this repository

Markdown, MyST include fragments (`.fragment`, Markdown text), YAML, JSON, Java properties files, SQL examples, PNG and SVG images, and the license texts. There are no Java, Go, Python, JavaScript, shell, Terraform, Helm template or Dockerfile files.

```bash
find . -path ./.git -prune -o -type f -print | sed 's/.*\.//' | sort | uniq -c | sort -rn
```

## Updating

To refresh for a newer release: check out the new tags, copy the same paths, regenerate the reference pages, then review every guide against the release notes between the two versions. Property names and chart keys change between releases, so the guides are only correct for the versions named at the top of this file.
