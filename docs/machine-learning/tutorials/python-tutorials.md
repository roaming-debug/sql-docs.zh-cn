---
title: Python 教程
titleSuffix: SQL machine learning
description: 本文介绍适用于 SQL 机器学习的 Python 教程。 了解如何运行脚本和构建机器学习模型。
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 4b8e997b6990d4d81e9667a41cf359208ca0f31e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341477"
---
# <a name="python-tutorials-for-sql-machine-learning"></a>适用于 SQL 机器学习的 Python 教程
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本文介绍适用于 [SQL Server 上的机器学习服务](../sql-server-machine-learning-services.md)和[大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)的 Python 教程和快速入门。
::: moniker-end
::: moniker range="=sql-server-2017"
本文介绍 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)的 Python 教程和快速入门。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
本文介绍 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)的 Python 教程和快速入门。
::: moniker-end

<a name="bkmk_pythontutorials"></a>

## <a name="python-tutorials"></a>Python 教程

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
| 教程 | 说明 |
|-|-|
| [使用线性回归预测雪橇租赁](python-ski-rental-linear-regression.md) | 使用 Python 和线性回归来预测滑雪租赁数量。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
| [使用 k-means 聚类分析对客户进行分类](python-clustering-model.md) | 使用 Python 开发和部署 K-Means 群集模型，对客户进行分类。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
| [使用 Revoscalepy 创建模型](use-python-revoscalepy-to-create-model.md) | 演示如何使用 SQL Server 作为计算上下文来运行远程 Python 客户端中的代码。 本教程使用 revoscalepy 库中的 rxLinMod 创建模型 。 |
| [适用于 SQL 开发者的 Python 数据分析](python-taxi-classification-introduction.md) | 本端到端演练演示使用 T-SQL 构建完整的 Python 解决方案的过程。 |
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
| 教程 | 说明 |
|-|-|
| [使用线性回归预测雪橇租赁](python-ski-rental-linear-regression.md) | 使用 Python 和线性回归来预测滑雪租赁数量。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
| [使用 k-means 聚类分析对客户进行分类](python-clustering-model.md) | 使用 Python 开发和部署 K-Means 群集模型，对客户进行分类。 在 Azure Data Studio 中使用笔记本准备数据并培训模型，并使用 T-SQL 进行模型部署。 |
::: moniker-end

## <a name="python-quickstarts"></a>Python 快速入门

如果不熟悉 SQL 机器学习，还可以尝试 Python 快速入门。

| 快速入门 | 说明 |
|-|-|
| [运行简单的 Python 脚本](quickstart-python-create-script.md) | 了解关于如何使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 在 T-SQL 中调用 Python 的基础知识。 |
| [使用 Python 的数据结构和对象](quickstart-python-data-structures.md) | 展示了 SQL 如何使用 Python pandas 包处理数据结构。 |
| [在 Python 中创建预测模型并对其进行评分](quickstart-python-train-score-model.md) | 说明如何创建、培训和使用 Python 模型，以根据新数据进行预测。 |

## <a name="next-steps"></a>后续步骤

+ [SQL Server 中的 Python 扩展](../concepts/extension-python.md)
