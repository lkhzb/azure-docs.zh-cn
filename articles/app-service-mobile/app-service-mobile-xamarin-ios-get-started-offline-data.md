---
title: 启用脱机同步（Xamarin iOS）
description: 了解如何在 Xamarin iOS 应用程序中使用应用服务移动应用来缓存和同步脱机数据。
ms.assetid: 828a287c-5d58-4540-9527-1309ebb0f32b
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 06/25/2019
ms.openlocfilehash: 615f8f028182178928d3d755c102daceef402ef4
ms.sourcegitcommit: 3d4917ed58603ab59d1902c5d8388b954147fe50
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2019
ms.locfileid: "74668255"
---
# <a name="enable-offline-sync-for-your-xamarinios-mobile-app"></a>为 Xamarin.iOS 移动应用启用离线同步
[!INCLUDE [app-service-mobile-selector-offline](../../includes/app-service-mobile-selector-offline.md)]

> [!NOTE]
> Visual Studio App Center 支持以移动应用开发为中心的端到端集成服务。 开发人员可以使用“生成”、“测试”和“分发”服务来设置“持续集成和交付”管道。 部署应用后，开发人员可以使用“分析”和“诊断”服务监视其应用的状态和使用情况，并使用“推送”服务吸引用户。 开发人员还可以利用“身份验证”对其用户进行身份验证，并使用“数据”服务在云中保留和同步应用数据。
>
> 如果希望将云服务集成到移动应用程序中，请立即注册到 [App Center](https://appcenter.ms/?utm_source=zumo&utm_medium=Azure&utm_campaign=zumo%20doc) 中。

## <a name="overview"></a>概述
本教程介绍适用于 Xamarin.iOS 的 Azure 移动应用的脱机同步功能。 脱机同步允许最终用户与移动应用交互（查看、添加或修改数据），即使在没有网络连接时也是如此。 更改存储在本地数据库中。 设备重新联机后，这些更改会与远程服务同步。

在本教程中，更新[创建 Xamarin iOS 应用]中的 Xamarin.iOS 应用项目，以支持 Azure 移动应用的脱机功能。 如果不使用下载的快速入门服务器项目，必须将数据访问扩展包添加到项目。 有关服务器扩展包的详细信息，请参阅[使用适用于 Azure 移动应用的 .NET 后端服务器 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

若要了解有关脱机同步功能的详细信息，请参阅主题 [Azure 移动应用中的脱机数据同步]。

## <a name="update-the-client-app-to-support-offline-features"></a>更新客户端应用以支持脱机功能
脱机情况下，可使用 Azure 移动应用脱机功能与本地数据库交互。 要在应用中使用这些功能，请将 [SyncContext] 初始化到本地存储。 通过 [IMobileServiceSyncTable] 接口引用表。 SQLite 在设备上用作本地存储。

1. 打开在[创建 Xamarin iOS 应用]教程中完成的项目中的 NuGet 包管理器，搜索并安装 **Microsoft.Azure.Mobile.Client.SQLiteStore** NuGet 包。
2. 打开 QSTodoService.cs 文件并取消注释 `#define OFFLINE_SYNC_ENABLED` 定义。
3. 重新生成并运行客户端应用。 应用的工作方式与启用脱机同步之前的工作方式相同。但是，本地数据库现在填充了可在脱机方案中使用的数据。

## <a name="update-sync"></a>更新应用以与后端断开连接
在本部分中，将断开与移动应用后端的连接，以模拟脱机情况。 添加数据项时，异常处理程序将指示该应用处于脱机模式。 在此状态下，新项将添加到本地存储，下次以连接状态运行推送时，这些新项将同步到移动应用后端。

1. 在共享项目中编辑 QSToDoService.cs。 更改 **applicationURL** 以指向无效的 URL：

         const string applicationURL = @"https://your-service.azurewebsites.fail";

    还可以通过在设备上禁用 wifi 和手机网络或使用飞行模式来演示脱机行为。
2. 生成并运行应用。 请注意，在应用启动时，同步刷新会失败。
3. 输入新项，并注意每次单击“保存”时，推送会失败，并显示 [CancelledByNetworkError] 状态。 但是，新的待办事项在推送到移动应用后端之前，存在于本地存储中。  在生产应用中，如果取消显示这些异常，客户端应用的行为就像它仍连接到移动应用后端一样。
4. 关闭应用程序并重新启动它，以验证你创建的新项目是否已永久保存到本地存储中。
5. （可选）如果电脑上装有 Visual Studio，打开“服务器资源管理器”。 导航到“Azure”-> “SQL 数据库”中的数据库。 右键单击数据库并选择“在 SQL Server 对象资源管理器中打开”。 现在便可以浏览 SQL 数据库表及其内容。 验证后端数据库中的数据是否未更改。
6. （可选）通过 Fiddler 或 Postman 之类的 REST 工具使用 `https://<your-mobile-app-backend-name>.azurewebsites.net/tables/TodoItem` 格式的 GET 查询，查询移动后端。

## <a name="update-online-app"></a>更新应用以重新连接移动应用后端
在本部分中，会将应用重新连接到移动应用后端。 这模拟的是通过移动应用后端从脱机状态转为联机状态的应用。   如果通过关闭网络连接来模拟网络故障，则不需要更改代码。
再次打开网络。  首次运行应用程序时，调用 `RefreshDataAsync` 方法。 这转而会调用 `SyncAsync`，将本地存储与后端数据库同步。

1. 在共享项目中打开 QSToDoService.cs，并恢复对 **applicationURL** 属性的更改。
2. 构建并运行应用程序。 执行 `OnRefreshItemsSelected` 方法时，应用使用推送和拉取操作将本地更改与 Azure 移动应用后端同步。
3. （可选）使用 SQL Server 对象资源管理器或 Fiddler 之类的 REST 工具查看更新后的数据。 请注意，数据已在 Azure 移动应用后端数据库和本地存储之间进行同步。
4. 在应用程序中，单击要在本地存储区中完成的几个项旁边的复选框。

   `CompleteItemAsync` 调用 `SyncAsync`，将每个已完成项与移动应用后端同步。 `SyncAsync` 同时调用推送和拉取操作。
   **每当对客户端已更改的表执行拉取操作时，始终先对客户端同步上下文自动执行推送操作**。 隐式推送可确保本地存储中的所有表以及关系都保持一致。 有关此行为的详细信息，请参阅 [Azure 移动应用中的脱机数据同步]。

## <a name="review-the-client-sync-code"></a>查看客户端同步代码
完成教程[创建 Xamarin iOS 应用]时下载的 Xamarin 客户端项目已包含使用本地 SQLite 数据库支持脱机同步的代码。 下面简要概述了在教程代码中已包含的内容。 有关功能的概念性概述，请参阅 [Azure 移动应用中的脱机数据同步]。

* 表操作之前，必须初始化本地存储区。 `QSTodoListViewController.ViewDidLoad()` 执行 `QSTodoService.InitializeStoreAsync()` 时，对本地存储数据库进行初始化。 此方法将使用 Azure 移动应用客户端 SDK 提供的 `MobileServiceSQLiteStore` 类创建一个新的本地 SQLite 数据库。

    `DefineTable` 方法与所提供的类型中的字段相匹配的本地存储中创建一个表 `ToDoItem` 这种情况下。 该类型无需包括远程数据库中的所有列。 可以只存储列的子集。

        // QSTodoService.cs

        public async Task InitializeStoreAsync()
        {
            var store = new MobileServiceSQLiteStore(localDbPath);
            store.DefineTable<ToDoItem>();

            // Uses the default conflict handler, which fails on conflict
            await client.SyncContext.InitializeAsync(store);
        }
* `QSTodoService` 的 `todoTable` 成员属于 `IMobileServiceSyncTable` 类型而不是 `IMobileServiceTable` 类型。 MobileServiceSyncTable 会将所有创建、读取、更新和删除 (CRUD) 表操作定向到本地存储数据库。

    通过调用 `IMobileServiceSyncContext.PushAsync()` 决定将这些更改推送到 Azure 移动应用后端的时间。 对于调用 `PushAsync` 时客户端应用修改的所有表，此同步上下文通过跟踪和推送这些表中的更改来帮助保持表关系。

    每当刷新 todoitem 列表或者添加或完成 todoitem 时，所提供的代码便会调用 `QSTodoService.SyncAsync()` 进行同步。 每次本地更改后，该应用同步。 对具有由上下文跟踪的未完成本地更新的表执行拉取操作时，该拉取操作将先自动触发上下文推送操作。

    在所提供的代码中，查询远程 `TodoItem` 表中的所有记录，但它还可以筛选记录，只需将查询 ID 和查询传递给 `PushAsync` 即可。 有关详细信息，请参阅 [Azure 移动应用中的脱机数据同步]中的增量同步部分。

        // QSTodoService.cs
        public async Task SyncAsync()
        {
            try
            {
                await client.SyncContext.PushAsync();
                await todoTable.PullAsync("allTodoItems", todoTable.CreateQuery()); // query ID is used for incremental sync
            }

            catch (MobileServiceInvalidOperationException e)
            {
                Console.Error.WriteLine(@"Sync Failed: {0}", e.Message);
            }
        }

## <a name="additional-resources"></a>其他资源
* [Azure 移动应用中的脱机数据同步]
* [Azure 移动应用 .NET SDK 如何][8]

<!-- Images -->

<!-- URLs. -->
[创建 Xamarin iOS 应用]: app-service-mobile-xamarin-ios-get-started.md
[Azure 移动应用中的脱机数据同步]: app-service-mobile-offline-data-sync.md
[SyncContext]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mobileservices.mobileserviceclient.synccontext(v=azure.10).aspx
[8]: app-service-mobile-dotnet-how-to-use-client-library.md
