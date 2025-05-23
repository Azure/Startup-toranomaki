# Quickstart & Hands-on Guide

This guide helps you get started with Azure and Azure AI services as quickly as possible, with step-by-step instructions and practical examples.

---

## 1. Create an Azure Account

- Go to [Azure Free Account](https://azure.microsoft.com/free/) and sign up.
- Verify your identity with a credit card (no charge for free tier).
- Note your subscription ID for later use.

---

## 2. Set Up Your Development Environment

- Install [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli) for your OS.
- Install [Visual Studio Code](https://code.visualstudio.com/) and the [Azure Tools extension pack](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-azureextensionpack).
- (Optional) Install Python, Node.js, or .NET SDK as needed for your sample app.

---

## 3. Deploy Your First AI Service

### 3.1 Azure OpenAI Service

- Go to the [Azure Portal](https://portal.azure.com/), search for "Azure OpenAI Service", and create a new resource.
- Select region, pricing tier, and assign to a resource group.
- After deployment, go to the resource and get your endpoint and API key.

### 3.2 Cognitive Services (Vision, Speech, etc.)

- Similar steps: create the service, get keys and endpoint from the portal.

---

## 4. Sample App Deployment

### 4.1 Clone and Configure a Sample App

- Example (Node.js):

```sh
git clone https://github.com/Azure-Samples/openai-nodejs-sample.git
cd openai-nodejs-sample
cp .env.example .env
# Edit .env to add your API key and endpoint
```

### 4.2 Deploy to Azure Web App

```sh
az webapp up --name <your-app-name> --resource-group <your-rg> --runtime "NODE|18-lts"
```

---

## 5. CI/CD Example

### 5.1 GitHub Actions

- Add a `.github/workflows/azure-webapp.yml` file:

```yaml
name: Deploy Node.js app to Azure Web App
on:
  push:
    branches:
      - main
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
      - uses: azure/webapps-deploy@v2
        with:
          app-name: <your-app-name>
          slot-name: production
          publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE }}
          package: .
```

- Store your Azure credentials and publish profile as GitHub secrets.

---

## 6. Useful Links

- [Azure Quickstart Center](https://portal.azure.com/#blade/Microsoft_Azure_ProjectWelcomeBlade/quickstart)
- [Azure Samples](https://github.com/Azure-Samples)
- [Azure CLI Documentation](https://learn.microsoft.com/cli/azure/)
- [Azure Web Apps Docs](https://learn.microsoft.com/azure/app-service/)
