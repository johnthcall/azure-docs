---
title: 'Reset an Azure VPN gateway to reestablish IPsec tunnels: PowerShell | Microsoft Docs'
description: This article walks you through resetting your Azure VPN Gateway to reestablish IPsec tunnels. The article applies to VPN gateways in both the classic, and the Resource Manager deployment models.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: azure-resource-manager,azure-service-management

ms.assetid: 79d77cb8-d175-4273-93ac-712d7d45b1fe
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/25/2017
ms.author: cherylmc

---
# Reset an Azure VPN Gateway using PowerShell

Resetting the Azure VPN gateway is helpful if you lose cross-premises VPN connectivity on one or more S2S VPN tunnels. In this situation, your on-premises VPN devices are all working correctly, but are not able to establish IPsec tunnels with the Azure VPN gateways.

This article walks you through resetting your Azure VPN Gateway using PowerShell cmdlets. These instructions include both the classic deployment model and the Resource Manager deployment model.

Each Azure VPN gateway is a virtual network gateway that is composed of two VM instances running in an active-standby configuration. When you use the PowerShell cmdlet to reset the gateway, it reboots the gateway, and then reapplies the cross-premises configurations to it. The gateway keeps the public IP address it already has. This means you won’t need to update the VPN router configuration with a new public IP address for Azure VPN gateway.  

Once the command is issued, the current active instance of the Azure VPN gateway is rebooted immediately. There will be a brief gap during the failover from the active instance (being rebooted), to the standby instance. The gap should be less than one minute.

If the connection is not restored after the first reboot, issue the same command again to reboot the second VM instance (the new active gateway). If the two reboots are requested back to back, there will be a slightly longer period where both VM instances (active and standby) are being rebooted. This will cause a longer gap on the VPN connectivity, up to 2 to 4 minutes for VMs to complete the reboots.

After two reboots, if you are still experiencing cross-premises connectivity problems, please open a support request from the Azure portal.

## Before you begin
Before you reset your gateway, verify the key items listed below for each IPsec Site-to-Site (S2S) VPN tunnel. Any mismatch in the items will result in the disconnect of S2S VPN tunnels. Verifying and correcting the configurations for your on-premises and Azure VPN gateways saves you from unnecessary reboots and disruptions for the other working connections on the gateways.

Verify the following items before resetting your gateway:

* The Internet IP addresses (VIPs) for both the Azure VPN gateway and the on-premises VPN gateway are configured correctly in both the Azure and the on-premises VPN policies.
* The pre-shared key must be the same on both Azure and on-premises VPN gateways.
* If you apply specific IPsec/IKE configuration, such as encryption, hashing algorithms, and PFS (Perfect Forward Secrecy), ensure both the Azure and on-premises VPN gateways have the same configurations.

## Reset - Resource Manager

You'll need the latest version of the PowerShell cmdlets. See [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for more information. The PowerShell Resource Manager cmdlet for resetting gateway is `Reset-AzureRmVirtualNetworkGateway`. The following example resets the Azure VPN gateway, "VNet1GW", in resource group "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## Reset - Classic

You'll need the latest version of the PowerShell cmdlets. See [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for more information. The PowerShell cmdlet for resetting Azure VPN gateway is `Reset-AzureVNetGateway`. The following example resets the Azure VPN gateway for the virtual network called "ContosoVNet".

    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Result:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## Next steps
See the [PowerShell Service Management cmdlet reference](https://msdn.microsoft.com/library/azure/mt617104.aspx) and the [PowerShell Resource Manager cmdlet reference](http://go.microsoft.com/fwlink/?LinkId=828732) for more information.

