---
title: Set dashboard permissions 
titleSuffix: Azure DevOps & TFS
description: How to set permissions and access levels to view, create, and edit charts and dashboards 
ms.custom: dashboards
ms.technology: devops-analytics
ms.prod: devops
ms.topic: conceptual
ms.manager: douge
ms.author: kaelli
author: KathrynEE
monikerRange: '>= tfs-2017'
ms.date: 11/19/2018
---

<a id="set-permissions">  </a>
# Set dashboard permissions    

[!INCLUDE [temp](../../_shared/version-tfs-2017-through-vsts.md)] 

As a team admin you can set dashboard permissions for your team. As a member of the Project Administrators group, you can set dashboard permissions for all teams. 

To learn more about adding and viewing dashboards, see [Add and manage dashboards](dashboards.md).   

> [!TIP]    
> If a user reports that they can't create or edit a team dashboard, and you've set the permissions to allow them to do so, check that they have been added as a member of the team. This includes adding them as a team member to the default project team. For details, see [Add users to a project or specific team](../../organizations/security/add-users-team-project.md). 

::: moniker range=">= azdevserver-2019"
By default, all team members have permissions to edit dashboards defined for the team. All other valid users of the project have view only permissions, except for administrators. You can change the edit permissions for specific team dashboards.
::: moniker-end

::: moniker range=">= tfs-2017  <= tfs-2018"  
By default, all team members have permissions to edit dashboards defined for the team. All other valid users of the project have view only permissions, except for administrators. You can change the view, edit, and manage permissions for all team dashboards for members of your team. 
::: moniker-end

[!INCLUDE [temp](../../_shared/new-navigation-7.md)] 

# [New navigation](#tab/new-nav)
::: moniker range=">= azdevserver-2019"

0. Open the **Mine** or **All** dashboards page, open the ![actions icon](../../_img/icons/actions-icon.png) actions menu for the dashboard, and choose the **Security** option. 

	> [!div class="mx-imgBorder"]  
	> ![Web portal, open Dashboards](_img/set-permissions/open-dashboard-security.png)

0. Add a user or group and set the permissions for that group. To learn more about adding groups or changing permissions, see [Add users to a project or specific team](../../organizations/security/add-users-team-project.md).
 
	Here we lock down the permissions for members of the Fabrikam team to edit the Analytics dashboard. 

 	> [!div class="mx-imgBorder"]  
	> ![Permissions for Analytics dashboard dialog](_img/set-permissions/dashboard-permission-dialog.png)

0. Choose **Save changes** and then **Close**. 

::: moniker-end 

::: moniker range=">= tfs-2017 <= tfs-2018"  
[!INCLUDE [temp](../../_shared/new-navigation-not-supported.md)]  
::: moniker-end  

# [Previous navigation](#tab/previous-nav)

::: moniker range="tfs-2017"
> [!NOTE]  
> The set dashboard permissions feature is available for TFS 2017.1 and later versions. For TFS 2017 and earlier versions, only team and project administrators can add and edit dashboards. 
::: moniker-end

::: moniker range="vsts"
0. Open the **Mine** or **All** dashboards page, open the ![actions icon](../../_img/icons/actions-icon.png) actions menu for the dashboard, and choose the **Security** option. 

	> [!div class="mx-imgBorder"]  
	> ![Web portal, open Dashboards](_img/set-permissions/open-dashboard-security.png)

0. Add a user or group and set the permissions for that group. To learn more about adding groups or changing permissions, see [Add users to a project or specific team](../../organizations/security/add-users-team-project.md).
 
	Here we lock down the permissions for members of the Fabrikam team to edit the Analytics dashboard. 

 	> [!div class="mx-imgBorder"]  
	> ![Permissions for Analytics dashboard dialog](_img/set-permissions/dashboard-permission-dialog.png)

0. Choose **Save changes** and then **Close**. 

::: moniker-end 

::: moniker range=">= tfs-2017  <= tfs-2018"  
1. To change the permissions for a dashboard, open the dashboard and then choose the ![configure icon](_img/icons/configure-icon.png) wrench icon for the dashboard.

	For example, here we open the Manage Dashboards dialog for the Fabrikam Fiber Web team's Test dashboard. 

	![Open Manage dashboards dialog](_img/dashboards-configure-ts.png) 

2. Choose the **Permissions** tab and check those checkboxes to grant or restrict permissions to your team members to edit and manage your team dashboards. The default settings, as shown in the illustration, provide all team members permissions to edit and manage dashboards.  

	::: moniker range="tfs-2018"  
	> [!div class="mx-imgBorder"]
	> ![Manage dashboards - permissions dialog, Azure DevOps and TFS 2018](_img/dashboards-permissions.png)   
	::: moniker-end  
	::: moniker range="tfs-2017"  
	Requires TFS 2017.1 or later version.   

	> [!div class="mx-imgBorder"]
	> ![Manage dashboards - permissions dialog, 2017.1](_img/dashboards-permissions-tfs.png) 
	::: moniker-end

3. Choose **Save** to save your changes and dismiss the Settings dialog. 

::: moniker-end

::: moniker range="azdevserver-2019"
[!INCLUDE [temp](../../_shared/previous-navigation-not-supported.md)] 
::: moniker-end

---

## Related articles

- [Add users to a project or specific team](../../organizations/security/add-users-team-project.md)
- [Add a team administrator](../../organizations/settings/add-team-administrator.md)
 
