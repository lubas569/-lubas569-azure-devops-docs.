---
title: What is a public project? 
titleSuffix: VSTS Public Project
description: Understand the benefits of creating a public project, provide anonymous users ability to view your projects
ms.technology: devops-public-projects
ms.prod: devops-alm
ms.assetid: 
ms.manager: douge
ms.author: kaelli
author: KathrynEE
ms.topic: conceptual
ms.date: 07/02/2018
monikerRange: 'vsts'
---

# What is a public project?

[!INCLUDE [temp](_shared/version-public-projects.md)]  

A Visual Studio Team Services (VSTS) public project provides support to share code with others and to support continuous integration/continuous deployment (CI/CD) of open source software. Users aren't required to sign in to gain read-only access to many of the services.  

> [!NOTE]   
> VSTS public projects is in preview. To learn more about the road map, see the [VSTS Public Projects Limited Preview](https://blogs.msdn.microsoft.com/devops/2018/04/27/vsts-public-projects-limited-preview/) blog post.


## Public versus private projects 
VSTS projects provide a repository for source code and a place for a group of developers and teams to plan, track progress, and collaborate on building software solutions. One or more projects can be defined within a VSTS organization.  

Users that aren't signed into the service have read-only access to public projects on VSTS. Private projects, on the other hand, require users to be granted access to the project and signed in to access the services. 

## Supported services  

Non-members of a public project will have read-only access to a limited set of services, specifically: 
* [Browse the code base, download code, view commits, branches, and pull requests](browse-code-public.md)
* [View and filter work items](view-filter-work-items-public.md)
* [View a project page or dashboard](view-project-dashboard-public.md)
* [View the project Wiki](view-wiki-public.md) 
* Perform semantic search of the [code](code-search-public.md) or [work items](work-item-search-public.md) 

For additional information, see [Differences and limitations for non-members of a public project](feature-differences.md).

To contribute to a project by adding or modifying code, work items, pipelines, or more, a user must be [added as a member of the project](invite-users-public.md). For an overview of services contributors have access to, see [What do I get with VSTS?](../../user-guide/services.md)


## Get started with a public project  

**To get started:**
* [Create a public project](create-public-project.md)
* [Invite users to contribute to your public project](invite-users-public.md)
* [Define a README for your project](../../project/wiki/project-vision-status.md)

**To share code:**
* [Share your code](../../git/gitquickstart.md)

**To define pipelines:** 
* [Build OSS repositories](../../pipelines/build/ci-public.md?toc=/vsts/organizations/public/toc.json&bc=/vsts/organizations/public/breadcrumb/toc.json)

## Migrate a private project to public

As the creator of a project, you can [change your project's visibility from public to private and vice-versa](make-project-public.md). When you choose to make a project public, all of its contents are included. You cannot choose specific repositories, area paths, or build folders to keep private. 

Prior to changing the visibility of your private project, we recommend you review [Private-to-public migration checklist](migration-checklist.md).

## Related articles 

- [Differences and limitations for non-members of a public project](feature-differences.md)
- [Public projects glossary](glossary-public.md)


