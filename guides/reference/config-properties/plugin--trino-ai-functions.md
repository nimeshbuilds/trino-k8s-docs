# Configuration properties: `plugin/trino-ai-functions`

Extracted from the Java sources of Trino release 484. Each table is one configuration class. See the [index](README.md) for how to read these pages.

Classes: [AiConfig](#aiconfig), [AnthropicConfig](#anthropicconfig), [OpenAiConfig](#openaiconfig)

## AiConfig

Source: `plugin/trino-ai-functions/src/main/java/io/trino/plugin/ai/functions/AiConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ai.analyze-sentiment.model` | String |  |  |  |
| `ai.classify.model` | String |  |  |  |
| `ai.extract.model` | String |  |  |  |
| `ai.fix-grammar.model` | String |  |  |  |
| `ai.generate.model` | String |  |  |  |
| `ai.mask.model` | String |  |  |  |
| `ai.model` | String |  |  |  |
| `ai.provider` | AiProvider |  |  |  |
| `ai.translate.model` | String |  |  |  |

## AnthropicConfig

Source: `plugin/trino-ai-functions/src/main/java/io/trino/plugin/ai/functions/AnthropicConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ai.anthropic.api-key` | String |  |  |  |
| `ai.anthropic.endpoint` | URI | `URI.create("https://api.anthropic.com")` |  |  |

## OpenAiConfig

Source: `plugin/trino-ai-functions/src/main/java/io/trino/plugin/ai/functions/OpenAiConfig.java`

| Property | Type | Default in source | Description | Notes |
|---|---|---|---|---|
| `ai.openai.api-key` | String |  |  |  |
| `ai.openai.endpoint` | URI | `URI.create("https://api.openai.com")` |  |  |
