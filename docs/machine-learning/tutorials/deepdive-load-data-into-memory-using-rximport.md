---
title: 使用 rxImport 加载数据
description: 了解如何从 SQL Server 获取数据，然后使用 rxImport 函数将相关数据放入本地文件中。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 1eb487a19e911808d11de0c4f56c06af2b30c0cb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470528"
---
# <a name="load-data-into-memory-using-rximport-sql-server-and-revoscaler-tutorial"></a>使用 rxImport 将数据加载到内存中（SQL Server 和 RevoScaleR 教程）
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

这是 [RevoScaleR 教程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的第 10 个教程，RevoScaleR 教程介绍如何在 SQL Server 中使用 [RevoScaleR 函数](/machine-learning-server/r-reference/revoscaler/revoscaler)。

在本教程中，将学习如何从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 获取数据，然后使用 rxImport  函数将感兴趣的数据放入本地文件中。 这样一来，就可以在本地计算上下文中重复对数据进行分析，而无需重新查询数据库。

[rxImport](/machine-learning-server/r-reference/revoscaler/rximport) 函数可用于将数据从数据源移到会话内存中的数据帧或磁盘中的 XDF 文件。 如果未指定某个文件作为目标，数据会作为数据框放入内存中。

## <a name="extract-a-subset-of-data-from-sql-server-to-local-memory"></a>从 SQL Server 将数据子集提取到本地内存

你已决定仅详细检查高风险个体。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的源表很大，因此，你只想获取有关高风险客户的信息。 随后，你将该数据加载到本地工作站内存中的数据帧。

1. 将计算上下文重置为本地工作站。

    ```R
    rxSetComputeContext("local")
    ```

2. 如果 sqlQuery  参数中存在效的 SQL 语句，则创建新的 SQL Server 数据源对象。 此示例获取具有最高风险评分的观测值的子集。 这样一来，只有真正需要的数据才会放入本地内存。

    ```R
    sqlServerProbDS \<- RxSqlServerData(
        sqlQuery = paste("SELECT * FROM ccScoreOutput2",
        "WHERE (ccFraudProb > .99)"),
        connectionString = sqlConnString)
    ```

3. 调用函数 [rxImport](/machine-learning-server/r-reference/revoscaler/rximport) 可将数据读取到本地 R 会话中的数据帧。

    ```R
    highRisk <- rxImport(sqlServerProbDS)
    ```

    如果操作成功，应该会看到如下所示的状态消息：“读取的行数：35，处理的总行数：35，总区块时间：0.036 秒”

4. 现在，高风险观察数据已位于内存中数据帧内，可使用各种 R 函数来操纵该数据帧。 例如，可根据风险评分对客户排序，并打印存在最高风险的客户的列表。

    ```R
    orderedHighRisk <- highRisk[order(-highRisk$ccFraudProb),]
    row.names(orderedHighRisk) <- NULL
    head(orderedHighRisk)
    ```

**结果**

```R
ccFraudLogitScore   state gender cardholder balance numTrans numIntlTrans creditLine ccFraudProb1
9.786345    SD   Male  Principal   23456       25            5 75   0.99994382
9.433040    FL Female  Principal   20629       24           28 75   0.99992003
8.556785    NY Female  Principal   19064       82           53 43   0.99980784
8.188668    AZ Female  Principal   19948       29            0 75   0.99972235
7.551699    NY Female  Principal   11051       95            0 75   0.99947516
7.335080    NV   Male  Principal   21566        4            6  75   0.9993482
```

## <a name="more-about-rximport"></a>详细了解 rxImport

不仅可以使用 rxImport  来移动数据，还可在读取它的过程中转换数据。 例如，可以为固定宽度的列指定字符数，提供变量的说明，设置因子列的级别，甚至还能创建可在导入后使用的新级别。

**rxImport** 函数在导入过程中将变量名称分配到列，但可以使用 *colInfo* 参数指示新变量名称，或使用 *colClasses* 参数更改数据类型。

通过在 *transforms* 参数中指定其他操作，可以对读取的每个数据区块执行基本处理。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 rxDataStep 创建新的 SQL Server 表](../../machine-learning/tutorials/deepdive-move-data-between-sql-server-and-xdf-file.md)