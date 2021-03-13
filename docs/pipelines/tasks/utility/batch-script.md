---
title: Batch Script task
description: Execute .bat or .cmd scripts when building your code in Azure Pipelines and Team Foundation Server (TFS)
ms.topic: reference
ms.prod: devops
ms.technology: devops-cicd
ms.assetid: E60FC8AE-EDA7-4C1D-BDA5-CDC741FAD3E4
ms.manager: mijacobs
ms.custom: seodec18
ms.author: macoope
author: vtbassmatt
ms.date: 02/18/2020
monikerRange: '>= tfs-2015'
---

# Batch Script task

[!INCLUDE [temp](../../includes/version-tfs-2015-rtm.md)]

Use this task in a build or release pipeline to run a Windows .bat or .cmd script.
Optionally, allow it to permanently modify environment variables.

> [!NOTE]
> This task is not compatible with Windows containers.
> If you need to run a batch script on a Windows container, use the [command line task](command-line.md) instead.

::: moniker range="<= tfs-2018"

[!INCLUDE [temp](../../includes/concept-rename-note.md)]

::: moniker-end

::: moniker range="> tfs-2018"

## YAML snippet

[!INCLUDE [temp](../includes/yaml/BatchScriptV1.md)]

::: moniker-end

## Arguments

|Argument|Description|
|--- |--- |
|`filename`<br/>Path|(Required) Path of the cmd or bat script to execute. Should be fully qualified path or relative to the default working directory|
|`arguments`<br/>Arguments|(Optional) Specify arguments to pass to the script.|
|`modifyEnvironment`<br/>Modify environment|(Optional) Determines whether environment variable modifications will affect subsequent tasks <br/>Default value: `False`|
|`workingFolder`<br/>Working folder|(Optional) Current working directory when script is run. Defaults to the agent's default working directory|
|`failOnStandardError`<br/>Fail on Standard Error|(Optional) If this is true, this task will fail if any errors are written to the StandardError stream. <br/>Default value: `false`|

## Example

Create ```test.bat``` at the root of your repo:

```bat
@echo off
echo Hello World from %AGENT_NAME%.
echo My ID is %AGENT_ID%.
echo AGENT_WORKFOLDER contents:
@dir %AGENT_WORKFOLDER%
echo AGENT_BUILDDIRECTORY contents:
@dir %AGENT_BUILDDIRECTORY%
echo BUILD_SOURCESDIRECTORY contents:
@dir %BUILD_SOURCESDIRECTORY%
echo Over and out.
```

On the Build tab of a build pipeline, add this task:

<table>
   <tr>
      <td>

![](media/batch-script.png)

<br/>**Utility: Batch Script**</td>

<td>
<p>Run test.bat.</p>
<ul>
<li>Path: <code>test.bat</code></li>
</ul>
      </td>
</tr>
</table>

## Open source

This task is open source [on GitHub](https://github.com/Microsoft/azure-pipelines-tasks). Feedback and contributions are welcome.

## Q & A

<!-- BEGINSECTION class="md-qanda" -->

### Where can I learn Windows commands?

[An A-Z Index of the Windows CMD command line](https://ss64.com/nt/)

[!INCLUDE [include](../../includes/variable-set-in-script-qa.md)]

[!INCLUDE [temp](../includes/build-step-common-qa.md)]

[!INCLUDE [temp](../../includes/qa-agents.md)]

::: moniker range="< azure-devops"

[!INCLUDE [temp](../../includes/qa-versions.md)]

::: moniker-end

<!-- ENDSECTION -->
