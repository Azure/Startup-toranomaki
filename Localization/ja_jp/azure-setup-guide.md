# Azure セットアップガイド（日本語）

このガイドは、Microsoft Azure の利用を始めるための詳細な手順をまとめたものです。アカウント作成、CLIインストール、リソースグループ作成、ベストプラクティスなど、スタートアップや開発者が効率的かつ安全に Azure を活用できるように設計されています。

---

## 1. Azure アカウントの作成

1. [Azure公式サイト](https://azure.microsoft.com/ja-jp/free/) にアクセスし、無料アカウントを作成します。
2. 有効なメールアドレスとクレジットカードが必要です（無料枠の範囲内で課金は発生しません）。
3. 登録後、サブスクリプション情報を控えておきましょう。

**ポイント:**

- 無料アカウントでは30日間有効な$200分のクレジットと、12か月間の人気サービス無料枠が利用できます。
- 1つのアカウントで複数のサブスクリプションを管理できます。

---

## 2. Azure ポータルの活用

- [Azure ポータル](https://portal.azure.com/) にアクセスし、ダッシュボードやリソースグループ、ナビゲーションメニューを確認しましょう。
- 検索バーで「リソースグループ」や「仮想マシン」などを検索してみてください。

---

## 3. Azure CLI のインストール

Azure CLI はコマンドラインから Azure リソースを管理できるツールです。

- [公式インストールガイド](https://learn.microsoft.com/ja-jp/cli/azure/install-azure-cli) を参照し、OSに合わせてインストールしてください。

macOS:

```sh
brew update && brew install azure-cli
```

Windows:

```powershell
winget install Microsoft.AzureCLI
```

**CLIバージョン確認:**

```sh
az version
```

**CLIアップデート:**

```sh
brew upgrade azure-cli
```

---

## 4. その他のツール

- **Azure Cloud Shell:** ブラウザから使えるシェル。Azure CLIがプリインストール済み。
- **VS Code 拡張:**
  - [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)
  - [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)

---

## 5. Azure CLI へのログイン

```sh
az login
```

ブラウザが開くので、Azureアカウントでサインインしてください。

**サブスクリプション切り替え:**

```sh
az account set --subscription "<サブスクリプション名またはID>"
```

---

## 6. サブスクリプション確認とリソースグループ作成

### サブスクリプション確認

```sh
az account list --output table
```

### リージョン選択

- 利用可能なリージョン一覧: [Azure リージョン](https://azure.microsoft.com/ja-jp/global-infrastructure/geographies/)
- ユーザーに近いリージョンを選ぶと低遅延です。

### リソースグループ作成

```sh
az group create --name <リソースグループ名> --location <リージョン>
```

**命名例:**

- 小文字・ハイフン・プロジェクト名を組み合わせる（例: `myapp-dev-japaneast`）

**例:**

```sh
az group create --name myResourceGroup --location japaneast
```

---

## 7. リソースのクリーンアップ

不要なリソースは削除してコストを抑えましょう。

```sh
az group delete --name <リソースグループ名> --yes --no-wait
```

---

## 8. トラブルシューティング・ヒント

- **CLIログイン状態確認:**

  ```sh
  az account show
  ```

- **よくあるエラー:**
  - 権限エラー: アカウントのロール・権限を確認
  - CLIが見つからない: PATHを確認、または再インストール

- **ヘルプ表示:**

  ```sh
  az --help
  az <コマンド> --help
  ```

- **公式サポート:**
  - [Azure サポート](https://azure.microsoft.com/ja-jp/support/options/)
  - [Microsoft Q&A](https://learn.microsoft.com/ja-jp/answers/topics/azure.html)
  - [Stack Overflow](https://stackoverflow.com/questions/tagged/azure)

---

## 9. 参考リンク

- [Azure ドキュメント](https://learn.microsoft.com/ja-jp/azure/)
- [Azure CLI ドキュメント](https://learn.microsoft.com/ja-jp/cli/azure/)
- [Azure 無料アカウント FAQ](https://azure.microsoft.com/ja-jp/free/free-account-faq/)

---

このガイドは基本的なセットアップ手順をまとめたものです。高度な構成やセキュリティ、運用自動化については他のドキュメントも参照してください。
