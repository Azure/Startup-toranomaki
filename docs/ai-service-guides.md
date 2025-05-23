# Azure AI Service Guides

This section provides practical, step-by-step guides for each major Azure AI service, including deployment, usage, security, and cost management tips.

---

## 1. Azure OpenAI Service

### 1.1 Deploying Azure OpenAI Service

- Go to Azure Portal, search for "Azure OpenAI Service", and create a new resource.
- Choose region, pricing tier, and assign to a resource group.
- Wait for deployment, then go to the resource to get your endpoint and API key.

### 1.2 Using the API (Python Example)

```python
import openai
openai.api_type = "azure"
openai.api_base = "<your-endpoint>"
openai.api_key = "<your-key>"
openai.api_version = "2023-05-15"

response = openai.ChatCompletion.create(
  engine="gpt-35-turbo",
  messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message["content"])
```

### 1.3 Pricing, Quotas, and Best Practices

- Review [pricing](https://azure.microsoft.com/pricing/details/cognitive-services/openai-service/)
- Request quota increases as needed
- Never expose API keys in client-side code

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

Below is an example of an Azure Functions HTTP-triggered function (Python) that calls Azure OpenAI Service using the `azure-functions` and `openai` libraries. This sample assumes you have set the endpoint, API key, and deployment name as environment variables.

```python
import os
import openai
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    prompt = req.params.get('prompt')
    if not prompt:
        return func.HttpResponse("Missing prompt", status_code=400)

    openai.api_type = "azure"
    openai.api_base = os.environ["AZURE_OPENAI_ENDPOINT"]
    openai.api_key = os.environ["AZURE_OPENAI_KEY"]
    openai.api_version = "2023-05-15"
    deployment = os.environ["AZURE_OPENAI_DEPLOYMENT"]

    response = openai.ChatCompletion.create(
        engine=deployment,
        messages=[{"role": "user", "content": prompt}]
    )
    return func.HttpResponse(response.choices[0].message["content"])
```

- Make sure to set the following environment variables in your Function App:
  - `AZURE_OPENAI_ENDPOINT`
  - `AZURE_OPENAI_KEY`
  - `AZURE_OPENAI_DEPLOYMENT`
- This function expects a query parameter `prompt` in the HTTP request.

---

## 5. Useful Links

- [Azure AI Documentation](https://learn.microsoft.com/azure/ai-services/)
- [Azure OpenAI Docs](https://learn.microsoft.com/azure/ai-services/openai/)
- [Azure ML Documentation](https://learn.microsoft.com/azure/machine-learning/)
- [Azure Functions Docs](https://learn.microsoft.com/azure/azure-functions/)
