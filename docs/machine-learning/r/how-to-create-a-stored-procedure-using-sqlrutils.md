---
title: 使用 sqlrutils 包从捆绑的 R 代码创建函数
description: 在 SQL Server 中使用 sqlrutils R 包将 R 语言代码捆绑到可作为参数传递给存储过程的单个函数。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 04bcac677a4cdbf3e96f60bb43244f2cc6a0eec7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336272"
---
# <a name="create-a-stored-procedure-using-sqlrutils"></a>使用 sqlrutils 创建存储过程
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

本文介绍了转换 R 代码以作为 T-SQL 存储过程运行的步骤。 为了获得最佳的可能结果，可能需要对代码进行某种程度的修改，以确保所有输入可参数化。

## <a name="step-1-rewrite-r-script"></a><a name="bkmk_rewrite"></a>步骤 1. 重写 R 脚本

为获得最佳结果，应重写 R 代码，以将其封装为单个函数。

所有该函数使用的变量都应在函数内进行定义，或应定义为输入参数。 请参阅本文中的[示例代码](#samples)。

此外，由于 R 函数的输入参数将成为 SQL 存储过程的输入参数，因此，必须确保输入和输出符合以下类型要求：

### <a name="inputs"></a>输入

输入参数中最多可有一个数据帧。

数据帧内的对象以及函数的其他输入参数必须是以下 R 数据类型之一：
- POSIXct
- numeric
- character
- integer
- 逻辑
- raw

如果输入类型不是上述类型之一，则需进行序列化并作为 *raw* 传入函数。 在这种情况下，该函数还必须包括要反序列化输入的代码。

### <a name="outputs"></a>Outputs

该函数可输出以下项之一：

- 包含受支持数据类型的数据帧。 数据帧中所有对象必须使用受支持数据类型之一。
- 包含最多一个数据帧的命名列表。 列表的所有成员应该使用受支持数据类型之一。
- 如果函数没有返回任何结果，则为 NULL

## <a name="step-2-generate-required-objects"></a>步骤 2. 生成所需的对象

在清理 R 代码并且可将该代码作为单个函数进行调用后，使用 sqlrutils 包中的函数来准备可传递给实际构建存储过程的构造函数的输入和输出  。

sqlrutils 提供定义输入和输出数据架构和类型的函数  。 它还包括可将 R 对象转换为所需输出类型的函数。 根据代码使用的数据类型，可以执行多个函数调用来创建所需对象。

### <a name="inputs"></a>输入

如果函数使用输入，则为每个输入调用以下函数：

- 如果输入为数据帧，则调用 `setInputData`
- 对于所有其他输入类型，调用 `setInputParameter`

在执行每个函数调用时，系统将创建一个 R 对象，稍后你将该对象作为参数传递到 `StoredProcedure` 以创建完整的存储过程。

### <a name="outputs"></a>Outputs

sqlrutils 提供多种函数，用于将 R 对象（如列表）转换为 SQL Server 所需的数据帧  。
如果函数直接输出数据帧，并未先将其包装到列表中，则可跳过此步骤。
如果函数返回 NULL，则还可以跳过此步骤的转换。

在转换列表或从列表中获取特定项时，请从以下函数中进行选择：

- 如果要从列表中获取的变量是数据帧，请选择 `setOutputData`
- 对于列表的所有其他成员，请选择 `setOutputParameter`

在执行每个函数调用时，系统将创建一个 R 对象，稍后你将该对象作为参数传递到 `StoredProcedure` 以创建完整的存储过程。

## <a name="step-3-generate-the-stored-procedure"></a>步骤 3. 生成存储过程

当所有输入和输出参数都已准备就绪时，调用 `StoredProcedure` 构造函数。

**使用情况**

`StoredProcedure (func, spName, ..., filePath = NULL ,dbName = NULL, connectionString = NULL, batchSeparator = "GO")`

为进行说明，假设要使用这些参数创建一个名为 sp_rsample 的存储过程  ：

- 使用现有函数 foosql  。 此函数基于 R 函数 foo 中的现有代码，但你已重写该函数以符合[本部分](#bkmk_rewrite)中所述的要求，并将更新后的函数命名为 foosql   。
- 使用数据帧 queryinput 作为输入 
- 生成一个 R 变量名为 sqloutput  的数据帧作为输出
- 你想要创建 T-SQL 代码作为 `C:\Temp` 文件夹中的文件，以便稍后可以使用 SQL Server Management Studio 来运行该代码

```R
StoredProcedure (foosql, sp_rsample, queryinput, sqloutput, filePath = "C:\\Temp")
```

> [!NOTE]
> 由于要将文件写入文件系统，因此可以省略定义数据库连接的参数。

函数的输出是一个 T-SQL 存储过程，可针对 SQL Server 2016（需要 R 服务）或 SQL Server 2017（需要具有 R 的机器学习服务）的实例执行。 

有关其他示例，请从 R 环境调用 `help(StoredProcedure)` 来查看包帮助。

## <a name="step-4-register-and-run-the-stored-procedure"></a>步骤 4. 注册并运行存储过程

可以通过两种方式来运行存储过程：

- 从支持连接到 SQL Server 2016 或 SQL Server 2017 实例的任何客户端中，使用 T-SQL 进行运行
- 从 R 环境中运行

这两种方法都要求将存储过程注册到你打算使用它的数据库中。

### <a name="register-the-stored-procedure"></a>注册存储过程

可以使用 R 来注册存储过程，也可以在 T-SQL 中运行 CREATE PROCEDURE 语句。

- 使用 T-SQL。  如果你更喜欢使用 T-SQL，请打开 SQL Server Management Studio（或任何其他可运行 SQL DDL 命令的客户端），然后使用 `StoredProcedure` 函数准备的代码执行 CREATE PROCEDURE 语句。
- 使用 R。虽然仍处于 R 环境中，但可以使用 sqlrutils 中的 `registerStoredProcedure` 函数向数据库注册存储过程  。

  例如，可以通过进行以下 R 调用，在 sqlConnStr 中定义的实例和数据库中注册存储过程 sp_rsample   ：

  ```R
  registerStoredProcedure(sp_rsample, sqlConnStr)
  ```


> [!IMPORTANT]
> 无论是使用 R 还是 SQL，都必须使用有权创建新数据库对象的帐户运行该语句。

### <a name="run-using-sql"></a>使用 SQL 运行

创建存储过程后，使用支持 T-SQL 的任何客户端打开到 SQL 数据库的连接，并传递存储过程所需的任何参数的值。

### <a name="run-using-r"></a>使用 R 运行

如果要通过 R 代码而不是 SQL Server 执行存储过程，则需要进行一些额外的准备工作。 例如，如果存储过程需要输入值，则必须先设置这些输入参数，然后才能执行函数，然后将这些对象传递到 R 代码中的存储过程。

调用已准备的 SQL 存储过程的整个过程如下所示：

1. 调用 `getInputParameters` 以获取输入参数对象的列表。
2. 定义 `$query` 或为每个输入参数设置 `$value` 。
3. 使用 `executeStoredProcedure` 从 R 开发环境执行该存储过程，传递设置的输入参数对象列表。

## <a name="example"></a><a name = "samples"></a>示例

此示例显示了从 SQL Server 数据库获取数据、对数据执行一些转换并将其保存到不同数据库的 R 脚本的前后版本。

此简单示例仅用于演示如何重新排列 R 代码，以便更轻松地转换为存储过程。

### <a name="before-code-preparation"></a>准备代码之前


```R
sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineSrc;Trusted_Connection=Yes;"
  
sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=AirlineTest;Trusted_Connection=Yes;"
  
sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"
  
dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
xFunc <- function(data) {
    data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
    return(data)
    }
  
xVars <- c("CRSDepTime")
  
sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
rxOpen(dsSqlFrom)
rxOpen(dsSqlTo)
  
if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo))   {
    rxSqlServerDropTable("cleanData")}
  
rxDataStep(inData = dsSqlFrom, 
     outFile = dsSqlTo,
     transformFunc = xFunc,
     transformVars = xVars,
     overwrite = TRUE)
```

> [!NOTE]
> 
> 如果使用 ODBC 连接而不是调用 RxSqlServerData 函数，则必须使用 rxOpen 打开连接，然后才能在数据库上执行操作   。


### <a name="after-code-preparation"></a>准备代码之后

在更新后的版本中，第一行定义函数名称。 原始 R 解决方案中的所有其他代码将成为该函数的一部分。

```R
myetl1function <- function() { 
   sqlConnFrom <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer01;Database=Airline01;Trusted_Connection=Yes;"
   sqlConnTo <- "Driver={ODBC Driver 13 for SQL Server};Server=MyServer02;Database=Airline02;Trusted_Connection=Yes;"
    
   sqlQueryAirline <- "SELECT TOP 10000 ArrDelay, CRSDepTime, DayOfWeek FROM [AirlineDemoSmall]"

   dsSqlFrom <- RxSqlServerData(sqlQuery = sqlQueryAirline, connectionString = sqlConnFrom)
  
   dsSqlTo <- RxSqlServerData(table = "cleanData", connectionString = sqlConnTo)
  
   xFunc <- function(data) {
     data$CRSDepHour <- as.integer(trunc(data$CRSDepTime))
     return(data)}
  
   xVars <- c("CRSDepTime")
  
   sqlCompute <- RxInSqlServer(numTasks = 4, connectionString = sqlConnTo)
  
   if (rxSqlServerTableExists("cleanData", connectionString = sqlConnTo)) {rxSqlServerDropTable("cleanData")}
  
   rxDataStep(inData = dsSqlFrom, 
        outFile = dsSqlTo,
        transformFunc = xFunc,
        transformVars = xVars,
        overwrite = TRUE)
   return(NULL)
}
```

> [!NOTE]
> 
> 尽管不需要显式打开 ODBC 连接作为代码的一部分，但 ODBC 连接仍需使用 **sqlrutils**。

## <a name="see-also"></a>另请参阅

[sqlrutils (SQL)](ref-r-sqlrutils.md)


