---
title: Deploy resources with PowerShell and template | Microsoft Docs
description: Use Azure Resource Manager and Azure PowerShell to deploy a resources to Azure. The resources are defined in a Resource Manager template.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn

ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/16/2016
ms.author: tomfitz

---
# Deploy resources with Resource Manager templates and Azure PowerShell
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure CLI](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST API](resource-group-template-deploy-rest.md)
> 
> 

This topic explains how to use Azure PowerShell with Resource Manager templates to deploy your resources to Azure. Your template can be either a local file or an external file that is available through a URI. When your template resides in a storage account, you can restrict access to the template and provide a shared access signature (SAS) token during deployment.

> [!TIP]
> For help with debugging an error during deployment, see:
> 
> * [View deployment operations](resource-manager-deployment-operations.md) to learn about getting information that helps you troubleshoot your error
> * [Troubleshoot common errors when deploying resources to Azure with Azure Resource Manager](resource-manager-common-deployment-errors.md) to learn how to resolve common deployment errors
> 
> 

## Quick steps to deployment
To quickly get started with deployment, use the following commands:

```powershell
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\example.json -TemplateParameterFile c:\MyTemplates\example.params.json
```

These commands create a resource group, and deploy a template to that resource group. The template file and parameter file are both local files. If that works for you, you have everything you need to deploy resources. However, there are more options available to you for specifying the resources to deploy. The rest of this article describes all the options available to you during deployment. 

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## Deploy
1. Log in to your Azure account.

   ```powershell
   Add-AzureRmAccount
   ```

    A summary of your account is returned.
    
   ```powershell
   Environment           : AzureCloud
   Account               : someone@example.com
   TenantId              : {guid}
   SubscriptionId        : {guid}
   SubscriptionName      : Example Subscription
   CurrentStorageAccount :
   ```

2. If you have multiple subscriptions, provide the subscription ID you wish to use for deployment with the **Set-AzureRmContext** command. 
   
   ```powershell
   Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
   ```
3. When deploying a template, you must specify a resource group that will contain the deployed resources. If you have an existing resource group that you wish to deploy to, you can skip this step and use that resource group. 
   
     To create a resource group, provide a name and location for your resource group. You provide a location for the resource group because the resource group stores metadata about the resources. For compliance reasons, you may want to specify where that metadata is stored. In general, we recommend that you specify a location where most of your resources will reside. Using the same location can simplify your template.
   
   ```powershell
   New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West US"
   ```
   
     A summary of the new resource group is returned.
4. Before executing your deployment, you can validate your deployment settings. The **Test-AzureRmResourceGroupDeployment** cmdlet enables you to find problems before creating actual resources. The following example shows how to validate a deployment.
   
   ```powershell
   Test-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate>
   ```
5. To deploy resources to your resource group, run the **New-AzureRmResourceGroupDeployment** command and provide the necessary parameters. The parameters include a name for your deployment, the name of your resource group, the path or URL to the template, and any other parameters needed for your scenario. If the **Mode** parameter is not specified, the default value of **Incremental** is used. To run a complete deployment, set **Mode** to **Complete**. Be careful when using the complete mode as you can inadvertently delete resources that are not in your template.
   
     To deploy a local template, use the **TemplateFile** parameter:
   
   ```powershell
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\example.json
   ```
   
     To deploy an external template, use **TemplateUri** parameter:
   
   ```powershell
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri https://raw.githubusercontent.com/exampleuser/MyTemplates/master/example.json
   ```
   
     The preceding two examples do not include parameter values. You learn about the options for passing parameter values in the [Parameters](#parameters) section. For now, you are prompted to provide parameter values with the following syntax:

   ```powershell  
   cmdlet New-AzureRmResourceGroupDeployment at command pipeline position 1
   Supply values for the following parameters:
   (Type !? for Help.)
   firstParameter: <type here>
   ```
     
     After the resources have been deployed, you see a summary of the deployment. The summary includes a **ProvisioningState**, which indicates whether the deployment succeeded.

   ```powershell   
   DeploymentName    : ExampleDeployment
   ResourceGroupName : ExampleResourceGroup
   ProvisioningState : Succeeded
   Timestamp         : 4/14/2015 7:00:27 PM
   Mode              : Incremental
   ```
      
6. If you want to log additional information about the deployment that may help you troubleshoot any deployment errors, use the **DeploymentDebugLogLevel** parameter. You can specify that request content, response content, or both be logged with the deployment operation.
   
   ```powershell
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -DeploymentDebugLogLevel All -ResourceGroupName ExampleResourceGroup -TemplateFile <PathOrLinkToTemplate>
   ```
   
     For more information about using this debugging content to troubleshoot deployments, see [View deployment operations](resource-manager-deployment-operations.md).

## Deploy private template with SAS token
You can add your templates to a storage account and link to them during deployment with a SAS token.

> [!IMPORTANT]
> By following the steps below, the blob containing the template is accessible to only the account owner. However, when you create a SAS token for the blob, the blob is accessible to anyone with that URI. If another user intercepts the URI, that user is able to access the template. Using a SAS token is a good way of limiting access to your templates, but you should not include sensitive data like passwords directly in the template.
> 
> 

### Add private template to storage account
The following steps set up a storage account for templates:

1. Create a resource group.
   
   ```powershell
   New-AzureRmResourceGroup -Name ManageGroup -Location "West US"
   ```
2. Create a storage account. The storage account name must be unique across Azure, so provide your own name for the account.
   
   ```powershell
   New-AzureRmStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates -Type Standard_LRS -Location "West US"
   ```
3. Set the current storage account.
   
   ```powershell
   Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
   ```
4. Create a container. The permission is set to **Off** which means the container is only accessible to the owner.
   
   ```powershell
   New-AzureStorageContainer -Name templates -Permission Off
   ```
5. Add your template to the container.
   
   ```powershell
   Set-AzureStorageBlobContent -Container templates -File c:\Azure\Templates\azuredeploy.json
   ```

### Provide SAS token during deployment
To deploy a private template in a storage account, retrieve a SAS token and include it in the URI for the template.

1. If you have changed the current storage account, set the current storage account to the one containing your templates.
   
   ```powershell
   Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
   ```
2. Create a SAS token with read permissions and an expiry time to limit access. Retrieve the full URI of the template including the SAS token.
   
   ```powershell
   $templateuri = New-AzureStorageBlobSASToken -Container templates -Blob azuredeploy.json -Permission r -ExpiryTime (Get-Date).AddHours(2.0) -FullUri
   ```
3. Deploy the template by providing the URI that includes the SAS token.
   
   ```powershell
   New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri $templateuri
   ```

For an example of using a SAS token with linked templates, see [Using linked templates with Azure Resource Manager](resource-group-linked-templates.md).

## Parameters

You have the following options for providing parameter values: 
   
- Inline parameters. Include individual parameter names in the cmdlet (for example, **-myParameterName**.)

   ```powershell   
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -myParameterName "parameterValue"
   ```
- Parameter object. Include the **-TemplateParameterObject** parameter.

   ```powershell   
   $parameters = @{"<ParameterName>"="<Parameter Value>"}
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterObject $parameters
   ```
- Local parameter file. Include the **-TemplateParameterFile** parameter.

   ```powershell   
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile <PathToTemplate> -TemplateParameterFile c:\MyTemplates\example.params.json
   ```
- External parameter file. Include the **-TemplateParameterUri** parameter.

   ```powershell   
   New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateUri <LinkToTemplate> -TemplateParameterUri https://raw.githubusercontent.com/exampleuser/MyTemplates/master/example.params.json
   ```
      
[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)] 

If your template includes a parameter with the same name as one of the parameters in the PowerShell command, you are prompted to provide a value for that parameter. Azure PowerShell presents the parameter from your template with the postfix **FromTemplate**. For example, a parameter named **ResourceGroupName** in your template conflicts with the **ResourceGroupName** parameter in the [New-AzureRmResourceGroupDeployment](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroupdeployment) cmdlet. You are prompted to provide a value for **ResourceGroupNameFromTemplate**. In general, you should avoid this confusion by not naming parameters with the same name as parameters used for deployment operations.

## Parameter precedence

You can use inline parameters and a local parameter file in the same deployment operation. For example, you can specify some values in the local parameter file and add other values inline during deployment. If you provide values for a parameter in both the local parameter file and inline, the inline value takes precedence.

However, when you use an external parameter file, you cannot pass other values either inline or from a local file. When you specify a parameter file in the **TemplateParameterUri** parameter, all inline parameters are ignored. Provide all parameter values in the external file. If your template includes a sensitive value that you cannot include in the parameter file, either add that value to a key vault, or dynamically provide all parameter values inline.

## Next steps
* For an example of deploying resources through the .NET client library, see [Deploy resources using .NET libraries and a template](../virtual-machines/virtual-machines-windows-csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* To define parameters in template, see [Authoring templates](resource-group-authoring-templates.md#parameters).
* For guidance on deploying your solution to different environments, see [Development and test environments in Microsoft Azure](solution-dev-test-environments.md).
* For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).
* For a four part series about automating deployment, see [Automating application deployments to Azure Virtual Machines](../virtual-machines/virtual-machines-windows-dotnet-core-1-landing.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). This series covers application architecture, access and security, availability and scale, and application deployment.

