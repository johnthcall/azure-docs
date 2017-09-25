---
title: Architectures for running VM workloads in Azure | Microsoft Docs
description: Explains some common architectures for deploying VMs that host enterprise-scale applications in Azure.
services: ''
documentationcenter: na
author: telmosampaio
manager: christb
editor: ''
tags: ''

ms.assetid: 0afd8bba-ce0c-438e-be82-70064808a956
ms.service: guidance
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2016
ms.author: telmosampaio

---
# Architectures for running VM workloads in Azure

[!INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Running a virtual machine (VM) in Azure involves more moving parts than just the VM itself. Other considerations include networking, load balancers, network security groups (NSGs), and redundancy within a region or across multiple regions.

The patterns & practices group has created a set of reference architectures to address these considerations. Each reference architecture includes:

* Recommendations and best practices.
* Considerations for availability, security, scalability, and manageability.
* An Azure Resource Manager template that you can modify and deploy.

The reference architectures in this series are designed to build on each other, starting from deploying a single VM, then moving to multiple VMs behind a load balancer, VMs running N-tier applications, and finally multi-region deployments.

## Running a single VM
This reference architecture contains a set of baseline recommendations for running any Windows or Linux VM in Azure. 

> [!NOTE]
> This architecture is the basis for the other architectures in this series. It is important to understand the concepts here, even if you are not deploying a single VM by itself.
> 
> 

[![0]][0]

Considerations:

* Not recommended for mission-critical systems. Instead, it is recommended to put two or more VMs in an availability set. The next architecture demonstrates this approach.

For detailed information, see [Running a Windows VM on Azure][single-vm] and [Running a Linux VM on Azure][single-vm-linux].

## Running multiple VMs behind a load balancer

To improve availability and scalability, deploy several VMs in an availability set and use a load balancer to distribute traffic across them.  

[![1]][1]

Benefits:

* Provides improved availability and scalability.
* This architecture is a building block for running N-tier applications on Azure, as shown in the next architecture.

For detailed information, see [Running multiple VMs on Azure for scalability and availability][multiple-vms].

## Running N-tier workloads
This architecture shows best practices for running an N-tier application on VMs in Azure.

The VMs for each tier are placed in separate availability sets.  Within a tier, load balancers distribute traffic across the VMs. At the data tier, the database is replicated for higher availability.

[![2]][2]

For detailed information, see:

* [Running Windows VMs for an N-tier architecture on Azure][multiple-tiers]. This architecture uses SQL Server Always On Availability Groups. 
* [Running Linux VMs for an N-tier architecture on Azure][multiple-tiers-linux]. This architecture uses Apache Cassandra.

## Running multi-region workloads
An application deployed to a single region could become unavailable if an incident occurs within that region. For mission-critical applications, consider deploying to more than one region.

This architecture show how to deploy a multi-region N-tier application in an active/passive configuration. During normal operation, Azure Traffic Manager routes traffic to the primary region. If the primary region becomes unavailable, Traffic Manager fails over to the secondary region.  

[![3]][3]

Benefits of deploying to multiple regions:

* High availability.
* Protection against regional outages.

Considerations:

* Increased complexity and cost.
* Failover and failback may require some manual steps, such as failing over the database and checking database consistency.

For detailed information, see:

* [Running Windows VMs in multiple regions for high availability][multiple-regions]. This architecture uses SQL Server Always On Availability Groups. 
* [Running Linux VMs in multiple regions for high availability][multiple-regions-linux]. This architecture uses Apache Cassandra.

## Next steps
The resources below explain how to implement the architectures described in this article.

* [Running a Windows VM on Azure][single-vm]
* [Running a Linux VM on Azure][single-vm-linux]
* [Running multiple VMs on Azure for scalability and availability][multiple-vms]
* [Running Windows VMs for an N-tier architecture on Azure][multiple-tiers]
* [Running Windows VMs for an N-tier architecture on Azure][multiple-tiers-linux]
* [Running Windows VMs in multiple regions for high availability][multiple-regions] 
* [Running Linux VMs in multiple regions for high availability][multiple-regions-linux]

<!-- Links -->
[0]: ./media/compute/compute-single-vm.png "Single VM architecture in Azure"
[1]: ./media/compute/compute-multi-vm.png "Multiple VM architecture in Azure"
[2]: ./media/compute/compute-multi-tier.png "Multiple tier architecture in Azure"
[3]: ./media/compute/compute-multi-region.png "Multiple region architecture in Azure"
[single-vm]: ./guidance-compute-single-vm.md
[single-vm-linux]: ./guidance-compute-single-vm-linux.md 
[multiple-vms]: ./guidance-compute-multi-vm.md
[multiple-tiers]: ./guidance-compute-n-tier-vm.md
[multiple-tiers-linux]: ./guidance-compute-n-tier-vm-linux.md
[multiple-regions]: ./guidance-compute-multiple-datacenters.md
[multiple-regions-linux]: ./guidance-compute-multiple-datacenters-linux.md
