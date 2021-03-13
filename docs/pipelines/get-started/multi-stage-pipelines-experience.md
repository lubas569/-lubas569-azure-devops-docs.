---
title: Multi-stage pipelines user experience
ms.custom: seodec18
description: Learn how to navigate using the multi-stage pipelines user interface
ms.topic: reference
ms.prod: devops
ms.technology: devops-cicd
ms.manager: mijacobs
ms.author: sdanie
author: steved0x
ms.date: 12/09/2019
monikerRange: '> azure-devops-2019'
---

# Multi-stage pipelines user experience

The multi-stage pipelines experience brings improvements and ease of use to the Pipelines portal UI. This article shows you how to view and manage your pipelines using this new experience.

## Navigating pipelines

You can view and manage your pipelines by choosing **Pipelines** from the left-hand menu.

![View pipelines](media/pipelines-overview.png)

You can drill down and view pipeline details, run details, pipeline analytics, job details, logs, and more.

At the top of each page is a breadcrumb navigation bar. Select the different areas of the bar to navigate to different areas of the portal. The breadcrumb navigation is a convenient way to go back one or more steps. 

![Breadcrumb navigation](media/breadcrumb-navigation.png)

1. This area of the breadcrumb navigation shows you what page you're currently viewing. In this example, the page is the run summary for run number **20191209.3**.
2. One level up is a link to the [pipeline details](#view-pipeline-details) for that run.
3. The next level up is the [pipelines landing page](#pipelines-landing-page).
4. This link is to the **FabrikamFiber** project, which contains the pipeline for this run.
5. The root breadcrumb link is to the Azure DevOps **fabrikam-tailspin** organization, which contains the project that contains the pipeline.

Many pages also contain a back button that takes you to the previous page.

![Pipelines back button](media/pipelines-back-button.png)

## Pipelines landing page

From the pipelines landing page you can view pipelines and pipeline runs, create and import pipelines, manage security, and drill down into pipeline and run details.

Choose **Recent** to view recently run pipelines (the default view), or choose **All** to view all pipelines.

![View pipelines](media/view-pipelines.png)

Select a pipeline to manage that pipeline and view its runs. Select the build number for the last run to view the results of that build, select the branch name to view the branch for that run, or select the context menu to run the pipeline and perform other management actions.

![Pipeline context menu](media/pipelines-overview-pipeline-context-menu.png)

Select **Runs** to view all pipeline runs. You can optionally filter the displayed runs.

![View pipeline runs](media/all-pipeline-runs.png)

Select a pipeline run to view information about that run.

## View pipeline details

The details page for a pipeline allows you to view and manage that pipeline.

![Pipeline details](media/pipeline-overview.png)

### Runs

Select **Runs** to view the runs for that pipeline. You can optionally filter the displayed runs.

![Pipeline runs](media/pipeline-runs.png)

You can choose to **Retain** or **Delete** a run from the context menu. For more information on run retention, see [Build and release retention policies](../policies/retention.md).

![Pipeline run context menu](media/pipeline-run-context-menu.png)

### Branches

Select **Branches** to view the history or run for that branch. Hover over the **History** to view a summary for each run, and select a run to navigate to the details page for that run.

![Pipeline branches](media/pipeline-branches.png)

### Analytics

Select **Analytics** to view pipeline metrics such as pass rate and run duration. Choose **View full report** for more information on each metric.

![Pipeline analytics](media/pipeline-analytics.png)

## View pipeline run details

From the pipeline run summary you can view the status of your run, both while it is running and when it is complete.

![Pipeline run summary](media/pipeline-run-summary.png)

From the summary pane you can download artifacts, and navigate to linked commits, test results, and work items.

### Cancel and re-run a pipeline

If the pipeline is running, you can cancel it by choosing **Cancel**. If the run has completed, you can re-run the pipeline by choosing **Run new**.

![Cancel pipeline run](media/cancel-pipeline-run.png)

### Download logs

From the context menu you can download logs, add tags, edit the pipeline, and configure [retention](../policies/retention.md) for the run.

![Pipeline run summary context menu](media/pipeline-run-summary-context-menu.png)

### Jobs and stages

The jobs pane displays on overview of the status of your stages and jobs. This pane may have multiple tabs depending on whether your pipeline has stages and jobs, or just jobs. In this example the pipeline has two stages named **Build** and **Deploy**. You can drill down into the pipeline steps by choosing the job from either the **Stages** or **Jobs** pane.

![Pipeline jobs](media/pipeline-jobs-pane.png)

Choose a job to see the steps for that job.

![Pipeline tasks](media/pipeline-steps-list.png)

From the steps view, you can review the status and details of each step. From the context menu you can toggle timestamps or view a raw log of all steps in the pipeline.

![Pipeline tasks context menu](media/pipeline-steps-context-menu.png)

## Manage security

You can configure pipelines security on a project level from the context menu on the pipelines landing page, and on a pipeline level on the pipeline details page.

![Pipeline security](media/pipelines-context-menu.png)

To support security of your pipeline operations, you can add users to a built-in security group, set individual permissions for a user or group, or add users to pre-defined roles. You can manage security for for Azure Pipelines in the web portal, either from the user or admin context. For more information on configuring pipelines security, see [Pipeline permissions and security roles](../policies/permissions.md).







