---
title: Azure 认可的 Linux 分发
description: 了解 Azure 认可的分发中的 Linux，包括 Ubuntu、CentOS、Oracle 和 SUSE 的指南。
services: virtual-machines-linux
documentationcenter: ''
author: MicahMcKittrick-MSFT
manager: gwallace
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 11/04/2019
ms.author: mimckitt
ms.openlocfilehash: 8f12224f6ea2b9b4cecce79809389419e0159217
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2020
ms.locfileid: "75748052"
---
# <a name="endorsed-linux-distributions-on-azure"></a>Azure 上认可的 Linux 分发
合作伙伴在 Azure 市场中提供了 Linux 映像。 我们正积极与各大 Linux 社区合作，争取为认可分发列表添加更多成员。 在此期间，对于市场未提供的分发，用户始终可以按照[创建并上传包含 Linux 操作系统的虚拟硬盘](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)中的准则安装自己的 Linux。

## <a name="supported-distributions-and-versions"></a>支持的分发和版本
下表列出了 Azure 支持的 Linux 分发和版本。 请参阅[Microsoft Azure 中对 linux 映像的支持](https://support.microsoft.com/help/2941892/support-for-linux-and-open-source-technology-in-azure)，了解有关 Azure 中 linux 和开源技术的支持的更多详细信息。

Hyper-V 和 Azure 的 Linux 集成服务 (LIS) 驱动程序是 Microsoft 直接为上游 Linux 内核提供的内核模块。  某些 LIS 驱动程序默认内置于分发的内核中。 基于 Red Hat Enterprise (RHEL)/CentOS 的早期分发在[用于 Hyper-V 和 Azure 的 Linux Integration Services 版本 4.2](https://www.microsoft.com/download/details.aspx?id=55106) 以单独下载的形式提供。 有关 LIS 驱动程序的详细信息，请参阅 [Linux 内核要求](create-upload-generic.md#linux-kernel-requirements)。

Azure Linux 代理已预安装在 Azure 市场映像中，通常可从分发的包存储库中获得。 源代码可在 [GitHub](https://github.com/azure/walinuxagent) 上找到。


| 分配 | 版本 | 驱动程序 | 代理 |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3 +、7.0 +、8.0 + |CentOS 6.3：[LIS 下载](https://www.microsoft.com/download/details.aspx?id=55106)<p>CentOS 6.4+：在内核中 |包：在“WALinuxAgent”下的[存储库](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/)中 <br/>源代码：[GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |在内核中 |源代码：[GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7.9 +，8.2 +，9，10 |在内核中 |包：在“waagent”下的存储库中 <br/>源代码：[GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+、7.0+ |在内核中 |包：在“WALinuxAgent”下的存储库中 <br/>源代码：[GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 +、7.1 +、8.0 + |在内核中 |包：在“WALinuxAgent”下的存储库中 <br/>源代码：[GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES for SAP<br>11 SP4<br>12 SP1+<br>15|在内核中 |包：<p> 对于 11，在 [Cloud:Tools](https://build.opensuse.org/project/show/Cloud:Tools) 存储库中<br>对于 12，包含在“公有云”模块中的“python-azure-agent”下<br/>源代码：[GitHub](https://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE Leap 42.2+ |在内核中 |包：在“python-azure-agent”下的 [Cloud:Tools](https://build.opensuse.org/project/show/Cloud:Tools) 存储库中 <br/>源代码：[GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04+ **<sup>1</sup>** |在内核中 |包：在“walinuxagent”下的存储库中 <br/>源代码：[GitHub](https://github.com/Azure/WALinuxAgent) |

  - **<sup>1</sup>** 有关 ubuntu 12.04 和14.04 扩展支持的信息，请参阅： [Ubuntu 扩展安全维护](https://www.ubuntu.com/esm)。


## <a name="image-update-cadence"></a>映像更新节奏
Azure 要求已认可的 Linux 分发版的发布者定期使用最新的修补程序和安全修补程序在 Azure Marketplace 中更新其映像，每季度或更快。 Azure Marketplace 中更新后的映像将自动提供给客户，作为新版本的映像 SKU。 有关如何查找 Linux 映像的详细信息：[在 Azure Marketplace 中查找 LINUX VM 映像](https://docs.microsoft.com/azure/virtual-machines/linux/cli-ps-findimage)。

### <a name="additional-links"></a>更多链接
 - [SUSE 公有云映像生命周期](https://www.suse.com/c/suse-public-cloud-image-life-cycle/)

## <a name="azure-tuned-kernels"></a>Azure 优化内核

Azure 与各种认可的 Linux 发行版密切合作，以优化发布到 Azure Marketplace 的映像。 这种协作的一个方面是开发 "优化的" Linux 内核，这些内核针对 Azure 平台进行了优化，并作为 Linux 分发版的完全支持组件提供。 Azure 优化内核结合了新功能和性能改进，并且与分发中可用的默认或通用内核相比，速度更快（通常是每季度）节奏。

在大多数情况下，你会发现在 Azure Marketplace 中的默认映像上预先安装了这些内核，因此 Azure 客户将立即获得这些优化内核的好处。 有关这些 Azure 优化内核的详细信息，请参阅以下链接：

 - CentOS Azure 优化内核-可通过 CentOS 虚拟化 SIG 获取-[详细信息](https://wiki.centos.org/SpecialInterestGroup/Virtualization)
 - Debian Cloud 内核-适用于 Azure 上的 Debian 10 和 Debian 9 "precise-backports" 映像-[详细信息](https://wiki.debian.org/Cloud/MicrosoftAzure)
 - SLES Azure 优化内核-[详细信息](https://www.suse.com/c/a-different-builtin-kernel-for-azure-on-demand-images/)
 - Ubuntu Azure 优化内核-[详细信息](https://blog.ubuntu.com/2017/09/21/microsoft-and-canonical-increase-velocity-with-azure-tailored-kernel)


## <a name="partners"></a>合作伙伴

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

从 CoreOS 网站：

*CoreOS 旨在实现安全性、一致性和可靠性。CoreOS 使用 Linux 容器在更高的抽象级别管理服务，而不是通过 yum 或 apt 安装包。单个服务的代码和所有依赖项都打包在一个容器中，该容器可以在一个或多个 CoreOS 计算机上运行。*

### <a name="credativ"></a>Credativ
[https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure](https://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ 是一家独立的咨询和服务公司，致力于使用免费软件开发和实施专业解决方案。 Credativ 是获得国际认可的开源领域专业先行者，为许多公司的 IT 部门提供支持。 Credativ 与 Microsoft 合作，目前正在为 Debian 8 (Jessie) 以及 Debian 7 (Wheezy) 之前的版本准备相应的 Debian 映像。 这些映像经过专门的设计，可以在 Azure 上运行并通过该平台轻松进行管理。 Credativ 还会通过其开源支持中心为 Azure 的 Debian 映像的维护和更新提供长期支持。

### <a name="oracle"></a>Oracle
[https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html](https://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracle 的策略是为公有云和私有云提供广泛的解决方案， 让客户面临如何在 Oracle 云以及其他云中部署 Oracle 软件这一问题时拥有更多选择权和灵活性。 Oracle 与 Microsoft 的合作关系使客户能够通过来自 Oracle 的认证和支持的自信在 Microsoft 公有和私有云中部署 Oracle 软件。  Oracle 对 Oracle 公有和私有云的承诺和投资保持不变。

### <a name="red-hat"></a>Red Hat
[https://www.redhat.com/en/partners/strategic-alliance/microsoft](https://www.redhat.com/en/partners/strategic-alliance/microsoft)

作为世界上领先的开源解决方案提供商，Red Hat 帮助 90% 以上的财富 500 强公司解决业务难题、调整 IT 与业务策略，以及为未来技术做准备。 Red Hat 通过开放式业务模型和价格合理、可预测的订阅模型提供安全的解决方案，从而实现了此目的。

### <a name="suse"></a>SUSE
[https://www.suse.com/suse-linux-enterprise-server-on-azure](https://www.suse.com/suse-linux-enterprise-server-on-azure)

Azure 上的 SUSE Linux Enterprise Server 是一个已验证的平台，该平台为云计算提供了高级可靠性和安全性。 SUSE 的通用 Linux 平台可与 Azure 云服务无缝集成以便交付易于管理的云环境。 借助 1,800 多个独立软件供应商提供的适用于 SUSE Linux Enterprise Server 的 9,200 多个认证应用程序，SUSE 可确保满怀信心地在 Azure 上部署数据中心内支持的运行工作负荷。

### <a name="canonical"></a>Canonical
[https://www.ubuntu.com/cloud/azure](https://www.ubuntu.com/cloud/azure)

Canonical 工程和开放社区监管对 Ubuntu 在客户端、服务器和云计算（包括用户的个人云服务）方面获得成功起到了推动作用。 Canonical 期望使用 Ubuntu 开发一个统一的免费平台（从手机到云），该平台带有一系列适用于手机、平板电脑、TV 和桌面的相关接口， 从而使 Ubuntu 成为各种机构（从公有云提供商到消费类电子产品制造商）的首选以及各个技术专家的最爱。

借助遍布全球的开发人员和工程中心这一独特优势，Canonical 将与硬件制造商、内容提供商和软件开发人员通力合作，将 Ubuntu 解决方案推向 PC、服务器和手持设备市场。
