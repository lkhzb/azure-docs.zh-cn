---
title: Azure Cosmos DB 的资源管理器模板 Cassandra API
description: 使用 Azure 资源管理器模板来创建和配置 Azure Cosmos DB Cassandra API。
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: thvankra
ms.openlocfilehash: e3dd086108ba8c518bf7caf027f149de2ab70e57
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/15/2020
ms.locfileid: "75980619"
---
# <a name="manage-azure-cosmos-db-cassandra-api-resources-using-azure-resource-manager-templates"></a>使用 Azure 资源管理器模板管理 Azure Cosmos DB Cassandra API 资源

本文介绍如何使用 Azure 资源管理器模板执行不同的操作来自动管理 Azure Cosmos DB 帐户、数据库和容器。 本文仅提供了有关 SQL API 帐户的示例，若要查找其他 API 类型帐户的示例，请参阅：将 Azure 资源管理器模板与适用于[SQL](manage-sql-with-resource-manager.md)、 [Gremlin](manage-gremlin-with-resource-manager.md)、 [MongoDB](manage-mongodb-with-resource-manager.md)、[表](manage-table-with-resource-manager.md)项目的 Azure Cosmos DB API 配合使用。

## 创建 Azure Cosmos 帐户、密钥空间和表<a id="create-resource"></a>

使用 Azure 资源管理器模板创建 Azure Cosmos DB 资源。 此模板将创建一个 Azure Cosmos Cassandra API 帐户，其中包含两个表，这些表在密钥空间级别共 400 RU/秒的吞吐量。 复制模板并按如下所示进行部署，或访问[Azure 快速入门库](https://azure.microsoft.com/resources/templates/101-cosmosdb-cassandra/)并从 Azure 门户部署。 还可以将模板下载到本地计算机，或者创建新模板，并使用 `--template-file` 参数指定本地路径。

> [!NOTE]
> 帐户名称必须是小写、44或更少字符。
> 若要更新 RU/秒，请用更新的吞吐量属性值重新提交模板。

[!code-json[create-cosmos-Cassandra](~/quickstart-templates/101-cosmosdb-cassandra/azuredeploy.json)]

## <a name="deploy-with-the-azure-cli"></a>部署 Azure CLI

若要使用 Azure CLI 部署 Azure 资源管理器模板，请**复制**该脚本，然后选择 "**尝试**" 以打开 Azure Cloud Shell。 若要粘贴脚本，请右键单击 shell，然后选择 "**粘贴**"：

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the keyset name: ' keysetName
read -p 'Enter the first table name: ' table1Name
read -p 'Enter the second table name: ' table2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-cassandra/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion keysetName=$keysetName \
   table1Name=$table1Name table2Name=$table2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

"`az cosmosdb show`" 命令显示预配新创建的 Azure Cosmos 帐户。 如果选择使用 Azure CLI 本地安装的版本，而不是使用 Cloud Shell，请参阅[Azure CLI](/cli/azure/)文章。


## <a name="next-steps"></a>后续步骤

下面是一些其他资源：

- [Azure 资源管理器文档](/azure/azure-resource-manager/)
- [Azure Cosmos DB 资源提供程序架构](/azure/templates/microsoft.documentdb/allversions)
- [Azure Cosmos DB 快速入门模板](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [排查常见的 Azure 资源管理器部署错误](../azure-resource-manager/templates/common-deployment-errors.md)
