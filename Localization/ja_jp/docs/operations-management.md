# Azure 運用・管理ガイド（日本語）

このガイドは、Azure を利用するスタートアップ向けにコスト管理・監視・自動化・ガバナンスの実践手順と例をまとめています。

---

## 1. コスト管理

- Azure ポータルの [コスト管理 + 請求](https://portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/costanalysis) で予算・コストアラートを設定
- コスト分析でリソースグループ・サービス・タグごとに支出を把握
- 例：リソースグループに予算アラートを作成

```sh
az consumption budget create --resource-group <group> --amount 100 --time-grain monthly --name myBudget
```

- 長期利用にはリザーブドインスタンスやセービングプランも検討

---

## 2. 監視・ログ

- WebアプリやAPIには Application Insights を有効化し、テレメトリや障害解析に活用
- Azure Monitor や Log Analytics でログ集約・クエリ
- 例：CPU高負荷時のアラート設定

```sh
az monitor metrics alert create --name HighCPU --resource-group <group> --scopes <resource-id> --condition "avg Percentage CPU > 80" --window-size 5m --evaluation-frequency 1m --action <action-group>
```

- ダッシュボードでリアルタイム監視も可能

---

## 3. リソース整理・自動化

- 自動化スクリプトや Azure Automation でリソース整理を定期実行（例：未使用VMの削除、夜間の開発環境停止）
- 例：リソースグループごと削除

```sh
az group delete --name <group> --yes --no-wait
```

- Azure Policy でタグ付け・リージョン・セキュリティ要件を強制
- 例：全リソースにタグ必須のポリシー（[サンプル](https://github.com/Azure/azure-policy/blob/master/samples/Tag/require-tag/azurepolicy.json)）

---

## 4. ガバナンス・ベストプラクティス

- マネジメントグループでサブスクリプションを整理
- ブループリントで環境構築を標準化
- アクセス・監査ログを定期確認
- リソース命名・タグ付け規則を文書化

---

## 5. 参考リンク

- [Azure コスト管理ドキュメント](https://learn.microsoft.com/ja-jp/azure/cost-management-billing/)
- [Azure 監視・管理](https://learn.microsoft.com/ja-jp/azure/azure-monitor/)
- [Azure Policy サンプル](https://github.com/Azure/azure-policy)
- [Azure Automation ドキュメント](https://learn.microsoft.com/ja-jp/azure/automation/)
