---
title: 'Tutorial: Azure Active Directory integration with Boomi | Microsoft Docs'
description: Learn how to configure single sign-on between Azure Active Directory and Boomi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
editor: na

ms.assetid: 8e05afa9-2eda-4975-a0cc-6d408065860f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: jeedes

---
# Tutorial: Azure Active Directory integration with Boomi

In this tutorial, you learn how to integrate Boomi with Azure Active Directory (Azure AD).

Integrating Boomi with Azure AD provides you with the following benefits:

- You can control in Azure AD who has access to Boomi
- You can enable your users to automatically get signed-on to Boomi (Single Sign-On) with their Azure AD accounts
- You can manage your accounts in one central location - the Azure classic portal

If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).

## Prerequisites

To configure Azure AD integration with Boomi, you need the following items:

- An Azure AD subscription
- A Boomi single-sign on enabled subscription


> [!NOTE]
> To test the steps in this tutorial, we do not recommend using a production environment.


To test the steps in this tutorial, you should follow these recommendations:

- You should not use your production environment, unless this is necessary.
- If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).


## Scenario description
In this tutorial, you test Azure AD single sign-on in a test environment. 
The scenario outlined in this tutorial consists of two main building blocks:

1. Adding Boomi from the gallery
2. Configuring and testing Azure AD single sign-on


## Adding Boomi from the gallery
To configure the integration of Boomi into Azure AD, you need to add Boomi from the gallery to your list of managed SaaS apps.

**To add Boomi from the gallery, perform the following steps:**

1. In the **Azure classic portal**, on the left navigation pane, click **Active Directory**. 

	![Active Directory][1]

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To open the applications view, in the directory view, click **Applications** in the top menu.

	![Applications][2]

4. Click **Add** at the bottom of the page.

	![Applications][3]

5. On the **What do you want to do** dialog, click **Add an application from the gallery**.

	![Applications][4]

6. In the search box, type **Boomi**.

	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_01.png)

7. In the results pane, select **Boomi**, and then click **Complete** to add the application.

	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_02.png)


##  Configuring and testing Azure AD single sign-on
In this section, you configure and test Azure AD single sign-on with Boomi based on a test user called "Britta Simon".

For single sign-on to work, Azure AD needs to know what the counterpart user in Boomi is to a user in Azure AD. In other words, a link relationship between an Azure AD user and the related user in Boomi needs to be established.

This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Boomi.

To configure and test Azure AD single sign-on with Boomi, you need to complete the following building blocks:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.
2. **[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.
3. **[Creating a Boomi test user](#creating-a-boomi-test-user)** - to have a counterpart of Britta Simon in Boomi that is linked to the Azure AD representation of her.
4. **[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.
5. **[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.

### Configuring Azure AD single sign-on

The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your Boomi application.

Boomi application expects the SAML assertion in specific format, which requires you to set the NameIdentifier attribute value with the user’s Federation ID. By default Azure AD uses the UserPrincipalName for NameIdentifier attribute. But for successful integration you need to adjust this value to match with user’s Federation ID in Boomi. You can change this from "**Atrribute**" tab as shown in the screenshot below. The integration will only work after completing the correct mapping.

![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_51.png)

**To configure Azure AD single sign-on with Boomi, perform the following steps:**

1. In the classic portal, on the **Boomi** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.

	![Configure Single Sign-On][6] 

2. On the **How would you like users to sign on to Boomi** page, select **Azure AD Single Sign-On**, and then click **Next**.
 
	![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_03.png)

3. On the **Configure App Settings** dialog page, perform the following steps:

	![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_04.png)

    a. In the **Reply URL** textbox, type a URL using the following pattern: `https://platform.boomi.com/sso/<account name>/saml`

    b. Click **Next**.

	> [!NOTE] 
	> Please note that this is not the real value. You have to update this value with the actual Reply URL. Contact Boomi support team to get this value.

4. On the **Configure single sign-on at Boomi** page, click **Download certificate** and then save the file on your computer:

	![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_05.png) 

	> [!NOTE]
	> Please note that NameID claim value in the Response has to match with Federation ID configured in Boomi system. So please work with Boomi support team to map the appropriate user identifier in your organization as Federation ID. By default Azure AD will set the NameIdentifier as UPN value. You can change this from Attribute tab as shown in the screenshot below. The integration will only work after completing the correct mapping. 
 	
	![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_51.png)

5. In a different web browser window, log into your Boomi company site as an administrator. 

6. Navigate to **Company Name** and go to **Set up**.

7. Click the **SSO options** tab and perform below steps.

	![Configure Single Sign-On On App Side](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_06.png)

	a. Check **Enable SAML Single Sign-On** checkbox.

	b. Click **Import** to upload the downloaded certificate from Azure AD to **Identity Provider Certificate**.
	
	c. In the **Identity Provider Login URL** textbox, put the value of **Remote login URL** from Azure AD application configuration wizard.

	d. As **Federation Id Location**, select **Federation Id is in NameID element of the Subject** radio button. 

	e. Click **Save** button.

8. In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.

	![Azure AD Single Sign-On][10]

9. On the **Single sign-on confirmation** page, click **Complete**.  
  
	![Azure AD Single Sign-On][11]


### Creating an Azure AD test user
The objective of this section is to create a test user in the classic portal called Britta Simon.

![Create Azure AD User][20]

**To create a test user in Azure AD, perform the following steps:**

1. In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.

	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/create_aaduser_09.png) 

2. From the **Directory** list, select the directory for which you want to enable directory integration.

3. To display the list of users, in the menu on the top, click **Users**.

	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/create_aaduser_03.png) 

4. To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.
 
	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/create_aaduser_04.png) 

5. On the **Tell us about this user** dialog page, perform the following steps:
 
	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/create_aaduser_05.png) 

    a. As Type Of User, select New user in your organization.

    b. In the User Name **textbox**, type **BrittaSimon**.

    c. Click **Next**.

6.  On the **User Profile** dialog page, perform the following steps:

	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/create_aaduser_06.png) 

    a. In the **First Name** textbox, type **Britta**.  

    b. In the **Last Name** textbox, type, **Simon**.

    c. In the **Display Name** textbox, type **Britta Simon**.

    d. In the **Role** list, select **User**.

    e. Click **Next**.

7. On the **Get temporary password** dialog page, click **create**.

	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/create_aaduser_07.png) 

8. On the **Get temporary password** dialog page, perform the following steps:

	![Creating an Azure AD test user](./media/active-directory-saas-boomi-tutorial/create_aaduser_08.png) 

    a. Write down the value of the **New Password**.

    b. Click **Complete**.   



### Creating a Boomi test user

In order to enable Azure AD users to log into Boomi, they must be provisioned into Boomi. In the case of Boomi, provisioning is a manual task.

####To provision a user account, perform the following steps:

1. Log into your Boomi company site as an administrator.

2. After logging in, navigate to **User Management** and go to **Users**.

	![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_001.png "Users")

3. Click **+**  icon and the **Add/Maintain User Roles** dialog opens.

	![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_002.png "Users")

	![Users](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_003.png "Users")

4. Enter the user's **User e-mail address**.

5. Enter the user's **First name** and **Last name**.

6. Enter the user's **Federation ID**. Each user must have a Federation ID that uniquely identifies the user within the account. 

7. Assign the **Standard User** role to the user. Do not assign the Administrator role because that would give him normal Atmosphere access as well as single sign-on access.

8. Click **OK**.

	> [!NOTE]
	> The user will not receive a welcome notification email containing a password that can be used to log into the AtomSphere account because his password will be managed through the identity provider. You may use any other Boomi user account creation tools or APIs provided by Boomi to provision AAD user accounts. 

### Assigning the Azure AD test user

In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Boomi.

![Assign User][200] 

**To assign Britta Simon to Boomi, perform the following steps:**

1. On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.

	![Assign User][201] 

2. In the applications list, select **Boomi**.

	![Configure Single Sign-On](./media/active-directory-saas-boomi-tutorial/tutorial_boomi_50.png) 

3. In the menu on the top, click **Users**.

	![Assign User][203] 

4. In the Users list, select **Britta Simon**.

5. In the toolbar on the bottom, click **Assign**.
	
	![Assign User][205]



### Testing single sign-on

In this section, you test your Azure AD single sign-on configuration using the Access Panel.

When you click the Boomi tile in the Access Panel, you should get automatically signed-on to your Boomi application.


## Additional resources

* [List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory](active-directory-saas-tutorial-list.md)
* [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-boomi-tutorial/tutorial_general_205.png
