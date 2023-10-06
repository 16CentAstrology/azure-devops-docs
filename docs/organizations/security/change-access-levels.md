---
title: Change access levels
titleSuffix: Azure DevOps 
description: Learn how to set the access level for a user or group based on their license. 
ms.subservice: azure-devops-security
ms.assetid: 84B0D454-09A7-414B-A9E0-FE9A9ACD7E99
ms.topic: how-to
ms.reviewer:  
ms.author: chcomley
author: chcomley
monikerRange: '< azure-devops'
ms.date: 09/28/2023 
---

# Change access levels

[!INCLUDE [version-lt-azure-devops](../../includes/version-lt-azure-devops.md)]

> [!NOTE]
> This article applies to Azure DevOps Server (on-premises). To manage access levels for Azure DevOps Services (cloud), see [Add users to your organization or project](../accounts/add-organization-users.md).

Access levels grant or restrict access to use the functions and features that Azure DevOps Server provides. Access levels are in addition to permissions granted through security groups, which provide or restrict specific tasks. In this article, learn how to change access levels for users and groups. For more information, see [About access levels](access-levels.md).

For a simplified overview of the permissions that are assigned to the most common groups&mdash;Readers, Contributors, and Project Administrators&mdash;and the Stakeholder access group, see [Permissions and access](permissions-access.md).  

[!INCLUDE [version-selector](../../includes/version-selector.md)]

## Prerequisites

* You must be a member of the Administrators group. If you aren't a member, [get added now](/azure/devops/server/admin/add-administrator).
* To manage access for a large group of users, create either a [Windows group, a group in Active Directory, or Azure DevOps security group](/azure/devops/server/admin/setup-ad-groups), and then add users to those groups.
* Users must be [added to a project](add-users-team-project.md).

## Open access levels

You can manage access levels for the collections defined on the application tier. The default access level affects all the projects in all the collections. When you add users or groups to teams, projects, or collections, they get the default access level. To give a different access level to a certain user or group, you need to add them to a non-default access level.

::: moniker range=">= azure-devops-2019"

- From the web portal home page for a project collection (for example, `http://MyServer:8080/tfs/DefaultCollection/`), open **Access levels**. If you're at a project level, choose the :::image type="icon" source="../../media/icons/project-icon.png" border="false"::: Azure DevOps logo and then choose **Access levels**.

   ![Screenshot of web portal, Open Access levels dialog.](media/change-access-levels/open-access-levels-2019.png)

	If you don't see **Access levels**, you aren't an administrator and need to [get permission](/azure/devops/server/admin/add-administrator).

::: moniker-end

::: moniker range="tfs-2018"

From a user context, open **Server Settings** by choosing the :::image type="icon" source="../../boards/media/icons/gear_icon.png" border="false"::: gear icon. The tabs and pages available differ depending on which settings level you access.

- From the web portal home page for a project (for example, `http://MyServer:8080/tfs/DefaultCollection/MyProject/`), open **Server settings**.

   ![Screenshot of TFS 2017, Web portal, open the Server settings admin context.](media/access-levels-2017-open-admin-context.png) 

::: moniker-end

## Add a user or group to an access level 

Changes you make to the access level settings take effect immediately.

::: moniker range=">= azure-devops-2019"

1. Select the access level you want to manage.

	For example, here we choose **Basic**, and then **Add** to add a group to Basic access.
 
   ![Screenshot of Basic access level, adding group.](media/change-access-levels/basic-access-2019.png)

1. Enter the name of the user or group into the text box. You can enter several identities into the text box, separated by commas. The system automatically searches for matches. Choose the matches that meet your choice.

   ![Screenshot of Add users and group dialog.](media/project-level-permissions-add-a-user.png)  

2. Choose **Save changes**. 

::: moniker-end

::: moniker range="tfs-2018"

1. From **Access levels**, select the access level you want to manage. For example, here we choose **Stakeholder**, and then **Add** to add a group to Stakeholder access.

	![Screenshot of TFS 2017, Web portal, adding user or group.](media/access-levels-2017-stakeholder-access.png)

	If you don't see **Access levels**, you aren't an administrator and need to [get permission](/azure/devops/server/admin/add-administrator).

2. Enter the name of the user or group into the text box. You can enter several identities into the text box, separated by commas. The system automatically searches for matches. Choose the matches that meet your choice.

   ![Screenshot of Add users and group dialog.](media/project-level-permissions-add-a-user.png)  

3. Choose **Save changes**.

::: moniker-end

## Change the access level for a user or group 

To assign a different access level to a user or group, you need to first delete their current access level and then grant them the new one.

Make sure to set each user's access level based on what you've purchased for that user. Basic access includes all Stakeholder features - Basic + Test Plans. Advanced and Visual Studio Enterprise subscriber access levels include all Basic features.

1. Choose the user or group and then select **Remove**.

   ![Screenshot of Collection level permissions and groups page.](media/change-access-levels/remove-user-from-access-level.png)  

2. Add the user or group to the other access level following the steps provided in the [previous section](#add-a-user-or-group-to-an-access-level).

## Change the default access level

Make sure the default access level is the same as the access you're licensed for. When you set the default access level to Stakeholder, only the users who are given the Basic or a higher level can access more features than the Stakeholder level.

You can set an access level from its page. Choose **Set as default access level** as shown.

::: moniker range=">= azure-devops-2019"

![Screenshot of Stakeholder access level, set as default.](media/change-access-levels/set-stakeholder-as-default-2019.png)
::: moniker-end

::: moniker range="tfs-2018"

![Screenshot of Admin context, Control panel, Access levels, Stakeholder tab, set as default access level.](media/change-access-levels-set-default.png)
 
::: moniker-end

> [!IMPORTANT]  
> Service accounts get added to the default access level. If you set Stakeholder as the default access level, you must add the Azure DevOps service accounts to the Basic or an advanced access level group.

## Related articles

- [About access levels](access-levels.md)
- [Export a list of users and their access levels](export-users-audit-log.md)
- [Default permissions and access](permissions-access.md)  
- [Web portal navigation](../../project/navigation/index.md)
