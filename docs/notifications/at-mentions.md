---
title: Use @mentions in work items and pull requests 
titleSuffix: VSTS & TFS 
description: Alert team members using the @mention control in work items and pull requests 
ms.prod: devops
ms.technology: devops-collab
ms.assetid: 
toc: show
ms.manager: douge
ms.author: kaelli
author: KathrynEE
ms.topic: conceptual
ms.date: 03/01/2018
monikerRange: '>= tfs-2015'
---


# Use &#64;mentions

**VSTS | TFS 2018 | TFS 2017 | TFS 2015.2**

The **@mention** control allows you to quickly pull someone into a work item or pull request.


::: moniker range="tfs-2015"
> [!NOTE]  
> The **@mention** control is available from TFS 2015.2 and later versions.    
::: moniker-end

<a id="mention-person-id">  </a>
::: moniker range=">= tfs-2015 <= tfs-2018"
In order for team members to receive notifications, [you must configure an SMTP sever](/tfs/server/admin/setup-customize-alerts).  
::: moniker-end

When leaving a code comment in a pull request, you can type **@** to trigger the **@mention** identity picker. From the identity selector, you'll see a list of those people that you have you've recently mentioned. You can choose one of those names or type in the name of the person you are looking for to perform a directory search.  

To filter the list, enter the user name or alias until you've found a match.

![Web portal, Pull Request, Type a user name or email alias to locate a match](_img/at-mention-pr-type-name.png)  

::: moniker range=">= tfs-2018"
You can also use group mentions. Simply type the name of a team or a security group, choose the search icon and then select from the options listed.   
::: moniker-end

To **@mention** a user you've never selected previously, just continue typing to perform your search against the full directory.  

Names of those that you mention appear in <span style="color:#0099FF">blue text</span>. Choose the**@mention link name** to open the user's contact card, which can provide you additional context for why they were pulled into the conversation.  

![Web portal, At mention user contact card accessible](_img/at-mention-link-to-user-contact-card.png)  

Upon completion of your selection and text entry, your **@mention** user will receive an email alerting them about the mention.  

![Email sent to at-mention user account](_img/mail-to-at-mention-user.png)

When viewing their own mentioned names in conversations, users will notice that their own name is are highlighted in orange text.  

![Web portal, At mention of ones own name appears in orange text](_img/at-mention-link-view-of-own-name.png)  

You can use the **@mention** control in pull request discussions, commit comments, changeset comments, and shelveset comments. You can also use the **@mention** control in any text field on the work item form.

## Related articles

- [Work item form controls](../work/work-items/work-item-form-controls.md)  
- [Pull requests](../git/tutorial/pullrequest.md)

