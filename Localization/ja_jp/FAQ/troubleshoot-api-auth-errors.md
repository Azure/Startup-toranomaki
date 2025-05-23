<!-- filepath: Localization/ja_jp/FAQ/troubleshoot-api-auth-errors.md -->
# API 認証エラーのトラブルシューティング方法は？

Azure API 呼び出し時に認証エラーが発生した場合：

- API キーやエンドポイント、認証情報を再確認する。
- 環境変数が正しく設定されているか確認する（[Azure Functions の環境変数設定方法](./set-env-vars-azure-functions.md) 参照）。
- Azure AD 保護 API の場合、アプリ登録や権限設定を確認する。
- リソース名やリージョン設定のタイプミスを確認する。
- エラーメッセージ（例：401 Unauthorized、403 Forbidden）を確認する。

**便利なコマンド例：**
```sh
az account show
az role assignment list --assignee <your-user-or-app>
```
