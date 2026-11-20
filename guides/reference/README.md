# Reference

Generated and hand-checked reference pages for Trino 484 and chart 1.45.0. Guides link here for exhaustive lists.

| Page | Content | How it was made |
|---|---|---|
| [config-properties/README.md](config-properties/README.md) | All 1,402 configuration property declarations in the main sources, 1,385 unique names, in 208 classes across 59 modules. A to Z index plus one page per module with type, default expression, description, legacy names and removed names. | Extracted from `@Config`, `@ConfigDescription`, `@LegacyConfig` and `@DefunctConfig` annotations in the Java sources. The count matches the number of `@Config` annotations exactly. |
| [session-properties-from-source.md](session-properties-from-source.md) | All 374 session properties: system properties and those of each connector | Extracted from the `*SessionProperties` classes. Checked against every property name constant in those classes. |
| [documented-properties.md](documented-properties.md) | The 209 server properties described in the upstream `admin/properties-*.md` pages, with documented type, default and session property | Extracted from the upstream documentation |
| [plugins-and-connectors.md](plugins-and-connectors.md) | Every connector, access control, authenticator, event listener, exchange manager, cache manager and other plugin with the name that selects it | Extracted from factory classes in the sources, cross-linked to docs and property pages |
| [error-codes.md](error-codes.md) | All 188 engine error codes by type | Extracted from `StandardErrorCode` |
| [helm-values-index-trino.md](helm-values-index-trino.md) | All 222 values of the `trino` chart with type, default and first line of description | Extracted from the chart README |
| [helm-values-index-gateway.md](helm-values-index-gateway.md) | All 64 values of the `trino-gateway` chart | Extracted from the chart README |
| [chart-templates.md](chart-templates.md) | What every chart template renders: objects, names, conditions, the exact lines written to each Trino file, probes, ports, failures | Written from the chart templates |
| [docker-image.md](docker-image.md) | Image layout, user, defaults, health check, differences from the chart | Written from the image build files |

## Which reference answers which question

| Question | Page |
|---|---|
| Does property X exist in 484? | `config-properties/README.md`, A to Z table |
| What is its default? | `documented-properties.md` first. Then the module page under `config-properties/`, where the default is the Java initialiser. |
| Was it renamed or removed? | Module page: "Legacy names" in the Notes column, "Removed properties" under each table |
| Which session property overrides it? | `documented-properties.md`, last column |
| What is the `connector.name` for a data source? | `plugins-and-connectors.md` |
| Which Helm key writes it? | `chart-templates.md`, then `helm-values-index-trino.md` |
| What does this error name mean? | `error-codes.md`, then guide 17 |

## Reading default expressions

Defaults in the module pages are Java source expressions, not formatted values.

| Expression | Value |
|---|---|
| `DataSize.of(64, MEGABYTE)` | `64MB` |
| `new Duration(5, MINUTES)` | `5m` |
| `Optional.empty()` | Unset |
| `ImmutableList.of()` | Empty list |
| An enum constant such as `RetryPolicy.NONE` | `NONE` |
| An arithmetic expression or a method call | Computed at startup, often from heap size or CPU count |
| Empty cell | No initialiser in that class. The default is set elsewhere or the property is unset. |

## Limits of the generated pages

- Properties of the Airlift framework are not in the Trino sources and so not in `config-properties/`. The documented ones are in `documented-properties.md`.
- A few configuration classes are bound under a prefix or reused by several plugins. The property key shown is the one declared in the class.
- Table, schema, column, materialized view and analyze properties are defined in code without annotations. See each connector page.
- Descriptions are the developers' own one-line texts. Many properties have none.
