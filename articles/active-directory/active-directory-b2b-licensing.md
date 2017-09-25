---
title: Azure Active Directory B2B collaboration licensing guidance | Microsoft Docs
description: Azure Active Directory B2B collaboration requires paid Azure AD licenses depending on the features you want for your B2B collaboration users
services: active-directory
documentationcenter: ''
author: sasubram
manager: femila
editor: ''
tags: ''

ms.assetid:
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 02/02/2017
ms.author: sasubram

---

# Azure Active Directory B2B collaboration licensing guidance

Azure Active Directory (Azure AD) B2B collaboration extends a select set of existing Azure AD features to guest users invited into the Azure AD tenant. Hence, Azure AD B2B collaboration guest users will be licensed through Azure AD licenses, and align with the existing Free, Basic, and Premium P1/P2 license tiers as seen here: https://azure.microsoft.com/en-us/pricing/details/active-directory/.

There is no charge for inviting B2B users and assigning them to an application in Azure AD. Also, up to 10 apps per guest user and 3 basic reports are also free for B2B users, since they are part of Azure AD 'Free' tier.
Any paid Azure AD features, extended to B2B users via B2B collaboration functionality, will need to be licensed with Azure AD paid licenses (Basic, Premium P1, or Premium P2, depending on the features that will be used). The inviting tenant will get 5 B2B user rights with each Azure AD paid license. That is, each Azure AD paid license that provides rights to one employee user in a tenant, will now also include rights to 5 B2B users invited to the tenant.

## Licensing examples
- A customer wants to invite 100 B2B users to its Azure AD tenant, and will use Group based access management and provisioning for all users, but 50 of those users will also be requiring MFA and conditional access. The customer here must purchase 10 Azure AD Basic licenses, and 10 Azure AD Premium P1 licenses to cover all of their B2B users correctly. Similarly, if an inviting tenant plans to use Identity Protection features with B2B users, then it must have enough Azure AD Premium P2 licenses to cover all these B2B users in the 5:1 ratio.
- A customer has 10 employees who are all currently licensed with Azure AD Premium P1. They now want to invite 60 B2B users, who will all require multi-factor authentication (MFA). Per the 5:1 licensing rule, the customer must have at least 12 Azure AD Premium P1 licenses to cover all 60 B2B collaboration users. Since they already have 10 Premium P1 licenses for their 10 employees, they already have rights to invite 50 B2B users with Premium P1 features like MFA. So in this example, they need to purchase 2 additional Premium P1 licenses to cover the remaining 10 B2B collaboration users.

> [!NOTE]
There is no facility or need to assign licenses to the B2B users to enable these B2B collaboration user rights.

The customer who owns the inviting tenant must be the one to determine how many B2B collaboration users need paid Azure AD capabilities and depending on whether they are Basic, Premium P1, or Premium P2 level features, customers must have the sufficient number of the appropriate Azure AD paid licenses to cover their B2B collaboration users in the 5:1 ratio. If a company needs additional B2B collaboration user rights, they must purchase the required Azure AD paid licenses.

## Additional licensing details
- B2B collaboration can provide different capabilities to different B2B collaboration users based on the existing model of Azure AD editions.
- Every Azure AD paid license will include rights to 5 B2B collaboration users (5:1 model).
- There is no need to actually assign licenses to B2B user accounts. There will be automatic calculation and reporting.
- Every invited user gets the rights that the Azure AD Free edition offers if no paid Azure AD license exists in the tenant.
- If a B2B collaboration user has a paid Azure AD license as an employee from his organization, he will not consume one of the B2B collaboration licenses of the inviting tenant.

## Next steps

Browse our other articles on Azure AD B2B collaboration:

* [What is Azure AD B2B collaboration?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [How do Azure Active Directory admins add B2B collaboration users?](active-directory-b2b-admin-add-users.md)
* [How do information workers add B2B collaboration users?](active-directory-b2b-how-it-works.md)
* [The elements of the B2B collaboration invitation email](active-directory-b2b-invitation-email.md)
* [B2B collaboration invitation redemption](active-directory-b2b-redemption-experience.md)
* [Troubleshooting Azure Active Directory B2B collaboration](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B collaboration frequently-asked questions (FAQ)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B collaboration API and customization](active-directory-b2b-api.md)
* [Multi-factor authentication for B2B collaboration users](active-directory-b2b-mfa-instructions.md)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md)
