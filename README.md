# ALTCHA Sentinel Deployment on Azure App Service

This repository contains an Azure Resource Manager (ARM) template for deploying the ALTCHA Sentinel service on Azure App Service with persistent storage.

## Overview

The deployment includes:
- A Linux-based Azure App Service with container support
- An App Service Plan (B2 SKU by default)
- A Storage Account with Azure Files share for persistent data
- Configuration to mount the Azure Files share to the container

## Prerequisites

1. Azure subscription
2. Azure CLI or PowerShell installed
3. Appropriate permissions to create resources in your Azure subscription

## Deployment Instructions

### Using Azure CLI

1. Login to Azure:
   ```bash
   az login
   ```

2. Create a resource group (if needed):
   ```bash
   az group create --name <resource-group-name> --location <location>
   ```

3. Deploy the template:
   ```bash
   az deployment group create \
     --resource-group <resource-group-name> \
     --template-file <path-to-template.json> \
     --parameters siteName=<your-site-name> dockerImage=<your-docker-image>
   ```

### Using Azure Portal

[Deploy to Azure](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Faltcha-org%2Faltcha-sentinel-deploy-azure%2Fmain%2Fazuredeploy.json)


1. Navigate to your resource group in the Azure Portal
2. Click "Deploy a custom template"
3. Upload the template file or paste the JSON content
4. Fill in the parameters or use the default values
5. Review and create the deployment

## Configuration Parameters

| Parameter | Description | Default Value |
|-----------|------------|---------------|
| `siteNamePrefix` | Name of the App Service | `sentinel` |
| `dockerRegistry` | Docker registry base URL | `https://ghcr.io` |
| `dockerRegistryUsername` | Optional username for the registry | |
| `dockerRegistryPassword` | Optional password for the registry | |
| `dockerImage` | Docker image to deploy | `ghcr.io/altcha-org/sentinel:<version>` |
| `storageAccountName` | Name of the storage account | Auto-generated |
| `fileShareName` | Name of the Azure Files share | `persistentdata` |
| `mountPath` | Mount path in the container | `/data` |
| `location` | Location for all resources | Resource group location |
| `sku` | SKU of App Service Plan | `B2` |
| `storageSKU` | SKU of Storage Account | `Premium_LRS` |

## Outputs

After deployment, the following outputs will be available:

- `appServiceName`: Name of the deployed App Service
- `appServiceUrl`: URL to access the deployed application
- `storageAccountName`: Name of the created storage account
- `fileShareName`: Name of the created file share

## Post-Deployment

1. The application should be available at: `https://<your-site-name>.azurewebsites.net`
2. You can view the deployment outputs in the Azure Portal or by querying the deployment:
   ```bash
   az deployment group show --resource-group <resource-group-name> --name <deployment-name>
   ```

## Customizing the Deployment

To customize the deployment:
1. Modify the `dockerImage` parameter to use a different container image
2. Adjust the `sku` parameter to change the App Service Plan tier
3. Change the `storageSKU` parameter to select a different storage account type

## Troubleshooting

1. If the container fails to start, check the logs in the App Service's "Container settings" or "Log stream"
2. Verify the storage account and file share were created successfully
3. Ensure the container has proper permissions to write to the mounted volume

## Cleanup

To remove all resources:
```bash
az group delete --name <resource-group-name> --yes --no-wait
```

## License

This ARM template is provided as-is. Refer to the ALTCHA Sentinel project for licensing information about the application itself.