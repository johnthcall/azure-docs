---
title: Community tools - Move classic resources to Azure Resource Manager | Microsoft Docs
description: This article catalogs the tools that have been provided by the community to help migrate IaaS resources from classic to the Azure Resource Manager deployment model.
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: timlt
editor: ''
tags: azure-resource-manager

ms.assetid: 228b697b-3950-49f5-84bb-283bb56621b1
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 1/23/2017
ms.author: singhkay

---
# Community tools to migrate IaaS resources from classic to Azure Resource Manager
This article catalogs the tools that have been provided by the community to assist with migration of IaaS resources from classic to the Azure Resource Manager deployment model.

> [!NOTE]
> These tools are not officially supported by Microsoft Support. Therefore they are open sourced on Github and we're happy to accept PRs for fixes or additional scenarios. To report an issue, use the Github issues feature.
> 
> Migrating with these tools will cause downtime for your classic Virtual Machine. If you're looking for platform supported migration, visit 
> 
> * [Platform supported migration of IaaS resources from Classic to Azure Resource Manager stack](virtual-machines-windows-migration-classic-resource-manager.md)
> * [Technical Deep Dive on Platform supported migration from Classic to Azure Resource Manager](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
> * [Migrate IaaS resources from Classic to Azure Resource Manager using Azure PowerShell](virtual-machines-windows-ps-migration-classic-resource-manager.md)
> 
> 

## ASM2ARM
This is a PowerShell script module for migrating your **single** Virtual Machine (VM) from classic to Azure Resource Manager deployment model. 

[Link to the tool documentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/asm2arm)

## migAz
migAz is an additional option to migrate a complete set of classic IaaS resources to Azure Resource Manager IaaS resources. The migration can occur within the same subscription or between different subscriptions and subscription types (ex: CSP subscriptions).

[Link to the tool documentation](https://github.com/Azure/classic-iaas-resourcemanager-migration/tree/master/migaz)

