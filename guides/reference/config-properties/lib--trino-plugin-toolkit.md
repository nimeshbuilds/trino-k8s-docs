# Configuration properties: `lib/trino-plugin-toolkit`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [AwsKmsConfig](#awskmsconfig), [FileBasedAccessControlConfig](#filebasedaccesscontrolconfig), [LdapClientConfig](#ldapclientconfig), [MappingConfig](#mappingconfig), [ObjectNameGeneratorConfig](#objectnamegeneratorconfig)

## AwsKmsConfig

Source: `lib/trino-plugin-toolkit/src/main/java/io/trino/plugin/base/encryption/AwsKmsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `aws.kms.access-key` | String | `Optional.empty()` | AWS KMS access key |  |
| `aws.kms.endpoint` | URI | `Optional.empty()` | KMS API endpoint URL |  |
| `aws.kms.external-id` | String | `Optional.empty()` | External ID for the IAM role trust policy when connecting to KMS |  |
| `aws.kms.iam-role` | String | `Optional.empty()` | ARN of an IAM role to assume when connecting to KMS |  |
| `aws.kms.region` | String | `Optional.empty()` | AWS Region for KMS |  |
| `aws.kms.secret-key` | String | `Optional.empty()` | AWS KMS secret key | sensitive |
| `aws.kms.sts.endpoint` | URI | `Optional.empty()` | AWS STS endpoint for KMS authentication |  |
| `aws.kms.sts.region` | String | `Optional.empty()` | AWS STS signing region for KMS authentication |  |

## FileBasedAccessControlConfig

Source: `lib/trino-plugin-toolkit/src/main/java/io/trino/plugin/base/security/FileBasedAccessControlConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `security.config-file` | String |  |  |  |
| `security.json-pointer` | String | `""` | JSON pointer (RFC 6901) to mappings inside JSON config |  |
| `security.refresh-period` | Duration |  |  |  |

## LdapClientConfig

Source: `lib/trino-plugin-toolkit/src/main/java/io/trino/plugin/base/ldap/LdapClientConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ldap.allow-insecure` | boolean | `false` | Allow insecure connection to the LDAP server |  |
| `ldap.ignore-referrals` | boolean | `false` | Referrals allow finding entries across multiple LDAP servers. Ignore them to only search within 1 LDAP server |  |
| `ldap.ssl.keystore.password` | String |  | Password for the key store | sensitive |
| `ldap.ssl.keystore.path` | File |  | Path to the PEM or JKS key store |  |
| `ldap.ssl.truststore.password` | String |  | Password for the trust store | sensitive |
| `ldap.ssl.truststore.path` | File |  | Path to the PEM or JKS trust store | Legacy names: `ldap.ssl-trust-certificate` |
| `ldap.timeout.connect` | Duration | `succinctDuration(1, MINUTES)` | Timeout for establishing a connection |  |
| `ldap.timeout.read` | Duration | `succinctDuration(1, MINUTES)` | Timeout for reading data from LDAP |  |
| `ldap.url` | String |  | URL of the LDAP server |  |

## MappingConfig

Source: `lib/trino-plugin-toolkit/src/main/java/io/trino/plugin/base/mapping/MappingConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `case-insensitive-name-matching` | boolean | `false` |  |  |
| `case-insensitive-name-matching.cache-ttl` | Duration | `new Duration(1, MINUTES)` |  |  |
| `case-insensitive-name-matching.config-file` | String |  |  |  |
| `case-insensitive-name-matching.config-file.refresh-period` | Duration |  |  |  |

## ObjectNameGeneratorConfig

Source: `lib/trino-plugin-toolkit/src/main/java/io/trino/plugin/base/jmx/ObjectNameGeneratorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `jmx.base-name` | String |  |  |  |
