---
title: FAQs for working in Excel
titleSuffix: Azure Boards
description: Find answers to frequently asked questions about working in Microsoft Excel to track work in Azure Boards
ms.technology: devops-agile
ms.author: kaelli
author: KathrynEE
ms.topic: conceptual
monikerRange: '>= tfs-2013'
ms.date: 11/22/2019
---


# FAQs: Work in Excel connected to Azure Boards 

[!INCLUDE [temp](../../includes/version-vsts-tfs-all-versions.md)]

Find answers to frequently asked questions when using Microsoft Excel to add or modify work items defined in Azure DevOps. 

[!INCLUDE [temp](../../../includes/version-selector-minimize.md)]

## Connect and versioning support

If you are having connection issues, make sure you meet the prerequisites as listed in [Bulk add or modify work items with Excel](bulk-add-modify-work-items-excel.md) and have reviewed the information in [Azure Boards and Office integration](track-work.md). 

### Q: What versions of Azure DevOps work with Excel?

**A:**  All versions from TFS 2013 through Azure DevOps Server 2019 and Azure DevOps Services/Azure Boards support integration with Microsoft Excel.

### Q: What do I need to use Excel to add or modify work items?

**A:**  You must get the Azure DevOps Office Integration add-in available from the [Downloads page, Other Tools and Frameworks](https://visualstudio.microsoft.com/downloads/#other-family). This add-in typically installs when you install any version of Visual Studio or Team Explorer. Also, you need to use Microsoft Excel 2010 or later version, including Microsoft Office Excel 365. 

[!INCLUDE [temp](../../includes/deprecate-project.md)]

Once you've installed the add-in, open Excel and look for the **Team** ribbon.

### Q: Can I use Excel on my Mac?

**A:** No. macOS isn't supported. Excel must be installed on the same computer where Visual Studio or Team Explorer are installed. These applications require Windows.

### Q: Can I open a query in Excel from the web portal?  

**A:** Yes. To open Excel from the web portal, install the [Azure DevOps Open in Excel](https://marketplace.visualstudio.com/items?itemName=blueprint.vsts-open-work-items-in-excel) Marketplace extension. Otherwise, you can open [Excel](bulk-add-modify-work-items-excel.md) and then open a query that you've created in the web portal or from Team Explorer. 

> [!div class="mx-imgBorder"]  
> ![Azure DevOps Open in Excel extension](media/excel/open-in-excel.png)

::: moniker range="azure-devops" 

### Q: Can I import or update work items without using Excel?  

**A:** Yes. You can do a bulk import of new work items or update existing work items without using Excel. See [Import work items](../../queries/import-work-items-from-csv.md).

::: moniker-end  

### Q: How do I connect an existing Excel workbook to Azure DevOps?  

**A:** See [Azure Boards and Office integration, Connect from Excel or Project](track-work.md#excel-project). 

### Q: How do I share an Excel workbook with others? 

If you want to share an Excel workbook that has work items listed within it, you may want to disconnect the connection to Azure DevOps to prevent accidental publishing of changes by others. You can disconnect the workbook, share it or work offline, and later reconnect the workbook. For details, see [Azure Boards and Office integration, Disconnect a document file from the network](track-work.md#WorkingOffline).


### Q: How do I connect when special protocols are in use on my network?  


**A:**  If your network uses TLS 1.1 or TLS 1.2 security protocol, then you may have network connection issues. To resolve these issues, see [Allowed address lists and network connections, Domain URLs to allow](../../../organizations/security/allow-list-ip-url.md#domain-urls-to-allow).


### Q: How do I disable the Team menu? 

**A:** If you want to disable the add-in, see [Add or remove an add-in](https://support.office.com/en-sg/article/Add-or-remove-add-ins-0af570c4-5cf3-4fa9-9b88-403625a0b460). 

## Work with linked work items

### Q: How do I publish to a tree? 

**A:** Follow the instructions provided in [Bulk add or modify work items with Excel, Add linked backlog items and tasks](bulk-add-modify-work-items-excel.md#tree-list) 

### Q: Can I bulk-edit link types other than parent-child links? 

**A:** No. Excel only supports adding and modifying parent-child or hierarchical links.   

To bulk edit links of other types, you can use the following clients: 

::: moniker range="azure-devops" 

-   Use [Project](../../backlogs/office/create-your-backlog-tasks-using-project.md) to edit parent-child and predecessor-successor link relationships.  
-   Use the web portal, to [map backlog items to portfolio backlog items](../../backlogs/organize-backlog.md) which creates parent-child links.  
-   Use either the web portal or Team Explorer, to modify parent-child links by [dragging items within a hierarchical backlog page](../../backlogs/organize-backlog.md#reparent) or within a tree query.
-  Use the [az boards work-item relation add](/cli/azure/ext/azure-devops/boards/work-item/relation) command.

::: moniker-end   

::: moniker range="azure-devops-2019" 

-   Use the web portal, to [map backlog items to portfolio backlog items](../../backlogs/organize-backlog.md) which creates parent-child links.  
-   Use either the web portal or Team Explorer, to modify parent-child links by [dragging items within a hierarchical backlog page](../../backlogs/organize-backlog.md#reparent) or within a tree query.

::: moniker-end  

::: moniker range=">= tfs-2015 <= tfs-2018" 

-   Use [Project](../../backlogs/office/create-your-backlog-tasks-using-project.md) to edit parent-child and predecessor-successor link relationships.  
-   Use the web portal, to [map backlog items to portfolio backlog items](../../backlogs/organize-backlog.md) which creates parent-child links.  
-   Use either the web portal or Team Explorer, to modify parent-child links by [dragging items within a hierarchical backlog page](../../backlogs/organize-backlog.md#reparent) or within a tree query.

::: moniker-end  

### Why does my direct-links query appear as a flat list in Excel?

**A:** When you open a direct-links query in Excel, the add-in converts the list to a flat list. While you can modify values for the fields and add work items, you can't view nor modify link relationships. 


## Work with test work items 

### Q: Can I bulk add or edit test cases?

**A:** No. You can't use Excel to export and import test case steps or other test artifacts. Instead, use the [grid view to bulk edit test cases supported via the web portal](../../../test/reference-qa.md#q-is-there-a-way-to-quickly-add-multiple-test-cases-at-the-same-time).  

> [!div class="mx-imgBorder"]  
> ![Test grid view](/azure/devops/test/media/create-test-cases/newtestcasesusinggrid.png)

## Publish and refresh 

### Q: How can I show additional fields?

**A:** If you start your worksheet with a **New List**, you'll see only a set of default field columns. You can add columns using the **Choose Columns** on the Team menu. 

If you start your worksheet from an existing query, you'll see all the column fields defined for the query. From there, you can add columns using the **Choose Columns** on the Team menu. However, your additions don't modify the underlying query.  


> [!div class="mx-imgBorder"]  
> ![Choose columns dialog](media/excel/choose-columns.png)



### Q: How do I resolve publishing issues? 

**A:** To resolve publishing errors, see one of these topics:   

- [Data conflicts](resolve-excel-data-conflicts-publish-refresh.md)  
- [Data validation errors](resolve-excel-data-validation-errors.md)  
- [Invalid links](resolve-excel-invalid-links-tree-list.md)  

### Can I delete work items from Excel?

::: moniker range="azure-devops" 
**A:** No. You can't delete work items from Excel. The only way to delete work items is from the web portal or the `az boards work-item delete` command-line tool. For details, see [Move, change, or delete work items](../../backlogs/remove-delete-work-items.md#delete-work-items).

::: moniker-end  

::: moniker range="< azure-devops" 

**A:** No. You can't delete work items from Excel. The only way to delete work items is from the web portal or the **witadmin** command-line tool. For details, see [Move, change, or delete work items](../../backlogs/remove-delete-work-items.md).

::: moniker-end 



## Use built-in Excel functions


### Q: Can I use multiple worksheets within Excel? 

**A:** Yes. Each worksheet in Excel can contain a different input list or query. However, all worksheets within the workbook must connect to the same project within an organization or project collection.  

To bulk add or modify work items in a different project, open a new Excel workbook. 


### Q: Can I use Excel cut and paste functions

**A:** Yes. You can use many Excel features, such as cut, paste, automatic fill, format, sort (flat list only), filter, and add formulas.  You can cut and paste rows to resequence items within a list and change link relationships among work items.

To drag a work item, select the work item or contiguous set of work items that you want to move, open the context menu and choose **Select**, **Table Row**, point to the border of the selection, and&mdash;when the pointer becomes a move pointer ![Move Pointer](media/bulk-modify-excel-pointer-icon.png)&mdash;drag the row to another location.

> [!TIP]  
> When you refresh the work item list, not all formats may be retained. For example, date formats are set by the server data store. Changes you make to a date format field are overwritten with the date format used by the server.  

### Q: How do I enable the Developer tab? 

**A:** See [Show the Developer Tab on the Ribbon](/visualstudio/vsto/how-to-show-the-developer-tab-on-the-ribbon).

## Related articles

::: moniker range="azure-devops" 
- [Bulk add or modify work items with Excel](bulk-add-modify-work-items-excel.md) 
- [Create your backlog](../../backlogs/create-your-backlog.md) 
- [Azure DevOps Office integration issues](tfs-office-integration-issues.md) 
- [Basic Excel tasks](https://support.office.com/article/basic-tasks-in-excel-dc775dd1-fa52-430f-9c3c-d998d1735fca) 
::: moniker-end  

::: moniker range="< azure-devops" 
- [Bulk add or modify work items with Excel](bulk-add-modify-work-items-excel.md) 
- [Create your backlog](../../backlogs/create-your-backlog.md)
- [Create your backlog and tasks using Project](create-your-backlog-tasks-using-project.md)  
- [Azure DevOps Office integration issues](tfs-office-integration-issues.md)
- [Basic Excel tasks](https://support.office.com/article/basic-tasks-in-excel-dc775dd1-fa52-430f-9c3c-d998d1735fca) 

::: moniker-end  

