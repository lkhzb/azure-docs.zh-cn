---
title: 用于搜索索引的 Azure SQL VM 连接
titleSuffix: Azure Cognitive Search
description: 启用加密连接，并将防火墙配置为允许从 Azure 认知搜索上的索引器连接到 Azure 虚拟机（VM）上的 SQL Server。
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 7933e2608ae0b59a6dce89169f4bb1faba0aa25e
ms.sourcegitcommit: 014e916305e0225512f040543366711e466a9495
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/14/2020
ms.locfileid: "75934147"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-server-on-an-azure-vm"></a>在 Azure VM 上配置从 Azure 认知搜索索引器到 SQL Server 的连接

如[使用索引器将 AZURE SQL 数据库连接到 azure 认知搜索](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq)中所述，azure 认知搜索支持对**azure vm 上的 SQL Server**创建索引器（或简短的**SQL Azure vm** ），但需要先处理一些与安全相关的先决条件。 

从 Azure 认知搜索到 VM 上 SQL Server 的连接是一种公共 internet 连接。 对于这些连接通常会遵循的所有安全措施在此处也适用：

+ 对于 Azure VM 上 SQL Server 实例的完全限定的域名，从[证书颁发机构提供程序](https://en.wikipedia.org/wiki/Certificate_authority#Providers)获取证书。
+ 将该证书安装在 VM 上，然后使用本文中的说明在 VM 上启用并配置加密连接。

## <a name="enable-encrypted-connections"></a>启用加密连接
Azure 认知搜索需要通过公共 internet 连接的所有索引器请求使用加密通道。 本部分列出了实现此目的的步骤。

1. 查看证书的属性，验证使用者名称是否是 Azure VM 的完全限定的域名 (FQDN)。 可以使用 CertUtils 等工具或证书管理单元查看属性。 可从 [Azure 门户](https://portal.azure.com/)中 VM 服务边栏选项卡的“基本要素”部分中获取 FQDN（位于“公共 IP 地址/DNS 名称标签”字段中）。
   
   * 对于使用较新的资源管理器模板创建的 VM，FQDN 的格式设置为 `<your-VM-name>.<region>.cloudapp.azure.com`
   * 对于创建为**经典** VM 的较旧 VM，FQDN 的格式设置为 `<your-cloud-service-name.cloudapp.net>`。

2. 使用注册表编辑器 (regedit) 将 SQL Server 配置为使用证书。 
   
    尽管 SQL Server 配置管理器通常用于此任务，但不能在此方案中使用它。 它不会查找导入的证书，因为 Azure 上 VM 的 FQDN 与该 VM（它将域标识为本地计算机或已加入到的网络域）确定的 FQDN 不匹配。 名称不匹配时，使用 regedit 指定证书。
   
   * 在 regedit 中，浏览到此注册表项：`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`。
     
     `[MSSQL13.MSSQLSERVER]` 部分因版本和实例名称而异。 
   * 将**证书**密钥的值设置为已导入到 VM 的 SSL 证书的**指纹**。
     
     可通过多种方式获取指纹，有些方式十分有效。 如果从 MMC 的**证书**管理单元中复制指纹，可能会[如此支持文章中所述](https://support.microsoft.com/kb/2023869/)选取不可见的前导字符，这会导致在尝试连接时出错。 提供了几种更正此问题的解决方法。 最简单的方法是按 Backspace 键退格，并重新键入指纹的第一个字符，以在 regedit 中删除密钥值字段中的前导字符。 此外，也可以使用其他工具复制指纹。

3. 向服务帐户授予权限。 
   
    请确保向 SQL Server 服务帐户授予 SSL 证书私钥的相应权限。 如果忽略此步骤，SQL Server 将不会启动。 可使用**证书**管理单元或 **CertUtils** 执行此任务。
    
4. 重启 SQL Server 服务。

## <a name="configure-sql-server-connectivity-in-the-vm"></a>在 VM 中配置 SQL Server 连接
设置 Azure 认知搜索所需的加密连接后，Azure Vm 上的 SQL Server 有一些其他配置步骤。 如果尚未执行这些步骤，下一步是使用以下文章之一完成配置：

* 有关 **Resource Manager** VM，请参阅[使用 Resource Manager 连接到 Azure 上的 SQL Server 虚拟机](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md)。 
* 有关**经典** VM，请参阅[连接到 Azure 上的 SQL Server 虚拟机（经典）](../virtual-machines/windows/classic/sql-connect.md)。

具体而言，查看每个文章中的“通过 Internet 连接”部分。

## <a name="configure-the-network-security-group-nsg"></a>配置网络安全组 (NSG)
若要使其他方可以访问 Azure VM，通常配置 NSG 和相应的 Azure 终结点或访问控制列表 (ACL)。 可能之前已完成此操作，以允许自己的应用程序逻辑连接到 SQL Azure VM。 对于 Azure 认知搜索连接到 SQL Azure VM，这一点并不相同。 

下面的链接提供了有关 VM 部署的 NSG 配置的说明。 根据这些说明，基于其 IP 地址为 Azure 认知搜索终结点启用 ACL。

> [!NOTE]
> 有关背景知识，请参阅[什么是网络安全组？](../virtual-network/security-overview.md)
> 
> 

* 有关 **Resource Manager** VM，请参阅[如何为 ARM 部署创建 NSG](../virtual-network/tutorial-filter-network-traffic.md)。 
* 有关**经典** VM，请参阅[如何为经典部署创建 NSG](../virtual-network/virtual-networks-create-nsg-classic-ps.md)。

IP 寻址会产生一些挑战，如果了解问题和潜在解决方法，则可以轻松应对。 剩余部分提供了有关处理 ACL 中与 IP 地址相关的问题的建议。

#### <a name="restrict-access-to-the-azure-cognitive-search"></a>限制对 Azure 认知搜索的访问
我们强烈建议你限制对搜索服务的 IP 地址的访问和 ACL 中 `AzureCognitiveSearch`[服务标记](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags)的 ip 地址范围，而不是使 SQL Azure vm 对所有连接请求开放。

可以通过对搜索服务的 FQDN （例如 `<your-search-service-name>.search.windows.net`）进行 ping 操作来找出 IP 地址。

可以通过使用[可下载的 JSON 文件](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files)或通过[服务标记发现 API](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api-public-preview)来找出 `AzureCognitiveSearch`[服务标记](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags)的 IP 地址范围。 IP 地址范围每周更新一次。

#### <a name="managing-ip-address-fluctuations"></a>管理 IP 地址波动
如果搜索服务只有一个搜索单位（即一个副本和一个分区），IP 地址会在例程服务重新启动期间发生更改，这会导致搜索服务的 IP 地址的现有 ACL 无效。

避免后续连接错误的一种方法是在 Azure 认知搜索中使用多个副本和一个分区。 这样做会增加成本，但也会解决 IP 地址问题。 在 Azure 认知搜索中，当你有多个搜索单位时，IP 地址不会更改。

第二种方法是允许连接失败，并在 NSG 中重新配置 ACL。 一般情况下，IP 地址应每隔几周更改一次。 对于不常执行受控编制索引的客户，此方法可能可行。

第三个可行（但不是特别安全）的方法是指定预配搜索服务的 Azure 区域的 IP 地址范围。 将公共 IP 地址分配到 Azure 资源时所依据的 IP 范围列表已在 [Azure 数据中心 IP 范围](https://www.microsoft.com/download/details.aspx?id=41653)中发布。 

#### <a name="include-the-azure-cognitive-search-portal-ip-addresses"></a>包括 Azure 认知搜索门户 IP 地址
如果使用 Azure 门户创建索引器，Azure 认知搜索门户逻辑还需要在创建时访问 SQL Azure VM。 可以通过对 `stamp2.search.ext.azure.com`进行 ping 操作来找到 Azure 认知搜索门户 IP 地址。

## <a name="next-steps"></a>后续步骤
使用配置时，你现在可以将 Azure VM 上的 SQL Server 指定为 Azure 认知搜索索引器的数据源。 有关详细信息，请参阅[将 AZURE SQL 数据库连接到 azure 认知搜索使用索引器](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)。

