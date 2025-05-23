# Azure AI 服务指南（中文）

本节为主要 Azure AI 服务提供实用、分步指南，包括部署、API 使用、安全与成本管理建议。

---

## 1. Azure OpenAI Service

### 1.1 部署 Azure OpenAI Service

- 登录 Azure 门户，搜索“Azure OpenAI Service”，创建新资源。
- 选择区域、定价层，并分配到资源组。
- 部署完成后，在资源页面获取 endpoint 和 API key。

### 1.2 API 使用示例（Python）

```python
import openai
openai.api_type = "azure"
openai.api_base = "<your-endpoint>"
openai.api_key = "<your-key>"
openai.api_version = "2023-05-15"

response = openai.ChatCompletion.create(
  engine="gpt-35-turbo",
  messages=[{"role": "user", "content": "你好！"}]
)
print(response.choices[0].message["content"])
```

### 1.3 价格、配额与最佳实践

- 查看[价格](https://azure.microsoft.com/zh-cn/pricing/details/cognitive-services/openai-service/)
- 如需更高配额可申请提升
- 切勿在客户端代码中暴露 API key

---

## 2. Azure Cognitive Services

### 2.1 服务类型

- Vision（计算机视觉、人脸、OCR）
- Speech（语音识别、语音合成）
- Language（文本分析、翻译）
- Decision（个性化、异常检测）

### 2.2 服务创建与安全

- 在 Azure 门户创建，获取密钥和 endpoint
- 通过网络规则和 Key Vault 控制访问

### 2.3 API 调用示例（REST）

```bash
curl -X POST "https://<your-endpoint>/vision/v3.2/analyze" \
  -H "Ocp-Apim-Subscription-Key: <your-key>" \
  -H "Content-Type: application/json" \
  --data '{"url": "https://example.com/image.jpg"}'
```

---

## 3. Azure Machine Learning

### 3.1 工作区设置

- 通过门户或 CLI 创建 Azure ML 工作区
- 挂载计算资源（CPU/GPU）

### 3.2 模型训练与部署

- 使用 Azure ML Studio 或 SDK 进行训练
- 将模型部署为 Web 服务（ACI/AKS）

### 3.3 MLOps 流水线基础

- 使用 Azure DevOps 或 GitHub Actions 实现 CI/CD
- 用 MLflow 跟踪实验和模型

---

## 4. Azure Functions & Logic Apps

### 4.1 无服务器 AI 集成

- 使用 Functions 按需调用 AI 服务
- 示例：HTTP 触发的 Function 调用 OpenAI API

### 4.2 示例函数（Python, Azure Functions）

以下为 HTTP 触发的 Azure Functions（Python）调用 Azure OpenAI Service 的示例。endpoint、API key 和部署名通过环境变量设置。

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

- 请在 Function App 的环境变量中设置：
  - `AZURE_OPENAI_ENDPOINT`
  - `AZURE_OPENAI_KEY`
  - `AZURE_OPENAI_DEPLOYMENT`
- 该函数通过 HTTP 请求的 `prompt` 参数获取用户输入。

---

## 5. 参考链接

- [Azure AI 文档](https://learn.microsoft.com/zh-cn/azure/ai-services/)
- [Azure OpenAI 文档](https://learn.microsoft.com/zh-cn/azure/ai-services/openai/)
- [Azure ML 文档](https://learn.microsoft.com/zh-cn/azure/machine-learning/)
- [Azure Functions 文档](https://learn.microsoft.com/zh-cn/azure/azure-functions/)
