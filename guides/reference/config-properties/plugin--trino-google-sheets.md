# Configuration properties: `plugin/trino-google-sheets`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [SheetsConfig](#sheetsconfig)

## SheetsConfig

Source: `plugin/trino-google-sheets/src/main/java/io/trino/plugin/google/sheets/SheetsConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `gsheets.connection-timeout` | Duration | `new Duration(20, TimeUnit.SECONDS)` | Timeout when connection to Google Sheets API |  |
| `gsheets.credentials-key` | String | `Optional.empty()` | The base64 encoded credentials key | sensitive |
| `gsheets.credentials-path` | String | `Optional.empty()` | Credential file path to google service account | Legacy names: `credentials-path` |
| `gsheets.data-cache-ttl` | Duration | `new Duration(5, TimeUnit.MINUTES)` | Sheets data expire after write duration | Legacy names: `sheets-data-expire-after-write` |
| `gsheets.delegated-user-email` | String |  | Delegated user email to impersonate the service account |  |
| `gsheets.max-data-cache-size` | int | `1000` | Sheet data max cache size | Legacy names: `sheets-data-max-cache-size` |
| `gsheets.metadata-sheet-id` | String | `Optional.empty()` | Metadata sheet id containing table sheet mapping | Legacy names: `metadata-sheet-id` |
| `gsheets.read-timeout` | Duration | `new Duration(20, TimeUnit.SECONDS)` | Timeout when reading from Google Sheets API |  |
| `gsheets.write-timeout` | Duration | `new Duration(20, TimeUnit.SECONDS)` | Timeout when writing to Google Sheets API |  |
