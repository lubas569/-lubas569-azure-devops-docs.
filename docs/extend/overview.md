---
ms.technology: devops-ecosystem
title: Developing extensions for Azure DevOps Services and TFS
description: Overview of creating extensions for Azure DevOps Services
ms.assetid: bd7bd829-e80e-4234-849f-d4b273605a22
ms.topic: conceptual
monikerRange: '>= tfs-2017'
ms.author: chcomley
author: chcomley
ms.date: 10/24/2019
---

# What are extensions?

Extensions are simple add-ons that can be used to customize and extend your DevOps experience with Azure DevOps Services. 
They are written with standard technologies - HTML, JavaScript, CSS - and can be developed using your preferred development tools. 
They use our [RESTful API Library](/rest/api/vsts/) in order to easily interact with Azure DevOps Services and applications/services.
The [Visual Studio Marketplace](https://marketplace.visualstudio.com/azuredevops) is where extensions are published, 
where they can be kept privately for you and your team or shared with the millions of developers currently using Azure DevOps Services. 


> [!NOTE]
> This section covers developing custom extensions and service-hooks, to find information on installing extensions from the Marketplace, or buying Visual Studio Subscriptions, visit the [Marketplace documentation](../marketplace/overview.md).


## What makes up an extension?

<div align="center" style="padding-top:15px">
<img alt="Components of an extension" src="./media/extension-components.png" style="padding-bottom:20px">
</div>

- A [JSON manifest file](./develop/manifest.md) contains basic info about the extension.
- Discovery assets - the markdown and images that make up your extension's overview and aesthetics in the Marketplace. 
- Static files that contain the logic of your extension, including HTML, JS, and CSS files. Static files are only applicable to contribution-based extensions.

All of these are bundled up to make a Team Extensions Service Package (.vsix file) that is published to the Marketplace. From the Marketplace,
extensions can be installed directly by Azure DevOps Services users.

## What can you do with extensions?

There are dozens of places where you can add to the Azure DevOps Services user interface, and we're adding more every sprint. Learn about all of the places where you can add a hub in the [contributions reference](./reference/targets/overview.md).

- [Provide new Azure Pipelines tasks](./develop/add-build-task.md) that teams can use in their builds.
- Use [dashboard widgets](./develop/add-dashboard-widget.md) to get custom views within Azure DevOps Services. 
- Extend the [work item form](./develop/add-workitem-extension.md) with new tabs, sections, and actions.
- Create [your own hub](./develop/add-hub.md) to embed new capabilities within our Agile, code, build, and test experiences. 
- Develop [actions](./develop/add-action.md) that can be run on hubs, whether they're ours or ones you've created. 

## Build an extension for your delivery pipeline
You can develop an Azure DevOps Services extension for your continuous delivery pipeline as described in the following video.

<a href="https://www.youtube.com/watch?v=uzQFvYY0xiM&list=PLe14MLC-Nwy6saThL6NSv2rTtFNpkvfp3" target="_blank"><img src="media/develop-extension-video.png" alt="Develop Extensions" width="200" /></a>


## Extension building checklist

1. Familiarize yourself with an overview of our platform and what's possible with it
    1. [Azure DevOps extensions overview](https://azure.microsoft.com/services/devops/extend/)
2. Learn to build your first extension or check out our full set samples
    1. [Build your first extension](./get-started/node.md)
    2. [Samples](./develop/samples-overview.md)
3. Familiarize yourself with our RESTful APIs. If you're integrating from a 3rd party app or service, you'll also want to check out our Service Hooks
    1. [REST APIs](../integrate/index.md)
    2. [Service Hooks](../service-hooks/overview.md)
4. Once your extension is ready, you'll want to package it up, publish it to the Marketplace, and we hope you'll share it with the community!
    1. [Package, publish, and install your extension](./publish/overview.md)
    2. [Package and publish your integration with an external app or service](./publish/integration.md)
    3. [Share your work publicly with the entire community](./publish/publicize.md)

## Helpful links

* [Visual Studio Marketplace](https://marketplace.visualstudio.com/azuredevops)
* [Extension Publisher Page](https://marketplace.visualstudio.com/manage)
* [Visual Studio Partner Program](https://vspartner.com/)

## Next Steps

### Quickstarts

* [Write your first extension (Add a hub)](./get-started/node.md)

### Reference

* [Extension manifest reference](./develop/manifest.md)

