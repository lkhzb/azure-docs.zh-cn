---
title: 如何添加 IoT 中心事件源-Azure 时序见解 |Microsoft Docs
description: 了解如何将 IoT 中心事件源添加到时序见解环境。
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/30/2020
ms.custom: seodec18
ms.openlocfilehash: 3ea73e2ca20faea30294bc5d5e1788415095c39f
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905364"
---
# <a name="add-an-iot-hub-event-source-to-your-time-series-insights-environment"></a>向时序见解环境添加 IoT 中心事件源

本文介绍如何使用 Azure 门户将从 Azure IoT 中心读取数据的事件源添加到 Azure 时序见解环境。

> [!NOTE]
> 本文中的说明适用于 Azure 时序见解 GA 版和时序见解预览版环境。

## <a name="prerequisites"></a>必备组件

* 创建 [Azure 时序见解环境](time-series-insights-update-create-environment.md)。
* [使用 Azure 门户创建 IoT 中心](../iot-hub/iot-hub-create-through-portal.md)。
* IoT 中心必须有正在发送进来的活动消息事件。
* 在 IoT 中心创建专用使用者组，以供时序见解环境使用。 每个时序见解事件源都必须具有自己的专用使用者组，该组不与其他使用者共享。 如果多个读取器使用同一个使用者组中的事件，则所有读取器都可能会出现故障。 有关详细信息，请阅读[Azure IoT 中心开发人员指南](../iot-hub/iot-hub-devguide.md)。

### <a name="add-a-consumer-group-to-your-iot-hub"></a>将使用者组添加到 IoT 中心

应用程序使用使用者组从 Azure IoT 中心提取数据。 若要可靠地从 IoT 中心读取数据，请提供仅供此时序见解环境使用的专用使用者组。

若要将新的使用者组添加到 IoT 中心，请执行以下操作：

1. 在[Azure 门户](https://portal.azure.com)中，找到并打开 IoT 中心。

1. 在 "**设置**" 下，选择 "**内置终结点**"，并选择 "**事件**" 终结点。

   [![在 "内置终结点" 页上，选择 "事件" 按钮](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-connect-iot-hub.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-connect-iot-hub.png#lightbox)

1. 在“使用者组”下，为使用者组输入唯一名称。 创建新的事件源时，请在时序见解环境中使用此相同名称。

1. 选择“保存”。

## <a name="add-a-new-event-source"></a>添加新的事件源

1. 登录 [Azure 门户](https://portal.azure.com)。

1. 在左侧菜单中，选择“所有资源”。 选择时序见解环境。

1. 在 "**设置**" 下，选择 "**事件源**"，然后选择 "**添加**"。

   [!["选择事件源"，然后选择 "添加" 按钮](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-add-event-source.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-add-event-source.png#lightbox)

1. 在“新建事件源”窗格中，输入一个特定于此时序见解环境的名称作为“事件源名称”。 例如，输入 **event-stream**。

1. 选择“IoT 中心”作为“源”。

1. 为“导入选项”选择一个值：

   * 在其中一个订阅上已有 IoT 中心时，请选择“从可用订阅使用 IoT 中心”。 此选项是最简单的方法。
   
     [![在 "新建事件源" 窗格中选择选项](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-select-an-import-option.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-select-an-import-option.png#lightbox)

    * 下表介绍的属性是“通过可用订阅使用 IoT 中心”选项所需的：

       [![新建事件源窗格-要在 "从可用订阅使用 IoT 中心" 选项中设置的属性](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-create-configure-confirm.png)](media/time-series-insights-how-to-add-an-event-source-iothub/tsi-create-configure-confirm.png#lightbox)

       | 属性 | Description |
       | --- | --- |
       | 订阅 | 所需的 iot 中心所属的订阅。 |
       | IoT 中心名称 | 所选 iot 中心的名称。 |
       | IoT 中心策略名称 | 选择共享访问策略。 可以在 "IoT 中心设置" 选项卡上找到共享访问策略。每个共享访问策略都有名称、所设置的权限以及访问密钥。 事件源的共享访问策略必须具有服务连接权限。 |
       | IoT 中心策略密钥 | 密钥已预填充。 |

    * 如果 IoT 中心在订阅外部，或者你希望选择高级选项，请选择“手动提供 IoT 中心设置”。

      下表介绍“手动提供 IoT 中心设置”所需的属性：

       | 属性 | Description |
       | --- | --- |
       | 订阅 ID | 所需的 iot 中心所属的订阅。 |
       | 资源组 | 在其中创建了此 IoT 中心的资源组名称。 |
       | IoT 中心名称 | IoT 中心的名称。 创建 IoT 中心时，为 IoT 中心输入了一个名称。 |
       | IoT 中心策略名称 | 共享访问策略。 可以在 "IoT 中心设置" 选项卡上创建共享访问策略。每个共享访问策略都有名称、所设置的权限以及访问密钥。 事件源的共享访问策略必须具有服务连接权限。 |
       | IoT 中心策略密钥 | 用于对 Azure 服务总线命名空间的访问权限进行身份验证的共享访问密钥。 在此处输入主密钥或辅助密钥。 |

    * 这两个选项共享以下配置选项：

       | 属性 | Description |
       | --- | --- |
       | IoT 中心使用者组 | 从 IoT 中心读取事件的使用者组。 强烈建议为事件源使用专用的使用者组。 |
       | 事件序列化格式 | 目前，JSON 是唯一可用的序列化格式。 事件消息必须采用此格式，否则将无法读取任何数据。 |
       | 时间戳属性名称 | 若要确定此值，需要了解发送到 IoT 中心的消息数据的消息格式。 此值是消息数据中你想要用作事件时间戳的特定事件属性的“名称”。 该值区分大小写。 如果留空，则事件源中的“事件排队时间”将用作事件时间戳。 |


1. 添加已添加到 IoT 中心的专用时序见解使用者组名称。

1. 选择“创建”。

1. 创建事件源以后，时序见解就会自动将数据流式传输到环境中。

## <a name="next-steps"></a>后续步骤

* [定义数据访问策略](time-series-insights-data-access.md)，以保护数据。

* [发送事件](time-series-insights-send-events.md)到事件源。

* 在[时序见解资源管理器中访问环境](https://insights.timeseries.azure.com)。
