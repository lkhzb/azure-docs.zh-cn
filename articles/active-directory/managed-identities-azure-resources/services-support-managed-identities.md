---
title: Azure Services that support managed identities - Azure AD
description: 支持 Azure 资源托管标识和 Azure AD 身份验证的服务列表
services: active-directory
author: MarkusVi
ms.author: markvi
ms.date: 09/24/2019
ms.topic: conceptual
ms.service: active-directory
ms.subservice: msi
manager: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0b79a27526054f76d9d44e277c401e93214ec3c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2020
ms.locfileid: "77018697"
---
# <a name="services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的服务

Azure 资源的托管标识在 Azure Active Directory 中为 Azure 服务提供了一个自动托管标识。 使用托管标识，可对支持 Azure AD 身份验证的任何服务进行身份验证，而无需在代码中插入凭据。 我们正在跨 Azure 将 Azure 资源托管标识与 Azure AD 身份验证集成。 请经常再回来看看，确定这部分内容是否有更新。

> [!NOTE]
> Azure 资源托管标识是以前称为托管服务标识 (MSI) 的服务的新名称。

## <a name="azure-services-that-support-managed-identities-for-azure-resources"></a>支持 Azure 资源托管标识的 Azure 服务

以下 Azure 服务支持 Azure 资源托管标识：

### <a name="azure-virtual-machines"></a>Azure 虚拟机

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 预览 | 预览 | 预览 | 
| 用户分配 | 可用 | 预览 | 预览 | 预览 |

请参阅以下列表来配置 Azure 虚拟机的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure 资源管理器模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-virtual-machine-scale-sets"></a>Azure 虚拟机规模集

|托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 预览 | 预览 | 预览 |
| 用户分配 | 可用 | 预览 | 预览 | 预览 |

请参阅以下列表来配置 Azure 虚拟机规模集的托管标识（在可用的区域中）：

- [Azure 门户](qs-configure-portal-windows-vm.md)
- [PowerShell](qs-configure-powershell-windows-vm.md)
- [Azure CLI](qs-configure-cli-windows-vm.md)
- [Azure 资源管理器模板](qs-configure-template-windows-vm.md)
- [REST](qs-configure-rest-vm.md)

### <a name="azure-app-service"></a>Azure App Service

| 托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 可用 | 可用 |
| 用户分配 | 可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 应用服务的托管标识（在可用的区域中）：

- [Azure 门户](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager 模板](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-blueprints"></a>Azure 蓝图

|托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 不可用 | 不可用 |
| 用户分配 | 可用 | 可用 | 不可用 | 不可用 |

Refer to the following list to use a managed identity with [Azure Blueprints](../../governance/blueprints/overview.md):

- [Azure portal - blueprint assignment](../../governance/blueprints/create-blueprint-portal.md#assign-a-blueprint)
- [REST API - blueprint assignment](../../governance/blueprints/create-blueprint-rest-api.md#assign-a-blueprint)

### <a name="azure-functions"></a>Azure Functions

托管标识类型 |所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 可用 | 可用 |
| 用户分配 | 可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure Functions 的托管标识（在可用的区域中）：

- [Azure 门户](/azure/app-service/overview-managed-identity#using-the-azure-portal)
- [Azure CLI](/azure/app-service/overview-managed-identity#using-the-azure-cli)
- [Azure PowerShell](/azure/app-service/overview-managed-identity#using-azure-powershell)
- [Azure Resource Manager 模板](/azure/app-service/overview-managed-identity#using-an-azure-resource-manager-template)

### <a name="azure-logic-apps"></a>Azure 逻辑应用

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 预览 | 预览 | 不可用 | 预览 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 逻辑应用的托管标识（在可用的区域中）：

- [Azure 门户](/azure/logic-apps/create-managed-service-identity#enable-system-assigned-identity-in-azure-portal)
- [Azure Resource Manager 模板](/azure/app-service/overview-managed-identity)

### <a name="azure-data-factory-v2"></a>Azure 数据工厂 V2

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 数据工厂 V2 的托管标识（在可用的区域中）：

- [Azure 门户](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity)
- [PowerShell](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-powershell)
- [REST](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-rest-api)
- [SDK](~/articles/data-factory/data-factory-service-identity.md#generate-managed-identity-using-sdk)

### <a name="azure-api-management"></a>Azure API 管理

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 可用 | 不可用 | 不可用 |
| 用户分配 | 不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure API 管理的托管标识（在可用的区域中）：

- [Azure Resource Manager 模板](/azure/api-management/api-management-howto-use-managed-service-identity)

### <a name="azure-container-instances"></a>Azure 容器实例

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | Linux：预览版<br>Windows：不可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | Linux：预览版<br>Windows：不可用 | 不可用 | 不可用 | 不可用 |

请参阅以下列表来配置 Azure 容器实例的托管标识（在可用的区域中）：

- [Azure CLI](~/articles/container-instances/container-instances-managed-identity.md)
- [Azure Resource Manager 模板](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-resource-manager-template)
- [YAML](~/articles/container-instances/container-instances-managed-identity.md#enable-managed-identity-using-yaml-file)

### <a name="azure-container-registry-tasks"></a>Azure 容器注册表任务

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 不可用 | 不可用 | 不可用 |
| 用户分配 | 预览 | 不可用 | 不可用 | 不可用 |

Refer to the following list to configure managed identity for Azure Container Registry Tasks (in regions where available):

- [Azure CLI](~/articles/container-registry/container-registry-tasks-authentication-managed-identity.md)

### <a name="azure-service-fabric"></a>Azure Service Fabric
[Managed Identity for Service Fabric Applications](https://docs.microsoft.com/azure/service-fabric/concepts-managed-identity) is in Preview and available in all regions.

托管标识类型 | 所有正式发布版<br>全球 Azure 区域 | Azure Government | Azure Germany | Azure 中国世纪互联 |
| --- | --- | --- | --- | --- |
| 系统分配 | 可用 | 不可用 | 不可用 | not Available |
| 用户分配 | 可用 | 不可用 | 不可用 |不可用 |

Refer to the following list to configure managed identity for Azure Service Fabric applications in all regions:
- [Azure Resource Manager 模板](https://github.com/Azure-Samples/service-fabric-managed-identity/tree/anmenard-docs)

## <a name="azure-services-that-support-azure-ad-authentication"></a>支持 Azure AD 身份验证的 Azure 服务

以下服务支持 Azure AD 身份验证，已通过使用 Azure 资源托管标识的客户端服务进行测试。

### <a name="azure-resource-manager"></a>Azure Resource Manager

Refer to the following list to configure access to Azure Resource Manager:

- [Assign access via Azure portal](howto-assign-access-portal.md)
- [Assign access via Powershell](howto-assign-access-powershell.md)
- [Assign access via Azure CLI](howto-assign-access-CLI.md)
- [Assign access via Azure Resource Manager template](../../role-based-access-control/role-assignments-template.md)

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://management.azure.com/`| 可用 |
| Azure Government | `https://management.usgovcloudapi.net/` | 可用 |
| Azure Germany | `https://management.microsoftazure.de/` | 可用 |
| Azure 中国世纪互联 | `https://management.chinacloudapi.cn` | 可用 |

### <a name="azure-key-vault"></a>Azure Key Vault

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://vault.azure.net`| 可用 |
| Azure Government | `https://vault.usgovcloudapi.net` | 可用 |
| Azure Germany |  `https://vault.microsoftazure.de` | 可用 |
| Azure 中国世纪互联 | `https://vault.azure.cn` | 可用 |

### <a name="azure-data-lake"></a>Azure Data Lake 

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://datalake.azure.net/` | 可用 |
| Azure Government |  | 不可用 |
| Azure Germany |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |

### <a name="azure-sql"></a>Azure SQL 

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://database.windows.net/` | 可用 |
| Azure Government | `https://database.usgovcloudapi.net/` | 可用 |
| Azure Germany | `https://database.cloudapi.de/` | 可用 |
| Azure 中国世纪互联 | `https://database.chinacloudapi.cn/` | 可用 |

### <a name="azure-event-hubs"></a>Azure 事件中心

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://eventhubs.azure.net` | 可用 |
| Azure Government |  | 不可用 |
| Azure Germany |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |

### <a name="azure-service-bus"></a>Azure 服务总线

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://servicebus.azure.net`  | 可用 |
| Azure Government |  | 可用 |
| Azure Germany |   | 不可用 |
| Azure 中国世纪互联 |  | 不可用 |









### <a name="azure-storage-blobs-and-queues"></a>Azure Storage blobs and queues

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://storage.azure.com/` <br /><br />`https://<account>.blob.core.windows.net` <br /><br />`https://<account>.queue.core.windows.net` | 可用 |
| Azure Government | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.usgovcloudapi.net` <br /><br />`https://<account>.queue.core.usgovcloudapi.net` | 可用 |
| Azure Germany | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.cloudapi.de` <br /><br />`https://<account>.queue.core.cloudapi.de` | 可用 |
| Azure 中国世纪互联 | `https://storage.azure.com/`<br /><br />`https://<account>.blob.core.chinacloudapi.cn` <br /><br />`https://<account>.queue.core.chinacloudapi.cn` | 可用 |










### <a name="azure-analysis-services"></a>Azure Analysis Services

| 云 | 资源 ID | 状态 |
|--------|------------|--------|
| Azure 全球 | `https://*.asazure.windows.net` | 可用 |
| Azure Government | `https://*.asazure.usgovcloudapi.net` | 可用 |
| Azure Germany | `https://*.asazure.cloudapi.de` | 可用 |
| Azure 中国世纪互联 | `https://*.asazure.chinacloudapi.cn` | 可用 |
