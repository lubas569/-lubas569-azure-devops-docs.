---
title: Define variables
ms.custom: seodec18
description: Variables are name-value pairs defined by you for use in a pipeline. You can use variables as inputs to tasks and in your scripts.
ms.topic: reference
ms.assetid: 4751564b-aa99-41a0-97e9-3ef0c0fce32a
ms.date: 02/27/2020

monikerRange: '>= tfs-2015'
---

# Define variables

[!INCLUDE [temp](../includes/concept-rename-note.md)]

Variables give you a convenient way to get key bits of data into various parts of the pipeline. The most common use of variables is to define a value that you can then use in your pipeline. All variables are stored as strings and are mutable. The value of a variable can change from run to run or job to job of your pipeline.

When you define the same variable in multiple places with the same name, the most locally scoped variable wins. So, a variable defined at the job level can override a variable set at the stage level. A variable defined at the stage level will override a variable set at the pipeline root level. A variable set in the pipeline root level will override a variable set in the Pipeline settings UI. 

::: moniker range="azure-devops"
Variables are different from [runtime parameters](runtime-parameters.md), which are typed and available during template parsing. 
::: moniker-end

## User-defined variables

When you define a variable, you can use [different syntaxes (macro, template expression, or runtime)](#understand-variable-syntax) and what syntax you use will determine where in the pipeline your variable will render. 

In YAML pipelines, you can set variables at the root, stage, and job level. You can also specify variables outside of a YAML pipeline in the UI. When you set a variable in the UI, that variable can be encrypted and set as secret. <a href="#secret-variables">Secret variables</a> are not automatically decrypted in YAML pipelines and need to be passed to your YAML file with `env:` or a variable at the root level.

User-defined variables can be [set as read-only](../security/inputs.md). 

You can [use a variable group](../library/variable-groups.md) to make variables available across multiple pipelines.  

You can use [templates](templates.md) to define variables that are used in multiple pipelines in one file. 

## System variables

In addition to user-defined variables, Azure Pipelines has system variables with predefined values. If you are using YAML or classic build pipelines, see [predefined variables](../build/variables.md) for a comprehensive list of system variables. 
If you are using classic release pipelines, see [release variables](../release/variables.md).

System variables are set with their current value when you run the pipeline. Some variables are set automatically. As a pipeline author or end user, you change the value of a system variable. 

System variables are read-only. 

## Environment variables

Environment variables are specific to the operating system you are using. They are injected into a pipeline in platform-specific ways. The format corresponds to how environment variables get formatted for your specific scripting platform. 

 On UNIX systems (MacOS and Linux), environment variables have the format `$NAME`. On Windows, the format is `%NAME%` for batch and `$env:NAME` in PowerShell.

System and user-defined variables also get injected as environment variables for your platform.  When variables are turned into environment variables, variable names become uppercase, and periods turn into underscores. For example, the variable name `any.variable` becomes the variable name `$ANY_VARIABLE`.

## Variable characters

User-defined variables can consist of letters, numbers, `.`, and `_` characters. Don't use variable prefixes that are reserved by the system. These are: `endpoint`, `input`, `secret`, and `securefile`. Any variable that begins with one of these strings (regardless of capitalization) will not be available to your tasks and scripts.

## Understand variable syntax

Azure Pipelines supports three different ways to reference variables: macro, template expression, and runtime expression. Each syntax can be used for a different purpose and has some limitations. 

Most documentation examples use macro syntax (`$(var)`). Variables with macro syntax are processed during runtime. Runtime happens [after template expansion](runs.md#process-the-pipeline). When the system encounters a macro expression, it replaces the expression with the contents of the variable. If there's no variable by that name, then the macro expression is left unchanged. For example, if `$(var)` can't be replaced, `$(var)` won't be replaced by anything. Macro variables are only expanded when they are used for a value, not as a keyword. Values appear on the right side of a pipeline definition. The following is valid: `key: $(value)`. The following isn't valid: `$(key): value`.

You can use template expression syntax to expand both [template parameters](../process/templates.md) and variables (`${{ variables.var }}`). Template variables are processed at compile time, and are replaced before runtime starts. Template variables silently coalesce to empty strings when a replacement value isn't found. Template expressions, unlike macro and runtime expressions, can appear as either keys (left side) or values (right side). The following is valid: `${{ variables.key }} : ${{ variables.value }}`.

You can use runtime expression syntax for variables that are expanded at runtime (`$[variables.var]`). Runtime expression variables silently coalesce to empty strings when a replacement value isn't found. Runtime expression variables are only expanded when they are used for a value, not as a keyword. Values appear on the right side of a pipeline definition. The following is valid: `key: $[variables.value]`. The following isn't valid: `$[variables.key]: value`.

|Syntax|Example|When is it processed?|Where does it expand in a pipeline definition?|How does it render when not found?|
|---|---|---|---|---|
|macro|`$(var)`|runtime|value (right side)|prints `$(var)`|
|template expression|`${{ variables.var }}`|compile time|key or value (left or right side)|empty string|
|runtime expression|`$[variables.var]`|runtime|value (right side)|empty string|

### What syntax should I use?

If you are defining a variable for a template or that has a static value, use a template expression.

If you are setting a variable that will be populated at runtime, use a macro expression. The exception to this is if you have a pipeline where it will cause a problem for your empty variable to print out. For example, if you have a conditional logic that relies on a variable having a specific value or no value. In that case, you should use a runtime expression. 

## Set variables in pipeline

#### [YAML](#tab/yaml/)
::: moniker range=">= azure-devops-2019"

In the most common case, you set the variables and use them within the YAML file. This allows you to track changes to the variable in your version control system. You can also define variables in the pipeline settings UI (see the Classic tab) and reference them in your YAML. 

Here's an example that shows how to set two variables, `configuration` and `platform`, and use them later in steps. To use a variable in a YAML statement, wrap it in `$()`. Variables can't be used to define a `repository` in a YAML statement. 

```yaml
# Set variables once
variables:
  configuration: debug
  platform: x64

steps:

# Use them once
- task: MSBuild@1
  inputs:
    solution: solution1.sln
    configuration: $(configuration) # Use the variable
    platform: $(platform)

# Use them again
- task: MSBuild@1
  inputs:
    solution: solution2.sln
    configuration: $(configuration) # Use the variable
    platform: $(platform)
```

### Variable scopes

In the YAML file, you can set a variable at various scopes:

- At the root level, to make it available to all jobs in the pipeline.
- At the stage level, to make it available only to a specific stage.
- At the job level, to make it available only to a specific job.

When a variable is defined at the top of a YAML, it will be available to all jobs and stages in the pipeline and is a global variable. Global variables defined in a YAML are not visible in the pipeline settings UI. 

Variables at the job level override variables at the root and stage level. Variables at the stage level override variables at the root level. 

```yaml
variables:
  global_variable: value    # this is available to all jobs

jobs:
- job: job1
  pool:
    vmImage: 'ubuntu-16.04'
  variables:
    job_variable1: value1    # this is only available in job1
  steps:
  - bash: echo $(global_variable)
  - bash: echo $(job_variable1)
  - bash: echo $JOB_VARIABLE1 # variables are available in the script environment too

- job: job2
  pool:
    vmImage: 'ubuntu-16.04'
  variables:
    job_variable2: value2    # this is only available in job2
  steps:
  - bash: echo $(global_variable)
  - bash: echo $(job_variable2)
  - bash: echo $GLOBAL_VARIABLE
```

### Specify variables

In the preceding examples, the `variables` keyword is followed by a list of key-value pairs.
The keys are the variable names and the values are the variable values.
There is another syntax, useful when you want to use [variable templates](templates.md) or [variable groups](../library/variable-groups.md).
In this alternate syntax, the `variables` keyword takes a list of variable specifiers.
The variable specifiers are `name` for a regular variable, `group` for a variable group, and `template` to include a variable template.
The following example demonstrates all three.

```yaml
variables:
# a regular variable
- name: myvariable
  value: myvalue
# a variable group
- group: myvariablegroup
# a reference to a variable template
- template: myvariabletemplate.yml
```

Learn more about [variable reuse with templates](templates.md). 

### Access variables through the environment

[!INCLUDE [temp](includes/access-variables-through-env.md)]

::: moniker-end
::: moniker range="< azure-devops-2019"
YAML is not supported in TFS.
::: moniker-end

#### [Classic](#tab/classic/)
You can set a variable for a build pipeline by following these steps:

1. Go to the **Pipelines** page, select the appropriate pipeline, and then select **Edit**.
1. Locate the **Variables** for this pipeline.
1. Add or update the variable.
1. To mark the variable as secret, select **Keep this value secret**.
1. Save the pipeline.

After the variable is set, you can use it as an input to a task or within the scripts in your pipeline.
To use a variable as an input to a task, wrap it in `$()`.

[!INCLUDE [temp](includes/access-variables-through-env.md)]

#### [Azure DevOps CLI](#tab/azure-devops-cli)

::: moniker range="azure-devops"  

Using the Azure DevOps CLI, you can create and update variables for the pipeline runs in your project. You can also delete the variables if you no longer need them.

[Create a variable](#create-variable) | [Update a variable](#update-variable) | [Delete a variable](#delete-variable) 

### Prerequisites

- You must have installed the Azure DevOps CLI extension as described in [Get started with Azure DevOps CLI](/azure/devops/cli/index).
- Sign into Azure DevOps using `az login`.
- For the examples in this article, set the default organization using `az devops configure --defaults organization=YourOrganizationURL`.

<a id="create-variable" />

### Create a variable

You can create variables in your pipeline with the [az pipelines variable create](/cli/azure/ext/azure-devops/pipelines/variable#ext-azure-devops-az-pipelines-variable-create) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli 
az pipelines variable create --name
                             [--allow-override {false, true}]
                             [--org]
                             [--pipeline-id]
                             [--pipeline-name]
                             [--project]
                             [--secret {false, true}]
                             [--value]
``` 

#### Parameters

- **name**: Required. Name of the variable.
- **allow-override**: Optional. Indicates whether the value can be set at queue time. Accepted values are *false* and *true*.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **pipeline-id**: Required if **pipeline-name** is not supplied. ID of the pipeline.
- **pipeline-name**: Required if **pipeline-id** is not supplied, but ignored if **pipeline-id** is supplied. Name of the pipeline.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **secret**: Optional. Indicates whether the variable's value is a secret. Accepted values are *false* and *true*.
- **value**: Required for non secret variable. Value of the variable. For secret variables, if **value** parameter is not provided, it is picked from environment variable prefixed with `AZURE_DEVOPS_EXT_PIPELINE_VAR_` or user is prompted to enter it via standard input. For example, a variable named **MySecret** can be input using the environment variable `AZURE_DEVOPS_EXT_PIPELINE_VAR_MySecret`.

#### Example

The following command creates a variable in **MyFirstProject** named **Configuration** with the value **platform** in the pipeline with ID **12**. It shows the result in table format.

```azurecli
az pipelines variable create --name Configuration --pipeline-id 12 --value platform --project MyFirstProject --output table

Name        Allow Override    Is Secret    Value
----------  ----------------  -----------  --------
Configuration  False             False        platform
```

<a id="update-variable" />

### Update a variable

You can update variables in your pipeline with the [az pipelines variable update](/cli/azure/ext/azure-devops/pipelines/variable#ext-azure-devops-az-pipelines-variable-update) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli 
az pipelines variable update --name
                             [--allow-override {false, true}]
                             [--new-name]
                             [--org]
                             [--pipeline-id]
                             [--pipeline-name]
                             [--project]
                             [--prompt-value {false, true}]
                             [--secret {false, true}]
                             [--value]
``` 

#### Parameters 

- **name**: Required. Original name of the variable.
- **allow-override**: Optional. Indicates whether the value can be set at queue time. Accepted values are *false* and *true*.
- **new-name**: Optional. Specify to change the name of the variable.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **pipeline-id**: Required if **pipeline-name** is not supplied. ID of the pipeline.
- **pipeline-name**: Required if **pipeline-id** is not supplied, but ignored if **pipeline-id** is supplied. Name of the pipeline.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **prompt-value**: Set to **true** to update the value of a secret variable using environment variable or prompt via standard input. Accepted values are *false* and *true*.
- **secret**: Indicates whether the variable's value is a secret. Accepted values are *false* and *true*.
- **value**: Updates the value of the variable. For secret variables, use the **prompt-value** parameter to be prompted to enter it via standard input. For non-interactive consoles, it can be picked from environment variable prefixed with `AZURE_DEVOPS_EXT_PIPELINE_VAR_`. For example, a variable named **MySecret** can be input using the environment variable `AZURE_DEVOPS_EXT_PIPELINE_VAR_MySecret`.

#### Example

The following command updates the **Configuration** variable with the new value **config.debug** in the pipeline with ID **12**. It specifies that the variable is not a **secret** and shows the result in table format.

```azurecli
az pipelines variable update --name Configuration --pipeline-id 12 --secret false --value config.debug --output table

Name           Allow Override    Is Secret    Value
-------------  ----------------  -----------  ------------
Configuration  False             False        config.debug
```

<a id="delete-variable" />

### Delete a variable

You can delete variables in your pipeline with the [az pipelines variable delete](/cli/azure/ext/azure-devops/pipelines/variable#ext-azure-devops-az-pipelines-variable-delete) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli 
az pipelines variable delete --name
                             [--org]
                             [--pipeline-id]
                             [--pipeline-name]
                             [--project]
                             [--yes]
``` 

#### Parameters 

- **name**: Required. Name of the variable you want to delete.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **pipeline-id**: Required if **pipeline-name** is not supplied. ID of the pipeline.
- **pipeline-name**: Required if **pipeline-id** is not supplied, but ignored if **pipeline-id** is supplied. Name of the pipeline.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **yes**: Optional. Does not prompt for confirmation.

#### Example

The following command deletes the **Configuration** variable from the pipeline with ID **12** and does not prompt for confirmation.

```azurecli
az pipelines variable delete --name Configuration --pipeline-id 12 --yes

Deleted variable 'Configuration' successfully.
```
::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *
<h2 id="secret-variables">Set secret variables</h2>

#### [YAML](#tab/yaml/)
::: moniker range=">= azure-devops-2019"

Don't set secret variables in your YAML file. Use the script's environment in order to pass secrets to the script. Operating systems often log commands for the processes that they run, and you wouldn't want the log to include a secret that you passed in as an input.

Instead, you should set secret variables in the pipeline settings UI for your pipeline. These variables are scoped to the pipeline in which you set them. You can also set [secret variables in variable groups](#reference-secret-variables-in-variable-groups). 

[!INCLUDE [temp](includes/set-secrets.md)]

The following example shows how to use a secret variable called `mySecret` in a PowerShell script.
Note that unlike a normal pipeline variable, there's no environment variable called `MYSECRET`.

```yaml
steps:

- powershell: |
    # Using an input-macro:
    Write-Host "This works: $(mySecret)"

    # Using the env var directly:
    Write-Host "This does not work: $env:MYSECRET"

    # Using the mapped env var:
    Write-Host "This works: $env:MY_MAPPED_ENV_VAR"    # Recommended
  env:
    MY_MAPPED_ENV_VAR: $(mySecret)
```

The output from the preceding script would look like this:

```text
This works: ***
This does not work:
This works: ***
```
This example shows how to use secret variables `$(vmsUser)` and `$(vmsAdminPass)` in an Azure file copy task. 

```yaml
variables:
  VMS_USER: $(vmsUser)
  VMS_PASS: $(vmsAdminPass)

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: AzureFileCopy@4
  inputs:
    SourcePath: 'my/path'
    azureSubscription: 'my-subscription'
    Destination: 'AzureVMs'
    storage: 'my-storage'
    resourceGroup: 'my-rg'
    vmsAdminUserName: $(VMS_USER)
    vmsAdminPassword: $(VMS_PASS)
```


### Reference secret variables in variable groups

This example shows how to reference a variable group in your YAML file, and also add variables within the YAML. There are two variables used from the variable group: `user` and `token`. The `token` variable is secret, and is mapped to the environment variable `$env:MY_MAPPED_TOKEN` so that it can be referenced in the YAML. 

This YAML makes a REST call to retrieve a list of releases, and outputs the result. 

```yaml
variables: 
- group: 'my-var-group' # variable group
- name: 'devopsAccount' # new variable defined in YAML
  value: 'contoso'
- name: 'projectName' # new variable defined in YAML
  value: 'contosoads'

steps:
- task: PowerShell@2
  inputs:
    targetType: 'inline'
    script: |
        # Encode the Personal Access Token (PAT)
        # $env:USER is a normal variable in the variable group
        # $env:MY_MAPPED_TOKEN is a mapped secret variable
        $base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $env:USER,$env:MY_MAPPED_TOKEN)))
        
        # Get a list of releases
        $uri = "https://vsrm.dev.azure.com/$(devopsAccount)/$(projectName)/_apis/release/releases?api-version=5.1"

        # Invoke the REST call
        $result = Invoke-RestMethod -Uri $uri -Method Get -ContentType "application/json" -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)}
        
        # Output releases in JSON
        Write-Host $result.value
  env:
    MY_MAPPED_TOKEN: $(token) # Maps the secret variable $(token) from my-var-group

```

> [!IMPORTANT]
> By default with GitHub repositories, secret variables associated with your pipeline aren't made available to pull request builds of forks. For more information, see [Validate contributions from forks](../repos/github.md#validate-contributions-from-forks).

::: moniker-end
::: moniker range="< azure-devops-2019"
YAML is not supported in TFS.
::: moniker-end

#### [Classic](#tab/classic/)
[!INCLUDE [temp](includes/set-secrets.md)]

Each task that needs to use the secret as an environment variable does remapping. If you want to use a secret variable called `mySecret` from a script, use the `Environment` section of the scripting task's input variables. Set the environment variable name to `MYSECRET`, and set the value to `$(mySecret)`.


> [!IMPORTANT]
> By default with GitHub repositories, secret variables associated with your pipeline aren't made available to pull request builds of forks. For more information, see [Validate contributions from forks](../repos/github.md#validate-contributions-from-forks).

#### [Azure DevOps CLI](#tab/azure-devops-cli/)

::: moniker range="azure-devops"  
To set secret variables using the Azure DevOps CLI, see [Create a variable](#create-variable) or [Update a variable](#update-variable).
::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *
## Share variables across pipelines

To share variables across multiple pipelines in your project, use the web interface. Under **Library**, use [variable groups](../library/variable-groups.md).

## Use output variables from tasks

Some tasks define output variables, which you can consume in downstream steps and jobs within the same stage. You can access variables across jobs by using [dependencies](expressions.md#dependencies). 

#### [YAML](#tab/yaml/)

For these examples, assume we have a task called `MyTask`, which sets an output variable called `MyVar`.

### Use outputs in the same job

```yaml
steps:
- task: MyTask@1  # this step generates the output variable
  name: ProduceVar  # because we're going to depend on it, we need to name the step
- script: echo $(ProduceVar.MyVar) # this step uses the output variable
```

### Use outputs in a different job

```yaml
jobs:
- job: A
  steps:
  - task: MyTask@1  # this step generates the output variable
    name: ProduceVar  # because we're going to depend on it, we need to name the step
- job: B
  dependsOn: A
  variables:
    # map the output variable from A into this job
    varFromA: $[ dependencies.A.outputs['ProduceVar.MyVar'] ]
  steps:
  - script: echo $(varFromA) # this step uses the mapped-in variable
```

#### [Classic](#tab/classic/)

### Use outputs in the same job

In the **Output variables** section, give the producing task a reference name.
Then, in a downstream step, you can use the form `$(<ReferenceName>.<VariableName>)` to refer to output variables. 

### Use outputs in a different job

You must use YAML to consume output variables in a different job.

#### [Azure DevOps CLI](#tab/azure-devops-cli/)

::: moniker range="azure-devops"  
There is no [**az pipelines**](/cli/azure/ext/azure-devops/pipelines) command that applies to using output variables from tasks. The Azure DevOps CLI commands are only valid for Azure DevOps Services (cloud service).
::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *

::: moniker range="azure-devops"  

## List variables

You can list all of the variables in your pipeline with the [az pipelines variable list](/cli/azure/ext/azure-devops/pipelines/variable#ext-azure-devops-az-pipelines-variable-list) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli 
az pipelines variable list [--org]
                           [--pipeline-id]
                           [--pipeline-name]
                           [--project]
``` 

#### Parameters 

- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **pipeline-id**: Required if **pipeline-name** is not supplied. ID of the pipeline.
- **pipeline-name**: Required if **pipeline-id** is not supplied, but ignored if **pipeline-id** is supplied. Name of the pipeline.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up by using `git config`.

#### Example

The following command lists all of the variables in the pipeline with ID **12** and shows the result in table format.

```azurecli
az pipelines variable list --pipeline-id 12 --output table

Name           Allow Override    Is Secret    Value
-------------  ----------------  -----------  ------------
MyVariable     False             False        platform
NextVariable   False             True         platform
Configuration  False             False        config.debug
```

::: moniker-end

## Set variables in scripts

A script in your pipeline can define a variable so that it can be consumed by one of the subsequent steps in the pipeline. All variables set by this method are treated as strings. To set a variable from a script, you use a command syntax and print to stdout.

#### [YAML](#tab/yaml/)
::: moniker range=">= azure-devops-2019"

### Set a job-scoped variable from a script

To set a variable from a script, you use the `task.setvariable` logging command.
This doesn't update the environment variables, but it does make the new
variable available to downstream steps within the same job.

```yaml
steps:

# Create a variable
- bash: |
    echo "##vso[task.setvariable variable=sauce]crushed tomatoes"

# Use the variable
# "$(sauce)" is replaced by the contents of the `sauce` variable by Azure Pipelines
# before handing the body of the script to the shell.
- bash: |
    echo my pipeline variable is $(sauce)
```

Subsequent steps will also have the pipeline variable added to their environment.

```yaml
steps:

# Create a variable
# Note that this does not update the environment of the current script.
- bash: |
    echo "##vso[task.setvariable variable=sauce]crushed tomatoes"

# An environment variable called `SAUCE` has been added to all downstream steps
- bash: |
    echo "my environment variable is $SAUCE"
- pwsh: |
    Write-Host "my environment variable is $env:SAUCE"
```

### Set a multi-job output variable

If you want to make a variable available to future jobs, you must mark it as
an output variable by using `isOutput=true`. Then you can map it into future jobs by using the `$[]` syntax and including the step name which set the variable. Multi-job output variables only work for jobs in the same stage. 
When you create a multi-job output variable, you should assign the expression to a variable. In this YAML, `$[ dependencies.A.outputs['setvarStep.myOutputVar'] ]` is assigned to the variable `$(myVarFromJobA)`. 

```yaml
jobs:

# Set an output variable from job A
- job: A
  pool:
    vmImage: 'vs2017-win2016'
  steps:
  - powershell: echo "##vso[task.setvariable variable=myOutputVar;isOutput=true]this is the value"
    name: setvarStep
  - script: echo $(setvarStep.myOutputVar)
    name: echovar

# Map the variable into job B
- job: B
  dependsOn: A
  pool:
    vmImage: 'ubuntu-16.04'
  variables:
    myVarFromJobA: $[ dependencies.A.outputs['setvarStep.myOutputVar'] ]  # map in the variable
                                                                          # remember, expressions require single quotes
  steps:
  - script: echo $(myVarFromJobA)
    name: echovar
```

If you're setting a variable from a [matrix](phases.md?tab=yaml#parallelexec)
or [slice](phases.md?tab=yaml#slicing), then, to reference the variable when you access it from a downstream job,
you must include:

- The name of the job.
- The step.

```yaml
jobs:

# Set an output variable from a job with a matrix
- job: A
  pool:
    vmImage: 'ubuntu-16.04'
  strategy:
    maxParallel: 2
    matrix:
      debugJob:
        configuration: debug
        platform: x64
      releaseJob:
        configuration: release
        platform: x64
  steps:
  - script: echo "##vso[task.setvariable variable=myOutputVar;isOutput=true]this is the $(configuration) value"
    name: setvarStep
  - script: echo $(setvarStep.myOutputVar)
    name: echovar

# Map the variable from the debug job
- job: B
  dependsOn: A
  pool:
    vmImage: 'ubuntu-16.04'
  variables:
    myVarFromJobADebug: $[ dependencies.A.outputs['debugJob.setvarStep.myOutputVar'] ]
  steps:
  - script: echo $(myVarFromJobADebug)
    name: echovar
```

```yaml
jobs:

# Set an output variable from a job with slicing
- job: A
  pool:
    vmImage: 'ubuntu-16.04'
    parallel: 2 # Two slices
  steps:
  - script: echo "##vso[task.setvariable variable=myOutputVar;isOutput=true]this is the slice $(system.jobPositionInPhase) value"
    name: setvarStep
  - script: echo $(setvarStep.myOutputVar)
    name: echovar

# Map the variable from the job for the first slice
- job: B
  dependsOn: A
  pool:
    vmImage: 'ubuntu-16.04'
  variables:
    myVarFromJobsA1: $[ dependencies.A.outputs['job1.setvarStep.myOutputVar'] ]
  steps:
  - script: "echo $(myVarFromJobsA1)"
    name: echovar
```

Be sure to prefix the job name to the output variables of a [deployment](deployment-jobs.md) job. In this case, the job name is `A`:

```yaml
jobs:

# Set an output variable from a deployment
- deployment: A
  pool:
    vmImage: 'ubuntu-16.04'
  environment: staging
  strategy:
    runOnce:
      deploy:
        steps:
        - script: echo "##vso[task.setvariable variable=myOutputVar;isOutput=true]this is the deployment variable value"
          name: setvarStep
        - script: echo $(setvarStep.myOutputVar)
          name: echovar

# Map the variable from the job for the first slice
- job: B
  dependsOn: A
  pool:
    vmImage: 'ubuntu-16.04'
  variables:
    myVarFromDeploymentJob: $[ dependencies.A.outputs['A.setvarStep.myOutputVar'] ]
  steps:
  - script: "echo $(myVarFromDeploymentJob)"
    name: echovar
```

::: moniker-end
::: moniker range="< azure-devops-2019"
YAML is not supported in TFS.
::: moniker-end

#### [Classic](#tab/classic/)
### Set a job-scoped variable from a script

To set a variable from a script, use the `task.setvariable` logging command.
This doesn't update the environment variables, but it does make the new
variable available to downstream steps within the same job.

[!INCLUDE [include](includes/set-variables-in-scripts.md)]

### Use variables as task inputs

In order to use a variable as a task input, you must make the variable an output variable, and you must give the producing task a reference name.
You can set a task's reference name on the **Output Variables** section of the task editor.
For instance, a script task whose output variable reference name is `producer` might have the following contents:

```cmd
mkdir myDir
echo ##vso[task.setvariable variable=newworkdir;isOutput=true]$(System.DefaultWorkingDirectory)\myDir
```

The output variable `newworkdir` can be referenced in the input of a downstream task as `$(producer.newworkdir)`.

### Set a multi-job output variable

You can't pass a variable from one job to another job of a build pipeline, unless you use YAML.

#### [Azure DevOps CLI](#tab/azure-devops-cli/)

::: moniker range="azure-devops"  
There is no [**az pipelines**](/cli/azure/ext/azure-devops/pipelines) command that applies to setting variables in scripts. The Azure DevOps CLI commands are only valid for Azure DevOps Services (cloud service).
::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *
## Set variables by using expressions

#### [YAML](#tab/yaml/)
::: moniker range=">= azure-devops-2019"

You can set a variable by using an expression. We already encountered one case of this to set a variable to the output of another from a previous job.

```yaml
- job: B
  dependsOn: A
  variables:
    myVarFromJobsA1: $[ dependencies.A.outputs['job1.setvarStep.myOutputVar'] ] # remember to use single quotes
```

You can use any of the supported expressions for setting a variable. Here's an example of setting a variable to act as a counter that starts at 100, gets incremented by 1 for every run, and gets reset to 100 every day.

```yaml
jobs:
- job:
  variables:
    a: $[counter(format('{0:yyyyMMdd}', pipeline.startTime), 100)]
  steps:
  - bash: echo $(a)
```

For more information about counters, dependencies, and other expressions, see [expressions](expressions.md).

::: moniker-end
::: moniker range="< azure-devops-2019"
YAML is not supported in TFS.
::: moniker-end

#### [Classic](#tab/classic/)
You can use any of the supported expressions for setting a variable. Here's an example of setting a variable to act as a counter that starts at 100, gets incremented by 1 for every run, and gets reset to 100 every day.

- In the variables of a build pipeline, set a variable `a` to this value: `$[counter(format('{0:yyyyMMdd}', pipeline.startTime), 100)]`

For more information about counters and other expressions, see [expressions](expressions.md).

#### [Azure DevOps CLI](#tab/azure-devops-cli/)

::: moniker range="azure-devops"  
There is no [**az pipelines**](/cli/azure/ext/azure-devops/pipelines) command that applies to setting variables using expressions. The Azure DevOps CLI commands are only valid for Azure DevOps Services (cloud service).
::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *
## Allow at queue time

#### [YAML](#tab/yaml/)
::: moniker range=">= azure-devops-2019"

You can choose which variables are allowed to be set at queue time, and which are fixed by the pipeline author.
If a variable appears in the `variables` block of a YAML file, it's fixed and can't be overridden at queue time.

To allow a variable to be set at queue time, make sure it doesn't appear in the `variables` block of a pipeline or job. 

You can also set a default value in the editor, and that value can be overridden by the person queuing the pipeline. To do this, select the variable in the **Variables** tab of the pipeline, and check **Let users override this value when running this pipeline**.


::: moniker-end
::: moniker range="< azure-devops-2019"
YAML is not supported in TFS.
::: moniker-end

#### [Classic](#tab/classic/)
You can choose which variables are allowed to be set at queue time, and which are fixed by the pipeline author.
To do this, select the variable in the **Variables** tab of the build pipeline, and mark it as **Settable at queue time**.

#### [Azure DevOps CLI](#tab/azure-devops-cli/)

::: moniker range="azure-devops"  
To choose which variables are allowed to be set at queue time using the Azure DevOps CLI, see [Create a variable](#create-variable) or [Update a variable](#update-variable).
::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *
## Expansion of variables

#### [YAML](#tab/yaml/)
::: moniker range=">= azure-devops-2019"

When you set a variable with the same name in multiple scopes, the following precedence applies (highest precedence first).

1. Job level variable set in the YAML file
1. Stage level variable set in the YAML file
1. Pipeline level variable set in the YAML file
1. Variable set at queue time
1. Pipeline variable set in Pipeline settings UI

In the following example, the same variable `a` is set at the pipeline level and job level in YAML file. It's also set in a variable group `G`, and as a variable in the Pipeline settings UI.

```yaml
variables:
  a: 'pipeline yaml'

stages:
- stage: one
  displayName: one
  variables:
  - name: a
    value: 'stage yaml'

  jobs:
  - job: A
    variables:
    - name: a
      value: 'job yaml'
    steps:
    - bash: echo $(a)        # This will be 'job yaml'
```

> [!NOTE]
> When you set a variable in the YAML file, don't define it in the web editor as settable at queue time. You can't currently change variables that are set in the YAML file at queue time. If you need a variable to be settable at queue time, don't set it in the YAML file.

Variables are expanded once when the run is started, and again at the beginning of each step. For example:

```yaml
jobs:
- job: A
  variables:
    a: 10
  steps:
  - bash: |
      echo $(a)            # This will be 10
      echo '##vso[task.setvariable variable=a]20'
      echo $(a)            # This will also be 10, since the expansion of $(a) happens before the step
  - bash: echo $(a)        # This will be 20, since the variables are expanded just before the step
```

There are two steps in the preceding example. The expansion of `$(a)` happens once at the beginning of the job, and once at the beginning of each of the two steps.

Because variables are expanded at the beginning of a job, you can't use them in a strategy. In the following example, you can't use the variable `a` to expand the job matrix, because the variable is only available at the beginning of each expanded job.

```yaml
jobs:
- job: A
  variables:
    a: 10
  strategy:
    matrix:
      x:
        some_variable: $(a)    # This does not work
```

If the variable `a` is an output variable from a previous job, then you can use it in a future job.

```yaml
- job: A
  steps:
  - powershell: echo "##vso[task.setvariable variable=a;isOutput=true]10"
    name: a_step

# Map the variable into job B
- job: B
  dependsOn: A
  variables:
    some_variable: $[ dependencies.A.outputs['a_step.a'] ]
```

### Recursive expansion

On the agent, variables referenced using `$( )` syntax are recursively expanded.
However, for service-side operations such as setting display names, variables aren't expanded recursively.
For example:

```yaml
variables:
  myInner: someValue
  myOuter: $(myInner)

steps:
- script: echo $(myOuter)  # prints "someValue"
  displayName: Variable is $(myOuter)  # display name is "Variable is $(myInner)"
```

::: moniker-end
::: moniker range="< azure-devops-2019"
YAML is not supported in TFS.
::: moniker-end

#### [Classic](#tab/classic/)
When you set a variable with the same name in multiple scopes, the following precedence applies (highest precedence first).

1. Variable set at queue time
1. Variable set in the pipeline
1. Variable set in the variable group

Variables are expanded once when the run is started, and again at the beginning of each step. Here's an example to demonstrate this:

- You set a variable called `a` to 10 in a pipeline.
- In one of the steps (a bash script step), run the following script:

   ```bash
   echo $(a)            # This will be 10
   echo '##vso[task.setvariable variable=a]20'
   echo $(a)            # This will also be 10, since the expansion of $(a) happens before the step
   ```

- In the next step (another bash script step), run the following script:

   ```bash
   echo $(a)            # This will be 20, since the variables are expanded just before the step
   ```

#### [Azure DevOps CLI](#tab/azure-devops-cli/)

::: moniker range="azure-devops"  
There is no [**az pipelines**](/cli/azure/ext/azure-devops/pipelines) command that applies to the expansion of variables. The Azure DevOps CLI commands are only valid for Azure DevOps Services (cloud service).
::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]
