# Azure 架构模式（中文）

本文件介绍了初创企业在 Azure 及其 AI 服务上常用的参考架构和最佳实践。

---

## 1. 最小可行架构
- 成本优化、可扩展、安全
- 示例：Web App + Azure OpenAI + Azure SQL + Key Vault

## 2. AI 应用参考架构
- API + Azure OpenAI Service + Blob Storage
- 架构图与 Bicep/Terraform 示例

## 3. 多区域与高可用
- 跨区域冗余存储，多区域 App Service
- Traffic Manager/Front Door 实现故障转移

## 4. 安全设计
- Managed Identity、Key Vault、RBAC 的使用

## 5. 参考链接
- [Azure 架构中心](https://learn.microsoft.com/zh-cn/azure/architecture/)
- [Well-Architected Framework](https://learn.microsoft.com/zh-cn/azure/architecture/framework/)
