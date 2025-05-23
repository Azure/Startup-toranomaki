# FAQ・トラブルシューティング（日本語）

このドキュメントは、Azure および Azure AI を利用するスタートアップ向けによくある質問・エラー・解決策をまとめたものです。実践的なトラブル対応手順を掲載しています。

---

## 1. アカウント・サブスクリプション関連

### 1.1 OpenAI や Cognitive Services のクォータ増加申請方法

- Azure ポータルでリソースを開き、「クォータ」または「使用量 + クォータ」を選択
- 「増加をリクエスト」から申請フォームを記入
- OpenAI の場合はビジネス上の理由も記載

### 1.2 リソースにアクセスできない・権限エラー

- リソースグループやサブスクリプションのロール割り当てを確認（詳細はセキュリティ・認証ドキュメント参照）
- 管理者に必要なロール（例: Contributor）を付与してもらう
- `az account show` や `az role assignment list` でデバッグ

---

## 2. デプロイ・API エラー

### 2.1 よくあるエラーメッセージ

- **ResourceNotFound**: リソース名・リージョン・スペルを確認
- **AuthorizationFailed**: RBAC ロール・権限を確認
- **QuotaExceeded**: クォータ増加を申請
- **InvalidApiKey**: API キーやエンドポイントを再確認

### 2.2 ログ・診断情報の確認場所

- Application Insights でアプリのログ・エラーを確認
- Azure Monitor や Log Analytics でインフラログを集約
- Azure ポータルの「アクティビティログ」でリソースイベントを確認

---

## 3. コスト・請求

### 3.1 予期しない課金を防ぐ方法

- Azure ポータルで予算・コストアラートを設定（詳細は運用管理ドキュメント参照）
- 使っていないリソースやリソースグループは削除
- Azure 料金計算ツールで事前にコストを見積もる

### 3.2 コストアラートの設定方法

- 「コスト管理 + 請求」>「予算」>「追加」から設定
- 閾値や通知先メールアドレスを指定

---

## 4. サポート

### 4.1 Microsoft サポートへの問い合わせ

- Azure ポータルで「ヘルプ + サポート」から新規サポートリクエストを作成
- 適切な重大度を選び、詳細情報を記載

### 4.2 コミュニティリソース

- [Microsoft Q&A](https://learn.microsoft.com/answers/topics/azure.html)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure)
- [Azure サポート](https://azure.microsoft.com/support/options/)

---

## 5. 参考リンク

- [Azure サポート](https://azure.microsoft.com/support/options/)
- [Microsoft Q&A](https://learn.microsoft.com/answers/topics/azure.html)
- [Azure CLI ドキュメント](https://learn.microsoft.com/cli/azure/)
- [Azure ステータス](https://status.azure.com/)
