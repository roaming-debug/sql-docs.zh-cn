---
title: microsoftml Python 包
description: microsoftml 是 Microsoft 推出的 Python 包，可提供高性能的机器学习算法。 它包括用于定型和转换、评分、文本和图像分析的功能，以及用于从现有数据中派生值的特征提取功能。 该包在 SQL Server 机器学习服务中提供。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: ec421e1b0e112a535e3caa59cf83e01c4e02086c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470938"
---
# <a name="microsoftml-python-package-in-sql-server-machine-learning-services"></a>microsoftml（SQL Server 机器学习服务中的 Python 包）
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

microsoftml 是 Microsoft 推出的 Python 包，可提供高性能的机器学习算法。 它包括用于定型和转换、评分、文本和图像分析的功能，以及用于从现有数据中派生值的特征提取功能。 该包在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中提供，支持高性能的大数据、多核处理及快速数据流式处理。

## <a name="full-reference-documentation"></a>完整参考文档

多个 Microsoft 产品中都分发有 microsoftml 包，但不管是在 SQL Server 还是在其他产品中获取该包，用法都是一样的。 由于函数相同，因此[单个 microsoftml 函数的文档](/machine-learning-server/python-reference/microsoftml/microsoftml-package)仅发布到 Microsoft Machine Learning Server 的 [ 引用](/machine-learning-server/python-reference/introducing-python-package-reference)下的一个位置。 如果存在任何特定于产品的行为，这些差异将在函数帮助页中注明。

## <a name="versions-and-platforms"></a>版本和平台

microsoftml 模块基于 Python 3.5，且仅在安装以下 Microsoft 产品或下载之一时才可用  ：

+ [SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)
+ [Microsoft Machine Learning Server 9.2.0 或更高版本](/machine-learning-server/)
+ [用于数据科学客户端的 Python 客户端库](setup-python-client-tools-sql.md)

> [!NOTE]
> 完整产品发布版本为 SQL Server 2017（仅限 Windows）。 [SQL Server 2019](../../linux/sql-server-linux-setup-machine-learning.md) 中的 microsoftml 同时支持 Windows 和 Linux  。

## <a name="package-dependencies"></a>包依赖项

microsoftml 中的算法依赖于以下内容的 [revoscalepy](ref-py-revoscalepy.md)  ：

+ 数据源对象。 microsoftml 函数使用的数据是使用 revoscalepy 函数创建的   。
+ 远程计算（将函数执行转移到远程 SQL Server 实例）。 revoscalepy 包提供用于创建和激活 SQL Server 远程计算上下文的函数。

在大多数情况下，在使用 microsoftml 时，需要一起加载包  。

## <a name="functions-by-category"></a>按类别列出函数

本部分按类别列出函数，以帮助了解每个函数的使用方式。 此外，还可以使用[目录](/machine-learning-server/python-reference/introducing-python-package-reference)按字母顺序查找函数。

## <a name="1-training-functions"></a>1 训练函数

| 函数 | 描述 |
|----------|-------------|
|[microsoftml.rx_ensemble](/machine-learning-server/python-reference/microsoftml/rx-ensemble) | 定型模型的系综。 |
|[microsoftml.rx_fast_forest](/machine-learning-server/python-reference/microsoftml/rx-fast-forest)  | 随机林。 |
|[microsoftml.rx_fast_linear](/machine-learning-server/python-reference/microsoftml/rx-fast-linear) | 线性模型。 以及随机双坐标上升。 |
|[microsoftml.rx_fast_trees](/machine-learning-server/python-reference/microsoftml/rx-fast-trees) | 提升树。 |
|[microsoftml.rx_logistic_regression](/machine-learning-server/python-reference/microsoftml/rx-logistic-regression) | 逻辑回归。 |
|[microsoftml.rx_neural_network](/machine-learning-server/python-reference/microsoftml/rx-neural-network) | 神经网络。 |
|[microsoftml.rx_oneclass_svm](/machine-learning-server/python-reference/microsoftml/rx-oneclass-svm) | 异常检测。 |

<a name="ml-transforms"></a>

## <a name="2-transform-functions"></a>2 转换函数

### <a name="categorical-variable-handling"></a>分类变量处理

| 函数 | 描述 |
|----------|-------------|
|[microsoftml.categorical](/machine-learning-server/python-reference/microsoftml/categorical) | 将文本列转换为类别。 |
|[microsoftml.categorical_hash](/machine-learning-server/python-reference/microsoftml/categorical-hash) | 将文本列进行哈希处理并转换为类别。 |

### <a name="schema-manipulation"></a>架构操作

| 函数 | 描述 |
|----------|-------------|
|[microsoftml.concat](/machine-learning-server/python-reference/microsoftml/concat) | 将多个列串联为一个矢量。 |
|[microsoftml.drop_columns](/machine-learning-server/python-reference/microsoftml/drop-columns) | 从数据集中删除列。 |
|[microsoftml.select_columns](/machine-learning-server/python-reference/microsoftml/select-columns) | 保留数据集的列。 |


### <a name="variable-selection"></a>变量选择

| 函数 | 描述 |
|----------|-------------|
|[microsoftml.count_select](/machine-learning-server/python-reference/microsoftml/count-select) |基于计数的功能选择。 |
|[microsoftml.mutualinformation_select](/machine-learning-server/python-reference/microsoftml/mutualinformation-select) | 基于互信息的功能选择。 |


### <a name="text-analytics"></a>文本分析

| 函数 | 描述 |
|----------|-------------|
|[microsoftml.featurize_text](/machine-learning-server/python-reference/microsoftml/featurize-text) | 将文本列转换为数字特征。 |
|[microsoftml.get_sentiment](/machine-learning-server/python-reference/microsoftml/get-sentiment) | 情绪分析。 |


### <a name="image-analytics"></a>图像分析 

| 函数 | 描述 |
|----------|-------------|
|[microsoftml.load_image](/machine-learning-server/python-reference/microsoftml/load-image) | 加载图像。 |
|[microsoftml.resize_image](/machine-learning-server/python-reference/microsoftml/resize-image) | 调整图像大小。 |
|[microsoftml.extract_pixels](/machine-learning-server/python-reference/microsoftml/extract-pixels) | 从图像中提取像素。 |
|[microsoftml.featurize_image](/machine-learning-server/python-reference/microsoftml/featurize-image) | 将图像转换为特征。 |

### <a name="featurization-functions"></a>特征化函数

| 函数 | 描述 |
|----------|-------------|
|[microsoftml.rx_featurize](/machine-learning-server/python-reference/microsoftml/rx-featurize) | 数据源的数据转换 |

<a name="ml-scoring"></a>

## <a name="3-scoring-functions"></a>3 评分函数

| 函数 | 说明 |
|----------|-------------|
|[microsoftml.rx_predict](/machine-learning-server/python-reference/microsoftml/rx-predict) | 使用 Microsoft 机器学习模型评分 |

## <a name="how-to-call-microsoftml"></a>如何调用 microsoftml

可在存储过程中封装的 Python 代码中调用 microsoftml 中的函数  。 大多数开发者会在本地构建 microsoftml 解决方案，然后将已完成的 Python 代码迁移到存储过程作为部署练习  。

默认情况下，将安装 Python 的 microsoftml 包，但与 revoscalepy 不同，使用随 SQL Server 安装的 Python 可执行文件启动 Python 会话时，默认不加载该包   。

首先，导入 microsoftml 包，然后在需要使用远程计算上下文或相关连接或数据源对象时导入 revoscalepy   。 然后，引用所需的各个函数。

```python
from microsoftml.modules.logistic_regression.rx_logistic_regression import rx_logistic_regression
from revoscalepy.functions.RxSummary import rx_summary
from revoscalepy.etl.RxImport import rx_import_datasource
```

## <a name="see-also"></a>另请参阅

+ [Python 教程](../tutorials/python-tutorials.md)
+ [Python 引用 (Microsoft Machine Learning Server)](/machine-learning-server/python-reference/introducing-python-package-reference)