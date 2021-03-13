---
title: Move, change, or delete work items
titleSuffix: Azure Boards and TFS  
description: Guide to removing or deleting working items and test artifacts in Azure Boards and Team Foundation Server
keywords: backlogs
ms.global_help.title: Move, change, or delete work items
ms.technology: devops-agile
ms.prod: devops
ms.assetid: 306929CA-DB58-45E3-AD45-B774901789D3  
ms.manager: douge
ms.author: kaelli
author: KathrynEE
ms.topic: tutorial
ms.date: 09/27/2018
---


::: moniker range="vsts"
# Move, change, or delete work items 

[!INCLUDE [temp](../_shared/azure-boards.md)]

Often times you find that someone created a work item of the wrong work item type (WIT) or within an incorrect project. You can correct these issues for individual work items or bulk modify several work items. You can also remove work items added to your backlog or taskboard that aren't relevant anymore.  

::: moniker-end

::: moniker range=">= tfs-2013 <= tfs-2018"
# Delete or restore work items 

[!INCLUDE [temp](../../_shared/version-tfs-all-versions.md)]

You can remove work items added to your backlog or taskboard that aren't relevant anymore. Simply change the State to Remove, or delete the work item. You can perform operations on individual work items or bulk modify several work items. 

> [!TIP]    
You can't change the work item type for an existing work item, but you can [copy the work item and specify a new type](copy-clone-work-items.md#copy-clone). Also, if you have several work items with type changes you want to make, you can [export them using Excel](office/bulk-add-modify-work-items-excel.md), and then re-add them as a new type. 
::: moniker-end

In this topic you'll learn:  

::: moniker range="vsts"

> [!div class="checklist"]    
> * How to change the work item type of one or more work items   
> * How to move one or more work items to another project     
> * How to remove work items from the backlog by changing the State to Removed     
> * How to delete work items and test artifacts  
> * How to restore or permanently delete work items (web portal)
> * What permissions are required to delete work items and test artifacts    

::: moniker-end

::: moniker range=">= tfs-2017 <= tfs-2018"

>[!div class="checklist"]         
> * How to remove work items from the backlog by changing the State to Removed 
> * How to delete work items and test artifacts  
> * How to restore or permanently delete work items (web portal)    
> * How to permanently delete work items (command-line tool)  
> * What permissions are required to delete work items and test artifacts    

::: moniker-end


::: moniker range=">= tfs-2013 <= tfs-2015"

>[!div class="checklist"]         
> * How to remove work items from the backlog by changing the State to Removed     
> * How to permanently delete work items (command-line tool)  
> * What permissions are required to delete work items   

::: moniker-end

You only have access to those actions that are supported on your platform and for which you have permissions. If you are a member of the Contributors group (anyone who has been added as a team member) or Project Administrators groups, you have access to the following features. For a simplified view of permissions assigned to built-in groups, see [Permissions and access](../../organizations/security/permissions-access.md). 


You can access the following actions for which you have permissions. If you are a member of the Contributors group (anyone who has been added as a team member) or Project Administrators groups, you have access to the following features. For a simplified view of permissions assigned to built-in groups, see [Permissions and access](../../organizations/security/permissions-access.md). 

::: moniker range="vsts"

<table>
<tbody valign="top">
<tr>
<th width="50%">Contributors & Stakeholders</th>
<th width="50%">Project Administrators</th>
</tr>
<tr>
<td>
<ul>
<li>[Change work item type](#change-type)</li> 
<li>[Remove work items (change State)](#remove)</li>
<li>[Delete work items](#delete)</li>
<li>[Restore work items](#restore)</li>
</ul>
</td>
<td><ul>
<li>[Move a work item to another project](#move)</li>
<li>[Permanently delete work items](#restore) </li>
<li>[Permanently delete test artifacts](#delete-test)</li>
</ul>
</td>
</tr>
</tbody>
</table>


You can't change type, move work items, or delete/restore work items whose work item types support test management or that belong to the [Hidden Types Category](../work-items/agile-glossary.md#hidden-types). This includes all work items that track tests&mdash;such as test cases, shared steps, and shared parameters&mdash;code review requests and responses, and feedback requests and responses.   

::: moniker-end


::: moniker range=">= tfs-2017 <= tfs-2018"

<table>
<tbody valign="top">
<tr>
<th width="50%">Contributors & Stakeholders</th>
<th width="50%">Project Administrators</th>
</tr>
<tr>
<td>
<ul>
<li>[Remove work items (change State)](#remove)</li>
<li>[Delete work items](#delete)</li>
<li>[Restore work items](#restore)</li>
</ul>
</td>
<td>
<ul>
<li>[Permanently delete work items (web portal](#restore)</li>
<li>[Permanently delete work items (command-line)](#perm-delete)</li>
<li>[Permanently delete test artifacts](#delete-test)</li>
</ul>
</td>
</tr>
</tbody>
</table>


::: moniker-end

::: moniker range="tfs-2015"


<table>
<tbody valign="top">
<tr>
<th width="50%">Contributors & Stakeholders</th>
<th width="50%">Project Administrators</th>
</tr>
<tr>
<td>
<ul>
<li>[Remove work items (change State)](#remove)</li>
<li>[Delete work items](#delete)</li>
<li>[Restore work items](#restore)</li>
</ul>
</td>
<td><ul>
<li>[Permanently delete work items (web portal](#restore)</li>
<li>[Permanently delete work items (command-line)](#perm-delete)</li>
</ul>
</td>
</tr>
</tbody>
</table>


::: moniker-end

::: moniker range="tfs-2013"

- **Contributors & Stakeholders**: [Remove work items (change State)](#remove)  
- **Project Administrators**: [Permanently delete work items (command-line)](#perm-delete)  


::: moniker-end

::: moniker range=">= tfs-2015"

> [!TIP]  
> From the web portal, you can [multi-select several work items](bulk-modify-work-items.md) from a backlog or query results page and perform a bulk update using the associated feature.  To change, move, delete, or restore several work items at the same time, see [Bulk modify work items](bulk-modify-work-items.md). 

::: moniker-end

::: moniker range="vsts"
## Prerequisites  
  
* To change the work item type, delete, or remove work items, you must be a member of the Contributors group or be granted [Stakeholder access for a public project](../../organizations/security/access-levels.md#stakeholder-access) 
	Or, you must have your **View work items in this node**, and your **Edit work items in this node** permissions set to **Allow** 
* To move work items to another project, you must be a member of the Project Administrators group or have the **Move work items out of this project** permission set to **Allow**. The Contributors group does not have this permission set at the project-level by default.
* To delete work items, you must be a member of the Project Administrators group or have the **Delete work items in this project** permission set to Allow. The Contributors group has **Delete and restore work items** at the project-level set to **Allow** by default.

To set these permissions, see [Set permissions at the project-level or project collection-level](../../organizations/security/set-project-collection-level-permissions.md) 

::: moniker-end

::: moniker range=">= tfs-2017 <= tfs-2018"
## Prerequisites  
  
* To remove or delete work items, you must be a member of the Contributors group or be granted [Stakeholder access](/azure/devops/organizations/security/get-started-stakeholder) 
	Or, you must have your **View work items in this node**, and your **Edit work items in this node** permissions set to **Allow** 
* To delete work items, you must be a member of the Project Administrators group or have the **Delete work items in this project** permission set to Allow. The Contributors group has **Delete and restore work items** at the project-level set to **Allow** by default.

::: moniker-end


::: moniker range=">= tfs-2013 <= tfs-2015"
## Prerequisites  
  
* To remove work items, you must be a member of the Contributors group or be granted [Stakeholder access](/azure/devops/organizations/security/get-started-stakeholder) 
* Or, you must have your **View work items in this node**, and your **Edit work items in this node** permissions set to **Allow** 
* To delete work items, you must be a member of the Project Administrators group or have the **Delete work items in this project** permission set to Allow. For TFS 2015.1 and earlier versions, the Contributors group has **Delete work items in this project** at the project-level set to **Not set** by default. This setting causes the Contributors group to inherit the value from the closest parent that has it explicitly set.

::: moniker-end

To learn more, see [Set permissions and access for work tracking](/azure/devops/organizations/security/set-permissions-access-work-tracking). 


::: moniker range="vsts"

<a id="change-type"> </a>  
## Change the work item type 

> [!NOTE]  
>You can't change the work item type of work items associated with test management. Both Contributors and users assigned Stakeholder access can change the work item type.  

Changing the work item type refreshes the work item form with the fields defined for the type selected. For example, you can change a bug to a task and the form will refresh with the fields defined for a task. 

You can change a single work item or several [multi-selected work items](bulk-modify-work-items.md) to a new type. 


[!INCLUDE [temp](../../_shared/new-navigation.md)]  

# [New navigation](#tab/new-nav)

0. Open a work item, choose the ![ ](../_img/icons/actions-icon.png) actions icon, and select the ![ ](../_img/icons/change-type-icon.png) **Change type...** option. 

	![Work item form, Change work item type menu option](_img/move-change-delete/change-work-item-type.png)  

	Or, from the backlog or query results page, multi-select several work items whose type you want to change. You can select several work items of the same type or different type so long as you want to change them all to the same work item type.  

	Choose the ![ ](../_img/icons/actions-icon.png) actions icon,  and select the ![ ](../_img/icons/change-type-icon.png) **Change type...** option.     

	> [!div class="mx-imgBorder"]  
	> ![Backlog, multi-select, open actions menu, choose Change type option](_img/move-change-delete/change-type-new-nav.png)  

	> [!IMPORTANT]   
	>From the Query results page, the **Change type&hellip;** option becomes unavailable if you have checked the Query Editor's **Query across projects** checkbox. 

2. Select the type and optionally enter a comment.  

	![Change work item type dialog](_img/move-change-delete/change-work-item-type-dialog.png)    

	Comments are automatically added to the [Discussion control](../work-items/work-item-form-controls.md#discussion). 

3. Save the work item to complete the change.  
 
	> [!NOTE]     
	> The system automatically resets the State and Reason fields to the default initial values of the specified type.  

	From the Query results page, you must save all work items that you bulk-modified. When you bulk modify items from the backlog, they are automatically saved. Work items shown in bold text indicate that local changes have not yet been saved to the data store. The system automatically saves each work item. Refresh the page to reflect your changes.   

# [Previous navigation](#tab/previous-nav)

1. Select the ![Change project icon](../_img/icons/change-type-icon.png) Change type... option from the work item form's ![Action icon](../_img/icons/actions-icon.png) Actions menu.    

	![Work item form, Change work item type menu option](_img/move-change-delete/change-work-item-type.png)  

	Or, from the backlog or query results page, multi-select several work items whose type you want to change. You can select several work items of the same type or different type so long as you want to change them all to the same work item type. 

	Choose the ![ ](../_img/icons/actions-icon.png) actions icon,  and select the ![ ](../_img/icons/change-type-icon.png) **Change type...** option.     

	![Backlog, multi-select, open actions menu, choose Change type option](_img/move-change-delete/change-work-item-type-from-backlog.png)  

	> [!IMPORTANT]   
	>From the Query results page, the **Change type&hellip;** option becomes unavailable if you have checked the Query Editor's **Query across projects** checkbox. 

2. Select the type and optionally enter a comment.  

	![Change work item type dialog](_img/move-change-delete/change-work-item-type-dialog.png)    

	Comments are automatically added to the [Discussion control](../work-items/work-item-form-controls.md#discussion). 

3. Save the work item to complete the change.  
 
	> [!NOTE]     
	> The system automatically resets the State and Reason fields to the default initial values of the specified type.  

	From the Query results page, you must save all work items that you bulk-modified. When you bulk modify items from the backlog, they are automatically saved. Work items shown in bold text indicate that local changes have not yet been saved to the data store. The system automatically saves each work item. Refresh the page to reflect your changes.   

--- 

::: moniker-end

::: moniker range="vsts"

<a id="move"> </a>  
## Move a work item to another project  

When you discover that a work item belongs to a different project within your organization or collection, you can move it where it belongs. You can move a single work item or several [multi-selected work items](bulk-modify-work-items.md). 

You can only move work items from one project to another project within the organization or collection. You can't move work items associated with test management. To move work items to another project, you must be a member of the Project Administrators group or be [granted explicit permissions to move work items](../../organizations/security/set-permissions-access-work-tracking.md#move-delete-permissions).

1. Open the work item and choose the ![Move work item icon](../_img/icons/change-team-project-icon.png) **Move...** option from the work item form's ![Action icon](../_img/icons/actions-icon.png) Actions menu.    

	If you don't see the option, then you haven't been granted [permissions to move work items out of the project](../../organizations/security/set-permissions-access-work-tracking.md#move-delete-permissions).  

	Or, from the backlog or query results page, multi-select several work items whose type you want to change. You can select several work items of the same type or different type so long as you want to change them all to the same work item type. 

	Click ![actions icon](../_img/icons/actions-icon.png) to open the context menu of one of the selected work items, and choose the ![Move work item icon](../_img/icons/change-team-project-icon.png) **Move&hellip;** option. 

2. Select the destination project and optionally enter a comment.  

	![Move work item type dialog](_img/move-change-delete/move-work-item-dialog.png)
  
	Comments are automatically added to the [Discussion control](../work-items/work-item-form-controls.md#discussion) and an entry is made to the History control. Also, the system automatically resets the State and Reason fields to the default initial values for the work item type that you move.  

::: moniker-end


<a id="remove"> </a>  
## Remove work items

By changing the **State** of a work item to *Removed*, you effectively remove it from a backlog or board view (product, portfolio, and sprint backlogs, Kanban board, and taskboards).

> [!div class="mx-imgBorder"]  
> ![Change State to Removed](_img/move-change-delete/remove-state.png)  

To cause removed items to not show up in queries, you must add a clause that filters on the **State** field. 




<a id="delete"> </a> 
::: moniker range="vsts" 

## Delete work items  

Deleted work items won't appear in your backlogs, boards, or queries. Deleted items are moved to a recycle bin from which you can recover them if needed. To delete a test case, test plan, or test suite, or other test-related work item types, see [Delete test artifacts](#delete-test). 

# [New navigation](#tab/new-nav)

0. You can delete a work item from within the work item form, or by multi-selecting work items from a backlog or query results page.   

0. Confirm you want to actually delete the item(s).  

	> [!div class="mx-imgBorder"]
	> ![Confirm delete dialog](_img/move-change-delete/delete-work-items-dialog.png)  

0. You can also delete work items from your Kanban or taskboard. 
 
	> [!div class="mx-imgBorder"]  
	> ![Delete work item from Kanban board](_img/move-change-delete/delete-work-items-from-kanban-board.png)


# [Previous navigation](#tab/previous-nav)

0. You can delete a work item from within the work item form, or by multi-selecting work items from a backlog or query results page.   

0. Confirm you want to actually delete the item(s).  

	> [!div class="mx-imgBorder"]
	> ![Confirm delete dialog](_img/move-change-delete/delete-work-items-dialog.png)  

0. You can also delete work items from your Kanban or taskboard. 
 
	> [!div class="mx-imgBorder"]  
	> ![Delete work item from Kanban board](_img/move-change-delete/delete-work-items-from-kanban-board.png)

	Or, from a backlog or query results list, you can drag them to the ![Recycle bin](_img/recycle-bin-icon.png) (Recycle bin). You can only access the (Recycle bin) from **Work**. 

--- 

::: moniker-end

::: moniker range=">= tfs-2015 <= tfs-2018"
## Delete work items  
::: moniker-end  

::: moniker range="tfs-2015"  
> [!NOTE]  
> The **Delete and Recycle bin** features are available from TFS 2015.2 and later versions. 
::: moniker-end

::: moniker range=">= tfs-2015 <= tfs-2018"

Deleted work items won't appear in your backlogs, boards, or queries. Deleted items are moved to a recycle bin from which you can recover them if needed. To delete a test case, test plan, or test suite, or other test-related work item types, see [Delete test artifacts](#delete-test). 

1. You can delete a work item from within the work item form, or by multi-selecting work items from a backlog or query results page.   

2. Confirm you want to actually delete the item(s).  

	![Confirm delete dialog](_img/move-change-delete/delete-work-items-dialog-tfs.png)

	> [!NOTE]    
	> The Delete work items confirmation dialog for on-premises TFS may indicate there are auto-delete settings (disabled). There are no settings you can enable or disable. There is only a background process which permanently deletes work items that have been set to delete.   

3. Using multi-select from a backlog or query results list, you can delete several work items at once. 

4. You can also delete work items from your Kanban or taskboard. 
 
	![Delete work item from Kanban board](_img/move-change-delete/delete-work-items-from-kanban-board.png)

	Or, you can drag them to the ![Recycle bin](_img/recycle-bin-icon.png) (Recycle bin). You can only access the (Recycle bin) from **Work**. 

::: moniker-end

<a id="restore" />

::: moniker range="vsts"
## Restore or permanently delete work items   

You restore deleted work items from the web portal Recycle bin. 

# [New navigation](#tab/new-nav)

1. Choose **Boards>Work Items** and then choose the **Recycle bin**.  
 
	> [!div class="mx-imgBorder"]  
	> ![Boards>Work Items page, Open Recycle bin](_img/move-change-delete/open-recycle-bin-new-nav.png)

	A new browser tab opens with the query which lists work items added to the Recycle bin. 

2.	Select the items you want to restore and then choose **Restore**.  
 
	![Restore selected items](_img/move-change-delete/restore-from-recycle-bin.png) 

	> [!NOTE] 
	> You can't open work items that have been moved to the Recycle bin.  

	Optionally, you can choose to permanently delete the items.

	> [!NOTE] 
	> You'll only see the Permanently delete option if your [Permanently delete work items permission](../../organizations/security/set-permissions-access-work-tracking.md#move-delete-permissions) is set to Allow.  

3.	Confirm your selection. 

# [Previous navigation](#tab/previous-nav)

0. Choose **Work>Work Items** and then choose the **Recycle bin**.  
 
	> [!div class="mx-imgBorder"]  
	> ![Boards>Work Items page, Open Recycle bin](_img/move-change-delete/open-recycle-bin-new-nav.png)

	Or, from a backlog or query, open the Recycle bin.  
 
	![Open Recycle bin](_img/move-change-delete/open-recycle-bin.png)

	A new browser tab opens with the query which lists work items added to the Recycle bin. 

0.	Select the items you want to restore  and then choose **Restore**.  
 
	![Restore selected items](_img/move-change-delete/restore-from-recycle-bin.png) 

	Optionally, you can choose to permanently delete the items.

	> [!NOTE] 
	> You'll only see the Permanently delete option if your [Permanently delete work items permission](../../organizations/security/set-permissions-access-work-tracking.md#move-delete-permissions) is set to Allow.  

3.	Confirm your selection. 

---

::: moniker-end


::: moniker range=">= tfs-2015 <= tfs-2018"  
## Restore or permanently delete work items   

You restore deleted work items from the web portal Recycle bin. 
::: moniker-end  
::: moniker range="tfs-2015"  
> [!NOTE]  
> **Feature availability**: The Delete and Recycle bin features require TFS 2015.2 or later version.  
::: moniker-end  

::: moniker range=">= tfs-2015"
0. Choose **Work>Work Items** and then choose the **Recycle bin**.  
 
	> [!div class="mx-imgBorder"]  
	> ![Boards>Work Items page, Open Recycle bin](_img/move-change-delete/open-recycle-bin-new-nav.png)

	Or, from a backlog or query, open the Recycle bin.  
 
	![Open Recycle bin](_img/move-change-delete/open-recycle-bin.png)

	A new browser tab opens with the query which lists work items added to the Recycle bin. 

0.	Select the items you want to restore  and then choose **Restore**.  
 
	![Restore selected items](_img/move-change-delete/restore-from-recycle-bin.png) 

	Optionally, you can choose to permanently delete the items.

	> [!NOTE] 
	> You'll only see the Permanently delete option if your [Permanently delete work items permission](../../organizations/security/set-permissions-access-work-tracking.md#move-delete-permissions) is set to Allow.  

3.	Confirm your selection. 

::: moniker-end


<a id="perm-delete" />
::: moniker range=">= tfs-2013 <= tfs-2018"
## Permanently delete work items (command line)  

To permanently delete work items from the web portal, you must be a member of the Project Administrators group or be [granted explicit permissions to delete or restore work items](../../organizations/security/set-permissions-access-work-tracking.md#move-delete-permissions).
::: moniker-end
::: moniker range="tfs-2018"
Deleting work items from the command line is deprecated for TFS 2018.2 and later versions, and not supported for Azure Boards.
::: moniker-end

<a id="perm-delete"> </a>    
::: moniker range=">= tfs-2013 <= tfs-2018"

Use the ```witadmin destroywi``` command to permanently remove work items from the data store. A permanent delete means all information in the work tracking data store is deleted and cannot be restored nor reactivated. You must be a member of the Project Administrators group of have your **Edit project-level information** permission set to Allow. 
::: moniker-end  

::: moniker range="tfs-2018"  
0. Open a Command Prompt window where the latest version of Visual Studio is installed and change the directory to where the **witadmin.exe** tool has been installed. For example, you would change to the following directory for TFS 2018. (For other versions, see [Remove work items permanently (witadmin destroywi)](../../reference/witadmin/remove-work-items-permanently.md)).  

	`%programfiles(x86)%\Microsoft Visual Studio\2018\Professional\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\Team Explorer`  

	On a 32-bit edition of Windows, replace %programfiles(x86)% with %programfiles%.      

	The **witadmin** command-line tool installs with any version of Visual Studio or Team Explorer. You can access this tool by installing the [free version of Visual Studio Community](https://visualstudio.microsoft.com/downloads/).  

0.	To delete several work items, enter the server name and directory path to the collection. For example:   

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:12,15,23```
 
	To delete a single work item, simply enter the ID as shown:  

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:2003```    

::: moniker-end

::: moniker range="tfs-2017"  
0. Open a Command Prompt window where the latest version of Visual Studio is installed and change the directory to where the **witadmin.exe** tool has been installed. For example, you would change to the following directory for TFS 2017. (For other versions, see [Remove work items permanently (witadmin destroywi)](../../reference/witadmin/remove-work-items-permanently.md)).  

	`%programfiles(x86)%\Microsoft Visual Studio\2017\Professional\Common7\IDE\CommonExtensions\Microsoft\TeamFoundation\Team Explorer`  

	On a 32-bit edition of Windows, replace %programfiles(x86)% with %programfiles%.      

	The **witadmin** command-line tool installs with any version of Visual Studio or Team Explorer. You can access this tool by installing the [free version of Visual Studio Community](https://visualstudio.microsoft.com/downloads/).  

0.	To delete several work items, enter the server name and directory path to the collection. For example:   

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:12,15,23```
 
	To delete a single work item, simply enter the ID as shown:  

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:2003```    

::: moniker-end

::: moniker range="tfs-2015"  

0. Open a Command Prompt window where the latest version of Visual Studio is installed and change the directory to where the **witadmin.exe** tool has been installed. For example, you would change to the following directory for TFS 2015. (For other versions, see [Remove work items permanently (witadmin destroywi)](../../reference/witadmin/remove-work-items-permanently.md)).  

	`cd %programfiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE` 

	> [!NOTE] 
	>**Required permissions:** For TFS 2015.2 or later versions, you must have [Permanently delete work items permission set to Allow](../../organizations/security/set-permissions-access-work-tracking.md#move-delete-permissions). For TFS 2015.1 or earlier versions, you must be a member of the Project Administrators group of have Edit project-level information permissions set to Allow. 	

	On a 32-bit edition of Windows, replace %programfiles(x86)% with %programfiles%.      

	The **witadmin** command-line tool installs with any version of Visual Studio or Team Explorer. You can access this tool by installing the [free version of Visual Studio Community](https://visualstudio.microsoft.com/downloads/).  

0.	To delete several work items, enter the server name and directory path to the collection. For example:   

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:12,15,23```
 
	To delete a single work item, simply enter the ID as shown:  

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:2003```    

::: moniker-end


::: moniker range="tfs-2013"  

0. Open a Command Prompt window where the latest version of Visual Studio is installed and change the directory to where the **witadmin.exe** tool has been installed. For example, you would change to the following directory for TFS 2013. (For other versions, see [Remove work items permanently (witadmin destroywi)](../../reference/witadmin/remove-work-items-permanently.md)).  

	`cd %programfiles(x86)%\Microsoft Visual Studio 12.0\Common7\IDE` 

	On a 32-bit edition of Windows, replace %programfiles(x86)% with %programfiles%.      

	The **witadmin** command-line tool installs with any version of Visual Studio or Team Explorer. You can access this tool by installing the [free version of Visual Studio Community](https://visualstudio.microsoft.com/downloads/).  

2.	To delete several work items, enter the server name and directory path to the collection. For example:   

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:12,15,23```
 
	To delete a single work item, simply enter the ID as shown:  

	```witadmin destroywi /collection:http://TFSServerName:8080/tfs/DefaultCollection /id:2003```    

::: moniker-end


::: moniker range=">= tfs-2017"

<a id="delete-test"> </a> 
## Delete test artifacts  

You must be a member of the Project Administrators group or have the [**Delete test artifacts** permission set to **Allow**](../../organizations/security/set-permissions-access-work-tracking.md#delete-test-permissions). You must also have your [access level set to Advanced](../../organizations/security/change-access-levels.md), which provides access to the full Test feature set. Users with Basic access and with permissions to permanently delete work items and manage test artifacts can only delete orphaned test cases. That is, they can delete test cases created from **Work** that aren't linked to any test plans or test suites. 

To delete test artifacts, the following restrictions and operations apply:  
- Users with Basic access and with permissions to permanently delete work items and manage test artifacts can only delete orphaned test cases. That is, they can delete test cases created from **Work** that aren't linked to any test plans or test suites.  
- When you delete a test plan, test suite, test case, shared steps, or shared parameters, you not only permanently delete them, you also delete all associated test artifacts such as test results.  
- You can't bulk delete test artifacts. If test artifacts are part of a bulk selection to be deleted, all other work items except the test artifact(s) will get deleted.
::: moniker-end

::: moniker range=">= tfs-2017 <= tfs-2018" 
> [!IMPORTANT]   
> The permanently delete feature of test artifacts is available from the Test and Work for TFS 2017.1 and later versions. 
>
> We only support permanent deletion of test artifacts such as test plans, test suites, test cases, shared steps and shared parameters. Deleted test artifacts won't appear in the recycle bin and cannot be restored. Deletion of test artifacts not only deletes the selected test artifact but also all its associated child items such as child test suites, test points across all configurations, testers (the underlying test case work item doesn't get deleted), test results history, and other associated history.
::: moniker-end

::: moniker range=">= tfs-2017"
1. To delete a test case, open it from the web portal and choose the Permanently delete option from the actions menu. (Bulk deletion is not supported from a query results page.)     
 
	![Delete a test case and associated test artifacts from the web form](_img/move-change-delete/delete-test-artifacts-form.png)  

	> [!NOTE] 
	>You'll only see the **Permanently delete** option if you have the necessary permissions and access. 

2. Confirm you want to actually delete the item.  
  
	![Confirm delete of test artifacts](_img/move-change-delete/perm-delete-test-artifacts-dialog.png)  
 
3. You can also delete test plans and test suites directly from **Test**. 

	![Delete test plans and artifacts from Test pages](_img/move-change-delete/delete-test-plans.png)  

4.	To delete shared steps and shared parameters you need to first manually remove all references to them before you can delete them. 
	
	<img src="_img/delete-test-shared-steps-remove-link.png" alt="Delete shared steps from form" style="border: 2px solid #C3C3C3;" />
 
::: moniker-end


## Related articles   

::: moniker range=">= tfs-2018"  
- [View and add work items using the Work Items page](../work-items/view-add-work-items.md)  
- [Remove work items permanently (witadmin destroywi)](../../reference/witadmin/remove-work-items-permanently.md)
- [Create a test plan](../../test/create-a-test-plan.md)
- [Control how long to keep test results](../../test/how-long-to-keep-test-results.md) 
::: moniker-end

::: moniker range="tfs-2017"  
- [Add, update, and follow a work item](../backlogs/add-work-items.md)  
- [Remove work items permanently (witadmin destroywi)](../../reference/witadmin/remove-work-items-permanently.md)
::: moniker-end


::: moniker range="<= tfs-2015"  
- [Add and update a work item](../backlogs/add-work-items.md)  
- [Remove work items permanently (witadmin destroywi)](../../reference/witadmin/remove-work-items-permanently.md)
::: moniker-end



::: moniker range=">= tfs-2015"  

### Delete and restore actions performed under the hood  

#### Delete work items 
When you delete a work item, the following actions occur:  

- Generates a new revision of the work item  
- Updates the Changed By/Changed Date fields to support traceability  
- Preserves the work item completely, including all field assignments, attachments, tags, and links  
- Causes work item to become non-queryable and therefore can't appear in any work tracking experience, query result, or report  
- Updates charts accordingly, CFD, velocity, burndown and lightweight charts are updated to remove deleted work items  
- Removes WIT extensions  
- Preserves trend data except for the latest value 
- Removes the work item from the data warehouse/cube similar to as if it was permanently removed.  

#### Restore work items
When you restore a work item, the following actions occur:   

- Causes a new revision of the work item to be made  
- Updates the Changed By/Changed Date fields to support traceability   
- Becomes queryable  
- All fields remain unchanged  
- History contains 2 new revisions, one for deletion, and one for restore  
- Reattaches WIT extensions   
- Updates charts accordingly, CFD, velocity, burndown and lightweight charts are updated to include the restored work items  
- Restores trend data  
- Adds the work item back to the data warehouse/cube similar  
- Sets the area or iteration path fields to the root node if the previous area path or iteration paths were deleted.   

::: moniker-end

::: moniker range=">= tfs-2017"

#### Delete test artifacts
1.	Removes the deleted test artifact from the test case management (TCM) data store and deletes the underlying work item
2.	Runs a job to delete all the child items both from the TCM side and the underlying work items. This action may take time (up to a few minutes) depending on the number of artifacts to be deleted. 
3.	Causes all information in the WIT data store and TCM data store to be deleted and cannot be reactivated nor restored. 

::: moniker-end



