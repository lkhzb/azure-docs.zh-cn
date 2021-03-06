---
title: Azure VMware 解决方案（AVS）-使用 ExpressRoute 将 AVS 私有云连接到 Azure 网络
description: 介绍如何使用 ExpressRoute 将你的 AVS 私有云环境连接到 Azure 虚拟网络
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3d487794e219f63150142db8df4b0c1abf112947
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015229"
---
# <a name="connect-your-avs-private-cloud-environment-to-the-azure-virtual-network-using-expressroute"></a>使用 ExpressRoute 将你的 AVS 私有云环境连接到 Azure 虚拟网络

你的 AVS 私有云可以使用 Azure ExpressRoute 连接到 Azure 虚拟网络。 此高带宽、低延迟连接使你能够从你的 AVS 私有云环境访问你的 Azure 订阅中运行的服务。

虚拟网络连接可让你：

* 使用 Azure 作为你的 AVS 私有云上的虚拟机的备份目标。
* 在 Azure 订阅中部署 KMS 服务器，以加密 AVS 私有云 vSAN 数据存储。
* 使用混合应用程序，在应用程序和数据库层在你的 AVS 私有云中运行时，应用程序的 web 层在公有云中运行。

![Azure ExpressRoute 与虚拟网络的连接](media/cloudsimple-azure-network-connection.png)

## <a name="set-up-a-virtual-network-connection"></a>设置虚拟网络连接

若要设置到你的 AVS 私有云的虚拟网络连接，需要你的授权密钥、对等线路 URI 以及对你的 Azure 订阅的访问权限。 此信息可在 AVS 门户的 "虚拟网络连接" 页面上找到。 有关说明，请参阅[获取 Azure 虚拟网络到 AVS 连接的对等互连信息](virtual-network-connection.md)。 如果在获取信息时遇到任何问题，请提交<a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">支持请求</a>。

> [!TIP]
> 如果已有 Azure 虚拟网络、网关子网和虚拟网络网关，则可以跳到步骤4。

1. 在 Azure 订阅上创建一个虚拟网络，并验证你选择的地址空间与你的 AVS 私有云的地址空间不同。 如果已有 Azure 虚拟网络，可以使用现有虚拟网络。 有关详细信息，请参阅[使用 Azure 门户创建虚拟网络](../virtual-network/quick-create-portal.md)。
2. 在 Azure 虚拟网络上创建网关子网。 如果 Azure 虚拟网络中已有一个网关子网，可以使用现有的子网。 有关详细信息，请参阅[创建网关子网](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet)。
3. 在虚拟网络上创建虚拟网络网关。 如果有现有的虚拟网络网关，可以使用现有的虚拟网络网关。 有关详细信息，请参阅[创建虚拟网络网关](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway)。
4. 按照[将虚拟网络连接到线路上的其他订阅](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription)中所述，通过兑换授权密钥，创建虚拟网络与 AVS 私有云之间的连接。

> [!WARNING]
> 如果使用现有的虚拟网络网关，并且它具有与 AVS ExpressRoute 线路相同的位置的 ExpressRoute 连接，则不会建立连接。 创建新的虚拟网络并按照前面的步骤操作。

## <a name="test-the-virtual-network-connection"></a>测试虚拟网络连接

创建连接后，可以通过选择 "**设置**" 下的 "**属性**" 来检查连接状态。 状态和设置状态应显示为 "**成功**"。

![连接状态](media/azure-expressroute-connection.png)

测试虚拟网络连接：

1. 在 Azure 订阅中创建虚拟机。
2. 查找你的 AVS 私有云 vCenter 的 IP 地址（请参阅你的欢迎电子邮件）。
3. 从 Azure 虚拟网络中创建的虚拟机对云 vCenter 进行 Ping 操作。
4. 从你的 AVS 私有云 vCenter 中运行的虚拟机对 Azure 虚拟机执行 Ping 操作。

如果在建立连接时遇到任何问题，请提交<a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">支持请求</a>。
