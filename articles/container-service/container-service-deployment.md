---
title: Deploy a Docker container cluster in Azure | Microsoft Docs
description: Deploy an Azure Container Service cluster by using the Azure portal, the Azure CLI, or PowerShell.
services: container-service
documentationcenter: ''
author: rgardler
manager: timlt
editor: ''
tags: acs, azure-container-service


keywords: Docker, Containers, Micro-services, Mesos, Azure, dcos, swarm, kubernetes, azure container service, acs


ms.assetid: 696a736f-9299-4613-88c6-7177089cfc23
ms.service: container-service
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2017
ms.author: rogardle

---
# Deploy an Azure Container Service cluster



Azure Container Service provides rapid deployment of popular open-source container clustering and orchestration solutions. By using Azure Container Service, you can deploy DC/OS, Kubernetes, and Docker Swarm clusters with Azure Resource Manager templates or the Azure portal. You deploy these clusters by using Azure Virtual Machine Scale Sets, and the clusters take advantage of Azure networking and storage offerings. To access Azure Container Service, you need an Azure subscription. If you don't have one, then you can sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935).

> [!NOTE]
> Kubernetes support in Azure Container Service is currently in preview.



This document walks you through deploying an Azure Container Service cluster by using the [Azure portal](#create-a-service-by-using-the-azure-portal), the [Azure command-line interface (CLI)](#create-a-service-by-using-the-azure-cli), and the [Azure PowerShell module](#create-a-service-by-using-powershell).

## Create a service by using the Azure portal
1. Sign in to the Azure portal, select **New**, and search the Azure Marketplace for **Azure Container Service**.

    ![Create deployment 1](media/acs-portal1.png)  <br />

2. Select **Azure Container Service**, and click **Create**.

    ![Create deployment 2](media/acs-portal2.png)  <br />

3. Enter the following information:

    * **User name**: This is the user name that will be used for an account on each of the virtual machines and virtual machine scale sets in the Azure Container Service cluster.
    * **Subscription**: Select an Azure subscription.
    * **Resource group**: Select an existing resource group, or create a new one.
    * **Location**: Select an Azure region for the Azure Container Service deployment.
    * **SSH public key**: Add the public key that will be used for authentication against Azure Container Service virtual machines. It is very important that this key contains no line breaks, and that it includes the 'ssh-rsa' prefix and the 'username@domain' postfix. It should look something like the following: **ssh-rsa AAAAB3Nz...<...>...UcyupgH azureuser@linuxvm**. For guidance on creating Secure Shell (SSH) keys, see the [Linux](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-linux/) and [Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-ssh-from-windows/) articles.

4. Click **OK** when you're ready to proceed.

    ![Create deployment 3](media/acs-portal3.png)  <br />

5. Select an Orchestration type. The options are:

  * **DC/OS**: Deploys a DC/OS cluster.
  * **Swarm**: Deploys a Docker Swarm cluster.
  * **Kubernetes**: Deploys a Kubernetes cluster


6. Click **OK** when you're ready to proceed.

    ![Create deployment 4](media/acs-portal4-new.png)  <br />

7. If **Kubernetes** is selected in the dropdown, you will need to enter a service principal client id and service principal client secret. For more information, see [About the service principal for a Kubernetes cluster](container-service-kubernetes-service-principal.md).

    ![Create deployment 4.5](media/acs-portal10.PNG)  <br />

7. In the **Azure Container service** settings blade, enter the following information:

    * **Master count**: The number of masters in the cluster. If Kubernetes is selected, the number of masters is set to a default of 1
    * **Agent count**: For Docker Swarm and Kubernetes, this will be the initial number of agents in the agent scale set. For DC/OS, this will be the initial number of agents in a private scale set. Additionally, a public scale set is created, which contains a predetermined number of agents. The number of agents in this public scale set is determined by how many masters have been created in the cluster--one public agent for one master, and two public agents for three or five masters.
    * **Agent virtual machine size**: The size of the agent virtual machines.
    * **DNS prefix**: A world unique name that will be used to prefix key parts of the fully qualified domain names for the service.

8. Click **OK** when you're ready to proceed.

    ![Create deployment 5](media/acs-portal5.png)  <br />

9. Click **OK** after service validation has finished.

    ![Create deployment 6](media/acs-portal6.png)  <br />

10. Click **Purchase** to start the deployment process.

    ![Create deployment 7](media/acs-portal7.png)  <br />

    If you've elected to pin the deployment to the Azure portal, you can see the deployment status.

    ![Create deployment 8](media/acs-portal8.png)  <br />

When the deployment has completed, the Azure Container Service cluster is ready for use.

## Kubernetes specific entires required in the portal

If 'Kubernetes' is selected as the orchestrator then you will be prompted to enter the Service Prinicipal client id and client secret.
To learn how to create a service principal, go to this [link](https://github.com/Azure/acs-engine/blob/master/docs/serviceprincipal.md)

![Create deployment 8](media/acs-portal10.png)  <br />

## Create a service by using the Azure CLI
To create an instance of Azure Container Service by using the command line, you need an Azure subscription. If you don't have one, then you can sign up for a [free trial](http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=AA4C1C935). You also need to have [installed](../xplat-cli-install.md) and [configured](../xplat-cli-connect.md) the Azure CLI.

1. To deploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of the following templates from GitHub.

    * [DC/OS template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Swarm template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Kubernetes template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Next, make sure that the Azure CLI has been connected to an Azure subscription. You can do this by using the following command:

    ```bash
    azure account show
    ```
    If an Azure account is not returned, use the following command to sign the CLI in to Azure.

    ```bash
    azure login -u user@domain.com
    ```

3. Configure the Azure CLI tools to use Azure Resource Manager.

    ```bash
    azure config mode arm
    ```

4. Create an Azure resource group and Container Service cluster with the following command, where:

    * **RESOURCE_GROUP** is the name of the resource group that you want to use for this service.
    * **LOCATION** is the Azure region where the resource group and Azure Container Service deployment will be created.
    * **TEMPLATE_URI** is the location of the deployment file. Note that this must be the Raw file, not a pointer to the GitHub UI. To find this URL, select the azuredeploy.json file in GitHub, and click the **Raw** button.

    > [!NOTE]
    > When you run this command, the shell will prompt you for deployment parameter values.
    >

    ```bash
    azure group create -n RESOURCE_GROUP DEPLOYMENT_NAME -l LOCATION --template-uri TEMPLATE_URI
    ```

### Provide template parameters
This version of the command requires you to define parameters interactively. If you want to provide parameters, such as a JSON-formatted string, you can do so by using the `-p` switch. For example:

 ```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -p '{ "param1": "value1" … }'
```

Alternatively, you can provide a JSON-formatted parameters file by using the `-e` switch:

```bash
azure group deployment create RESOURCE_GROUP DEPLOYMENT_NAME --template-uri TEMPLATE_URI -e PATH/FILE.JSON
```

To see an example parameters file named `azuredeploy.parameters.json`, look for it with the Azure Container Service templates in GitHub.

## Create a service by using PowerShell
You can also deploy an Azure Container Service cluster with PowerShell. This document is based on the version 1.0 [Azure PowerShell module](https://azure.microsoft.com/blog/azps-1-0/).

1. To deploy a DC/OS, Docker Swarm, or Kubernetes cluster, select one of the following templates. Note that both of these templates are the same, with the exception of the default orchestrator selection.

    * [DC/OS template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos)
    * [Swarm template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-swarm)
    * [Kubernetes template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes)

2. Before creating a cluster in your Azure subscription, verify that your PowerShell session has been signed in to Azure. You can do this with the `Get-AzureRMSubscription` command:

    ```powershell
    Get-AzureRmSubscription
    ```

3. If you need to sign in to Azure, use the `Login-AzureRMAccount` command:

    ```powershell
    Login-AzureRmAccount
    ```

4. If you're deploying to a new resource group, you must first create the resource group. To create a new resource group, use the `New-AzureRmResourceGroup` command, and specify a resource group name and destination region:

    ```powershell
    New-AzureRmResourceGroup -Name GROUP_NAME -Location REGION
    ```

5. After you create a resource group, you can create your cluster with the following command. The URI of the desired template will be specified for the `-TemplateUri` parameter. When you run this command, PowerShell will prompt you for deployment parameter values.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DEPLOYMENT_NAME -ResourceGroupName RESOURCE_GROUP_NAME -TemplateUri TEMPLATE_URI
    ```

### Provide template parameters
If you're familiar with PowerShell, you know that you can cycle through the available parameters for a cmdlet by typing a minus sign (-) and then pressing the TAB key. This same functionality also works with parameters that you define in your template. As soon as you type the template name, the cmdlet fetches the template, parses the parameters, and adds the template parameters to the command dynamically. This makes it very easy to specify the template parameter values. And, if you forget a required parameter value, PowerShell prompts you for the value.

Below is the full command, with parameters included. You can provide your own values for the names of the resources.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName RESOURCE_GROUP_NAME-TemplateURI TEMPLATE_URI -adminuser value1 -adminpassword value2 ....
```

## Next steps
Now that you have a functioning cluster, see these documents for connection and management details:

* [Connect to an Azure Container Service cluster](container-service-connect.md)
* [Work with Azure Container Service and DC/OS](container-service-mesos-marathon-rest.md)
* [Work with Azure Container Service and Docker Swarm](container-service-docker-swarm.md)
* [Work with Azure Container Service and Kubernetes](container-service-kubernetes-walkthrough.md)
