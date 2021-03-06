---
title: Azure Functions 的存储注意事项
description: 了解 Azure Functions 和有关加密存储的数据的存储要求。
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 353fdd3bf7775e3bc7a9d017a9e8dd8238b09830
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964977"
---
# <a name="storage-considerations-for-azure-functions"></a>Azure Functions 的存储注意事项

创建 function app 实例时，Azure Functions 需要一个 Azure 存储帐户。 函数应用可能使用以下存储服务：


|存储服务  | 函数使用情况  |
|---------|---------|
| [Azure Blob 存储](/storage/blobs/storage-blobs-overview.md)     | 维护绑定状态和功能键。  <br/>还由[中的任务中心使用 Durable Functions](durable/durable-functions-task-hubs.md)。 |
| [Azure 文件](../storage/files/storage-files-introduction.md)  | 用于在[消耗计划](functions-scale.md#consumption-plan)中存储和运行函数应用代码的文件共享。 |
| [Azure 队列存储](../storage/queues/storage-queues-introduction.md)     | 由[中的任务中心使用 Durable Functions](durable/durable-functions-task-hubs.md)。   |
| [Azure 表存储](../storage/tables/table-storage-overview.md)  |  由[中的任务中心使用 Durable Functions](durable/durable-functions-task-hubs.md)。       |

> [!IMPORTANT]
> 使用消耗托管计划时，函数代码和绑定配置文件存储在主存储帐户的 Azure 文件存储中。 删除主存储帐户时，此内容将随之删除且无法恢复。

## <a name="storage-account-requirements"></a>存储帐户要求

创建 function app 时，必须创建或链接支持 Blob、队列和表存储的常规用途的 Azure 存储帐户。 这是因为函数依赖 Azure 存储进行操作，例如管理触发器和记录函数执行。 某些存储帐户不支持队列和表。 这些帐户包括仅限 blob 的存储帐户、Azure 高级存储和具有 ZRS 复制的常规用途存储帐户。 创建函数应用时，不受支持的帐户将从 "存储帐户" 边栏选项卡中筛选掉。

若要了解有关存储帐户类型的详细信息，请参阅 [Azure 存储服务简介](../storage/common/storage-introduction.md#azure-storage-services)。 

虽然你可以将现有存储帐户用于 function app，但必须确保它满足这些要求。 作为 function app create flow 的一部分创建的存储帐户可保证满足这些存储帐户要求。  

## <a name="storage-account-guidance"></a>存储帐户指南

每个 Function App 都需要存储帐户才能运行。 如果删除了该帐户，则不会运行函数应用。 若要解决与存储相关的问题，请参阅[如何解决与存储相关的问题](functions-recover-storage-account.md)。 以下附加注意事项适用于函数应用使用的存储帐户。

### <a name="storage-account-connection-setting"></a>存储帐户连接设置

存储帐户连接在[AzureWebJobsStorage 应用程序设置](./functions-app-settings.md#azurewebjobsstorage)中维护。 

重新生成存储密钥时，必须更新存储帐户连接字符串。 [有关存储密钥管理的详细信息，请参阅此处](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account)。

### <a name="shared-storage-accounts"></a>共享存储帐户

多个函数应用可以共享同一存储帐户，而不会出现任何问题。 例如，在 Visual Studio 中，可以使用 Azure 存储模拟器开发多个应用。 在这种情况下，模拟器的作用类似于单个存储帐户。 函数应用使用的同一个存储帐户也可用于存储应用程序数据。 但是，在生产环境中，这种方法并不总是很好。

### <a name="optimize-storage-performance"></a>优化存储性能

[!INCLUDE [functions-shared-storage](../../includes/functions-shared-storage.md)]

## <a name="storage-data-encryption"></a>存储数据加密

Azure 存储加密静态存储帐户中的所有数据。 有关详细信息，请参阅[静态数据的 Azure 存储加密](../storage/common/storage-service-encryption.md)。

默认情况下，使用 Microsoft 托管的密钥对数据进行加密。 为了更进一步控制加密密钥，你可以提供客户管理的密钥，以用于加密 blob 和文件数据。 这些密钥必须存在于 Azure Key Vault 中才能访问存储帐户。 若要了解详细信息，请参阅[使用 Azure 门户配置 Azure Key Vault 客户管理的密钥](../storage/common/storage-encryption-keys-portal.md)。  

## <a name="next-steps"></a>后续步骤

详细了解 Azure Functions 宿主选项。

> [!div class="nextstepaction"]
> [Azure Functions 的缩放和托管](functions-scale.md)


