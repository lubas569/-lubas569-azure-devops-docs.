---
title: What access levels does Azure DevOps & TFS support?
titleSuffix: Azure DevOps Services & TFS
description: Understand how access levels are used to support stakeholder, basic, advanced, or VS Enterprise access  
ms.technology: devops-security
ms.prod: devops
ms.assetid: E2C63C7B-6273-41D7-BD14-BFB340DF8D65
ms.topic: conceptual
ms.manager: douge
ms.reviewer: jrice 
ms.author: kaelli
author: KathrynEE
ms.date: 07/07/2018
monikerRange: '>= tfs-2013'
---

# About access levels

[!INCLUDE [temp](../../_shared/version-vsts-tfs-all-versions.md)]

Access levels enable administrators the ability to provide their user base access to the features they need and only pay for those features. To connect and use the functions and features that TFS provides, users must be added to a group with the appropriate permissions. To use select web portal features, they must also belong to the access level that enables access to that feature.

Make sure to set each user's access level based on what you've purchased for that user. Basic access includes all Stakeholder features. Advanced and Visual Enterprise access levels include all Basic features. 

::: moniker range="vsts"
To add user accounts or groups to specific access levels, see [Manage users and access](../accounts/add-organization-users-from-user-hub.md).
::: moniker-end

::: moniker range=">= tfs-2013 <= tfs-2018"
To add user accounts or groups to specific access levels, see [Change access levels](change-access-levels.md). 
::: moniker-end


When you add a user or group to a team or project, they're automatically granted access to those features supported by the default access level, which is Basic. This provides most users all the features they need. For a simplified overview of the permissions assigned to the most common groups&#151;Readers, Contributors, and Project Administrators&#151;as well as the Stakeholder access group, see [Permissions and access](permissions-access.md).  

The systems employ these access levels:

- **Stakeholders**: provides partial access, can be assigned to unlimited users for free
- **Basic**: provides access to most features  
- **VS Enterprise** (TFS 2017.1 and later versions): provides access to premium features
- **Advanced** (TFS 2017 and earlier versions): provides access to premium features 

## Basic access

Assign **Basic** access to all users with a Visual Studio subscriptions and paid Azure DevOps users, including a TFS client access license (CAL). Basic provides access to most features, except for Test and other premium features.

![Basic access features](_img/access-levels-2017-basic.png)

<a id="stakeholder-access">  </a>
## Stakeholder access 

::: moniker range="vsts"
Assign **Stakeholder** access to an unlimited number of users for free.

Stakeholder access grants access to features differently depending on whether you're working from a private or a public project. To learn more about public projects, see [What is a public project?](../public/about-public-projects.md).  

| Service, application, or setting | Private project | Public project|
|------------|-----------------|---------------|
|**Dashboards** | Partial access | Full access | 
|**Wiki** | Full access | Full access | 
|**Boards (Work)** | Partial access | Full access | 
|**Repos (Code)**| No access | Full access | 
|**Pipelines (Build and Release)** | Full access | Full access | 
|**Test Plans (Test)** | No access | No access | 
|**Notifications** | Full access | Full access | 
|**Semantic search** | Full access | Full access | 
|**Project settings** | Partial access | Partial access | 
|**Organization settings** | Partial access | Partial access | 

::: moniker-end

::: moniker range=">= tfs-2013 <= tfs-2018"
Assign **Stakeholder** access to those users who need to enter bugs, view backlogs, boards, charts, and dashboards, but who don't have a TFS CAL. Stakeholders can also view releases and manage release approvals. Stakeholder access is free.

![Stakeholder access features](_img/access-levels-2017-stakeholder.png)  
::: moniker-end


<a id="feature-access">  </a>

::: moniker range="vsts"
### Stakeholder access to user features for private projects

The following features are available to Stakeholders from the web portal for private projects.

> [!div class="mx-tdBreakAll"]
> |Boards/Work  |Organization, Dashboards, Wiki, and Notifications|
> |-------------|----------|  
> |- [View, create, and modify work items](#create-work-item) <sup>1</sup><br/>- [View, add, and modify items on backlogs](#check-backlog) <sup>2</sup><br/>- [View, and modify items on sprint backlogs](../../boards/sprints/assign-work-sprint.md) <sup>2</sup><br/>- [View, and modify items on the task board](../../boards/sprints/task-board.md) <sup>2, 3</sup><br/>- [View, and modify items (Kanban)](../../boards/boards/kanban-basics.md)  <sup>2, 3</sup><br/>- [Add tasks to the checklist (Kanban)](../../boards/boards/add-task-checklists.md) <sup>2, 3</sup><br/>- [Follow changes made to work items](../../boards/work-items/follow-work-items.md) <br/>- [View the cumulative flow diagram](../../report/dashboards/cumulative-flow.md)<br/>- [View, create, and save queries](#query) <sup>4</sup> <br/>- [Submit, view, and change feedback responses](../../project/feedback/give-feedback.md)<br/>- [Change work item type](../../boards/backlogs/remove-delete-work-items.md)|- [Work across projects](../../project/navigation/work-across-projects.md)<br/>- [View project welcome pages](../../project/wiki/project-vision-status.md) <sup>5</sup><br/>- [View team dashboards](../../report/dashboards.md) <br/>- [Manage personal notifications](../../notifications/manage-personal-notifications.md)<br/>- [Invite users and assign licenses](../accounts/add-organization-users-from-user-hub.md) <sup>6</sup><br/>- [View wiki pages](../../project/wiki/filter-print-wiki.md) <sup>7</sup><br/><br/>**Pipelines/Build & Release**<br/> - All features <sup>8</sup>  |

**Notes:**

1. Can assign existing tags to work items, but not create new tags. 
2. Cannot change the backlog priority order (all items are added at the end of the backlog), assign items to an iteration using drag and drop, use the mapping pane or forecasting. 
3. Cannot move cards on the board to update status, set the values of fields shown on cards, or set or view  team capacity.
4. Can save queries under My Queries but cannot save under Shared Queries. 
5. Cannot view markdown README files defined for repositories. 
6. Can add users and assign licenses when added to the [Project Collection Administrators](/tfs/server/admin/add-administrator-tfs) group. To learn more, see [Manage users and access](../accounts/add-organization-users-from-user-hub.md).
7. Have read-only permissions to wiki pages. These permissions can't be changed. 
8. When the [**Free access to Pipelines Preview** feature is enabled](provide-stakeholder-pipeline-access.md), Stakeholders gain access to all **Pipeline (Build and Release)** features. If it is disabled, Stakeholders have access to [View releases](../../pipelines/release/approvals/index.md) and [Approve releases](../../pipelines/release/approvals/index.md) only.

### Stakeholder access to user features for public projects
From the web portal for private projects, Stakeholders have access to the following features in full, similar to those granted to users who were granted Basic access.  

> [!div class="mx-tdBreakAll"]
> |Boards/Work  |Organization, Dashboards, Wiki, and Notifications|
> |-------------|----------|  
> |- [View, create, and modify work items](#create-work-item) <br/>- [View, add, and modify items on backlogs](#check-backlog)<br/>- [View, and modify items on sprint backlogs](../../boards/sprints/assign-work-sprint.md)<br/>- [View, and modify items on the task board](../../boards/sprints/task-board.md)<br/>- [View, and modify items (Kanban)](../../boards/boards/kanban-basics.md)  <br/>- [Add tasks to the checklist (Kanban)](../../boards/boards/add-task-checklists.md)<br/>- [Follow changes made to work items](../../boards/work-items/follow-work-items.md) <br/>- [View the cumulative flow diagram](../../report/dashboards/cumulative-flow.md)<br/>- [View, create, and save queries](#query)<br/>- [Submit, view, and change feedback responses](../../project/feedback/give-feedback.md)<br/>- [Change work item type](../../boards/backlogs/remove-delete-work-items.md)|- [Work across projects](../../project/navigation/work-across-projects.md)<br/>- [View and edit project welcome pages](../../project/wiki/project-vision-status.md)<br/>- [View and manage team dashboards](../../report/dashboards.md)<br/>- [Manage personal notifications](../../notifications/manage-personal-notifications.md)<br/>- [Invite users and assign licenses](../accounts/add-organization-users-from-user-hub.md) <sup>1</sup><br/>- [View and edit wiki pages](../../project/wiki/filter-print-wiki.md) <br/><br/>**Pipelines/Build & Release**<br/> - All features <sup>2</sup>  |


**Notes:**

1. To add users and assign licenses, stakeholders must be added to the [Project Collection Administrators](/tfs/server/admin/add-administrator-tfs) group. To learn more, see [Manage users and access](../accounts/add-organization-users-from-user-hub.md).
2. When the [**Free access to Pipelines Preview** feature is enabled](provide-stakeholder-pipeline-access.md), Stakeholders gain access to all **Pipeline (Build and Release)** features. If it is disabled, Stakeholders have access to [View releases](../../pipelines/release/approvals/index.md) and [Approve releases](../../pipelines/release/approvals/index.md) only.


### Stakeholder access to administrative features

The following administrative features are granted or denied to users with Stakeholder access by default. Additional features are granted to Stakeholders in public projects. 

Items with a ![](_img/icons/checkmark.png) checkmark are granted permission by default. Items with an ![red x](_img/icons/delete-icon.png) indicate that permissions aren't granted and can't be granted to Stakeholders. Members of the Project Collection Administrators or Project Administrators group can grant or deny these permissions for Stakeholders. 

**Project settings**
| Permission | Private project | Public project|
|------------|-----------------|---------------|
|Bypass rules on work item updates | ![yes](_img/icons/checkmark.png)| ![yes](_img/icons/checkmark.png) |
|Change process of project | ![yes](_img/icons/checkmark.png)| ![yes](_img/icons/checkmark.png) |
|Create work item tag definition | ![no](_img/icons/delete-icon.png)| ![yes](_img/icons/checkmark.png) |
|Delete and restore work items | ![no](_img/icons/delete-icon.png)| ![yes](_img/icons/checkmark.png) |
|Move work items out of a project | ![no](_img/icons/delete-icon.png)| ![yes](_img/icons/checkmark.png) |
|Permanently delete work items | ![no](_img/icons/delete-icon.png)| ![yes](_img/icons/checkmark.png) |
|Suppress notifications for work item updates | ![yes](_img/icons/checkmark.png)| ![yes](_img/icons/checkmark.png) |
|Agile backlog tools management| ![no](_img/icons/delete-icon.png)| ![yes](_img/icons/checkmark.png) | 

The following permissions to manage area and iteration path settings are granted to Stakeholders by default in both private and public projects: 
- Create, delete, and edit child nodes 
- Edit work items in this node (area path only) 
- View work items in this node (area path only)  

The following permissions to manage organization settings are granted to Stakeholders in both private and public projects: 

- Administer process permissions 
- Create, delete, edit processes  
- Delete field from account 
- Add and manage users  

You can change the permissions granted to Stakeholders. See [Grant or restrict access to select features and functions](restrict-access.md).

::: moniker-end

::: moniker range="tfs-2018"
### Stakeholder access to user features
The following features are available to Stakeholders from the web portal.

> [!div class="mx-tdBreakAll"]
> |Work  |Dashboards, Wiki, and Notifications|
> |-------------|----------|  
> |- [View, create, and modify work items](#create-work-item) <sup>1</sup><br/>- [View, add, and modify items on backlogs](#check-backlog) <sup>2</sup><br/>- [View, and modify items on sprint backlogs](../../boards/sprints/assign-work-sprint.md) <sup>2</sup><br/>- [View, and modify items on the task board](../../boards/sprints/task-board.md) <sup>2, 3</sup><br/>- [View, and modify items (Kanban)](../../boards/boards/kanban-basics.md)  <sup>2, 3</sup><br/>- [Add tasks to the checklist (Kanban)](../../boards/boards/add-task-checklists.md) <sup>5, 3</sup><br/>- [Follow changes made to work items](../../boards/work-items/follow-work-items.md) <br/>- [View the cumulative flow diagram](../../report/dashboards/cumulative-flow.md)<br/>- [View, create, and save queries](#query) <sup>4</sup> <br/>- [Submit, view, and change feedback responses](../../project/feedback/give-feedback.md)<br/>- [Change work item type](../../boards/backlogs/remove-delete-work-items.md)|- [Work across projects](../../project/navigation/work-across-projects.md)<br/>- [View project welcome pages](../../project/wiki/project-vision-status.md) <sup>6</sup><br/>- [View team dashboards](../../report/dashboards.md) <br/>- [Manage personal notifications](../../notifications/manage-personal-notifications.md)<br/>- [View wiki pages](../../project/wiki/add-edit-wiki.md) <sup>7</sup><br/><br/>**Build & Release**<br/>- [View releases](../../pipelines/release/approvals/index.md) <sup>5</sup><br/>- [Approve a release](../../pipelines/release/approvals/index.md) |

**Notes:**

1. Stakeholders can assign existing tags to work items, but not create new tags.
2. Stakeholders cannot change the backlog priority order (all items are added at the end of the backlog), assign items to an iteration using drag and drop, use the mapping pane or forecasting.
3. Stakeholders cannot move cards on the board to update status, set the values of fields shown on cards, or set or view  team capacity.
4. Stakeholders can save queries under My Queries but cannot save under Shared Queries.
5. Stakeholders can only view and approve releases.
6. Stakeholders cannot view markdown README files defined for repositories.
7. Stakeholders have read-only permissions to wiki pages. These permissions can't be changed.

::: moniker-end


::: moniker range=">= tfs-2013 <= tfs-2017"
### Stakeholder access to user features
The following features are available to Stakeholders from the web portal of the listed TFS or later version. Those not annotated are available from all versions. To determine your platform or TFS version, see [Platform and version support](../../user-guide/provide-feedback.md#platform-version).   

> [!div class="mx-tdBreakAll"]
> |Work  |Dashboards and Notifications|
> |-------------|----------|  
> |- [View, create, and modify work items](#create-work-item) <sup>1</sup><br/>- [View, add, and modify items on backlogs](#check-backlog) <sup>2</sup><br/>- [View, and modify items on sprint backlogs](../../boards/sprints/assign-work-sprint.md) <sup>2</sup><br/>- [View, and modify items on the task board](../../boards/sprints/task-board.md) <sup>2, 3</sup><br/>- [View, and modify items (Kanban)](../../boards/boards/kanban-basics.md)  <sup>2, 3</sup><br/>- [Add tasks to the checklist (Kanban)](../../boards/boards/add-task-checklists.md) <sup>2, 3</sup> (TFS 2015.1)<br/>- [Follow changes made to work items](../../boards/work-items/follow-work-items.md) (TFS 2017)<br/>- [View the cumulative flow diagram](../../report/dashboards/cumulative-flow.md)<br/>- [View, create, and save queries](#query) <sup>4</sup><br/>- [Submit, view, and change feedback responses](../../project/feedback/give-feedback.md) |- [Work across projects](../../project/navigation/work-across-projects.md) (TFS 2017)<br/>- [View project welcome pages](../../project/wiki/project-vision-status.md) <sup>6</sup> (TFS 2017)<br/>- [View team dashboards](../../report/dashboards/dashboards.md) (TFS 2015)<br/>- [Manage personal notifications](../../notifications/manage-personal-notifications.md) (TFS 2017) <br/>- [Set personal alerts for changes to work items](../../boards/queries/alerts-and-notifications.md) (TFS 2013, 2015)<br/><br/>- **Build & Release**<br/>- [View releases](../../pipelines/release/approvals/index.md) <sup>5</sup> (TFS 2015.2)<br/>- [Approve a release](../../pipelines/release/approvals/index.md) <sup>5</sup> (TFS 2015.2) |

**Notes:**   
1. Stakeholders can assign existing tags to work items, but not create new tags.  
2. Stakeholders cannot change the backlog priority order (all items are added at the end of the backlog), assign items to an iteration using drag and drop, use the mapping pane or forecasting.
3. Stakeholders cannot move cards on the board to update status, set the values of fields shown on cards, or set or view  team capacity.
4. Stakeholders can save queries under My Queries but cannot save under Shared Queries.
5. Stakeholders can only view and approve releases.  
6. Stakeholders cannot view markdown README files defined for repositories.  

::: moniker-end


### Features stakeholders can't access 

If you need access to the following features&mdash;which support the daily work of product owners, team leads, developers, testers, and project administrators&mdash;you need to be have **Basic** access.  

> [!NOTE]   
> Stakeholders that choose a feature that's not available to them  receive an error message indicating that they don't have  permissions to complete the task.

::: moniker range="vsts"
**For Private projects:**
- Change the priority of an item within a backlog  
- Delete work items or move work items to another project
- Create shared queries, view charts, and modify the home page  
- View Delivery Plans (a Marketplace extension)    
- Access the full set of features under **Pipelines (Build and Release)**, **Repos (Code)** or **Test Plans (Test)**.  

**For Public projects:**
- View Delivery Plans (a Marketplace extension)    
- Access the full set of features under **Repos (Code)** or **Test Plans (Test)**. 
::: moniker-end


::: moniker range="tfs-2018"
- Change the priority of an item within a backlog  
- Delete work items or move work items to another project
- Create shared queries, view charts, and modify the home page  
- View Delivery Plans (a Marketplace extension)    
- Access the full set of features provided under **Code**, **Build and Release**, and **Test**   
::: moniker-end

::: moniker range=">= tfs-2013 <= tfs-2017"
- Change the priority of an item within a backlog  
- Delete work items or move work items to another project
- Create shared queries, view charts, and modify the home page  
- View Delivery Plans (a Marketplace extension)    
- Access the full set of features provided under **Code**, **Build and Release**, and **Test**   
- Participate in team rooms, which capture interactive, detailed conversations about the project.  
::: moniker-end


::: moniker range=">= tfs-2017 <= tfs-2018"
## VS Enterprise

For TFS 2017.2 and later versions, assign **VS Enterprise** to those users for whom you've purchased Visual Studio Enterprise. These include a TFS CAL plus the rights to access VS Enterprise features. (For users with MSDN Platforms subscriptions or Test Professional, assign the Basic access level and the Test Manager extension.) To learn more, see [Assign paid extension access to users](../../marketplace/how-to/assign-paid-extension-access.md). For example, for users with Visual Studio Test Professional or Visual Studio Enterprise, assign them [access to the Test Manager extension](../../marketplace/how-to/assign-paid-extension-access.md).

<!--- **TFS 2017.2** -->

<img src="_img/access-levels-2017-vs.png" alt="VS Enterprise access features" style="border: 1px solid #CCCCCC;" />  

::: moniker-end

::: moniker range=">= tfs-2013 <= tfs-2017"
## Advanced 

For TFS 2017 and earlier versions, you should assign the **Advanced** level to those users for whom you've purchased the full Test feature set. Here are the purchasing options:  
- Higher-level Visual Studio subscriptions: Visual Studio Test Professional, Visual Studio Enterprise, or MSDN Platforms subscriptions.
These include a TFS CAL plus the rights to access the full set of Test features.  
- A paid Azure DevOps user (which includes a TFS CAL) plus the [Test Manager extension](change-access-levels.md#test-manager). 

::: moniker-end

::: moniker range="tfs-2017"
For TFS 2017.2, Assign **Advanced** access to those users for whom you've purchased MSDN Platforms or Visual Studio Test Professional subscriptions. These include a TFS CAL plus the rights to access Test Manager. To learn more, see [Get extensions for TFS, Assign paid extension access to users](../../marketplace/how-to/assign-paid-extension-access.md).
	

**TFS 2017.2**

![TFS 2017.2, Advanced Access](_img/access-levels-2017-update2-vs-t.png)

**TFS 2017.1**

> [!NOTE]   
> With TFS 2017.1, the Advanced access level was temporarily disabled. Updating to TFS 2017.2 will re-enable it. If you are on TFS 2017.1 and have users with Visual Studio Test Professional or MSDN Platforms subscriptions, you should assign them Basic access level. In addition, you need to open **Users** for the project collections in which they are a member and [assign them the Test Manager extension](../../marketplace/assign-paid-extensions.md). To learn more, see [Buy access to TFS or TFS Test](../../billing/buy-access-tfs-test-hub.md). 

::: moniker-end

::: moniker range=">= tfs-2013 <= tfs-2017"
**TFS 2017, TFS 2015, TFS 2013**

![Advanced access features](_img/access-levels-2015-advanced.png)  

> [!NOTE]   
> The **Advanced** access level is deprecated for TFS 2017 and later versions of TFS. Use the **VS Enterprise** access level only for active Visual Studio Enterprise subscribers. For MSDN Platforms and Visual Studio Test Professional with MSDN subscribers needing access to **Test**, assign them to the **Advanced** access level and the Test Manager extension.  
 
::: moniker-end

 
<a id="test-manager"  >  </a>
::: moniker range="vsts"
## Test Plans/Test features and Marketplace extensions

Full access to **Test Plans/Test** features requires **VS Enterprise** access. Visual Studio Test Professional plus the test features in the web portal are managed through Azure DevOps, Azure billing services, and purchase of Test Manager extensions from the Marketplace.  

To learn more, see [Start free trials for paid Azure DevOps Services features and extensions](../billing/try-additional-features-vs.md). 

::: moniker-end

::: moniker range=">= tfs-2013 <= tfs-2018"
## Test features and Marketplace extensions

Full access to **Test Plans/Test** features requires **Advanced** (TFS 2015 or earlier versions) or **VS Enterprise** (TFS 2017 or later version) access. Visual Studio Test Professional plus the test features in the TFS web portal are managed through Azure DevOps, Azure billing services, and purchase of Test Manager extensions from the Marketplace.  

To learn how to grant access to an extension, see [Get extensions for TFS](../../marketplace/get-tfs-extensions.md).  

::: moniker-end

## What features can users access who are added to two different groups?
If a user belongs to a group that has **Basic** access and another group that has **VS Enterprise** access, the user has access to all features available through **VS Enterprise**, which is a superset of **Basic**.

::: moniker range=">= tfs-2013 <= tfs-2018"

## TFS Service account access  
[TFS service accounts](/tfs/tfs-server/admin/service-accounts-dependencies-tfs) are added to the default access level. If you make Stakeholder the default access level, you must set the TFS service accounts to Basic or Advanced/VS Enterprise access.  

Service accounts don't require a TFS CAL or other purchase.  

::: moniker-end

## Related articles  

::: moniker range="vsts"
- [Manage users and access](../accounts/add-organization-users-from-user-hub.md)
- [Export a list of users and their access levels](export-users-audit-log.md)
- [Default permissions and access](permissions-access.md)
::: moniker-end


::: moniker range=">= tfs-2013 <= tfs-2018"
- [Change access levels](change-access-levels.md)
- [Export a list of users and their access levels](export-users-audit-log.md)
- [Default permissions and access](permissions-access.md) 
::: moniker-end