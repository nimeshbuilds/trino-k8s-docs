# Configuration properties: `plugin/trino-ranger`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [RangerConfig](#rangerconfig)

## RangerConfig

Source: `plugin/trino-ranger/src/main/java/io/trino/plugin/ranger/RangerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ranger.hadoop.config.resource` | List<File> | `ImmutableList.of()` | List of paths to hadoop configuration files |  |
| `ranger.plugin.config.resource` | List<File> | `ImmutableList.of()` | List of paths to Ranger plugin configuration files |  |
| `ranger.service.name` | String |  | Name of Ranger service containing policies to enforce |  |
