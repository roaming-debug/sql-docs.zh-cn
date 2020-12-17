---
title: 创建 Python 模型 - revoscalepy
description: 使用 revoscalepy 函数编写 Python 脚本，以创建在 SQL Server 中远程运行的数据科学模型。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/25/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15'
ms.openlocfilehash: aa5e3e9da0e13a9946ed0a2c985512a7e9452307
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470068"
---
# <a name="use-python-with-revoscalepy-to-create-a-model-that-runs-remotely-on-sql-server"></a>将 Python 与 revoscalepy 配合使用，创建在 SQL Server 上远程运行的模型
[!INCLUDE [SQL Server 2017](../../includes/applies-to-version/sqlserver2017.md)]

来自 Microsoft 的 [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package) Python 库提供了用于数据探索、可视化、转换和分析的数据科学算法。 该库在 SQL Server 中的 Python 集成方案中具有重要的战略意义。 在多核服务器上，revoscalepy 函数可并行运行  。 在具有中央服务器和客户端工作站（单独的物理计算机，均具有相同的 revoscalepy 库）的分布式体系结构中，可编写在本地启动的 Python 代码，但随后需将执行转移到数据所在的远程 SQL server 实例  。

可在以下 Microsoft 产品和分发中找到 revoscalepy  ：

+ [SQL Server 机器学习服务（数据库内）](../install/sql-machine-learning-services-windows-install.md)
+ [Microsoft Machine Learning Server（非 SQL，独立服务器）](/machine-learning-server/index)
+ [客户端 Python 库（用于开发工作站）](/machine-learning-server/install/python-libraries-interpreter) 

本练习演示了如何基于 [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod) 创建线性回归模型，该模型是 revoscalepy 中的一种接受计算上下文作为输入的算法  。 在本练习中运行的代码将代码执行从本地计算环境移动至远程计算环境，远程计算环境是通过 revoscalepy 函数启用的，该函数可启用远程计算上下文  。

学完本教程后，你将了解如何执行以下操作：

> [!div class="checklist"]
> * 使用 revoscalepy 创建线性模型 
> * 将操作从本地计算上下文移动至远程计算上下文

## <a name="prerequisites"></a>先决条件

本练习中使用的示例数据是 [flightdata](demo-data-airlinedemo-in-sql.md) 数据库  。

需要 IDE 以运行本文中的示例代码，且必须将 IDE 链接到 Python 可执行文件。

要练习计算上下文移动，需要[本地工作站](../python/setup-python-client-tools-sql.md)和已启用[机器学习服务](../install/sql-machine-learning-services-windows-install.md)和 Python 的 SQL Server 数据库引擎实例。 

> [!Tip]
> 如果没有两台计算机，则可以通过安装相关应用程序在一台物理计算机上模拟远程计算上下文。 首先，安装 [SQL Server 机器学习服务](../install/sql-machine-learning-services-windows-install.md)作为“远程”实例运行。 其次，安装 [Python 客户端库](../python/setup-python-client-tools-sql.md)作为客户端运行。 同一台计算机上将具有相同 Python 分发和 Microsoft Python 库的两个副本。 必须跟踪文件路径以及正在使用的 Python.exe 副本才能成功完成练习。

## <a name="remote-compute-contexts-and-revoscalepy"></a>远程计算上下文和 revoscalepy

此示例演示了在远程计算上下文中创建 Python 模型的过程，通过该示例可从客户端进行工作，但是请选择实际执行操作的远程环境（例如 SQL Server、Spark 或 Machine Learning Server）。 远程计算上下文的目标是将计算引入数据所在的位置。

要在 SQL Server 中执行 Python 代码，需要使用 revoscalepy 包  。 这是由 Microsoft 提供的一种特殊 Python 包，类似于 R 语言的 RevoScaleR 包  。 revoscalepy 包支持创建计算上下文，并提供用于在本地工作站和远程服务器之间传递数据和模型的基础结构  。 支持数据库内代码执行的 revoscalepy 函数是 [RxInSqlServer](/machine-learning-server/python-reference/revoscalepy/rxinsqlserver)  。

在本课程中，你将使用 SQL Server 中的数据来定型基于 [rx_lin_mod](/machine-learning-server/python-reference/revoscalepy/rx-lin-mod) 的线性模型，这是 revoscalepy 中的一个函数，支持对大型数据集进行回归  。 

本课程还演示了如何在 Python 中设置并使用 SQL Server 计算上下文的基础知识  。 有关如何将计算上下文与其他平台一起使用以及支持哪些计算上下文的讨论，请参阅 [Compute context for script execution in Machine Learning Server](/machine-learning-server/r/concept-what-is-compute-context)（用于在 Machine Learning Server 中执行脚本的计算上下文）。


## <a name="run-the-sample-code"></a>运行示例代码

准备好数据库并将用于定型的数据存储在表格中后，请打开 Python 开发环境并运行代码示例。

此代码执行以下步骤：

1. 导入所需的库和函数。
2. 创建与 SQL Server 的连接。 创建“数据源”对象以处理数据  。
3. 使用转换修改数据，以便逻辑回归算法可以使用该数据  。
4. 调用 `rx_lin_mod` 并定义用于拟合模型的公式。
5. 根据原始数据生成一组预测。
6. 根据预测值创建摘要。

使用 SQL Server 实例作为计算上下文执行所有操作。

> [!NOTE]
> 有关从命令行运行此示例的演示，请参阅以下视频：[使用 Python 的 SQL Server 2017 高级分析](https://www.youtube.com/watch?v=FcoY795jTcc)

### <a name="sample-code"></a>示例代码

```python
from revoscalepy import RxComputeContext, RxInSqlServer, RxSqlServerData
from revoscalepy import rx_lin_mod, rx_predict, rx_summary
from revoscalepy import RxOptions, rx_import

import os

def test_linmod_sql():
    sql_server = os.getenv('PYTEST_SQL_SERVER', '.')
    
    sql_connection_string = 'Driver=SQL Server;Server=' + sqlServer + ';Database=sqlpy;Trusted_Connection=True;'
    print("connectionString={0!s}".format(sql_connection_string))

    data_source = RxSqlServerData(
        sql_query = "select top 10 * from airlinedemosmall",
        connection_string = sql_connection_string,

        column_info = {
            "ArrDelay" : { "type" : "integer" },
            "DayOfWeek" : {
                "type" : "factor",
                "levels" : [ "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday" ]
            }
        })

    sql_compute_context = RxInSqlServer(
        connection_string = sql_connection_string,
        num_tasks = 4,
        auto_cleanup = False
        )

    #
    # Run linmod locally
    #
    linmod_local = rx_lin_mod("ArrDelay ~ DayOfWeek", data = data_source)
    #
    # Run linmod remotely
    #
    linmod = rx_lin_mod("ArrDelay ~ DayOfWeek", data = data_source, compute_context = sql_compute_context)

    # Predict results
    # 
    predict = rx_predict(linmod, data = rx_import(input_data = data_source))
    summary = rx_summary("ArrDelay ~ DayOfWeek", data = data_source, compute_context = sql_compute_context)
```

### <a name="defining-a-data-source-vs-defining-a-compute-context"></a>定义数据源与定义计算上下文

数据源与计算上下文不同。 数据源定义代码中使用的数据  。 计算上下文定义将执行代码的位置。 但是，它们使用一些相同的信息：

+ Python 变量（例如 `sql_query` 和 `sql_connection_string`），定义数据源。 

    将这些变量传递给 [RxSqlServerData](/r-server/python-reference/revoscalepy/rxsqlserverdata) 构造函数，以实现名为 `data_source` 的数据源对象  。

+ 可使用 [RxInSqlServer](/machine-learning-server/python-reference/revoscalepy/rxinsqlserver) 构造函数创建计算上下文对象  。 将生成的计算上下文对象命名为 `sql_cc`  。

    本示例重用了在数据源中使用的相同连接字符串，假设数据位于将用作计算上下文的同一 SQL Server 实例。 
    
    但是，数据源和计算上下文可能位于不同的服务器上。
 
### <a name="changing-compute-contexts"></a>更改计算上下文

定义计算上下文后，必须设置活动计算上下文  。 

默认情况下，大多数操作都在本地运行，这意味着，如果未指定其他计算上下文，则将从数据源中提取数据，而代码将在当前的 Python 环境中运行。

可以通过两种方式设置活动计算上下文：
+ 作为方法或函数的参数
+ 通过调用 `rx_set_computecontext`

#### <a name="set-compute-context-as-an-argument-of-a-method-or-function"></a>将计算上下文设置为方法或函数的参数

在此示例中，通过使用单个 rx 函数的参数来设置计算上下文  。
    
`linmod = rx_lin_mod_ex("ArrDelay ~ DayOfWeek", data = data, compute_context = sql_compute_context)`

在对 [rxsummary](/machine-learning-server/python-reference/revoscalepy/rx-summary) 的调用中重用此计算上下文。

`summary = rx_summary("ArrDelay ~ DayOfWeek", data = data_source, compute_context = sql_compute_context)`

#### <a name="set-a-compute-context-explicitly-using-rx_set_compute_context"></a>使用 rx_set_compute_context 显式设置计算上下文

通过函数 [rx_set_compute_context](/machine-learning-server/python-reference/revoscalepy/rx-set-compute-context) 可在已定义的计算上下文之间进行切换。

设置活动计算上下文后，在更改前，它将保持活动状态。

### <a name="using-parallel-processing-and-streaming"></a>使用并行处理和流式处理

定义计算上下文时，还可设置参数以控制计算上下文处理数据的方式。 这些参数因数据源类型而异。

对于 SQL Server 计算上下文，可设置批大小，或提供有关要在正在运行的任务中使用的并行度的提示。

+ 该示例在具有四个处理器的计算机上运行，因此将 `num_tasks` 参数设置为 4 可以最大程度地利用资源。 
+ 如果将此值设置为 0，则 SQL Server 将使用默认值，即在服务器的当前 MAXDOP 设置下并行运行尽可能多的任务。 但是，可分配的确切任务数取决于许多其他因素，例如服务器设置和正在运行的其他作业。

## <a name="next-steps"></a>后续步骤

这些附加的 Python 示例和教程使用更复杂的数据源演示了端到端方案，以及远程数据上下文的用法。

+ [面向 SQL 开发人员的数据库内 Python](python-taxi-classification-introduction.md)
+ [使用 Python 和 SQL Server 生成预测模型](https://microsoft.github.io/sql-ml-tutorials/python/rentalprediction/)