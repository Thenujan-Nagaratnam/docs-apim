# Single Model Provider Configuration

This guide explains how to configure **single model provider** AI Service Providers in WSO2 API Manager. These providers offer models from a single AI provider and have a straightforward configuration process.

## Supported Single Model Provider Vendors

The following AI service providers are available as single model providers:

- **Anthropic** - Claude family of models
- **Azure OpenAI** - OpenAI models through Azure infrastructure
- **Gemini** - Google's advanced language models
- **Mistral AI** - High-performance language models
- **OpenAI** - GPT and other OpenAI models  

## Configuration Process

### Step 1: Access Configuration

1. Login to the Admin Portal (`https://<hostname>:9443/admin`)
2. Navigate to **AI Service Providers** → **[Provider Name]**

### Step 2: Configure Models

#### Read-Only Configurations

The following configurations are **read-only** and cannot be modified:

<table>
    <thead>
        <tr>
            <th style="width: 30%">Category</th>
            <th style="width: 70%">Fields</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>General Details</strong></td>
            <td>
                • Name<br>
                • API Version<br>
                • Description
            </td>
        </tr>
        <tr>
            <td><strong>LLM Configurations</strong></td>
            <td>
                • Request Model<br>
                • Response Model<br>
                • Prompt Token Count<br>
                • Completion Token Count<br>
                • Total Token Count<br>
                • Remaining Token Count
            </td>
        </tr>
        <tr>
            <td><strong>LLM Provider Auth Configurations</strong></td>
            <td>
                • Auth Type: Header, Query Parameter or Unsecured<br>
                • Auth Type Identifier: Header/Query Parameter Identifier
            </td>
        </tr>
        <tr>
            <td><strong>Connector Type for AI Service Provider</strong></td>
            <td>
                • Connector Type
            </td>
        </tr>
    </tbody>
</table>

#### Editable Configurations

The following configurations can be updated:

<table>
    <thead>
        <tr>
            <th style="width: 30%">Category</th>
            <th style="width: 70%">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>API Definition</strong></td>
            <td>AI service provider exposed API definition file</td>
        </tr>
        <tr>
            <td><strong>Model List</strong></td>
            <td>Add the list of models supported by the AI service provider. This list enables you to configure routing strategies within your AI APIs.</td>
        </tr>
    </tbody>
</table>

### Step 3: Save Configuration

Click **Update** to apply your changes.

## Provider-Specific Details

### Anthropic

- **Description**: Claude family of advanced language models designed for safety and helpfulness
- **Documentation**: [Anthropic API Documentation](https://docs.anthropic.com/)
- **Default Models**: `claude-opus-4-1-20250805`, `claude-sonnet-4-20250514`, `claude-3-7-sonnet-20250219`


### Azure OpenAI

- **Description**: OpenAI models through Azure's infrastructure
- **Documentation**: [Azure OpenAI Documentation](https://learn.microsoft.com/en-us/azure/ai-foundry/openai/reference)
- **Default Models**: N/A (configure with your Azure OpenAI deployment ids)

### Gemini

- **Description**: Google's advanced language models
- **Documentation**: [Gemini API Documentation](https://ai.google.dev/docs)
- **Default Models**: `gemini-2.5-flash-lite`, `gemini-2.5-flash`, `gemini-2.5-pro`

### Mistral AI

- **Description**: High-performance language models through Mistral's API
- **Documentation**: [Mistral AI Documentation](https://docs.mistral.ai/)
- **Default Models**: `mistral-small-latest`, `mistral-medium`, `open-mistral-7b`

### OpenAI

- **Description**: OpenAI's advanced language models including GPT series
- **Documentation**: [OpenAI API Documentation](https://platform.openai.com/docs)
- **Default Models**: `gpt-4o`, `gpt-4o-mini`, `o3-mini`

## Model Configuration

- To add available models supported by the provider, type the model name and press enter
- This enables model-based load balancing and failover capabilities
- For more details, see [Multi-Model Routing Overview]({{base_path}}/ai-gateway/multi-model-routing/overview/)

Once you have saved your changes, the updated configuration will be applied and made available for use in your AI APIs, enabling seamless integration with the selected models.
