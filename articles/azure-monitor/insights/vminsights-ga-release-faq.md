---
title: 用于 VM 的 Azure Monitor （GA）常见问题 |Microsoft Docs
description: 用于 VM 的 Azure Monitor 是 Azure 中的一个解决方案，它合并了 Azure VM 操作系统的运行状况和性能监视、应用程序组件及其与其他资源的依赖关系的自动发现功能，并映射这些组件和资源之间的通信。 本文解答了有关 GA 版本的常见问题。
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/31/2020
ms.openlocfilehash: ea7c695ddb92d441018503839b974c1f4bb33473
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2020
ms.locfileid: "77047847"
---
# <a name="azure-monitor-for-vms-generally-available-ga-frequently-asked-questions"></a>用于 VM 的 Azure Monitor 公开发布（GA）常见问题

此正式发行版常见问题解答介绍了在为我们的 GA 版本做准备时用于 VM 的 Azure Monitor 中所发生的更改。 

## <a name="updates-for-azure-monitor-for-vms"></a>用于 VM 的 Azure Monitor 更新

我们发布了新版本的用于 VM 的 Azure Monitor。 为 Vm 启用 Azure 监视器的客户现在会收到新版本，但已在使用用于 VM 的 Azure Monitor 的现有客户将会收到升级提示。 如果在多个工作区中部署大型部署，则此 FAQ 和我们的文档将提供指导以进行大规模升级。

在此升级过程中，用于 VM 的 Azure Monitor 性能数据存储在[容器 Azure Monitor](container-insights-overview.md)相同的*InsightsMetrics*表中，这使你可以更轻松地查询两个数据集。 此外，还可以存储在以前使用的表中无法存储的多个不同的数据集。 

在接下来的一周或第二周，我们的性能视图也将更新为使用此新表。

我们认识到，请求现有客户升级导致工作流中断，这就是我们目前在公共预览版中（而不是在 GA 之后）完成此操作的原因。


## <a name="what-is-changing"></a>有什么变化？

我们发布了一个名为 VMInsights 的新解决方案，其中包括用于数据收集的其他功能，以及用于将此数据存储在 Log Analytics 工作区中的新位置。 

过去，我们在你的工作区中启用了 ServiceMap 解决方案，并在 Log Analytics 工作区中设置了性能计数器，以将数据发送到*Perf*表。 这一新的解决方案会将数据发送到名为*InsightsMetrics*的表，用于容器 Azure Monitor。 此表架构使我们可以存储与*Perf*表格式不兼容的其他度量值和服务数据集。


## <a name="how-do-i-upgrade"></a>如何实现升级？
Log Analytics 工作区升级到 Vm 的最新 Azure Monitor 版本时，它会升级附加到该工作区的每个 Vm 上的依赖关系代理。 需要升级的每个 VM 都将在 Azure 门户中用于 VM 的 Azure Monitor 的 "**入门**" 选项卡中进行标识。 选择升级 VM 时，会将该 VM 的工作区与附加到该工作区的任何其他 Vm 一起升级。 可以选择单个 VM、多个 Vm、资源组或订阅。 

使用以下命令通过 PowerShell 升级工作区：

```PowerShell
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName <resource-group-name> -WorkspaceName <workspace-name> -IntelligencePackName "VMInsights" -Enabled $True
```

## <a name="what-should-i-do-about-the-performance-counters-in-my-workspace-if-i-install-the-vminsights-solution"></a>如果安装 VMInsights 解决方案，我应该如何处理我的工作区中的性能计数器？

启用用于 VM 的 Azure Monitor 的当前方法在工作区中使用性能计数器。 新方法将此数据存储在名为 `InsightsMetrics`的新表中。

更新用户界面以使用 `InsightsMetrics` 表中的数据后，我们将更新文档，并通过多个渠道传达此公告，包括在 Azure 门户中显示横幅。 此时，如果不再需要使用这些[性能计数器](vminsights-enable-overview.md#performance-counters-enabled)，可以选择在工作区中禁用这些计数器。 

>[!NOTE]
>如果有在 `Perf` 表中引用这些计数器的警报规则，则需要更新它们以引用存储在 `InsightsMetrics` 表中的新数据。 请参阅我们的文档，以了解可用于引用此表的示例日志查询。
>

如果决定保持启用性能计数器，则将根据 [Log Analytics 定价 [（ https://azure.microsoft.com/pricing/details/monitor/)，为数据引入计费，并将其存储在 `Perf` 表中。

## <a name="how-will-this-change-affect-my-alert-rules"></a>此更改如何影响我的警报规则？

如果你创建了[日志警报](../platform/alerts-unified-log.md)来查询在工作区中启用了目标性能计数器的 `Perf` 表，则应更新这些规则以改为引用 `InsightsMetrics` 表。 本指南还适用于使用 `ServiceMapComputer_CL` 和 `ServiceMapProcess_CL`的任何日志搜索规则，因为这些数据集将移到 `VMComputer` 和 `VMProcess` 表。

我们将更新此 FAQ 和我们的文档，以包含我们收集的数据集的示例日志搜索警报规则。

## <a name="how-will-this-affect-my-bill"></a>这会对我的帐单产生什么影响？

计费仍基于数据引入，并保存在 Log Analytics 工作区中。

我们收集的计算机级别性能数据是相同的，它的大小与我们在 `Perf` 表中存储的数据大小相同，并且成本大致相同。

## <a name="what-if-i-only-want-to-use-service-map"></a>如果我只想使用服务映射，该怎么办？

这是正常的。 查看即将推出的更新用于 VM 的 Azure Monitor 时，你将看到 Azure 门户中的提示。 发布后，你会收到一条提示，要求你将更新为新版本。 如果你只想使用[地图](vminsights-maps.md)功能，则可以选择不升级并继续使用用于 VM 的 Azure Monitor 中的 "地图" 功能和从你的工作区或仪表板磁贴访问的服务映射解决方案。

如果选择手动启用工作区中的性能计数器，则可能会在 Azure Monitor 查看的某些性能图表中看到数据。 新解决方案发布后，我们将更新性能图表以查询存储在 `InsightsMetrics` 表中的数据。 如果要在这些图表中看到该表中的数据，则需要升级到用于 VM 的 Azure Monitor 的新版本。

从 `ServiceMapComputer_CL` 和 `ServiceMapProcess_CL` 移动数据所做的更改将同时影响服务映射和用于 VM 的 Azure Monitor，因此仍需要规划此更新。

如果你选择不升级到**VMInsights**解决方案，我们将继续提供引用 `Perf` 表中的数据的旧版本的性能工作簿。  

## <a name="will-the-service-map-data-sets-also-be-stored-in-insightsmetrics"></a>服务映射数据集也会存储在 InsightsMetrics 中吗？

如果同时使用这两种解决方案，则将不会复制数据集。 这两个产品/服务都共享将存储在 `VMComputer` （以前称为 ServiceMapComputer_CL）、`VMProcess` （以前的 ServiceMapProcess_CL）、`VMConnection`和 `VMBoundPort` 表中的数据集，以存储我们收集的地图数据集。  

如果你使用用于 VM 的 Azure Monitor 和 VM Insights 解决方案，则 `InsightsMetrics` 表将存储我们收集并仅填充的 VM、进程和服务数据集。 服务映射解决方案将不会收集或存储 `InsightsMetrics` 表中的数据。

## <a name="will-i-be-double-charged-if-i-have-the-service-map-and-vminsights-solutions-in-my-workspace"></a>如果我的工作区中有服务映射和 VMInsights 解决方案，是否会向我收费？

不是，这两个解决方案共享存储在 `VMComputer` （以前称为 ServiceMapComputer_CL）、`VMProcess` （以前的 ServiceMapProcess_CL）、`VMConnection`和 `VMBoundPort`中的地图数据集。 如果工作区中同时具有两个解决方案，则不会对此数据收费。

## <a name="if-i-remove-either-the-service-map-or-vminsights-solution-will-it-remove-my-data"></a>如果删除服务映射或 VMInsights 解决方案，它将删除我的数据吗？

不是，这两个解决方案共享存储在 `VMComputer` （以前称为 ServiceMapComputer_CL）、`VMProcess` （以前的 ServiceMapProcess_CL）、`VMConnection`和 `VMBoundPort`中的地图数据集。 如果删除其中一个解决方案，则这些数据集会注意到，仍存在一个使用数据的解决方案，并且该解决方案仍保留在 Log Analytics 工作区中。 需要从工作区中删除这两个解决方案才能从中删除数据。

## <a name="when-will-this-update-be-released"></a>何时会发布此更新？

我们预计在2020年1日之前发布用于 VM 的 Azure Monitor 的更新。 在1月的发布日期更近，我们将在此发布更新，并在打开 Azure Monitor 时在 Azure 门户中显示通知。

## <a name="health-feature-is-in-limited-public-preview"></a>运行状况功能处于有限公共预览状态

我们从客户那里收到了有关 VM 健康功能集的大量出色反馈。 这项功能与对支持监视工作流的可能会有多大意义。 我们计划做出一系列的更改来添加功能并处理我们收到的反馈。 

为了最大限度地减少对新客户的这些更改的影响，我们已将此功能转移到**有限的公共预览版**中。 此更新发生于10月2019。

我们计划在用于 VM 的 Azure Monitor 公开上市后，在2020中重新启动此运行状况功能。

## <a name="how-do-existing-customers-access-the-health-feature"></a>现有客户如何访问运行状况功能？

使用运行状况功能的现有客户将继续具有访问权限，但不会提供给新客户。  

若要访问该功能，可以将以下功能标志添加到 Azure 门户 URL [https://portal.azure.com](https://portal.azure.com)`feature.vmhealth=true`。 示例 `https://portal.azure.com/?feature.vmhealth=true`。

你还可以使用此短 url，它会自动设置功能标志： [https://aka.ms/vmhealthpreview](https://aka.ms/vmhealthpreview)。

作为现有客户，你可以继续在连接到使用运行状况功能的现有工作区设置的 Vm 上使用运行状况功能。  

## <a name="i-use-vm-health-now-with-one-environment-and-would-like-to-deploy-it-to-a-new-one"></a>现在，我将 VM 运行状况用于一个环境，并想将其部署到新的环境中

如果你是使用运行状况功能的现有客户，并想要将其用于新的推出，请在 vminsights@microsoft.com 与我们联系以请求说明。

## <a name="next-steps"></a>后续步骤

请参阅[部署用于 VM 的 Azure Monitor](vminsights-enable-overview.md)，了解有助于监视虚拟机的要求和方法。
