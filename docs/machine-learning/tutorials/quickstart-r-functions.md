---
title: 快速入门：R 函数
titleSuffix: SQL machine learning
description: 本快速入门介绍如何通过 SQL 机器学习使用 R 数学和实用程序函数。
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 0da73dd1202d4b5eab2c14077dfe54b04dfe9bd8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98092661"
---
# <a name="quickstart-r-functions-with-sql-machine-learning"></a>快速入门：通过 SQL 机器学习使用 R 函数
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本快速入门介绍如何在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中或[大数据群集](../../big-data-cluster/machine-learning-services.md)上使用 R 数学和实用程序函数。 通常在 T-SQL 中难以实现的统计函数在 R 中只需几行代码就可以实现。
::: moniker-end
::: moniker range="=sql-server-2017"
本快速入门介绍如何通过 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)使用 R 数学和实用程序函数。 通常在 T-SQL 中难以实现的统计函数在 R 中只需几行代码就可以实现。
::: moniker-end
::: moniker range="=sql-server-2016"
本快速入门介绍如何通过 [SQL Server R Services](../r/sql-server-r-services.md) 使用 R 数学和实用程序函数。 通常在 T-SQL 中难以实现的统计函数在 R 中只需几行代码就可以实现。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
本快速入门介绍在 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)中使用 R 时如何使用数据结构和数据类型。 将了解如何在 R 与 SQL 托管实例之间迁移数据，以及可能出现的常见问题。
::: moniker-end

## <a name="prerequisites"></a>先决条件

若要运行本快速入门，需要具备以下先决条件。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server 机器学习服务。 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json)。 还可以[启用 SQL Server 大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server 机器学习服务。 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)。 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services。 如需安装 R Services，请参阅 [Windows 安装指南](../install/sql-r-services-windows-install.md)。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。
::: moniker-end

- 一个用于运行包含 R 脚本的 SQL 查询的工具。 本快速入门使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)。

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>创建一个存储过程来生成随机数字

为简单起见，我们使用默认安装并加载的 R `stats` 包。 此包中包含数百个用于执行常用统计任务的函数，其中，`rnorm` 函数在给定了标准差和平均数的情况下使用正态分布生成指定数量的随机数字。

例如，以下 R 代码在标准偏差为 3 的情况下返回平均值为 50 的 100 个数字。

```R
as.data.frame(rnorm(100, mean = 50, sd = 3));
```

若要从 T-SQL 调用此行 R 代码，请在 `sp_execute_external_script` 的 R 脚本参数中添加此 R 函数，如下所示：

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(100, mean = 50, sd =3));'
    , @input_data_1 = N'   ;'
      WITH RESULT SETS (([Density] float NOT NULL));
```

如果你希望更轻松地生成不同的一组随机数字，那该怎么办？

与 T-SQL 结合使用时操作非常简单。 定义一个存储过程，从用户那里获取参数，然后将这些参数作为变量传递给 R 脚本。

```sql
CREATE PROCEDURE MyRNorm (
    @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
         OutputDataSet <- as.data.frame(rnorm(mynumbers, mymean, mysd));'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- 第一行定义了在执行该存储过程时必需的每个 SQL 输入参数。

- 以 `@params` 开头的行定义了 R 代码使用的所有变量，以及对应的 SQL 数据类型。

- 紧跟在后面的行将 SQL 参数名称映射到对应的 R 变量名称。

现在，你已将 R 函数包装在了一个存储过程中，你可以轻松调用该函数并传入不同的值，如下所示：

```sql
EXECUTE MyRNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-r-utility-functions-for-troubleshooting"></a>使用 R 实用工具函数进行故障排除

默认安装的 **utils** 包提供各种实用工具函数来调查当前 R 环境。 如果你发现 R 代码在 SQL Server 和外部环境中的执行方式存在差异，那么这些函数会很有用。

例如，可以使用 R 中的系统计时函数（例如 `system.time` 和 `proc.time`）来捕获 R 进程使用的时间并分析性能问题。 有关示例，请参阅教程[创建数据特征](../tutorials/walkthrough-create-data-features.md)，其中的解决方案嵌入了 R 计时函数。

```sql
EXECUTE sp_execute_external_script
      @language = N'R'
    , @script = N'
        library(utils);
        start.time <- proc.time();
        
        # Run R processes
        
        elapsed_time <- proc.time() - start.time;'
```

有关其他有用的函数，请参阅[使用 R 代码分析函数提高性能](../r/using-r-code-profiling-functions.md)。

## <a name="next-steps"></a>后续步骤

若要通过 SQL 机器学习使用 R 创建机器学习模型，请按以下快速入门操作：

> [!div class="nextstepaction"]
> [通过 SQL 机器学习在 R 中创建预测模型并对其进行评分](quickstart-r-train-score-model.md)