# Azure AI サービス別ガイド（日本語）

このセクションでは、主要な Azure AI サービスごとの実践ガイドをまとめます。デプロイ、API 利用例、セキュリティ、コスト管理のポイントも解説します。

---

## 1. Azure OpenAI Service

### 1.1 Azure OpenAI Service のデプロイ

#### 基本セットアップ
- Azure ポータルで「Azure OpenAI Service」を検索し、新規リソースを作成します。
- リージョンを選択します（推奨：East US、West Europe で高い可用性）。
- 料金プランを選択します（本番環境では Standard を推奨）。
- リソースグループを選択し、一意の名前を付けます。

#### デプロイ後の設定
- デプロイ完了を待ちます（通常 5-10 分）。
- Azure OpenAI リソースに移動します。
- 「キーとエンドポイント」に移動して API キーとエンドポイント URL を取得します。
- 「モデルデプロイ」に移動して使用したいモデル（GPT-3.5-turbo、GPT-4 など）をデプロイします。

#### モデルデプロイ
- Azure OpenAI Studio で「新しいデプロイの作成」をクリックします。
- モデルを選択します（初心者には GPT-3.5-turbo を推奨）。
- デプロイ名を選択します（例：「gpt-35-turbo-deployment」）。
- 必要に応じて分当たりトークン数の制限を設定します。

### 1.2 API 利用例

#### Python 例
```python
import openai
import os
from openai import AzureOpenAI

# クライアントの初期化
client = AzureOpenAI(
    api_key=os.getenv("AZURE_OPENAI_KEY"),
    api_version="2024-10-21",
    azure_endpoint=os.getenv("AZURE_OPENAI_ENDPOINT")
)

# チャット完了
response = client.chat.completions.create(
    model="gpt-35-turbo-deployment",  # あなたのデプロイ名
    messages=[
        {"role": "system", "content": "あなたは親切なアシスタントです。"},
        {"role": "user", "content": "こんにちは、今日はどのようにお手伝いできますか？"}
    ],
    max_tokens=100,
    temperature=0.7
)

print(response.choices[0].message.content)
```

#### JavaScript/Node.js 例
```javascript
const { OpenAI } = require('openai');

const openai = new OpenAI({
  apiKey: process.env.AZURE_OPENAI_KEY,
  baseURL: `${process.env.AZURE_OPENAI_ENDPOINT}/openai/deployments/gpt-35-turbo-deployment`,
  defaultQuery: { 'api-version': '2024-10-21' },
  defaultHeaders: {
    'api-key': process.env.AZURE_OPENAI_KEY,
  },
});

async function chatWithAI() {
  try {
    const response = await openai.chat.completions.create({
      model: 'gpt-35-turbo-deployment',
      messages: [
        { role: 'system', content: 'あなたは親切なアシスタントです。' },
        { role: 'user', content: 'こんにちは、今日はどのようにお手伝いできますか？' }
      ],
      max_tokens: 100,
      temperature: 0.7
    });
    
    console.log(response.choices[0].message.content);
  } catch (error) {
    console.error('エラー:', error);
  }
}

chatWithAI();
```

#### C# 例
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
        new ChatRequestSystemMessage("あなたは親切なアシスタントです。"),
        new ChatRequestUserMessage("こんにちは、今日はどのようにお手伝いできますか？"),
    },
    MaxTokens = 100,
    Temperature = 0.7f
};

Response<ChatCompletions> response = await client.GetChatCompletionsAsync(chatCompletionsOptions);
Console.WriteLine(response.Value.Choices[0].Message.Content);
```

### 1.3 高度な機能と使用例

#### エンベディングの操作
```python
# テキストの類似性と検索のためのエンベディングを生成
response = client.embeddings.create(
    model="text-embedding-ada-002",  # エンベディングモデルのデプロイ名
    input="ここに埋め込むテキストを入力"
)

embedding = response.data[0].embedding
print(f"エンベディングベクトルの長さ: {len(embedding)}")
```

#### ストリーミングレスポンス
```python
# より良いユーザーエクスペリエンスのためのストリーミングレスポンス
stream = client.chat.completions.create(
    model="gpt-35-turbo-deployment",
    messages=[{"role": "user", "content": "物語を教えてください"}],
    stream=True
)

for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="")
```

#### 関数呼び出し
```python
# モデルが呼び出すための関数を定義
functions = [
    {
        "name": "get_weather",
        "description": "現在の天気情報を取得",
        "parameters": {
            "type": "object",
            "properties": {
                "location": {
                    "type": "string",
                    "description": "都市と州、例：東京, 日本"
                }
            },
            "required": ["location"]
        }
    }
]

response = client.chat.completions.create(
    model="gpt-35-turbo-deployment",
    messages=[{"role": "user", "content": "東京の天気はどうですか？"}],
    functions=functions,
    function_call="auto"
)
```

### 1.4 モデル選択ガイド

#### GPT-3.5 Turbo
- **適用場面**: 一般的な会話、コンテンツ生成、基本的なコーディングタスク
- **利点**: 高速、コスト効率的、良好なパフォーマンス
- **使用例**: チャットボット、コンテンツ作成、簡単な Q&A

#### GPT-4
- **適用場面**: 複雑な推論、高度なコーディング、詳細な分析
- **利点**: 優れた推論、より良いコード生成、高い精度
- **使用例**: コードレビュー、複雑な問題解決、研究支援

#### Text-Embedding-Ada-002
- **適用場面**: セマンティック検索、類似性マッチング、クラスタリング
- **使用例**: ドキュメント検索、推薦システム、コンテンツ分類

### 1.5 エラーハンドリングとトラブルシューティング

#### 一般的なエラーと解決策
```python
import openai
from openai import RateLimitError, APIError, AuthenticationError

try:
    response = client.chat.completions.create(
        model="gpt-35-turbo-deployment",
        messages=[{"role": "user", "content": "こんにちは"}]
    )
except AuthenticationError as e:
    print(f"認証が失敗しました: {e}")
    # API キーとエンドポイントを確認
except RateLimitError as e:
    print(f"レート制限を超えました: {e}")
    # 指数バックオフを実装
except APIError as e:
    print(f"API エラー: {e}")
    # サービスステータスを確認して再試行
except Exception as e:
    print(f"予期しないエラー: {e}")
```

#### レート制限のベストプラクティス
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

### 1.6 セキュリティのベストプラクティス

#### API キー管理
- API キーは Azure Key Vault または環境変数に保存
- 可能な場合は Azure ホストアプリケーションでマネージド ID を使用
- API キーを定期的にローテーション
- API キーをソースコントロールにコミットしない

#### ネットワークセキュリティ
```python
# セキュリティ強化のために Azure プライベートエンドポイントを使用
# ファイアウォールルールを設定してアクセスを制限
# 例: Azure ポータルで特定の IP 範囲に制限
```

#### 入力検証とコンテンツフィルタリング
```python
# コンテンツフィルタリングの実装
def validate_input(user_input):
    # 悪意のあるコンテンツをチェック
    if len(user_input) > 4000:  # トークン制限の考慮
        raise ValueError("入力が長すぎます")
    
    # カスタム検証ロジックを追加
    prohibited_terms = ["有害な用語1", "有害な用語2"]
    if any(term in user_input.lower() for term in prohibited_terms):
        raise ValueError("禁止されたコンテンツが検出されました")
    
    return user_input

# 追加のフィルタリングには Azure Content Safety サービスを使用
```

### 1.7 トークン使用量の最適化

#### トークンの理解
- 1 トークン ≈ 英語で 4 文字
- 入力と出力の両方が使用量にカウント
- コストを最適化するためにトークン使用量を監視

#### 最適化戦略
```python
# 1. システムメッセージを効率的に使用
system_message = "簡潔に答えて。"  # 短いが効果的

# 2. レスポンスの max_tokens を制限
response = client.chat.completions.create(
    model="gpt-35-turbo-deployment",
    messages=[{"role": "user", "content": "AI について説明してください"}],
    max_tokens=50  # レスポンスの長さを制限
)

# 3. temperature を賢く使用
# 事実に基づくレスポンスには低い temperature（0.1-0.3）
# クリエイティブなコンテンツには高い temperature（0.7-0.9）
```

### 1.8 他の Azure サービスとの統合

#### Azure Functions との統合
```python
import azure.functions as func
from azure.ai.openai import AzureOpenAI

def main(req: func.HttpRequest) -> func.HttpResponse:
    client = AzureOpenAI(
        api_key=os.environ["AZURE_OPENAI_KEY"],
        api_version="2024-10-21",
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

#### Azure Logic Apps との統合
- HTTP コネクタを使用して Azure OpenAI エンドポイントを呼び出し
- OpenAI レスポンスを処理するワークフローを実装
- Cosmos DB、Service Bus などの他の Azure サービスと接続

#### Azure Cognitive Search + OpenAI
```python
# RAG（検索拡張生成）のために検索と OpenAI を組み合わせ
def search_and_generate(query):
    # 1. 関連ドキュメントを検索
    search_results = cognitive_search_client.search(query)
    
    # 2. 結果を OpenAI のコンテキストとして使用
    context = "\n".join([result['content'] for result in search_results])
    
    response = client.chat.completions.create(
        model="gpt-35-turbo-deployment",
        messages=[
            {"role": "system", "content": f"このコンテキストを使用: {context}"},
            {"role": "user", "content": query}
        ]
    )
    
    return response.choices[0].message.content
```

### 1.9 料金・クォータ・ベストプラクティス

#### 料金体系
- **GPT-3.5 Turbo**: 1K トークンあたり約 $0.002
- **GPT-4**: 1K トークンあたり約 $0.03-$0.06（モデルによって異なる）
- **エンベディング**: 1K トークンあたり約 $0.0001
- 現在の[料金表](https://azure.microsoft.com/ja-jp/pricing/details/cognitive-services/openai-service/)を確認

#### クォータ管理
- デフォルトクォータはリージョンとモデルによって異なる
- Azure ポータルを通じてクォータ増加をリクエスト
- Azure ポータルダッシュボードで使用量を監視
- 予期しない料金を避けるために請求アラートを設定

#### コスト最適化のヒント
- 簡単なタスクには GPT-3.5 Turbo を使用
- 適切な場合はレスポンスをキャッシュ
- リクエストバッチングを実装
- 可能な場合は短いプロンプトを使用
- トークン使用パターンを監視・分析

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

以下は HTTP トリガーの Azure Functions（Python）で Azure OpenAI Service を呼び出す例です。最新の `azure-ai-openai` ライブラリを使用しています。エンドポイントや API キー、デプロイ名は環境変数で設定してください。

```python
import os
import json
import azure.functions as func
from azure.ai.openai import AzureOpenAI

def main(req: func.HttpRequest) -> func.HttpResponse:
    try:
        # リクエストボディを取得
        req_body = req.get_json()
        if not req_body or 'message' not in req_body:
            return func.HttpResponse(
                json.dumps({"error": "リクエストボディに 'message' フィールドがありません"}),
                status_code=400,
                mimetype="application/json"
            )

        user_message = req_body['message']
        
        # Azure OpenAI クライアントを初期化
        client = AzureOpenAI(
            api_key=os.environ["AZURE_OPENAI_KEY"],
            api_version="2024-10-21",
            azure_endpoint=os.environ["AZURE_OPENAI_ENDPOINT"]
        )

        # チャット完了を作成
        response = client.chat.completions.create(
            model=os.environ["AZURE_OPENAI_DEPLOYMENT"],
            messages=[
                {"role": "system", "content": "あなたは親切なアシスタントです。"},
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
            }, ensure_ascii=False),
            mimetype="application/json"
        )

    except Exception as e:
        return func.HttpResponse(
            json.dumps({"error": str(e)}, ensure_ascii=False),
            status_code=500,
            mimetype="application/json"
        )
```

#### 必要な環境変数:
- `AZURE_OPENAI_ENDPOINT`: Azure OpenAI サービスエンドポイント
- `AZURE_OPENAI_KEY`: Azure OpenAI API キー
- `AZURE_OPENAI_DEPLOYMENT`: モデルデプロイ名

#### 必要な依存関係 (requirements.txt):
```
azure-functions
azure-ai-openai
```

#### サンプルリクエスト:
```bash
curl -X POST "https://your-function-app.azurewebsites.net/api/your-function" \
  -H "Content-Type: application/json" \
  -d '{"message": "こんにちは、今日はどのようにお手伝いできますか？"}'
```

---

## 5. 参考リンク

- [Azure AI ドキュメント](https://learn.microsoft.com/ja-jp/azure/ai-services/)
- [Azure OpenAI Docs](https://learn.microsoft.com/ja-jp/azure/ai-services/openai/)
- [Azure ML ドキュメント](https://learn.microsoft.com/ja-jp/azure/machine-learning/)
- [Azure Functions ドキュメント](https://learn.microsoft.com/ja-jp/azure/azure-functions/)
