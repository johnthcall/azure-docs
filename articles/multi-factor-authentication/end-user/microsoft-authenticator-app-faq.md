---
title: Microsoft Authenticator app help and support | Microsoft Docs
description: Provides a list of frequently asked questions and answers related to the Microsoft Authentication app and Azure Multi-Factor Authentication.
services: multi-factor-authentication
documentationcenter: ''
author: kgremban
manager: femila
editor: pblachar, librown

ms.assetid: f04d5bce-e99e-4f75-82d1-ef6369be3402
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2017
ms.author: kgremban

---
# Microsoft Authenticator app FAQ
The Microsoft Authenticator app replaced the Azure Authenticator app, and is the recommended app when you use Azure Multi-Factor Authentication. This app is available for Windows Phone, Android, and iOS.

## Frequently asked questions
### What happened to the Azure Authenticator, Multi-Factor Auth, and Microsoft account apps?
The Microsoft Authenticator app replaces each other these apps. Azure Authenticator upgraded to Microsoft Authenticator. If you use the Multi-Factor Auth or Microsoft account apps, install Microsoft Authenticator and add your accounts again. Make sure to finish adding your accounts to the new app before deleting the old ones.

The Microsoft Authenticator app is available for [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), and [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

### I'm already using the Microsoft Authenticator application for verification codes. How do I switch to one-click push notifications?
Approving a sign-in through push notification is only available for personal Microsoft accounts or work and school Microsoft accounts, not for third-party accounts like Google or Facebook. If you have a work or school Microsoft account, your organization can choose to disable this option.

If you use a Microsoft account for your personal account and want to switch over to push notifications, you need to add your account again. Re-register the device with your account, and set up push notifications.  

If you use Microsoft Authenticator for your work or school account, then your organization decides whether to allow one-click notifications.

### Do one-click push notifications work for non-Microsoft accounts?
No, push notifications only work with Microsoft accounts and Azure Active Directory accounts. If your work or school uses Azure AD accounts, they may disable this feature.  

### I restored my device from a backup, and my account codes are missing or not working. What happened?
For security purposes, we don't restore accounts from app backups. If you restore the iOS app from a backup, your accounts are still displayed but they don't work to receive sign-in verifications or generate security codes. After you restore the app, delete your accounts and add them again.

### I got a new device. How do I remove the Microsoft Authenticator app from my old device and move to the new one?
Adding the Microsoft Authenticator app to a new device does not automatically remove it from any other devices. To manage which devices are configured for your account, visit the same website that you use to manage two-step verification, and choose to remove old apps.

For personal Microsoft accounts, this website is your [account security](https://account.microsoft.com/security) page. For work or school Microsoft accounts, this website may be either [MyApps](https://myapps.microsoft.com) or a custom portal that your organization has set up.

### How do I remove an account from the app?
* iOS: From the main screen, swipe left on an account tile. Select **Delete**.
* Windows Phone: From the main screen, select the menu button, then **Edit accounts**. Tap the **X** next to the account name.
* Android: From the main screen, select the menu button, then **Edit accounts**. Tap the **X** next to the account name.

If you have an Android device that is registered with your organization, you may need to complete an extra step to remove your account. On these devices, the Microsoft Authenticator app is automatically registered as a device administrator. If you want to completely uninstall the app, you need to first unregister the app in the app settings.

### Why does the app request so many permissions?
Here is a full list of permissions we ask for, and how they are used in the app:

* **Camera**: We use your camera to scan QR codes when you add a work, school, or non-Microsoft account.
* **Contacts and phone**: When you sign in with your personal Microsoft account, we try to simplify the process by finding existing accounts that you use on your phone.
* **SMS**: When you sign in with your personal Microsoft account for the first time, we have to make sure that your phone number matches the one we have on record. We send a text message to the phone where you downloaded the app. The message contains a 6-8 digit verification code. Instead of asking you to find this code and enter it in the app, we find it in the text message for you.
* **Draw over other apps**: When you receive a notification to verify your identity, we display that notification over any other app that might be running.
* **Receive data from the internet**: This permission is required for sending notifications.
* **Prevent phone from sleeping**: If you register your device with your organization, they can change this policy on your phone.
* **Control vibration**: You have the option to choose whether you would like a vibration whenever you receive a notification to verify your identity.
* **Use fingerprint hardware**: Some work and school accounts require an additional PIN whenever you verify your identity. TO make the process easier, we allow you to use your fingerprint instead of entering the PIN.
* **View network connections**: When you add a Microsoft account, the app requires network/internet connection.
* **Read the contents of your storage**: This permission is only used when you report a technical problem through the app settings. Some information from your storage is collected to diagnose the issue.
* **Full network access**: This permission is required for sending notifications to verify your identity.
* **Run at startup**: If you restart your phone, this permission ensures that you continue you receive notifications to verify your identity.

### Why does the Microsoft Authenticator App allow you to approve a request without unlocking the device?

This is by design. Two-step verification requires proving two things – a thing you know, and a thing you have. The thing you know is the password. The thing you have is your phone (set up with the Microsoft Authenticator app and registered as an MFA proof.)  Therefore, having the phone and approving the request meets the criteria for the second factor of authentication. 

## Next steps

### Contact us
If your question wasn't answered here, we want to hear from you. Go to the [Microsoft Authenticator app forum](https://social.technet.microsoft.com/Forums/en-US/home?forum=MicrosoftAuthenticatorApp) to post your question and get help from the community, leave a comment on this page, or [contact support](https://support.microsoft.com/contactus) and we'll respond to your problem as soon as we can.


### Related topics
* [About two-step verification](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) for Microsoft accounts
* [Having trouble with two-step verification](multi-factor-authentication-end-user-troubleshoot.md) for your work or school account?

