<properties
	pageTitle="使用标记组织 Azure 资源 | Azure"
	description="演示如何应用标记来组织资源进行计费和管理。"
	services="azure-resource-manager"
	documentationCenter=""
	authors="tfitzmac"
	manager="timlt"
	editor="tysonn"/>  


<tags
	ms.service="azure-resource-manager"
	ms.workload="multiple"
	ms.tgt_pltfrm="AzurePortal"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/16/2016"
	wacn.date="10/24/2016"
	ms.author="tomfitz"/>


# 使用标记来组织 Azure 资源

资源管理器使您可以通过应用标记来按照逻辑组织资源。标记由通过您所定义的属性标识资源的键值对组成。若要将资源标记为属于同一类别，请将相同的标记应用到这些资源。

查看具有特定标记的资源时，会看到所有资源组中的资源。不仅可以组织同一资源组中的资源，还能够以独立于部署关系的方式组织资源。需要为计费或管理目的组织资源时，标记可能十分有用。

添加到资源或资源组的每个标记都会自动添加到订阅范围的分类。您也可以将标记名称预先填入订阅的分类，而且您想要作为资源使用的值会在未来加以标记。

每个资源或资源组最多可以有 15 个标记。标记名称不能超过 512 个字符，标记值不能超过 256 个字符。

> [AZURE.NOTE] 您只能将标记应用到支持资源管理器操作的资源。如果通过经典部署模型（如通过经典门户）创建虚拟机、虚拟网络或存储，则无法向该资源应用标记。若要支持标记，需通过 Resource Manager 重新部署这些资源。所有其他资源均支持标记。

## 模板

若要在部署过程中标记资源，只需将 **tags** 元素添加到正在部署的资源，然后提供标记名称和值即可。订阅中不需要预先存在标记名称和值。最多可为每个资源提供 15 个标记。

以下示例显示了一个包含标记的存储帐户。

    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2015-06-15",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "tags": {
                "dept": "Finance"
            },
            "properties": 
            {
                "accountType": "Standard_LRS"
            }
        }
    ]

Resource Manager 当前不支持处理标记名称和值对象。可以传递标记值对象，但仍需指定标记名称，如以下示例所示。

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "tagvalues": {
          "type": "object",
          "defaultValue": {
            "dept": "Finance",
            "project": "Test"
          }
        }
      },
      "resources": [
      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Storage/storageAccounts",
        "name": "examplestorage",
        "tags": {
          "dept": "[parameters('tagvalues').dept]",
          "project": "[parameters('tagvalues').project]"
        },
        "location": "[resourceGroup().location]",
        "properties": {
          "accountType": "Standard_LRS"
        }
      }]
    }


## 门户

[AZURE.INCLUDE [resource-manager-tag-resource](../includes/resource-manager-tag-resources.md)]

## PowerShell

[AZURE.INCLUDE [resource-manager-tag-resources](../includes/resource-manager-tag-resources-powershell.md)]

## Azure CLI

标记直接存在于资源和资源组中。若要查看现有标记，只需使用 **azure group show** 获取资源组及其资源。

    azure group show -n tag-demo-group
    
该操作返回有关资源组的元数据，包括任何应用到其中的标记。
    
    info:    Executing command group show
    + Listing resource groups
    + Listing resources for the group
    data:    Id:                  /subscriptions/{guid}/resourceGroups/tag-demo-group
    data:    Name:                tag-demo-group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: Dept=Finance;Environment=Production
    data:    Resources:
    data:
    data:      Id      : /subscriptions/{guid}/resourceGroups/tag-demo-group/providers/Microsoft.Sql/servers/tfsqlserver
    data:      Name    : tfsqlserver
    data:      Type    : servers
    data:      Location: eastus2
    data:      Tags    : Dept=Finance;Environment=Production
    ...

若要仅获取资源组的标记，请使用 JSON 实用工具，例如 [jq](http://stedolan.github.io/jq/download/)。

    azure group show -n tag-demo-group --json | jq ".tags"
    
该操作返回该资源组的标记。
    
    {
      "Dept": "Finance",
      "Environment": "Production" 
    }

使用 **azure resource show** 可以查看特定资源的标记。

    azure resource show -g tag-demo-group -n tfsqlserver -r Microsoft.Sql/servers -o 2014-04-01-preview --json | jq ".tags"
    
该操作返回该资源的标记。
    
    {
      "Dept": "Finance",
      "Environment": "Production"
    }
    
以下示例演示如何检索包含标记名称和值的所有资源。

    azure resource list --json | jq ".[] | select(.tags.Dept == "Finance") | .name"
    
该操作返回带有该标记的资源的名称。
    
    "tfsqlserver"
    "tfsqlserver/tfsqldata"

标记作为一个整体更新。若要将一个标记添加到包含现有标记的资源，请检索要保留的所有现有标记。若要为资源组设置标记值，请使用 **azure group set** 并提供该资源组的所有标记。

    azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
    
将返回带新标记的资源组的摘要。
    
    info:    Executing command group set
    ...
    data:    Name:                tag-demo-group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: Dept=Finance;Environment=Production;Project=Upgrade
    ...
    
可以使用 **azure tag list** 列出订阅中的现有标记，使用 **azure tag create** 添加标记。若要从订阅的分类中删除某个标记，首先请从所有资源中删除该标记。然后，使用 **azure tag delete** 删除该标记。

## REST API

门户和 PowerShell 在幕后都使用[资源管理器 REST API](https://msdn.microsoft.com/zh-cn/library/azure/dn848368.aspx)。如果需要将标记集成到另一个环境中，可以对资源 ID 使用 GET 以获取标记，并使用 PATCH 调用更新标记集。


## 标记和计费

对于受支持的服务，您可以使用标记对计费数据进行分组。例如，[与 Azure 资源管理器集成的虚拟机](/documentation/articles/virtual-machines-windows-compare-deployment-models/)可让你定义并应用标签来组织虚拟机的计费使用情况。如果您针对不同组织运行多个虚拟机，可以使用标记根据成本中心对使用情况进行分组。您还可以使用标记根据运行时环境对成本进行分类；例如，在生产环境中运行的虚拟机的计费使用情况。

在为支持标记和计费的服务下载使用情况 CSV 时，标记将显示在“标记”列中。有关更多详细信息，请参阅[了解 Microsoft Azure 的计费](/documentation/articles/billing-understand-your-bill/)。

![在计费中查看标记](./media/resource-group-using-tags/billing_csv.png)

## 后续步骤

- 可以使用自定义策略对订阅应用限制和约定。你定义的策略可能要求为所有资源设置特定的标记。有关详细信息，请参阅[使用策略来管理资源和控制访问](/documentation/articles/resource-manager-policy/)。
- 有关部署资源时使用 Azure PowerShell 的说明，请参阅[将 Azure PowerShell 与 Azure 资源管理器配合使用](/documentation/articles/powershell-azure-resource-manager/)。
- 有关部署资源时使用 Azure CLI 的说明，请参阅[将适用于 Mac、Linux 和 Windows 的 Azure CLI 与 Azure 资源管理配合使用](/documentation/articles/xplat-cli-azure-resource-manager/)。
- 有关使用门户的说明，请参阅 [Using the Azure portal to manage your Azure resources](/documentation/articles/resource-group-portal/)（使用 Azure 门户管理 Azure 资源）

<!---HONumber=Mooncake_1017_2016-->