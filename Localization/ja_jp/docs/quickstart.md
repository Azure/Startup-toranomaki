# クイックスタート & ハンズオンガイド（日本語）

このガイドは、Azure および Azure AI サービスをすぐに使い始めるための手順と実践例をまとめています。

---

## 1. Azure アカウント作成

- [Azure 無料アカウント](https://azure.microsoft.com/free/) でサインアップ
- クレジットカードで本人確認（無料枠のみなら課金なし）
- サブスクリプションIDを控えておく

---

## 2. 開発環境セットアップ

- [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) をインストール
- [Visual Studio Code](https://code.visualstudio.com/) と [Azure Tools 拡張パック](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack) を導入
- （必要に応じて）Python, Node.js, .NET SDK もインストール

---

## 3. AI サービスのデプロイ

### 3.1 Azure OpenAI Service

- [Azure ポータル](https://portal.azure.com/) で「Azure OpenAI Service」を検索し新規作成
- リージョン・料金プラン・リソースグループを選択
- デプロイ後、リソース画面でエンドポイントと API キーを取得

### 3.2 Cognitive Services（Vision, Speech など）

- 同様にサービスを作成し、ポータルからキーとエンドポイントを取得

---

## 4. サンプルアプリのデプロイ

### 4.1 サンプルアプリのクローンと設定

- 例（Node.js）

```sh
git clone https://github.com/Azure-Samples/openai-nodejs-sample.git
cd openai-nodejs-sample
cp .env.example .env
# .env を編集し API キーとエンドポイントを設定
```

### 4.2 Azure Web App へデプロイ

```sh
az webapp up --name <your-app-name> --resource-group <your-rg> --runtime "NODE|18-lts"
```

---

## 5. CI/CD 例

### 5.1 GitHub Actions

- `.github/workflows/azure-webapp.yml` を追加

```yaml
name: Deploy Node.js app to Azure Web App
on:
  push:
    branches:
      - main
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
      - uses: azure/webapps-deploy@v2
        with:
          app-name: <your-app-name>
          slot-name: production
          publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE }}
          package: .
```

- Azure認証情報やパブリッシュプロファイルは GitHub Secrets に保存

---

## 6. 参考リンク

- [Azure クイックスタートセンター](https://portal.azure.com/#blade/Microsoft_Azure_ProjectWelcomeBlade/quickstart)
- [Azure サンプル集](https://github.com/Azure-Samples)
- [Azure CLI ドキュメント](https://learn.microsoft.com/cli/azure/)
- [Azure Web Apps ドキュメント](https://learn.microsoft.com/ja-jp/azure/app-service/)
