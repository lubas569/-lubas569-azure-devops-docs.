---
title: Define organizations and projects in Azure DevOps
titleSuffix: Azure DevOps
description: Guidelines for defining your organizations and projects and what credentials you should use to create an organization 
ms.assetid: 
ms.prod: devops
ms.technology: devops-new-user
ms.manager: douge
ms.author: chcomley
author: chcomley
ms.date: 09/10/2018
ms.topic: conceptual
monikerRange: '>= tfs-2013'
---

# Define your Azure DevOps organizations and projects

[!INCLUDE [dev15-version-header](../_shared/dev15-version-header.md)]

In Azure DevOps, an organization is a mechanism for organizing and connecting groups of related projects. Examples are business divisions, regional divisions, or other organizational structure. You can choose one organization for your entire company, or separate organizations for specific business units, or an organization just for you.

For a larger company, you may want to create multiple organizations using different user accounts (most likely Azure Active Directory accounts). Think about what groups and users in your company share strategies and work, and group them into specific organizations. For example, the (fictional) Fabrikam company might create three Azure DevOps organizations: Fabrikam-Marketing, Fabrikam-Engineering, and Fabrikam-Sales. Each organization will have a separate URL, such as https://dev.azure.com/Fabrikam-Marketing, https://dev.azure.com/Fabrikam-Engineering, and https://dev.azure.com/Fabrikam-Sales. The organizations are all for the same company but are mostly isolated from each other.

## Choose your organization admin account type

You can create one or more Azure DevOps organizations. These organizations can be created by using a Microsoft account or with an Azure Active Directory (Azure AD)-backed account. This account provides the credentials to sign in to your new Azure DevOps organization at `https://dev.azure.com/{yourorganization}`.

### Microsoft account

Use your Microsoft account if you don't need to authenticate users for an organization with [Azure AD](https://azure.microsoft.com/documentation/articles/active-directory-whatis/). All users must sign in to your organization with a Microsoft account.

If you don’t have an Azure Active Directory instance, you can either [create one for free](https://portal.azure.com) from the Azure portal or use your Microsoft account to create an organization. An example is `johndoe@outlook.com`.

### Azure Active Directory-backed account

Use your work or school account managed by its Azure Active Directory instance. If you use Azure or Office 365, you might have one already. If you don't, learn how to [sign up for Azure Active Directory](https://azure.microsoft.com/documentation/articles/sign-up-organization/) to **automatically connect** your Azure DevOps organization to your Azure Active Directory. All users must be members in that directory to access your organization. To add users from other organizations, use [Azure AD B2B collaboration capabilities](/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).

## Define organizations

Organization settings are managed by administrators. As the creator of the organization, you're an administrator by default. You can access those settings by using the **Organization settings** button in the lower-left of your Azure DevOps portal.

![Open Organization settings](../_shared/_img/settings/open-admin-settings-vert.png)

For more information on configuring an organization, read [Create an organization](../organizations/accounts/create-organization-msa-or-work-student.md).

## Define projects

Each organization contains one or more projects. Each project contains a set of features: boards and backlogs for agile planning, pipelines for continuous integration and deployment, repos for version control and management of source code and artifacts, and continuous test integration throughout the life cycle.

Within an organization, you can have one large single project or multiple projects. Choose either of the following:

- Create a single project that contains many repos and teams.
- Create multiple projects, each containing its own set of teams, repos, builds, work items, and other elements.

Projects can be created or removed as you need. Think about the specific strategic work scoped to one of the organizations you created previously and who should have access to it. Use this information to name and create a project. This project will have a URL defined under the organization you created it in and can be accessed at `https://dev.azure.com/{organization-name}/{project-name}`.

Configure your project by visiting its URL and selecting the **Project settings** button at the lower-right of the page.

![Open project settings](../_shared/_img/settings/open-project-settings-vert-brn.png)

For more information on configuring a project, read [Create a project](../organizations/projects/create-project.md).

### Single project

You might have a large product or service that's managed by many teams. Those teams have tight interdependencies on each other across the product life cycle. You create a project and divide the work by using teams and area paths. This setup gives your teams visibility into each other’s work, so the organization stays aligned. Your teams use the same taxonomy for work item tracking, making it easier to communicate and stay consistent.

> [!RECOMMENDATION]  
 > When multiple teams work on the same product, we recommend that you have all teams on the same iteration schedule. This arrangement helps keep your teams aligned and delivering value on the same cadence.

A high volume of queries and boards can make it difficult to find what you're looking for. Depending on the architecture of your product, this difficulty can bleed into other areas such as builds, releases, and repos. To help alleviate this issue, make sure that you use good naming conventions and a simple folder structure. When you add a new repo to your project, it's a good time to reflect on your strategy and determine if that repo can be placed into its own project.

### Multiple projects

Most companies work on several products or services at a time. In those cases, we recommend having multiple projects. A project is best determined by how you ship the product. Having several projects shifts the administration burden and gives your teams more autonomy to manage the project as the team decides. It also provides greater control of security and access to assets across the different projects.

Having team independence with multiple projects creates some alignment challenges. If each project is using a different process or iteration schedule, it can make communication and collaboration difficult if the taxonomies aren't the same.

> [!CONSIDER THE FOLLOWING]
> -	Use the same process across all your projects.
> -	Enforce the same iteration schedules across all projects.

Azure DevOps provides cross-project experiences when it comes to managing work. You can easily create cross-project queries and move work items from one project to another.

If the projects stored in multiple repos work on independent schedules or processes, then splitting them into multiple projects might make the most sense. When you’re considering multiple projects, note that Git repo portability makes it easy to move a repo between projects and still retain full-fidelity commit history. Other history cannot be migrated between projects. Examples are push and pull request history.

## Try this next  

> [!div class="nextstepaction"]
> [Create an organization](../organizations/accounts/create-organization-msa-or-work-student.md)
> or
> [Create a project](../organizations/projects/create-project.md)

Or, after you've created a new organization and project in Azure DevOps, you can begin sharing your code with others: [Code with git](code-with-git.md).
