---
title: Azure VMware 解决方案（AVS）-适用于 AVS 服务的安全性
description: 介绍适用于 AVS 服务安全性的共享责任模型
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d2d45f827d4165175a4a5429f0b9393a55e2ff1e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024987"
---
# <a name="avs-security-overview"></a>AVS 安全概述

本文概述了如何通过 AVS 服务、基础结构和数据中心实现 Azure VMware 解决方案的安全性。 了解数据保护和安全性、网络安全以及如何管理漏洞和修补程序。

## <a name="shared-responsibility"></a>共担责任

Azure VMware 解决方案由 AVS 使用共享的责任模式来保证安全性。 云中的可信安全是通过客户和 Microsoft 服务提供商的共享责任实现的。 此责任矩阵提供更高的安全性，并消除了单一故障点。

## <a name="azure-infrastructure"></a>Azure 基础结构

Azure 基础结构安全注意事项包括数据中心和设备位置。

### <a name="datacenter-security"></a>数据中心安全性

Microsoft 拥有一个用于设计、构建和运营支持 Azure 的物理设施的整个部门。 此团队在维持一流物理安全性方面投入了大量的人力物力。 有关物理安全的详细信息，请参阅[Azure 设施、本地和物理安全性](../security/azure-physical-security.md)。

### <a name="equipment-location"></a>设备位置

运行 AVS 私有云的裸机硬件设备托管在 Azure 数据中心位置。 设备所在的 cages 需要基于生物识别的双因素身份验证才能获得访问权限。

## <a name="dedicated-hardware"></a>专用硬件

作为 AVS 服务的一部分，所有 AVS 客户都将获得具有本地附加磁盘的专用裸机主机，这些主机与其他租户硬件物理隔离。 在每个节点上运行包含 vSAN 的 ESXi 虚拟机监控程序。 节点通过客户专用 VMware vCenter 和 NSX 进行管理。 不在租户之间共享硬件还能提供额外的隔离和安全保护层。

## <a name="data-security"></a>数据安全

客户保持其数据的控制和所有权。 客户数据的数据保管是客户的责任。

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>对静态数据和内部网络中的数据进行数据保护

对于 AVS 私有云环境中的静态数据，可以使用 vSAN 加密。 vSAN 加密适用于你自己的虚拟网络或本地中的 VMware 认证的外部密钥管理服务器（KMS）。 你自己控制数据加密密钥。 对于在 AVS 私有云内动作中的数据，vSphere 支持通过网络加密所有 vmkernel 流量（包括 vMotion 流量）的数据。

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>数据保护，适用于在公用网络中移动所需的数据

若要保护通过公用网络移动的数据，可以为你的 AVS 私有云创建 IPsec 和 SSL VPN 隧道。 支持通用加密方法，其中包括128和256字节的 AES。 传输中的数据（包括身份验证、管理访问和客户数据）使用标准加密机制（SSH、TLS 1.2 和安全 RDP）进行加密。 传输敏感信息的通信使用标准加密机制。

### <a name="secure-disposal"></a>安全处置

如果你的 AVS 服务过期或已终止，你将负责删除或删除数据。 AVS 将与你合作来删除或返回客户协议中提供的所有客户数据，除非适用法律要求使用 AVS 来保留部分或全部个人数据。 如果需要保留任何个人数据，AVS 会将数据存档并实现合理的措施，以防止客户数据被进一步处理。

### <a name="data-location"></a>数据位置

设置你的 AVS 私有云时，你可以选择将在其中部署你的 AVS 的 Azure 区域。 除非执行数据迁移或场外数据备份，否则不会从该物理数据中心移动 VMware 虚拟机数据。 如果适合需要，还可以在多个 Azure 区域内托管工作负荷并存储数据。

在无需租户管理员显式操作的情况下，在 AVS 私有云超聚合节点中驻留的客户数据不会遍历位置。 以高度可用的方式实现工作负荷是您的责任。

### <a name="data-backups"></a>数据备份

AVS 不会备份或存档客户数据。 AVS 会定期备份 vCenter 和 NSX 数据，以提供管理服务器的高可用性。 在备份之前，将使用 VMware Api 在 vCenter 源对所有数据进行加密。 加密的数据会传输并存储在 Azure blob 中。 备份的加密密钥存储在 Azure 中的 AVS 虚拟网络上运行的高度安全的 AVS 托管保管库中。

## <a name="network-security"></a>网络安全

AVS 解决方案依赖于网络安全层。

### <a name="azure-edge-security"></a>Azure 边缘安全

AVS 服务构建在 Azure 提供的基本网络安全基础之上。 Azure 应用深层防御技术来检测和及时响应与异常进出流量模式和分布式拒绝服务（DDoS）攻击相关的基于网络的攻击。 此安全控制适用于 avs 私有云环境和由 AVS 开发的控制平面软件。

### <a name="segmentation"></a>分段

AVS 服务在逻辑上分离了第2层网络，在你的 AVS 私有云环境中限制对你自己的专用网络的访问。 你可以使用防火墙进一步保护你的 AVS 私有云网络。 通过 AVS 门户，你可以为所有网络流量定义 "新建" 和 "NS 网络流量" 规则，包括在 AVS 的私有云流量中、AVS 间私有云流量、到 Internet 的一般流量和通过 IPsec VPN 的本地流量和网络流量ExpressRoute 连接。

## <a name="vulnerability-and-patch-management"></a>漏洞和修补程序管理

AVS 负责对托管 VMware 软件（ESXi、vCenter 和 NSX）进行定期安全修补。

## <a name="identity-and-access-management"></a>身份和访问管理

客户可使用多重身份验证或 SSO 作为首选来向其 Azure 帐户（在 Azure AD 中）进行身份验证。 在 Azure 门户中，可以启动 AVS 门户，而无需重新登录凭据。

AVS 支持适用于 AVS 私有云 vCenter 标识源的可选配置。 你可以使用[本地标识源](set-vcenter-identity.md)、适用于 AVS 私有云的新标识源或[Azure AD](azure-ad.md)。

默认情况下，会向客户提供在 AVS 私有云中的 vCenter 日常操作所需的特权。 此权限级别不包括对 vCenter 的管理访问权限。 如果临时需要管理访问权限，你可以在完成管理任务时，在有限的时间内[提升你的权限](escalate-private-cloud-privileges.md)。
