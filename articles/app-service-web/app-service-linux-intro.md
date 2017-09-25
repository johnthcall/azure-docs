---
title: Introduction to App Service on Linux | Microsoft Docs
description: Learn about App Service on Linux.
keywords: azure app service, linux, oss
services: app-service
documentationcenter: ''
author: naziml
manager: wpickett
editor: ''

ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: naziml

---
# Introduction to App Service on Linux
Azure App Service on Linux is currently in public preview and supports running web apps natively on Linux.

## Overview
Customers can use App Service on Linux to host web apps natively on Linux for supported application stacks. The following section lists the application stacks that are currently supported. 

## Features
App Service on Linux currently supports the following application stacks:

* Node.js
* PHP
* .Net Core

Customers can deploy their applications by using:

* FTP
* Local Git
* GitHub or Bitbucket

For application scaling:

* Customers can scale their web apps up and down by changing the tier in their App Service plan.
* Customers can scale out their applications out and run their app across multiple instances within the confines of their SKU.

For Kudu, some of the basic functionality works with the following:

* Environments
* Deployments
* Basic consoles

## Limitations
The Azure portal shows only features that currently work for App Service on Linux and hides the rest. As we enable more features, you will see them reflected on the portal.

Some features, such as virtual network integration, Azure Active Directory/third-party authentication, or Kudu site extensions, don't currently work. But as we get these working, we will update our documentation and blog about the changes.

This public preview is currently only available in the following regions:

* West US
* West Europe 
* Southeast Asia

Web Apps on Linux is only supported in the Dedicated app service plans and does not have a Free or Shared tier. Also, App Service plans for regular and Linux web apps are mutually exclusive, so you cannot create a Linux web app in a non-Linux app service plan.

Web Apps on Linux must be created in a resource group that does not contain non-Linux web apps in the same region.

## Next steps
See the following links to get started with App Service on Linux. You can post questions and concerns on [our forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazurewebsitespreview).

* [Creating Web Apps in App Service on Linux](app-service-linux-how-to-create-a-web-app.md)
* [How to use a custom Docker image for App Service on Linux](app-service-linux-using-custom-docker-image.md)
* [Using PM2 Configuration for Node.js in Web Apps on Linux](app-service-linux-using-nodejs-pm2.md)
* [Using .NET Core in Azure App Service Web Apps on Linux](app-service-linux-using-dotnetcore.md)
