---
title: 在存储过程中部署 R 代码
description: 在 SQL Server 存储过程中嵌入 R 语言代码，使其可以供任何有权访问 SQL Server 数据库的客户端应用程序可用。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: a959bf5c29a125aefb390bef1428e6a35745e665
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353753"
---
# <a name="operationalize-r-code-using-stored-procedures-in-sql-server-machine-learning-services"></a>在 SQL Server 机器学习服务中使用存储过程操作 R 代码
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

在 SQL Server 机器学习服务中使用 R 和 Python 功能时，将解决方案移动到生产环境的最常见方法是在存储过程中嵌入代码。 本文总结了 SQL 开发人员在使用 SQL Server 操作 R 代码时要考虑的要点。

## <a name="deploy-production-ready-script-using-t-sql-and-stored-procedures"></a>使用 T-SQL 和存储过程部署生产就绪脚本

传统上，数据科学解决方案的集成意味着需要进行大量重新编码以支持性能和集成。 SQL Server 机器学习服务简化了此任务，因为 R 和 Python 代码可以在 sqlserver 中运行并使用存储过程进行调用。 有关在存储过程中嵌入代码的机制的详细信息，请参阅：

+ [在 SQL Server 中创建并运行简单的 R 脚本](../tutorials/quickstart-r-create-script.md)
+ [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md)

有关使用存储过程将 R 代码部署到生产环境中的更全面的示例，请参阅[R 教程：使用二元分类来预测纽约市出租车费用](../tutorials/r-taxi-classification-introduction.md)。

## <a name="guidelines-for-optimizing-r-code-for-sql"></a>针对 SQL 优化 R 代码的准则

如果事先在 R 或 Python 代码中进行了一些优化，那么在 SQL 中转换 R 代码会更容易。 这些优化包括避免导致问题的数据类型、避免不必要的数据转换，以及将 R 代码重写为一个易于参数化的函数调用。 有关详细信息，请参阅：

+ [R 库和数据类型](r-libraries-and-data-types.md)
+ [使用 sqlrutils 帮助程序函数](ref-r-sqlrutils.md)

## <a name="integrate-r-and-python-with-applications"></a>将 R 和 Python 与应用程序集成

由于可从存储过程运行 R 或 Python，因此可从任何可以发送 T-SQL 语句并处理结果的应用程序执行脚本。 例如，可以使用 Integration Services 中的[执行 T-SQL 任务](../../integration-services/control-flow/execute-t-sql-statement-task.md)或使用另一个可以运行存储过程的作业计划程序，按计划重新定型模型。

评分是一项重要任务，可以轻松地自动化或从外部应用程序启动。 事先使用 R 或 Python 或存储过程定型模型，然后[将模型以二进制格式保存](../tutorials/walkthrough-build-and-save-the-model.md)到表中。 然后，可以将模型作为存储过程调用的一部分加载到变量中，使用以下选项之一从 T-SQL 进行评分：

+ 实时评分，针对小批量进行了优化
+ 单行评分，用于从应用程序进行调用
+ [本机评分](../predictions/native-scoring-predict-transact-sql.md)，可从 SQL Server 快速进行批量预测，而无需调用 R

下面的教程提供了在批处理和单行模式下使用存储过程进行评分的示例：

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"
+ [SQL Server 中 R 的端到端数据科学演练](../tutorials/walkthrough-data-science-end-to-end-walkthrough.md)
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
+ [R 教程：使用二元分类来预测纽约市出租车费用](../tutorials/r-taxi-classification-introduction.md)
::: moniker-end

## <a name="boost-performance-and-scale"></a>提高性能和缩放性

尽管开放源代码的 R 语言在处理大型数据集方面存在已知的局限性，但是 SQL Server 机器学习服务中包含的 [RevoScaleR 包 API](ref-r-revoscaler.md) 可以对大型数据集执行操作，并从多线程、多核、多进程数据库内计算中获益。

如果你的 R 解决方案使用复杂的聚合或涉及大型数据集，则可以利用 SQL Server 的高效内存中聚合和列存储索引，然后让 R 代码处理统计计算和评分。

::: moniker range=">=sql-server-2016||>=sql-server-linux-ver15"

## <a name="adapt-r-code-for-other-platforms-or-compute-contexts"></a>为其他平台或计算上下文调整 R 代码

在 SQL Server 安装程序中使用[独立服务器选项](../install/sql-machine-learning-standalone-windows-install.md)或安装非 SQL 品牌的产品 Microsoft Machine Learning Server（以前称为“Microsoft R Server”）时，针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据运行的同一 R 代码可用于其他数据源，例如用于 HDFS 的 Spark：

+ [Machine Learning Server 文档](/r-server/)

::: moniker-end
