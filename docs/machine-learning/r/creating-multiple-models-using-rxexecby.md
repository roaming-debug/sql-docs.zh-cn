---
title: 使用 rxExecBy 创建多个模型
description: 使用 RevoScaleR 库中的 rxExecBy 函数对存储在 SQL Server 中的计算机数据构建多个迷你模型。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 04/15/2018
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 94f54e84a7f78dd92bacee399415149ca3b08a07
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470898"
---
# <a name="creating-multiple-models-using-rxexecby"></a>使用 rxExecBy 创建多个模型
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

RevoScaleR 中的 rxExecBy 函数支持多个相关模型的并行处理  。 数据科学家可以快速创建许多相关的模型，每个模型都使用特定于单个实体的数据，而不是基于来自多个类似实体的数据定型一个大型模型。 

例如，假设你正在监视设备故障，为许多不同类型的设备捕获数据。 通过使用 rxExecBy，你可以提供单个大型数据集作为输入，指定数据集的分层列（如设备类型），然后为单个设备创建多个模型。

此用例被称为[“易并行”](https://en.wikipedia.org/wiki/Embarrassingly_parallel)，因为它将一个大型复杂的问题分解为多个组件来进行并发处理。

此方法的典型应用包括对单个家庭智能电表进行预测、为单独的产品线创建收入预测，或针对单个银行分行的贷款批准创建模型。

## <a name="how-rxexec-works"></a>RxExec 的工作原理

RevoScaleR 中的 rxExecBy 函数旨在对大量小数据集进行大容量并行处理。

1. 可在 R 代码中调用 rxExecBy 函数，并传递无序数据。
2. 指定对数据进行分组和排序的分区。
3. 定义应该应用于每个数据分区的转换或建模函数
4. 函数执行时，如果你的环境支持，则并行处理数据查询。 此外，建模或转换任务分布在各个核心之间，并并行执行。 支持的计算上下文操作包括 RxSpark 和 RxInSQLServer。
5. 返回多个结果。

## <a name="rxexecby-syntax-and-examples"></a>rxExecBy 语法和示例

rxExecBy 使用四个输入，其中一个输入是数据集或数据源对象，可在指定的“键”列上进行分区   。 该函数返回每个分区的输出。 输出的形式取决于作为参数传递的函数。 例如，如果传递一个建模函数（如 rxLinMod），则可以为数据集的每个分区返回一个单独的已定型模型。

### <a name="supported-functions"></a>支持的函数

建模函数：`rxLinMod`、`rxLogit`、`rxGlm`、`rxDtree`

评分函数：`rxPredict`，

转换或分析函数：`rxCovCor`

## <a name="example"></a>示例

下面的示例演示如何使用 Airline 数据集创建多个模型，该数据集在 [DayOfWeek] 列上进行分区。 用户定义的函数 `delayFunc` 通过调用 rxExecBy 应用于每个分区。 该函数为周一、周二等创建单独的模型。

```sql
EXEC sp_execute_external_script
@language = N'R'
, @script = N'
delayFunc <- function(key, data, params) { 
    df <- rxImport(inData = airlineData) 
    rxLinMod(ArrDelay ~ CRSDepTime, data = df) 
} 
OutputDataSet <- rxExecBy(airlineData, c("DayOfWeek"), delayFunc)
'
, @input_data_1 = N'select ArrDelay, DayOfWeek, CRSDepTime from AirlineDemoSmall]'
, @input_data_1_name = N'airlineData'

```

如果收到错误“`varsToPartition is invalid`”，请检查键列的名称或列名称的输入是否正确。 R 语言区分大小写。

此特例没有针对 SQL Server 进行优化，在许多情况下，使用 SQL 对数据进行分组可以获得更好的性能。 但是，使用 rxExecBy 可以从 R 创建并行作业。

以下示例演示了 R 中使用 SQL Server 作为计算上下文的过程：

```R
sqlServerConnString <- "SERVER=hostname;DATABASE=TestDB;UID=DBUser;PWD=Password;"
inTable <- paste("airlinedemosmall")
sqlServerDataDS <- RxSqlServerData(table = inTable, connectionString = sqlServerConnString)

# user function
".Count" <- function(keys, data, params)
{
  myDF <- rxImport(inData = data)
  return (nrow(myDF))
}

# Set SQL Server compute context with level of parallelism = 2
sqlServerCC <- RxInSqlServer(connectionString = sqlServerConnString, numTasks = 4)
rxSetComputeContext(sqlServerCC)

# Execute rxExecBy in SQL Server compute context
sqlServerCCResults <- rxExecBy(inData = sqlServerDataDS, keys = c("DayOfWeek"), func = .Count)
```


