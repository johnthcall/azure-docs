---
title: Deploy Linux VM into existing network with the Azure CLI 2.0 (Preview) | Microsoft Docs
description: Learn how to deploy a Linux virtual machine into an existing Virtual Network using the Azure CLI 2.0 (Preview)
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: ''
tags: azure-resource-manager

ms.assetid:
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: iainfou

---

# Deploy a Linux VM into an existing virtual network using the Azure CLI 2.0 (Preview)

This article shows you how to use the Azure CLI 2.0 (Preview) to deploy a virtual machine (VM) into an existing virtual network. The requirements are:

- [an Azure account](https://azure.microsoft.com/pricing/free-trial/)
- [SSH public and private key files](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)


## CLI versions to complete the task
You can complete the task using one of the following CLI versions:

- [Azure CLI 1.0](virtual-machines-linux-deploy-linux-vm-into-existing-vnet-using-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – our CLI for the classic and resource management deployment models
- [Azure CLI 2.0 (Preview)](#quick-commands) - our next generation CLI for the resource management deployment model (this article)


## Quick Commands
If you need to quickly accomplish the task, the following section details the  commands needed. More detailed information and context for each step can be found the rest of the document, [starting here](#detailed-walkthrough).

To create this custom environment, you need the latest [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).

In the following examples, replace example parameter names with your own values. Example parameter names include `myResourceGroup`, `myVnet`, and `myVM`.

**Pre-requirements:** Azure resource group, virtual network and subnet, network security group with SSH inbound, and a virtual network interface card.

### Deploy the VM into the virtual network infrastructure

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username ops \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic \
    --vnet myVnet \
    --subnet-name mySubnet \
    --nsg myNetworkSecurityGroup
```

## Detailed walkthrough

It is recommended that Azure assets like virtual networks and network security groups should be static and long lived resources that are rarely deployed. Once a virtual network has been deployed, it can be reused by new deployments without any adverse affects to the infrastructure. Think about a virtual network as being a traditional hardware network switch - you would not need to configure a brand new hardware switch with each deployment. With a correctly configured virtual network, we can continue to deploy new VMs into that virtual network over and over with few, if any, changes required over the life of the virtual network.

To create this custom environment, you need the latest [Azure CLI 2.0 (Preview)](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).

In the following examples, replace example parameter names with your own values. Example parameter names include `myResourceGroup`, `myVnet`, and `myVM`.

## Create the resource group

First we create an Azure resource group to organize everything we create in this walkthrough. For more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Create the resource group with [az group create](/cli/azure/group#create). The following example creates a resource group named `myResourceGroup` in the `westus` location:

```azurecli
az group create \
    --name myResourceGroup \
    --location westus
```

## Create the virtual network

Lets build an Azure virtual network to launch the VMs into. For more information on virtual networks, see [Create a virtual network by using the Azure CLI](../virtual-network/virtual-networks-create-vnet-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Create the virtual network with [az network vnet create](/cli/azure/network/vnet#create). The following example creates a virtual network named `myVnet` and subnet named `mySubnet`:

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --location westus \
    --name myVnet \
    --address-prefix 10.10.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.10.1.0/24
```

## Create the network security group

Azure network security groups are equivalent to a firewall at the network layer. For more information on network security groups, see [How to create network security groups in the Azure CLI](../virtual-network/virtual-networks-create-nsg-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Create the network security group with [az network nsg create](/cli/azure/network/nsg#create). The following example creates a network security group named `myNetworkSecurityGroup`:

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --location westus \
    --name myNetworkSecurityGroup
```

## Add an inbound SSH allow rule

The Linux VM needs access from the internet, so a rule allowing inbound port 22 traffic to be passed through the network to port 22 on the Linux VM is needed. Add an inbound rule for the network security group with [az network nsg rule create](/cli/azure/network/nsg/rule#create). The following example creates a rule named `myNetworkSecurityGroupRuleSSH`:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRuleSSH \
    --protocol tcp \
    --direction inbound \
    --priority 1000 \
    --source-address-prefix '*' \
    --source-port-range '*' \
    --destination-address-prefix '*' \
    --destination-port-range 22 \
    --access allow
```

## Attach the subnet to the network security group

The network security group rules can be applied to a subnet or a specific virtual network interface. Lets attach the network security group to our subnet. Attach your subnet to the network security group with [az network vnet subnet update](/cli/azure/network/vnet/subnet#update):

```azurecli
az network vnet subnet update \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnet \
    --network-security-group myNetworkSecurityGroup
```

## Add a virtual network interface card to the subnet

Virtual network interface cards (VNics) are important as you can reuse them by connecting them to different VMs. This reuse allows you to keep the VNic as a static resource while the VMs can be temporary. Create a VNic and associate it with the subnet with [az network nic create](/cli/azure/network/nic#create). The following example creates a VNic named `myNic`:

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --location westus \
    --name myNic \
    --vnet-name myVnet \
    --subnet mySubnet
```

## Deploy the VM into the virtual network infrastructure

We now have a virtual network, a subnet, and a network security group acting as a firewall to protect our subnet by blocking all inbound traffic except port 22 for SSH. The VM can now be deployed inside this existing network infrastructure.

Create your VM with [az vm create](/cli/azure/vm#create). For more information on the flags to use with the Azure CLI 2.0 (Preview) to deploy a complete VM, see [Create a complete Linux environment by using the Azure CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Debian \
    --admin-username ops \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic \
    --vnet myVnet \
    --subnet-name mySubnet \
    --nsg myNetworkSecurityGroup
```

By using the CLI flags to call out existing resources, we instruct Azure to deploy the VM inside the existing network. To reiterate, once a virtual network and subnet have been deployed, they can be left as static or permanent resources inside your Azure region. In this example, we did not create and assign a public IP address to the VNic, so this VM is not publicly accessible over the Internet. For more information, see [Create a VM with a static public IP using the Azure CLI](../virtual-network/virtual-network-deploy-static-pip-arm-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## Next steps
For more information about ways to create virtual machines in Azure, see the following resources:

* [Use an Azure Resource Manager template to create a specific deployment](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Create your own custom environment for a Linux VM using Azure CLI commands directly](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Create a Linux VM on Azure using templates](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
