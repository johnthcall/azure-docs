---
title: Troubleshooting Azure Active Directory B2B collaboration | Microsoft Docs
description: Remedies for common problems with Azure Active Directory B2B collaboration
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

# Troubleshooting Azure Active Directory B2B collaboration

Here are some remedies for common problems with Azure Active Directory (Azure AD) B2B collaboration.

## I can’t create an external user due to an existing contact

If the external user you are inviting already has a pre-existing contact object, you will be unable to invite that user until you resolve the conflict, usually by removing the contact object. Until general availability of B2B collaboration, the conflict must be resolved manually.

## I’ve added an external user but do not see them in my Global Address Book or in the people picker

In cases where external users are not populated in the list, the object might take a few minutes to replicate.

## Invitations have been disabled for directory

If you receive an error message indicating that you do not have permissions to invite users, verify that your user account is authorized to invite external users. This can be done under User Settings:

![](media/active-directory-b2b-troubleshooting/external-user-settings.png)

If you have recently modified these settings or assigned the Guest Inviter role to a user, there might be a 15-60 minute delay before the changes take effect.

## The user that I invited is receiving an error during redemption

Common errors include:

### Invitee’s Admin has disallowed EmailVerified Users from being created in their tenant:

When inviting users whose organization is utilizing Azure Active Directory where the specific user’s account does not exist (User does not exist in AAD contoso.com). The administrator of contoso.com may have a policy in place preventing users from being created. The External User must check with their admin to determine if external users are allowed, the external user’s admin may need to allow Email Verified users in their domain (see this [article](https://docs.microsoft.com/en-us/powershell/msonline/v1/set-msolcompanysettings#parameters) on allowing EmailVerified Users).

![](media/active-directory-b2b-troubleshooting/allow-email-verified-users.png)

### External user does not exist already in a federated domain

In cases where the external user is using a federation solution where authentication is being performed on-premises and the user does not already exist in Azure Active Directory, the user cannot be invited.

To resolve this issue, the external user’s admin must synchronize the user’s account to Azure Active Directory.

## How does ‘\#’, which is normally an invalid character, sync with Azure AD?

“\#” is a reserved character in UPNs for Azure AD B2B collaboration or external users (that is, &lt;user@contoso.com&gt; invited, becomes &lt;user_contoso.com#EXT@fabrikam.onmicrosoft.com&gt;) so \# in UPNs coming from on-premises are not allowed to sign in to the Azure portal.

## I receive an error when adding external users to a synchronized group

External users can be added only to “assigned” or “Security” groups and not to groups that are mastered on-premises.

## My external user did not receive an email to redeem

The invitee should check with their ISP or spam filter to ensure that the following address is allowed: &lt;Invites@microsoft.com&gt;

## My recipient received multiple emails from me

In some cases, where the invitation recipient has multiple aliases for their account, they might receive two invitations. In these cases, the first link redeemed is the account that gets created, and the second redemption link is then not valid.

## Next steps

Browse our other articles on Azure AD B2B collaboration:

* [What is Azure AD B2B collaboration?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [How do Azure Active Directory admins add B2B collaboration users?](active-directory-b2b-admin-add-users.md)
* [How do information workers add B2B collaboration users?](active-directory-b2b-how-it-works.md)
* [The elements of the B2B collaboration invitation email](active-directory-b2b-invitation-email.md)
* [B2B collaboration invitation redemption](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B collaboration licensing](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B collaboration frequently-asked questions (FAQ)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B collaboration API and customization](active-directory-b2b-api.md)
* [Multi-factor authentication for B2B collaboration users](active-directory-b2b-mfa-instructions.md)
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md)
