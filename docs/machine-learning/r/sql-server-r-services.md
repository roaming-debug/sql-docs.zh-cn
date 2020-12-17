---
title: 什么是 SQL Server 2016 R Services？
titleSuffix: ''
description: R Services 是 SQL Server 2016 中的一项功能，借助此功能可以使用关系数据运行 R 脚本。 可以使用开源包和框架，以及 Microsoft R 包进行预测分析和机器学习。 脚本在数据库中执行，而不将数据移动到 SQL Server 外部或是在网络上移动。 本文介绍 SQL Server R Services 的基础知识。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/06/2020
ms.topic: overview
author: dphansen
ms.author: davidph
monikerRange: =sql-server-2016
ms.openlocfilehash: d1910099e1f0aa4a8f3e58f1faa01dfbe177c517
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470758"
---
# <a name="what-is-sql-server-2016-r-services"></a>什么是 SQL Server 2016 R Services？

[!INCLUDE[SQL Server 2016 only](../../includes/applies-to-version/sqlserver2016-only.md)]

R Services 是 SQL Server 2016 中的一项功能，借助此功能可以使用关系数据运行 R 脚本。 可以使用开源包和框架，以及 [Microsoft R 包](#packages)进行预测分析和机器学习。 脚本在数据库中执行，而不将数据移动到 SQL Server 外部或是在网络上移动。 本文介绍 SQL Server R Services 的基础知识。

> [!Note]
> 在 SQL Server 2017 及更高版本中，R Services 被重命名为[机器学习服务](../sql-server-machine-learning-services.md)，并支持 Python 和 R。

## <a name="what-is-r-services"></a>什么是 R Services？

使用 SQL Server R Services，可以在数据库中执行 R 脚本。 可以使用它来准备和清理数据、执行特征工程以及在数据库中定型、评估和部署机器学习模型。 此功能在数据所在的位置运行脚本，无需通过网络将数据传输到其他服务器。

R Services 中包含 R 的基础分发。 除了 Microsoft 包[RevoScaleR](../r/ref-r-revoscaler.md)、[MicrosoftML](../r/ref-r-microsoftml.md)、[olapR]../r/ref-r-olapr.md) 和 [sqlrutils](../r/ref-r-sqlrutils.md) 之外，还可以使用开源包和框架。

R Services 使用扩展性框架在 SQL Server 中运行 R 脚本。 详细了解工作原理：

+ [扩展性框架](../concepts/extensibility-framework.md)
+ [R 扩展](../concepts/extension-r.md)

## <a name="what-can-i-do-with-r-services"></a>使用 R Services 可以执行哪些操作？

可以使用 R Services 在 SQL Server 中生成和定型机器学习和深度学习模型。 还可以将现有模型部署到 R Services，并使用关系数据进行预测。

可以使用 SQL Server R Services 进行多种类型的预测，下面是一些示例：

|预测类型|示例|
|-|-|
|分类/类别|自动将客户反馈分为积极和消极两类|
|回归/预测连续值|根据面积和地段预测房价|
|异常检测|检测欺诈性的银行交易 |
|建议|根据网购者以前的购买情况，推荐他们可能想购买的产品|

### <a name="how-to-execute-r-scripts"></a>如何执行 R 脚本

有两种方法可以在 R Services 中执行 R 脚本：

+ 最常见的方法是使用 T-SQL 存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)。

+ 还可以使用首选的 R 客户端并编写将执行（称为“远程计算上下文”）推送到远程 SQL Server 的脚本。 有关详细信息，请参阅如何[设置数据科学客户端 R 开发](../r/set-up-a-data-science-client.md)。

<a name="version"></a>

## <a name="r-versions"></a>R 版本

下面列出了 SQL Server 2016 R 服务中包含的 R 运行时版本。

SQL Server 版本 | 默认 R 运行时版本 |
|-|-|
| SQL Server 2016 RTM - SP2 CU13 | 3.2.2 |
| SQL Server 2016 SP2 CU14 及更高版本 | 3.2.2 和 3.5.2 |

SQL Server 2016 Service Pack (SP) 2 及更高版本的累积更新 (CU) 14 包括更新的 R 运行时。 有关详细信息，请参阅[更改默认语言运行时版本](../install/change-default-language-runtime-version.md)。

对于 R 的其他版本或者如果要运行 Python，请使用[用于 SQL Server 2017 及更高版本的机器学习服务](../sql-server-machine-learning-services.md)。

<a name="packages"></a>

## <a name="r-packages"></a>R 包

除了 Microsoft 的企业包外，还可以使用开源包和框架。 大多数常见的开源 R 包已预先安装在 R Services 中。 还包括 Microsoft 提供的以下 R 包：

| 程序包 | 说明 |
|-|-|
| [RevoScaleR](../r/ref-r-revoscaler.md) | 用于可缩放 R 的主包。数据转换和操作、统计摘要、可视化和多种形式的建模。 此外，此包中的函数会自动在可用内核之间分配工作负载以进行并行处理。 |
| [MicrosoftML (R)](../r/ref-r-microsoftml.md) | 添加机器学习算法，以便为文本分析、图像分析和情绪分析创建自定义模型。 |
| [olapR](../r/ref-r-olapr.md) | R 函数，用于针对 SQL Server Analysis Services OLAP 多维数据集进行 MDX 查询。 |
| [sqlrutils](../r/ref-r-sqlrutils.md) | 一种在 T-SQL 存储过程中使用 R 脚本、将该存储过程注册到数据库并在 [R 开发环境](../r/set-up-a-data-science-client.md)中运行存储过程的机制。 |
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open (MRO) 是 Microsoft 中 R 的增强型分发。 它是用于统计分析和数据科学的完整开源平台。 它基于 R，并 100% 与 R 兼容，包括很多其他功能，可以带来更高的性能和可再现性。 |

## <a name="how-do-i-get-started-with-rservices"></a>如何开始使用 RServices？

1. [安装 SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)

1. 配置开发工具。 可用工具如下：

    + 使用 [Azure Data Studio](../../azure-data-studio/what-is.md) 或 [SQL Server Management Studio (SSMS)](../../ssms/sql-server-management-studio-ssms.md) 以采用 T-SQL 和存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 来执行 R 脚本。
    + 使用开发便携式计算机或工作站上的 R 来执行脚本。 可以在本地拉取数据，也可以使用 [RevoScaleR](../r/ref-r-revoscaler.md) 将执行远程推送到 SQL Server。 有关详细信息，请参阅如何[设置数据科学客户端 R 开发](../r/set-up-a-data-science-client.md)。

1. 编写第一个 R 脚本

    + 快速入门：[在 SQL Server 中创建并运行简单的 R 脚本](../tutorials/quickstart-r-create-script.md)
    + 快速入门：[在 R 中创建和定型预测模型](../tutorials/quickstart-r-train-score-model.md)
    + 教程：[在 T-SQL 中使用 R](../tutorials/r-taxi-classification-introduction.md)：探索数据、执行特征工程、定型和部署模型，并进行预测（五部分构成的系列）
    + 教程：[在 R 工具中使用 R Services](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)：探索数据、创建图表和绘图、执行特征工程、定型和部署模型，并进行预测（六部分构成的系列）

## <a name="next-steps"></a>后续步骤

+ [安装 SQL Server 2016 R Services](../install/sql-r-services-windows-install.md)
+ [设置用于 R 开发的数据科学客户端](../r/set-up-a-data-science-client.md)
