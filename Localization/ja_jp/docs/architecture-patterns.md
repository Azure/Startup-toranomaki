# Architecture Patterns for Startups on Azure (日本語)

このドキュメントは、Azure および Azure AI を活用するスタートアップ向けのリファレンスアーキテクチャとベストプラクティスを紹介します。

---

## 1. 最小構成アーキテクチャ
- コスト最適化・スケーラブル・セキュア
- 例: Web App + Azure OpenAI + Azure SQL + Key Vault

## 2. AI 組込アプリのリファレンス
- API + Azure OpenAI Service + Blob Storage
- 図解や Bicep/Terraform サンプル

## 3. マルチリージョン・高可用性
- ジオ冗長ストレージ、マルチリージョン App Service
- フェイルオーバー用 Traffic Manager/Front Door

## 4. セキュリティ設計
- Managed Identity, Key Vault, RBAC の活用

## 5. 参考リンク
- [Azure アーキテクチャセンター](https://learn.microsoft.com/ja-jp/azure/architecture/)
- [Well-Architected Framework](https://learn.microsoft.com/ja-jp/azure/architecture/framework/)
