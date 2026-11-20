# Configuration properties: `plugin/trino-opa`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [OpaConfig](#opaconfig)

## OpaConfig

Source: `plugin/trino-opa/src/main/java/io/trino/plugin/opa/OpaConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `opa.allow-permission-management-operations` | boolean | `false` | Whether to allow permission management (GRANT, DENY, ...) and role management operations - OPA will not be queried for any such operations, they will be bulk allowed or denied depending on this setting |  |
| `opa.context-file` | Path | `Optional.empty()` | Optional properties file, containing user defined properties (e.g. tenant namespace, tier or cluster) to be included in the OPA query context |  |
| `opa.identity.extra-credentials-keys` | Set<String> | `ImmutableSet.of()` | Comma-separated list of extra credential keys from client connections to forward to OPA in the identity context |  |
| `opa.log-requests` | boolean | `false` | Whether to log requests (URI, entire body and headers) prior to sending them to OPA |  |
| `opa.log-responses` | boolean | `false` | Whether to log responses (URI, entire body, status code and headers) received from OPA |  |
| `opa.policy.batch-column-masking-uri` | URI | `Optional.empty()` | URI for fetching batch column masks - if not set column-masking-uri will be used |  |
| `opa.policy.batched-uri` | URI | `Optional.empty()` | URI for Batch OPA policies - if not set, a single request will be sent for each entry on filtering methods |  |
| `opa.policy.column-masking-uri` | URI | `Optional.empty()` | URI for fetching column masks - if not set no masking will be applied |  |
| `opa.policy.row-filters-uri` | URI | `Optional.empty()` | URI for fetching row filters - if not set no row filtering will be applied |  |
| `opa.policy.uri` | URI |  | URI for OPA policies |  |
