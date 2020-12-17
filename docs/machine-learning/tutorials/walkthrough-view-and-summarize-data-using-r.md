---
title: R 教程：浏览数据
description: 教程演示如何使用 R 函数对 SQL Server 上的数据库内分析进行可视化并生成统计摘要。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/26/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0e746113d49c3cfa419a51826405bc993d90c51c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470038"
---
# <a name="view-and-summarize-sql-server-data-using-r-walkthrough"></a>使用 R 查看并汇总 SQL Server 数据（演练）
[!INCLUDE [SQL Server 2016](../../includes/applies-to-version/sqlserver2016.md)]

本课程介绍 RevoScaleR 包中的功能，并指导你完成以下任务  ：

> [!div class="checklist"]
> * 连接到 SQL Server
> * 定义具有所需数据的查询或指定表或视图
> * 定义当运行 R 代码时要使用的一个或多个计算上下文
> * （可选）定义从源读取数据时应用于数据源的转换

## <a name="define-a-sql-server-compute-context"></a>定义 SQL Server 计算上下文

在客户端工作站上的 R 环境中运行以下 R 语句。 本部分假设使用 [Microsoft R Client 的数据科学工作站](../r/set-up-a-data-science-client.md)，因为它包含所有 RevoScaleR 包以及一组基本的轻型 R 工具。 例如，可以使用 Rgui.exe 来运行本部分中的 R 脚本。

1. 如果尚未加载 RevoScaleR 包，请运行以下 R 代码行  ：

    ```R
    library("RevoScaleR")
    ```

     在本例中，可选择使用引号（尽管建议使用引号）。
     
     如果遇到错误，请确保你的 R 开发环境使用的是包括 RevoScaleR 包的库。 使用命令（例如 `.libPaths()`）查看当前的库路径。

2. 为 SQL Server 创建连接字符串，并将其保存到 R 变量 connStr 中  。

   必须将占位符“your_server_name”更改为有效的 SQL Server 实例名称。 对于服务器名称，根据网络，可能只能使用实例名称，或者可能需要完全限定该名称。
    
   对于 SQL Server 身份验证，连接语法如下：

    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Uid=your-sql-login;Pwd=your-login-password"
    ```

    对于 Windows 身份验证，语法稍有不同：
    
    ```R
    connStr <- "Driver=SQL Server;Server=your_server_name;Database=nyctaxi_sample;Trusted_Connection=True"
    ```

    通常，我们建议尽可能使用 Windows 身份验证，以避免在 R 代码中保存密码。

3. 定义在创建新的计算上下文时使用的变量  。 创建计算上下文对象后，可以用其在 SQL Server 实例上运行 R 代码。

    ```R
    sqlShareDir <- paste("C:\\AllShare\\",Sys.getenv("USERNAME"),sep="")
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    ```

    - 在工作站和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机之间来回序列化 R 对象时，R 会使用临时目录。 可指定用作 sqlShareDir  的本地目录，或者接受默认目录。
  
    - 使用 sqlWait 来指示是否要 R 等待来自服务器的结果  。  有关等待作业与非等待作业的讨论，请参阅[使用 Microsoft R 中的 RevoScaleR 进行分布式并行计算](/r-server/r/how-to-revoscaler-distributed-computing)。
  
    - 使用 sqlConsoleOutput 参数来指示不需要显示 R 控制台的输出  。

4. 调用 [RxInSqlServer](/r-server/r-reference/revoscaler/rxinsqlserver) 构造函数来创建具有已定义变量和连接字符串的计算上下文对象，并将该新对象保存在 R 变量 sqlcc 中  。
  
    ```R
    sqlcc <- RxInSqlServer(connectionString = connStr, shareDir = sqlShareDir, wait = sqlWait, consoleOutput = sqlConsoleOutput)
    ```

5. 默认情况下，计算上下文位于本地，因此你需要显式设置活动的计算上下文  。

    ```R
    rxSetComputeContext(sqlcc)
    ```

    + [rxSetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) 会无形返回先前活动的计算上下文，使其可供使用
    + [rxGetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) 返回活动的计算上下文
    
    请注意，设置计算上下文仅影响使用 RevoScaleR 包中的函数的操作；该计算上下文不会影响开放源代码 R 操作的执行方式  。

## <a name="create-a-data-source-using-rxsqlserver"></a>使用 RxSqlServer 创建数据源

在使用 RevoScaleR 和 MicrosoftML 等 Microsoft R 库时，数据源是使用 RevoScaleR 函数创建的对象  。 数据源对象指定了要用于任务（例如模型定型或功能提取）的某些数据集。 可以从各种源（包括 SQL Server）中获取数据。 有关当前支持的源的列表，请参阅 [RxDataSource](/r-server/r-reference/revoscaler/rxdatasource)。

之前，你定义了一个连接字符串，并将该信息保存在 R 变量中。 可以重复使用该连接信息来指定要获取的数据。

1. 将 SQL 查询另存为字符串变量。 查询定义用于定型模型的数据。

    ```R
    sampleDataQuery <- "SELECT TOP 1000 tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"
    ```

    我们在此处使用了 TOP 子句来加快运行速度，但查询返回的实际行可能会根据顺序而有所不同。 因此，摘要结果也可能与下面列出的结果不同。 随意删除 TOP 子句。

2. 将查询定义作为参数传递到 [RxSqlServerData](/r-server/r-reference/revoscaler/rxsqlserverdata) 函数。

    ```R
    inDataSource <- RxSqlServerData(
      sqlQuery = sampleDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )
    ```
    
    + colClasses   参数指定在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 R 间移动数据时要使用的列类型。这一点非常主要，因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的数据类型不同于 R，且使用的数据类型更多。 有关详细信息，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。
  
    + rowsPerRead 参数对于管理内存使用率和高效计算非常重要  。  [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 中大多数增强的分析函数都可以处理区块中的数据并累积中间结果，并在读取所有的数据后返回最终的计算。  通过添加 rowsPerRead 参数，可以控制每个区块读取的数据行数，以进行处理  。  如果此参数的值太大，则数据访问可能很慢，因为没有足够的内存来有效地处理如此大型的数据区块。  在某些系统中，将 rowsPerRead 设置为太小的值也可能使性能下降  。

3. 此时，你已经创建 inDataSource 对象，但该对象未包含任何数据  。 运行参数（例如 [rxImport](/r-server/r-reference/revoscaler/rxdatastep) 或 [rxSummary](/r-server/r-reference/revoscaler/rxsummary)）时数据才会从 SQL 查询提取到本地环境中。

    但是，在定义数据对象后，可以将其用作其他函数的参数。

## <a name="use-the-sql-server-data-in-r-summaries"></a>在 R 摘要中使用 SQL Server 数据

在此部分中，你将尝试 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 中提供的几个支持远程计算上下文的函数。 通过将 R 函数应用到数据源，可以浏览、总结和绘制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据。

1. 调用 [rxGetVarInfo](/r-server/r-reference/revoscaler/rxgetvarinfo) 函数，以获取数据源中的变量列表及其数据类型。

    rxGetVarInfo 是一种方便的函数；可以在远程数据对象中的任何数据帧或数据集上调用该函数，以获取最大值和最小值、数据类型和因子列中的级别数等信息  。
    
    请考虑在进行任何类型的数据输入、功能转换或功能工程后运行此函数。 这样做可以确保你想要在模型中使用的所有功能都为预期的数据类型，并避免错误。
  
    ```R
    rxGetVarInfo(data = inDataSource)
    ```

    **结果**
    
    ```R
    Var 1: tipped, Type: integer
    Var 2: fare_amount, Type: numeric
    Var 3: passenger_count, Type: integer
    Var 4: trip_time_in_secs, Type: numeric, Storage: int64
    Var 5: trip_distance, Type: numeric
    Var 6: pickup_datetime, Type: character
    Var 7: dropoff_datetime, Type: character
    Var 8: pickup_longitude, Type: numeric
    Var 9: pickup_latitude, Type: numeric
    Var 10: dropoff_longitude, Type: numeric
    ```

2. 现在，调用 RevoScaleR 函数 [rxSummary](/r-server/r-reference/revoscaler/rxsummary) 来获取有关各个变量的详细统计信息。

    rxSummary 虽然基于 R `summary` 函数，但具有一些其他功能和优点。 rxSummary 在多个计算上下文中工作，并且支持分块。  还可以使用 rxSummary 来转换值，或基于因子级别进行汇总。
    
    在此示例中，基于乘客的数量汇总票费金额。
    
    ```R
    start.time <- proc.time()
    rxSummary(~fare_amount:F(passenger_count,1,6), data = inDataSource)
    used.time <- proc.time() - start.time
    print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
      Elapsed Time=", round(used.time[3],2),
      " seconds to summarize the inDataSource.", sep=""))
    ```
    + rxSummary 的第一个参数指定汇总所依据的公式或字词。 此处，在汇总前使用 `F()` 函数将 passenger\_count 中的值转换为因子  。 还必须为 passenger\_count 因子变量指定最小值 (1) 和最大值 (6)  。
    + 如果未指定要输出的统计信息，则 rxSummary 默认输出有效和缺失的观察值的 Mean、StDev、Min、Max 和数量。
    + 本示例中也包括跟踪函数开始时间和完成时间的代码，以便可以比较性能。
  
    **结果**

    如果 rxSummary 函数成功运行，则应看到如下所示的结果，后跟按类别列出的统计信息列表。 

    ```R
    rxSummary(formula = ~fare_amount:F(passenger_count, 1,6), data = inDataSource)
    Data: inDataSource (RxSqlServerData Data Source)
    Number of valid observations: 1000
    ```

### <a name="bonus-exercise-on-big-data"></a>针对大数据的额外练习

尝试使用所有行来定义新的查询字符串。 建议为此试验设置新的数据源对象。 还可以尝试更改 rowsToRead 参数，以查看该参数如何影响吞吐量  。

```R
bigDataQuery  <- "SELECT tipped, fare_amount, passenger_count,trip_time_in_secs,trip_distance, pickup_datetime, dropoff_datetime, pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude FROM nyctaxi_sample"

bigDataSource <- RxSqlServerData(
      sqlQuery = bigDataQuery,
      connectionString = connStr,
      colClasses = c(pickup_longitude = "numeric", pickup_latitude = "numeric",
      dropoff_longitude = "numeric", dropoff_latitude = "numeric"),
      rowsPerRead=500
      )

start.time <- proc.time()
rxSummary(~fare_amount:F(passenger_count,1,6), data = bigDataSource)
used.time <- proc.time() - start.time
print(paste("It takes CPU Time=", round(used.time[1]+used.time[2],2)," seconds,
  Elapsed Time=", round(used.time[3],2),
  " seconds to summarize the inDataSource.", sep=""))
```

> [!TIP]
> 在此过程中，可以使用[进程资源管理器](/sysinternals/downloads/process-explorer)或 SQL 事件探查器等工具来查看如何建立连接，以及如何使用 SQL Server 服务运行 R 代码。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 R 创建图形和绘图](walkthrough-create-graphs-and-plots-using-r.md)