---
title: High-performance storage for Azure virtual machine workloads | Microsoft Docs
description: Premium Storage offers high-performance, low-latency disk support for I/O-intensive workloads running on Azure Virtual Machines. Azure DS-series, DSv2-series and GS-series VMs support Premium Storage.
services: storage
documentationcenter: ''
author: yuemlu
manager: aungoo-msft
editor: tysonn

ms.assetid: e2a20625-6224-4187-8401-abadc8f1de91
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: yuemlu

---
# Premium Storage: High-performance storage for Azure virtual machine workloads
## Overview
Azure Premium Storage delivers high-performance, low-latency disk support for virtual machines running I/O-intensive workloads. Virtual machine (VM) disks that use Premium Storage store data on solid state drives (SSDs). You can migrate your application's VM disks to Azure Premium Storage to take advantage of the speed and performance of these disks.

An Azure VM supports attaching several Premium Storage disks, so that your applications can have up to 64 TB of storage per VM. With Premium Storage, your applications can achieve 80,000 IOPS (input/output operations per second) per VM and 2000 MB per second disk throughput per VM with extremely low latencies for read operations.

With Premium Storage, Azure offers the ability to truly lift-and-shift your demanding enterprise applications like, Dynamics AX, Dynamics CRM, Exchange Server, SharePoint Farms, and SAP Business Suite, to the cloud. You can run a variety of performance intensive database workloads like SQL Server, Oracle, MongoDB, MySQL, Redis, that require consistent high performance and low latency on Premium Storage.

> [!NOTE]
> We recommend migrating any virtual machine disk requiring high IOPS to Azure Premium Storage for the best performance for your application. If your disk does not require high IOPS, you can limit costs by maintaining it in Standard Storage, which stores virtual machine disk data on Hard Disk Drives (HDDs) instead of SSDs.
> 
> 

To get started with Azure Premium Storage, visit [Get started for free](https://azure.microsoft.com/pricing/free-trial/) page. For information on migrating your existing virtual machines to Premium Storage, see [Migrating to Azure Premium Storage](storage-migration-to-premium-storage.md).

> [!NOTE]
> Premium Storage is currently supported in some regions. You can find the list of available regions in [Azure Services by Region](https://azure.microsoft.com/regions/#services).
> 
> 

## Premium Storage Features
**Premium Storage Disks**: Azure Premium Storage supports VM disks that can be attached to Premium Storage supported Azure VMs (DS, DSv2, GS, or Fs series). When using Premium Storage you have a choice of three disk sizes namely, P10 (128GiB), P20 (512GiB) and P30 (1024GiB), each with its own performance specifications. Depending on your application requirement you can attach one or more of these disks to your Premium Storage supported VM. In the following section on [Premium Storage Scalability and Performance Targets ](#premium-storage-scalability-and-performance-targets) we will describe the specifications in more detail.

**Premium Page Blob**: Premium Storage supports Azure Page Blobs, which are used to hold persistent disks for Azure Virtual Machines (VMs). Currently, Premium Storage does not support Azure Block Blobs, Azure Append Blobs, Azure Files, Azure Tables, or Azure Queues. Any other object placed in a Premium Storage account will be a Page Blob, and it will snap to one of the supported provisioned sizes. Hence Premium Storage account is not meant for storing tiny blobs.

**Premium Storage account**: To start using Premium Storage, you must create a Premium Storage account. If you prefer to use the [Azure portal](https://portal.azure.com), you can create a Premium Storage account by specifying the “Premium” performance tier and “Locally-redundant storage (LRS)” as the replication option. You can also create a Premium Storage account by specifying the type as “Premium_LRS” using the [Storage REST API](http://msdn.microsoft.com//library/azure/dd179355.aspx) version 2014-02-14 or later; the [Service Management REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx) version 2014-10-01 or later (Classic deployments); the [Azure Storage Resource Provider REST API Reference](http://msdn.microsoft.com/library/azure/mt163683.aspx) (Resource Manager deployments); and the [Azure PowerShell](/powershell/azureps-cmdlets-docs) version 0.8.10 or later. Learn about premium storage account limits in the following section on [Premium Storage Scalability and Performance Targets](#premium-storage-scalability-and-performance-targets).

**Premium Locally Redundant Storage**: A Premium Storage account only supports Locally Redundant Storage (LRS) as the replication option and keeps three copies of the data within a single region. For considerations regarding geo replication when using Premium Storage, see the [Snapshots and Copy Blob](#snapshots-and-copy-blob) section in this article.

Azure uses the storage account as a container for your operating system (OS) and data disks. When you create an Azure DS, DSv2, GS, or Fs  VM and select an Azure Premium Storage account, your operating system and data disks are stored in that storage account.

You can use Premium Storage for Disks in one of two ways:

* First, create a new premium storage account. Next, when creating a new DS, DSv2, GS, or Fs VM, select the premium storage account in the Storage configuration settings. OR,
* When creating a new DS, DSv2, GS, or Fs VM create a new premium storage account in Storage configuration settings, or let Azure portal create a default premium storage account.

For step-by-step instructions, see the [Quick Start](#quick-start) section later in this article.

> [!NOTE]
> A premium storage account cannot be mapped to a custom domain name.
> 
> 

## Premium Storage supported VMs
Premium Storage supports DS-series, DSv2-series, GS-series, and Fs-series Azure Virtual Machines (VMs). You can use both Standard and Premium storage disks with Premium Storage supported of VMs. But you cannot use Premium Storage disks with VM series which are not Premium Storage compatible.

For information on available Azure VM types and sizes for Windows VMs, see [Windows VM sizes](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). For information on VM types and sizes for Linux VMs, see [Linux VM sizes](../virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Following are some of the features of DS, DSv2, GS, and Fs series VMs:

**Cloud Service**: DS-series VMs can be added to a cloud service that includes only DS-series VMs. Do not add DS-series VMs to an existing cloud service that includes non-DS-series VMs. You can migrate your existing VHDs to a new cloud service running only DS-series VMs. If you want to retain the same virtual IP address (VIP) for the new cloud service that hosts your DS-series VMs, use the [Reserved IP Addresses](../virtual-network/virtual-networks-instance-level-public-ip.md). GS-series VMs can be added to an existing cloud service running only G-series VMs.

**Operating System Disk**: The Premium Storage supported Azure virtual machines can be configured to use an operating system (OS) disk hosted either on a Standard Storage account or on a Premium Storage account. We recommend using Premium Storage based OS disk for best experience.

**Data Disks**: You can use both Premium and Standard storage disks in the same Premium Storage supported VM. With Premium Storage, you can provision a Premium Storage supported VM and attach several persistent data disks to the VM. If needed, you can stripe across the disks to increase the capacity and performance of the volume.

> [!NOTE]
> If you stripe Premium Storage data disks using [Storage Spaces](http://technet.microsoft.com/library/hh831739.aspx), you should configure it with one column for each disk that is used. Otherwise, overall performance of the striped volume may be lower than expected due to uneven distribution of traffic across the disks. By default, the Server Manager user interface (UI) allows you to setup columns up to 8 disks. But if you have more than 8 disks, you need to use PowerShell to create the volume and also specify the number of columns manually. Otherwise, the Server Manager UI continues to use 8 columns even though you have more disks. For example, if you have 32 disks in a single stripe set, you should specify 32 columns. You can use the *NumberOfColumns* parameter of the [New-VirtualDisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell cmdlet to specify the number of columns used by the virtual disk. For more information, see [Storage Spaces Overview](http://technet.microsoft.com/library/hh831739.aspx) and [Storage Spaces Frequently Asked Questions](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx).
> 
> 

**Cache**: Premium Storage supported VMs have a unique caching capability with which you can get high levels of throughput and latency, which exceeds underlying Premium Storage disk performance. You can configure disk caching policy on the Premium Storage disks as ReadOnly, ReadWrite or None. The default disk caching policy is ReadOnly for all premium data disks and ReadWrite for operating system disks. Use the right configuration setting to achieve optimal performance for your application. For example, for read heavy or read only data disks, such as SQL Server data files, set disk caching policy to “ReadOnly”. For write heavy or write only data disks, such as SQL Server log files, set disk caching policy to “None”. Learn more about optimizing your design with Premium Storage in [Design for Performance with Premium Storage](storage-premium-storage-performance.md).

**Analytics**: To analyze the performance of VMs using disks on Premium Storage accounts, you can enable the Azure VM Diagnostics in the Azure portal. Refer to [Microsoft Azure Virtual Machine Monitoring with Azure Diagnostics Extension](https://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/) for details. To see the disk performance, use operating system based tools, such as [Windows Performance Monitor](https://technet.microsoft.com/library/cc749249.aspx) for Windows VMs and [IOSTAT](http://linux.die.net/man/1/iostat) for Linux VMs.

**VM scale limits and performance**: Each Premium Storage supported VM size has scale limits and performance specification for IOPS, bandwidth and number of disks that can be attached per VM. When using premium storage disks with Premium Storage supported VMs, make sure there is sufficient IOPS and Bandwidth available on your VM to drive the disk traffic.
For example, a STANDARD_DS1 VM has 32 MB per second dedicated bandwidth available for Premium Storage disk traffic. A P10 premium storage disk can provide 100 MB per second bandwidth. If a P10 Premium Storage disk were attached to this VM, it can only go up to 32 MB per second but not up to 100 MB per second that the P10 disk can provide.

Currently, the largest VM on DS-series is Standard_DS15_v2 and it can provide up to 960 MB per second across all disks. The largest VM on GS-series is Standard_GS5 and it can give up to 2000 MB per second across all disks.
Note that these limits are for disk traffic alone, not including cache-hits and network traffic. There is a separate bandwidth available for VM network traffic, which is different from the dedicated bandwidth for Premium Storage disks.

For the most up-to-date information on maximum IOPS and throughput (bandwidth) for Premium Storage supported VMs, see [Windows VM sizes](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) or [Linux VM sizes](../virtual-machines/virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

To learn about the Premium storage disks and their IOPs and throughput limits, see the table in the [Premium Storage Scalability and Performance Targets](#premium-storage-scalability-and-performance-targets) section in this article.

## Premium Storage Scalability and Performance Targets
In this section, we will describe all the Scalability and Performance targets you must consider when using Premium Storage.

### Premium Storage account limits
Premium Storage accounts have following scalability targets:

| Total Account Capacity | Total Bandwidth for a Locally Redundant Storage account |
| --- | --- | 
| Disk Capacity: 35 TB <br>Snapshot capacity: 10 TB | Up to 50 gigabits per second for Inbound + Outbound |

* Inbound refers to all data (requests) being sent to a storage account.
* Outbound refers to all data (responses) being received from a storage account.

For more information, see [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).

If the needs of your application exceed the scalability targets of a single storage account, build your application to use multiple storage accounts, and partition your data across those storage accounts. For example, if you want to attach 51 terabytes (TB) disks across a number of VMs, spread them across two storage accounts since 35 TB is the limit for a single Premium Storage account. Make sure that a single Premium Storage account has never more than 35 TB of provisioned disks.

### Premium Storage Disks Limits
When you provision a disk against a Premium Storage account, how much input/output operations per second (IOPS) and throughput (bandwidth) it can get depends on the size of the disk. Currently, there are three types of Premium Storage disks: P10, P20, and P30. Each one has specific limits for IOPS and throughput as specified in the following table:

|Premium Storage Disk Type | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Disk Size | 128 GiB | 512 GiB | 1024 GiB (1 TB) |
| IOPS per disk | 500 | 2300 | 5000 |
Throughput per disk | 100 MB per second | 150 MB per second | 200 MB per second |

> [!NOTE]
> Make sure that there is sufficient bandwidth available on your VM to drive the disk traffic as explained in the [Premium Storage supported VMs](#ds-dsv2-and-gs-series-vms) section earlier in this article. Otherwise, your disk throughput and IOPS will be constrained to lower values based on the VM limits rather than the disk limits mentioned in the previous table.  
> 
> 

Here are some important things you must know regarding Premium Storage scalability and performance targets:

* **Provisioned Capacity and Performance**: When you provision a premium storage disk, unlike standard storage, you are guaranteed the Capacity, IOPS and Throughput for that disk. For example, if you create a P30 disk, Azure provisions 1024 GB storage capacity, 5000 IOPS and 200 MB per second Throughput for that disk. Your application can use all or part of the capacity and performance.
* **Disk Size**: Azure maps the disk size (rounded up) to the nearest Premium Storage Disk option as specified in the table. For example, a disk of size 100 GiB is classified as a P10 option and can perform up to 500 IO units per second, and with up to 100 MB per second throughput. Similarly, a disk of size 400 GiB is classified as a P20 option, and can perform up to 2300 IO units per second and up to 150 MB per second throughput.
  
  > [!NOTE]
  > You can easily increase the size of existing disks. For example, if you wish to increase the size of a 30 GB disk to 128GB or to 1 TB. Or, if you wish to convert your P20 disk to a P30 disk because you need more capacity or more IOPS and throughput. You can expand the disk using "Update-AzureDisk" PowerShell commandlet with "-ResizedSizeInGB" property. For performing this action, disk needs to be detached from the VM or the VM needs to be stopped.
  > 
  > 
* **IO Size**: The input/output (I/O) unit size is 256 KB. If the data being transferred is less than 256 KB, it is considered a single I/O unit. The larger I/O sizes are counted as multiple I/Os of size 256 KB. For example, 1100 KB I/O is counted as five I/O units.
* **Throughput**: The throughput limit includes writes to the disk as well as reads from that disk that are not served from the cache. For example, a P10 disk has 100 MB per second throughput per disk. Some examples of valid throughput for the P10 disk are,

| Max Throughput per P10 disk | Non-cache Reads from disk | Non-cache Writes to disk |
| --- | --- | --- |
| 100 MB per sec | 100 MB per sec | 0 |
| 100 MB per sec | 0 | 100 MB per sec |
| 100 MB per sec | 60 MB per sec | 40 MB per sec |
  
* **Cache hits**: Cache-hits are not limited by the allocated IOPS/Throughput of the disk. For example, when you use a data disk with ReadOnly cache setting on a Premium Storage supported VM, Reads that are served from the cache are not subject to Hence you could get very high throughput from a disk if the workload is predominantly Reads. Note that, cache is subject to separate IOPS / Throughput limits at VM level based on the VM size. DS-series VMs have roughly 4000 IOPS and 33 MB/sec per core for cache and local SSD IOs. GS-series VMs have a limit of 5000 IOPS and 50 MB/sec per core for cache and local SSD IOs.

## Throttling
You may see throttling if your application IOPS or throughput exceed the allocated limits for a Premium Storage disk or if your total disk traffic across all disks on the VM exceeds the disk bandwidth limit available for the VM. To avoid throttling, we recommend that you limit the number of pending I/O requests for disk based on the scalability and performance targets for the disk you have provisioned and based on the disk bandwidth available to the VM.  

Your application can achieve the lowest latency when it is designed to avoid throttling. On the other hand, if the number of pending I/O requests for the disk is too small, your application cannot take advantage of the maximum IOPS and throughput levels that are available to the disk.

The following examples demonstrate how to calculate the throttling levels. All calculations are based on I/O unit size of 256 KB:

### Example 1:
Your application has done 495 I/O units of 16 KB size in one second on a P10 disk. These will be counted as 495 I/O Units per second (IOPS). If you try a 2 MB I/O in the same second, the total of I/O units is equal to 495 + 8. This is because 2 MB I/O results in 2048 KB / 256 KB = 8 I/O Units when the I/O unit size is 256 KB. Since the sum of 495 + 8 exceeds the 500 IOPS limit for the disk, throttling occurs.

### Example 2:
Your application has done 400 I/O units of 256 KB size on a P10 disk. The total bandwidth consumed is (400 * 256) / 1024 = 100 MB/sec. A P10 disk has throughput limit of 100 MB per second. If your application tries to perform more I/O in that second, it gets throttled because it exceeds the allocated limit.

### Example 3:
You have a DS4 VM with two P30 disks attached. Each P30 disk is capable of 200 MB per second throughput. However, a DS4 VM has a total disk bandwidth capacity of 256 MB per second. Therefore, you cannot drive the attached disks to the maximum throughput on this DS4 VM at the same time. To resolve this, you can sustain traffic of 200 MB per second on one disk and 56 MB per second on the other disk. If the sum of your disk traffic goes over 256 MB per second, the disk traffic gets throttled.

> [!NOTE]
> If the disk traffic mostly consists of small I/O sizes, it is highly likely that your application will hit the IOPS limit before the throughput limit. On the other hand, if the disk traffic mostly consists of large I/O sizes, it is highly likely that your application will hit the throughput limit instead of the IOPS limit. You can maximize your application IOPS and throughput capacity by using optimal I/O sizes and also by limiting the number of pending I/O requests for disk.
> 
> 

To learn about designing for high performance using Premium Storage read the article, [Design for Performance with Premium Storage](storage-premium-storage-performance.md).

## Snapshots and Copy Blob
You can create a snapshot for Premium Storage in the same way as you create a snapshot when using Standard Storage. Since Premium Storage only supports Locally Redundant Storage (LRS) as the replication option, we recommend that you create snapshots and then copy those snapshots to a geo-redundant standard storage account. For more information, see [Azure Storage Redundancy Options](storage-redundancy.md).

If a disk is attached to a VM, certain API operations are not permitted on the page blob backing the disk. For example, you cannot perform a [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) operation on that blob as long as the disk is attached to a VM. Instead, first create a snapshot of that blob by using the [Snapshot Blob](http://msdn.microsoft.com/library/azure/ee691971.aspx) REST API method, and then perform the [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) of the snapshot to copy the attached disk. Alternatively, you can detach the disk and then perform any necessary operations on the underlying blob.

Following limits apply to Premium Storage blob snapshots:

| Premium Storage Limit | Value |
| --- | --- |
| Max. number of snapshots per blob | 100 |
| Storage account capacity for snapshots (includes data in snapshots only, and does not include data in base blob) | 10 TB |
| Min. time between consecutive snapshots | 10 minutes |

To maintain geo-redundant copies of your snapshots, you can copy snapshots from a Premium Storage account to a geo-redundant standard storage account by using AzCopy or Copy Blob. For more information, see [Transfer data with the AzCopy Command-Line Utility](storage-use-azcopy.md) and [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx).

For detailed information on performing REST operations against page blobs in Premium Storage accounts, see [Using Blob Service Operations with Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969) in the MSDN library.

## Using Linux VMs with Premium Storage
Please refer to important instructions below for configuring your Linux VMs on Premium Storage:

* For all Premium Storage disks with cache setting as either “ReadOnly” or “None”, you must disable “barriers” while mounting the file system in order to achieve the scalability targets for Premium Storage. You do not need barriers for this scenario because the writes to Premium Storage backed disks are durable for these cache settings. When the write request successfully completes, data has been written to the persistent store. Please use the following methods for disabling “barriers” depending on your file system:
  
  * If you use **reiserFS**, disable barriers using the mount option “barrier=none” (For enabling barriers, use “barrier=flush”)
  * If you use **ext3/ext4**, disable barriers using the mount option “barrier=0” (For enabling barriers, use “barrier=1”)
  * If you use **XFS**, disable barriers using the mount option “nobarrier” (For enabling barriers, use the option “barrier”)
* For Premium Storage disks with cache setting “ReadWrite”, barriers should be enabled for durability of writes.
* For the volume labels to persist after VM reboot, you must update /etc/fstab with the UUID references to the disks. Also refer to [How to Attach a Data Disk to a Linux Virtual Machine](../virtual-machines/virtual-machines-linux-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json)

Following are the Linux Distributions that we validated with Premium Storage. We recommend that you upgrade your VMs to at least one of these versions (or later) for better performance and stability with Premium Storage. Also, some of the versions require the latest LIS (Linux Integration Services v4.0 for Microsoft Azure). Please follow the link provided below for download and installation. We will continue to add more images to the list as we complete additional validations. Please note, our validations showed that performance varies for these images, and it also depends on workload characteristics and settings on the images. Different images are tuned for different kinds of workload.

| Distribution | Version | Supported Kernel | Details |
| --- | --- | --- | --- |
| Ubuntu | 12.04 | 3.2.0-75.110+ | Ubuntu-12_04_5-LTS-amd64-server-20150119-en-us-30GB |
| Ubuntu | 14.04 | 3.13.0-44.73+ | Ubuntu-14_04_1-LTS-amd64-server-20150123-en-us-30GB |
| Debian | 7.x, 8.x | 3.16.7-ckt4-1+ | &nbsp; |
| SUSE | SLES 12| 3.12.36-38.1+| suse-sles-12-priority-v20150213 <br> suse-sles-12-v20150213 |
| SUSE | SLES 11 SP4 | 3.0.101-0.63.1+ | &nbsp; |
| CoreOS | 584.0.0+| 3.18.4+ | CoreOS 584.0.0 |
| CentOS | 6.5, 6.6, 6.7, 7.0 | &nbsp; | [LIS4 Required](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *See note below* |
| CentOS | 7.1+ | 3.10.0-229.1.2.el7+ | [LIS4 Recommended](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) <br> *See note below* |
| RHEL | 6.8+, 7.2+ | &nbsp; | &nbsp; |
| Oracle | 6.0+, 7.2+ | &nbsp; | UEK4 or RHCK |
| Oracle | 7.0-7.1 | &nbsp; | UEK4 or RHCK w/[LIS 4.1+](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |
| Oracle | 6.4-6.7 | &nbsp; | UEK4 or RHCK w/[LIS 4.1+](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409) |


### LIS Drivers for Openlogic CentOS
Customers running OpenLogic CentOS VMs should run the following command to install the latest drivers:

```
sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
sudo yum install microsoft-hyper-v
```

A reboot will then be required to activate the new drivers.

## Pricing and Billing
When using Premium Storage, the following billing considerations apply:

* Premium Storage Disk / Blob Size
* Premium Storage Snapshots
* Outbound data transfers

**Premium Storage Disk / Blob Size**: Billing for a Premium Storage disk/blob depends on the provisioned size of the disk/blob. Azure maps the provisioned size (rounded up) to the nearest Premium Storage Disk option as specified in the table given in the [Scalability and Performance Targets when using Premium Storage](#premium-storage-scalability-and-performance-targets) section. All objects stored in a Premium Storage account will map to one of the the supported provisioned sizes and will be billed accordingly. Hence avoid using Premium Storage account for storing tiny blobs. Billing for any provisioned disk/blob is prorated hourly using the monthly price for the Premium Storage offer. For example, if you provisioned a P10 disk and deleted it after 20 hours, you are billed for the P10 offering prorated to 20 hours. This is regardless of the amount of actual data written to the disk or the IOPS/throughput used.

**Premium Storage Snapshots**: Snapshots on Premium Storage are billed for the additional capacity used by the snapshots. For information on snapshots, see [Creating a Snapshot of a Blob](http://msdn.microsoft.com/library/azure/hh488361.aspx).

**Outbound data transfers**: [Outbound data transfers](https://azure.microsoft.com/pricing/details/data-transfers/) (data going out of Azure data centers) incur billing for bandwidth usage.

For detailed information on pricing for Premium Storage,  Premium Storage supported VMs, see:

* [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/)
* [Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/)

## Backup
Virtual machines using premium storage can be backed up using Azure Backup. [More details](../backup/backup-azure-vms-first-look-arm.md).

## Quick Start: Create and use a Premium Storage account for a virtual machine data disk
In this section we will demonstrate the following scenarios using Azure portal, Azure PowerShell and Azure CLI:

* How to create a Premium Storage account.
* How to create a virtual machine and attach a data disk to the virtual machine when using Premium Storage.
* How to change disk caching policy of a data disk attached to a virtual machine.

### Create an Azure virtual machine using Premium Storage via the Azure portal

To create a virtual machine in Premium Storage, you first have to create a Premium Storage account.

#### Create a Premium Storage account in Azure portal

This section shows how to create a Premium Storage account using the Azure portal.

1. Sign in to the [Azure portal](https://portal.azure.com). Check out the [Free Trial](https://azure.microsoft.com/pricing/free-trial/) offer if you do not have a subscription yet.
2. On the Hub menu, select **New** -> **Data + Storage** -> **Storage account**.
3. Enter a name for your storage account.
   
   > [!NOTE]
   > Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only.
   > 
   > Your storage account name must be unique within Azure. The Azure portal will indicate if the storage account name you select is already in use.
   > 
   > 
4. Specify the deployment model to be used: **Resource Manager** or **Classic**. **Resource Manager** is the recommended deployment model. For more information, see [Understanding Resource Manager deployment and classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).
5. Specify the performance tier for the storage account as **Premium**.
6. **Locally-redundant storage (LRS)** is the only available replication option with Premium Storage. For more details on Azure Storage replication options, see [Azure Storage replication](storage-redundancy.md).
7. Select the subscription in which you want to create the new storage account.
8. Specify a new resource group or select an existing resource group. For more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).
9. Select the geographic location for your storage account. You can confirm whether Premium Storage is available in the selected Location by referring to [Azure Services by Region](https://azure.microsoft.com/regions/#services).
10. Click **Create** to create the storage account.

#### Create an Azure virtual machine via Azure portal

You must create a Premium Storage supported VM to be able to use Premium Storage. Follow the steps in [Create a Windows virtual machine in the Azure portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) to create a new DS, DSv2, GS, or Fs virtual machine.

#### Attach a premium storage data disk via Azure portal

1. Find the new or existing DS, DSv2, GS, or Fs VM in Azure portal.
2. In the VM **All Settings**, go to **Disks** and click on **Attach New**.
3. Enter the name of your data disk and select the **Type** as **Premium**. Select the desired **Size** and **Host caching** setting.
   
    ![Premium Disk](./media/storage-premium-storage/Azure_attach_premium_disk.png)

	See more detailed steps in [How to attach a data disk in Azure portal](../virtual-machines/virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

#### Change disk caching policy via Azure portal

1. Find the new or existing DS, DSv2, GS, or Fs VM in Azure portal.
2. In the VM All Settings, go to Disks and click on the disk you wish to change.
3. Change the Host caching option to the desired value, None or ReadOnly or ReadWrite

> [!WARNING]
> Changing the cache setting of an Azure disk detaches and re-attaches the target disk. If it is the operating system disk, the VM is restarted. Stop all applications/services that might be affected by this disruption before changing the disk cache setting.
> 
> 

### Create an Azure virtual machine using Premium Storage via Azure PowerShell

To create a virtual machine in Premium Storage, you first have to create a Premium Storage account.

#### Create a Premium Storage account in Azure PowerShell

This PowerShell example shows how to create a new Premium Storage account and attach a data disk that uses that account to a new Azure virtual machine.

Setup your PowerShell environment by following the steps given at [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).

Start the PowerShell console, connect to your subscription, and run the following PowerShell cmdlet in the console window. As seen in this PowerShell statement, you need to specify the **Type** parameter as **Premium_LRS** when you create a Premium Storage account.
 
```powershell  
New-AzureStorageAccount -StorageAccountName "yourpremiumaccount" -Location "West US" -Type "Premium_LRS"
```

#### Create an Azure virtual machine via Azure PowerShell

Next, create a new DS-Series VM and specify that you want Premium Storage by running the following PowerShell cmdlets in the console window. You can create a GS-series VM using the same steps. Specify the appropriate VM size in the commands. For e.g. Standard_GS2:

```powershell
$storageAccount = "yourpremiumaccount"
$adminName = "youradmin"
$adminPassword = "yourpassword"
$vmName ="yourVM"
$location = "West US"
$imageName = "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201409.01-en.us-127GB.vhd"
$vmSize ="Standard_DS2"
$OSDiskPath = "https://" + $storageAccount + ".blob.core.windows.net/vhds/" + $vmName + "_OS_PIO.vhd"
$vm = New-AzureVMConfig -Name $vmName -ImageName $imageName -InstanceSize $vmSize -MediaLocation $OSDiskPath
Add-AzureProvisioningConfig -Windows -VM $vm -AdminUsername $adminName -Password $adminPassword
New-AzureVM -ServiceName $vmName -VMs $VM -Location $location
```

#### Attach a premium storage data disk via Azure PowerShell
If you want more disk space for your VM, attach a new data disk to an existing Premium Storage supported VM after it is created by running the following PowerShell cmdlets in the console window:

```powershell
$storageAccount = "yourpremiumaccount"
$vmName ="yourVM"
$vm = Get-AzureVM -ServiceName $vmName -Name $vmName
$LunNo = 1
$path = "http://" + $storageAccount + ".blob.core.windows.net/vhds/" + "myDataDisk_" + $LunNo + "_PIO.vhd"
$label = "Disk " + $LunNo
Add-AzureDataDisk -CreateNew -MediaLocation $path -DiskSizeInGB 128 -DiskLabel $label -LUN $LunNo -HostCaching ReadOnly -VM $vm | Update-AzureVm
```

#### Change disk caching policy via Azure PowerShell
To update the disk caching policy, note the LUN number of the data disk attached. Run the following command to update data disk attached at LUN number 2, to ReadOnly.

```powershell
Get-AzureVM "myservice" -name "MyVM" | Set-AzureDataDisk -LUN 2 -HostCaching ReadOnly | Update-AzureVM
```

> [!WARNING]
> Changing the cache setting of an Azure disk detaches and re-attaches the target disk. If it is the operating system disk, the VM is restarted. Stop all applications/services that might be affected by this disruption before changing the disk cache setting.
> 
> 

### Create an Azure virtual machine using Premium Storage via the Azure Command-Line Interface

The [Azure Command-Line Interface](../xplat-cli-install.md)(Azure CLI) provides a provides a set of open source, cross-platform commands for working with the Azure Platform. The following examples show how to use Azure CLI (version 0.8.14 and later) to create a Premium Storage account, a new virtual machine, and attach a new data disk from a Premium Storage account.

#### Create a Premium Storage account via Azure CLI

```
azure storage account create "premiumtestaccount" -l "west us" --type PLRS
```

#### Create a DS-series virtual machine via Azure CLI

```
azure vm create -z "Standard_DS2" -l "west us" -e 22 "premium-test-vm"
    "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB" -u "myusername" -p "passwd@123"
```

Display information about the virtual machine

```
azure vm show premium-test-vm
```

#### Attach a new premium data disk via Azure CLI

```
azure vm disk attach-new premium-test-vm 20 https://premiumstorageaccount.blob.core.windows.net/vhd-store/data1.vhd
```

Display information about the new data disk

```
azure vm disk show premium-test-vm-premium-test-vm-0-201502210429470316
```

#### Change disk caching policy

To change the cache policy on one of your disks using Azure CLI, run the following command:

```
$ azure vm disk attach -h ReadOnly <VM-Name> <Disk-Name>
```

Note that the caching policy options can be ReadOnly, None, or ReadWrite. For more options, see the help by running the following command:

```
azure vm disk attach --help
```

> [!WARNING]
> Changing the cache setting of an Azure disk detaches and re-attaches the target disk. If it is the operating system disk, the VM is restarted. Stop all applications/services that might be affected by this disruption before changing the disk cache setting.
> 
> 

## FAQs
1. **Can I attach both premium and standard data disks to a Premium Storage supported VM?**
   
    Yes. You can attach both premium and standard data disks to a Premium Storage supported series VM.

2. **Can I attach both premium and standard data disks to a D, Dv2, G or F series VM?**
   
    No. You can only attach a standard data disk to all VMs that are not Premium Storage supported series.

3. **If I create a premium data disk from an existing VHD that was 80 GB in size, how much will that cost me?**
   
    A premium data disk created from 80 GB VHD will be treated as the next available premium disk size, a P10 disk. You will be charged as per the P10 disk pricing.

4. **Are there any transaction costs when using Premium Storage?**
   
    There is a fixed cost for each disk size which comes provisioned with certain number of IOPS and Throughput. The only other costs are outbound bandwidth and snapshots capacity, if applicable. See [Azure Storage Pricing](https://azure.microsoft.com/pricing/details/storage/) for more details.

5. **Where can I store boot diagnostics for my Premium Storage supported series VM?**
   
    Create a standard storage account to store the boot diagnostics of your Premium Storage supported series VM.

6. **How many IOPS and Throughput can I get from the disk cache?**
   
    The combined limits for cache and local SSD for a DS series are 4000 IOPS per core and 33 MB per second per core. GS series offers 5000 IOPS per core and 50 MB per second per core.

7. **What is the local SSD in a Premium Storage supported series VM?**
   
    The local SSD is a temporary storage that is included with a Premium Storage supported series VM. There is no extra cost for this temporary storage. It is recommended that you do not use this temporary storage or local SSD for storing your application data as it is not persisted in Azure Blob Storage.

8. **Can I convert my standard storage account to a Premium Storage account?**
   
    No. It is not possible to convert standard storage account to Premium Storage account or vice versa. You must create a new storage account with the desired type and copy data to new storage account, if applicable.

9. **How can I convert my D series VM to a DS series VM?**
   
    Please refer to the migration guide, [Migrating to Azure Premium Storage](storage-migration-to-premium-storage.md) to move your workload from a D series VM using standard storage account to a DS series VM using Premium Storage account.

## Next steps
For more information about Azure Premium Storage refer to the following articles.

### Design and implement with Azure Premium Storage
* [Design for Performance with Premium Storage](storage-premium-storage-performance.md)
* [Using Blob Service Operations with Azure Premium Storage](http://go.microsoft.com/fwlink/?LinkId=521969)

### Operational guidance
* [Migrating to Azure Premium Storage](storage-migration-to-premium-storage.md)

### Blog Posts
* [Azure Premium Storage Generally Available](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/)
* [Announcing the GS-Series: Adding Premium Storage Support to the Largest VMs in the Public Cloud](https://azure.microsoft.com/blog/azure-has-the-most-powerful-vms-in-the-public-cloud/)
