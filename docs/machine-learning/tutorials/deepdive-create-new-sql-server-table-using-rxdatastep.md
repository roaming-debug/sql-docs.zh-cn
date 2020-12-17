---
title: 使用 rxDataStep 创建表
description: 学习如何使用 rxDataStep 在内存中数据帧、SQL Server 上下文和本地文件之间移动数据。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 011f2c80f7b59f28fbc5ba49de3613716b27a68a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470608"
---
# <a name="create-new-sql-server-table-using-rxdatastep-sql-server-and-revoscaler-tutorial"></a>使用 rxDataStep 创建新的 SQL Server 表（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)的 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 11 个教程。

在本教程中，你将学习如何在内存中数据帧、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上下文和本地文件之间移动数据。

> [!NOTE]
> 本教程使用不同的数据集。 Airline Delays 数据集是广泛用于机器学习试验的公共数据集。 本示例中使用的数据文件与其他产品示例在同一目录中。

## <a name="load-data-from-a-local-xdf-file"></a>从本地 XDF 文件加载数据

在本教程系列的前半部分，已使用 RxTextData  函数将数据从文本文件导入 R 中，然后使用 RxDataStep  函数将数据移动到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中。

本教程采用不同的方法，并使用以 [XDF 格式](https://en.wikipedia.org/wiki/Extensible_Data_Format)保存的文件中的数据。 使用 XDF 文件对数据执行一些轻型转换后，将转换的数据保存到新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中。

**什么是 XDF？**

XDF 格式是针对高维数据开发的 XML 标准，并且是 [Machine Learning Server](/machine-learning-server/r/concept-what-is-xdf) 使用的本机文件格式。 其为二进制文件格式，具有用于优化行和列的处理与分析的 R 接口。  可将其用于移动数据和存储对分析有用的数据子集。

1. 将计算上下文设置为本地工作站。 **此步骤需要 DDL 权限。**

    ```R
    rxSetComputeContext("local")
    ```
  
2. 使用 RxXdfData  函数定义新数据源对象。 若要定义 XDF 数据源，请指定数据文件的路径。  

    可以使用文本变量指定文件的路径。 但在这种情况下，有一种便捷的快捷方式，即，使用 **rxGetOption** 函数并从示例数据目录中获取文件 (AirlineDemoSmall.xdf)。
  
    ```R
    xdfAirDemo <- RxXdfData(file.path(rxGetOption("sampleDataDir"),  "AirlineDemoSmall.xdf"))
    ```

3. 对内存中数据调用 [rxGetVarInfo](/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf) 以查看数据集的摘要。
  
    ```R
    rxGetVarInfo(xdfAirDemo)
    ```

**结果**

```R
Var 1: ArrDelay, Type: integer, Low/High: (-86, 1490)
Var 2: CRSDepTime, Type: numeric, Storage: float32, Low/High: (0.0167, 23.9833)
Var 3: DayOfWeek 7 factor levels: Monday Tuesday Wednesday Thursday Friday Saturday Sunday
```

> [!NOTE]
> 
> 你是否注意将数据加载到 XDF 文件时不需要调用任何其他函数，并可立即对该数据调用 rxGetVarInfo  ？ 这是因为 XDF 是 **RevoScaleR** 的默认临时存储方法。 除了 XDF 文件之外，**rxGetVarInfo** 函数现在还支持多种源类型。

## <a name="move-contents-to-sql-server"></a>将内容移动到 SQL Server

借助在本地 R 会话中创建的 XDF 数据源，现在可以将该数据移到数据库表中，并将 *DayOfWeek* 存储为一个整数，其值介于 1 到 7 之间。

1. 定义一个 SQL Server 数据源对象，并指定要包含数据的表以及与远程服务器的连接。
  
    ```R
    sqlServerAirDemo <- RxSqlServerData(table = "AirDemoSmallTest", connectionString = sqlConnString)
    ```
  
2. 为预防起见，可增加一个步骤：检查是否已存在具有相同名称的表，如果存在，则删除该表。 现有的同名表会阻止创建新表。
  
    ```R
    if (rxSqlServerTableExists("AirDemoSmallTest",  connectionString = sqlConnString))  rxSqlServerDropTable("AirDemoSmallTest",  connectionString = sqlConnString)
    ```
  
3. 使用 **rxDataStep** 将数据加载到表中。 此函数在两个已定义的数据源之间移动数据，并且可以选择在途中转换数据。
  
    ```R
    rxDataStep(inData = xdfAirDemo, outFile = sqlServerAirDemo,
        transforms = list( DayOfWeek = as.integer(DayOfWeek),
        rowNum = .rxStartRow : (.rxStartRow + .rxNumRows - 1) ),
        overwrite = TRUE )
    ```
  
    这个表相当大，因此请耐心等待，直到看到如下所示的最终状态消息：*读取的行数:200000，已处理的总行数:600000*。
     
## <a name="load-data-from-a-sql-table"></a>从 SQL 表中加载数据

一旦表中存在数据，就可以使用简单的 SQL 查询来加载数据。 

1. 创建一个新的 SQL Server 数据源。 其输入是对刚刚创建的新表的查询，该表已加载数据。 此定义使用 **RxSqlServerData** 的 *colInfo* 参数为 *DayOfWeek* 列添加因素级别。
  
    ```R
    sqlServerAirDemo2 <- RxSqlServerData(
        sqlQuery = "SELECT * FROM AirDemoSmallTest",
        connectionString = sqlConnString,
        rowsPerRead = 50000,
        colInfo = list(DayOfWeek = list(type = "factor",  levels = as.character(1:7))))
    ```
  
2. 在查询中再次调用  **rxSummary** 以查看数据摘要。
  
    ```R
    rxSummary(~., data = sqlServerAirDemo2)
    ```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 rxDataStep 执行区块分析](../../machine-learning/tutorials/deepdive-perform-chunking-analysis-using-rxdatastep.md)