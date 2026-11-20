# Configuration properties: `plugin/trino-memory`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [MemoryConfig](#memoryconfig)

## MemoryConfig

Source: `plugin/trino-memory/src/main/java/io/trino/plugin/memory/MemoryConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `memory.enable-lazy-dynamic-filtering` | boolean | `true` |  |  |
| `memory.max-data-per-node` | DataSize | `DataSize.of(128, DataSize.Unit.MEGABYTE)` |  |  |
| `memory.splits-per-node` | int | `Runtime.getRuntime().availableProcessors()` |  |  |
