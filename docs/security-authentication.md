# Security & Authentication on Azure

This document provides in-depth best practices and practical guidance for securing your Azure resources and applications, with concrete examples and references.

---

## 1. Identity & Access Management

### 1.1 Azure AD/Entra ID Basics

- Use Azure Active Directory (Entra ID) for centralized identity management.
- Organize users and groups by role (e.g., admin, developer, operator).
- Enable Multi-Factor Authentication (MFA) for all users.
- Use Conditional Access policies to restrict access by location, device, or risk.

### 1.2 RBAC (Role-Based Access Control)

- Assign least-privilege roles at the resource group or resource level.
- Use built-in roles (Owner, Contributor, Reader) or create custom roles for fine-grained control.
- Regularly review and audit role assignments.
- Example: Assigning a Reader role to a user for a specific resource group:

```sh
az role assignment create --assignee <user-email> --role Reader --resource-group <group-name>
```

---

## 2. Secrets & Key Management

### 2.1 Azure Key Vault

- Store secrets, API keys, certificates, and encryption keys securely.
- Use access policies or RBAC to control who/what can access secrets.
- Enable Key Vault logging and soft-delete for recovery.
- Example: Storing a secret in Key Vault:

```sh
az keyvault secret set --vault-name <vault-name> --name <secret-name> --value <secret-value>
```

### 2.2 Managed Identities

- Use managed identities for Azure resources to avoid hardcoding credentials.
- Assign Key Vault access to managed identities for VMs, App Services, Functions, etc.

---

## 3. API Security

### 3.1 Authentication & Authorization

- Use OAuth2, OpenID Connect, or Azure AD for API authentication.
- Validate JWT tokens in your backend.
- Example: Protecting an API with Azure AD App Registration.

### 3.2 CORS & Rate Limiting

- Configure CORS policies to only allow trusted origins.
- Implement rate limiting to prevent abuse (e.g., via API Management).

### 3.3 Network Security

- Use Private Endpoints or Service Endpoints for sensitive APIs.
- Restrict public access with NSGs, firewalls, or API Management IP filtering.

---

## 4. Compliance & Data Protection

### 4.1 Encryption

- Enable encryption at rest for all storage (default in Azure Storage, SQL, etc.).
- Use customer-managed keys (CMK) if required.
- Ensure TLS/SSL is enforced for all endpoints.

### 4.2 Regulatory Compliance

- Review Azure compliance offerings: [Azure Compliance Documentation](https://learn.microsoft.com/azure/compliance/)
- Use Azure Policy to enforce compliance (e.g., location, encryption, tag requirements).

---

## 5. Monitoring & Incident Response

- Enable Azure Security Center/Defender for threat detection and recommendations.
- Set up alerts for suspicious activity (e.g., failed logins, privilege escalation).
- Regularly review audit logs and access reports.
- Prepare an incident response plan and test it.

---

## 6. Useful Links

- [Azure Security Documentation](https://learn.microsoft.com/azure/security/)
- [Azure Key Vault Docs](https://learn.microsoft.com/azure/key-vault/)
- [Azure AD Documentation](https://learn.microsoft.com/azure/active-directory/)
- [Azure Security Best Practices](https://learn.microsoft.com/azure/security/fundamentals/best-practices)
- [Azure Compliance Documentation](https://learn.microsoft.com/azure/compliance/)
