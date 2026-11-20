# Configuration properties: `plugin/trino-resource-group-managers`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [DbResourceGroupConfig](#dbresourcegroupconfig), [FileResourceGroupConfig](#fileresourcegroupconfig), [ObjectNameGeneratorConfig](#objectnamegeneratorconfig)

## DbResourceGroupConfig

Source: `plugin/trino-resource-group-managers/src/main/java/io/trino/plugin/resourcegroups/db/DbResourceGroupConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `resource-groups.config-db-password` | String |  | Database password | sensitive |
| `resource-groups.config-db-url` | String |  |  |  |
| `resource-groups.config-db-user` | String |  | Database user name |  |
| `resource-groups.db-migrations-enabled` | boolean | `true` | Whether to run migrations on startup |  |
| `resource-groups.exact-match-selector-enabled` | boolean | `false` |  |  |
| `resource-groups.max-refresh-interval` | Duration | `new Duration(1, HOURS)` | Time period for which the cluster will continue to accept queries after refresh failures cause configuration to become stale |  |
| `resource-groups.refresh-interval` | Duration | `new Duration(1, SECONDS)` | How often the cluster reloads from the database |  |

## FileResourceGroupConfig

Source: `plugin/trino-resource-group-managers/src/main/java/io/trino/plugin/resourcegroups/FileResourceGroupConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `resource-groups.config-file` | String |  |  |  |

## ObjectNameGeneratorConfig

Source: `plugin/trino-resource-group-managers/src/main/java/io/trino/plugin/resourcegroups/db/ObjectNameGeneratorConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `jmx.base-name` | String |  |  |  |
