# Architecture Patterns for Startups on Azure

This document introduces reference architectures and best practices for startups using Azure and Azure AI, with diagrams, code samples, and practical design tips.

---

## 1. Minimal Viable Architecture

- **Goal:** Cost-optimized, scalable, and secure foundation for MVPs.
- **Components:**
  - Azure App Service (Web/API)
  - Azure OpenAI Service (AI/LLM)
  - Azure SQL Database or Cosmos DB
  - Azure Key Vault (secrets)
- **Sample Diagram:**

```text
[User] -> [App Service] -> [OpenAI Service]
                        -> [SQL/CosmosDB]
                        -> [Key Vault]
```

- **IaC Example (Bicep):**

```bicep
resource app 'Microsoft.Web/sites@2022-03-01' = {
  name: 'myapp'
  location: resourceGroup().location
  kind: 'app'
  properties: {
    serverFarmId: appServicePlan.id
  }
}
```

---

## 2. AI-Enabled App Reference

- **Pattern:** API backend + Azure OpenAI + Blob Storage for file input/output.
- **Design Tips:**
  - Use Managed Identity for secure service-to-service calls.
  - Store user uploads in Blob Storage, process with AI, save results.
- **Sample Flow:**

```text
[User] -> [API] -> [Blob Storage]
                -> [OpenAI Service]
```

---

## 3. Multi-region & High Availability

- **Pattern:**
  - Deploy App Service and databases in multiple regions.
  - Use Azure Front Door or Traffic Manager for global load balancing and failover.
- **Design Tips:**
  - Use geo-redundant storage (GRS) for critical data.
  - Automate failover testing.

---

## 4. Security by Design

- Use Managed Identity for all resource access.
- Store all secrets in Key Vault.
- Apply RBAC at the resource group level.
- Enforce HTTPS and restrict public access with NSGs/firewalls.

---

## 5. Useful Links

- [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/)
- [Well-Architected Framework](https://learn.microsoft.com/azure/architecture/framework/)
- [Reference Architectures](https://learn.microsoft.com/azure/architecture/reference-architectures/)
