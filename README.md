# FastAPI app deployed to Azure WebApp with GitHub Actions

## Create Azure WebApp using Bicep

```bicep
az group create -n rg-fastapi -l eastasia
az deployment group create -g rg-fastapi -f main.bicep
```

## Add GitHub Actions credential

Add the credential as secret so GitHub Actions can login Azure in CI/CD pipelines.

```bash
az ad sp create-for-rbac \
  --name "github-fastapi-deploy" \
  --role contributor \
  --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/rg-fastapi \
  --json-auth | gh secret set AZURE_CREDENTIALS
```

## Trigger GitHub Actions CI/CD pipelines

Each git push on main branch will trigger the GitHub Actions pipelines automatically.

To trigger it manually and view the logs, use blow commands:

```bash
gh workflow list # List workfow files
gh workflow run # Create a `workflow_dispatch` event to trigger a workflow run
gh run watch # Watch a run until it completes, showing its progress
gh run view # View a summary of a workflow run
```

## Visit the web app

```bash
curl https://my-fastapi-app-prod.azurewebsites.net/hello?name=joekyo
```

and to view the application logs, use below command:

```bash
az webapp log tail \
  --name my-fastapi-app-prod \
  --resource-group rg-fastapi
```
