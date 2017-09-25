---
title: Introduction to FreeBSD on Azure | Microsoft Docs
description: Learn about using FreeBSD virtual machines on Azure
services: virtual-machines-linux
documentationcenter: ''
author: KylieLiang
manager: timlt
editor: ''
tags: azure-service-management

ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/09/2017
ms.author: kyliel

---
# Introduction to FreeBSD on Azure
This topic provides an overview of running a FreeBSD virtual machine in Azure.

## Overview
FreeBSD for Microsoft Azure is an advanced computer operating system used to power modern servers, desktops, and embedded platforms.

Microsoft Corporation is making images of FreeBSD available on Azure with the [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) pre-configured. Currently, the following FreeBSD versions are offered as images by Microsoft:

- FreeBSD 10.3-RELEASE
- FreeBSD 11.0-RELEASE

The agent is responsible for communication between the FreeBSD VM and the Azure fabric for operations such as provisioning the VM on first use (user name, password or SSH key, host name, etc.) and enabling functionality for selective VM extensions.

As for future versions of FreeBSD, the strategy is to stay current and make the latest releases available shortly after they are published by the FreeBSD release engineering team.

## Deploying a FreeBSD virtual machine
Deploying a FreeBSD virtual machine is a straightforward process using an image from the Azure Marketplace:

- [FreeBSD 10.3 on the Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [FreeBSD 11.0 on the Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

## VM extensions for FreeBSD
Following are supported VM extensions in FreeBSD.

### VMAccess
The [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extension can:

* Reset the password of the original sudo user.
* Create a new sudo user with the password specified.
* Set the public host key with the key given.
* Reset the public host key provided during VM provisioning if the host key is not provided.
* Open the SSH port (22) and restore the sshd_config if reset_ssh is set to true.
* Remove the existing user.
* Check disks.
* Repair an added disk.

### CustomScript
The [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extension can:

* If provided, download the customized scripts from Azure Storage or external public storage (for example, GitHub).
* Run the entry point script.
* Support inline commands.
* Convert Windows-style newline in shell and Python scripts automatically.
* Remove BOM in shell and Python scripts automatically.
* Protect sensitive data in CommandToExecute.

[!NOTE]FreeBSD VM only supports CustomScript version 1.x by now.  

## Authentication: user names, passwords, and SSH keys
When you're creating a FreeBSD virtual machine by using the Azure portal, you must provide a user name, password, or SSH public key.
User names for deploying a FreeBSD virtual machine on Azure must not match names of system accounts (UID <100) already present in the virtual machine ("root", for example).
Currently, only the RSA SSH key is supported. A multiline SSH key must begin with `---- BEGIN SSH2 PUBLIC KEY ----` and end with `---- END SSH2 PUBLIC KEY ----`.

## Obtaining superuser privileges
The user account that is specified during virtual machine instance deployment on Azure is a privileged account. The package of sudo was installed in the published FreeBSD image.
After you're logged in through this user account, you can run commands as root by using the command syntax.

    $ sudo <COMMAND>

You can optionally obtain a root shell by using `sudo -s`.

## Known issues
1. There is currently an outstanding issue with FreeBSD 11.0 on Hyper-V (and Azure) that may cause VMs to fail to boot if the operating system is patched using `freebsd-update`. The [proposed patch](https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=212721) is included in the FreeBSD images on the Azure Marketplace. However, it has not been merged upstream by the FreeBSD team, so running `freebsd-update` replaces the kernel with an unpatched one. It is recommended that users on Azure do not install patches for FreeBSD 11.0 until the fix is published as an ERRATA.

2. The [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 has a [known issue] (https://github.com/Azure/WALinuxAgent/pull/517) that causes the provision failure for FreeBSD VM on Azure. It is recommended that FreeBSD VM users on Azure use 2.2.1 or earlier release. The fix will be captured by [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.3. 

## Next steps
* Go to [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) to create a FreeBSD VM.
* If you want to bring your own FreeBSD to Azure, refer to [Create and upload a FreeBSD VHD to Azure](./virtual-machines-linux-classic-freebsd-create-upload-vhd.md).
