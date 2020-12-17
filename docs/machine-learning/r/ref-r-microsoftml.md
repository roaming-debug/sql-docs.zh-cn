---
title: MicrosoftML R 包
description: MicrosoftML 是 Microsoft 的一个 R 包，可提供高性能的机器学习算法。 它包括用于定型和转换、评分、文本和图像分析的功能，以及用于从现有数据中派生值的特征提取功能。 该包包含在 SQL Server 机器学习服务和 SQL Server 2016 R Services 中，为大数据提供高性能支持，使用多核处理，支持快速数据流式处理。 此外，MicrosoftML 还包括大量的文本和图像处理转换。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/06/2019
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 9c8986f3407eaa5241b3534ec8988837164e049f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470808"
---
# <a name="microsoftml-r-package-in-sql-server-machine-learning-services"></a>MicrosoftML（SQL Server 机器学习服务中的 R 包）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

“MicrosoftML”是 Microsoft 的一个 R 包，可提供高性能的机器学习算法。 它包括用于定型和转换、评分、文本和图像分析的功能，以及用于从现有数据中派生值的特征提取功能。 该包包含在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)和 [SQL Server 2016 R Services](sql-server-r-services.md) 中，为大数据提供高性能支持，使用多核处理，支持快速数据流式处理。 此外，MicrosoftML 还包括大量的文本和图像处理转换。

## <a name="full-reference-documentation"></a>完整参考文档

“MicrosoftML”包分布于多种 Microsoft 产品中，但不管是在 SQL Server 还是在其他产品中获取该包，用法都是一样的。 由于函数相同，因此[单个 RevoScaleR 函数的文档](/machine-learning-server/r-reference/revoscaler/revoscaler)仅发布到 Microsoft Machine Learning Server 的 [R 引用](/machine-learning-server/r-reference/introducing-r-server-r-package-reference)下的一个位置。 如果存在任何特定于产品的行为，这些差异将在函数帮助页中注明。

## <a name="versions-and-platforms"></a>版本和平台

“MicrosoftML”包基于 R 3.4.3，且仅在安装以下 Microsoft 产品之一或下载时可用：

+ [SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)
+ [SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)
+ [Microsoft Machine Learning Server 9.2.0 或更高版本](/machine-learning-server/)
+ [Microsoft R Client](set-up-a-data-science-client.md)

> [!NOTE]
> 完整产品发布版本为 SQL Server 2017（仅限 Windows）。 在 [SQL Server 2019](../../linux/sql-server-linux-setup-machine-learning.md) 中，MicrosoftML 同时支持 Windows 和 Linux。

## <a name="package-dependencies"></a>包依赖项

对于以下各项，MicrosoftML 中的算法依赖于 [RevoScaleR](ref-r-revoscaler.md)：

+ 数据源对象。 MicrosoftML 函数使用的数据是使用 RevoScaleR 函数创建的 。
+ 远程计算（将函数执行转移到远程 SQL Server 实例）。 “RevoScaleR”包提供用于创建和激活 SQL Server 远程计算上下文的函数。

在大多数情况下，只要使用 MicrosoftML，就需同时加载这些包。

## <a name="functions-by-category"></a>按类别列出函数

本部分按类别列出函数，以帮助了解每个函数的使用方式。 此外，还可以使用[目录](/machine-learning-server/r-reference/introducing-r-server-r-package-reference)按字母顺序查找函数。

<a name="ml-algorithms"></a>

## <a name="1-machine-learning-algorithms"></a>1 - 机器学习算法

| 函数名称 | 说明 |
|---------------|-------------|
|[rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees) | FastRank（MART 梯度提升算法的一种有效实现）的一种实现。  |
|[rxFastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest) | 一种使用 [rxFastTrees](/machine-learning-server/r-reference/microsoftml/rxfasttrees) 的随机林和分位数回归林实现。  |
|[rxLogisticRegression](/machine-learning-server/r-reference/microsoftml/logisticregression) | 使用 L-BFGS 的逻辑回归。  |
|[rxOneClassSvm](/machine-learning-server/r-reference/microsoftml/rxoneclasssvm) | 单类支持向量机。  
|[rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet) | 二进制、多类和回归神经网络。  |
|[rxFastLinear](/machine-learning-server/r-reference/microsoftml/rxfastlinear) | 用于线性二元分类和回归的随机双坐标上升优化。 |
|[rxEnsemble](/machine-learning-server/r-reference/microsoftml/rxensemble) | 定型多种不同类型的模型，以获得比单个模型更好的预测性能。|

<a name="ml-transforms"></a>

## <a name="2-transformation-functions"></a>2 - 转换函数

| 函数名称 | 说明 |
|---------------|-------------|
|[concat](/machine-learning-server/r-reference/microsoftml/concat) | 用于从多个列创建单个向量值列的转换。  |
|[categorical](/machine-learning-server/r-reference/microsoftml/categorical) | 使用带字典的分类转换创建指示器向量。  |
|[categoricalHash](/machine-learning-server/r-reference/microsoftml/categoricalhash) | 通过哈希将分类值转换为指示器数组。 |
|[featurizeText](/machine-learning-server/r-reference/microsoftml/featurizetext) | 从给定的文本语料库中生成大量的连续单词序列（称为 n-grams）。 它提供了语言检测、词汇切分、非索引字删除、文本规范化和功能生成功能。  |
|[getSentiment](/machine-learning-server/r-reference/microsoftml/getsentiment) | 为自然语言文本评分，并创建一个列，该列显示文本中的情绪为积极情绪的可能性。|
|[ngram](/machine-learning-server/r-reference/microsoftml/ngram) | 允许为基于计数和基于哈希的功能提取定义参数。|
|[selectColumns](/machine-learning-server/r-reference/microsoftml/selectcolumns) | 选择一组要重新定型的列，删除所有其他列。 |
|[selectFeatures](/machine-learning-server/r-reference/microsoftml/selectfeatures) | 使用指定模式从指定变量中选择特性。|
|[loadImage](/machine-learning-server/r-reference/microsoftml/loadimage) | 加载图像数据。|
|[resizeImage](/machine-learning-server/r-reference/microsoftml/resizeimage) | 使用指定的大小调整方法，将图像的大小调整为指定的维度。|
|[extractPixels](/machine-learning-server/r-reference/microsoftml/extractpixels) | 从图像中提取像素值。|
|[featurizeImage](/machine-learning-server/r-reference/microsoftml/featurizeimage) | 使用预先定型的深度神经网络模型使图像特征化。|


## <a name="3-scoring-and-training-functions"></a>3 - 评分和定型函数

| 函数名称 | 说明 |
|---------------|-------------|
|[rxPredict.mlModel](/machine-learning-server/r-reference/microsoftml/rxpredict) | 使用存储过程从 SQL Server 运行评分库，或从支持实时评分的 R 代码运行评分库，从而提供更快的预测性能。|
|[rxFeaturize](/machine-learning-server/r-reference/microsoftml/rxfeaturize) | 将数据从输入数据集转换为输出数据集。|
|[mlModel](/machine-learning-server/r-reference/microsoftml/mlmodel) | 提供 Microsoft R 机器学习模型的摘要。|


## <a name="4-loss-functions-for-classification-and-regression"></a>4 - 分类和回归的损失函数

| 函数名称 | 说明 |
|---------------|-------------|
|[expLoss](/machine-learning-server/r-reference/microsoftml/loss) | 适用于指数分类损失函数的规范。 | 
|[logLoss](/machine-learning-server/r-reference/microsoftml/loss) | 适用于对数分类损失函数的规范。  |
|[hingeLoss](/machine-learning-server/r-reference/microsoftml/loss) | 适用于合页分类损失函数的规范。 | 
|[smoothHingeLoss](/machine-learning-server/r-reference/microsoftml/loss) | 适用于平滑合页分类损失函数的规范。  |
| [poissonLoss](/machine-learning-server/r-reference/microsoftml/loss) | 适用于泊松回归损失函数的规范。 | 
|[squaredLoss](/machine-learning-server/r-reference/microsoftml/loss) | 适用于平方回归损失函数的规范。   |   

## <a name="5-feature-selection-functions"></a>5 - 功能选择函数

| 函数名称 | 说明 |
|---------------|-------------|
|[minCount](/machine-learning-server/r-reference/microsoftml/mincount) | 计数模式下的功能选择规范。 |
|[mutualInformation](/machine-learning-server/r-reference/microsoftml/mutualinformation) | 互信息模式下的功能选择规范。 |

## <a name="6-ensemble-modeling-functions"></a>6 - 集成建模函数

| 函数名称 | 说明 |
|---------------|-------------|
|[fastTrees](/machine-learning-server/r-reference/microsoftml/fasttrees) | 创建一个包含函数名称和参数的列表，以使用 [rxEnsemble](/machine-learning-server/r-reference/microsoftml/rxensemble) 定型 Fast Tree 模型。|
|[fastForest](/machine-learning-server/r-reference/microsoftml/rxfastforest) | 创建一个包含函数名称和参数的列表，以使用 [rxEnsemble](/machine-learning-server/r-reference/microsoftml/rxensemble) 定型 Fast Forest 模型。|
|[fastLinear](/machine-learning-server/r-reference/microsoftml/fastlinear) | 创建一个包含函数名称和参数的列表，以使用 [rxEnsemble](/machine-learning-server/r-reference/microsoftml/rxensemble) 定型 Fast Linear 模型。|
|[logisticRegression](/machine-learning-server/r-reference/microsoftml/logisticregression) | 创建一个包含函数名称和参数的列表，以使用 [rxEnsemble](/machine-learning-server/r-reference/microsoftml/rxensemble) 定型逻辑回归模型。|
|[oneClassSvm](/machine-learning-server/r-reference/microsoftml/oneclasssvm) | 创建一个包含函数名称和参数的列表，以使用 [rxEnsemble](/machine-learning-server/r-reference/microsoftml/rxensemble) 定型 OneClassSvm 模型。|

## <a name="7-neural-networking-functions"></a>7 - 神经网络函数

| 函数名称 | 说明 |
|---------------|-------------|
|[optimizer](/machine-learning-server/r-reference/microsoftml/optimizer) | 指定 [rxNeuralNet](/machine-learning-server/r-reference/microsoftml/rxneuralnet) 机器学习算法的优化算法。|


## <a name="8-package-state-functions"></a>8 - 包状态函数

| 函数名称 | 说明 |
|---------------|-------------|
|[rxHashEnv](/machine-learning-server/r-reference/microsoftml/rxHashEnv) | 用于存储包范围的状态的环境对象。 |


## <a name="how-to-use-microsoftml"></a>如何使用 MicrosoftML

MicrosoftML 中的函数可在封装在存储过程中的代码中调用。 大多数开发者会在本地构建 MicrosoftML 解决方案，然后将已完成的 R 代码迁移到存储过程作为部署练习。

适用于 R 的 MicrosoftML 包在 SQL Server 2017 中安装为“开箱即用”。 如果升级实例的 R 组件，它还可以与 SQL Server 2016 一起使用：[使用绑定升级 SQL Server 的实例](../install/upgrade-r-and-python.md)

默认情况下不加载此包。 因此首先需加载 MicrosoftML 包，然后在需要使用远程计算上下文/相关连接或数据源对象时加载 RevoScaleR。 然后，引用所需的各个函数。

```R
library(microsoftml);
library(RevoScaleR);
logisticRegression(args);
```

## <a name="see-also"></a>另请参阅

+ [R 教程](../tutorials/r-tutorials.md)
+ [了解如何使用计算上下文](../tutorials/deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)
+ [面向 SQL 开发者的 R：定型和操作模型](../tutorials/r-taxi-classification-introduction.md)
+ [GitHub 上的 Microsoft 产品示例](https://github.com/Microsoft/SQL-Server-R-Services-Samples)
+ [R 引用 (Microsoft Machine Learning Server)](/machine-learning-server/r-reference/introducing-r-server-r-package-reference)