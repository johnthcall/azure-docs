---
title: Deploy resources with Azure CLI and template | Microsoft Docs
description: Use Azure Resource Manager and Azure CLI to deploy a resources to Azure. The resources are defined in a Resource Manager template.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn

ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: tomfitz

---
# Deploy resources with Resource Manager templates and Azure CLI
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

This topic explains how to use Azure CLI with Resource Manager templates to deploy your resources to Azure.  Your template can be either a local file or an external file that is available through a URI. When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.

> [!TIP]
> For help with debugging an error during deployment, see:
> 
> * [View deployment operations](resource-manager-deployment-operations.md) to learn about getting information that helps you troubleshoot your error
> * [Troubleshoot common errors when deploying resources to Azure with Azure Resource Manager](resource-manager-common-deployment-errors.md) to learn how to resolve common deployment errors
> 
> 

## Quick steps to deployment
To quickly get started with deployment, use the following commands:

```
azure group create -n examplegroup -l "West US"
azure group deployment create -f "c:\MyTemplates\example.json" -e "c:\MyTemplates\example.params.json" -g examplegroup -n exampledeployment
```

These commands create a resource group, and deploy a template to that resource group. The template file and parameter file are both local files. If that works for you, you have everything you need to deploy resources. However, there are more options available to you for specifying the resources to deploy. The rest of this article describes all the options available to you during deployment. 

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## Deploy
If you have not previously used Azure CLI with Resource Manager, see [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management](xplat-cli-azure-resource-manager.md).

1. Log in to your Azure account. After providing your credentials, the command returns the result of your login.
   
   ```
   azure login
   ```
2. If you have multiple subscriptions, provide the subscription ID you wish to use for deployment.
   
   ```
   azure account set <YourSubscriptionNameOrId>
   ```
4. When deploying a template, you must specify a resource group that will contain the deployed resources. If you have an existing resource group that you wish to deploy to, you can skip this step and use that resource group. 
   
     To create a resource group, provide a name and location for your resource group. You provide a location for the resource group because the resource group stores metadata about the resources. For compliance reasons, you may want to specify where that metadata is stored. In general, we recommend that you specify a location where most of your resources will reside. Using the same location can simplify your template.
   
   ```
   azure group create -n ExampleResourceGroup -l "West US"
   ```

     A summary of the new resource group is returned.
   
5. Validate your deployment before executing it by running the **azure group template validate** command. When testing the deployment, provide parameters exactly as you would when executing the deployment (shown in the next step).
   
   ```
   azure group template validate -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup
   ```
6. To deploy resources to your resource group, run the **azure group deployment create** command and provide the necessary parameters. The parameters include a name for your deployment, the name of your resource group, the path or URL to the template, and any other parameters needed for your scenario. If the **mode** parameter is not specified, the default value of **Incremental** is used. To run a complete deployment, set **mode** to **Complete**. Be careful when using the complete mode as you can inadvertently delete resources that are not in your template.
   
     To deploy a local template, use the **template-file** parameter:
   
   ```
   azure group deployment create --resource-group examplegroup --template-file "c:\MyTemplates\example.json"
   ```
   
     To deploy an external template, use **template-uri** parameter:
   
   ```
   azure group deployment create --resource-group examplegroup --template-uri "https://raw.githubusercontent.com/exampleuser/MyTemplates/master/example.json"
   ```
   
     The preceding two examples do not include parameter values. You learn about the options for passing parameter values in the [Parameters](#parameters) section. For now, you are prompted to provide parameter values with the following syntax:

   ```
   info:    Executing command group deployment create
   info:    Supply values for the following parameters
   firstParameters:  <type here>
   ```

      After the resources have been deployed, you see a summary of the deployment. The summary includes a **ProvisioningState**, which indicates whether the deployment succeeded.

   ```
   + Initializing template configurations and parameters
   + Creating a deployment
   info:    Created template deployment "example"
   + Waiting for deployment to complete
   +
   +
   data:    DeploymentName     : example
   data:    ResourceGroupName  : examplegroup
   data:    ProvisioningState  : Succeeded
   ```
7. If you want to log additional information about the deployment that may help you troubleshoot any deployment errors, use the **debug-setting** parameter. You can specify that request content, response content, or both be logged with the deployment operation.
   
   ```
   azure group deployment create --debug-setting All -f <PathToTemplate> -e <PathToParameterFile> -g examplegroup -n exampleDeployment
   ```

## Deploy template from storage with SAS token
You can add your templates to a storage account and link to them during deployment with a SAS token.

> [!IMPORTANT]
> By following the steps below, the blob containing the template is accessible to only the account owner. However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI. If another user intercepts the URI, that user is able to access the template. Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.
> 
> 

### Add private template to storage account
The following steps set up a storage account for templates:

1. Create a resource group.
   
   ```
   azure group create -n "ManageGroup" -l "westus"
   ```
2. Create a storage account. The storage account name must be unique across Azure, so provide your own name for the account.
   
   ```
   azure storage account create -g ManageGroup -l "westus" --sku-name LRS --kind Storage storagecontosotemplates
   ```
3. Set variables for the storage account and key.
   
   ```
   export AZURE_STORAGE_ACCOUNT=storagecontosotemplates
   export AZURE_STORAGE_ACCESS_KEY={storage_account_key}
   ```
4. Create a container. The permission is set to **Off** which means the container is only accessible to the owner.
   
   ```
   azure storage container create --container templates -p Off 
   ```
5. Add your template to the container.
   
   ```
   azure storage blob upload --container templates -f c:\MyTemplates\azuredeploy.json
   ```

### Provide SAS token during deployment
To deploy a private template in a storage account, retrieve a SAS token and include it in the URI for the template.

1. Create a SAS token with read permissions and an expiry time to limit access. Set the expiry time to allow enough time to complete the deployment. Retrieve the full URI of the template including the SAS token.
   
   ```
   expiretime=$(date -I'minutes' --date "+30 minutes")
   fullurl=$(azure storage blob sas create --container templates --blob azuredeploy.json --permissions r --expiry $expiretimetime --json  | jq ".url")
   ```
2. Deploy the template by providing the URI that includes the SAS token.
   
   ```
   azure group deployment create --template-uri $fullurl -g ExampleResourceGroup
   ```

For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).

## Parameters

You have the following options for providing parameter values: 
   
- Use inline parameters. Each parameter is in the format: `"ParameterName": { "value": "ParameterValue" }`. The following example shows the parameters with escape characters.

   ```   
   azure group deployment create -f <PathToTemplate> -p "{\"ParameterName\":{\"value\":\"ParameterValue\"}}" -g ExampleResourceGroup -n ExampleDeployment
   ```
- Use a parameter file.

  ```    
  azure group deployment create -f "c:\MyTemplates\example.json" -e "c:\MyTemplates\example.params.json" -g ExampleResourceGroup -n ExampleDeployment
  ```
      

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## Next steps
* For an example of deploying resources through the .NET client library, see [Deploy resources using .NET libraries and a template](../virtual-machines/virtual-machines-windows-csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).
* For guidance on deploying your solution to different environments, see [Development and test environments in Microsoft Azure](solution-dev-test-environments.md).
* For details about using a KeyVault reference to pass secure values, see [Pass secure values during deployment](resource-manager-keyvault-parameter.md).
* For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).
* For a four part series about automating deployment, see [Automating application deployments to Azure Virtual Machines](../virtual-machines/virtual-machines-windows-dotnet-core-1-landing.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). This series covers application architecture, access and security, availability and scale, and application deployment.

