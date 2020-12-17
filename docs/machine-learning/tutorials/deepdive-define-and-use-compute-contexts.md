---
title: 使用 RevoScaleR 计算上下文
description: 了解 RxInSqlServer 函数，该函数可用于为远程 SQL Server 定义计算上下文。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 074597a032fbcaa8231dfea22d567204cb079f58
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470548"
---
# <a name="define-and-use-compute-contexts-sql-server-and-revoscaler-tutorial"></a>定义并使用计算上下文（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)的 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 4 个教程。

在上一教程中，已使用 RevoScaleR  函数检查数据对象。 本教程介绍了 [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) 函数，它可用于为远程 SQL Server 定义计算上下文。 借助远程计算上下文，可以将 R 执行任务从本地会话转移到服务器上的远程会话。 

> [!div class="checklist"]
> * 了解远程 SQL Server 计算上下文的各个元素
> * 对计算上下文对象启用跟踪

**RevoScaleR** 支持多种计算上下文：Hadoop、Spark on HDFS 以及 SQL Server 数据库内。 对于 SQL Server，**RxInSqlServer** 函数用于服务器连接，以及在本地计算机和远程执行上下文之间传递对象。

## <a name="create-and-set-a-compute-context"></a>创建并设置计算上下文

用于创建 SQL Server 计算上下文的 **RxInSqlServer** 函数使用以下信息：

+ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的连接字符串
+ 输出处理方式的规范
+ 共享数据目录的可选规范
+ 用于启用跟踪或指定跟踪级别的可选参数

本部分将指导你完成每个部分。

1. 为执行计算的实例指定连接字符串。 可以重新使用之前创建的连接字符串。

    **使用 SQL 登录名**

    ```R
    sqlConnString <- "Driver=SQL Server;Server=<SQL Server instance name>; Database=<database name>;Uid=<SQL user nme>;Pwd=<password>"
      ```

    **使用 Windows 身份验证**

    ```R
    sqlConnString <- "Driver=SQL Server;Server=instance_name;Database=RevoDeepDive;Trusted_Connection=True"
    ```
    
2. 指定所需的输出处理方式。 以下脚本指示本地 R 会话在处理下一个操作之前，先等待服务器上的 R 作业结果。 它还禁止在本地会话中显示远程计算的输出。
  
    ```R
    sqlWait <- TRUE
    sqlConsoleOutput <- FALSE
    ```
  
    到 RxInSqlServer  的 wait  参数支持以下选项：
  
    -   **TRUE**。 作业被配置为具有阻塞性且不会返回，直到它完成或失败。  有关详细信息，请参阅 [Machine Learning Server 中的分布式计算和并行计算](/machine-learning-server/r/how-to-revoscaler-distributed-computing)。
  
    -   **FALSE**。 作业被配置为具有非阻塞性且会立即返回，你可以继续运行其他 R 代码。 但是，即使在非阻塞模式下，作业运行时，也必须维持客户端与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的连接。

3. 或者，指定本地 R 会话与远程 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机及其帐户共用的本地目录的位置。

    ```R
    sqlShareDir <- paste("c:\\AllShare\\", Sys.getenv("USERNAME"), sep="")
    ```
    
   如果要手动创建用于共享的特定目录，可以添加如下所示的代码行：

    ```R
    dir.create(sqlShareDir, recursive = TRUE)
    ```

4. 向 **RxInSqlServer** 构造函数传递参数，以创建 *计算上下文对象*。

    ```R
    sqlCompute <- RxInSqlServer(  
         connectionString = sqlConnString,
         wait = sqlWait,
         consoleOutput = sqlConsoleOutput)
    ```
    
    **RxInSqlServer** 的语法与之前用来定义数据源的 **RxSqlServerData** 函数的语法看上去差不多。 但是，它们之间存在以下重要差异。
      
    - 使用函数 [RxSqlServerData](/machine-learning-server/r-reference/revoscaler/rxsqlserverdata) 定义的数据源对象指定数据的存储位置。
    
    - 与之相反，使用函数 [RxInSqlServer](/machine-learning-server/r-reference/revoscaler/rxinsqlserver) 定义的计算上下文指示聚合和其他计算发生的位置。
    
    定义计算上下文不会影响任何其他可能在工作站上执行的泛型 R 计算，也不会更改数据源。 例如，你可以将本地文本文件定义为数据源，但会将计算上下文更改为 SQL Server 并对 SQL Server 计算机上的数据执行所有的读取和汇总操作。

5. 激活远程计算上下文。

    ```R
    rxSetComputeContext(sqlCompute)
    ```

6. 返回有关计算上下文的信息，包括其属性。

    ```R
    rxGetComputeContext()
    ```

7. 通过指定“local”关键字，将计算上下文重置回本地计算机（下一教程展示了如何使用远程计算上下文）。

    ```R
    rxSetComputeContext("local")
    ```

> [!Tip]
> 有关此函数支持的其他关键字的列表，请从 R 命令行键入 `help("rxSetComputeContext")` 。

## <a name="enable-tracing"></a>启用跟踪

有时操作在本地上下文中正常工作，但在远程计算上下文中运行时会有问题。 如果想要分析问题或监视性能，则可以在计算上下文中启用跟踪，以支持运行时故障排除。

1. 创建使用相同连接字符串的新计算上下文，但将参数 *traceEnabled* 和 *traceLevel* 添加到 **RxInSqlServer** 构造函数中。

    ```R
    sqlComputeTrace <- RxInSqlServer(
        connectionString = sqlConnString,
        #shareDir = sqlShareDir,
        wait = sqlWait,
        consoleOutput = sqlConsoleOutput,
        traceEnabled = TRUE,
        traceLevel = 7)
    ```
  
   在此示例中，将 *traceLevel* 属性设置为 7，这意味着“显示所有跟踪信息”。

2. 使用 [rxSetComputeContext](/machine-learning-server/r-reference/revoscaler/rxsetcomputecontext) 函数按名称指定启用了跟踪的计算上下文。

    ```R
    rxSetComputeContext(sqlComputeTrace)
    ```

## <a name="next-steps"></a>后续步骤

了解如何切换计算上下文以在服务器上或在本地运行 R 代码。

> [!div class="nextstepaction"]
> [本地和远程计算上下文中的计算摘要统计信息](../../machine-learning/tutorials/deepdive-create-and-run-r-scripts.md)