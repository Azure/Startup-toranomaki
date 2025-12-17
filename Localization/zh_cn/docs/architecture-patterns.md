# Azure 架构模式（中文）

本文件介绍了初创企业在 Azure 及其 AI 服务上常用的参考架构和最佳实践，包含架构图、代码示例和设计建议。

---

## 1. 最小可行架构

- **目标：** 面向 MVP 的成本优化、可扩展、安全基础
- **组件：**
  - Azure App Service（Web/API）
  - Azure OpenAI Service（AI/LLM）
  - Azure SQL 数据库或 Cosmos DB
  - Azure Key Vault（密钥管理）
- **架构图示例：**

```text
[用户] -> [App Service] -> [OpenAI Service]
                        -> [SQL/CosmosDB]
                        -> [Key Vault]
```

- **IaC 示例（Bicep）：**

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

## 2. AI 应用参考架构

- **模式：** API 后端 + Azure OpenAI + Blob Storage（文件输入/输出）
- **设计要点：**
  - 服务间调用建议用 Managed Identity
  - 用户上传文件存入 Blob Storage，经 AI 处理后保存结果
- **流程示意：**

```text
[用户] -> [API] -> [Blob Storage]
                -> [OpenAI Service]
```

---

## 3. 多区域与高可用

- **模式：**
  - App Service 和数据库多区域部署
  - Azure Front Door 或 Traffic Manager 实现全局负载均衡与故障转移
- **设计要点：**
  - 关键数据用 GRS（地理冗余存储）
  - 自动化故障转移测试

---

## 4. 安全设计

- 所有资源访问均用 Managed Identity
- 所有密钥存储于 Key Vault
- 资源组级别应用 RBAC
- 强制 HTTPS，NSG/防火墙限制公网访问

---

## 5. 参考链接

- [Azure 架构中心](https://learn.microsoft.com/zh-cn/azure/architecture/)
- [Well-Architected Framework](https://learn.microsoft.com/zh-cn/azure/architecture/framework/)
- [参考架构集](https://learn.microsoft.com/zh-cn/azure/architecture/reference-architectures/)
