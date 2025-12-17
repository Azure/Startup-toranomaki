# Azure スタートアップ向けアーキテクチャパターン（日本語）

このドキュメントは、Azure および Azure AI を活用するスタートアップ向けのリファレンスアーキテクチャとベストプラクティスを、図解・コード例・設計のヒントとともに紹介します。

---

## 1. 最小構成アーキテクチャ

- **目的:** MVP 向けのコスト最適・スケーラブル・セキュアな基盤
- **構成例:**
  - Azure App Service（Web/API）
  - Azure OpenAI Service（AI/LLM）
  - Azure SQL Database または Cosmos DB
  - Azure Key Vault（シークレット管理）
- **構成図例：**

```text
[ユーザー] -> [App Service] -> [OpenAI Service]
                        -> [SQL/CosmosDB]
                        -> [Key Vault]
```

- **IaC サンプル（Bicep）：**

```bicep
resource app 'Microsoft.Web/sites@2025-03-01' = {
  name: 'myapp'
  location: resourceGroup().location
  kind: 'app'
  properties: {
    serverFarmId: appServicePlan.id
  }
}
```

---

## 2. AI 組込アプリのリファレンス

- **パターン:** API バックエンド + Azure OpenAI + Blob Storage（ファイル入出力）
- **設計ポイント:**
  - サービス間認証は Managed Identity を活用
  - ユーザーアップロードは Blob Storage に保存し、AI で処理・結果保存
- **フロー例：**

```text
[ユーザー] -> [API] -> [Blob Storage]
                -> [OpenAI Service]
```

---

## 3. マルチリージョン・高可用性

- **パターン:**
  - App Service や DB を複数リージョンにデプロイ
  - Azure Front Door または Traffic Manager でグローバル負荷分散・フェイルオーバー
- **設計ポイント:**
  - 重要データはジオ冗長ストレージ（GRS）を利用
  - フェイルオーバーテストを自動化

---

## 4. セキュリティ設計

- すべてのリソースアクセスに Managed Identity を利用
- シークレットは必ず Key Vault で管理
- リソースグループ単位で RBAC を適用
- HTTPS 強制・NSG/ファイアウォールでパブリックアクセス制限

---

## 5. 参考リンク

- [Azure アーキテクチャセンター](https://learn.microsoft.com/ja-jp/azure/architecture/)
- [Well-Architected Framework](https://learn.microsoft.com/ja-jp/azure/architecture/framework/)
- [リファレンスアーキテクチャ集](https://learn.microsoft.com/ja-jp/azure/architecture/reference-architectures/)
