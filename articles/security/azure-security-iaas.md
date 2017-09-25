---
  title: Security best practices for IaaS workloads in Azure | Microsoft Docs
  description: " The migration of workloads to Azure IaaS brings about opportunities to re-evaluate our designs "
  services: security
  documentationcenter: na
  author: barclayn
  manager: MBaldwin
  editor: TomSh

  ms.assetid: 02c5b7d2-a77f-4e7f-9a1e-40247c57e7e2
  ms.service: security
  ms.devlang: na
  ms.topic: article
  ms.tgt_pltfrm: na
  ms.workload: na
  ms.date: 01/09/2017
  ms.author: barclayn



---

# Security best practices for IaaS workloads in Azure

As you started thinking about moving workloads to Azure IaaS you probably came to the realization that some considerations are familiar. You may already have experience securing virtual environments. The move to Azure IaaS allow you to apply your expertise in securing virtual environments and also brings a new set of options to help you secure your assets.

Before we get going let's start by saying that we should not expect to bring on-premises resources as one-to-one to Azure. The new challenges and the new options bring about an opportunity to reevaluate existing deigns, tools, and processes.




![Areas of Responsibility](./media/azure-security-iaas/sec-cloudstack-new.png)

>[!NOTE]
> Your responsibility for security is based on the type of cloud service. The chart above summarizes the balance of responsibility for both Microsoft and you.

## Best practices
We will be discussing some of the options available in Azure that could help you meet your organization’s security requirements. While doing this we must keep in mind the different types of workloads and how their security requirements may vary. Not one of these best practices can by itself secure your systems. Like anything else in security, you have to choose the appropriate options and see how the solutions can complement each other by filling gaps left by the others.

### Use Privileged Access Workstations (PAW)

Organizations often fall prey to cyber-attacks because of administrators performing actions while using accounts with elevated rights. Usually this isn’t done maliciously but because existing configuration and processes allow them to do it. Most of these users understand the risk from a conceptual standpoint but still choose to take steps that they would agree are risky.

Doing things like checking email and browsing the Internet seem innocent enough but may expose elevated accounts to compromise by malicious actors who may use browsing activities, specially crafted emails, or other techniques to gain access to your enterprise. The use of secure management workstations for conducting all Azure administration tasks is highly recommended as a way of reducing exposure to accidental compromise.

Privileged Access Workstations (PAWs) provide a dedicated operating system for sensitive tasks that is protected from Internet attacks and threat vectors. Separating these sensitive tasks and accounts from the daily use workstations and devices provides very strong protection from phishing attacks, application and OS vulnerabilities, various impersonation attacks, and credential theft attacks such as keystroke logging, Pass-the-Hash, and Pass-The-Ticket.

The PAW approach is an extension of the well-established recommended practice to use separate admin and user accounts for administrative personnel. This practice uses an individually assigned administrative account that is separate from the user's standard user account. PAW builds on that account separation practice by providing a trustworthy workstation for those sensitive accounts.

For more information on Privileged Access Workstations and guidance for PAW implementation, follow this link:

- [Privileged Access Workstations](https://technet.microsoft.com/windows-server-docs/security/securing-privileged-access/privileged-access-workstations)

### Use multifactor authentication

In the past, your network perimeter was used to control access to corporate data. In a cloud-first, mobile-first world, identity is the control plane: You use it to control access to IaaS services from any device, and you use it to get visibility and insight into where and how your data is being used. Protecting the digital identity of your Azure users is the cornerstone of protecting your subscriptions from identity theft and other cybercrimes.

One of the most beneficial steps that you can take to secure an account is to enable two factor authentication. Two factor authentication is a way of authenticating by using more than just your password. The second factor is something in addition to the password.  This helps mitigate the risk of access by someone who manages to get a hold of someone else’s password.

Azure Multi-Factor Authentication helps safeguard access to data and applications while meeting user demand for a simple sign-in process. It delivers strong authentication with a range of easy verification options—phone call, text message, or mobile app notification—allowing users to choose the method they prefer.

The easiest way to use [Azure MFA](../multi-factor-authentication/multi-factor-authentication.md) is the Microsoft Authenticator mobile app that can be used on mobile devices running Windows, IOS and Android. With the latest release of Windows 10 and your on premises Active Directory integrated with Azure AD, [Microsoft Hello for Business](../active-directory/active-directory-azureadjoin-passport-deployment.md) can be used for seamless single sign-on to Azure resources. In this case, the Windows 10 device will be used as the second factor for authentication.


In the case of Azure, the simplest way to enable two factor authentication is to use Azure Multifactor Authentication (MFA). [Azure MFA](../multi-factor-authentication/multi-factor-authentication.md) has an application that can be used on your mobile devices, it can work via phone calls, text messages or via a code generated in the app and it can integrate with your on-premises directory.

For accounts that manage your Azure subscription you should use MFA and for accounts that can logon to the Virtual Machines you should use MFA when possible. Using MFA for these accounts gives you much greater level of security than just using a password. Using other forms of two factor authentication could work just as well but may be more involved to get deployed if they are not already in production.

The screenshot below shows some of the options available for Azure MFA authentication.

![MFA options](./media/azure-security-iaas/mfa-options.png)

### Limit and Constrain Administrative Access

Securing the accounts that can manage your Azure subscription is extremely important. The compromise of any of those accounts effectively negates the value of all the other steps you may take to ensure the confidentiality and integrity of your data. As recently illustrated by the [Edward Snowden](https://en.wikipedia.org/wiki/Edward_Snowden) leak of classified information, internal attacks pose a huge threat to the overall security of any organization.

Individuals who have administrative rights should have been evaluated by following a criteria similar to the one below:

- Are they performing tasks that require administrative privileges?
- How often are the tasks performed?
- Specific reason why the tasks cannot be performed by another administrator on their behalf.
- Document all other known alternative approaches to granting the privilege and why each isn't acceptable.

The use of just in time administration prevents the unnecessary existence of accounts with elevated rights during time periods when those rights are not needed. Accounts have elevated rights for a limited time enabling administrators to do their jobs and then they have those rights removed at the end of a shift or when a task is completed.

[PIM](../active-directory/active-directory-privileged-identity-management-configure.md) allows you to manage, monitor, and control access within your organization. It helps you remain aware of the actions taken by individuals within your organization and brings just-in-time administration to Azure AD by introducing the concept of an eligible admin. These are individuals who have accounts with the potential to be granted admin rights. These types of users can go through an activation process and be granted admin rights for a limited time.


### Use DevTest Labs

Using Azure for labs and development environments allows organizations to gain agility in testing and development by taking away the delays introduced by hardware procurement. Unfortunately, there is the risk that a lack of familiarity with Azure or a desire to help expedite its adoption may lead the administrator to be overly permissive with rights assignment. This may be unintentionally exposing the organization to internal attacks. Some users could be granted a lot more access than they should have.

In Azure we now include a service called [DevTest Labs](../devtest-lab/devtest-lab-overview.md). DevTest Labs uses [Azure Role based access control(RBAC)](../active-directory/role-based-access-control-what-is.md)(RBAC). RBAC allows you to segregate duties within your team into roles that grant only the level of access necessary for users to do their jobs. It comes with pre-defined roles (Owner, lab user and contributor). These roles can even be used to assign rights to external partners and greatly simplify collaboration.

Since DevTest Labs uses RBAC, it is possible to create additional [custom roles](../devtest-lab/devtest-lab-grant-user-permissions-to-specific-lab-policies.md). DevTest Labs not only simplifies the management of permissions, it is also designed to simplify the process of getting environments provisioned and to deal with other typical challenges of teams working on development and test environments. It requires some preparation but in the long term it will make things easier for your team.

Some key Azure DevTest Labs features include:

- Administrative control over the options available to users. Things like allowed VM sizes, maximum number of VMs and when VMs are started and shut down can be centrally managed by the administrator
- Automation of lab environment creation
- Cost tracking
- Simplified distribution of VMs for temporary collaborative work
- Self-service allowing users to provision their labs using templates
- Managing and limiting consumption

![Creating a DevTestLab](./media/azure-security-iaas/devtestlabs.png)

There is no additional cost associated with the usage of DevTest Labs. The creation of labs, policy configuration, templates, and artifacts are all free. You only pay for the azure resources used within your labs such as virtual machines, storage accounts and virtual networks.



### Control and Limit Endpoint Access

Hosting labs or production systems in Azure means that your systems need to be accessible from the Internet. By default, a new windows virtual machine has the RDP port accessible from the Internet and a Linux virtual machine has the SSH port open. This means that taking steps to 'limit exposed endpoints' is necessary to minimize the risk of unauthorized access.

There are technologies in Azure that can help you limit the access to those administrative endpoints. In Azure you can use Network Security Groups ([NSGs](../virtual-network/virtual-networks-nsg.md)). When you use Resource Manager for deployment NSGs are used to limit the access from all networks to just the management endpoints (RDP or SSH).  When you think NSGs, think router ACLs. You can use them to tightly control the network communication between various segments of your Azure networks. This is similar to creating networks in DMZs or other isolated networks. They do not inspect the traffic but they will help with network segmentation.


In Azure you can configure a [site-to-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) from your on-premises network, effectively extending your on-premises network to the cloud.  This would provide you with another opportunity to use NSGs, as you could also modify the NSGs to not allow access from anywhere other than the local network and then require that administration is done by first connecting to the Azure network via VPN.

The site-to-site VPN option may be most attractive in cases where you are hosting production systems that are closely integrated with your on-premises resources in Azure.

Alternatively, the [point to site](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md) option could be used in situations where you want to manage systems that don't need access to on-premises resources. Those systems can be isolated in their own Azure Virtual Network and administrators could VPN into the Azure hosted environment from their administrative workstation.

>[!NOTE]
Either VPN option would allow you to reconfigure the ACLs on the NSGs to not allow access to management endpoints from the Internet.

Another option worth considering is a [Remote Desktop Gateway](../multi-factor-authentication/multi-factor-authentication-get-started-server-rdg.md) deployment. You could use this remote desktop gateway deployment to securely connect over HTTPS to remote desktop servers while applying more granular controls to those connections.

Some of the features that you would have access to include:

- Administrator options to limit connections to requests from specific systems.
- Smartcard authentication or Azure MFA
- Control over which systems someone can connect to via the gateway.
- Control over device and disk redirection.

### Use a Key Management solution

Secure key management is essential to protecting data in the cloud. With [Azure Key Vault](../key-vault/key-vault-whatis.md), you can securely store encryption keys and small secrets like passwords in hardware security modules (HSMs). For added assurance, you can import or generate keys in HSMs.

If you choose to do this, Microsoft will process your keys in FIPS 140-2 Level 2 validated HSMs (hardware and firmware). Monitor and audit key use with Azure logging—pipe logs into Azure applying or your SIEM for additional analysis and threat detection.

Anyone with an Azure subscription can create and use key vaults. Although Key Vault benefits developers and security administrators, it could be implemented and managed by an organization’s administrator responsible for managing Azure services.


### Encrypt Virtual Disks and Disk Storage

[Azure Disk Encryption](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0) addresses the threat of data theft or exposure from unauthorized access achieved by moving a disk. The disk could be attached to another system as a way of bypassing other security controls. Disk encryption uses [BitLocker](https://technet.microsoft.com/library/hh831713 ) in windows and DM-Crypt in Linux to encrypt operating system and data drives. Azure Disk Encryption integrates with key vault to control and manage the encryption keys and it is available for standard VMs and VMs with premium storage.

For more information, look at the article covering [Azure Disk Encryption in Windows and Linux IaaS VMs](azure-security-disk-encryption.md).

[Azure Storage Service Encryption](../storage/storage-service-encryption.md) protects your data at rest. It is enabled at the storage account level and it encrypts data as its written in our datacenters and it is automatically decrypted as you access it. It supports the following scenarios:

- Encryption of block blobs, append blobs, and page blobs.
- Encryption of archived VHDs and templates brought to Azure from on-premises.
- Encryption of underlying OS and data disks for IaaS VMs created using your VHDs.

Before you proceed with Azure Storage encryption you should be aware of two notable limitations:

- It is not available on classic storage accounts.
- It only encrypts data written after encryption is enabled.

### Use a Centralized Security Management System

Your servers need to be monitored for patching, configuration, events, and activities that may be considered security concerns. To address those concerns you can use [Security Center](https://azure.microsoft.com/services/security-center/) and [Operations Management Suite Security and Compliance](https://azure.microsoft.com/services/security-center/). Both of these options go beyond the configuration within the operating system and also provide monitoring of the configuration of the underlying infrastructure like network configuration and virtual appliance use.

### Operating system management best practices

In an IaaS deployment you are still responsible for the management of the systems that you deploy just like any other server or workstation in your environment. This means that patching, hardening, rights assignments and any other activity related to the maintenance of your system is still your responsibility.  For systems that are tightly integrated with your on-premises resources you may want to use the same tools and procedures that you are using on-premises for things like anti-virus, anti-malware, patching, and backup.

**Hardening**
All virtual machines in Azure IaaS should be hardened so that they only expose services endpoints that are required for the applications that are installed. For Windows virtual machines, follow the recommendations that are published by Microsoft as baselines for the Security Compliance Manager solution.

[Security Compliance Manager](https://technet.microsoft.com/solutionaccelerators/cc835245.aspx) - we recently released version 4.0 - This is a free tool that enables you to quickly configure and manage your desktops, traditional datacenter, private and public cloud using Group Policy and System Center Configuration Manager.

SCM provides ready to deploy policies and DCM configuration packs that are tested. These baselines are based on [Microsoft Security guide](https://technet.microsoft.com/en-us/library/cc184906.aspx) recommendations and industry best practices, allowing you to manage configuration drift, address compliance requirements, and reduce security threats.

You can leverage SCM to import the current configuration of your computers using two different methods: first, you can import Active Directory-based group policies; second, you can import the configuration of a “golden master” reference machine by using the [LocalGPO tool](https://blogs.technet.microsoft.com/secguide/2016/01/21/lgpo-exe-local-group-policy-object-utility-v1-0/) to backup the local group policy which you can then import into SCM.

Compare your standards to industry best practices, customize them , and create new policies and DCM configuration packs.
Baselines have been published for all supported operating systems, including Windows 10 Anniversary Update and Windows Server 2016.


**Install and manage antimalware**

For environments that are hosted separately from your production environment there is an antimalware extension that can be used to protect your virtual machines and cloud services and it integrates with [Azure Security Center](../security-center/security-center-intro.md).


[Microsoft Antimalware](azure-security-antimalware.md) includes features like real-time protection, scheduled scanning, malware remediation, signature updates, engine updates, samples reporting, exclusion event collection, and [PowerShell support](https://msdn.microsoft.com/library/dn771715.aspx).

![Azure Antimalware](./media/azure-security-iaas/azantimalware.png)

**Install the latest security updates**
Some of the first workloads we see customers move to Azure are labs and external facing systems. If you are hosting virtual machines in Azure that host applications or services that need to be made accessible to the Internet, you need to be vigilant about patching. Remember that this goes beyond patching the operating system. Unpatched vulnerabilities on third-party applications can also lead to problems that would have been easily avoided if good patch management was in place.

For more information on managing patching in Azure IaaS look at [Best practices for software updates on Microsoft Azure IaaS](azure-security-best-practices-software-updates-iaas.md).

**Deploy and test a backup solution**

Just like security updates, backup needs to be handled the same way you handle any other operation. This is true of systems that are part of your production environment extending to the cloud. Test and Dev Systems must follow backup strategies that are able of providing similar restore capabilities to what users have grown accustomed to based on their experience with on-premise environments.

Production workloads moved to Azure should integrate with existing backup solutions when possible or you can use [Azure Backup](../backup/backup-azure-arm-vms.md) to help you address your backup requirements.


### Monitor

[Security Center](../security-center/security-center-intro.md) provides ongoing evaluation of the security state of your Azure resources to identify potential security vulnerabilities. A list of recommendations guides you through the process of configuring needed controls.

Examples include:

- Provisioning antimalware to help identify and remove malicious software
- Configuring network security groups and rules to control traffic to virtual machines
- Provisioning of web application firewalls to help defend against attacks that target your web applications
- Deploying missing system updates
- Addressing OS configurations that do not match the recommended baselines

In the image below you can see some of the options available for you to enable in Security Center.

![Azure security center policies](./media/azure-security-iaas/security-center-policies.png)

 [Operations Management Suite](../operations-management-suite/operations-management-suite-overview.md) is a Microsoft cloud based IT management solution that helps you manage and protect your on-premise and cloud infrastructure. Since OMS is implemented as a cloud based service it can be deployed quickly and with minimal investment in infrastructure resources.

New features are delivered automatically saving you from ongoing maintenance and upgrade costs. It also integrates with System Center Operations Manager.  OMS has different components to help you better manage your Azure workloads including a [Security and Compliance](../operations-management-suite/oms-security-getting-started.md) module.

The security and compliance features in OMS allow you to view information about your resources organized into four major categories:

- Security Domains: in this area you will be able to further explore security records over time, access malware assessment, update assessment, network security, identity and access information, computers with security events and quickly have access to Azure Security Center dashboard.
- Notable Issues: this option will allow you to quickly identify the number of active issues and the severity of these issues.
- Detections (Preview): enables you to identify attack patterns by visualizing security alerts as they take place against your resources.
- Threat Intelligence: enables you to identify attack patterns by visualizing the total number of servers with outbound malicious IP traffic, the malicious threat type, and a map that shows where these IPs are coming from.
- Common security queries: this option provides you a list of the most common security queries that you can use to monitor your environment. When you click in one of those queries, it opens the Search blade with the results for that query

The screenshot below shows an example of the type of information that can be displayed by OMS.

![OMS security baselines](./media/azure-security-iaas/oms-security-baseline.png)



## Next Steps


* [Azure security Team Blog](https://blogs.msdn.microsoft.com/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)
* [Azure Security Best Practices and Patterns](security-best-practices-and-patterns.md)
