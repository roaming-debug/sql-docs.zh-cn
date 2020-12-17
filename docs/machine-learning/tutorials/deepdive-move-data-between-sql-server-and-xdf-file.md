---
title: 使用 XDF 文件移动数据
description: 使用 XDF 文件在远程和本地计算上下文之间传输数据。 将数据存储在 XDF 文件中，则可以对数据执行转换。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 99fad5d6f8ec325e51eb898e34400c1ce8bbaea8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470538"
---
# <a name="move-data-between-sql-server-and-xdf-file-sql-server-and-revoscaler-tutorial"></a>在 SQL Server 和 XDF 文件之间移动数据（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)的 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 13 个教程。

在此教程中，你将学习如何使用 XDF 文件在远程和本地计算上下文之间传输数据。 将数据存储在 XDF 文件中，则可以对数据执行转换。

完成后，使用该文件中的数据创建新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表。 函数 [rxDataStep](/machine-learning-server/r-reference/revoscaler/rxdatastep) 可以将转换应用于数据，并执行数据帧和 .xdf 文件之间的转换。
  
## <a name="create-a-sql-server-table-from-an-xdf-file"></a>从 XDF 文件创建 SQL Server 表

在本练习中，会再次使用信用卡欺诈数据。 在此方案中，要求对加利福尼亚州、俄勒冈州和华盛顿州的用户执行一些额外分析。 为了提高效率，决定在本地计算机上只存储这些州的数据并只处理 gender、ardholder、state 和 balance 这几个变量。

1. 再次使用之前创建的 `stateAbb` 变量以确定要包括的级别，并将其写入新变量 `statesToKeep`。
  
    ```R
    statesToKeep <- sapply(c("CA", "OR", "WA"), grep, stateAbb)
    statesToKeep
    ```
    **结果**
    
    CA|OR|WA
    ----|----|----
    5|38|48
    
2. 使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查询定义想要从 SQL Server 导入的数据。  稍后，将此变量用作 rxImport 的 inData 参数。
  
    ```R
    importQuery <- paste("SELECT gender,cardholder,balance,state FROM",  sqlFraudTable,  "WHERE (state = 5 OR state = 38 OR state = 48)")
    ```
  
    确保没有隐藏字符，如换行符或查询中的选项卡。
  
3. 接下来，定义在 R 中处理数据时要使用的列。例如，在较小的数据集中，只需三个因素级别，因为该查询只返回三个州的数据。  应用 `statesToKeep` 变量以确定要包括的正确级别。
  
    ```R
    importColInfo <- list(
        gender = list( type = "factor",  levels = c("1", "2"), newLevels = c("Male", "Female")),
        cardholder = list(  type = "factor",  levels = c("1", "2"), newLevels = c("Principal", "Secondary")),
        state = list(   type = "factor",  levels = as.character(statesToKeep), newLevels = names(statesToKeep))
            )
    ```
  
4. 将计算上下文设置为本地，因为需在本地计算机上使用所有数据。
  
    ```R
    rxSetComputeContext("local")
    ```
    
    [rxImport](/machine-learning-server/r-reference/revoscaler/rxsqlserverdata) 函数可以将数据从任何受支持的数据源导入到本地 XDF 文件。 如果想要对数据进行多种不同的分析，但避免反复运行相同的查询，则使用数据的本地副本会非常方便。

5. 通过将之前定义为参数的变量传递给 RxSqlServerData，创建数据源对象。
  
    ```R
    sqlServerImportDS <- RxSqlServerData(
        connectionString = sqlConnString,
        sqlQuery = importQuery,
        colInfo = importColInfo)
    ```
  
6. 调用 rxImport，将数据写入当前工作目录中一个名为 `ccFraudSub.xdf` 的文件。
  
    ```R
    localDS <- rxImport(inData = sqlServerImportDS,
        outFile = "ccFraudSub.xdf",
        overwrite = TRUE)
    ```
  
    rxImport 函数返回的 `localDs` 对象为轻量 RxXdfData 数据源对象，表示存储在本地磁盘上的 `ccFraud.xdf` 数据文件。
  
7. 对 XDF 文件调用 [rxGetVarInfo](/machine-learning-server/r-reference/revoscaler/rxgetvarinfoxdf) 以验证数据架构是否相同。
  
    ```R
    rxGetVarInfo(data = localDS)
    ```

    **结果**
    
    ```R
    rxGetVarInfo(data = localDS)
    Var 1: gender, Type: factor, no factor levels available
    Var 2: cardholder, Type: factor, no factor levels available
    Var 3: balance, Type: integer, Low/High: (0, 22463)
    Var 4: state, Type: factor, no factor levels available
    ```

8. 现在，可以调用各种不同的 R 函数以分析 localDs 对象，正如对 SQL Server 上的源数据执行的操作一样。 例如，可以按 gender 进行汇总：
  
    ```R
    rxSummary(~gender + cardholder + balance + state, data = localDS)
    ```

## <a name="next-steps"></a>后续步骤

本教程为介绍 RevoScaleR 和 SQL Server 的多部分系列教程画上了句号。 它介绍了许多与数据相关的计算概念，为你处理自己的数据和项目需求打下了基础。

若要加深对 RevoScaleR 的了解，可返回到 R 教程列表，逐步完成你可能错过的任何练习。 或者，查看目录中的操作方法文章，获取有关一般任务的信息。

> [!div class="nextstepaction"]
> [SQL Server 的 R 教程](./r-tutorials.md)