---
title: 'Hybrid Identity required ports and protocols - Azure | Microsoft Docs'
description: This page is a technical reference page for ports that are required to be open for Azure AD Connect
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: curtand
ms.assetid: de97b225-ae06-4afc-b2ef-a72a3643255b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2016
ms.author: billmath

---
# Hybrid Identity Required Ports and Protocols
The following document is a technical reference on the required ports and protocols for implementing a hybrid identity solution. Use the following illustration and refer to the corresponding table.

![What is Azure AD Connect](./media/active-directory-aadconnect-ports/required2.png)

## Table 1 - Azure AD Connect and On-premises AD
This table describes the ports and protocols that are required for communication between the Azure AD Connect server and on-premises AD.

| Protocol | Ports | Description |
| --- | --- | --- |
| DNS |53 (TCP/UDP) |DNS lookups on the destination forest. |
| Kerberos |88 (TCP/UDP) |Kerberos authentication to the AD forest. |
| MS-RPC |135 (TCP/UDP) |Used during the initial configuration of the Azure AD Connect wizard when it binds to the AD forest. |
| LDAP |389 (TCP/UDP) |Used for data import from AD. Data is encrypted with Kerberos Sign & Seal. |
| LDAP/SSL |636 (TCP/UDP) |Used for data import from AD. The data transfer is signed and encrypted. Only used if you are using SSL. |
| RPC |49152- 65535 (Random high RPC Port)(TCP/UDP) |Used during the initial configuration of Azure AD Connect when it binds to the AD forests. See [KB929851](https://support.microsoft.com/kb/929851), [KB832017](https://support.microsoft.com/kb/832017), and [KB224196](https://support.microsoft.com/kb/224196) for more information. |

## Table 2 - Azure AD Connect and Azure AD
This table describes the ports and protocols that are required for communication between the Azure AD Connect server and Azure AD.

| Protocol | Ports | Description |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Used to download CRLs (Certificate Revocation Lists) to verify SSL certificates. |
| HTTPS |443(TCP/UDP) |Used to synchronize with Azure AD. |

For a list of URLs and IP addresses you need to open in your firewall, see [Office 365 URLs and IP address ranges](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

## Table 3 - Azure AD Connect and AD FS Federation Servers/WAP
This table describes the ports and protocols that are required for communication between the Azure AD Connect server and AD FS Federation/WAP servers.  

| Protocol | Ports | Description |
| --- | --- | --- |
| HTTP |80 (TCP/UDP) |Used to download CRLs (Certificate Revocation Lists) to verify SSL certificates. |
| HTTPS |443(TCP/UDP) |Used to synchronize with Azure AD. |
| WinRM |5985 |WinRM Listener |

## Table 4 - WAP and Federation Servers
This table describes the ports and protocols that are required for communication between the Federation servers and WAP servers.

| Protocol | Ports | Description |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Used for authentication. |

## Table 5 - WAP and Users
This table describes the ports and protocols that are required for communication between users and the WAP servers.

| Protocol | Ports | Description |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Used for device authentication. |
| TCP |49443 (TCP) |Used for certificate authentication. |

## Table 6 - Pass-through Authentication
This table describes the ports and protocols that are required for communication between the connector and Azure AD.

|Protocol|Port Number|Description
| --- | --- | ---
|HTTP|80|Enable outbound HTTP traffic for security validation such as SSL.
|HTTPS|443|	Enable user authentication against Azure AD
|HTTPS|10100–10120|	Enable responses from the connector back to the Azure AD 
|Azure service bus|9352, 5671|	Enable communication between the Connector toward the Azure service for incoming requests.
|HTTPS|9350|	Optional, to enables better performance for incoming requests
|HTTPS|8080/443|	Enable the Connector bootstrap sequence and Connector automatic update
|HTTPS|9090|	Enable Connector registration (required only for the Connector registration process)
|HTTPS|9091|	Enable Connector trust certificate automatic renewal

## Table 7a & 7b - Azure AD Connect Health agent for (AD FS/Sync) and Azure AD
The following tables describe the endpoints, ports, and protocols that are required for communication between Azure AD Connect Health agents and Azure AD

### Table 7a - Ports and Protocols for Azure AD Connect Health agent for (AD FS/Sync) and Azure AD
This table describes the following outbound ports and protocols that are required for communication between the Azure AD Connect Health agents and Azure AD.  

| Protocol | Ports | Description |
| --- | --- | --- |
| HTTPS |443(TCP/UDP) |Outbound |
| Azure Service Bus |5671 (TCP/UDP) |Outbound |

### 7b - Endpoints for Azure AD Connect Health agent for (AD FS/Sync) and Azure AD
For a list of endpoints, see [the Requirements section for the Azure AD Connect Health agent](../connect-health/active-directory-aadconnect-health-agent-install.md#requirements).

