---
title: Deploy a Docker container cluster - Azure CLI | Microsoft Docs
description: Deploy an Azure Container Service cluster using Azure CLI 2.0 Preview
services: container-service
documentationcenter: ''
author: sauryadas
manager: timlt
editor: ''
tags: acs, azure-container-service
keywords: ''

ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: saudas

---
# Using the Azure CLI 2.0 preview to create an Azure Container Service cluster

To create an Azure Container Service cluster, you must:
* have an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/))
* have installed the [Azure CLI v. 2.0 (Preview)](https://github.com/Azure/azure-cli#installation) installed
* be logged in to your Azure account (see below)

## Log in to your account
```azurecli
az login 
```
Go to this [link](https://login.microsoftonline.com/common/oauth2/deviceauth) to authenticate with the device code provided in the CLI.

![type command](media/container-service-create-acs-cluster-cli/login.png)

![browser](media/container-service-create-acs-cluster-cli/login-browser.png)


## Create a resource group
```azurecli
az group create -n acsrg1 -l "westus"
```

![Image resource group create](media/container-service-create-acs-cluster-cli/rg-create.png)

## List of available Azure Container Service CLI commands

```azurecli
az acs -h
```

![ACS command usage](media/container-service-create-acs-cluster-cli/acs-command-usage-help.png)

## Create an Azure Container Service Cluster

*ACS create usage in the CLI*

```azurecli
az acs create -h
```
The name of the container service, the resource group created in the previous step, and a unique DNS name are mandatory. 
Other inputs are set to default values (see the screenshot below) unless overwritten using their respective switches.
![Image ACS create help](media/container-service-create-acs-cluster-cli/acs-command-usage-help.png)

*Quick ACS create using defaults. If you do not have a SSH key use the second command. This second create command with the --generate-ssh-keys switch will create one for you*

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

*Please ensure that the dns-prefix (-d switch) is unique. If you get an error, please try again with a unique string.*

After you type the preceding command, wait for about 10 minutes for the cluster to be created.

![Image ACS create](media/container-service-create-acs-cluster-cli/cluster-create.png)

## List ACS clusters 

### Under a subscription

```azurecli
az acs list --output table
```

### In a specific resource group

```azurecli
az acs list -g acsrg1 --output table
```

![Image ACS list](media/container-service-create-acs-cluster-cli/acs-list.png)


## Display details of a container service cluster

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![Image ACS list](media/container-service-create-acs-cluster-cli/acs-show.png)


## Scale the ACS cluster
*Both scaling in and scaling out are allowed. The paramater new-agent-count is the new number of agents in the ACS cluster.*

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![Image ACS scale](media/container-service-create-acs-cluster-cli/acs-scale.png)

## Delete a container service cluster
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
*Note that this delete command does not delete all resources (network and storage) created while creating the container service. To delete all resources, it is recommended that a single ACS cluster be created per resource group and then the resource group itself be deleted when the acs cluster is no longer required to ensure that all related resources are deleted and you are not charged for them.*
