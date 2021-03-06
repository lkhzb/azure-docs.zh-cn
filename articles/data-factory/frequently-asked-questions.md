---
title: 'Azure 数据工厂：常见问题 '
description: 获取有关 Azure 数据工厂的常见问题的解答。
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: 8238f2ea8395fc53044703db619d768918cb1834
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75644692"
---
# <a name="azure-data-factory-faq"></a>Azure 数据工厂常见问题解答
本文提供有关 Azure 数据工厂的常见问题解答。  

## <a name="what-is-azure-data-factory"></a>什么是 Azure 数据工厂？ 
数据工厂是一项完全托管的基于云的数据集成 ETL 服务，可自动执行数据移动和转换。 如同工厂运转设备将原材料转换为成品一样，Azure 数据工厂可协调现有的服务，收集原始数据并将其转换为随时可用的信息。 

使用 Azure 数据工厂可以创建数据驱动的工作流，用于在本地与云数据存储之间移动数据。 您可以处理数据并将数据转换为数据流。 ADF 还支持使用计算服务（例如 Azure HDInsight、Azure Databricks 和 SQL Server Integration Services （SSIS）集成运行时）进行手动编码转换的外部计算引擎。 

使用数据工厂，可在基于 Azure 的云服务或自己的自承载计算环境（例如 SSIS、SQL Server 或 Oracle）中执行数据处理。 创建执行所需操作的管道后，可以将其计划为定期运行（例如每小时、每天或每周）、时间窗口计划或从事件发生中触发管道。 有关详细信息，请参阅 [Azure 数据工厂简介](introduction.md)。

### <a name="control-flows-and-scale"></a>控制流和缩放 
为了支持现代数据仓库中的各种集成流和模式，数据工厂实现了灵活的数据管道建模。 这需要完全控制流编程范例，其中包括条件执行、数据管道中的分支，以及在这些流内和之间显式传递参数的功能。 控制流还包括通过复制活动将数据通过活动调度转换为外部执行引擎和数据流功能（包括大规模移动数据）。

使用数据工厂可自由地对数据集成所需的任何流样式进行建模，可按需调度或按计划重复调度这些样式。 此模型支持的几个常见流有：   

- 控制流：
    - 活动可以在管道内按顺序链接在一起。
    - 可以在管道中对活动进行分支。
    - 参数：
        - 可在管道级别定义参数，并可在按需或从触发器调用管道时传递参数。
        - 活动可以使用传递给管道的自变量。
    - 自定义状态传递：
        - 活动输出（包括状态）可由管道中的后续活动使用。
    - 循环容器：
        - Foreach 活动将循环访问循环中指定的活动集合。 
- 基于触发器的流：
    - 可以按需或按时钟时间触发管道。
- 增量流：
    - 参数可用于定义增量复制的高水位线，同时从本地或云中的关系存储移动维度或引用表，将数据加载到 lake。 

有关详细信息，请参阅[教程：控制流](tutorial-control-flow.md)。

### <a name="data-transformed-at-scale-with-code-free-pipelines"></a>大规模转换了无代码管道的数据
使用基于浏览器的新工具，可进行无代码管道创作和部署，获得现代化的、基于 Web 的交互式体验。

对于 visual data 开发人员和数据工程师，数据工厂 web UI 是用于生成管道的无代码设计环境。 它与 Visual Studio Online Git 完全集成，并为 CI/CD 和迭代开发提供调试选项集成。

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>适用于高级用户的丰富跨平台 Sdk
数据工厂 V2 提供一组丰富的 Sdk，可用于使用你最喜欢的 IDE 创作、管理和监视管道，其中包括：
* Python SDK
* PowerShell CLI
* C# SDK

用户还可以使用记录的 REST Api 与数据工厂 V2 交互。

### <a name="iterative-development-and-debugging-by-using-visual-tools"></a>使用可视化工具进行迭代开发和调试
Azure 数据工厂可视化工具可实现迭代开发和调试。 您可以通过在管道画布中使用**调试**功能来创建管道并执行测试运行，而无需编写一行代码。 可以在管道画布的 "**输出**" 窗口中查看测试运行的结果。 测试运行成功后，可以将更多活动添加到管道，并以迭代方式继续调试。 你还可以在测试运行完成后将其取消。 

在选择 "**调试**" 之前，无需将更改发布到数据工厂服务。 在您想要确保在开发、测试或生产环境中更新数据工厂工作流之前，新的添加或更改将按预期工作，这非常有用。 

### <a name="ability-to-deploy-ssis-packages-to-azure"></a>能够将 SSIS 包部署到 Azure 
如果想要移动 SSIS 工作负荷，可以创建一个数据工厂，并预配 Azure-SSIS 集成运行时。 Azure SSIS 集成运行时是一种完全托管的 Azure Vm （节点）群集，专用于在云中运行 SSIS 包。 有关分步说明，请参阅[将 SSIS 包部署到 Azure](tutorial-create-azure-ssis-runtime-portal.md) 教程。 
 
### <a name="sdks"></a>SDK
如果你是高级用户并寻找编程界面，数据工厂提供了一组丰富的 Sdk，可用于使用你最喜欢的 IDE 创作、管理或监视管道。 语言支持包括 .NET、PowerShell、Python 以及 REST。

### <a name="monitoring"></a>监视
可以通过 PowerShell、SDK 或浏览器用户界面中的可视化监视工具监视数据工厂。 可以采用高效有效的方式监视和管理按需、基于触发器和时钟驱动的自定义流。 取消现有任务，查看失败，并向下钻取以获取详细的错误消息，并调试问题，所有这些问题都可以在不进行上下文切换或在屏幕之间来回导航的单个窗格中进行。 

### <a name="new-features-for-ssis-in-data-factory"></a>数据工厂中的 SSIS 的新功能
由于2017中的初始公开预览版，数据工厂为 SSIS 添加了以下功能：

-   支持 Azure SQL 数据库的三个更多配置/变体，以托管项目/包的 SSIS 数据库（SSISDB）：
-   具有虚拟网络服务终结点的 SQL 数据库
-   托管实例
-   弹性池
-   支持在经典虚拟网络顶层使用 Azure 资源管理器虚拟网络，以使其在将来不推荐使用，这使你可以将 Azure SSIS 集成运行时插入到配置为使用虚拟网络服务终结点/MI/本地数据访问的 SQL 数据库的虚拟网络。 有关详细信息，请参阅将[AZURE SSIS 集成运行时加入到虚拟网络](join-azure-ssis-integration-runtime-virtual-network.md)。
-   支持 Azure Active Directory （Azure AD）身份验证和 SQL 身份验证连接到 SSISDB，允许通过 Azure 资源的数据工厂托管标识进行 Azure AD 身份验证
-   支持自带本地 SQL Server 许可证，通过 Azure 混合权益选项获得极大的成本节约
-   支持 Enterprise Edition 的 Azure SSIS 集成运行时，它允许你使用高级/高级功能、用于安装其他组件/扩展的自定义安装界面和合作伙伴生态系统。 有关详细信息，请参阅 "[企业版"、"自定义设置" 和 "适用于 ADF 的第三方可扩展性](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/)"。 
-   数据工厂中 SSIS 的更深层集成，使你能够在数据工厂管道中调用/触发器第一类执行 SSIS 包活动，并通过 SSMS 计划这些活动。 有关详细信息，另请参阅[在 ADF 管道中通过 SSIS 活动实现现代化处理和扩展 ETL/ELT 工作流](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)。


## <a name="what-is-the-integration-runtime"></a>什么是集成运行时？
集成运行时是 Azure 数据工厂用于在不同的网络环境中提供以下数据集成功能的计算基础结构：

- **数据移动**：对于数据移动，集成运行时在源和目标数据存储之间移动数据，同时为内置连接器、格式转换、列映射和高性能和可缩放的数据传输提供支持。
- **调度活动**：对于转换，集成运行时提供本机执行 SSIS 包的能力。
- **执行 SSIS 包**：集成运行时在托管的 Azure 计算环境中本机执行 SSIS 包。 集成运行时还支持调度和监视在各种计算服务（例如 Azure HDInsight、Azure 机器学习、SQL 数据库和 SQL Server）上运行的转换活动。

你可以根据需要部署集成运行时的一个或多个实例来移动和转换数据。 集成运行时可以在 Azure 公用网络或专用网络（本地、Azure 虚拟网络或 Amazon Web Services 虚拟私有云 [VPC]）上运行。 

有关详细信息，请参阅 [Azure 数据工厂中的集成运行时](concepts-integration-runtime.md)。

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>对集成运行时的数目有何限制？
对于可在数据工厂中使用多少个集成运行时实例，没有硬性限制。 不过，对于集成运行时在每个订阅中可用于执行 SSIS 包的 VM 核心数有限制。 有关详细信息，请参阅[数据工厂限制](../azure-resource-manager/management/azure-subscription-service-limits.md#data-factory-limits)。

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Azure 数据工厂的顶层概念有哪些？
一个 Azure 订阅可以包含一个或多个 Azure 数据工厂实例（或数据工厂）。 Azure 数据工厂包含四个关键组件，这些组件组合成为一个平台，可在其中编写数据驱动型工作流，以及用来移动和转换数据的步骤。

### <a name="pipelines"></a>管道
数据工厂可以包含一个或多个数据管道。 管道是执行任务单元的活动的逻辑分组。 管道中的活动可以共同执行一项任务。 例如，一个管道可以包含一组活动，这些活动从 Azure Blob 引入数据，并在 HDInsight 群集上运行 Hive 查询，以便对数据分区。 优点在于，可以使用管道以集的形式管理活动，而无需单独管理每个活动。 管道中的活动可以链接在一起来按顺序运行，也可以独立并行运行。

### <a name="data-flows"></a>数据流
数据流是在数据工厂中直观地生成的对象，可在后端 Spark 服务大规模转换数据。 不需要了解编程或 Spark 内部机制。 只需使用图形（映射）或电子表格（整理）设计数据转换方法。

### <a name="activities"></a>活动
活动表示管道中的处理步骤。 例如，可以使用复制活动将数据从一个数据存储复制到另一个数据存储。 同样，可以使用在 Azure HDInsight 群集上运行 Hive 查询的 Hive 活动来转换或分析数据。 数据工厂支持三种类型的活动：数据移动活动、数据转换活动和控制活动。

### <a name="datasets"></a>数据集
数据集代表数据存储中的数据结构，这些结构直接指向需要在活动中使用的数据，或者将其作为输入或输出引用。 

### <a name="linked-services"></a>链接服务
链接的服务类似于连接字符串，它定义数据工厂连接到外部资源时所需的连接信息。 以这种方式思考：链接服务定义到数据源的连接，数据集表示数据的结构。 例如，Azure 存储链接服务指定连接到 Azure 存储帐户所需的连接字符串。 和 Azure blob 数据集指定 blob 容器以及包含数据的文件夹。

数据工厂中的链接服务有两个用途：

- 代表数据存储，包括但不限于本地 SQL Server 实例、Oracle 数据库实例、文件共享或 Azure Blob 存储帐户。 有关支持的数据存储列表，请参阅 [Azure 数据工厂中的复制活动](copy-activity-overview.md)。
- 代表可托管活动执行的*计算资源*。 例如，HDInsight Hive 活动在 HDInsight Hadoop 群集上运行。 有关转换活动列表和支持的计算环境，请参阅[在 Azure 数据工厂中转换数据](transform-data.md)。

### <a name="triggers"></a>触发器
触发器表示处理单元，确定何时启动管道执行。 不同类型的事件有不同类型的触发器类型。 

### <a name="pipeline-runs"></a>管道运行
管道运行是管道执行实例。 我们通常通过将自变量传递给管道中定义的参数来实例化管道运行。 可以手动传递自变量，也可以在触发器定义中传递。

### <a name="parameters"></a>参数
参数是只读配置中的键值对。 在管道中定义参数，并在执行期间通过运行上下文传递所定义参数的自变量。 运行上下文由触发器创建，或通过手动执行的管道创建。 管道中的活动使用参数值。

数据集是强类型参数和可重用或引用的实体。 活动可以引用数据集，并且它可以使用在数据集定义中定义的属性。

链接服务也是强类型参数，其中包含数据存储或计算环境的连接信息。 它也是可重用或引用的实体。

### <a name="control-flows"></a>控制流
控制流协调管道活动。管道活动包含序列中的链接活动、分支、在管道级别定义的参数，以及按需或通过触发器调用管道时传递的自变量。 控制流还包括自定义状态传递和循环容器（即 foreach 迭代器）。


有关数据工厂概念的详细信息，请参阅以下文章：

- [数据集和链接服务](concepts-datasets-linked-services.md)
- [管道和活动](concepts-pipelines-activities.md)
- [集成运行时](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>数据工厂的定价模型是怎样的？
有关 Azure 数据工厂的定价详细信息，请参阅[数据工厂定价详细信息](https://azure.microsoft.com/pricing/details/data-factory/)。

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>如何及时了解有关数据工厂的最新信息？
有关 Azure 数据工厂的最新信息，请访问以下站点：

- [博客](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [文档主页](/azure/data-factory)
- [产品主页](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>技术深入了解 

### <a name="how-can-i-schedule-a-pipeline"></a>如何计划管道？ 
可以采用计划程序触发器或时间范围触发器来计划管道。 该触发器使用时钟日历计划，该计划可定期或基于日历的重复模式（例如，在星期一的上午6:00 和星期四 9:00 PM）计划管道。 有关详细信息，请参阅[管道执行和触发器](concepts-pipeline-execution-triggers.md)。

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>是否可以向管道传递参数？
是的，参数是数据工厂中第一类的顶级概念。 可以在管道级别定义参数，并在按需或使用触发器执行管道运行时传递自变量。  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>是否可以为管道参数定义默认值？ 
可以。 可以为管道中的参数定义默认值。 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>管道中的活动是否可以使用传递给管道运行的自变量？ 
可以。 管道中的每个活动都可以使用通过 `@parameter` 构造传递给管道运行的参数值。 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>活动输出属性是否可以在其他活动中使用？ 
可以。 在后续活动中可以通过 `@activity` 构造来使用活动输出。
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>如何得体地处理活动输出中的 NULL 值？ 
可以在表达式中使用 `@coalesce` 构造来得体地处理 NULL 值。 

## <a name="mapping-data-flows"></a>映射数据流

### <a name="i-need-help-troubleshooting-my-data-flow-logic-what-info-do-i-need-to-provide-to-get-help"></a>我需要帮助排查我的数据流逻辑问题。 若要获取帮助，需要提供哪些信息？

当 Microsoft 为数据流提供帮助或故障排除时，请提供数据流脚本。 这是来自数据流图形的代码隐藏脚本。 在 ADF UI 中，打开数据流，然后单击右上角的 "脚本" 按钮。 复制并粘贴此脚本或将其保存在文本文件中。

### <a name="how-do-i-access-data-by-using-the-other-90-dataset-types-in-data-factory"></a>使用数据工厂中的其他90数据集类型如何实现访问数据？

映射数据流功能目前允许 Azure SQL 数据库、Azure SQL 数据仓库、来自 Azure Blob 存储或 Azure Data Lake Storage Gen2 的带分隔符的文本文件，以及从 Blob 存储或本机 Data Lake Storage Gen2 的 Parquet 文件用于源和接收器。 

使用复制活动可从任何其他连接器暂存数据，然后执行数据流活动，在暂存数据后对其进行转换。 例如，管道将首先复制到 Blob 存储，然后数据流活动将使用源中的数据集来转换该数据。

### <a name="is-the-self-hosted-integration-runtime-available-for-data-flows"></a>是否可用于数据流的自承载集成运行时？

自承载 IR 是一种 ADF 管道构造，可与复制活动一起使用，以便在本地或基于 VM 的数据源和接收器之间获取或移动数据。 首先使用副本来暂存数据，然后使用数据流进行转换，然后将转换后的数据移回本地存储。

## <a name="wrangling-data-flows"></a>整理数据流

### <a name="what-are-the-supported-regions-for-wrangling-data-flow"></a>整理数据流支持哪些区域？

当前在以下区域中创建的数据工厂中支持整理数据流：

* 澳大利亚东部
* 加拿大中部
* 印度中部
* 美国中部
* 美国东部
* 美国东部 2
* 日本东部
* 北欧
* 东南亚
* 美国中南部
* 英国南部
* 美国中西部
* 西欧
* 美国西部
* 美国西部 2

### <a name="what-are-the-limitations-and-constraints-with-wrangling-data-flow"></a>整理数据流有哪些限制和约束？

数据集名称只能包含字母数字字符。 支持以下数据存储：

* 使用帐户密钥身份验证 DelimitedText Azure Blob 存储中的数据集
* 使用帐户密钥或服务主体身份验证 Azure Data Lake Storage gen2 中的 DelimitedText 数据集
* 使用服务主体身份验证 Azure Data Lake Storage gen1 中的 DelimitedText 数据集
* 使用 SQL 身份验证的 Azure SQL 数据库和数据仓库。 请参阅下面支持的 SQL 类型。 对于数据仓库，无 PolyBase 或过渡支持。

目前，整理数据流不支持链接服务 Key Vault 集成。

### <a name="what-is-the-difference-between-mapping-and-wrangling-data-flows"></a>映射和整理数据流之间的区别是什么？

映射数据流提供了一种无需任何编码即可大规模转换数据的方法。 可以通过构造一系列转换在数据流画布中设计数据转换作业。 从任意数量的源转换开始，然后是数据转换步骤。 使用接收器完成数据流，以将结果置于目标位置。 映射数据流非常适合于在接收器和源中映射和转换已知和未知架构的数据。

通过整理数据流，可通过 spark 执行大规模执行敏捷数据准备和浏览（通过使用 Power Query Online 混合编辑器）。 随着数据 lake 的增长，有时你只需浏览数据集或在 lake 中创建数据集。 你不会映射到已知目标。 整理数据流用于不太正式和基于模型的分析方案。

### <a name="what-is-the-difference-between-power-platform-dataflows-and-wrangling-data-flows"></a>Power Platform 数据流和整理数据流之间的区别是什么？

使用 Power Platform 数据流，用户可以将各种数据源中的数据导入和转换到 Common Data Service，并 Azure Data Lake 生成 PowerApps 应用程序、Power BI 报表或流自动化。 Power Platform 数据流使用已建立的 Power Query 数据准备体验，与 Power BI 和 Excel 类似。 使用 Power Platform 数据流，还可以轻松地在组织中重用，并自动处理业务流程（例如，在刷新前一数据流时，自动刷新依赖于其他数据流的数据流）。

Azure 数据工厂（ADF）是一个托管数据集成服务，它允许数据工程师和公民数据集成器创建复杂的混合提取-转换-加载（ETL）和提取-加载转换（ELT）工作流。 ADF 中的整理数据流为用户提供了一个无代码的无服务器环境，该环境可简化云中的数据准备并扩展到任何数据大小，无需基础结构管理。 它使用 Power Query 数据准备技术（在 Power Platform 数据流、Excel 和 Power BI 中也使用）来准备和调整数据。 整理数据流允许用户通过 spark 执行以大规模方式快速准备数据，旨在处理大数据集成的所有复杂和规模挑战。 用户可以使用基于浏览器的界面在可访问的视觉对象环境中构建可复原的数据管道，并让 ADF 处理 Spark 执行的复杂性。 生成管道的计划，并从 ADF 监视门户监视数据流的执行情况。 使用 ADF 丰富的可用性监视和警报轻松管理数据可用性 Sla，并利用内置的持续集成和部署功能在托管环境中保存和管理流。 建立警报并查看执行计划，以验证你的逻辑在你调整数据流时是否按计划执行。

### <a name="supported-sql-types"></a>支持的 SQL 类型

整理数据流支持 SQL 中的以下数据类型。 使用不受支持的数据类型时，将会收到验证错误。

* short
* double
* real
* FLOAT
* char
* nchar
* varchar
* nvarchar
* integer
* int
* bit
* boolean
* smallint
* tinyint
* bigint
* long
* text
* date
* datetime
* datetime2
* smalldatetime
* timestamp
* uniqueidentifier
* xml

以后将支持其他数据类型。

## <a name="next-steps"></a>后续步骤
有关创建数据工厂的分步说明，请参阅以下教程：

- [快速入门：创建数据工厂](quickstart-create-data-factory-dot-net.md)
- [教程：在云中复制数据](tutorial-copy-data-dot-net.md)
