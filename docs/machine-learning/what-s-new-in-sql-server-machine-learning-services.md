---
title: SQL Server 机器学习服务中的新增功能？
titleSuffix: ''
description: 各版本 SQL Server 机器学习服务和 SQL Server 2016 R Services 的新功能公告。
ms.date: 11/17/2020
ms.topic: overview
author: dphansen
ms.author: davidph
ms.custom: sqlseattle
ms.prod: sql
ms.technology: machine-learning-services
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: fdf9cb30fead518c36fef8de4e62db4cd56d9560
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469988"
---
# <a name="whats-new-in-sql-server-machine-learning-services"></a>SQL Server 机器学习服务中的新增功能
[!INCLUDE [SQL Server 2016 and later](../includes/applies-to-version/sqlserver2016.md)]

本文介绍 [SQL Server 机器学习服务](sql-server-machine-learning-services.md)的每个版本中包含的新功能和功能。 随着我们继续扩大、扩展和深化数据平台、高级分析和数据科学之间的集成，机器学习功能会添加到每个版本的 SQL Server 中。 

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="new-in-sql-server-2019"></a>SQL Server 2019 中的新增功能

此版本在 SQL Server 中添加了用于 Python 和 R 机器学习操作的需求最大的功能。 有关此版本中所有功能的详细信息，请参阅 [SQL Server 2019 中的新增功能](../sql-server/what-s-new-in-sql-server-ver15.md)和 [SQL Server 2019 的发行说明](../sql-server/sql-server-version-15-release-notes.md)。

> [!NOTE]
> 有关 SQL Server 2019 中 Java 的新增功能文档，请参阅 [What's new in SQL Server Language Extensions?](../language-extensions/language-extensions-whats-new.md)（SQL Server 语言扩展中的新增功能？）

以下是 SQL Server 机器学习服务的新增功能，在 Windows 和 Linux 上均可使用 ：

- 在 Python 和 R 的机器学习服务中添加了 Linux 平台支持。开始[在 Linux 上安装 SQL Server 机器学习服务](../linux/sql-server-linux-setup-machine-learning.md)。
- [从 Python 或 R 脚本到 SQL Server 的环回连接](connect/loopback-connection.md)。 
- 适用于 Python 和 R 的[CREATE EXTERNAL LIBRARY (Transact-SQL)](../t-sql/statements/create-external-library-transact-sql.md)。
- [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 引入了两个新参数，使你能够轻松地从分区数据生成多个模型。 有关详细信息，请参阅教程[在 R 中创建基于分区的模型](tutorials/r-tutorial-create-models-per-partition.md)。
- 可在 Launchpad 服务上使用故障转移群集支持的前提是在所有节点上启动 SQL Server Launchpad 服务。 有关详细信息，请参阅 [SQL Server 故障转移群集安装](../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)。
- 机器学习服务的隔离机制更改。 有关详细信息，请参阅 [Windows 上的 SQL Server 2019：机器学习服务的隔离更改](install/sql-server-machine-learning-services-2019.md)。

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="new-in-sql-server-2017"></a>SQL Server 2017 的新增功能

此版本添加了 [Python 支持和行业领先的机器学习算法](https://cloudblogs.microsoft.com/sqlserver/2017/04/19/python-in-sql-server-2017-enhanced-in-database-machine-learning/)。 重命名以反映新的作用域，SQL Server 2017 标志着 [SQL Server 机器学习服务（数据库内）](sql-server-machine-learning-services.md)的引入，同时对 Python 和 R 提供语言支持。 

有关所有的功能公告，请参阅 [SQL Server 2017 的新增功能](../sql-server/what-s-new-in-sql-server-2017.md)。

### <a name="r-enhancements"></a>R 增强功能

SQL Server 机器学习服务 R 组件是下一代 SQL Server 2016 R Services，其中包含基本 R、RevoScaler 和其他包的更新版本。

R 的新功能包括[包管理](package-management/install-r-packages-with-tsql.md)，以下是一些亮点  ： 

+ 数据库角色可帮助 DBA 管理包，并为包安装分配权限。
+ [CREATE EXTERNAL LIBRARY](../t-sql/statements/create-external-library-transact-sql.md) 帮助 DBA 以熟悉的 T-SQL 语言管理包。
+ [RevoScaleR](package-management/install-r-packages-with-revoscaler.md) 函数可帮助安装、删除或列出用户拥有的包。 有关详细信息，请参阅[如何使用 RevoScaleR 函数在 SQL Server 上查找或安装 R 包](package-management/install-r-packages-with-revoscaler.md)。

### <a name="r-libraries"></a>R 库

| 程序包 | 说明 |
|---------|-------------|
| [**MicrosoftML**](r/ref-r-microsoftml.md) | 在此版本中，MicrosoftML 包含在默认 R 安装中，从而消除了之前 SQL Server 2016 R Services 中所需的升级步骤。 MicrosoftML 提供先进的机器学习算法和可在远程计算上下文中扩展或运行的数据转换。 算法包括可自定义的深层神经网络、快速决策树和决策林、线性回归和逻辑回归。  |

### <a name="python-integration-for-in-database-analytics"></a>用于数据库内分析的 Python 集成

Python 是一种语言，可为各种机器学习任务提供极大灵活性和强大功能。 用于 Python 的开放源代码库包括可自定义神经网络的多个平台以及用于自然语言处理的常用库。 

由于 Python 与数据库引擎集成，因此可以保持与数据接近的分析结果，同时清除与数据移动相关的成本和安全风险。 可使用 Visual Studio 之类的工具基于 Python 部署机器学习解决方案。 使用 SQL Server 数据访问方法，生产应用程序可以从 Python 3.5 运行时获取预测、模型或视觉对象。

通过 [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 系统存储过程支持 T-SQL 和 Python 集成。 可使用此存储过程调用任何 Python 代码。 代码在安全的双体系结构中运行，该体系结构可使用企业级的 Python 模型和脚本部署，可使用简单的存储过程从应用程序中调用。 通过将数据从 SQL 流式传输到 Python 进程以及 MPI 环并行化，实现更多性能提升。

可使用 T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) 函数在以前以所需的二进制格式保存的预定型模型上执行[本机评分](predictions/native-scoring-predict-transact-sql.md)。

### <a name="python-libraries"></a>Python 库

| 程序包 | 说明 |
|---------|-------------|
|[**revoscalepy**](python/ref-py-revoscalepy.md)| RevoScaleR 的 Python 等效项。 你可以为线性和逻辑回归、决策树、提升树和随机林创建 Python 模型，所有这些都是可并行化的，并能够在远程计算上下文中运行。 此包支持使用多个数据源和远程计算上下文。 数据科学家或开发人员可以在远程 SQL Server 上执行 Python 代码，以浏览数据或生成模型，而无需移动数据。 |
|[**microsoftml**](python/ref-py-microsoftml.md) |MicrosoftML R 包的 Python 等效项。 |

### <a name="pre-trained-models"></a>预定型模型

[预定型模型](install/sql-pretrained-models-install.md)可用于 Python 和 R。使用这些模型进行图像识别和正负情绪分析，以便根据自己的数据生成预测  。 

### <a name="standalone-server-as-a-shared-feature-in-sql-server-setup"></a>独立服务器作为 SQL Server 安装程序中的共享功能

此版本还添加了 [SQL Server 机器学习服务器（独立版）](r/r-server-standalone.md)，这是一个完全独立的数据科学服务器，支持 R 和 Python 中的统计分析和预测分析。 与 R Services 一样，此服务器是 SQL Server 2016 R Server（独立版）的下一版本。 使用该独立服务器，你可以分发和扩展 R 或 Python 解决方案，而无需依赖于 SQL Server。
::: moniker-end

::: moniker range=">=sql-server-2016"
## <a name="new-in-sql-server-2016"></a>SQL Server 2016 中的新增功能

此版本通过 SQL Server 2016 R Services 将机器学习功能引入到 SQL Server，这是一个数据库内分析引擎，用于处理数据库引擎实例中常驻数据上的 R 脚本  。

此外，SQL Server 2016 R Server（独立版）是作为在 Windows 服务器上安装 R Server 的方式发布的  。 最初，SQL Server 安装程序提供了安装 R Server for Windows 的唯一方法。 在更高版本中，希望在 Windows 上使用 R Server 的开发人员和数据科学家可以使用另一个独立的安装程序实现相同的目标。 SQL Server 中的独立服务器在功能上等同于独立服务器软件 [Microsoft R Server for Windows](/machine-learning-server/install/r-server-install-windows)。

有关所有的功能公告，请参阅 [SQL Server 2016 的新增功能](../sql-server/what-s-new-in-sql-server-2016.md)。

| 发布 |功能更新 |
|---------|----------------|
| CU 添加件 | [实时评分](predictions/real-time-scoring.md)依赖于本机 C++ 库来读取以优化的二进制格式存储的模型，然后生成预测，而无需调用 R 运行时  。 这使得评分操作的速度更快。 使用实时评分，可以运行存储过程或从 R 代码执行实时评分。 如果实例升级到 [!INCLUDE[rsql-platform-md](../includes/rsql-platform-md.md)] 的最新版本，则实时评分也可用于 SQL Server 2016。 |
| 初始版本 | [**用于数据库内分析的 R 集成**](r/sql-server-r-services.md)。 <br/><br/> 用于在 T-SQL 中调用 R 函数的 R 包，反之亦然。 RevoScaleR 函数通过将数据分块到组件部分、协调和管理分布式处理以及聚合结果，从而大规模提供 R 服务。 在 SQL Server 2016 R Services（数据库内）中，RevoScaleR 引擎与数据库引擎实例集成在一起，并在同一处理上下文中将数据和分析结合在一起。 <br/><br/>通过 [sp_execute_external_script](../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 实现 T-SQL 和 R 的集成。 可使用此存储过程调用任何 R 代码。 此安全体系结构支持企业级 Rn 模型和脚本的部署，这些模型和脚本可以使用简单的存储过程从应用程序中调用。 通过将数据从 SQL 流式传输到 R 进程以及 MPI 环并行化，实现更多性能提升。 <br/><br/>可使用 T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md) 函数在以前以所需的二进制格式保存的预定型模型上执行[本机评分](predictions/native-scoring-predict-transact-sql.md)。|

::: moniker-end

::: moniker range=">=sql-server-2017"
## <a name="linux-support"></a>Linux 支持

在使用数据库引擎实例安装机器学习包时，SQL Server 2019 会为 R 和 Python 添加 Linux 支持。 有关详细信息，请参阅[在 Linux 上安装 SQL Server 机器学习服务](../linux/sql-server-linux-setup-machine-learning.md)。

在 Linux 上，SQL Server 2017 没有 R 或 Python 集成，但你可以在 Linux 上使用[本机评分](predictions/native-scoring-predict-transact-sql.md)，因为该功能可通过在 Linux 上运行的 T-SQL [PREDICT](../t-sql/queries/predict-transact-sql.md)提供。 本机评分可从预定型模型进行高性能评分，无需进行调用，甚至不需要 R 运行时。
::: moniker-end

## <a name="next-steps"></a>后续步骤

+ [安装 SQL Server 机器学习服务（数据库内）](install/sql-machine-learning-services-windows-install.md)
