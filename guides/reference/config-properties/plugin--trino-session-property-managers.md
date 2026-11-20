# Configuration properties: `plugin/trino-session-property-managers`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [DbSessionPropertyManagerConfig](#dbsessionpropertymanagerconfig), [FileSessionPropertyManagerConfig](#filesessionpropertymanagerconfig)

## DbSessionPropertyManagerConfig

Source: `plugin/trino-session-property-managers/src/main/java/io/trino/plugin/session/db/DbSessionPropertyManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `session-property-manager.db.password` | String |  |  | sensitive |
| `session-property-manager.db.refresh-period` | Duration | `new Duration(10, SECONDS)` |  |  |
| `session-property-manager.db.url` | String |  |  |  |
| `session-property-manager.db.username` | String |  |  |  |

## FileSessionPropertyManagerConfig

Source: `plugin/trino-session-property-managers/src/main/java/io/trino/plugin/session/file/FileSessionPropertyManagerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `session-property-manager.config-file` | File |  |  |  |
