# Azure AI サービス別ガイド（日本語）

このセクションでは、主要な Azure AI サービスごとの実践ガイドをまとめます。デプロイ、API 利用例、セキュリティ、コスト管理のポイントも解説します。

---

## 1. Azure OpenAI Service

### 1.1 Azure OpenAI Service のデプロイ

- Azure ポータルで「Azure OpenAI Service」を検索し、新規リソースを作成します。
- リージョン、料金プラン、リソースグループを選択します。
- デプロイ完了後、リソース画面でエンドポイントと API キーを取得します。

### 1.2 API 利用例（Python）

```python
import openai
openai.api_type = "azure"
openai.api_base = "<your-endpoint>"
openai.api_key = "<your-key>"
openai.api_version = "2023-05-15"

response = openai.ChatCompletion.create(
  engine="gpt-35-turbo",
  messages=[{"role": "user", "content": "こんにちは！"}]
)
print(response.choices[0].message["content"])
```

### 1.3 料金・クォータ・ベストプラクティス

- [料金表](https://azure.microsoft.com/ja-jp/pricing/details/cognitive-services/openai-service/) を確認
- 必要に応じてクォータ増加を申請
- API キーはクライアント側で公開しない

---

## 2. Azure Cognitive Services

### 2.1 サービス種別

- Vision（画像認識、顔認識、OCR）
- Speech（音声認識、音声合成）
- Language（テキスト分析、翻訳）
- Decision（パーソナライザー、異常検知）

### 2.2 サービス作成とセキュリティ

- Azure ポータルで作成し、キーとエンドポイントを取得
- ネットワーク制限や Key Vault でアクセス制御

### 2.3 API 呼び出し例（REST）

```bash
curl -X POST "https://<your-endpoint>/vision/v3.2/analyze" \
  -H "Ocp-Apim-Subscription-Key: <your-key>" \
  -H "Content-Type: application/json" \
  --data '{"url": "https://example.com/image.jpg"}'
```

---

## 3. Azure Machine Learning

### 3.1 ワークスペース作成

- ポータルまたは CLI で Azure ML ワークスペースを作成
- 計算リソース（CPU/GPU）をアタッチ

### 3.2 モデル学習・デプロイ

- Azure ML Studio や SDK で学習
- モデルを Web サービス（ACI/AKS）としてデプロイ

### 3.3 MLOps パイプラインの基本

- Azure DevOps や GitHub Actions で CI/CD
- MLflow で実験・モデル管理

---

## 4. Azure Functions & Logic Apps

### 4.1 サーバーレスでの AI 連携

- Functions で AI サービスをオンデマンド呼び出し
- 例：HTTP トリガー Function から OpenAI API を呼び出す

### 4.2 サンプル（Python, Azure Functions）

以下は HTTP トリガーの Azure Functions（Python）で Azure OpenAI Service を呼び出す例です。エンドポイントや API キー、デプロイ名は環境変数で設定してください。

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

- Function App の環境変数として以下を設定してください：
  - `AZURE_OPENAI_ENDPOINT`
  - `AZURE_OPENAI_KEY`
  - `AZURE_OPENAI_DEPLOYMENT`
- HTTP リクエストのクエリパラメータ `prompt` を受け取ります。

---

## 5. 参考リンク

- [Azure AI ドキュメント](https://learn.microsoft.com/ja-jp/azure/ai-services/)
- [Azure OpenAI Docs](https://learn.microsoft.com/ja-jp/azure/ai-services/openai/)
- [Azure ML ドキュメント](https://learn.microsoft.com/ja-jp/azure/machine-learning/)
- [Azure Functions ドキュメント](https://learn.microsoft.com/ja-jp/azure/azure-functions/)
