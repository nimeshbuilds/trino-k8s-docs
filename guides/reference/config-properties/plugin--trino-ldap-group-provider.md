# Configuration properties: `plugin/trino-ldap-group-provider`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [LdapFilteringGroupProviderConfig](#ldapfilteringgroupproviderconfig), [LdapGroupProviderConfig](#ldapgroupproviderconfig), [LdapSingleQueryGroupProviderConfig](#ldapsinglequerygroupproviderconfig)

## LdapFilteringGroupProviderConfig

Source: `plugin/trino-ldap-group-provider/src/main/java/io/trino/plugin/ldapgroup/LdapFilteringGroupProviderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ldap.group-base-dn` | String |  | Base distinguished name for groups. Example: dc=example,dc=com |  |
| `ldap.group-search-filter` | String |  | Search filter for group documents. Example: (cn=trino_*) |  |
| `ldap.group-search-member-attribute` | String | `"member"` | Attribute from group documents used for filtering by member. Example: cn |  |

## LdapGroupProviderConfig

Source: `plugin/trino-ldap-group-provider/src/main/java/io/trino/plugin/ldapgroup/LdapGroupProviderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ldap.admin-password` | String |  | Bind password used for admin user. Example: password1234 | sensitive |
| `ldap.admin-user` | String |  | Bind distinguished name for admin user. Example: CN=User Name,OU=CITY_OU,OU=STATE_OU,DC=domain,DC=domain_root |  |
| `ldap.group-name-attribute` | String | `"cn"` | Attribute from group documents used as the name. Example: cn |  |
| `ldap.use-group-filter` | boolean | `false` | Enable group filtering |  |
| `ldap.user-base-dn` | String |  | Base distinguished name for users. Example: dc=example,dc=com |  |
| `ldap.user-search-filter` | String | `"(uid={0})"` | Search filter for user documents, templated with the Trino username. Example: (cn={0}) |  |

## LdapSingleQueryGroupProviderConfig

Source: `plugin/trino-ldap-group-provider/src/main/java/io/trino/plugin/ldapgroup/LdapSingleQueryGroupProviderConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ldap.user-member-of-attribute` | String | `"memberOf"` | Group membership attribute in user documents. Example: memberOf |  |
