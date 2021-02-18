---
title: 快速入门：Python 函数
titleSuffix: SQL machine learning
description: 在本快速入门中，你将学习如何在 SQL 机器学习中运用 Python 数学函数和效用函数。
ms.prod: sql
ms.technology: machine-learning
ms.date: 09/28/2020
ms.topic: quickstart
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 5871307e3bdd5be271547624089c9c47d2932739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351441"
---
# <a name="quickstart-python-functions-with-sql-machine-learning"></a>快速入门：在 SQL 机器学习中使用 Python 函数
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

本快速入门介绍将 Python 数学和实用函数与 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)、[Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)或 [SQL Server 大数据群集](../../big-data-cluster/machine-learning-services.md)配合使用。 统计函数在 T-SQL 中实现起来通常很复杂，但在 Python 中只需几行代码就可以完成。

## <a name="prerequisites"></a>先决条件

若要运行本快速入门，需要具备以下先决条件。

- 以下平台之一上的 SQL 数据库：
  - [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)。 如需安装，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json)。
  - SQL Server 大数据群集。 了解如何[在 SQL Server 大数据群集上启用机器学习服务](../../big-data-cluster/machine-learning-services.md)。
  - Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。

- 用于运行包含 Python 脚本的 SQL 查询的工具。 本快速入门使用 [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md)。

## <a name="create-a-stored-procedure-to-generate-random-numbers"></a>创建一个存储过程来生成随机数字

为简单起见，我们使用默认安装并加载的 Python `numpy` 包。 此包中包含数百个用于执行常用统计任务的函数，其中，`random.normal` 函数在给定了标准差和平均数的情况下使用正态分布生成指定数量的随机数字。

例如，以下 Python 代码在给定了标准差 3 的情况下返回平均数为 50 的 100 个数字。

```Python
numpy.random.normal(size=100, loc=50, scale=3)
```

若要从 T-SQL 调用此行 Python，请在 `sp_execute_external_script` 的 Python 脚本参数中添加 Python 函数。 输出需要一个数据帧，因此使用 `pandas` 来转换它。

```sql
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=100, loc=50, scale=3));
'
    , @input_data_1 = N'   ;'
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

如果你希望更轻松地生成不同的一组随机数字，那该怎么办？ 定义一个存储过程，从用户那里获取参数，然后将这些参数作为变量传递给 Python 脚本。

```sql
CREATE PROCEDURE MyPyNorm (
      @param1 INT
    , @param2 INT
    , @param3 INT
    )
AS
EXECUTE sp_execute_external_script @language = N'Python'
    , @script = N'
import numpy
import pandas
OutputDataSet = pandas.DataFrame(numpy.random.normal(size=mynumbers, loc=mymean, scale=mysd));
'
    , @input_data_1 = N'   ;'
    , @params = N' @mynumbers int, @mymean int, @mysd int'
    , @mynumbers = @param1
    , @mymean = @param2
    , @mysd = @param3
WITH RESULT SETS(([Density] FLOAT NOT NULL));
```

- 第一行定义了在执行该存储过程时必需的每个 SQL 输入参数。

- 以 `@params` 开头的行定义了 Python 代码使用的所有变量，以及对应的 SQL 数据类型。

- 紧跟在后面的行将 SQL 参数名称映射到对应的 Python 变量名称。

现在，你已将 Python 函数包装在了一个存储过程中，轻松调用该函数并传入不同的值，如下所示：

```sql
EXECUTE MyPyNorm @param1 = 100,@param2 = 50, @param3 = 3
```

## <a name="use-python-utility-functions-for-troubleshooting"></a>使用 Python 实用工具函数进行故障排除

Python 包提供了多种实用函数来调查当前的 Python 环境。 如果你发现 Python 代码在 SQL Server 中执行时和在环境外执行时执行方式存在差异，则上述函数会比较有用。

例如，你可以使用 `time` 包中的系统计时函数来测量 Python 进程使用的时间量并分析性能问题。

```sql
EXECUTE sp_execute_external_script
      @language = N'Python'
    , @script = N'
import time
start_time = time.time()

# Run Python processes

elapsed_time = time.time() - start_time
'
    , @input_data_1 = N' ;';
```

## <a name="next-steps"></a>后续步骤

若要在 SQL 机器学习中使用 Python 创建机器学习模型，请按以下快速入门操作：

> [!div class="nextstepaction"]
> [快速入门：在 Python 中创建预测模型并对其进行评分](quickstart-python-train-score-model.md)