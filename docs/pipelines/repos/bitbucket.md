---
title: Build BitBucket Cloud repositories
description: Using a BitBucket Cloud repository with Azure Pipelines
ms.topic: reference
ms.author: vijayma
author: vijayma
ms.date: 03/29/2020
monikerRange: azure-devops
---

# Build BitBucket Cloud repositories

[!INCLUDE [version-team-services](../includes/version-team-services.md)]

Azure Pipelines can automatically build and validate every pull request and commit to your BitBucket Cloud repository. This article describes how to configure the integration between BitBucket Cloud and Azure Pipelines.

BitBucket and Azure Pipelines are two independent services that integrate well together. Your BitBucket Cloud users do not automatically get access to Azure Pipelines. You must add them explicitly to Azure Pipelines.

## Choose a repository to build

# [YAML](#tab/yaml/)

You create a new pipeline by first selecting a BitBucket Cloud repository and then a YAML file in that repository. The repository in which the YAML file is present is called `self` repository. By default, this is the repository that your pipeline builds. 

You can later configure your pipeline to check out a different repository or multiple repositories. To learn how to do this, see [multi-repo checkout](multi-repo-checkout.md).

# [Classic](#tab/classic/)

You create a new pipeline by first selecting **BitBucket Cloud** for repository type, and then one of the repositories you have access to.

---

Azure Pipelines must be granted access to your repositories to trigger their builds, and fetch their code during builds.

There are 2 authentication types for granting Azure Pipelines access to your BitBucket Cloud repositories while creating a pipeline.

| Authentication type            | Pipelines run using              |
|--------------------------------|----------------------------------|
| 1. [OAuth](#oauth-authentication)           | Your personal BitBucket identity |
| 2. [Username and password](#password-authentication) | Your personal BitBucket identity |

### OAuth authentication

OAuth is the simplest authentication type to get started with for repositories in your BitBucket account. BitBucket status updates will be performed on behalf of your personal BitBucket identity. For pipelines to keep working, your repository access must remain active.

To use OAuth, login to BitBucket when promoted during pipeline creation. Then, click **Authorize** to authorize with OAuth. An OAuth connection will be saved in your Azure DevOps project for later use, as well as used in the pipeline being created.

### Password authentication

Builds and BitBucket status updates will be performed on behalf of your personal identity. For builds to keep working, your repository access must remain active.

To create a password connection, visit [Service connections](../library/service-endpoints.md) in your Azure DevOps project settings.
Create a new BitBucket service connection and provide the user name and password to connect to your BitBucket Cloud repository.

## CI triggers

Continuous integration (CI) triggers cause a pipeline to run whenever you push an update to the specified branches or you push  specified tags.

# [YAML](#tab/yaml/)

[!INCLUDE [ci-triggers](includes/ci-triggers1.md)]

> [!NOTE]
> For Bitbucket Cloud repos, using `branches` syntax is the only way to specify tag triggers. The `tags:` syntax is not supported for BitBucket.

[!INCLUDE [ci-triggers](includes/ci-triggers3.md)]

# [Classic](#tab/classic/)

[!INCLUDE [ci-triggers](includes/ci-triggers4.md)]

![ci trigger git branches](media/ci-trigger-git-branches-neweditor.png)

---

### Skipping CI for individual commits

You can also tell Azure Pipelines to skip running a pipeline that a commit would normally trigger. Just include `[skip ci]` in the commit message or description of the HEAD commit and Azure Pipelines will skip running CI. You can also use any of the variations below.

- `[skip ci]` or `[ci skip]`
- `skip-checks: true` or `skip-checks:true`
- `[skip azurepipelines]` or `[azurepipelines skip]`
- `[skip azpipelines]` or `[azpipelines skip]`
- `[skip azp]` or `[azp skip]`
- `***NO_CI***`

[!INCLUDE [ci-triggers](includes/ci-triggers5.md)]

## PR triggers

Pull request (PR) triggers cause a pipeline to run whenever a pull request is opened with one of the specified target branches,
or when updates are made to such a pull request.

# [YAML](#tab/yaml/)

### Branches

You can specify the target branches when validating your pull requests.
For example, to validate pull requests that
target `master` and `releases/*`, you can use the following `pr` trigger. 

```yaml
pr:
- master
- releases/*
```

This configuration starts a new run the first time a new pull request is created, and after every update made to the pull request.

You can specify the full name of the branch (for example, `master`) or a wildcard (for example, `releases/*`).

> [!NOTE]
> You cannot use [variables](../process/variables.md) in triggers, as variables are evaluated at runtime (after the trigger has fired).

> [!NOTE]
> If you use [templates](../process/templates.md) to author YAML files, then you can only specify triggers in the main YAML file for the pipeline. You cannot specify triggers in the template files.

BitBucket creates a new _ref_ when a pull request is created. The ref points to a _merge commit_, which is the merged code between the source and target branches of the pull request. The PR validation pipeline builds the commit this ref points to. This means that the YAML file that is used to run the pipeline is also a merge between the source and the target branch. As a result, the changes you make to the YAML file in source branch of the pull request can override the behavior defined by the YAML file in target branch.

If no `pr` triggers appear in your YAML file, pull request validations are automatically enabled for all 
branches, as if you wrote the following `pr` trigger. This configuration triggers a build when any 
pull request is created, and when commits come into the source branch of any active pull request.

```yaml
pr:
  branches:
    include:
    - '*'  # must quote since "*" is a YAML reserved character; we want a string
```

>[!IMPORTANT]
>When you specify a `pr` trigger, it replaces the default implicit `pr` trigger, and only pushes to branches that are explicitly configured to be included will trigger a pipeline.

For more complex triggers that need to exclude certain branches, you must use the full syntax as shown in the following example.

```yaml
# specific branch
pr:
  branches:
    include:
    - master
    - releases/*
    exclude:
    - releases/old*
```

### Paths

You can specify file paths to include or exclude. For example:

```yaml
# specific path
pr:
  branches:
    include:
    - master
    - releases/*
  paths:
    include:
    - docs/*
    exclude:
    - docs/README.md
```

> [!NOTE]
> You cannot use [variables](../process/variables.md) in paths, as variables are evaluated at runtime (after the trigger has fired).

### Multiple PR updates

You can specify whether additional updates to a PR should cancel in-progress validation runs for the same PR. The default is `true`.

```yaml
# auto cancel false
pr:
  autoCancel: false
  branches:
    include:
    - master
```

### Opting out of PR validation

You can opt out of pull request validation entirely by specifying `pr: none`.

```yaml
# no PR triggers
pr: none
```

For more information, see [PR trigger](../yaml-schema.md#pr-trigger) in the [YAML schema](../yaml-schema.md).

> [!NOTE]
> If your `pr` trigger isn't firing, ensure that you have not overridden YAML PR triggers in the UI.

# [Classic](#tab/classic/)

Select the **Pull request validation** trigger and check the **Enable pull request validation** check box to enable builds on pull requests.

You can specify branches to include and exclude.
Select a branch name from the drop-down menu and select **Include** or **Exclude** as appropriate.
For included branches, a build will be triggered on each push to a pull request targeting that branch.

---

## Pricing

Azure Pipelines is free for BitBucket Cloud repositories, with multiple free offerings available depending on whether your BitBucket repository is public or private.

If your BitBucket repository is open source, you can make your Azure DevOps project **public** so that anyone can view your pipeline's build results, logs, and test results without signing in. When users outside your organization fork your repository and submit pull requests, they can view the status of builds that automatically validate those pull requests. If both your BitBucket repository and your pipeline are public, you can run up to 10 parallel jobs in Azure Pipelines for free. These free jobs have a maximum timeout of 360 minutes (6 hours) each.

If either your BitBucket repository or your pipeline is private, we still provide a free tier. In this tier, you can run one free parallel job that can run up to 60 minutes each time until you've used 1800 minutes per month. When the free tier is no longer sufficient, you can purchase additional Microsoft-hosted parallel jobs.

Learn more about pricing based on [parallel jobs](../licensing/concurrent-jobs.md).

## Q & A

[!INCLUDE [qa](includes/qa1.md)]

[!INCLUDE [qa](includes/qa2.md)]

[!INCLUDE [qa](includes/qa3.md)]
