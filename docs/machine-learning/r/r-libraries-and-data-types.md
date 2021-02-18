---
title: 转换 R 和 SQL 数据类型
description: 查看数据科学和机器学习解决方案中 R 和 SQL Server 之间的隐式和显式数据类型转换。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/06/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: e024d65ed8a2dd9b55b7940fa19ed400a01af175
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348121"
---
# <a name="data-type-mappings-between-r-and-sql-server"></a>R 与 SQL Server 之间的数据类型映射
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

本文列出了在 SQL Server 机器学习服务中使用 R 集成功能时所支持的数据类型以及所执行的数据类型转换。

## <a name="base-r-version"></a>基本 R 版本

SQL Server 2016 R Services 和带有 R 的 SQL Server 机器学习服务与 Microsoft R Open 的特定版本保持一致。 例如，最新版本 SQL Server 2019 机器学习服务是基于 Microsoft R Open 3.5.2 构建的。

若要查看与 SQL Server 的特定实例关联的 R 版本，请打开 SQL 实例中的 RGui。 例如，SQL Server 2019 中默认实例的路径为：`C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\R_SERVICES\bin\x64\Rgui.exe`。

此工具将加载基本 R 和其他库。 会话启动时加载的每个包的通知中都提供了包版本信息。

## <a name="r-and-sql-data-types"></a>R 和 SQL 数据类型

尽管 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持几十种数据类型，但 R 的标量数据类型（数字、整数、复杂、逻辑、字符、日期/时间和原始）则有数量限制。 因此，每当在 R 脚本中使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的数据时，数据都可能隐式转换为兼容的数据类型。 但是，通常无法自动执行精确的转换并将返回错误，如“SQL 数据类型未处理”。

本节列出了提供的隐式转换，并列出了不受支持的数据类型。 提供了一些有关在 R 和 SQL Server 之间映射数据类型的指南。

## <a name="implicit-data-type-conversions"></a>隐式数据类型转换

下表显示了当来自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据在 R 脚本中使用，然后返回到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]时，数据类型和值中的变化。

| SQL 类型                         | R 类     | RESULT SET 类型    | 注释            |
|----------------------------------|-------------|--------------------|---------------------|
| **bigint**                       | `numeric`   | **float**          | 使用 `sp_execute_external_script` 执行 R 脚本支持将 bigint 数据类型作为输入数据。 但是，因为它们会转换为 R 的数值类型，所以会造成精度损失，其值非常高或具有小数点值。 R 仅支持最多 53 位整数，如果位数更高将开始有精度损失。                                                                                         |
| **binary(n)**<br /> n <= 8000    | `raw`       | **varbinary(max)** | 仅允许作为输入参数和输出 |
| **bit**                          | `logical`   | **bit**            |                     |
| **char(n)**<br /> n <= 8000      | `character` | **varchar(max)**   | 输入数据帧 (input_data_1) 是在未明确设置 stringsAsFactors 参数的情况下创建的，因此列类型取决于 R 中的 default.stringsAsFactors()                                                                                                                                                                                                                                            |
| **datetime**                     | `POSIXct`   | **datetime**       | 表示为 GMT  |
| **date**                         | `POSIXct`   | **datetime**       | 表示为 GMT  |
| **decimal(p,s)**                 | `numeric`   | **float**          | 使用 `sp_execute_external_script` 执行 R 脚本支持将 decimal 数据类型作为输入数据。 但是，因为它们会转换为 R 的数值类型，所以会造成精度损失，其值非常高或具有小数点值。 带有 R 脚本的 `sp_execute_external_script` 不支持数据类型的完整范围，它会更改最后几个小数位，尤其是小数部分。 |
| **float**                        | `numeric`   | **float**          |                     |
| **int**                          | `integer`   | **int**            |                     |
| **money**                        | `numeric`   | **float**          | 使用 `sp_execute_external_script` 执行 R 脚本支持将 money 数据类型作为输入数据。 但是，因为它们会转换为 R 的数值类型，所以会造成精度损失，其值非常高或具有小数点值。 有时，美分值会不精确，将发出警告：“警告：无法精确表示的美分值”。                                               |
| **numeric(p,s)**                 | `numeric`   | **float**          | 使用 `sp_execute_external_script` 执行 R 脚本支持将 numeric 数据类型作为输入数据。 但是，因为它们会转换为 R 的数值类型，所以会造成精度损失，其值非常高或具有小数点值。 带有 R 脚本的 `sp_execute_external_script` 不支持数据类型的完整范围，它会更改最后几个小数位，尤其是小数部分。 |
| **real**                         | `numeric`   | **float**          |                     |
| **smalldatetime**                | `POSIXct`   | **datetime**       | 表示为 GMT  |
| **smallint**                     | `integer`   | **int**            |                     |
| **smallmoney**                   | `numeric`   | **float**          |                     |
| **tinyint**                      | `integer`   | **int**            |                     |
| **uniqueidentifier**             | `character` | **varchar(max)**   |                     |
| **varbinary(n)**<br /> n <= 8000 | `raw`       | **varbinary(max)** | 仅允许作为输入参数和输出 |
| **varbinary(max)**               | `raw`       | **varbinary(max)** | 仅允许作为输入参数和输出 |
| **varchar(n)**<br /> n <= 8000   | `character` | **varchar(max)**   | 输入数据帧 (input_data_1) 是在未明确设置 stringsAsFactors 参数的情况下创建的，因此列类型取决于 R 中的 default.stringsAsFactors()                                                                                                                                                                                                                                            |

## <a name="data-types-not-supported-by-r"></a>R 不支持的数据类型

在 [SQL Server 类型系统](../../t-sql/data-types/data-types-transact-sql.md)支持的数据类型类别中，如果将以下类型传递给 R 代码，则有可能会造成问题：

+ SQL 类型系统文章的“其他”部分中列出的数据类型包括：cursor、timestamp、hierarchyid、uniqueidentifier、sql_variant、xml、table        
+ 所有空间类型
+ **图像**

## <a name="data-types-that-might-convert-poorly"></a>转换结果可能不佳的数据类型

+ 除 datetimeoffset 以外的大多数 datetime 类型应可正常转换。
+ 支持大部分数字数据类型，但“money”和“smallmoney”的转换可能会失败 。
+ 支持 **varchar**，但由于 SQL Server 往往使用 Unicode，因此，我们建议尽量使用 **nvarchar** 和其他 Unicode 文本数据类型。
+ RevoScaleR 库中带有 rx 前缀的函数可以处理 SQL 二进制数据类型（**binary** 和 **varbinary**），但大多数情况下，需要对这些类型进行特殊处理。 大多数 R 代码无法处理二进制列。

  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型的详细信息，请参阅[数据类型 &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)。

## <a name="changes-in-data-types-between-sql-server-versions"></a>SQL Server 版本之间数据类型的变化

Microsoft SQL Server 2016 及更高版本对数据类型转换和其他一些操作进行了改进。 其中的大多数改进都提高了处理浮点类型时的精度，同时对经典 **datetime** 类型的操作做了轻微的改变。

使用 130 或更高的数据库兼容级别时，默认会使用这些改进。 但是，如果使用不同的兼容级别，或者使用旧版本连接到数据库，可能会看到数字精度的变化或其他结果。 

有关详细信息，请参阅 [SQL Server 2016 improvements in handling some data types and uncommon operations](https://support.microsoft.com/help/4010261/sql-server-2016-improvements-in-handling-some-data-types-and-uncommon-)（SQL Server 2016 在处理某些数据类型和不常见操作方面所做的改进）。
 

## <a name="verify-r-and-sql-data-schemas-in-advance"></a>提前验证 R 和 SQL 数据架构

一般情况下，每当你对特定的数据类型或数据结构在 R 中如何使用有疑问时，请使用  `str()` 函数获取 R 对象的内部结构和类型。 函数的结果将打印到 R 控制台，并且也在 **中的“消息”** [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]选项卡中的查询结果中可用。 

从数据库检索要在 R 代码中使用的数据时，始终应消除无法在 R 中使用的列以及不可用于分析的列，例如 GUID（唯一标识符）、时间戳和用于审核的其他列，或者 ETL 进程创建的沿袭信息。 

请注意，包含不必要的列可以会极大降低 R 代码的性能，尤其是将高基数列用作因子时。 因此，我们建议使用 SQL Server 系统存储过程和信息视图来提前获取给定表的数据类型，并消除或转换不兼容的列。 有关详细信息，请参阅 [Information Schema Views in Transact-SQL](../../relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)（Transact-SQL 中的信息架构视图）

如果某个特定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型不受 R 支持，但你需要在 R 脚本中使用数据列，我们建议在 R 脚本中使用这些数据之前，使用 [CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md) 函数确保数据类型转换按预期执行。  

> [!WARNING]
> 如果在移动数据时使用 **rxDataStep** 删除不兼容的列，请注意，**RxSqlServerData** 数据源类型不支持 _varsToKeep_ 和 _varsToDrop_ 参数。


## <a name="examples"></a>示例

### <a name="example-1-implicit-conversion"></a>示例 1：隐式转换

以下示例演示在 SQL Server 与 R.之间往返访问时如何转换数据。

该查询从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中获取一系列的值，然后使用存储过程 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 输入使用 R 运行时的值。

```sql
CREATE TABLE MyTable (    
 c1 int,    
 c2 varchar(10),    
 c3 uniqueidentifier    
);    
go    
INSERT MyTable VALUES(1, 'Hello', newid());    
INSERT MyTable VALUES(-11, 'world', newid());    
SELECT * FROM MyTable;    
  
EXECUTE sp_execute_external_script    
 @language = N'R'    
 , @script = N'    
inputDataSet["cR"] <- c(4, 2)    
str(inputDataSet)    
outputDataSet <- inputDataSet'    
 , @input_data_1 = N'SELECT c1, c2, c3 FROM MyTable'    
 , @input_data_1_name = N'inputDataSet'    
 , @output_data_1_name = N'outputDataSet'    
 WITH RESULT SETS((C1 int, C2 varchar(max), C3 varchar(max), C4 float));  
```

**结果**

|行 \#|C1|C2|C3|C4|
|-|-|-|-|-|
|1|1|你好|6e225611-4b58-4995-a0a5-554d19012ef1|4|
|2|-11|world|6732ea46-2d5d-430b-8ao1-86e7f3351c3e|2|

注意使用 R 中的 `str` 函数可获取输出数据的架构。 此函数返回以下信息：

```output
'data.frame':2 obs. of  4 variables:
 $ c1: int  1 -11
 $ c2: Factor w/ 2 levels "Hello","world": 1 2
 $ c3: Factor w/ 2 levels "6732EA46-2D5D-430B-8A01-86E7F3351C3E",..: 2 1
 $ cR: num  4 2
```

由此，可以看到下面的数据类型转换作为此查询的一部分隐式地执行：

+ **列 C1**。 列被表示为 **ssNoversion** 中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、R 中的 `integer` 和输出结果集中的 **ssNoversion** 。  
  
  未执行任何类型转换。  
  
+ **列 C2**。 列被表示为 **ssNoversion** 中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、R 中的 `factor` 和输出结果集中的 **varchar(max)** 。  
  
  注意输出如何变化；R 中的任何字符串（因子或常规字符串）将被表示为 **varchar(max)** ，不论字符串的长度为多少。  
  
+ **列 C3**。  列被表示为 **ssNoversion** 中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、R 中的 `character` 和输出结果集中的 **varchar(max)** 。
  
  注意发生的数据类型转换。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持 **ssNoversion** ，但 R 不支持；因此，标识符表示为字符串。
  
+ **列 C4**。 列包含由 R 脚本生成的值且不会出现在原始数据中。


## <a name="example-2-dynamic-column-selection-using-r"></a>示例 2：使用 R 进行动态列选择

以下示例演示如何使用 R 代码来检查无效的列类型。 该查询使用 SQL Server 系统视图获取指定表的架构，然后删除指定的类型无效的所有列。

```R
connStr <- "Server=.;Database=TestDB;Trusted_Connection=Yes"
data <- RxSqlServerData(connectionString = connStr, sqlQuery = "SELECT COLUMN_NAME FROM TestDB.INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = N'testdata' AND DATA_TYPE <> 'image';")
columns <- rxImport(data)
columnList <- do.call(paste, c(as.list(columns$COLUMN_NAME), sep = ","))
sqlQuery <- paste("SELECT", columnList, "FROM testdata")
```

## <a name="see-also"></a>另请参阅

+ [Python 与 SQL Server 之间的数据类型映射](../python/python-libraries-and-data-types.md)
