---
title: 'Azure Backup: Restore virtual machines using the Azure portal | Microsoft Docs'
description: Restore an Azure virtual machine from a recovery point using Azure portal
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: restore backup; how to restore; recovery point;

ms.assetid: 372b87c6-3544-4dc5-bbc9-c742ca502159
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/26/2017
ms.author: trinadhk;jimpark;

---
# Use Azure portal to restore virtual machines
> [!div class="op_single_selector"]
> * [Restore VMs in Classic portal](backup-azure-restore-vms.md)
> * [Restore VMs in Azure portal](backup-azure-arm-restore-vms.md)
>
>

Protect your data by taking snapshots of your data at defined intervals. These snapshots are known as recovery points, and they are stored in recovery services vaults. If or when it is necessary to repair or rebuild a VM, you can restore the VM from any of the saved recovery points. When you restore a recovery point, you return or revert the VM to the state when the recovery point was taken. This article explains how to restore a VM.

> [!NOTE]
> Azure has two deployment models for creating and working with resources: [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md). This article provides the information and procedures for restoring VMs deployed using the Resource Manager model.
>
>

## Restore a recovery point
1. Sign in to the [Azure portal](http://portal.azure.com/)
2. On the Azure menu, click **Browse** and in the list of services, type **Recovery Services**. The list of services adjusts to what you type. When you see **Recovery Services vaults**, select it.

    ![Open Recovery Services vault](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    The list of vaults in the subscription is displayed.

    ![List of Recovery Services vaults](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
3. From the list, select the vault associated with the VM you want to restore. When you click the vault, its dashboard opens.

    ![List of Recovery Services vaults](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade.png)
4. Now that you're in the vault dashboard. On the **Backup Items** tile, click **Azure Virtual Machines** to display the VMs associated with the vault.

    ![vault dashboard](./media/backup-azure-arm-restore-vms/vault-dashboard.png)

    The **Backup Items** blade opens and displays the list of Azure virtual machines.

    ![list of VMs in vault](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault.png)
5. From the list, select a VM to open the dashboard. The VM dashboard opens to the Monitoring area, which contains the Restore points tile.

    ![list of VMs in vault](./media/backup-azure-arm-restore-vms/vm-blade.png)
6. On the VM dashboard menu, click **Restore**

    ![list of VMs in vault](./media/backup-azure-arm-restore-vms/vm-blade-menu-restore.png)

    The Restore blade opens.

    ![restore blade](./media/backup-azure-arm-restore-vms/restore-blade.png)
7. On the **Restore** blade, click **Restore point** to open the **Select Restore point** blade.

    ![restore blade](./media/backup-azure-arm-restore-vms/recovery-point-selector.png)

    By default, the dialog displays all restore points from the last 30 days. Use the **Filter** to alter the time range of the restore points displayed. By default, restore points of all consistency are displayed. Modify **All Restore points** filter to select a specific consistency of restore points. For more information about each type of restoration point, see the explanation of [Data consistency](backup-azure-vms-introduction.md#data-consistency).  

   * **Restore point consistency** from this list choose:
     * Crash consistent restore points,
     * Application consistent restore points,
     * File system consistent restore points
     * All restore points.  
8. Choose a Restore point and click **OK**.

    ![choose restore point](./media/backup-azure-arm-restore-vms/select-recovery-point.png)

    The **Restore** blade shows the Restore point is set.

    ![restore point is set](./media/backup-azure-arm-restore-vms/recovery-point-set.png)
9. On the **Restore** blade, **Restore configuration** opens automatically after restore point is set.

    ![restore configuration wizard is set](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)

## Choosing a VM restore configuration
Now that you have selected the restore point, choose a configuration for your restore VM. Your choices for configuring the restored VM are to use: Azure portal or PowerShell.

> [!NOTE]
> Portal provides a Quick Create option for restored VM. If you want to customize the VM configuration of the to-be restored VM, use PowerShell or portal to restore backed up disks and use PowerShell commands to attach them to choice of VM configuration. See [Restoring a VM with special network configurations](#restoring-vms-with-special-network-configurations).
>
>

1. If you are not already there, go to the **Restore** blade. Ensure a **Restore point** has been selected, and click **Restore configuration** to open the **Recovery configuration** blade.

    ![recovery configuration wizard is set](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard.png)
2. On the **Restore configuration** blade, you have two choices:

   * Restore full virtual machine
   * Restore backed up disks

   ### Restore full virtual machine
   On the **Restore configuration** blade, enter or select values for each of the following fields:

   * **Restore Type** - Create virtual machine.
   * **Virtual machine name** - Provide a name for the VM. The name must be unique to the resource group (for a Resource Manager-deployed VM) or cloud service (for a Classic VM). You cannot replace the virtual machine if it already exists in the subscription.
   * **Resource group** - Use an existing resource group, or create a new one. If you are restoring a Classic VM, use this field to specify the name of a new cloud service. If you are creating a new resource group/cloud service, the name must be globally unique. Typically, the cloud service name is associated with a public-facing URL - for example: [cloudservice].cloudapp.net. If you attempt to use a name for the cloud resource group/cloud service that has already been used, Azure assigns the resource group/cloud service the same name as the VM. Azure displays resource groups/cloud services and VMs not associated with any affinity groups. For more information, see [How to migrate from Affinity Groups to a Regional Virtual Network (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
   * **Virtual Network** - Select the virtual network (VNET) when creating the VM. The field provides all VNETs associated with the subscription. Resource group of the VM is displayed in parentheses.
   * **Subnet** - If the VNET has subnets, the first subnet is selected by default. If there are additional subnets, select the desired subnet.
   * **Storage account** - This menu lists the storage accounts in the same location as the Recovery Services vault. When choosing a storage account, select an account that shares the same location as the Recovery Services vault. Storage accounts that are Zone redundant are not supported. If there are no storage accounts with the same location as the Recovery Services vault, you must create one before starting the restore operation. The storage account's replication type is mentioned in parentheses.

     > [!NOTE]
     > If you are restoring a Resource Manager-deployed VM, you must identify a virtual network (VNET). A virtual network (VNET) is optional for a Classic VM.
     >
     >

   ### Restore backed up disks
   If you would like to customize the virtual machine you would like to create from backed up disks than what is present in restore configuration blade, select **Restore disks** as value for **Restore Type**. This choice asks for a storage account(same restrictions apply as Restore full virtual machine storage account selection) where disks from backups are copied to. You can [use the restored disks to attach to an existing virtual machine](../virtual-machines/virtual-machines-windows-attach-disk-portal.md) or [create a new virtual machine from restored disks.](./backup-azure-vms-automation.md#restore-an-azure-vm)

3. On the **Restore configuration** blade, click **OK** to finalize the restore configuration.
4. On the **Restore** blade, click **Restore** to trigger the restore operation.

    ![Recovery configuration completed](./media/backup-azure-arm-restore-vms/trigger-restore-operation.png)

## Track the restore operation
Once you trigger the restore operation, the Backup service creates a job for tracking the restore operation. The Backup service also creates and temporarily displays the notification in Notifications area of portal. If you do not see the notification, you can always click the Notifications icon to view your notifications.

![Restore triggered](./media/backup-azure-arm-restore-vms/restore-notification.png)

To view the operation while it is processing, or to view when it completed, open the Backup jobs list.

1. On the Azure menu, click **Browse** and in the list of services, type **Recovery Services**. The list of services adjusts to what you type. When you see **Recovery Services vaults**, select it.

    ![Open Recovery Services vault](./media/backup-azure-arm-restore-vms/open-recovery-services-vault.png)

    The list of vaults in the subscription is displayed.

    ![List of Recovery Services vaults](./media/backup-azure-arm-restore-vms/list-of-rs-vaults.png)
2. From the list, select the vault associated with the VM you restored. When you click the vault, its dashboard opens.
3. In the vault dashboard on the **Backup Jobs** tile, click **Azure Virtual Machines** to display the jobs associated with the vault.

    ![vault dashboard](./media/backup-azure-arm-restore-vms/vault-dashboard-jobs.png)

    The **Backup Jobs** blade opens and displays the list of jobs.

    ![list of VMs in vault](./media/backup-azure-arm-restore-vms/restore-job-in-progress.png)

## Post-Restore steps
If you are using a cloud-init based Linux distribution such as Ubuntu, for security reasons, password is blocked post restore. Please use VMAccess extension on the restored VM to [reset the password](../virtual-machines/virtual-machines-linux-classic-reset-access.md). We recommend using SSH keys on these distributions to avoid resetting password post restore.

## Backup for restored VMs
If you have restored VM to same Resource Group with the same name as originally backed up VM, backup continues on the VM post restore. If you have either restored VM to a different Resource group or specified a different name for restored VM, this is treated as a new VM and you need to setup backup for restored VM.

## Restoring a VM during Azure dataCenter disaster
Azure Backup allows restoring backed up VMs to the paired data center in case the primary data center where VMs are running experiences disaster and you configured Backup vault to be geo-redundant. During such scenarios, you need to select a storage account, which is present in paired data center and rest of the restore process remains same. Azure Backup uses Compute service from paired geo to create the restored virtual machine.

## Restoring VMs with special network configurations
It is possible to back up and restore VMs with the following special network configurations. However, these configurations require some special consideration while going through the restore process.

* VMs under load balancer (internal and external)
* VMs with multiple reserved IPs
* VMs with multiple NICs

> [!IMPORTANT]
> When creating the special network configuration for VMs, you must use PowerShell to create VMs from the disks restored.
>
>

To fully recreate the virtual machines after restoring to disk, follow these steps:

1. Restore the disks from a recovery services vault using [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm)
2. Create the VM configuration required for load balancer/multiple NIC/multiple reserved IP using the PowerShell cmdlets and use it to create the VM of desired configuration.

   * Create VM in cloud service with [Internal Load balancer ](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Create VM to connect to [Internet facing load balancer](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Create VM with [multiple NICs](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Create VM with [multiple reserved IPs](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## Next steps
Now that you can restore your VMs, see the troubleshooting article for information on common errors with VMs. Also, check out the article on managing tasks with your VMs.

* [Troubleshooting errors](backup-azure-vms-troubleshoot.md#restore)
* [Manage virtual machines](backup-azure-manage-vms.md)
