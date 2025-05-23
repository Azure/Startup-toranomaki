<!-- filepath: Localization/ja_jp/FAQ/set-env-vars-azure-functions.md -->
# Azure Functions で環境変数を設定するには？

## ローカル開発の場合
- `local.settings.json` の `Values` セクションに変数を追加します。
- 例：
```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "...",
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "MY_API_KEY": "sk-..."
  }
}
```

## Azure ポータルの場合
- Function App > 「構成」>「アプリケーション設定」に移動します。
- 各変数を新しいキーと値のペアとして追加します。
- これらはコード内で `os.environ["MY_API_KEY"]` として利用できます。

**セキュリティのポイント：**
- シークレットはソース管理にコミットせず、環境変数や Azure Key Vault を利用しましょう。
