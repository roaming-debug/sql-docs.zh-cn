---
title: RevoScaleR 中的分块分析
description: RevoScaleR 教程 12：如何在 SQL Server 上使用 R 语言对数据分块以供分布式分析。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 38749e2c0e0bea1d70b253a9bc1dcbcae1c54de3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470518"
---
# <a name="perform-chunking-analysis-using-rxdatastep-sql-server-and-revoscaler-tutorial"></a>使用 rxDataStep 执行分块分析（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 12 个教程，RevoScaleR 教程介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)。

在本教程中，你将使用 rxDataStep 函数来处理区块中的数据，而不是像传统 R 中那样，要求将整个数据集一次加载到内存中并进行处理。rxDataStep 函数会读取区块中的数据，并将 R 函数依次应用到每个区块的数据，然后将每个区块的摘要结果保存到常见 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源   。 读取所有数据后，将合并结果。

> [!TIP]
> 在本教程中，将通过使用 R 中的表函数来计算列联表。本示例仅用于说明目的  。 
> 
> 如果需要将真实数据集制成表格，建议使用 RevoScaleR 中的 rxCrossTabs 或 rxCube 函数，这些函数已针对此类操作进行了优化    。

## <a name="partition-data-by-values"></a>按值的分区数据

1. 创建自定义 R 函数，该函数对每个区块的数据调用 R 表函数，然后将该新函数命名为 ProcessChunk   。
  
    ```R
    ProcessChunk <- function( dataList) {
    # Convert the input list to a data frame and compute contingency table
    chunkTable <- table(as.data.frame(dataList))
  
    # Convert table output to a data frame with a single row
    varNames <- names(chunkTable)
    varValues <- as.vector(chunkTable)
    dim(varValues) <- c(1, length(varNames))
    chunkDF <- as.data.frame(varValues)
    names(chunkDF) <- varNames
  
    # Return the data frame, which has a single row
    return( chunkDF )
    }
    ```

2. 设置服务器的计算上下文。
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
3. 定义 SQL Server 数据源以保存正在处理的数据。 首先将 SQL 查询分配给变量。 然后，在新的 SQL Server 数据源的 sqlQuery 参数中使用该变量  。
  
    ```R
    dayQuery <-  "SELECT DayOfWeek FROM AirDemoSmallTest"
    inDataSource <- RxSqlServerData(sqlQuery = dayQuery,
        connectionString = sqlConnString,
        rowsPerRead = 50000,
        colInfo = list(DayOfWeek = list(type = "factor",
            levels = as.character(1:7))))
    ```

4. （可选）可在此数据源上运行 rxGetVarInfo  。 此时，它包含单个列：“Var 1：  DayOfWeek，类型：因子，没有可用的因子级别”
     
5. 将此因子变量应用到源数据之前，先创建单独的表以保存中间结果。 同样，只需使用 RxSqlServerData 函数定义数据，并确保删除所有同名的现有表  。
  
    ```R
    iroDataSource = RxSqlServerData(table = "iroResults",   connectionString = sqlConnString)
    # Check whether the table already exists.
    if (rxSqlServerTableExists(table = "iroResults",  connectionString = sqlConnString))  { rxSqlServerDropTable( table = "iroResults", connectionString = sqlConnString) }
    ```
  
7.  调用自定义函数 ProcessChunk，在读取数据的同时转换数据，方法是将其用作 rxDataStep 函数的 transformFunc 参数    。
  
    ```R
    rxDataStep( inData = inDataSource, outFile = iroDataSource, transformFunc = ProcessChunk, overwrite = TRUE)
    ```
  
8.  若要查看 ProcessChunk 的中间结果，请将 rxImport 的结果分配给一个变量，然后将结果输出到控制台   。
  
    ```R
    iroResults <- rxImport(iroDataSource)
    iroResults
    ```

    **部分结果**

    | 行 \# |    1  |   2   |  3   |  4   |  5  |   6   |  7 |
    | --- | ---  | --- | ---  |  ---  | ---  | ---  | --- |
    | 1 | 8228 | 8924 | 6916 | 6932 | 6944 | 5602 | 6454 |
    | 2  | 8321  | 5351 | 7329 | 7411 | 7409 | 6487 | 7692 |

9. 若要计算所有区块的最终结果，可以汇总各列，并将结果显示在控制台中。

    ```R
    finalResults <- colSums(iroResults)
    finalResults
    ```

    **结果**

    1  |   2  |   3  |   4  |   5  |   6  |   7
    ---  |   ---  |   ---  |   ---  |   ---  |   ---  |   ---
    97975 | 77725 | 78875 | 81304 | 82987 | 86159 | 94975 

10. 若要删除中间结果表，请调用 rxSqlServerDropTable。
  
    ```R
    rxSqlServerDropTable( table = "iroResults", connectionString = sqlConnString)
    ```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [SQL Server 的 R 教程](./r-tutorials.md)