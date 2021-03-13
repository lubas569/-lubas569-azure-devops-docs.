---
title: Azure Pipelines New User Guide - Key concepts
ms.custom: seodec18
description: Learn how Azure Pipelines works with your code and tools to automate build and deployment, and the key concepts behind it.
ms.topic: overview
ms.prod: devops
ms.technology: devops-cicd
ms.manager: mijacobs
ms.author: jukullam
author: juliakm
ms.date: 01/15/2020
monikerRange: 'azure-devops'
---

# Key concepts for new Azure Pipelines users

Learn about the key concepts and components that are used in Azure Pipelines. Understanding the basic terms and parts of Azure Pipelines helps you further explore how it can help you deliver better code more efficiently and reliably.


**Key concepts overview**

![key concepts graphic](media/key-concepts-overview.svg)

- A [trigger](#trigger) tells a Pipeline to run. 
- A [pipeline](#pipeline) is made up of one or more [stages](#stage). A pipeline can deploy to one or more [environments](#environment).
- A [stage](#stage) is a way of organizing [jobs](#job) in a pipeline and each stage can have one or more jobs.
- Each [job](#job) runs on one [agent](#agent). A job can also be agentless. 
- Each [agent](#agent) runs a job that contains one or more [steps](#step).
- A [step](#step) can be a [task](#task) or [script](#script) and is the smallest building block of a pipeline. 
- A [task](#task) is a pre-packaged script that performs an action, such as invoking a REST API or publishing a build artifact.
- An [artifact](#artifact) is a collection of files or packages published by a [run](#run).

**Azure Pipelines terms**

## Agent

When your build or deployment runs, the system begins one or more jobs. An agent is installable software that runs one job at a time.

For more in-depth information about the different types of agents and how to use them, see [Build and release agents](../agents/agents.md).

## Approvals

[Approvals](../process/approvals.md) define a set of validations required before a deployment can be performed. Manual approval is a common check performed to control deployments to production environments. When checks are configured on an environment, pipelines will stop before starting a stage that deploys to the environment until all the checks are completed successfully. 

## Artifact

An artifact is a collection of files or packages published by a run. Artifacts are made available to subsequent tasks, such as distribution or deployment. For more information, see [Artifacts in Azure Pipelines](../artifacts/artifacts-overview.md).

## Continuous delivery

Continuous delivery &#40;CD&#41; is a process by which code is built, tested, and deployed to one or more test and production stages. Deploying and testing in multiple stages helps drive quality. Continuous integration systems produce deployable artifacts, which includes infrastructure and apps. Automated release pipelines consume these artifacts to release new versions and fixes to existing systems. Monitoring and alerting systems run constantly to drive visibility into the entire CD process. This process ensures that  errors are caught often and early.

## Continuous integration

Continuous integration &#40;CI&#41; is the practice used by development teams to simplify the testing and building of code. CI helps to catch bugs or problems early in the development cycle, which makes them easier and faster to fix. Automated tests and builds are run as part of the CI process. The process can run on a set schedule, whenever code is pushed, or both. Items known as artifacts are produced from CI systems. They're used by the continuous delivery release pipelines to drive automatic deployments.

## Environment

An environment is a collection of resources, where you deploy your application. It can contain one or more virtual machines, containers, web apps, or any service that's used to host the application being developed. A pipeline might deploy the app to one or more environments after build is completed and tests are run.

## Job

A stage contains one or more jobs. Each job runs on an agent. A job represents an execution boundary of a set of steps. All of the steps run together on the same agent. For example, you might build two configurations - x86 and x64. In this case, you have one build stage and two jobs.

## Pipeline

A pipeline defines the continuous integration and deployment process for your app. It's made up of one or more stages. It can be thought of as a workflow that defines how your test, build, and deployment steps are run.

## Run

A run represents one execution of a pipeline. It collects the logs associated with running the steps and the results of running tests.

## Script

A script runs code as a step in your pipeline using command line, PowerShell, or Bash. You can write [cross-platform scripts](../scripts/cross-platform-scripting.md) for macOS, Linux, and Windows. Unlike a [task](#task), a script is custom code that is specific to your pipeline. 

## Stage

A stage is a logical boundary in the pipeline. It can be used to mark separation of concerns (e.g., Build, QA, and production). Each stage contains one or more jobs.

## Step

A step is the smallest building block of a pipeline. For example, a pipeline might consist of build and test steps. A step can either be a script or a task. A task is simply a pre-created script offered as a convenience to you. To view the available tasks, see the [Build and release tasks](../tasks/index.md) reference. For information on creating custom tasks, see [Create a custom task](../../extend/develop/add-build-task.md).

## Task

A [task](../process/tasks.md) is the building block for defining automation in a pipeline. A task is packaged script or procedure that has been abstracted with a set of inputs.

## Trigger

A trigger is something that's set up to tell the pipeline when to run. You can configure a pipeline to run upon a push to a repository, at scheduled times, or upon the completion of another build. All of these actions are known as triggers. For more information, see [build triggers](../build/triggers.md) and [release triggers](../release/triggers.md).

### About the authors
- [Dave Jarvis](https://github.com/DaveJarvis) contributed to the key concepts overview graphic.
