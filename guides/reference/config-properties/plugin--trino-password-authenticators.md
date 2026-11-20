# Configuration properties: `plugin/trino-password-authenticators`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [FileConfig](#fileconfig), [FileGroupConfig](#filegroupconfig), [LdapAuthenticatorConfig](#ldapauthenticatorconfig), [SalesforceConfig](#salesforceconfig)

## FileConfig

Source: `plugin/trino-password-authenticators/src/main/java/io/trino/plugin/password/file/FileConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `file.auth-token-cache.max-size` | int | `1000` | Max number of cached authenticated passwords |  |
| `file.password-file` | File |  | Location of the file that provides user names and passwords |  |
| `file.refresh-period` | Duration | `new Duration(5, SECONDS)` | How often to reload the password file |  |

## FileGroupConfig

Source: `plugin/trino-password-authenticators/src/main/java/io/trino/plugin/password/file/FileGroupConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `file.group-file` | File |  | Location of the file that provides user group membership |  |
| `file.refresh-period` | Duration | `new Duration(5, SECONDS)` | How often to reload the group file |  |

## LdapAuthenticatorConfig

Source: `plugin/trino-password-authenticators/src/main/java/io/trino/plugin/password/ldap/LdapAuthenticatorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ldap.bind-dn` | String |  | Bind distinguished name. Example: CN=User Name,OU=CITY_OU,OU=STATE_OU,DC=domain,DC=domain_root |  |
| `ldap.bind-password` | String |  | Bind password used. Example: password1234 | sensitive |
| `ldap.cache-ttl` | Duration | `new Duration(1, TimeUnit.HOURS)` |  |  |
| `ldap.group-auth-pattern` | String |  | Custom group authorization check query. Example: &(objectClass=user)(memberOf=cn=group)(user=username) |  |
| `ldap.user-base-dn` | String |  | Base distinguished name of the user. Example: dc=example,dc=com |  |
| `ldap.user-bind-pattern` | String | `ImmutableList.of()` | Custom user bind pattern. Example: ${USER}@example.com |  |

## SalesforceConfig

Source: `plugin/trino-password-authenticators/src/main/java/io/trino/plugin/password/salesforce/SalesforceConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `salesforce.allowed-organizations` | String |  | Comma separated list of Salesforce 18 Character Organization Ids. |  |
| `salesforce.cache-expire-duration` | Duration | `Duration.succinctDuration(2, TimeUnit.MINUTES)` | Expire duration for an entry in cache since last write. |  |
| `salesforce.cache-size` | int | `4096` | Maximum size of the cache that holds authenticated users. Default is 4096 entries. |  |
