---
title: 训练聚类模型：模块引用
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习中的 "定型聚类分析模型" 模块来定型聚类分析模型。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 11/19/2019
ms.openlocfilehash: 6b154cdbf6490abd935156e6d081d2260cfbc578
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719358"
---
# <a name="train-clustering-model"></a>训练聚类模型

本文介绍 Azure 机器学习设计器（预览版）中的模块。

使用此模块可以定型聚类分析模型。

该模块采用了一个已使用[K 平均值聚类分析](k-means-clustering.md)模块进行配置的未训练的聚类模型，并使用标签或未标记的数据集训练该模型。 该模块创建一个可用于预测的定型模型，以及针对定型数据中每个事例的一组群集分配。

> [!NOTE]
> 聚类分析模型无法使用 "[训练模型](train-model.md)" 模块定型，后者是用于训练机器学习模型的通用模块。 这是因为[训练模型](train-model.md)仅适用于监督学习算法。 K 平均值和其他聚类分析算法允许无人监督学习，这意味着该算法可以从未标记的数据中进行学习。  
  
## <a name="how-to-use-train-clustering-model"></a>如何使用定型聚类模型  

1.  在设计器中将 "**训练聚类模型**" 模块添加到管道。 可以在 "**训练**" 类别的 "**机器学习模块**" 下找到该模块。  
  
2. 添加[K 平均值聚类分析](k-means-clustering.md)模块或另一个创建兼容的聚类分析模型的自定义模块，并设置聚类分析模型的参数。  
    
3.  将定型数据集附加到**定型聚类分析模型**的右侧输入。
  
5.  在 "**列集**" 中，选择要在生成群集中使用的数据集中的列。 请确保选择可提供良好功能的列：例如，避免使用 Id 或其他具有唯一值的列或具有相同值的列。

    如果标签可用，可以将其用作一项功能，或将其保留。  
  
6. 如果要将定型数据与新的群集标签一起输出，请选择选项 "**仅检查追加或取消选中结果**"。

    如果取消选择此选项，则仅输出群集分配。 

7. 运行管道，或单击 "**训练聚类模型**" 模块并选择 "**运行所选项**"。  
  
### <a name="results"></a>结果

完成训练后：

+ 若要保存训练模型的快照，请选择 "**定型模型**" 模块右侧面板中的 "**输出**" 选项卡。 选择 "**注册数据集**" 图标，将该模型保存为可重复使用的模块。

+ 若要从模型中生成评分，请使用 "[将数据分配给群集](assign-data-to-clusters.md)"。

## <a name="next-steps"></a>后续步骤

查看可用于 Azure 机器学习[的模块集](module-reference.md)。 