# Azure 设置指南（中文）

本指南详细介绍了如何开始使用 Microsoft Azure，包括账户注册、CLI 安装、资源组创建及最佳实践，帮助初创企业和开发者高效且安全地利用 Azure。

---

## 1. 创建 Azure 账户

1. 访问 [Azure 官网](https://azure.microsoft.com/zh-cn/free/) 注册免费账户。
2. 需要有效的邮箱和信用卡用于身份验证（免费额度内不会扣费）。
3. 注册后请记录下您的订阅信息。

**提示：**

- 免费账户可获得 30 天 $200 美元额度和 12 个月热门服务免费。
- 一个账户可管理多个订阅。

---

## 2. 使用 Azure 门户

- 访问 [Azure 门户](https://portal.azure.com/)，这是管理所有 Azure 资源的网页界面。
- 熟悉仪表盘、资源组和导航菜单。
- 可在搜索栏输入“资源组”或“虚拟机”进行体验。

---

## 3. 安装 Azure CLI

Azure CLI 是用于管理 Azure 资源的命令行工具。

- 按照 [官方安装指南](https://learn.microsoft.com/zh-cn/cli/azure/install-azure-cli) 选择对应操作系统进行安装。

macOS:

```sh
brew update && brew install azure-cli
```

Windows:

```powershell
winget install Microsoft.AzureCLI
```

**检查 CLI 版本：**

```sh
az version
```

**升级 CLI：**

```sh
brew upgrade azure-cli
```

---

## 4. 其他工具

- **Azure Cloud Shell：** 浏览器内置 Shell，预装 Azure CLI，可在 Azure 门户直接访问。
- **VS Code 扩展：**
  - [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)
  - [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)

---

## 5. 登录 Azure CLI

```sh
az login
```

浏览器会自动打开，请用 Azure 账户登录。

**切换订阅：**

```sh
az account set --subscription "<订阅名称或ID>"
```

---

## 6. 查询订阅与创建资源组

### 查询订阅

```sh
az account list --output table
```

### 选择区域

- 可用区域列表：[Azure 区域](https://azure.microsoft.com/zh-cn/global-infrastructure/geographies/)
- 建议选择靠近用户的区域以降低延迟。

### 创建资源组

```sh
az group create --name <资源组名称> --location <区域>
```

**命名建议：**

- 使用小写、短横线和项目名组合（如：`myapp-dev-chinaeast2`）

**示例：**

```sh
az group create --name myResourceGroup --location chinaeast2
```

---

## 7. 清理资源

为避免不必要的费用，请及时删除不再使用的资源：

```sh
az group delete --name <资源组名称> --yes --no-wait
```

---

## 8. 故障排查与小贴士

- **检查 CLI 登录状态：**

  ```sh
  az account show
  ```

- **常见错误：**
  - 权限不足：请确认账户有正确的角色/权限。
  - 未找到 CLI：请检查 PATH 或重新安装 Azure CLI。

- **获取帮助：**

  ```sh
  az --help
  az <命令> --help
  ```

- **官方支持：**
  - [Azure 支持](https://azure.microsoft.com/zh-cn/support/options/)
  - [Microsoft Q&A](https://learn.microsoft.com/zh-cn/answers/topics/azure.html)
  - [Stack Overflow](https://stackoverflow.com/questions/tagged/azure)

---

## 9. 参考链接

- [Azure 文档](https://learn.microsoft.com/zh-cn/azure/)
- [Azure CLI 文档](https://learn.microsoft.com/zh-cn/cli/azure/)
- [Azure 免费账户 FAQ](https://azure.microsoft.com/zh-cn/free/free-account-faq/)

---

本指南仅涵盖基础设置。有关高级配置、安全和自动化，请参阅本仓库的其他文档。
