# Azure セキュリティ・認証ガイド（日本語）

このドキュメントは、Azure リソースやアプリのセキュリティ強化のベストプラクティスと実践例を詳しく解説します。

---

## 1. ID・アクセス管理

### 1.1 Azure AD/Entra ID の基本

- Azure Active Directory（Entra ID）で一元的にID管理
- ロール（管理者・開発者・運用者）ごとにユーザー・グループを整理
- 全ユーザーに多要素認証（MFA）を有効化
- 条件付きアクセスで場所・デバイス・リスクごとに制御

### 1.2 RBAC（ロールベースアクセス制御）

- リソースグループやリソース単位で最小権限ロールを割り当て
- 標準ロール（Owner, Contributor, Reader）やカスタムロールを活用
- ロール割り当てを定期監査
- 例：特定リソースグループに Reader ロールを付与

```sh
az role assignment create --assignee <user-email> --role Reader --resource-group <group-name>
```

---

## 2. シークレット・鍵管理

### 2.1 Azure Key Vault

- シークレット・APIキー・証明書・暗号鍵を安全に保管
- アクセスポリシーやRBACでアクセス制御
- Key Vault の監査ログ・ソフトデリートを有効化
- 例：Key Vault にシークレットを保存

```sh
az keyvault secret set --vault-name <vault-name> --name <secret-name> --value <secret-value>
```

### 2.2 Managed Identity

- Azureリソースの認証情報をハードコーディングせず、Managed Identity を利用
- VM, App Service, Functions などに Key Vault アクセス権を付与

---

## 3. API セキュリティ

### 3.1 認証・認可

- API認証には OAuth2, OpenID Connect, Azure AD を利用
- バックエンドで JWT トークンを検証
- 例：Azure AD アプリ登録でAPIを保護

### 3.2 CORS・レート制限

- CORSポリシーで信頼できるオリジンのみ許可
- API Management などでレート制限を実装

### 3.3 ネットワークセキュリティ

- 機密APIは Private Endpoint や Service Endpoint を利用
- NSG・ファイアウォール・API Management のIP制限で公開アクセスを制限

---

## 4. コンプライアンス・データ保護

### 4.1 暗号化

- すべてのストレージで暗号化を有効化（Azure Storage, SQL などはデフォルト）
- 必要に応じて顧客管理キー（CMK）を利用
- すべてのエンドポイントで TLS/SSL を強制

### 4.2 規制対応

- Azure のコンプライアンス対応状況を確認：[Azure コンプライアンス ドキュメント](https://learn.microsoft.com/ja-jp/azure/compliance/)
- Azure Policy でリージョン・暗号化・タグ要件などを強制

---

## 5. 監査・インシデント対応

- Azure Security Center/Defender で脅威検知・推奨事項を確認
- 不審なアクティビティ（失敗ログイン・権限昇格など）にアラート設定
- 監査ログ・アクセスレポートを定期確認
- インシデント対応計画を策定し、定期的にテスト

---

## 6. 参考リンク

- [Azure セキュリティ ドキュメント](https://learn.microsoft.com/ja-jp/azure/security/)
- [Azure Key Vault ドキュメント](https://learn.microsoft.com/ja-jp/azure/key-vault/)
- [Azure AD ドキュメント](https://learn.microsoft.com/ja-jp/azure/active-directory/)
- [Azure セキュリティ ベストプラクティス](https://learn.microsoft.com/ja-jp/azure/security/fundamentals/best-practices)
- [Azure コンプライアンス ドキュメント](https://learn.microsoft.com/ja-jp/azure/compliance/)
