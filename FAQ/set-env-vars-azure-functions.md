# How do I set environment variables for Azure Functions?

## Local Development
- Add variables to `local.settings.json` under the `Values` section.
- Example:
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

## In Azure Portal
- Go to your Function App > Configuration > Application settings.
- Add each variable as a new key-value pair.
- These will be available as `os.environ["MY_API_KEY"]` in your code.

**Security Tip:**
- Never commit secrets to source control. Use environment variables or Azure Key Vault.
