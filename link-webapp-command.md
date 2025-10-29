# Link Existing Azure Static Web App to GitHub

## Prerequisites
1. Azure CLI installed (`az --version` to check)
2. GitHub Personal Access Token with `repo` and `workflow` permissions

## Command Template

```bash
az staticwebapp update \
  --name <your-static-web-app-name> \
  --resource-group <your-resource-group-name> \
  --source https://github.com/ymeknassi/touti-support \
  --branch main \
  --app-location "/public" \
  --api-location "" \
  --output-location "" \
  --token <your-github-token>
```

## Fill in Your Details

You need to find these values in Azure Portal:

### 1. Static Web App Name
- Go to: https://portal.azure.com
- Find your Static Web App resource
- The name is at the top (e.g., "touti-support" or "touti-web")

### 2. Resource Group Name  
- In the same page, look for "Resource group" 
- (e.g., "touti-resources" or "Default-ResourceGroup")

### 3. GitHub Token
- The personal access token you just created
- Should look like: `ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

## Example Command (Replace with your values)

```bash
az staticwebapp update \
  --name touti-support \
  --resource-group touti-resources \
  --source https://github.com/ymeknassi/touti-support \
  --branch main \
  --app-location "/public" \
  --api-location "" \
  --output-location "" \
  --token ghp_your_github_token_here
```

## Quick Find Your Values

Run these commands to list your Azure resources:

```bash
# Login to Azure
az login

# List all Static Web Apps in your subscription
az staticwebapp list --query "[].{Name:name, ResourceGroup:resourceGroup, Location:location}" --output table

# This will show you:
# - Name: Your Static Web App name
# - ResourceGroup: Your resource group name
```

## After Running the Command

1. Azure will create a GitHub Actions workflow in your repo
2. The workflow file will be at: `.github/workflows/azure-static-web-apps-<random>.yml`
3. GitHub Actions will automatically build and deploy
4. Check deployment status: https://github.com/ymeknassi/touti-support/actions

## Troubleshooting

### "Resource not found"
- Double-check the Static Web App name and resource group name
- Use `az staticwebapp list` to see all your static web apps

### "Invalid token"
- Make sure you copied the full GitHub token
- Verify the token has `repo` and `workflow` permissions
- Create a new token if needed

### "Already configured"
- If already linked, you may need to disconnect first:
```bash
az staticwebapp disconnect \
  --name <your-static-web-app-name> \
  --resource-group <your-resource-group-name>
```
Then run the update command again.

## Alternative: Use Azure Portal (Easier)

If you prefer not to use CLI:
1. Azure Portal → Your Static Web App
2. Left menu → "Source control" or "Deployment"  
3. Click "Connect" 
4. Select GitHub → Authorize → Select repo
5. Done!

