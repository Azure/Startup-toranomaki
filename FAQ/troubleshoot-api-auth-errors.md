# How do I troubleshoot API authentication errors?

If you encounter authentication errors when calling Azure APIs:

- Double-check your API key, endpoint, and credentials.
- Ensure your environment variables are set correctly (see [How do I set environment variables for Azure Functions?](./set-env-vars-azure-functions.md)).
- For Azure AD-protected APIs, verify your app registration and permissions.
- Check for typos in resource names or region settings.
- Review error messages for details (e.g., 401 Unauthorized, 403 Forbidden).

**Useful command:**
```sh
az account show
az role assignment list --assignee <your-user-or-app>
```
