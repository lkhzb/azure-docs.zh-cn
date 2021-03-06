---
title: 用于管理 Azure VMware 解决方案（AVS）的主要概念
description: 介绍用于管理 Azure VMware 解决方案（AVS）的主要概念
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c581ffbba7fbaefc8fbd0ffc7e92350bd69802b8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025225"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solutions-avs"></a>Azure VMware 解决方案管理的关键概念（AVS）

管理 Azure VMware 解决方案（AVS）需要了解以下概念：

* AVS 服务，它显示为 Azure VMware 解决方案（AVS）-服务
* AVS 节点，它显示为 Azure VMware 解决方案（AVS）节点
* AVS 私有云
* 服务网络
* 显示为 Azure VMware 解决方案（AVS）虚拟机的 AVS 虚拟机

## <a name="avs-service"></a>AVS 服务

利用 AVS 服务，你可以从 Azure 门户创建和管理与 VMware 解决方案（AVS）关联的所有资源。 在要使用服务的每个区域中创建一个服务资源。

了解有关[AVS 服务](cloudsimple-service.md)的详细信息。

## <a name="avs-node"></a>AVS 节点

AVS 节点是部署 VMware ESXi 虚拟机监控程序的专用、裸机、超聚合计算和存储主机。 然后，将此节点合并到 VMware vSphere、vCenter、vSAN 和 NSX 平台。 还启用了 AVS 网络服务和边缘网络服务。 每个节点都可用作计算和存储容量的单元，你可以预配它来创建[AVS 私有云](cloudsimple-private-cloud.md)。 在 AVS 服务可用的区域中预配或保留节点。

了解有关[AVS 节点](cloudsimple-node.md)的详细信息。

## <a name="avs-private-cloud"></a>AVS 私有云

AVS 私有云是由 vCenter server 在其自己的管理域中管理的独立 VMware 堆栈环境。 VMware 堆栈包括 ESXi 主机、vSphere、vCenter、vSAN 和 NSX。 堆栈在专用节点上运行（专用和隔离裸机硬件），用户通过包含 vCenter 和 NSX 管理器的本机 VMware 工具来使用。 专用节点部署在 Azure 位置，由 Azure 管理。 每个 AVS 私有云都可以通过使用网络服务（例如 Vlan 和子网和防火墙表）进行分段和保护。 通过使用安全、专用 VPN 和 Azure ExpressRoute 连接来创建与本地环境和 Azure 网络的连接。

了解有关[AVS 私有云](cloudsimple-private-cloud.md)的详细信息。

## <a name="service-networking"></a>服务网络

AVS 服务为每个区域提供一个在其中部署了 AVS 服务的网络。 网络是单个 TCP 第3层地址空间，默认情况下启用路由。 在此区域中创建的所有 AVS 私有云和子网都无需任何其他配置即可相互通信。 使用 Vlan 在 vCenter 上创建分布式端口组。 你可以使用以下网络功能来配置和保护你的 AVS 私有云中的工作负荷资源：

* [Vlan 和子网](cloudsimple-vlans-subnets.md)
* [防火墙表](cloudsimple-firewall-tables.md)
* [VPN 网关](cloudsimple-vpn-gateways.md)
* [公共 IP](cloudsimple-public-ip-address.md)
* [Azure 网络连接](cloudsimple-azure-network-connection.md)

## <a name="avs-virtual-machine"></a>AVS 虚拟机

利用 AVS 服务，你可以从 Azure 门户管理 VMware 虚拟机。 你的 vSphere 环境中的一个或多个群集或资源池可映射到创建该服务的订阅。

了解有关以下方面的详细信息：

* [AVS 虚拟机](cloudsimple-virtual-machines.md)
* [Azure 订阅映射](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
