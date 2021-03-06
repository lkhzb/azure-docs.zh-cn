---
title: 响应 Azure Blob 存储事件 | Microsoft Docs
description: 使用 Azure 事件网格订阅 Blob 存储事件。
author: normesta
ms.author: normesta
ms.date: 01/30/2018
ms.topic: conceptual
ms.service: storage
ms.subservice: blobs
ms.reviewer: cbrooks
ms.openlocfilehash: 78ec5b6d330f03d78dcb4e798b23d588fd93398e
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76835957"
---
# <a name="reacting-to-blob-storage-events"></a>响应 Blob 存储事件

Azure 存储事件允许应用程序响应事件，例如创建和删除 blob。 为此，它无需复杂的代码或高价低效的轮询服务。

使用[Azure 事件网格](https://azure.microsoft.com/services/event-grid/)将事件推送到 Azure Functions、Azure 逻辑应用等订阅服务器，甚至可推送到自己的 http 侦听器。 最好的做法是只为使用的部分付费。

Blob 存储将事件发送到事件网格，通过丰富的重试策略和死信向应用程序提供可靠的事件传递。

常见的 Blob 存储事件方案包括图像或视频处理、搜索索引或任何面向文件的工作流。 异步文件上传十分适合事件。 基于事件的体系结构对于鲜少更改，但要求立即响应的情况尤为有效。

若要立即试用，请参阅以下任一快速入门文章：

|如果要使用此工具：    |请参阅以下文章： |
|--|-|
|Azure 门户    |[快速入门：将 Blob 存储事件路由到具有 Azure 门户的 web 终结点](https://docs.microsoft.com/azure/event-grid/blob-event-quickstart-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[快速入门：通过 PowerShell 将存储事件路由到 web 终结点](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure CLI    |[快速入门：将存储事件路由到具有 Azure CLI 的 web 终结点](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

如果你的帐户具有分层命名空间，本教程将演示如何在 Azure Databricks 中连接事件网格订阅、Azure 函数和[作业](https://docs.azuredatabricks.net/user-guide/jobs.html)：[教程：使用 Azure Data Lake Storage Gen2 事件来更新 Databricks 增量表](data-lake-storage-events.md)。

>[!NOTE]
> 只有类型**StorageV2 （常规用途 v2）** 和**BlobStorage**的存储帐户支持事件集成。 **Storage （genral）** *不支持与*事件网格集成。

## <a name="the-event-model"></a>事件模型

事件网格使用[事件订阅](../../event-grid/concepts.md#event-subscriptions)将事件消息路由到订阅服务器。 此图说明了事件发布者、事件订阅和事件处理程序之间的关系。

![事件网格模型](./media/storage-blob-event-overview/event-grid-functional-model.png)

首先，将终结点订阅到事件。 然后，在触发事件时，事件网格服务将有关该事件的数据发送到终结点。

若要查看，请参阅[Blob 存储事件架构](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)一文：

> [!div class="checklist"]
> * Blob 存储事件以及每个事件的触发方式的完整列表。
> * 事件网格将针对每个事件发送的数据的示例。
> * 数据中显示的每个键值对的用途。

## <a name="filtering-events"></a>筛选事件

可以按事件类型、容器名称或创建/删除的对象的名称来[筛选 Blob 事件](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest)。 事件网格中的筛选器与主题的开头或结尾匹配，因此具有匹配的主题的事件将跳到订阅服务器。

若要了解有关如何应用筛选器的详细信息，请参阅 "[事件网格的筛选事件](https://docs.microsoft.com/azure/event-grid/how-to-filter-events)"。

Blob 存储事件使用者使用的格式：

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

要匹配存储帐户的所有事件，可将主题筛选器留空。

要匹配在一组共享前缀的容器中创建的 blob 的事件，请使用 `subjectBeginsWith` 筛选器，如下所示：

```
/blobServices/default/containers/containerprefix
```

要匹配在特定容器中创建的 blob 的事件，请使用 `subjectBeginsWith` 筛选器，如下所示：

```
/blobServices/default/containers/containername/
```

要匹配在共享 blob 名称前缀的特定容器中创建的 blob 的事件，请使用 `subjectBeginsWith` 筛选器，如下所示：

```
/blobServices/default/containers/containername/blobs/blobprefix
```

要匹配在共享 blob 后缀的特定容器中创建的 blob 事件，请使用 `subjectEndsWith` 筛选器，例如“.log”或“.jpg”。 有关详细信息，请参阅 [事件网格概念](../../event-grid/concepts.md#event-subscriptions)。

## <a name="practices-for-consuming-events"></a>使用事件的做法

处理 Blob 存储事件的应用程序应遵循以下建议的做法：
> [!div class="checklist"]
> * 由于可将多个订阅配置为将事件路由至相同的事件处理程序，因此请勿假定事件来自特定的源，而是应检查消息的主题，确保它来自所期望的存储帐户。
> * 同样，检查 eventType 是否为准备处理的项，并且不假定所接收的全部事件都是期望的类型。
> * 消息在一段延迟时间后会无序到达，请使用 etag 字段来了解对象的相关信息是否是最新的。  此外，还可使用 sequencer 字段来了解任何特定对象的事件顺序。
> * 使用 blobType 字段可了解 blob 中允许何种类型的操作，以及应当使用哪种客户端库类型来访问该 blob。 有效值为 `BlockBlob` 或 `PageBlob`。 
> * 将 URL 字段与 `CloudBlockBlob` 和 `CloudAppendBlob` 构造函数配合使用，以访问 blob。
> * 忽略不了解的字段。 此做法有助于适应将来可能添加的新功能。
> * 如果要确保仅在完全提交块 Blob 时触发**BlobCreated**事件，请筛选 `CopyBlob`、`PutBlob`、`PutBlockList` 或 `FlushWithClose` REST API 调用的事件。 仅当数据完全提交到块 Blob 后，这些 API 调用才会触发**BlobCreated**事件。 若要了解如何创建筛选器，请参阅 "[事件网格的筛选事件](https://docs.microsoft.com/azure/event-grid/how-to-filter-events)"。


## <a name="next-steps"></a>后续步骤

了解关于事件网格的详细信息，尝试一下 Blob 存储事件：

- [关于事件网格](../../event-grid/overview.md)
- [将 Blob 存储事件路由到自定义 Web 终结点](storage-blob-event-quickstart.md)
