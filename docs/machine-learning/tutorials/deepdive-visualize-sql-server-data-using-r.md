---
title: 使用 RevoScaleR 实现数据的可视化效果
description: 了解如何使用 R 函数来可视化 creditLine 列中值的分布（按性别）。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: e9158976b1827eb32e807af1cc97279b0866ff97
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470478"
---
#  <a name="visualize-sql-server-data-using-r-sql-server-and-revoscaler-tutorial"></a>使用 R 实现 SQL Server 数据的可视化效果（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是 [RevoScaleR 教程](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 6 个教程，RevoScaleR 教程介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)。

在本教程中，你将使用 R 函数来查看 creditLine  列中值的分布（按性别）。

> [!div class="checklist"]
> * 创建直方图输入的 min-max 变量
> * 使用 RevoScaleR 中的 rxHistogram 以直方图的形式实现数据的可视化效果  
> * 通过使用基本 R 分发中包含的 lattice 中的 levelplot 以散点图的形式实现可视化效果  

如本教程所示，可以在同一脚本中合并开放源代码和 Microsoft 特定函数。

## <a name="add-maximum-and-minimum-values"></a>添加最大值和最小值

根据上一教程的计算摘要统计信息，你会发现一些有用的数据信息，可以将这些数据插入数据源以进行进一步的计算。 例如，最小值和最大值可用于计算直方图。 在此练习中，将高值和低值添加到 RxSqlServerData 数据源  。

1. 首先设置一些临时变量。
  
    ```R
    sumDF <- sumOut$sDataFrame
    var <- sumDF$Name
    ```
  
2. 使用上一教程中创建的变量 ccColInfo  定义数据源中的列。
  
   将新的计算列（numTrans、numIntlTrans 和 creditLine）添加到替代原始定义的列集合中    。 下面的脚本基于最小值和最大值添加了因子，这些值获取自 sumOut（它存储了 rxSummary 的内存中的输出）  。 
  
    ```R 
    ccColInfo <- list(
        gender = list(type = "factor",
          levels = c("1", "2"), 
          newLevels = c("Male", "Female")),
        cardholder = list(type = "factor",
          levels = c("1", "2"), 
          newLevels = c("Principal", "Secondary")), 
        state = list(type = "factor", 
          levels = as.character(1:51), 
          newLevels = stateAbb), 
        balance  = list(type = "numeric"),
        numTrans = list(type = "factor", 
          levels = as.character(sumDF[var == "numTrans", "Min"]:sumDF[var == "numTrans", "Max"])),
        numIntlTrans = list(type = "factor",  
            levels = as.character(sumDF[var == "numIntlTrans", "Min"]:sumDF[var =="numIntlTrans", "Max"])),
        creditLine = list(type = "numeric")
            )
    ```
  
3. 更新列集合后，应用以下语句创建之前定义的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据源的更新版本。
  
    ```R
    sqlFraudDS <- RxSqlServerData(
        connectionString = sqlConnString,
        table = sqlFraudTable,
        colInfo = ccColInfo,
        rowsPerRead = sqlRowsPerRead)
    ```
  
    现在，sqlFraudDS 数据源包括使用 ccColInfo 添加的新列  。
  
目前，修改仅影响 R 中的数据源对象；还未向数据库表写入任何新数据。 但是，你可以使用在 sumOut 变量中捕获的数据来创建可视化效果和摘要。 

> [!TIP]
> 如果忘记了使用的计算上下文，请运行 rxGetComputeContext()  。 如果返回值为“RxLocalSeq Compute Context”，则表示你正在本地计算上下文中运行。

## <a name="visualize-data-using-rxhistogram"></a>使用 rxHistogram 实现数据的可视化效果

1. 使用以下 R 代码来调用 [rxHistogram](/machine-learning-server/r-reference/revoscaler/rxhistogram) 函数并传递公式和数据源。 可首先在本地运行，查看预期的结果以及需要的时间。
  
    ```R
    rxHistogram(~creditLine|gender, data = sqlFraudDS,  histType = "Percent")
    ```
 
    在内部，**rxHistogram** 调用 [rxCube](/machine-learning-server/r-reference/revoscaler/rxcube) 函数，它包含在 **RevoScaleR** 包中。 rxCube 输出一个列表（或数据框架），其中包括针对公式中指定的每个变量的一个列，以及一个计数列  。
    
2. 现在，将计算上下文设置为远程 SQL Server 计算机并再次运行 rxHistogram  。
  
    ```R
    rxSetComputeContext(sqlCompute)
    rxHistogram(~creditLine|gender, data = sqlFraudDS,  histType = "Percent")
    ```
 
3. 因为你使用的是相同的数据源，所以结果是完全相同的；但在第二步中，计算是在远程服务器上执行的。 结果将返回到本地工作站用于绘图。
   
  ![直方图结果](media/rsql-sue-histogramresults.jpg "直方图结果")


## <a name="visualize-with-scatter-plots"></a>使用散点图实现可视化效果

散点图通常在数据浏览过程中用于比较两个变量之间的关系。 为此，你可以采用 RevoScaleR 函数提供的输入来使用内置的 R 包  。

1. 对于 numTrans 和 numIntlTrans 的每个组合，调用 [rxCube](/machine-learning-server/r-reference/revoscaler/rxcrosstabs) 函数来计算 fraudRisk 的平均值    ：
  
    ```R
    cube1 <- rxCube(fraudRisk~F(numTrans):F(numIntlTrans),  data = sqlFraudDS)
    ```
  
    若要指定用于计算组均值的组，请使用 `F()` 表示法。 在本示例中，`F(numTrans):F(numIntlTrans)` 表明变量 `numTrans` 和 `numIntlTrans` 中的整数应视为类别变量，并且每个整数值都有一个级别。
  
    rxCube 的默认返回值为表示交叉表的 rxCube object   。 
  
2. 调用 [rxResultsDF](/machine-learning-server/r-reference/revoscaler/rxresultsdf) 函数将结果转换为一个数据框架，可轻松将此数据框架用于 R 的一个标准绘图函数。
  
    ```R
    cubePlot <- rxResultsDF(cube1)
    ```
  
    rxCube 函数包含一个可选参数，returnDataFrame = TRUE，可使用此参数将结果直接转换成数据框架    。 例如：
    
    `print(rxCube(fraudRisk~F(numTrans):F(numIntlTrans), data = sqlFraudDS, returnDataFrame = TRUE))`
       
    但是，rxResultsDF 的输出更加清晰，并保留了源列的名称  。 可以运行后跟 `head(cubePlot)` 的 `head(cube1)` 来比较输出。
  
3. 使用来自所有 R 分发中包含的 lattice 包的 levelplot 函数创建热度地图   。
  
    ```R
    levelplot(fraudRisk~numTrans*numIntlTrans, data = cubePlot)
    ```
  
    **结果**
  
    ![散点图结果](media/rsql-sue-scatterplotresults.jpg "散点图结果")
  
从此快速分析中可以看出欺诈风险随交易数量和国际交易数量的增加而增高。

有关 rxCube 函数和常规交叉表的详细信息，请参阅[使用 RevoScaleR 的数据摘要](/machine-learning-server/r/how-to-revoscaler-data-summaries)  。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 SQL Server 数据创建 R 模型](../../machine-learning/tutorials/deepdive-create-models.md)