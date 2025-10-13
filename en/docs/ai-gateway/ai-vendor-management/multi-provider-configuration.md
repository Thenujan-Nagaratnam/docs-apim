# Multi Model Provider Configuration

This guide explains how to configure **multi model provider** AI Service Providers in WSO2 API Manager. These providers allow you to manage multiple AI models from various providers within a single service, enabling advanced routing strategies and failover capabilities.

## Supported Multi Model Provider Vendors

The following AI service providers support multiple model providers:

- **AWS Bedrock** - Multiple AI models from various providers through AWS
- **Azure AI Foundry** - Multiple AI models from various providers through Azure

## Configuration Process

### Step 1: Access Configuration

1. Login to the Admin Portal (`https://<hostname>:9443/admin`)
2. Navigate to the **AI Service Providers** section in the left navigation pane
3. Find the multi model provider service in the list of AI Service Providers and click on it to edit the configuration

### Step 2: Configure Model Providers

The **Model Provider(s)** section allows you to add and configure different AI model providers within the multi model provider service.

#### Adding Model Providers

1. Click the **"+ Add Model Provider"** button to add a new provider family
2. Configure each provider with the following details:

##### Provider Configuration Fields

<table>
    <thead>
        <tr>
            <th style="width: 30%">Field</th>
            <th style="width: 70%">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Provider Name</strong></td>
            <td>Name of the AI provider (e.g., Meta, Anthropic, DeepSeek, Cohere, xAI)</td>
        </tr>
        <tr>
            <td><strong>Models</strong></td>
            <td>List of model names/IDs available from this provider</td>
        </tr>
    </tbody>
</table>

!!! Note "Add Multiple Model Providers and models"
    Adding multiple models under a provider allows you to use advanced routing strategies such as failover, load balancing, and other traffic management options. You can configure these routing policies when creating AI APIs to control how requests are distributed among the available models. For more details, see [Multi-Model Routing Overview]({{base_path}}/ai-gateway/multi-model-routing/overview/).

#### Adding Models to a Provider

1. In the provider configuration, you'll see an input field labeled **Type Model name and press Enter**
2. Type the complete model name/ID and press Enter to add it to the provider
3. **You can add multiple models by typing your model name and pressing enter for each one.** This enables model-based load balancing and failover capabilities within the AI Gateway.
4. You can add or remove individual models as needed to match your requirements

### Step 3: Save Configuration

After configuring your model providers, click **Update** to apply the changes.

## Provider-Specific Details

### AWS Bedrock

- **Description**: Multiple AI models from various providers through AWS infrastructure
- **Documentation**: [AWS Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)

#### Example Provider Configurations

<table>
    <thead>
        <tr>
            <th style="width: 30%">Provider Name</th>
            <th style="width: 70%">Example Models</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Meta</strong></td>
            <td><code>us.meta.llama3-3-70b-instruct-v1:0</code>, <code>us.meta.llama4-maverick-17b-instruct-v1:0</code></td>
        </tr>
        <tr>
            <td><strong>DeepSeek</strong></td>
            <td><code>us.deepseek.r1-v1:0</code></td>
        </tr>
        <tr>
            <td><strong>Anthropic</strong></td>
            <td><code>us.anthropic.claude-3-5-sonnet-20240620-v1:0</code>, <code>us.anthropic.claude-sonnet-4-20250514-v1:0</code></td>
        </tr>
    </tbody>
</table>

#### Supported Region Prefixes

- **us-east-1 region**: Use `us.` prefix
  - Example: `us.anthropic.claude-3-5-sonnet-20240620-v1:0`
  - Example: `us.meta.llama3-3-70b-instruct-v1:0`
  - Example: `us.deepseek.r1-v1:0`

For a complete and up-to-date list of all supported models, see the [AWS Bedrock Supported Models](https://docs.aws.amazon.com/bedrock/latest/userguide/models-supported.html) documentation.

### Azure AI Foundry

- **Description**: Multiple AI models from various providers through Azure infrastructure
- **Documentation**: [Azure AI Foundry Documentation](https://learn.microsoft.com/azure/ai-studio/)

#### Example Provider Configurations

<table>
    <thead>
        <tr>
            <th style="width: 30%">Provider Name</th>
            <th style="width: 70%">Example Models</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong>Azure OpenAI</strong></td>
            <td><code>gpt-4o</code>, <code>gpt-4o-mini</code>, <code>o3-mini</code></td>
        </tr>
        <tr>
            <td><strong>Cohere</strong></td>
            <td><code>cohere-command-a</code></td>
        </tr>
        <tr>
            <td><strong>xAI</strong></td>
            <td><code>grok-3</code>, <code>grok-3-mini</code></td>
        </tr>
    </tbody>
</table>

For a complete and up-to-date list of all supported models, see the [Azure AI Foundry Supported Models](https://ai.azure.com/catalog/models) documentation.

## Benefits of Multi Model Provider Configuration

- **Vendor Diversity**: Access models from multiple AI providers through a single service
- **Advanced Routing**: Configure failover and load balancing across different providers
- **Cost Optimization**: Choose the most cost-effective model for specific use cases
- **Reliability**: Reduce dependency on a single provider
- **Flexibility**: Easily switch between different model families based on requirements

Once you have saved your changes, the updated multi model provider configuration will be applied and made available for use in your AI APIs, enabling seamless integration with the selected models from multiple providers.
