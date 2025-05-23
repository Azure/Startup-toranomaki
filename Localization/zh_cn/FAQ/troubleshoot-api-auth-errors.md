<!-- filepath: Localization/zh_cn/FAQ/troubleshoot-api-auth-errors.md -->
# 如何排查 API 认证错误？

调用 Azure API 时遇到认证错误时：

- 仔细检查 API 密钥、终结点和凭据。
- 确认环境变量设置正确（参见 [如何为 Azure Functions 设置环境变量？](./set-env-vars-azure-functions.md)）。
- 若为 Azure AD 保护的 API，请检查应用注册和权限。
- 检查资源名称或区域设置是否有拼写错误。
- 查看错误信息（如 401 Unauthorized、403 Forbidden）。

**常用命令：**
```sh
az account show
az role assignment list --assignee <your-user-or-app>
```
