---
title: revoscalepy Python 包
description: revoscalepy 是 Microsoft 推出的 Python 包，支持分布式计算、远程计算上下文和高性能数据科学算法。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 07/14/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: 1fcaa82829b35926e2707dda792ac2c376241650
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470928"
---
# <a name="revoscalepy-python-package-in-sql-server-machine-learning-services"></a>revoscalepy（SQL Server 机器学习服务中的 Python 包）
[!INCLUDE [SQL Server 2017 and later](../../includes/applies-to-version/sqlserver2017.md)]

revoscalepy 是 Microsoft 推出的 Python 包，支持分布式计算、远程计算上下文和高性能数据科学算法。 该包在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中提供。

该包提供以下功能：

+ 具有相同版本 revoscalepy 的系统上的本地和远程计算上下文 
+ 数据转换和可视化功能
+ 数据科学函数，可通过分布式或并行处理进行扩展
+ 改进的性能，包括使用 Intel 数学库

在 revoscalepy 中创建的数据源和计算上下文也可用于机器学习算法  。 有关这些算法的简介，请参阅 [SQL Server 中的 microsoftml Python 模块](ref-py-microsoftml.md)。

## <a name="full-reference-documentation"></a>完整参考文档

多个 Microsoft 产品中都分发有 revoscalepy 包，但不管是在 SQL Server 还是在其他产品中获取该包，用法都是一样的。 由于函数相同，因此[单个 revoscalepy 函数的文档](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package)仅发布到 Microsoft Machine Learning Server 的 [Python 引用](/machine-learning-server/python-reference/introducing-python-package-reference)下的一个位置。 如果存在任何特定于产品的行为，这些差异将在函数帮助页中注明。

## <a name="versions-and-platforms"></a>版本和平台

revoscalepy 模块基于 Python 3.5，且仅在安装以下 Microsoft 产品或下载之一时才可用  ：

+ [SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)
+ [Microsoft Machine Learning Server 9.2.0 或更高版本](/machine-learning-server/)
+ [用于数据科学客户端的 Python 客户端库](setup-python-client-tools-sql.md)

> [!NOTE]
> 完整产品发布版本为 SQL Server 2017（仅限 Windows）。 [SQL Server 2019](../../linux/sql-server-linux-setup-machine-learning.md) 及高更版本中的 revoscalepy 同时支持 Windows 和 Linux。

## <a name="functions-by-category"></a>按类别列出函数

本部分按类别列出函数，以帮助了解每个函数的使用方式。 此外，还可以使用[目录](/machine-learning-server/python-reference/introducing-python-package-reference)按字母顺序查找函数。

## <a name="1-data-source-and-compute"></a>1 数据源和计算

revoscalepy 包含用于创建数据源和设置执行计算的位置或计算上下文的函数   。 下表列出了与 SQL Server 方案相关的函数。

在某些情况下，SQL Server 和 Python 使用不同的数据类型。 有关 SQL 和 Python 数据类型之间的映射的列表，请参阅 [Python 到 SQL 数据类型](python-libraries-and-data-types.md)。

| 函数| 说明|
| ------- | ---------- |
| [RxInSqlServer](/machine-learning-server/python-reference/revoscalepy/rxinsqlserver) |  创建 SQL Server 计算上下文对象以将计算推送到远程实例。 好几个 revoscalepy 函数都将计算上下文作为参数  。 有关上下文切换示例，请参阅[使用 revoscalepy 创建模型](../tutorials/use-python-revoscalepy-to-create-model.md)。|
| [RxSqlServerData](/machine-learning-server/python-reference/revoscalepy/rxsqlserverdata) | 基于 SQL Server 查询或表创建数据对象。 |
| [RxOdbcData](/machine-learning-server/python-reference/revoscalepy/rxodbcdata)| 基于 ODBC 连接创建数据源。 |
| [RxXdfData](/machine-learning-server/python-reference/revoscalepy/rxxdfdata) | 基于本地 XDF 文件创建数据源。 XDF 文件通常用于将内存中数据卸载到磁盘。 当处理的数据多于可以在一批中从数据库传输的数据时，或者数据多于内存中可以容纳的数据时，XDF 文件可能比较有用。 例如，如果定期将大量数据从数据库移动到本地工作站，而非针对每个 R 操作重复查询数据库，则可以使用 XDF 文件作为缓存将数据保存在本地，然后在 R 工作区中使用它。 |

> [!TIP]
> 如果不熟悉数据源或计算上下文，建议从 Microsoft Machine Learning Server 文档中的[分布式计算](/machine-learning-server/r/how-to-revoscaler-distributed-computing)开始。

## <a name="2-data-manipulation-etl"></a>2 数据操作 (ETL)

| 函数 | 说明 |
|----------|-------------|
|[rx_import](/machine-learning-server/python-reference/revoscalepy/rx-import) | 将数据导入 .xdf 文件或数据框。|
|[rx_data_step](/machine-learning-server/python-reference/revoscalepy/rx-data-step) | 将数据从输入数据集转换为输出数据集。|

<a name="bkmk_algorithms"></a>

## <a name="3-training-and-summarization"></a>3 训练和摘要

| 函数| 说明|
| ------- | ---------- |
|[rx_btrees](/machine-learning-server/python-reference/revoscalepy/rx-btrees) | 调整随机梯度提升的决策树|
|[rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-dforest) | 调整分类和回归决策林|
|[rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-dtree) | 调整分类和回归树 |
|[rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod) | 创建线性回归模型|
|[rx_logit](/machine-learning-server/python-reference/revoscalepy/rx-logit) | 创建逻辑回归模型|
|[rx_summary](/machine-learning-server/python-reference/revoscalepy/rx-summary) | 在 revoscalepy 中生成对象的单变量摘要。|

还应查看 [microsoftml](/machine-learning-server/python-reference/microsoftml/microsoftml-package) 中的函数以了解其他方法。

<a name="ml-scoring"></a>

## <a name="4-scoring-functions"></a>4 评分函数

| 函数| 说明|
| ------- | ---------- |
| [rx_predict](/machine-learning-server/python-reference/revoscalepy/rx-predict) | 从已定型模型生成预测|) | 从已定型模型生成预测，并可用于实时评分。 |
|[rx_predict_default](/machine-learning-server/python-reference/revoscalepy/rx-predict-default) | 使用 rx_lin_mod 和 rx_logit 对象计算预测值和残差。 |
|[rx_predict_rx_dforest](/machine-learning-server/python-reference/revoscalepy/rx-predict-rx-dforest) | 计算 rx_dforest 或 rx_btrees 对象中的数据集的预测值或拟合值。 |
|[rx_predict_rx_dtree](/machine-learning-server/python-reference/revoscalepy/rx-predict-rx-dtree) | 计算 rx_dtree 对象中数据集的预测值或拟合值。 |

## <a name="how-to-work-with-revoscalepy"></a>如何使用 revoscalepy

可在存储过程中封装的 Python 代码中调用 revoscalepy 中的函数  。 大多数开发者会在本地构建 revoscalepy 解决方案，然后将已完成的 Python 代码迁移到存储过程作为部署练习  。

在本地运行时，通常可在命令行或 Python 开发环境中运行 Python 脚本，并使用 revoscalepy 函数之一指定 SQL Server 计算上下文  。 可将远程计算上下文用于整个代码或单个函数。 例如，你可能希望将模型定型卸载到服务器上以使用最新数据并避免数据移动。

准备好将 Python 脚本封装在存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 中时，建议将代码重写为具有明确定义的输入和输出的单个函数。 

输入和输出必须为 pandas 数据帧  。 完成此操作后，可从任何支持 T-SQL 的客户端调用存储过程，轻松地将 SQL 查询作为输入传递，并将结果保存到 SQL 表中。 有关示例，请参阅[了解面向 SQL 开发者的数据库内 Python 分析](../tutorials/python-taxi-classification-introduction.md)。

### <a name="using-revoscalepy-with-microsoftml"></a>将 revoscalepy 与 microsoftml 配合使用

将用于 [microsoftml](ref-py-microsoftml.md) 的 Python 函数与 revoscalepy 中提供的计算上下文和数据源集成在一起。 从 microsoftml 调用函数时，例如在定义和训练模型时，请使用 revoscalepy 函数在本地或在 SQL Server 远程计算上下文中执行 Python 代码。

以下示例显示了在 Python 代码中导入模块的语法。 然后可以引用所需的各个函数。

```python
from microsoftml.modules.logistic_regression.rx_logistic_regression import rx_logistic_regression
from revoscalepy.functions.RxSummary import rx_summary
from revoscalepy.etl.RxImport import rx_import_datasource
```

## <a name="see-also"></a>另请参阅

+ [Python 教程](../tutorials/python-tutorials.md)
+ [Python 引用 (Microsoft Machine Learning Server)](/machine-learning-server/python-reference/introducing-python-package-reference)