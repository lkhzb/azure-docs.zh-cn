---
title: Azure 虚拟机规模集的安全控制
description: 用于评估 Azure 虚拟机规模集的安全控制清单
ms.service: virtual-machine-scale-sets
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: 51ddd4f3775ae8e41c0d0bdd4ed8a3a2a59b7db4
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/19/2020
ms.locfileid: "76271781"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Azure 虚拟机规模集的安全控制

本文介绍 Azure 虚拟机规模集中内置的安全控制。

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>网络

| 安全控制 | Yes/No | 说明 |
|---|---|--|
| 服务终结点支持| 是 | |
| VNet 注入支持| 是 | |
| 网络隔离和防火墙支持| 是 |  |
| 强制隧道支持| 是 | 请参阅[使用 Azure 资源管理器部署模型配置强制隧道](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm)。 |

## <a name="monitoring--logging"></a>监视 & 日志记录

| 安全控制 | Yes/No | 说明|
|---|---|--|
| Azure 监视支持（Log analytics、App insights 等）| 是 | 请参阅[在 azure 中监视和更新 Linux 虚拟机](/azure/virtual-machines/linux/tutorial-monitoring)和[在 azure 中监视和更新 Windows 虚拟机](/azure/virtual-machines/windows/tutorial-monitoring)。 |
| 控制和管理平面日志记录和审核| 是 |  |
| 数据平面日志记录和审核 | 否 |  |

## <a name="identity"></a>身份标识

| 安全控制 | Yes/No | 说明|
|---|---|--|
| 身份验证| 是 |  |
| 授权| 是 |  |

## <a name="data-protection"></a>数据保护

| 安全控制 | Yes/No | 说明 |
|---|---|--|
| 服务器端加密： Microsoft 托管密钥 | 是 | 请参阅[虚拟机规模集的 Azure 磁盘加密](disk-encryption-overview.md)。 |
| 传输中的加密（如 ExpressRoute 加密、VNet 加密和 VNet-VNet 加密）| 是 | Azure 虚拟机支持[ExpressRoute](/azure/expressroute)和 VNet 加密。 请参阅[Vm 中的传输中加密](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)。 |
| 服务器端加密：客户托管的密钥（BYOK） | 是 | 客户托管的密钥是受支持的 Azure 加密方案;请参阅查看[虚拟机规模集的 Azure 磁盘加密](disk-encryption-overview.md)|
| 列级加密（Azure 数据服务）| N/A | |
| 加密的 API 调用| 是 | 通过 HTTPS 和 SSL。 |

## <a name="configuration-management"></a>配置管理

| 安全控制 | Yes/No | 说明|
|---|---|--|
| 配置管理支持（配置的版本控制等）| 是 |  | 

## <a name="next-steps"></a>后续步骤

- 详细了解[Azure 服务中的内置安全控件](../security/fundamentals/security-controls.md)。
