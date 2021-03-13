---
title: Parallel jobs 
titleSuffix: Azure DevOps
ms.custom: seodec18
description: Learn about parallel jobs in Azure Pipelines
ms.topic: conceptual
ms.assetid: FAFB2DE4-F462-4E9E-8312-4F343F2A35B8
ms.author: jukullam
ms.date: 03/11/2020
monikerRange: '>= tfs-2015'
---


# Run parallel jobs 

[!INCLUDE [version-tfs-2015-rtm](../includes/version-tfs-2015-rtm.md)]

::: moniker range="azure-devops"

For each _parallel job_ in Azure Pipelines, you can run a single job at a time in your organization. In Azure Pipelines, you can run parallel jobs on Microsoft-hosted infrastructure or your own (self-hosted) infrastructure.

::: moniker-end




::: moniker range="< azure-devops-2019"

This article describes the licensing model for Azure Pipelines in Team Foundation Server 2017 (TFS 2017) or newer. We don't charge you for Team Foundation Build (TFBuild) so long as you have a TFS Client Access License (CAL).

A TFS _parallel job_ gives you the ability to run a single release at a time in a project collection. You can keep hundreds or even thousands of release jobs in your collection. But, to run more than one release at a time, you need additional parallel jobs.

One free parallel job is included with every collection in a Team Foundation server. Every Visual Studio Enterprise subscriber in a Team Foundation server contributes one additional parallel job. 

You can buy additional private jobs from the Visual Studio Marketplace.

::: moniker-end

::: moniker range=">= azure-devops-2019 < azure-devops"

> [!IMPORTANT]
> Starting with Azure DevOps Server 2019, you do not have to pay for self-hosted concurrent jobs in releases. You are only limited by the number of agents that you have.

::: moniker-end

::: moniker range="= tfs-2015"

> Do I need parallel jobs in TFS 2015? Short answer: no. [More details](#tfs_before_2017)

::: moniker-end



::: moniker range="azure-devops"

## Microsoft-hosted CI/CD

If you want to run your jobs on machines that Microsoft manages, use _Microsoft-hosted parallel jobs_. Your jobs run on our pool of [Microsoft-hosted agents](../agents/hosted.md).

We provide a *free tier* of service by default in every organization:

- Public project: 10 free Microsoft-hosted parallel jobs that can run for up to 360 minutes (6 hours) each time, with no overall time limit per month.
- Private project: One free job that can run for up to 60 minutes each time, until you've used 1,800 minutes (30 hours) per month.
 
> [!TIP]
> If your pipeline exceeds the maximum job timeout, try splitting your pipeline 
> into multiple jobs. For more information on jobs, see 
> [Specify jobs in your pipeline](../process/phases.md).

When the free tier is no longer sufficient, you can pay for additional capacity per parallel job. Paid parallel jobs remove the monthly time limit and allow you to run each job for up to 360 minutes (6 hours). [Buy Microsoft-hosted parallel jobs](https://marketplace.visualstudio.com/items?itemName=ms.build-release-hosted-pipelines).

> [!NOTE] 
> When you purchase your first Microsoft-hosted parallel job, the number of parallel jobs you have in the organization is still 1. To be able to run two jobs concurrently, you will need to purchase two parallel jobs if you are currently on the free tier. The first purchase only removes the time limits on the first job.

## Self-hosted CI/CD

If you want Azure Pipelines to orchestrate your builds and releases, but use your own machines to run them, use _self-hosted parallel jobs_. You start by deploying our [self-hosted agents](../agents/agents.md) on your machines. You can register any number of these self-hosted agents in your organization. We charge based on the number of jobs you want to run at a time, not the number of agents registered. 

We provide a *free tier* of service by default in your organization:

- Public project: Unlimited parallel jobs.
- Private project: One self-hosted parallel job. Additionally, for each active Visual Studio Enterprise subscriber who is a member of your organization, you get one additional self-hosted parallel job.

When the free tier is no longer sufficient:

- Private project: You can pay for additional capacity per parallel job. [Buy self-hosted parallel jobs](https://marketplace.visualstudio.com/items?itemName=ms.build-release-private-pipelines).

There are no time limits on self-hosted jobs.




## How a parallel job is consumed by a pipeline

For example, consider an organization that has only one Microsoft-hosted parallel job. This job allows users in that organization to collectively run only one job at a time. When additional jobs are triggered, they are queued and will wait for the previous job to finish.

> [!NOTE] 
> If you use release pipelines or multi-stage YAML pipelines, then a run consumes a parallel job only when it's being actively deployed to a stage. While the release is waiting for an approval or a manual intervention, it does not consume a parallel job.


> [!NOTE] 
> When you run a [server job](../process/phases.md#server-jobs) or deploy to a [deployment group](../process/deployment-group-phases.md) using release pipelines, you don't consume any parallel jobs.


![Simple example of parallel jobs](media/concurrent-pipelines-vsts/concurrent-pipelines-simple-example.png)

1. FabrikamFiber CI Build 102 (master branch) starts first.
2. Deployment of FabrikamFiber Release 11 is triggered by completion of FabrikamFiber CI Build 102.
3. FabrikamFiber CI Build 101 (feature branch) is triggered. The build can't start yet because Release 11's deployment is active. So the build stays queued.
4. Release 11 waits for approvals. Fabrikam CI Build 101 starts because a release that's waiting for approvals does not consume a parallel job.
5. Release 11 is approved. It resumes only after Fabrikam CI Build 101 is completed.

## Relationship between jobs and parallel jobs

The term *job* can refer to multiple concepts, and its meaning depends on the context:

* When you define a pipeline, you can define it as a collection of [jobs](../process/phases.md). When a pipeline runs, you can run multiple jobs as part of that pipeline.

* Each job consumes a *parallel job* that runs on an agent. When there aren't enough parallel jobs available for your organization, the jobs are queued up and run one after the other.

## Determine how many parallel jobs you need

You can begin by seeing if the free tier offered in your organization is enough for your teams.
When you've reached the limit of 1,800 minutes per month for the free tier of Microsoft-hosted parallel jobs,
you can start by buying one parallel job to remove this monthly time limit before deciding to purchase more.

As the number of queued builds and releases exceeds the number of parallel jobs you have, your build and release queues will grow longer.
When you find the queue delays are too long, you can purchase additional parallel jobs as needed.

### Simple estimate

A simple rule of thumb: Estimate that you'll need one parallel job for every four to five users in your organization.

### Detailed estimate

In the following scenarios, you might need multiple parallel jobs:

* If you have multiple teams, and if each of them require CI, you'll likely need a parallel job for each team.

* If your CI trigger applies to multiple branches, you'll likely need a parallel job for each active branch.

* If you develop multiple applications by using one organization or server, you'll likely need additional parallel jobs: one to deploy each application at the same time.

## View available parallel jobs

1. Browse to **Organization settings** > **Pipelines** > **Retention and parallel jobs** > **Parallel jobs**.

   ![Location of parallel jobs in organization settings](media/concurrent-pipelines-vsts/control-panel-account-build-and-release-resource-limits.png)

   URL example: `https://{your_organization}/_admin/_buildQueue?_a=resourceLimits`

2. View the maximum number of parallel jobs that are available in your organization.

3. Select **View in-progress jobs** to display all the builds and releases that are actively consuming an available parallel job or that are queued waiting for a parallel job to be available.

## Sharing of parallel jobs across projects in a collection

Parallel jobs are purchased at the organization level, and they are shared by all projects in an organization. Currently, there isn't a way to partition or dedicate parallel job capacity to a specific project or agent pool. For example:

1. You purchase two parallel jobs in your organization.

1. You start two runs in the first project, and both the parallel jobs are consumed.

1. You start a run in the second project. That run won't start until one of the runs in your first project is completed.

## Q&A

### How do I qualify for the free tier of public projects?

We'll automatically apply the free tier limits for public projects if you meet both of these conditions:

* Your pipeline is part of an Azure Pipelines [public project](../../organizations/public/index.md). 
* Your pipeline builds a public repository from GitHub or from the same public project in your Azure DevOps organization.

### Are there limits on who can use Azure Pipelines?

You can have as many users as you want when you're using Azure Pipelines. There is no per-user charge for using Azure Pipelines. Users with both [basic and stakeholder access](https://visualstudio.microsoft.com/products/visual-studio-team-services-feature-matrix-vs) can author as many builds and releases as they want.

### Are there any limits on the number of builds and release pipelines that I can create?

No. You can create hundreds or even thousands of pipelines for no charge. You can register any number of self-hosted agents for no charge.

### As a Visual Studio Enterprise subscriber, do I get additional parallel jobs for TFS and Azure Pipelines?

Yes. Visual Studio Enterprise subscribers get [one parallel job in Team Foundation Server 2017 or later](concurrent-pipelines-tfs.md) and one self-hosted parallel job in each Azure DevOps Services organization where they are a member.

### What about the option to pay for hosted agents by the minute?

Some of our earlier customers are still on a per-minute plan for the hosted agents. In this plan, you pay $0.05/minute for the first 20 hours after the free tier, and $0.01/minute after 20 hours. Because of the following limitations in this plan, you might want to consider moving to the parallel jobs model:

- When you're using the per-minute plan, you can run only one job at a time.
- If you run builds for more than 14 paid hours in a month, the per-minute plan might be less cost-effective than the parallel jobs model.

### I use XAML build controllers with my organization. How am I charged for those?

You can register one XAML build controller for each self-hosted parallel job in your organization.
Your organization gets at least one free self-hosted parallel job, so you can register one XAML build controller for no additional charge.
For each additional XAML build controller, you'll need an additional self-hosted parallel job.


::: moniker-end


::: moniker range="< azure-devops-2019"

## How a parallel job is consumed

For example, a collection in a Team Foundation server has one parallel job. This allows users in that collection to run only one release at a time. When additional releases are triggered, they are queued and will wait for the previous one to complete.

A release requires a parallel job only when it is being actively deployed to a stage. Waiting for an approval does not consume a parallel job. However, waiting for a manual intervention in the middle of a deployment does consume a parallel job.

![Parallel jobs simple example](media/concurrent-pipelines-tfs/concurrent-pipelines-simple-example.png)

1. FabrikamFiber Release 10 is first to be deployed.
2. Deployment of FabrikamFiber Release 11 starts after Release 10's deployment is complete.
3. Release 12 is queued until Release 11's deployment is active.
4. Release 11 waits for an approval. Release 12's deployment starts because a release waiting for approvals does not consume a parallel job.
5. Even though Release 11 is approved, it resumes only after Release 12's deployment is completed.
6. Release 11 is waiting for manual intervention. Release 13 cannot start because the manual intervention state consumes a parallel job.

> Manual intervention does not consume a job in TFS 2017.1 and newer.

## Parallel processing within a single release

Parallel processing within a single release does not require additional parallel jobs. So long as you have enough agents, you can deploy to multiple stages in a release at the same time.

For example, suppose your collection has three parallel jobs. You can have more than three agents running at the same time to perform parallel operations within releases. For instance, notice below that four or five agents are actively running jobs from three parallel jobs.

![Parallel jobs with additional agents example](media/concurrent-pipelines-tfs/concurrent-pipelines-with-additional-agents-example.png)

## Parallel jobs in an organization

For example, here's an organization that has multiple  Team Foundation Servers. Two of their users have Visual Studio Enterprise subscriptions that they can use at the same time across all their on-premises servers and in each collection so long as the customer adds them as users to both the servers as explained below.

![Parallel jobs in an organization example](media/concurrent-pipelines-tfs/concurrent-pipelines-in-an-organization-example.png)

## Determine how many parallel jobs you need

You can begin by seeing if your teams can get by with the parallel jobs you've got by default. As the number of queued releases exceeds the number of parallel jobs you have, your release queues will grow longer. When you find the queue delays are too long, you can purchase additional parallel jobs as needed.

### Simple estimate

A simple rule of thumb: Estimate that you'll need one parallel job for every 10 users in your server.

### Detailed estimate

In the following scenarios you might need multiple parallel jobs:

* If you have multiple teams, if each of them require a CI build, and if each of the CI builds is configured to trigger a release, then you'll likely need a parallel job for each team.

* If you develop multiple applications in one collection, then you'll likely need additional parallel jobs: one to deploy each application at the same time.

## Use your Visual Studio Enterprise subscription benefit

Users who have Visual Studio Enterprise subscriptions are assigned to **VS Enterprise** access level in the Users hub of TFS instance. Each of these users contributes one additional parallel job to each collection. You can use this benefit on all Team Foundation Servers in your organization.

1. Browse to **Server settings**, **Access levels**.

   ![control-panel-server-vs-enterprise-access-levels](media/concurrent-pipelines-tfs/control-panel-server-vs-enterprise-access-levels.png)

   URL example: `http://{your_server}:8080/tfs/_admin/_licenses`

2. On the left side of the page, click **VS Enterprise**.

3. Add your users who have Visual Studio Enterprise subscriptions.

After you've added these users, additional licenses will appear on the resource limits page described below.



## Purchase additional parallel jobs

If you need to run more parallel releases, you can [buy additional private jobs from the Visual Studio marketplace](https://marketplace.visualstudio.com/items?itemName=ms.build-release-private-pipelines). Since there is no way to directly purchase parallel jobs from Marketplace for a TFS instance at present, you must first buy parallel jobs for an Azure DevOps organization. After you buy the private jobs for an Azure DevOps organization, you enter the number of purchased parallel jobs manually on the resource limits page described below.

## View and manage parallel jobs

1. Browse to **Collection settings**, **Pipelines**, **Resource limits**.

   ![control-panel-account-build-and-release-resource-limits](media/concurrent-pipelines-tfs/control-panel-account-build-and-release-resource-limits.png)

   URL example: `http://{your_server}:8080/tfs/DefaultCollection/_admin/_buildQueue?_a=resourceLimits`

2. View or edit the number of purchased parallel jobs.



## Q & A

### Who can use the system?

TFS users with a [TFS CAL](https://visualstudio.microsoft.com/team-services/tfs-pricing) can author as many releases as they want.

To approve releases, a TFS CAL is not necessary; any user with [stakeholder access](../..//organizations/security/access-levels.md) can approve or reject releases.

### Do I need parallel jobs to run builds on TFS?

No, on TFS you don't need parallel jobs to run builds. You can run as many builds as you want at the same time for no additional charge.

<a id="tfs_before_2017" />

### Do I need parallel jobs to manage releases in versions before TFS 2017?</h3>

No.

In TFS 2015, so long as your users have a TFS CAL, they can manage releases for no additional charge in trial mode. We called it "trial mode" to indicate that we would eventually charge for managing releases. Despite this label, we fully support managing releases in TFS 2015.

::: moniker-end