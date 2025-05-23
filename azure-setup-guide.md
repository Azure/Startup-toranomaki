# Azure Setup Guide

This guide provides detailed steps to get started with Microsoft Azure, including account creation, CLI installation, resource group setup, and best practices for managing your environment. It is designed for startups and developers who want to leverage Azure efficiently and securely.

---

## 1. Create an Azure Account

1. Go to the [Azure official site](https://azure.microsoft.com/free/) and sign up for a free account.
2. You will need a valid email address and a credit card for identity verification. (No charges will be made for the free tier.)
3. Complete the registration process and note your subscription details.

**Tips:**

- The free account provides $200 in credits for 30 days and access to popular free services for 12 months.
- You can manage multiple subscriptions under one account.

---

## 2. Explore the Azure Portal

- Access the [Azure Portal](https://portal.azure.com/), a web-based interface for managing all your Azure resources.
- Familiarize yourself with the dashboard, resource groups, and navigation menu.
- Try searching for "Resource groups" or "Virtual machines" in the search bar.

---

## 3. Install Azure CLI

Azure CLI is a command-line tool for managing Azure resources.

- Follow the [official installation guide](https://learn.microsoft.com/cli/azure/install-azure-cli) for your OS.

On macOS:

```sh
brew update && brew install azure-cli
```

On Windows:

```powershell
winget install Microsoft.AzureCLI
```

**Check your CLI version:**

```sh
az version
```

**Update Azure CLI:**

```sh
brew upgrade azure-cli
```

---

## 4. Other Tools

- **Azure Cloud Shell:** Browser-based shell with Azure CLI pre-installed. Access from the Azure Portal.
- **Visual Studio Code Extensions:**
  - [Azure Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack)
  - [Azure CLI Tools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)

---

## 5. Log in to Azure CLI

```sh
az login
```

A browser window will open. Sign in with your Azure account.

**Switch subscription (if you have multiple):**

```sh
az account set --subscription "<subscription-name-or-id>"
```

---

## 6. Check Subscription and Create a Resource Group

### Check your subscriptions

```sh
az account list --output table
```

### Choose a region

- See available regions: [Azure Regions](https://azure.microsoft.com/en-us/global-infrastructure/geographies/)
- Choose a region close to your users for lower latency.

### Create a resource group

```sh
az group create --name <resource-group-name> --location <region>
```

**Naming tips:**

- Use lowercase, hyphens, and project-specific names (e.g., `myapp-dev-japaneast`).

**Example:**

```sh
az group create --name myResourceGroup --location japaneast
```

---

## 7. Clean Up Resources

To avoid unnecessary charges, delete unused resources:

```sh
az group delete --name <resource-group-name> --yes --no-wait
```

---

## 8. Troubleshooting & Tips

- **Check CLI login status:**

  ```sh
  az account show
  ```

- **Common errors:**
  - Permission denied: Ensure your account has the right role/permissions.
  - CLI not found: Check your PATH or reinstall Azure CLI.

- **Get help:**

  ```sh
  az --help
  az <command> --help
  ```

- **Official support:**
  - [Azure Support](https://azure.microsoft.com/support/options/)
  - [Microsoft Q&A](https://learn.microsoft.com/answers/topics/azure.html)
  - [Stack Overflow](https://stackoverflow.com/questions/tagged/azure)

---

## 9. References

- [Azure Documentation](https://learn.microsoft.com/azure/)
- [Azure CLI Documentation](https://learn.microsoft.com/cli/azure/)
- [Azure Free Account FAQ](https://azure.microsoft.com/free/free-account-faq/)

---

This guide covers the essential and recommended setup steps. For advanced configuration, security, and automation, see other documents in this repository.
