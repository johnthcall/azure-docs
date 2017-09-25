---
title: 'Installing Mobility Service (VMware/Physical to Azure) | Microsoft Docs'
description: This article describes how to install the Mobility Service Agent on your on-premises machines to start protecting them.
services: site-recovery
documentationcenter: ''
author: AnoopVasudavan
manager: gauravd
editor: ''

ms.assetid:
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 12/9/2016
ms.author: anoopkv
---

# Installing Mobility Service (VMware/Physical to Azure)
Mobility Service needs to be  deployed on every machine (VMware VM or physical server) that you want to replicate to Azure. It captures data writes on the machine, and forwards them to the process server.  Mobility Service can be deployed onto the servers that require protection in the following methods
1. [Install Mobility Service using Software deployment tools like System Center Configuration Manager](site-recovery-install-mobility-service-using-sccm.md)
2. [Install Mobility Service using Azure Automation and Desired State Configuration(DSC)](site-recovery-automate-mobility-service-install.md)
3. [Install Mobility Service manually using the Graphical User Interface(GUI)](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-using-the-graphical-user-interface)
4. [Install Mobility Service manually using Command-line](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-using-command-line)
5. [Install Mobility Service using Push Install from Azure Site Recovery](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-using-push-install-from-azure-site-recovery)

## Prerequisites
Perform these prerequisites before you start manually installing the Mobility Service on your servers.
1. Login on to your Configuration Server and open up a command prompt in with Administrative privileges.
2. Change directory to the bin folder and create a passphrase file

  ```
  cd %ProgramData%\ASR\home\svsystems\bin
  genpassphrase.exe -v > MobSvc.passphrase
  ```
3. Store this file in a secure location as we will need to use it during the Mobility Service installation.
4. The Mobility Service installers for all the supported operating systems can be found under the directory     

  `%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository`

#### Mobility Service installer to Operating System mapping

| Installer file template name| Operating System |
|---|--|
|Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2008 R2 (64 bit) SP1</br> Windows Server 2012 (64 bit) </br> Windows Server 2012 R2 (64 bit) |
|Microsoft-ASR\_UA\*RHEL6-64*release.tar.gz| RHEL 6.4, 6.5, 6.6 (64 bit only) </br> CentOS 6.4, 6.5, 6.6 (64 bit only) |
|Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 (64 bit only)|
|Microsoft-ASR_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5 (64 bit only)|
|Microsoft-ASR_UA\*Ubuntu-14.04-64\*release.tar.gz | Ubuntu 14.04 (64 bit only)|

## Install Mobility Service manually using the Graphical User Interface

>[!NOTE]
> The Graphical User Interface based install is supported only for Microsoft Windows Operating Systems.

[!INCLUDE [site-recovery-install-mob-svc-gui](../../includes/site-recovery-install-mob-svc-gui.md)]

## Install Mobility Service manually using Command-line
### Command-line based install on Windows Computers
[!INCLUDE [site-recovery-install-mob-svc-win-cmd](../../includes/site-recovery-install-mob-svc-win-cmd.md)]

### Command-line based install on Linux Computers
[!INCLUDE [site-recovery-install-mob-svc-lin-cmd](../../includes/site-recovery-install-mob-svc-lin-cmd.md)]


## Install Mobility Service using Push Install from Azure Site Recovery
To be able to perform push installation of Mobility Service using Azure Site Recovery, you need to ensure the following pre-requisites are met on all target computers.

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-win](../../includes/site-recovery-prepare-push-install-mob-svc-win.md)]

[!INCLUDE [site-recovery-prepare-push-install-mob-svc-lin](../../includes/site-recovery-prepare-push-install-mob-svc-lin.md)]


## Next steps
Once the Mobility Service is installed you can use the **+Replicate** button in Azure Portal to start enabling protection for these VMs.
