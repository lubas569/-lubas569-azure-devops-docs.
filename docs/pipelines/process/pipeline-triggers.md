---
title: Configure pipeline triggers
description: Configure pipeline triggers
ms.topic: conceptual
ms.author: ashkir
author: ashkir
ms.date: 03/29/2020
monikerRange: ">=azure-devops-2019"
---

# Trigger one pipeline after another

Large products have several components that are dependent on each other.
These components are often independently built. When an upstream component (a library, for example) changes, the downstream dependencies have to be rebuilt and revalidated.

In situations like these, add a pipeline trigger to run your pipeline upon the successful completion of the **triggering pipeline**.

# [YAML](#tab/yaml)

To trigger a pipeline upon the completion of another, specify the triggering pipeline as a [pipeline resource](resources.md#resources-pipelines).

> [!NOTE]
> Previously, you may have navigated to the classic editor for your YAML pipeline and configured **build completion triggers** in the UI. While that model still works, it is no longer recommended. The recommended approach is to specify **pipeline triggers** directly within the YAML file. Build completion triggers as defined in the classic editor have various drawbacks, which have now been addressed in pipeline triggers. For instance, there is no way to trigger a pipeline on the same branch as that of the triggering pipeline using build completion triggers.


```yaml
# this is being defined in app-ci pipeline
resources:
  pipelines:
  - pipeline: securitylib   # Name of the pipeline resource
    source: security-lib-ci # Name of the triggering pipeline
    trigger: 
      branches:
      - releases/*
      - master
```

In this example, `pipeline: securitylib` specifies the name of the pipeline resource (used when referring to the pipeline resource from other parts of the pipeline, such as pipeline resource variables), 
and `source: security-lib-ci` specifies the name of the triggering pipeline. You can retrieve a pipeline's name from the Azure DevOps portal in several places, such as the [Pipelines landing page](../get-started/multi-stage-pipelines-experience.md#pipelines-landing-page). To configure the pipeline name 
setting, edit the YAML pipeline, choose **Triggers** from the settings menu, and navigate to the **YAML** pane.

![Pipeline settings](../repos/media/pipelines-options-for-git/yaml-pipeline-git-options-menu.png)

> [!NOTE] 
> If the triggering pipeline is in another Azure DevOps project, you must specify the
> project name using `project: OtherProjectName`. If the triggering pipeline is in another
> Azure DevOps organization, you must also create a 
> [service connection](../library/service-endpoints.md) to that project and reference it 
> in your pipeline resource. For more information, see [pipeline resource](resources.md#resources-pipelines).

In the above example, we have two pipelines - `app-ci` and `security-lib-ci`. We want the `app-ci` pipeline to run automatically every time a new version of the security library is built in master or a release branch.

Similar to CI triggers, you can specify the branches to include or exclude:

```yaml
resources:
  pipelines:
  - pipeline: securitylib
    source: security-lib-ci
    trigger: 
      branches:
        include: 
        - releases/*
        exclude:
        - releases/old*
```

If the triggering pipeline and the triggered pipeline use the same repository, then both the pipelines will run using the same commit when one triggers the other. This is helpful if your first pipeline builds the code, and the second pipeline tests it. However, if the two pipelines use different repositories, then the triggered pipeline will use the latest version of the code from its default branch.

When you specify both CI triggers and pipeline triggers, you can expect new runs to be started every time (a) an update is made to the repository and (b) a run of the upstream pipeline is completed. Consider an example of a pipeline `B` that depends on `A`. Let us also assume that both of these pipelines use the same repository for the source code, and that both of them also have CI triggers configured. When you push an update to the repository, then:

- A new run of `A` is started.
- At the same time, a new run of `B` is started. This run will consume the previously produced artifacts from `A`.
- As `A` completes, it will trigger another run of `B`.

To prevent triggering two runs of `B` in this example, you must remove its CI trigger or pipeline trigger.

# [Classic](#tab/classic)

In the classic editor, pipeline triggers are called **build completion triggers**. You can select any other build in the same project to be the triggering pipeline.

After you add a **build completion** trigger, select the **triggering build**. If the triggering build is sourced from a Git repo, you can also specify **branch filters**. If you want to use wildcard characters, then type the branch specification (for example, `features/modules/*`) and then press Enter.

> [!NOTE]
> Keep in mind that in some cases, a single [multi-job build](phases.md) could meet your needs.
> However, a build completion trigger is useful if your requirements include different configuration settings, options, or a different team to own the dependent pipeline.

### Download artifacts from the triggering build

In many cases, you'll want to download artifacts from the triggering build. To do this:

1. Edit your build pipeline.

1. Add the **Download Build Artifacts** task to one of your jobs under **Tasks**.

1. For **Download artifacts produced by**, select **Specific build**.

1. Select the team **Project** that contains the triggering build pipeline.

1. Select the triggering **Build pipeline**.

1. Select **When appropriate, download artifacts from the triggering build**.

1. Even though you specified that you want to download artifacts from the triggering build, you must still select a value for **Build**. The option you choose here determines which build will be the source of the artifacts whenever your triggered build is run because of any other reason than `BuildCompletion` (e.g. `Manual`, `IndividualCI`, `Schedule`, and so on).

1. Specify the **Artifact name** and make sure it matches the name of the artifact published by the triggering build.

1. Specify the **Destination directory** to which you want to download the artifacts. For example: `$(Build.BinariesDirectory)`

---
