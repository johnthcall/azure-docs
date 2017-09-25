---
title: Encode EDIFACT messages in Azure logic apps | Microsoft Docs
description: How to use the EDIFACT encoder in the Enterprise Integration Pack with your logic apps 
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: ''

ms.assetid: 974ac339-d97a-4715-bc92-62d02281e900
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2017
ms.author: padmavc

---
# Get started with Encode EDIFACT Message
Validates EDI and partner-specific properties 

## Prereqs
* An Azure account; you can create a [free account](https://azure.microsoft.com/free)
* An Integration Account is required to use Encode EDIFACT message connector. See details on how to create an [Integration Account](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), [partners](logic-apps-enterprise-integration-partners.md) and [EDIFACT agreement](../logic-apps/logic-apps-enterprise-integration-edifact.md)

## Encode EDIFACT messages
1. [Create a Logic App](../logic-apps/logic-apps-create-a-logic-app.md).
2. This connector does not have any triggers. Use other triggers to start the Logic App, such as a Request trigger.  In the Logic App designer, add a trigger and add an action.  Select Show Microsoft managed APIs in the drop-down list and then enter "EDIFACT" in the search box.  Select either Encode EDIFACT Message by agreement name or Encode to EDIFACT message by identities:
   
    ![search EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  
3. If you haven’t previously created any connections to Integration Account, you are prompted for the connection details:
   
    ![create integration account connection](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  
4. Enter the Integration account details.  Properties with an asterisk are required:
   
   | Property | Details |
   | --- | --- |
   | Connection Name * |Enter any name for your connection |
   | Integration Account * |Enter the Integration Account name. Be sure your Integration Account and Logic app are in the same Azure location |
   
    Once complete, your connection details look similar to the following:
   
    ![integration account connection](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)
5. Select **Create**.
6. Notice the connection has been created:
   
    ![integration account connection details](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### Encode EDIFACT Message by agreement name
Enter the EDIFACT agreement name and the XML message to encode:
   
   ![provide mandatory fields](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### Encode EDIFACT Message by identities
Enter the sender identifier, sender qualifier, receiver identifier, and receiver qualifier as configured in the EDIFACT agreement. Select the XML message to encode:  
    ![provide mandatory fields](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## EDIFACT Encode details
The Encode EDIFACT connector does the following: 

* Resolve the agreement by matching the sender qualifier & identifier and receiver qualifier and identifier
* Serializes the EDI interchange, converting XML-encoded messages into EDI transaction sets in the interchange.
* Applies transaction set header and trailer segments
* Generates an interchange control number, a group control number, and a transaction set control number for each outgoing interchange
* Replaces separators in the payload data
* Validates EDI and partner-specific properties
  * Schema validation of the transaction-set data elements against the message schema.
  * EDI validation performed on transaction-set data elements.
  * Extended validation performed on transaction-set data elements
* Generates an XML document for each transaction set.
* Requests a Technical and/or Functional acknowledgment (if configured).
  * As a technical acknowledgment, the CONTRL message indicates receipt of an interchange.
  * As a functional acknowledgment, the CONTRL message indicates acceptance or rejection of the received interchange, group, or message, with a list of errors or unsupported functionality

## Next steps
[Learn more about the Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Learn about Enterprise Integration Pack") 

