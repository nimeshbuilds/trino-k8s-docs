# Configuration properties: `lib/trino-filesystem`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [LocalFileSystemConfig](#localfilesystemconfig)

## LocalFileSystemConfig

Source: `lib/trino-filesystem/src/main/java/io/trino/filesystem/local/LocalFileSystemConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `local.location` | Path | `Path.of(System.getProperty(JAVA_IO_TMPDIR.key()))` | Local file system root directory |  |
