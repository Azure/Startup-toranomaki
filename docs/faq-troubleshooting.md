# FAQ & Troubleshooting for Azure Startups

This document collects common questions, error patterns, and solutions for startups using Azure and Azure AI, with actionable troubleshooting steps.

---

## 1. Account & Subscription Issues

### 1.1 How do I request a quota increase for OpenAI or Cognitive Services?

- Go to the Azure Portal, open the resource, and select "Quota" or "Usage + quotas".
- Click "Request increase" and fill out the form.
- For OpenAI, you may need to provide business justification.

### 1.2 I can't access a resource or get a permission error

- Check your role assignment in the resource group or subscription (see Security & Authentication doc).
- Ask your admin to grant you the required role (e.g., Contributor).
- Use `az account show` and `az role assignment list` to debug.

---

## 2. Deployment & API Errors

### 2.1 Common error messages

- **ResourceNotFound**: Check resource name, region, and spelling.
- **AuthorizationFailed**: Check your RBAC role and permissions.
- **QuotaExceeded**: Request a quota increase.
- **InvalidApiKey**: Double-check your API key and endpoint.

### 2.2 Where to find logs and diagnostics

- Use Application Insights for app logs and errors.
- Use Azure Monitor and Log Analytics for infrastructure logs.
- Check the "Activity log" in the Azure Portal for resource events.

---

## 3. Cost & Billing

### 3.1 How to avoid unexpected charges

- Set up budgets and cost alerts (see Operations & Management doc).
- Delete unused resources and resource groups.
- Use the Azure Pricing Calculator to estimate costs before deploying.

### 3.2 How to set up cost alerts

- Go to Cost Management + Billing > Budgets > Add.
- Set a threshold and notification email.

---

## 4. Support

### 4.1 How to contact Microsoft support

- Go to the Azure Portal, click "Help + support", and create a new support request.
- Choose the appropriate severity and provide detailed information.

### 4.2 Community resources

- [Microsoft Q&A](https://learn.microsoft.com/answers/topics/azure.html)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/azure)
- [Azure Support](https://azure.microsoft.com/support/options/)

---

## 5. Useful Links

- [Azure Support](https://azure.microsoft.com/support/options/)
- [Microsoft Q&A](https://learn.microsoft.com/answers/topics/azure.html)
- [Azure CLI Documentation](https://learn.microsoft.com/cli/azure/)
- [Azure Status](https://status.azure.com/)
