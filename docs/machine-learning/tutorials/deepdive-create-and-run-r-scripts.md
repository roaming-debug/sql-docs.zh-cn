---
title: RevoScaleR 中的摘要统计信息
description: RevoScaleR 教程 5：如何在 SQL Server 中使用 R 语言计算统计摘要统计信息。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: bb362f078dd4fbebedf88dc41e997a5feb702f55
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470638"
---
# <a name="compute-summary-statistics-in-r-sql-server-and-revoscaler-tutorial"></a>计算 R 中的摘要统计信息（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)的 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 5 个教程。

本教程使用已建立的数据源和在前面课程中创建的计算上下文来运行高性能 R 脚本。 本教程使用本地和远程服务器计算上下文来执行以下任务：

> [!div class="checklist"]
> * 将计算上下文切换到 SQL Server
> * 获取有关远程数据对象的摘要统计信息
> * 计算本地摘要

如果已完成前面的教程后，应有以下远程计算上下文：sqlCompute 和 sqlComputeTrace。 接下来，你将在后续教程中使用 sqlCompute 和本地计算上下文。

在本教程中，使用 R IDE 或 Rgui  运行 R 脚本。

## <a name="compute-summary-statistics-on-remote-data"></a>计算有关远程数据的摘要统计信息

需要指定远程计算上下文，然后才能远程运行任何 R 代码。 所有后续计算都在 sqlCompute 参数中指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机上进行  。

在更改计算上下文前，它将保持活动状态。 但是，任何无法在远程服务器上下文中运行的 R 脚本都将自动在本地运行  。

要查看计算上下文的工作原理，请在远程 SQL Server 上的 sqlFraudDS 数据源上生成摘要统计信息。 此数据源对象是在[教程 2](deepdive-create-sql-server-data-objects-using-rxsqlserverdata.md) 中创建的，表示 RevoDeepDive 数据库中的 ccFraudSmall 表。 

1. 将计算上下文切换为在前面教程中创建的 sqlCompute：
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```

2. 调用 [rxSummary](/machine-learning-server/r-reference/revoscaler/rxsummary) 函数并传递所需的参数（如公式和数据源），并将结果分配给变量 `sumOut`。
  
    ```R
    sumOut <- rxSummary(formula = ~gender + balance + numTrans + numIntlTrans + creditLine, data = sqlFraudDS)
    ```
  
    R 语言提供了许多 summary 函数，但 RevoScaleR 中的 rxSummary 支持在各种远程计算上下文上执行，包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]   。 有关类似函数的信息，请参阅[使用 RevoScaleR 的数据摘要](/machine-learning-server/r/how-to-revoscaler-data-summaries)。
  
3. 将 sumOut 的内容打印至控制台。
  
    ```R
    sumOut
    ```
    > [!NOTE]
    > 如果遇到错误，请等待几分钟，等待执行完成后再重试命令。

**结果**

```R
Summary Statistics Results for: ~gender + balance + numTrans + numIntlTrans + creditLine
Data: sqlFraudDS (RxSqlServerData Data Source)
Number of valid observations: 10000

 Name  Mean    StdDev  Min Max ValidObs    MissingObs
 balance       4075.0318 3926.558714            0   25626 100000
 numTrans        29.1061   26.619923 0     100 10000    0           100000
 numIntlTrans     4.0868    8.726757 0      60 10000    0           100000
 creditLine       9.1856    9.870364 1      75 10000    0          100000
 
 Category Counts for gender
 Number of categories: 2
 Number of valid observations: 10000
 Number of missing observations: 0

 gender Counts
  Male   6154
  Female 3846
```

## <a name="create-a-local-summary"></a>创建本地摘要

1. 更改计算上下文，以在本地完成所有工作。
  
    ```R
    rxSetComputeContext ("local")
    ```
  
2. 在从 SQL Server 提取数据时，通常可以通过增加为每次读取提取的行数来获得更好的性能（假定内存中可以容纳增加的块大小）。 运行以下命令以增加数据源上 rowsPerRead 参数的值  。 之前，rowsPerRead  的值设置为 5000。
  
    ```R
    sqlServerDS1 <- RxSqlServerData(
       connectionString = sqlConnString,
       table = sqlFraudTable,
       colInfo = ccColInfo,
       rowsPerRead = 10000)
    ```

3. 调用新数据源上的 rxSummary  。
  
    ```R
    rxSummary(formula = ~gender + balance + numTrans + numIntlTrans + creditLine, data = sqlServerDS1)
    ```
  
   实际结果应与在 **计算机的上下文中运行** rxSummary [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的结果相同。 但是，该操作可能更快，也可能更慢。 因为正在将数据传输到本地计算机进行分析，因此操作很大程度取决于数据库的连接。

4. 切换回远程计算上下文，以完成后续几个教程。

    ```R
    rxSetComputeContext(sqlCompute)
    ```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 R 可视化 SQL Server 数据](../../machine-learning/tutorials/deepdive-visualize-sql-server-data-using-r.md)