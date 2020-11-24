---
title: Python 教程：使用二元分类来预测纽约市出租车费用
titleSuffix: SQL machine learning
description: 了解如何使用 SQL 机器学习在 SQL Server 存储过程和 T-SQL 函数中嵌入 Python 代码，以使用二元分类来预测纽约市出租车费用。
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/28/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current||=sqlallproducts-allversions'
ms.openlocfilehash: 7b8e0e8df7bd2a5453299751df682e0c33502c25
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94585069"
---
# <a name="python-tutorial-predict-nyc-taxi-fares-with-binary-classification"></a>Python 教程：使用二元分类来预测纽约市出租车费用
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions"
在本面向 SQL 程序员的由五部分组成的系列教程中，你将学习如何在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)或[大数据群集](../../big-data-cluster/machine-learning-services.md)中集成 Python。
::: moniker-end

::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
在本面向 SQL 程序员的由五部分组成的系列教程中，你将学习如何在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中集成 Python。
::: moniker-end

::: moniker range=">=azuresqldb-mi-current||=sqlallproducts-allversions"
在本面向 SQL 程序员的由五部分组成的系列教程中，你将学习如何在 [Azure SQL 托管实例中的机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)上集成 Python。
::: moniker-end

你将使用 SQL Server 上的示例数据库来生成和部署基于 Python 的机器学习解决方案。 你将使用 T-SQL、Azure Data Studio 或 SQL Server Management Studio，以及支持 SQL 机器学习和 Python 语言的数据库实例。

本系列教程介绍了在数据建模工作流中使用的 Python 函数。 其中的部分包括数据浏览、生成和训练二元分类模型，以及模型部署等。 你将使用来自纽约市出租车和豪华轿车委员会的示例数据。 要生成的模型会根据一天中的时间、行程距离和上车位置来预测行程是否可能会产生小费。

在本系列的第一部分中，你将安装必备组件，并还原示例数据库。 在第二和第三部分中，你将开发一些 Python 脚本，以准备数据并训练机器学习模型。 然后，在第四和第五部分中，你将使用 T-SQL 存储过程在数据库中运行这些 Python 脚本。

在本文中，你将：

> [!div class="checklist"]
> + 安装必备组件
> + 还原示例数据库

在[第二部分](python-taxi-classification-explore-data.md)中，你将探索示例数据，并生成一些绘图。

在[第三部分](python-taxi-classification-create-features.md)中，你将学习如何使用 Transact-SQL 函数根据原始数据创建特征。 然后从存储过程调用该函数，创建包含该功能值的表。

在[第四部分](python-taxi-classification-train-model.md)中，你将加载模块，并调用必要的函数，以使用 SQL Server 存储过程来创建和训练模型。

在[第五部分](python-taxi-classification-deploy-model.md)中，你将了解如何操作在第四部分中训练和保存的模型。

> [!NOTE]
> R 和 Python 均提供此教程。 有关 R 版本，请参阅 [R 教程：使用二元分类来预测纽约市出租车费用](r-taxi-classification-introduction.md)。

## <a name="prerequisites"></a>必备知识

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15||=sqlallproducts-allversions"
+ [使用 Python 安装 SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md#verify-installation)
::: moniker-end

+ [授予执行 Python 脚本的权限](../security/user-permission.md)

+ 还原[纽约市出租车演示数据库](demo-data-nyctaxi-in-sql.md)

所有任务都可以使用 Azure Data Studio 或 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程来完成。

若要更好地学习本系列教程，你需要熟悉基本数据库操作，如创建数据库和表、导入数据以及编写 SQL 查询。 但无需了解 Python，因为所有 Python 代码都已提供。

## <a name="background-for-sql-developers"></a>SQL 开发者背景

构建机器学习解决方案是一种复杂的过程，它可能涉及多种工具，并且需要主题专家跨多个阶段进行协调：

+ 获取和清除数据
+ 探索数据并构建有助于建模的功能
+ 定型和优化模型
+ 部署到生产环境

使用专用的开发环境，可以以最佳方式执行实际代码的开发和测试。 不过，在完全测试脚本后，可以在熟悉的 Azure Data Studio 或 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 环境中使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 存储过程将脚本轻松部署到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 在存储过程中包装外部代码是在 SQL Server 中操作代码的主要机制。

将模型保存到数据库后，可以使用存储过程从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 调用该模型以用于预测。

无论你是初次接触 Python 的 SQL 程序员，还是初次接触 SQL 的 Python 开发人员，都可以学习由五部分组成的本系列教程，其中介绍了使用 Python 和 SQL Server 进行数据库内分析的典型工作流。

## <a name="next-steps"></a>后续步骤

本文内容：

> [!div class="checklist"]
> + 安装必备组件
> + 还原示例数据库

> [!div class="nextstepaction"]
> [Python 教程：探索并可视化数据](python-taxi-classification-explore-data.md)