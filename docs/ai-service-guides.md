# Azure AI Service Guides

This section provides practical, step-by-step guides for each major Azure AI service, including deployment, usage, security, and cost management tips.

---

## 1. Azure OpenAI Service

### 1.1 Deploying Azure OpenAI Service

#### Basic Setup
- Go to Azure Portal, search for "Azure OpenAI Service", and create a new resource.
- Choose region (recommended: East US, West Europe for better availability).
- Select pricing tier (Standard is recommended for production).
- Assign to a resource group and provide a unique name.

#### Post-Deployment Configuration
- Wait for deployment completion (usually 5-10 minutes).
- Navigate to your Azure OpenAI resource.
- Go to "Keys and Endpoint" to get your API key and endpoint URL.
- Navigate to "Model deployments" to deploy your desired models (e.g., GPT-3.5-turbo, GPT-4).

#### Model Deployment
- Click "Create new deployment" in the Azure OpenAI Studio.
- Select model (GPT-3.5-turbo recommended for beginners).
- Choose deployment name (e.g., "gpt-35-turbo-deployment").
- Set tokens per minute rate limit based on your needs.

### 1.2 API Usage Examples

#### Python Example
```python
import openai
import os
from openai import AzureOpenAI

# Initialize the client
client = AzureOpenAI(
    api_key=os.getenv("AZURE_OPENAI_KEY"),
    api_version="2024-02-01",
    azure_endpoint=os.getenv("AZURE_OPENAI_ENDPOINT")
)

# Chat completion
response = client.chat.completions.create(
    model="gpt-35-turbo-deployment",  # Your deployment name
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": "Hello, how can you help me today?"}
    ],
    max_tokens=100,
    temperature=0.7
)

print(response.choices[0].message.content)
```

#### JavaScript/Node.js Example
```javascript
const { OpenAI } = require('openai');

const openai = new OpenAI({
  apiKey: process.env.AZURE_OPENAI_KEY,
  baseURL: `${process.env.AZURE_OPENAI_ENDPOINT}/openai/deployments/gpt-35-turbo-deployment`,
  defaultQuery: { 'api-version': '2024-02-01' },
  defaultHeaders: {
    'api-key': process.env.AZURE_OPENAI_KEY,
  },
});

async function chatWithAI() {
  try {
    const response = await openai.chat.completions.create({
      model: 'gpt-35-turbo-deployment',
      messages: [
        { role: 'system', content: 'You are a helpful assistant.' },
        { role: 'user', content: 'Hello, how can you help me today?' }
      ],
      max_tokens: 100,
      temperature: 0.7
    });
    
    console.log(response.choices[0].message.content);
  } catch (error) {
    console.error('Error:', error);
  }
}

chatWithAI();
```

#### C# Example
```csharp
using Azure;
using Azure.AI.OpenAI;

var client = new OpenAIClient(
    new Uri(Environment.GetEnvironmentVariable("AZURE_OPENAI_ENDPOINT")),
    new AzureKeyCredential(Environment.GetEnvironmentVariable("AZURE_OPENAI_KEY"))
);

var chatCompletionsOptions = new ChatCompletionsOptions()
{
    DeploymentName = "gpt-35-turbo-deployment",
    Messages =
    {
        new ChatRequestSystemMessage("You are a helpful assistant."),
        new ChatRequestUserMessage("Hello, how can you help me today?"),
    },
    MaxTokens = 100,
    Temperature = 0.7f
};

Response<ChatCompletions> response = await client.GetChatCompletionsAsync(chatCompletionsOptions);
Console.WriteLine(response.Value.Choices[0].Message.Content);
```

### 1.3 Advanced Features and Use Cases

#### Working with Embeddings
```python
# Generate embeddings for text similarity and search
response = client.embeddings.create(
    model="text-embedding-ada-002",  # Your embedding model deployment
    input="Your text to embed here"
)

embedding = response.data[0].embedding
print(f"Embedding vector length: {len(embedding)}")
```

#### Streaming Responses
```python
# Stream responses for better user experience
stream = client.chat.completions.create(
    model="gpt-35-turbo-deployment",
    messages=[{"role": "user", "content": "Tell me a story"}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="")
```

#### Function Calling
```python
# Define functions for the model to call
functions = [
    {
        "name": "get_weather",
        "description": "Get current weather information",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "The city and state, e.g. San Francisco, CA"
                }
            },
            "required": ["location"]
        }
    }
]

response = client.chat.completions.create(
    model="gpt-35-turbo-deployment",
    messages=[{"role": "user", "content": "What's the weather like in Tokyo?"}],
    functions=functions,
    function_call="auto"
)
```

### 1.4 Model Selection Guide

#### GPT-3.5 Turbo
- **Best for**: General conversations, content generation, basic coding tasks
- **Pros**: Fast, cost-effective, good performance
- **Use cases**: Chatbots, content creation, simple Q&A

#### GPT-4
- **Best for**: Complex reasoning, advanced coding, detailed analysis
- **Pros**: Superior reasoning, better code generation, more accurate
- **Use cases**: Code review, complex problem solving, research assistance

#### Text-Embedding-Ada-002
- **Best for**: Semantic search, similarity matching, clustering
- **Use cases**: Document search, recommendation systems, content classification

### 1.5 Error Handling and Troubleshooting

#### Common Errors and Solutions
```python
import openai
from openai import RateLimitError, APIError, AuthenticationError

try:
    response = client.chat.completions.create(
        model="gpt-35-turbo-deployment",
        messages=[{"role": "user", "content": "Hello"}]
    )
except AuthenticationError as e:
    print(f"Authentication failed: {e}")
    # Check your API key and endpoint
except RateLimitError as e:
    print(f"Rate limit exceeded: {e}")
    # Implement exponential backoff
except APIError as e:
    print(f"API error: {e}")
    # Check service status and retry
except Exception as e:
    print(f"Unexpected error: {e}")
```

#### Rate Limiting Best Practices
```python
import time
import random

def make_request_with_backoff(func, max_retries=3):
    for attempt in range(max_retries):
        try:
            return func()
        except RateLimitError:
            if attempt == max_retries - 1:
                raise
            wait_time = (2 ** attempt) + random.uniform(0, 1)
            time.sleep(wait_time)
```

### 1.6 Security Best Practices

#### API Key Management
- Store API keys in Azure Key Vault or environment variables
- Use Managed Identity when possible for Azure-hosted applications
- Rotate API keys regularly
- Never commit API keys to source control

#### Network Security
```python
# Use Azure Private Endpoints for enhanced security
# Configure firewall rules to restrict access
# Example: Restrict to specific IP ranges in Azure Portal
```

#### Input Validation and Content Filtering
```python
# Implement content filtering
def validate_input(user_input):
    # Check for malicious content
    if len(user_input) > 4000:  # Token limit consideration
        raise ValueError("Input too long")
    
    # Add custom validation logic
    prohibited_terms = ["harmful_term1", "harmful_term2"]
    if any(term in user_input.lower() for term in prohibited_terms):
        raise ValueError("Prohibited content detected")
    
    return user_input

# Use Azure Content Safety service for additional filtering
```

### 1.7 Token Usage Optimization

#### Understanding Tokens
- 1 token ≈ 4 characters in English
- Both input and output count towards usage
- Monitor token usage to optimize costs

#### Optimization Strategies
```python
# 1. Use system messages efficiently
system_message = "Be concise."  # Short but effective

# 2. Limit max_tokens for responses
response = client.chat.completions.create(
    model="gpt-35-turbo-deployment",
    messages=[{"role": "user", "content": "Explain AI"}],
    max_tokens=50  # Limit response length
)

# 3. Use temperature wisely
# Lower temperature (0.1-0.3) for factual responses
# Higher temperature (0.7-0.9) for creative content
```

### 1.8 Integration with Other Azure Services

#### Azure Functions Integration
```python
import azure.functions as func
from azure.ai.openai import AzureOpenAI

def main(req: func.HttpRequest) -> func.HttpResponse:
    client = AzureOpenAI(
        api_key=os.environ["AZURE_OPENAI_KEY"],
        api_version="2024-02-01",
        azure_endpoint=os.environ["AZURE_OPENAI_ENDPOINT"]
    )
    
    user_input = req.get_json().get('message', '')
    
    response = client.chat.completions.create(
        model="gpt-35-turbo-deployment",
        messages=[{"role": "user", "content": user_input}]
    )
    
    return func.HttpResponse(
        response.choices[0].message.content,
        mimetype="application/json"
    )
```

#### Azure Logic Apps Integration
- Use HTTP connector to call Azure OpenAI endpoints
- Implement workflows that process OpenAI responses
- Connect with other Azure services like Cosmos DB, Service Bus

#### Azure Cognitive Search + OpenAI
```python
# Combine search with OpenAI for RAG (Retrieval-Augmented Generation)
def search_and_generate(query):
    # 1. Search relevant documents
    search_results = cognitive_search_client.search(query)
    
    # 2. Use results as context for OpenAI
    context = "\n".join([result['content'] for result in search_results])
    
    response = client.chat.completions.create(
        model="gpt-35-turbo-deployment",
        messages=[
            {"role": "system", "content": f"Use this context: {context}"},
            {"role": "user", "content": query}
        ]
    )
    
    return response.choices[0].message.content
```

### 1.9 Pricing, Quotas, and Best Practices

#### Pricing Structure
- **GPT-3.5 Turbo**: ~$0.002 per 1K tokens
- **GPT-4**: ~$0.03-$0.06 per 1K tokens (varies by model)
- **Embeddings**: ~$0.0001 per 1K tokens
- Review current [pricing](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/)

#### Quota Management
- Default quotas vary by region and model
- Request quota increases through Azure Portal
- Monitor usage in Azure Portal dashboard
- Set up billing alerts to avoid unexpected charges

#### Cost Optimization Tips
- Use GPT-3.5 Turbo for simple tasks
- Cache responses when appropriate
- Implement request batching
- Use shorter prompts when possible
- Monitor and analyze token usage patterns

---

## 2. Azure Cognitive Services

### 2.1 Service Types

- Vision (Computer Vision, Face, OCR)
- Speech (Speech-to-Text, Text-to-Speech)
- Language (Text Analytics, Translator)
- Decision (Personalizer, Anomaly Detector)

### 2.2 Creating and Securing a Service

- Create from Azure Portal, get keys and endpoint
- Restrict access with network rules and Key Vault

### 2.3 Calling the API (REST Example)

```bash
curl -X POST "https://<your-endpoint>/vision/v3.2/analyze" \
  -H "Ocp-Apim-Subscription-Key: <your-key>" \
  -H "Content-Type: application/json" \
  --data '{"url": "https://example.com/image.jpg"}'
```

---

## 3. Azure Machine Learning

### 3.1 Workspace Setup

- Create an Azure ML workspace from the portal or CLI
- Attach compute resources (CPU/GPU)

### 3.2 Model Training & Deployment

- Use Azure ML Studio or SDK for training
- Deploy models as web services (ACI/AKS)

### 3.3 MLOps Pipeline Basics

- Use Azure DevOps or GitHub Actions for CI/CD
- Track experiments and models with MLflow

---

## 4. Azure Functions & Logic Apps

### 4.1 Serverless AI Integration

- Use Functions to call AI services on demand
- Example: HTTP-triggered Function that calls OpenAI API

### 4.2 Sample Function (Python)

Below is an example of an Azure Functions HTTP-triggered function (Python) that calls Azure OpenAI Service using the latest `azure-ai-openai` library. This sample assumes you have set the endpoint, API key, and deployment name as environment variables.

```python
import os
import json
import azure.functions as func
from azure.ai.openai import AzureOpenAI

def main(req: func.HttpRequest) -> func.HttpResponse:
    try:
        # Get the request body
        req_body = req.get_json()
        if not req_body or 'message' not in req_body:
            return func.HttpResponse(
                json.dumps({"error": "Missing 'message' field in request body"}),
                status_code=400,
                mimetype="application/json"
            )

        user_message = req_body['message']
        
        # Initialize Azure OpenAI client
        client = AzureOpenAI(
            api_key=os.environ["AZURE_OPENAI_KEY"],
            api_version="2024-02-01",
            azure_endpoint=os.environ["AZURE_OPENAI_ENDPOINT"]
        )

        # Create chat completion
        response = client.chat.completions.create(
            model=os.environ["AZURE_OPENAI_DEPLOYMENT"],
            messages=[
                {"role": "system", "content": "You are a helpful assistant."},
                {"role": "user", "content": user_message}
            ],
            max_tokens=500,
            temperature=0.7
        )

        return func.HttpResponse(
            json.dumps({
                "response": response.choices[0].message.content,
                "usage": {
                    "prompt_tokens": response.usage.prompt_tokens,
                    "completion_tokens": response.usage.completion_tokens,
                    "total_tokens": response.usage.total_tokens
                }
            }),
            mimetype="application/json"
        )

    except Exception as e:
        return func.HttpResponse(
            json.dumps({"error": str(e)}),
            status_code=500,
            mimetype="application/json"
        )
```

#### Required Environment Variables:
- `AZURE_OPENAI_ENDPOINT`: Your Azure OpenAI service endpoint
- `AZURE_OPENAI_KEY`: Your Azure OpenAI API key  
- `AZURE_OPENAI_DEPLOYMENT`: Your model deployment name

#### Required Dependencies (requirements.txt):
```
azure-functions
azure-ai-openai
```

#### Sample Request:
```bash
curl -X POST "https://your-function-app.azurewebsites.net/api/your-function" \
  -H "Content-Type: application/json" \
  -d '{"message": "Hello, how can you help me today?"}'
```

---

## 5. Useful Links

- [Azure AI Documentation](https://learn.microsoft.com/azure/ai-services/)
- [Azure OpenAI Docs](https://learn.microsoft.com/azure/ai-services/openai/)
- [Azure ML Documentation](https://learn.microsoft.com/azure/machine-learning/)
- [Azure Functions Docs](https://learn.microsoft.com/azure/azure-functions/)
