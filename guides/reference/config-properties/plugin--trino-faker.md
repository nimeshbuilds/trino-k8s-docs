# Configuration properties: `plugin/trino-faker`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [FakerConfig](#fakerconfig)

## FakerConfig

Source: `plugin/trino-faker/src/main/java/io/trino/plugin/faker/FakerConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `faker.default-limit` | long | `1000L` | Default number of rows for each table, when the LIMIT clause is not specified in the query |  |
| `faker.dictionary-detection-enabled` | boolean | `true` | If true, when creating a table using existing data, columns with a low number of distinct values are treated as dictionaries, and get the allowed_values column property populated with random values |  |
| `faker.locale` | String | `Locale.ENGLISH` | Default locale for generating character based data, specified as a IETF BCP 47 language tag string |  |
| `faker.null-probability` | double | `0.5` | Default null probability for any column in any table that allows them |  |
| `faker.sequence-detection-enabled` | boolean | `true` | If true, when creating a table using existing data, columns with the number of distinct values close to the number of rows are treated as sequences |  |
