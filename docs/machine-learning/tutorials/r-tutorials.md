---
title: R 教程
titleSuffix: SQL machine learning
description: 本文介绍适用于 SQL 机器学习的 R 教程。 了解如何运行脚本和构建机器学习模型。
ms.prod: sql
ms.technology: machine-learning
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.reviewer: garye, davidph
ms.date: 05/21/2020
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: aee6dcfce5e07b62d53420328221999bed0f887f
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870255"
---
# <a name="r-tutorials-for-sql-machine-learning"></a>适用于 SQL 机器学习的 R 教程
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
本文介绍适用于 [SQL Server 上的机器学习服务](../sql-server-machine-learning-services.md)和[大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)的 R 教程和快速入门。
::: moniker-end
::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
本文介绍了 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)的 R 教程和快速入门。
::: moniker-end
::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
本文介绍了适用于 [SQL Server 2016 R Services](../r/sql-server-r-services.md) 的 R 教程和快速入门。
::: moniker-end
::: moniker range="=azuresqldb-mi-current||=sqlallproducts-allversions"
本文介绍 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)的 Python 教程和快速入门。
::: moniker-end

<a name="bkmk_sqltutorials"></a>

## <a name="r-tutorials"></a>R 教程

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15||=sqlallproducts-allversions"
| 教程 | 说明 |
|------|-------------|
| [使用决策树预测雪橇租赁](r-predictive-model-introduction.md) | 使用 R 和决策树模型预测将来的雪橇租赁数量。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
| [使用 k-means 聚类分析对客户进行分类](r-clustering-model-introduction.md) | 使用 R 开发和部署 K-Means 聚类分析模型，对客户进行分类。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
| [适用于数据科学家的数据库内 R 分析](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md) | 对于刚接触 SQL 机器学习的 R 开发者，本教程介绍了如何在 SQL 中执行常见的数据科学任务。 加载和可视化数据，定型模型并将其保存在数据库中，以及使用模型进行预测分析。 |
| [适用于 SQL 开发者的数据库内 R 分析](../tutorials/r-taxi-classification-introduction.md) | 仅使用 SQL 工具来生成并部署一个完整的 R 解决方案。 重点介绍如何将解决方案移动到生产环境中。 学习如何将 R 代码包含在存储过程中，将 R 模型保存在数据库中，以及参数化调用 R 模型用于预测。 |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| 教程 | 说明 |
|------|-------------|
| [使用决策树预测雪橇租赁](r-predictive-model-introduction.md) | 使用 R 和决策树模型预测将来的雪橇租赁数量。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
| [使用 k-means 聚类分析对客户进行分类](r-clustering-model-introduction.md) | 使用 R 开发和部署 K-Means 聚类分析模型，对客户进行分类。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
::: moniker-end

## <a name="r-quickstarts"></a>R 快速入门

如果不熟悉 SQL 机器学习，还可以尝试 R 快速入门。

| 快速入门 | 说明 |
|-|-|
| [运行简单的 R 脚本](quickstart-r-create-script.md) | 了解有关如何使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 在 T-SQL 中调用 R 的基础知识。 |
| [使用 R 的数据结构和对象](quickstart-r-data-types-and-objects.md) | 展示了 SQL 如何使用 R 处理数据结构。 |
| [在 R 中创建预测模型并对其进行评分](quickstart-r-data-types-and-objects.md) | 说明如何创建、训练和使用 R 模型，以根据新数据进行预测。 |

## <a name="next-steps"></a>后续步骤

+ [SQL Server 中的 Python 扩展](../concepts/extension-r.md)
