---
title: Create, link, delete, or move an integration account in Azure logic apps | Microsoft Docs
description: How to create an integration account, and link it to your logic apps
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 

ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: mandia

---

# What is an integration account?
An integration account allows enterprise integration apps to manage artifacts, including schemas, maps, certificates, partners and agreements. Any integration app you create uses an integration account to access these schemas, maps, certificates, and so on.

## Create an integration account
1. Select **Browse**:   
   ![](./media/logic-apps-enterprise-integration-accounts/account-1.png)  
2. Enter **integration** in the filter search box and select **Integration Accounts** from the results list:     
   ![](./media/logic-apps-enterprise-integration-accounts/account-2.png)  
3. Select *Add* button from the menu at the top of the page:      
   ![](./media/logic-apps-enterprise-integration-accounts/account-3.png)  
4. Enter the **Name**, select the **Subscription** you want to use, either create a new **Resource group** or select an existing resource group, select a **Location** where your integration account will be hosted, select a **Pricing tier**, then select the **Create** button.   
   
   At this point the integration account will be provisioned in the location you selected. This should complete within 1 minute.    
   ![](./media/logic-apps-enterprise-integration-accounts/account-4.png)  
5. Refresh the page. You see your new integration account listed:  
   ![](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Next, link the integration account you just created to your logic app. 

## Link an integration account to a Logic app
For your Logic apps to access to maps, schemas, agreements, and other artifacts in your integration account, link the integration account to your Logic app.

#### Prereqs
* An integration account
* A logic app

> [!NOTE] 
> Be sure your integration account and Logic app are in the **same Azure location** before you begin.

1. Select **Settings** link from the menu of your Logic app:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)   
2. Select the **Integration Account** item from the Settings blade:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)   
3. Select the integration account you wish to link to your Logic app from the **Select an Integration account** drop down list box:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)   
4. Save your work:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-4.png)   
5. You will see a notification that indicates that your integration account has been linked to your Logic app and that all artifacts in your integration account are now available to your Logic app:  
   ![](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)   

Now that your integration account is linked to your logic app, you can use the B2B connectors within you logic apps. Some common B2B connectors include XML validation, flat file encode/decode.  

## How to delete an integration account?
1. Select **Browse**:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)    
2. Enter **integration** in the filter search box and select **Integration Accounts** from the results list:     
   ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)  
3. Select the **integration account** that you wish to delete:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
4. Select the **Delete** link that's located on the menu:   
   ![](./media/logic-apps-enterprise-integration-accounts/delete.png)  
5. Confirm your choice.    

## How to move an integration account?
You can easily move an integration account to a new subscription and a new resource group. Follow these steps if you need to move your integration account:

> [!IMPORTANT]
> You need to update all scripts to use the new resource IDs after you move an integration account.

1. Select **Browse**:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)    
2. Enter **integration** in the filter search box and select **Integration Accounts** from the results list:     
   ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)  
3. Select the **integration account** that you wish to delete:  
   ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)  
4. Select the **Move** link that's located on the menu:   
   ![](./media/logic-apps-enterprise-integration-accounts/move.png)  
5. Confirm your choice.    

## Next Steps
* [Learn more about agreements](../logic-apps/logic-apps-enterprise-integration-agreements.md "Learn about enterprise integration agreements")  

