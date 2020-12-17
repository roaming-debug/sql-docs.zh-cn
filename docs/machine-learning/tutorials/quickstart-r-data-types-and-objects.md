---
title: 快速入门：R 数据结构、数据类型和对象
titleSuffix: SQL machine learning
description: 在本快速入门中，你将了解在通过 SQL 机器学习使用 R 时如何使用数据结构、数据类型和对象。
ms.prod: sql
ms.technology: machine-learning
ms.date: 05/21/2020
ms.topic: quickstart
author: garyericson
ms.author: garye
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e307dcd98396d937b14ffab92a60d4ba9a7280dd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470248"
---
# <a name="quickstart-data-structures-data-types-and-objects-using-r-with-sql-machine-learning"></a>快速入门：通过 SQL 机器学习使用 R 时的数据结构、数据类型和对象
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
本快速入门介绍了在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中或[大数据群集](../../big-data-cluster/machine-learning-services.md)上使用 R 时如何使用数据结构和数据类型。 你将了解如何在 R 与 SQL Server 之间迁移数据，以及可能出现的常见问题。
::: moniker-end
::: moniker range="=sql-server-2017"
本快速入门介绍了在 [SQL Server 机器学习服务](../sql-server-machine-learning-services.md)中使用 R 时如何使用数据结构和数据类型。 你将了解如何在 R 与 SQL Server 之间迁移数据，以及可能出现的常见问题。
::: moniker-end
::: moniker range="=sql-server-2016"
本快速入门介绍了在 [SQL Server R Services](../r/sql-server-r-services.md) 中使用 R 时如何使用数据结构和数据类型。 你将了解如何在 R 与 SQL Server 之间迁移数据，以及可能出现的常见问题。
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
本快速入门介绍在 [Azure SQL 托管实例机器学习服务](/azure/azure-sql/managed-instance/machine-learning-services-overview)中使用 R 时如何使用数据结构和数据类型。 将了解如何在 R 与 SQL 托管实例之间迁移数据，以及可能出现的常见问题。
::: moniker-end

要预先了解的常见问题包括：

- 数据类型有时不匹配
- 可能会发生隐式转换
- 有时需要执行强制转换和转换操作
- R 和 SQL 使用不同的数据对象

## <a name="prerequisites"></a>先决条件

若要运行本快速入门，需要具备以下先决条件。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
- SQL Server 机器学习服务。 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)或 [Linux 安装指南](../../linux/sql-server-linux-setup-machine-learning.md?toc=%2Fsql%2Fmachine-learning%2Ftoc.json)。 还可以[启用 SQL Server 大数据群集上的机器学习服务](../../big-data-cluster/machine-learning-services.md)。
::: moniker-end
::: moniker range="=sql-server-2017"
- SQL Server 机器学习服务。 如需安装机器学习服务，请参阅 [Windows 安装指南](../install/sql-machine-learning-services-windows-install.md)。 
::: moniker-end
::: moniker range="=sql-server-2016"
- SQL Server 2016 R Services。 如需安装 R Services，请参阅 [Windows 安装指南](../install/sql-r-services-windows-install.md)。 
::: moniker-end
::: moniker range="=azuresqldb-mi-current"
- Azure SQL 托管实例机器学习服务。 有关信息，请参阅 [Azure SQL 托管实例机器学习服务概述](/azure/azure-sql/managed-instance/machine-learning-services-overview)。
::: moniker-end

- 一个用于运行包含 R 脚本的 SQL 查询的工具。 本快速入门使用 [Azure Data Studio](../../azure-data-studio/what-is.md)。

## <a name="always-return-a-data-frame"></a>始终返回数据框架

当脚本将结果从 R 返回给 SQL Server 时，必须以 **data.frame** 的形式返回数据。 在脚本中生成的其他任何对象类型（不管是列表、因子、向量还是二进制数据）都必须转换为数据框架，这样才能将它输出为存储过程结果的一部分。 幸运的是，有多个 R 函数支持将其他对象更改为数据框架。 你甚至可以序列化二进制模型并在数据框架中返回该模型。本快速入门稍后将介绍此内容。

首先，让我们体验 R 的一些基本 R 对象（向量、矩阵和列表），并了解转换为数据框架后会对传递给 SQL Server 的输出造成怎样的变化。

比较 R 中的这两个“Hello World”脚本。这两个脚本看上去几乎完全相同，但第一个脚本返回一个包含三个值的列，而第二个脚本则返回三个包含单个值的列。

**示例 1**

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
     , @script = N' mytextvariable <- c("hello", " ", "world");
       OutputDataSet <- as.data.frame(mytextvariable);'
     , @input_data_1 = N' ';
```

**示例 2**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
      , @input_data_1 = N'  ';
```

## <a name="identify-schema-and-data-types"></a>标识架构和数据类型

结果为何有这么大的差别？

使用 R `str()` 命令通常可以找到答案。 在 R 脚本中的任何位置添加函数 `str(object_name)` 可使指定 R 对象的数据架构作为信息性消息返回。

若要找出示例 1 和示例 2 的结果为何有这么大的差别，请在每个语句的 `@script` 变量定义末尾插入 `str(OutputDataSet)` 行，如下所示：

**添加了 str 函数的示例 1**

```sql
EXECUTE sp_execute_external_script
        @language = N'R'
      , @script = N' mytextvariable <- c("hello", " ", "world");
      OutputDataSet <- as.data.frame(mytextvariable);
      str(OutputDataSet);'
      , @input_data_1 = N'  '
;
```

**添加了 str 函数的示例 2**

```sql
EXECUTE sp_execute_external_script
  @language = N'R', 
  @script = N' OutputDataSet <- data.frame(c("hello"), " ", c("world"));
    str(OutputDataSet);' , 
  @input_data_1 = N'  ';
```

现在，查看“消息”中的文本，了解输出为何不相同的原因。

**结果 - 示例 1**

```sql
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**结果 - 示例 2**

```sql
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

可以看到，对 R 语法进行轻微的更改会给结果的架构造成很大的影响。 本文不会深究原因，但 [Hadley Wickham 所著的“Advanced R”](http://adv-r.had.co.nz)中的“Data Structures”（数据结构）一节详细介绍了 R 数据类型的差异。

暂时，你只需在将 R 对象强制转换成数据框架时注意检查预期的结果。

> [!TIP]
> 你还可以使用 `is.matrix`、`is.vector` 等 R 标识函数返回有关内部数据结构的信息。

## <a name="implicit-conversion-of-data-objects"></a>数据对象的隐式转换

每个 R 数据对象都有自己的规则，指定在将值与其他数据对象组合时，如果两个数据对象的维度数相同，或如果任意数据对象包含异类数据类型，应如何处理值。

首先，创建一个小型测试数据表。

```sql
CREATE TABLE RTestData (col1 INT NOT NULL)

INSERT INTO RTestData
VALUES (1);

INSERT INTO RTestData
VALUES (10);

INSERT INTO RTestData
VALUES (100);
GO
```

例如，假设你要运行以下语句来使用 R 执行矩阵乘法。将包含三个值的单列矩阵乘以包含四个值的数组，预期结果为 4x3 矩阵。

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:15);
    OutputDataSet <- as.data.frame(x %*% y);'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (([Col1] int, [Col2] int, [Col3] int, Col4 int));
```

在幕后，包含三个值的列将转换为单列矩阵。 由于矩阵只是 R 中数组的特殊表示形式，数组 `y` 将隐式强制转换为单列矩阵，使这两个参数相符。

**结果**

|Col1|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1500|

但是，请注意更改数组 `y` 的大小时会发生什么情况。

```sql
execute sp_execute_external_script
   @language = N'R'
   , @script = N'
        x <- as.matrix(InputDataSet);
        y <- array(12:14);
   OutputDataSet <- as.data.frame(y %*% x);'
   , @input_data_1 = N' SELECT [Col1]  from RTestData;'
   WITH RESULT SETS (([Col1] int ));
```

现在，R 返回单个值作为结果。

**结果**

|Col1|
|---|
|1542|

为什么？ 在本例中，由于可以将这两个参数作为长度相同的向量进行处理，R 会以矩阵形式返回内积。  根据线性代数的规则，这种行为符合预期；但是，如果下游应用程序预期输出架构永远不变，则这种行为可能会导致问题！

> [!TIP]
>
> 出现错误？ 确保在包含表的数据库的上下文中运行存储过程，而不是在 master 或其他数据库中运行。
>
> 此外，建议避免在这些示例中使用临时表。 某些 R 客户端将停止批次之间的连接，从而删除临时表。

## <a name="merge-or-multiply-columns-of-different-length"></a>对不同长度的列进行合并或相乘

在处理不同大小的向量以及将这些类似于列的结构组合成数据框架方面，R 提供了很大的弹性。 向量列表可能看起来像表，但它们并不遵循控制数据库表的所有规则。

例如，以下脚本定义长度为 6 的数字数组，并将其存储在 R 变量 `df1` 中。 然后将该数值数组与包含 3 个值的 RTestData 表的整数相组合，从而构成新的数据框架 `df2`。

```sql
EXECUTE sp_execute_external_script
    @language = N'R'
    , @script = N'
               df1 <- as.data.frame( array(1:6) );
               df2 <- as.data.frame( c( InputDataSet , df1 ));
               OutputDataSet <- df2'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
    WITH RESULT SETS (( [Col2] int not null, [Col3] int not null ));
```

为了填充数据框架，R 将以所需的次数重复从 RTestData 检索的元素，以匹配数组 `df1` 中的元素数目。

**结果**

|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

请记住，数据框架只是看起来像表，但实际上是向量列表。

## <a name="cast-or-convert-data"></a>强制转换或转换数据

R 和 SQL Server 使用的数据类型不同，因此，如果你在 SQL Server 中运行查询来获取数据，然后将该数据传递给 R 运行时，通常会发生某种类型的隐式转换。 将数据从 R 返回到 SQL Server 时，会发生另一套转换。

- SQL Server 将数据从查询推送到由 Launchpad 服务管理的 R 进程，并将其转换为内部表示形式，从而提高效率。
- R 运行时将数据加载到 data.frame 变量中，并对数据执行其自身的操作。
- 数据库引擎使用受保护的内部连接将数据返回到 SQL Server，并以 SQL Server 数据类型呈现数据。
- 若要获取数据，可以使用能够发出 SQL 查询并处理表格数据集的客户端或网络库连接到 SQL Server。 此客户端应用程序可能会通过其他方式影响数据。

若要了解这种处理方式的工作原理，请针对 [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) 数据仓库运行如下所示的查询。 此视图返回用于创建预测的销售数据。

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = 'M200 Europe'
           ORDER BY ReportingDate ASC
```

> [!NOTE]
>
> 可以使用任何版本的 AdventureWorks，也可以使用自己的数据库创建不同的查询。 要点在于尝试处理一些包含文本、日期时间和数值的数据。

现在，尝试将此查询作为输入粘贴到存储过程。

```sql
EXECUTE sp_execute_external_script
       @language = N'R'
      , @script = N' str(InputDataSet);
      OutputDataSet <- InputDataSet;'
      , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

如果遇到错误，可能需要对查询文本进行一些编辑。 例如，必须用两组单引号将 WHERE 子句中的字符串谓词括起来。

正常运行查询后，请查看 `str` 函数的结果，了解 R 如何处理输入数据。

**结果**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- 日期时间列已使用 R 数据类型 **POSIXct** 进行处理。
- 文本列“ProductSeries”已标识为因子，意味着它是一个分类变量。 默认情况下，字符串值将作为因子处理。 如果将某个字符串传递给 R，该字符串将转换为整数供内部使用，然后映射回到输出中的字符串。

### <a name="summary"></a>总结

即使是这些简短的示例，你也可以看出将 SQL 查询作为输入进行传递时需要检查数据转换的影响。 由于 R 不支持某些 SQL Server 数据类型，因此为了避免出错，请考虑以下方式：

- 在将数据传递给 R 代码之前，请预先测试数据并验证架构中可能会造成问题的列或值。
- 在输入数据源中单独指定列而不要使用 `SELECT *`，并知道如何处理每个列。
- 准备输入数据时请根据需要执行显式强制转换，避免出现意外。
- 避免传递导致错误以及对建模无用的数据列（例如 GUID 或 rowguid）。

若要详细了解受支持和不受支持的数据类型，请参阅 [R 库和数据类型](../r/r-libraries-and-data-types.md)。

## <a name="next-steps"></a>后续步骤

若要了解如何通过 SQL 机器学习编写高级 R 函数，请参阅以下快速入门：

> [!div class="nextstepaction"]
> [通过 SQL 机器学习编写高级 R 函数](quickstart-r-functions.md)
