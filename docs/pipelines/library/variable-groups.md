---
title: Variable groups for Azure Pipelines and TFS
ms.custom: seodec18
description: Share common variables across pipelines using variable groups
ms.assetid: A8AA9882-D3FD-4A8A-B22A-3A137CEDB3D7
ms.prod: devops
ms.technology: devops-cicd
ms.topic: conceptual
ms.manager: mijacobs
ms.author: ronai
author: RoopeshNair
ms.date: 02/05/2019
monikerRange: '>= tfs-2017'
---

# Add & use variable groups

Use a variable group to store values that you want to control and make available across
multiple pipelines. You can also use variable groups to store secrets and other values
that might need to be [passed into a YAML pipeline](variable-groups.md?tabs=yaml&view=azure-devops#use-a-variable-group). Variable groups are defined and managed in the **Library** page under
**Pipelines**.

[!INCLUDE [temp](../includes/concept-rename-note.md)]

::: moniker range="< tfs-2018"
> [!NOTE]
> Variable groups can be used in a build pipeline in only Azure DevOps and TFS 2018. They cannot be used in a build pipeline in earlier versions of TFS.
::: moniker-end

## Create a variable group

#### [YAML](#tab/yaml/)
Variable groups can’t be created in YAML, but they can be used as described in [Use a variable group](#use-a-variable-group).

#### [Classic](#tab/classic/)
1. Open the **Library** tab to see a list of existing variable groups for your project.
Choose **+ Variable group**.

1. Enter a name and description for the group.

1. Decide if you want the variable group to be accessible for any pipeline
   by setting the **Allow access to all pipelines** option. This option allows
   pipelines defined in YAML, which are not automatically authorized for variable groups,
   to use this variable group. See [Use a variable group](variable-groups.md?tabs=yaml&view=azure-devops#use-a-variable-group)

1. If you want to link secrets from an Azure key vault as variables, see [Link secrets from an Azure key vault](variable-groups.md#link-secrets-from-an-azure-key-vault).

1. Enter the name and value for each [variable](../release/variables.md#custom-variables)
   you want to include in the group, choosing **+ Add** for each one.
   If you want to encrypt and securely store the value, choose the "lock" icon 
   at the end of the row.

1. When you're finished adding variables, choose **Save**.

   ![Saving a variable group](media/save-variable-group.png) 

> Variable groups follow the [library security model](index.md#security).

#### [Azure DevOps CLI](#tab/azure-devops-cli)

::: moniker range="= azure-devops"

Using the Azure DevOps CLI, you can create and update variable groups for the pipeline runs in your project. You can also [manage the variable groups](#manage-a-variable-group) as well as [manage the individual variables within a variable group](#manage-variables-in-a-variable-group).

### Prerequisites

- You must have installed the Azure DevOps CLI extension as described in [Get started with Azure DevOps CLI](/azure/devops/cli/index).
- Sign into Azure DevOps using `az login`.
- For the examples in this article, set the default organization using `az devops configure --defaults organization=YourOrganizationURL`.

[Create a variable group](#create-variable-group) | [Update a variable group](#update-variable-group) 

<a id="create-variable-group" />  

### Create a variable group 

You can create a variable group with the [az pipelines variable-group create](/cli/azure/ext/azure-devops/pipelines/variable-group#ext-azure-devops-az-pipelines-variable-group-create) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group create --name
                                   --variables
                                   [--authorize {false, true}]
                                   [--description]
                                   [--org]
                                   [--project]
```

#### Parameters

- **name**: Required. Name of the variable group.
- **variables**: Required. Variables in format `key=value` space-separated pairs. Secret variables should be managed using [az pipelines variable-group variable](#manage-variables-in-a-variable-group) commands.
- **authorize**: Optional. Specify whether the variable group should be accessible by all pipelines. Accepted values are *false* and *true*.
- **description**: Optional. Description of the variable group.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.

#### Example

The following command creates a variable group named **new-app-variables**. It includes the variables **app-location=Head_Office** and **app-name=Fabrikam** and returns the result in YAML format.

```azurecli
az pipelines variable-group create --name new-app-variables --variables app-location=Head_Office app-name=Fabrikam --output yaml

authorized: false
description: null
id: 5
name: new-app-variables
providerData: null
type: Vsts
variables:
  app-location:
    isSecret: null
    value: Head_Office
  app-name:
    isSecret: null
    value: Fabrikam
```

<a id="update-variable-group" />  

### Update a variable group

You can update a variable group with the [az pipelines variable-group update](/cli/azure/ext/azure-devops/pipelines/variable-group#ext-azure-devops-az-pipelines-variable-group-update) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group update --group-id
                                   [--authorize {false, true}]
                                   [--description]
                                   [--name]
                                   [--org]
                                   [--project]
```

#### Parameters

- **group-id**: Required. ID of the variable group. To find the variable group ID, see [List variable groups](#list-variable-group).
- **authorize**: Required unless **description** or **name** is specified. Indicates whether the variable group should be accessible by all pipelines. Accepted values are *false* and *true*.
- **description**: Required unless **authorize** or **name** is specified. Use to change the description of the variable group.
- **name**: Required unless **authorize** or **description** is specified. Use to change the name of the variable group.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.

#### Example

The following command updates the variable group with the ID **4**. The **description** and **name** are changed and the results are shown in table format.

```azurecli
az pipelines variable-group update --group-id 4 --name MyNewAppVariables --description "Variables for my new app" --output table

ID    Name               Type    Description               Is Authorized    Number of Variables
----  -----------------  ------  ------------------------  ---------------  ---------------------
4     MyNewAppVariables  Vsts    Variables for my new app  False            2
```

::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *

## Use a variable group

#### [YAML](#tab/yaml/)
::: moniker range="> tfs-2018"

To use a variable from a variable group, you need to add a reference to the group in your YAML file:

```yaml
variables:
- group: my-variable-group
```

Thereafter variables from the variable group can be used in your YAML file.

If you use both variables and variable groups, you'll have to use `name`/`value` syntax for the individual (non-grouped) variables:

```yaml
variables:
- group: my-variable-group
- name: my-bare-variable
  value: 'value of my-bare-variable'
```

Next you must authorize the variable group (this is a security feature: if you only had to name the variable group in YAML, then anyone who can push code
to your repository could extract the contents of secrets in the variable group).
To do this, or if you encounter a resource authorization error in your build,
use one of the following techniques:

* If you want to authorize any pipeline to use the variable group,
  which may be a suitable option if you do not have any secrets in the group,
  go to Azure Pipelines, open the **Library** page, choose **Variable groups**, select the variable group in question,
  and enable the setting **Allow access to all pipelines**.

* If you want to authorize a variable group for a specific pipeline, open the pipeline
  by selecting **Edit** and queue a build manually. You will see a resource authorization error
  and a "Authorize resources" action on the error. Choose this action to explicitly add the pipeline as an
  authorized user of the variable group.

> [!Note]
> If you added a variable group to a pipeline and did not get a resource authorization error in your build when you expected one, turn off the **Allow access to all pipelines** setting described above.

::: moniker-end

::: moniker range="<= tfs-2018"

YAML builds are not yet available on TFS.

::: moniker-end

#### [Classic](#tab/classic/)
To use a variable group, open your pipeline, select the **Variables**
tab, select **Variable groups**, and then choose **Link variable group**.
In a build pipeline, you see a list of available groups. In a release pipeline (as shown below), you
also see a drop-down list of stages in the pipeline - you can link the variable group to one or more of these stages.

![Linking a variable group](media/link-variable-group.png)

* In a **build pipeline**, the variable group is linked to the pipeline and all the variables in the group are available for use within this pipeline.
* In a **release pipeline**, you can link a variable group to the pipeline itself, or to a specific stage of the release pipeline.
  - If you link to a release pipeline, all the variables in the group are available for use in the pipeline and in all stages of that pipeline.
  - If you link to one or more stages in a release pipeline, the variables from the variable group are scoped to these stages and are not accessible in the other stages of the same release.

> [!NOTE]
> Linking a variable group to a specific stage is available only on Azure Pipelines and on TFS 2018 Update 2 and later.

#### [Azure DevOps CLI](#tab/azure-devops-cli) 

::: moniker range="azure-devops"

There is no [**az pipelines**](/cli/azure/ext/azure-devops/pipelines) command that applies to using a variable group. The Azure DevOps CLI commands are only valid for Azure DevOps Services (cloud service). 

::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *

You access the value of the variables in a linked variable group in exactly
the same way as [variables you define within the pipeline itself](../process/variables.md).
For example, to access the value of a variable named **customer** in a variable group linked to the pipeline,
use `$(customer)` in a task parameter or a script. However, secret variables (encrypted variables and key vault variables) 
cannot be accessed directly in scripts - instead they must be passed as arguments to a task. For more information, see [secrets](../process/variables.md#secret-variables)

Any changes made centrally to a variable group, such as a change in the value of a variable or the addition of new variables,
will automatically be made available to all the definitions or stages to which the variable group is linked.

::: moniker range="azure-devops"

## Manage a variable group

Using the Azure DevOps CLI, you can list the variable groups for the pipeline runs in your project and show details for each one. You can also delete variable groups if you no longer need them.

[List variable groups](#list-variable-group) | [Show details for a variable group](#show-variable-group) | [Delete a variable group](#delete-variable-group)

<a id="list-variable-group" />  

### List variable groups

You can list the variable groups in your project with the [az pipelines variable-group list](/cli/azure/ext/azure-devops/pipelines/variable-group#ext-azure-devops-az-pipelines-variable-group-list) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group list [--action {manage, none, use}]
                                 [--continuation-token]
                                 [--group-name]
                                 [--org]
                                 [--project]
                                 [--query-order {Asc, Desc}]
                                 [--top]
```

#### Optional parameters

- **action**: Specifies the action that can be performed on the variable groups. Accepted values are *manage*, *none* and *use*.
- **continuation-token**: Lists the variable groups after a continuation token is provided.
- **group-name**: Name of the variable group. Wildcards are accepted, such as `new-var*`.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **query-order**: Lists the results in either ascending or descending (the default) order. Accepted values are *Asc* and *Desc*.
- **top**: Number of variable groups to list.

#### Example

The following command lists the top 3 variable groups in ascending order and returns the results in table format.

```azurecli
az pipelines variable-group list --top 3 --query-order Asc --output table

ID    Name               Type    Number of Variables
----  -----------------  ------  ---------------------
1     myvariables        Vsts    2
2     newvariables       Vsts    4
3     new-app-variables  Vsts    3
```

<a id="show-variable-group" />  

### Show details for a variable group

You can display the details of a variable group in your project with the [az pipelines variable-group show](/cli/azure/ext/azure-devops/pipelines/variable-group#ext-azure-devops-az-pipelines-variable-group-show) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group show --group-id
                                 [--org]
                                 [--project]
```

#### Parameters

- **group-id**: Required. ID of the variable group. To find the variable group ID, see [List variable groups](#list-variable-group).
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.

#### Example

The following command shows details for the variable group with the ID **4** and returns the results in YAML format.

```azurecli
az pipelines variable-group show --group-id 4 --output yaml

authorized: false
description: Variables for my new app
id: 4
name: MyNewAppVariables
providerData: null
type: Vsts
variables:
  app-location:
    isSecret: null
    value: Head_Office
  app-name:
    isSecret: null
    value: Fabrikam
```

<a id="delete-variable-group" />  

### Delete a variable group

You can delete a variable group in your project with the [az pipelines variable-group delete](/cli/azure/ext/azure-devops/pipelines/variable-group#ext-azure-devops-az-pipelines-variable-group-delete) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group delete --group-id
                                   [--org]
                                   [--project]
                                   [--yes]
```

#### Parameters

- **group-id**: Required. ID of the variable group. To find the variable group ID, see [List variable groups](#list-variable-group).
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **yes**: Optional. Doesn't prompt for confirmation.

#### Example

The following command deletes the variable group with the ID **1** and doesn't prompt for confirmation.

```azurecli
az pipelines variable-group delete --group-id 1 --yes

Deleted variable group successfully.
```

## Manage variables in a variable group

Using the Azure DevOps CLI, you can add and delete variables from a variable group in a pipeline run. You can also list the variables in the variable group and make updates to them as needed.

[Add variables to a variable group](#add-variables-group) | [List variables in a variable group](#list-variables-group) | [Update variables in a variable group](#update-variables-group) | [Delete variables from a variable group](#delete-variables-group)

<a id="add-variables-group" />  

### Add variables to a variable group

You can add a variable to a variable group with the [az pipelines variable-group variable create](/cli/azure/ext/azure-devops/pipelines/variable-group/variable#ext-azure-devops-az-pipelines-variable-group-variable-create) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group variable create --group-id
                                            --name
                                            [--org]
                                            [--project]
                                            [--secret {false, true}]
                                            [--value]
```

#### Parameters

- **group-id**: Required. ID of the variable group. To find the variable group ID, see [List variable groups](#list-variable-group).
- **name**: Required. Name of the variable you are adding.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **secret**: Optional. Indicates whether the variable's value is a secret. Accepted values are *false* and *true*.
- **value**: Required for non secret variable. Value of the variable. For secret variables, if **value** parameter is not provided, it is picked from environment variable prefixed with `AZURE_DEVOPS_EXT_PIPELINE_VAR_` or user is prompted to enter it via standard input. For example, a variable named **MySecret** can be input using the environment variable `AZURE_DEVOPS_EXT_PIPELINE_VAR_MySecret`.

#### Example

The following command creates a variable in the variable group with ID of **4**. The new variable is named **requires-login** and has a value of **True**, and the result is shown in table format.

```azurecli
az pipelines variable-group variable create --group-id 4 --name requires-login --value True --output table

Name            Is Secret    Value
--------------  -----------  -------
requires-login  False        True
```

<a id="list-variables-group" />  

### List variables in a variable group

You can list the variables in a variable group with the [az pipelines variable-group variable list](/cli/azure/ext/azure-devops/pipelines/variable-group/variable#ext-azure-devops-az-pipelines-variable-group-variable-list) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group variable list --group-id
                                          [--org]
                                          [--project]
```

#### Parameters

- **group-id**: Required. ID of the variable group. To find the variable group ID, see [List variable groups](#list-variable-group).
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.

#### Example

The following command lists all of the variables in the variable group with ID of **4** and shows the result in table format.

```azurecli
az pipelines variable-group variable list --group-id 4 --output table

Name            Is Secret    Value
--------------  -----------  -----------
app-location    False        Head_Office
app-name        False        Fabrikam
requires-login  False        True
```

<a id="update-variables-group" />  

### Update variables in a variable group

You can update a variable in a variable group with the [az pipelines variable-group variable update](/cli/azure/ext/azure-devops/pipelines/variable-group/variable#ext-azure-devops-az-pipelines-variable-group-variable-update) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group variable update --group-id
                                            --name
                                            [--new-name]
                                            [--org]
                                            [--project]
                                            [--prompt-value {false, true}]
                                            [--secret {false, true}]
                                            [--value]
```

#### Parameters

- **group-id**: Required. ID of the variable group. To find the variable group ID, see [List variable groups](#list-variable-group).
- **name**: Required. Name of the variable you are adding.
- **new-name**: Optional. Specify to change the name of the variable.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **prompt-value**: Set to **true** to update the value of a secret variable using environment variable or prompt via standard input. Accepted values are *false* and *true*.
- **secret**: Optional. Indicates whether the variable's value is kept secret. Accepted values are *false* and *true*.
- **value**: Updates the value of the variable. For secret variables, use the **prompt-value** parameter to be prompted to enter it via standard input. For non-interactive consoles, it can be picked from environment variable prefixed with `AZURE_DEVOPS_EXT_PIPELINE_VAR_`. For example, a variable named **MySecret** can be input using the environment variable `AZURE_DEVOPS_EXT_PIPELINE_VAR_MySecret`.

#### Example

The following command updates the **requires-login** variable with the new value **False** in the variable group with ID of **4**. It specifies that the variable is a **secret** and shows the result in YAML format. Notice that the output shows the value as **null** instead of **False** since it is a secret value (hidden).

```azurecli
az pipelines variable-group variable update --group-id 4 --name requires-login --value False --secret true --output yaml

requires-login:
  isSecret: true
  value: null
```

<a id="delete-variables-group" />  

### Delete variables from a variable group

You can delete a variable from a variable group with the [az pipelines variable-group variable delete](/cli/azure/ext/azure-devops/pipelines/variable-group/variable#ext-azure-devops-az-pipelines-variable-group-variable-delete) command. To get started, see [Get started with Azure DevOps CLI](../../cli/index.md).

```azurecli
az pipelines variable-group variable delete --group-id
                                            --name
                                            [--org]
                                            [--project]
                                            [--yes]
```

#### Parameters

- **group-id**: Required. ID of the variable group. To find the variable group ID, see [List variable groups](#list-variable-group).
- **name**: Required. Name of the variable you are deleting.
- **org**: Azure DevOps organization URL. You can configure the default organization using `az devops configure -d organization=ORG_URL`. Required if not configured as default or picked up using `git config`. Example: `--org https://dev.azure.com/MyOrganizationName/`.
- **project**: Name or ID of the project. You can configure the default project using `az devops configure -d project=NAME_OR_ID`. Required if not configured as default or picked up using `git config`.
- **yes**: Optional. Doesn't prompt for confirmation.

#### Example

The following command deletes the **requires-login** variable from the variable group with ID of **4** and prompts for confirmation.

```azurecli
az pipelines variable-group variable delete --group-id 4 --name requires-login

Are you sure you want to delete this variable? (y/n): y
Deleted variable 'requires-login' successfully.
```

::: moniker-end

## Link secrets from an Azure key vault

Link an existing Azure key vault to a variable group and map selective vault secrets to the variable group.

1. In the **Variable groups** page, enable **Link secrets from an Azure key vault as variables**.
   You'll need an existing key vault containing your secrets. You can create a 
   key vault using the [Azure portal](https://portal.azure.com).

   ![Variable group with Azure key vault integration](media/link-azure-key-vault-variable-group.png)

1. Specify your Azure subscription end point and the name of the vault containing your secrets.

   Ensure the Azure service connection has at least **Get** and **List** management permissions on the vault for secrets.
   You can enable Azure Pipelines to set these permissions by choosing **Authorize** next to the vault name.
   Alternatively, you can set the permissions manually in the [Azure portal](https://portal.azure.com):

   - Open the **Settings** blade for the vault, choose **Access policies**, then **Add new**.
   - In the **Add access policy** blade, choose **Select principal** and select the service principal for your client account.
   - In the **Add access policy** blade, choose **Secret permissions** and ensure that **Get** and **List** are checked (ticked).
   - Choose **OK** to save the changes.<p />

1. In the **Variable groups** page, choose **+ Add** to select specific secrets from your vault that will be mapped to this variable group.

### Secrets management notes

* Only the secret *names* are mapped to the variable group, not the secret values. The latest version of the value of each secret
  is fetched from the vault and used in the pipeline linked to the variable group during the run.

* Any changes made to *existing* secrets in the key vault, such as a change in the value of a secret, will be made available
  automatically to all the pipelines in which the variable group is used.

* When new secrets are added to the vault, or a secret is deleted from the vault, the associated variable groups are not updated
  automatically. The secrets included in the variable group must be explicitly updated in order for the pipelines using the
  variable group to execute correctly.

* Azure Key Vault supports storing and managing cryptographic keys and secrets in Azure.
  Currently, Azure Pipelines variable group integration supports mapping only secrets from the Azure key vault. Cryptographic keys and certificates are not supported.

## Expansion of variables in a group

#### [YAML](#tab/yaml/)
::: moniker range=">= azure-devops-2019"

When you set a variable in a group and use it in a YAML file, it has the same precedence as any other variable defined within the YAML file. 
For more information about precedence of variables, see the topic on [variables](../process/variables.md#expansion-of-variables).

::: moniker-end
::: moniker range="< azure-devops-2019"
YAML is not supported in TFS.
::: moniker-end

#### [Classic](#tab/classic/)
When you set a variable with the same name in multiple scopes, the following precedence is used (highest precedence first).

1. Variable set at queue time
1. Variable set in the pipeline
1. Variable set in the variable group

[!INCLUDE [variable-collision](../includes/variable-collision.md)]

#### [Azure DevOps CLI](#tab/azure-devops-cli) 

::: moniker range="azure-devops"

There is no [**az pipelines**](/cli/azure/ext/azure-devops/pipelines) command that applies to the expansion of variables in a group. The Azure DevOps CLI commands are only valid for Azure DevOps Services (cloud service). 

::: moniker-end

[!INCLUDE [temp](../../includes/note-cli-not-supported.md)]

* * *
