---
title: 使用 RevoScaleR 对数据进行评分
description: 使用在上一教程中创建的逻辑回归模型，对使用相同的独立变量作为输入的其他数据集进行评分。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 314520a54bb9052fb091932b63b9cf4c817a0f16
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470498"
---
# <a name="score-new-data-sql-server-and-revoscaler-tutorial"></a>对新数据进行评分（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)的 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 8 个教程。

在本教程中，你将使用在上一教程中创建的逻辑回归模型，对使用相同的独立变量作为输入的其他数据集进行评分。

> [!div class="checklist"]
> * 对新数据进行评分
> * 创建分数直方图

> [!NOTE]
> 其中一些步骤需要具有 DDL 管理员权限。

## <a name="generate-and-save-scores"></a>生成并保存分数
  
1. 将在[教程 2](deepdive-create-sql-server-data-objects-using-rxsqlserverdata.md) 中创建的 sqlScoreDS 数据源更新为，使用在上一教程中创建的列信息。
  
    ```R
    sqlScoreDS <- RxSqlServerData(
        connectionString = sqlConnString,
        table = sqlScoreTable,
        colInfo = ccColInfo,
        rowsPerRead = sqlRowsPerRead)
    ```
  
2. 若要确保不会丢失结果，请创建一个新的数据源对象。 然后，使用新的数据源对象在 RevoDeepDive 数据库中填充新表。
  
    ```R
    sqlServerOutDS <- RxSqlServerData(table = "ccScoreOutput",
        connectionString = sqlConnString,
        rowsPerRead = sqlRowsPerRead )
    ```
    此时尚未创建表。 此语句仅定义数据的容器。
     
3. 使用 rxGetComputeContext() 检查当前计算上下文，并将计算上下文设置为服务器（如果需要）  。
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
4. 作为预防措施，请检查输出表是否存在。 如果已经存在具有相同名称的文件，则在尝试写入新表时会出现错误。
  
    若要执行此操作，请调用函数 [rxSqlServerTableExists](/machine-learning-server/r-reference/revoscaler/rxsqlserverdroptable) 和 [rxSqlServerDropTable](/machine-learning-server/r-reference/revoscaler/rxsqlserverdroptable)，传递表名称作为输入。
  
    ```R
    if (rxSqlServerTableExists("ccScoreOutput"))     rxSqlServerDropTable("ccScoreOutput")
    ```
  
    + 函数 rxSqlServerTableExists 会查询 ODBC 驱动程序，如果表存在，则返回 TRUE，否则返回 FALSE  。
    + 函数 rxSqlServerDropTable 会执行 DDL，如果已成功删除表，则返回 TRUE，否则返回 FALSE  。

5. 执行 [rxPredict](/machine-learning-server/r-reference/revoscaler/rxpredict) 以创建评分，并将其保存在数据源 sqlScoreDS 中定义的新表中。
  
    ```R
    rxPredict(modelObject = logitObj,
        data = sqlScoreDS,
        outData = sqlServerOutDS,
        predVarNames = "ccFraudLogitScore",
          type = "link",
        writeModelVars = TRUE,
        overwrite = TRUE)
    ```
  
    rxPredict  函数是另一种函数，支持在远程计算上下文中运行。 可以使用 rxPredict 函数从基于 [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)、[rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit) 或 [rxGlm](/machine-learning-server/r-reference/revoscaler/rxglm) 创建的模型创建评分  。
  
    - 参数 *writeModelVars* 在此处设置为 **TRUE** 。 这意味着新表中将包含用于估计的变量。
  
    - 参数 *predVarNames* 指定将在其中存储结果的变量。 此处需要传递一个新变量 `ccFraudLogitScore`。
  
    - rxPredict  的 type  参数定义计算预测的方式。 指定关键字“响应”以根据响应变量的比例生成分数  。 或者，使用关键字“链接”根据底层的链接函数生评分，在这种情况下，使用逻辑量表创建预测  。

6. 不久，可以在 Management Studio 中刷新表的列表，以查看新的表及其数据。

7. 若要将其他变量添加到输出预测中，请使用 *extraVarsToWrite* 参数。  例如，在以下代码中，会将来自评分数据表的变量 custID  添加到预测的输出表中。
  
    ```R
    rxPredict(modelObject = logitObj,
            data = sqlScoreDS,
            outData = sqlServerOutDS,
            predVarNames = "ccFraudLogitScore",
              type = "link",
            writeModelVars = TRUE,
            extraVarsToWrite = "custID",
            overwrite = TRUE)
    ```

## <a name="display-scores-in-a-histogram"></a>显示直方图中的评分

创建新表后，计算并显示具有 10,000 个预测评分的直方图。 如果指定下限值和上限值，计算速度会更快，可从数据库获得这些值将并它们添加到需要处理的数据中。

1. 创建新数据源 sqlMinMax，查询数据库以获取下限值和上限值。
  
    ```R
    sqlMinMax <- RxSqlServerData(
        sqlQuery = paste("SELECT MIN(ccFraudLogitScore) AS minVal,",
        "MAX(ccFraudLogitScore) AS maxVal FROM ccScoreOutput"),
        connectionString = sqlConnString)
    ```

     从本示例中，可以看到使用 RxSqlServerData  数据源对象根据 SQL 查询、函数或存储过程定义任意数据集，然后将其用于 R 代码中非常容易。 变量不会存储实际值，只存储数据源定义；仅当在函数（如 rxImport  ）中使用此查询时，才会执行此查询以生成该值。
      
2. 调用 [rxImport](/machine-learning-server/r-reference/revoscaler/rximport) 函数，将这些值放在能够跨计算上下文共享的数据框中。
  
    ```R
    minMaxVals <- rxImport(sqlMinMax)
    minMaxVals <- as.vector(unlist(minMaxVals))
    ```

    **结果**
     
    ```R
    > minMaxVals
     
    [1] -23.970256   9.786345
    ```

3. 最大值和最小值现已可用，请使用这些值为生成的评分创建另一个数据源。
  
    ```R
    sqlOutScoreDS <- RxSqlServerData(sqlQuery = "SELECT ccFraudLogitScore FROM ccScoreOutput",
        connectionString = sqlConnString,
        rowsPerRead = sqlRowsPerRead,
            colInfo = list(ccFraudLogitScore = list(
                low = floor(minMaxVals[1]),
                        high = ceiling(minMaxVals[2]) ) ) )
    ```

4. 使用数据源对象 sqlOutScoreDS 获取评分的数据，并计算和显示一个直方图。 添加代码以根据需要设置计算上下文。
  
    ```R
    # rxSetComputeContext(sqlCompute)
    rxHistogram(~ccFraudLogitScore, data = sqlOutScoreDS)
    ```
  
    **结果**
  
    ![由 R 创建的复杂直方图](media/rsql-sue-complex-histogram.png "由 R 创建的复杂直方图")
  
## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 R 转换数据](../../machine-learning/tutorials/deepdive-transform-data-using-r.md)