# Operations & Management on Azure

This guide covers cost management, monitoring, automation, and governance for startups using Azure, with practical steps and examples.

---

## 1. Cost Management

- Set up budgets and cost alerts in the Azure Portal ([Cost Management + Billing](https://portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/costanalysis)).
- Use cost analysis to track spending by resource group, service, or tag.
- Example: Create a budget alert for a resource group:

```sh
az consumption budget create --resource-group <group> --amount 100 --time-grain monthly --name myBudget
```

- Review reserved instances and savings plans for long-term workloads.

---

## 2. Monitoring & Logging

- Enable Application Insights for web apps and APIs to collect telemetry and diagnose issues.
- Use Azure Monitor and Log Analytics to aggregate logs and set up custom queries.
- Example: Set up an alert for high CPU usage:

```sh
az monitor metrics alert create --name HighCPU --resource-group <group> --scopes <resource-id> --condition "avg Percentage CPU > 80" --window-size 5m --evaluation-frequency 1m --action <action-group>
```

- Set up dashboards for real-time monitoring.

---

## 3. Resource Cleanup & Automation

- Use automation scripts or Azure Automation to schedule resource cleanup (e.g., delete unused VMs, stop dev environments at night).
- Example: Delete a resource group and all its resources:

```sh
az group delete --name <group> --yes --no-wait
```

- Use Azure Policy to enforce resource tagging, location, and security requirements.
- Example: Policy to require tags on all resources ([sample policy](https://github.com/Azure/azure-policy/blob/master/samples/Tag/require-tag/azurepolicy.json)).

---

## 4. Governance & Best Practices

- Use Management Groups to organize subscriptions.
- Apply Blueprints for repeatable environment setup.
- Regularly review access and audit logs.
- Document your resource naming and tagging conventions.

---

## 5. Useful Links

- [Azure Cost Management Docs](https://learn.microsoft.com/azure/cost-management-billing/)
- [Monitor and Manage Azure](https://learn.microsoft.com/azure/azure-monitor/)
- [Azure Policy Samples](https://github.com/Azure/azure-policy)
- [Azure Automation Docs](https://learn.microsoft.com/azure/automation/)
