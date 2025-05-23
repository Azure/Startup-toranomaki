<!-- filepath: Localization/zh_cn/FAQ/set-env-vars-azure-functions.md -->
# 如何为 Azure Functions 设置环境变量？

## 本地开发
- 在 `local.settings.json` 的 `Values` 部分添加变量。
- 示例：
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

## Azure 门户
- 进入 Function App > 配置 > 应用程序设置。
- 每个变量作为新的键值对添加。
- 在代码中可通过 `os.environ["MY_API_KEY"]` 获取。

**安全提示：**
- 切勿将密钥提交到源码库。请使用环境变量或 Azure Key Vault。
