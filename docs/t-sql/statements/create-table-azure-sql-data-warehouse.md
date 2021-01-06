---
description: CREATE TABLE (Azure Synapse Analytics)
title: CREATE TABLE (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 07/03/2019
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: ea21c73c-40e8-4c54-83d4-46ca36b2cf73
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: bf8ef986da54559c5928677643f6bd99c63c2266
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643013"
---
# <a name="create-table-azure-synapse-analytics"></a>CREATE TABLE (Azure Synapse Analytics)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中创建新表。  

若要了解表以及如何使用它们，请参阅 [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的表](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-overview/)。

> [!NOTE]
>  除非另有说明，否则本文中有关 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 的讨论适用于 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]。

 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

<a name="Syntax"></a>

## <a name="syntax"></a>语法
  
```syntaxsql
-- Create a new table.
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    ( 
      { column_name <data_type>  [ <column_options> ] } [ ,...n ]
    )  
    [ WITH ( <table_option> [ ,...n ] ) ]  
[;]  

<column_options> ::=
    [ COLLATE Windows_collation_name ]  
    [ NULL | NOT NULL ] -- default is NULL  
    [ <column_constraint> ]

<column_constraint>::=
    {
        DEFAULT DEFAULT constant_expression
        | PRIMARY KEY NONCLUSTERED  NOT ENFORCED -- Applies to Azure Synapse Analytics only
        | UNIQUE NOT ENFORCED -- Applies to Azure Synapse Analytics only
    }

<table_option> ::=
    {
       CLUSTERED COLUMNSTORE INDEX --default for Azure Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column [,...n])  
      | HEAP --default for Parallel Data Warehouse
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) -- default is ASC
    }  
    {
        DISTRIBUTION = HASH ( distribution_column_name )
      | DISTRIBUTION = ROUND_ROBIN -- default for Azure Synapse Analytics
      | DISTRIBUTION = REPLICATE -- default for Parallel Data Warehouse
    }
    | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] -- default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) )

<data type> ::=
      datetimeoffset [ ( n ) ]  
    | datetime2 [ ( n ) ]  
    | datetime  
    | smalldatetime  
    | date  
    | time [ ( n ) ]  
    | float [ ( n ) ]  
    | real [ ( n ) ]  
    | decimal [ ( precision [ , scale ] ) ]   
    | numeric [ ( precision [ , scale ] ) ]   
    | money  
    | smallmoney  
    | bigint  
    | int   
    | smallint  
    | tinyint  
    | bit  
    | nvarchar [ ( n | max ) ]  -- max applies only to Azure Synapse Analytics 
    | nchar [ ( n ) ]  
    | varchar [ ( n | max )  ] -- max applies only to Azure Synapse Analytics  
    | char [ ( n ) ]  
    | varbinary [ ( n | max ) ] -- max applies only to Azure Synapse Analytics  
    | binary [ ( n ) ]  
    | uniqueidentifier  
```  

<a name="Arguments"></a>
## <a name="arguments"></a>参数

 *database_name*  
 将包含新表的数据库的名称。 默认为当前数据库。  
  
 *schema_name*  
 表的架构。 可选择指定架构  。 如果是空白，将使用默认架构。  
  
 *table_name*  
 新表的名称。 若要创建本地临时表，请在表名前加上 #。  有关临时表的说明和指南，请参阅 [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的临时表](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-temporary/)。 

 column_name  
 表列的名称。

### <a name="column-options"></a><a name="ColumnOptions"></a> 列选项

 `COLLATE` *Windows_collation_name*  
 指定表达式的排序规则。 此排序规则必须是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的 Windows 排序规则之一。 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持的 Windows 排序规则列表，请参阅 [Windows 排序规则名称 (Transact-SQL)](windows-collation-name-transact-sql.md)/)。  
  
 `NULL` | `NOT NULL`  
 指定列中是否允许使用 `NULL` 值。 默认为 `NULL`。  
  
 [ `CONSTRAINT` *constraint_name* ] `DEFAULT` *constant_expression*  
 指定默认列值。  
  
 | 参数 | 说明 |
 | -------- | ----------- |
 | constraint_name | 约束的可选名称。 该约束名称在数据库中是唯一的。 此名称可以重用于其他数据库。 |
 | constant_expression | 列的默认值。 表达式必须是文本值或一个常数。 例如，允许的常数表达式：`'CA'`、`4`。 禁止使用这些常量表达式：`2+3`、`CURRENT_TIMESTAMP`。 |
  
### <a name="table-structure-options"></a><a name="TableOptions"></a> 表结构选项

有关选择表类型的指南，请参阅[为 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的表编制索引](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index/)。
  
 `CLUSTERED COLUMNSTORE INDEX` 
 
将表存储为聚集列存储索引。 聚集列存储索引应用于所有表数据。 这是 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 的默认行为。
 
 `HEAP`：将表存储为堆。 这是 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 的默认行为。  
  
 `CLUSTERED INDEX` ( index_column_name [ ,...n ] )  
 将表存储为具有一个或多个键列的聚集索引。 此行为按行存储数据。 在索引中使用 index_column_name 来指定一个或多个键列的名称  。  有关详细信息，请参阅常规注释中的行存储表。
 
 `LOCATION = USER_DB`：此选项已遭弃用。 虽然在语法上可接受，但已不再需要它，而且它也不再影响行为。   
  
### <a name="table-distribution-options"></a><a name="TableDistributionOptions"></a> 表分发选项

若要了解如何选择最佳分发方法并使用分布式表，请参阅[在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中分发表](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-distribute/)。

`DISTRIBUTION = HASH` (distribution_column_name  )：通过哈希处理 distribution_column_name  中存储的值，将每行都分配到一个分发。 算法是确定性的。也就是说，它总是将相同的值哈希到相同的分发。  应将分发列定义为 NOT NULL，因为所有包含 NULL 值的行都分配到相同的分发。

`DISTRIBUTION = ROUND_ROBIN`：以轮循机制在所有分发上均匀地分发行。 这是 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 的默认行为。

`DISTRIBUTION = REPLICATE`：将表的一个副本存储在每个 Compute 节点上。 对于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]，表存储在每个 Compute 节点上的分发数据库上。 对于 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]，表存储在跨 Compute 节点的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 文件组中。 这是 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 的默认行为。
  
### <a name="table-partition-options"></a><a name="TablePartitionOptions"></a> 表分区选项
有关使用表分区的指南，请参阅[对 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的表进行分区](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/)。

 `PARTITION` ( *partition_column_name* `RANGE` [ `LEFT` | `RIGHT` ] `FOR VALUES` ( [ *boundary_value* [,...*n*] ] ))   
创建一个或多个表分区。 这些分区是水平表切片，可便于向行的子集应用操作，无论表是作为堆、聚集索引还是聚集列存储索引进行存储。 与分发列不同，表分区不确定存储每行的分发。 表分区决定行如何分组并存储在每个分发中。  

| 参数 | 说明 |
| -------- | ----------- |
|partition_column_name| 指定 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将用于行分区的列。 此列可以是任何数据类型。 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 按升序对分区列值进行排序。 在 `LEFT` 规范中，由低到高的排序是从 `RIGHT` 到 `RANGE`。 |  
| `RANGE LEFT` | 指定属于左侧分区的边界值（较低值）。 默认为“左”。 |
| `RANGE RIGHT` | 指定属于右侧分区的边界值（较高值）。 | 
| `FOR VALUES` ( boundary_value [,...n] ) | 指定分区的边界值。 boundary_value 是一个常数表达式  。 它不得为 NULL。 它必须匹配或可以隐式转换为 partition_column_name 的数据类型  。 无法在隐式转换期间截断它，这样值的大小和确定位数与 partition_column_name  的数据类型不匹配<br></br><br></br>如果你指定 `PARTITION` 子句，但不指定边界值，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 会创建包含一个分区的已分区表。 如果适用，稍后可以将表拆分成两个分区。<br></br><br></br>如果指定一个边界值，生成的表格有两个分区；一个用于低于边界值的值，另一个用于高于边界值的值。 如果你将分区移到未分区表中，未分区表会接收数据，但它的元数据中不会有分区边界。| 

 请在“示例”部分中查看[创建已分区表](#PartitionedTable)。

### <a name="ordered-clustered-columnstore-index-option"></a>有序聚集列存储索引选项 

聚集列存储索引 (CCI) 是用于在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中创建表的默认索引。  在将 CCI 中的数据在压缩到列存储段之前，不会对其进行排序。  使用 ORDER 创建 CCI 时，先对数据进行排序，然后再将其添加到索引段中，这样可以提高查询性能。 有关详细信息，请查看[使用有序聚集列存储索引进行性能调整](/azure/sql-data-warehouse/performance-tuning-ordered-cci?view=azure-sqldw-latest&preserve-view=true)。  

可以在 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 支持的任何数据类型的列（字符串列除外）上创建有序的 CCI。  

由于表中的列按顺序排列，用户可以在 sys.index_columns 中查询 column_store_order_ordinal 列   。  

有关详细信息，请查看[使用有序聚集列存储索引进行性能调整](/azure/sql-data-warehouse/performance-tuning-ordered-cci)。   

### <a name="data-type"></a><a name="DataTypes"></a> 数据类型

[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 支持最常用的数据类型。 以下是支持的数据类型及其详细信息和存储字节的列表。 要更好地理解数据类型以及如何使用它们，请参阅 [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中表的数据类型](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-data-types)。

有关数据类型转换的表，请参阅 [CAST 和 CONVERT (Transact-SQL)](../functions/cast-and-convert-transact-sql.md) 的“隐式转换”部分。

>[!NOTE]
>如需了解更多详情，请参阅[日期和时间数据类型和函数 &#40;Transact-SQL&#41;](../functions/date-and-time-data-types-and-functions-transact-sql.md)。

`datetimeoffset` [ ( n ) ]  
 n 的默认值为 7  。  
  
 `datetime2` [ ( n ) ]  
与 `datetime` 相同，只不过可以指定秒小数的数值。 n 的默认值为  `7`。  
  
|n 值|Precision|缩放|  
|--:|--:|-:|  
|`0`|19|0|  
|`1`|21|1|  
|`2`|22|2|  
|`3`|23|3|  
|`4`|24|4|  
|`5`|25|5|  
|`6`|26|6|  
|`7`|27|7|  
  
 `datetime`  
 根据公历，用 19 到 23 个字符存储日期和时间。 日期可以包含年、月和日。 时间包含小时、分钟、秒。可选择显示小数秒的三位数。 存储大小为 8 个字节。  
  
 `smalldatetime`  
 存储日期和时间。 存储大小为 4 个字节。  
  
 `date`  
 根据公历，使用最多 10 个字符的年、月和日来存储日期。 存储大小为 3 个字节。 日期存储为整数。  
  
 `time` [ ( n ) ]  
 n 的默认值为  `7`。  
  
 `float` [ ( n ) ]  
 用于表示浮点数值数据的近似数值数据类型。 浮点数据为近似值；也就是说，并非数据类型范围内的所有值都能精确地表示。 n 指定用于存储科学记数法中  *尾数的字节数*`float`。 n  表示精度和存储大小。 如果指定了 n，它必须是介于 *和* 之间的某个值`1``53`。 n 的默认值为  `53`。  
  
| n 值 | Precision | 存储大小 |  
| --------: | --------: | -----------: |  
| 1-24   | 7 位数  | 4 个字节      |  
| 25-53  | 15 位数 | 8 字节      |  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 将 n 视为下列两个可能值之一  。 如果 `1`<= n  ，将 n 视为  <= `24`  `24`。 如果 `25` <= n  ，将 n 视为  <= `53`  `53`。  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]`float` 数据类型从 *到* 之间的所有 n 值均符合 ISO 标准`1``53`。 double precision 的同义词是 `float(53)`。  
  
 `real` [ ( n ) ]  
 real 的定义与 float 相同。 `real` 的 ISO 同义词为 `float(24)`。  
  
 `decimal` [ ( precision [ , scale ] ) ] |  *[ ( precision [ , scale ] ) ]*  `numeric`  
 存储固定的精度和小数位数。  
  
 *精度*  
 最多可以存储的十进制数字的总位数，包括小数点左边和右边的位数。 该精度的取值范围必须为 `1` 到最大精度 `38`。 默认精度为 `18`。  
  
 *scale*  
 小数点右边可以存储的十进制数字的最大位数。 小数位数的取值范围必须为  *到精度*`0`  。 只有指定了精度，才能指定小数位数   。 默认确定位数为 `0`；因此，`0` <= 确定位数   <= 精度  。 最大存储大小基于精度而变化。  
  
| Precision | 存储字节数  |  
| ---------: |-------------: |  
|  1-9       |             5 |  
| 10-19      |             9 |  
| 20-28      |            13 |  
| 29-38      |            17 |  
  
 `money` | `smallmoney`  
 表示货币值的数据类型。  
  
| 数据类型 | 存储字节数 |  
| --------- | ------------: |  
| `money`|8|  
| `smallmoney` |4|  
  
 `bigint` \| `int` \| `smallint` \| `tinyint`  
 使用整数数据的精确数字数据类型。 存储如下表所示。  
  
| 数据类型 | 存储字节数 |  
| --------- | ------------: |  
| `bigint`|8|  
| `int` |4|  
| `smallint` |2|  
| `tinyint` |1|  
  
 `bit`  
 可以取值为 `1`、`0` 或 `NULL 的 integer 数据类型。 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 可优化 bit 列的存储。 如果表中的 bit 列为 8 列或更少，这些列作为 1 个字节存储。 如果 bit 列为 9 到 16 列，这些列作为 2 个字节存储，以此类推。  
  
 `nvarchar` [ ( n | `max` ) ]  -- `max` 仅适用于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]。  
 可变长度 Unicode 字符数据。 n 的取值范围为 1 至 4,000。 `max` 指示最大存储大小是 2^31-1 个字节 (2 GB)。 存储大小（以字节为单位）是所输入字符个数的两倍 + 2 个字节。 已输入数据的长度可以为 0 个字符。  
  
 `nchar` [ ( n ) ]  
 固定长度 Unicode 字符数据，长度为 n 个字节。 n 的取值范围必须为 `1` 到 `4000`。 存储大小为 n 字节的两倍。  
  
 `varchar` [ ( n  | `max` ) ]  -- `max` 仅适用于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]。   
 可变长度非 Unicode 字符数据，长度为 n 个字节。 n 的取值范围必须为 `1` 到 `8000`。 `max` 指示最大存储大小为 2 ^31-1 个字节 (2 GB)。存储大小是输入数据的实际长度 + 2 个字节。  
  
 `char` [ ( n ) ]  
 固定长度非 Unicode 字符数据，长度为 n 个字节。 n 的取值范围必须为 `1` 到 `8000`。 存储大小为 n 字节。 n 的默认值为 `1`。  
  
 `varbinary` [ ( n  | `max` ) ]  -- `max` 仅适用于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]。  
 可变长度二进制数据。 n 的取值范围为 `1` 到 `8000`。 `max` 指示最大存储大小是 2^31-1 个字节 (2 GB)。 存储大小是输入数据的实际长度加 2 个字节。 n 的默认值为 7。  
  
 `binary` [ ( n ) ]  
 固定长度二进制数据，长度为 n 个字节。 n 的取值范围为 `1` 到 `8000`。 存储大小为 n 字节。 n 的默认值为 `7`。  
  
 `uniqueidentifier`  
 16 字节 GUID。  
   
<a name="Permissions"></a>  
## <a name="permissions"></a>权限  
 创建表需要 `db_ddladmin` 固定数据库角色的权限，或者：
 - 数据库的 `CREATE TABLE` 权限
 - 将包含表的架构的 `ALTER SCHEMA` 权限。 

创建已分区表需要 `db_ddladmin` 固定数据库角色的权限，或者

- `ALTER ANY DATASPACE` 权限
  
 创建本地临时表的登录名在该表上获取 `CONTROL``INSERT``SELECT` 和 `UPDATE` 权限。  
 
<a name="GeneralRemarks"></a>  
## <a name="general-remarks"></a>一般备注  
 
有关最小和最大限制，请参阅 [[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 容量限制](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-service-capacity-limits/)。 
 
### <a name="determining-the-number-of-table-partitions"></a>确定表分区的数目
每个用户定义表划分为多个较小的表，这些表存储在称为“分发”的不同位置上。 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 使用 60 个分发。 在 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中，分发的数目取决于 Compute 节点的数目。
 
每个分发包含所有的表分区。 例如，如果有 60 个分发和 4 个表分区，再加上一个空分区，则会有 300 个分区 (5 x 60= 300)。 如果表是群集列存储索引，每个分区就有一个列存储索引。也就是说，将拥有 300 个列存储索引。

我们建议使用更少的表分区，确保每个列存储索引具有足够的行以充分利用列存储索引的优势。 有关详细信息，请参阅[对 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的表进行分区](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-partition/)和[[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] ](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)  

### <a name="rowstore-table-heap-or-clustered-index"></a>行存储表（堆或聚集索引）

行存储表是以逐行顺序存储的表。 它是堆或聚集索引。 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 创建所有包含页压缩的行存储表；此行为不是用户可配置的。

### <a name="columnstore-table-columnstore-index"></a>列存储表（列存储索引）

列存储表是以逐列顺序存储的表。 列存储索引是管理存储在列存储表中的数据的技术。  聚集列存储索引不影响数据的分发方式，而是影响数据在每个分发中的存储方式。

若要将行存储表更改为列存储表，请删除表上所有现有索引并创建一个聚集列存储索引。 有关示例，请参阅 [CREATE COLUMNSTORE INDEX (Transact-SQL)](../../t-sql/statements/create-columnstore-index-transact-sql.md)。

有关详细信息，请参阅以下文章：
- [列存储索引版本的功能摘要](/sql/relational-databases/indexes/columnstore-indexes-what-s-new)
- [为 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 中的表编制索引](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-tables-index/)
- [列存储索引指南](~/relational-databases/indexes/columnstore-indexes-overview.md) 

<a name="LimitationsRestrictions"></a>  
## <a name="limitations-and-restrictions"></a>限制和局限  
 无法对分发列定义 DEFAULT 约束。  
  
### <a name="partitions"></a>分区
如果使用的是分区，分区列无法使用仅 Unicode 排序规则。 例如，以下语句将失败。  
  
 ```sql
CREATE TABLE t1 ( c1 varchar(20) COLLATE Divehi_90_CI_AS_KS_WS) WITH (PARTITION (c1 RANGE FOR VALUES (N'')))
```  
 
 如果 boundary_value 是必须隐式转换为 partition_column_name 中数据类型的文本值，会出现差异 。 通过 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 系统视图显示文本值，但转换后的值用于 [!INCLUDE[tsql](../../includes/tsql-md.md)] 操作。 

### <a name="temporary-tables"></a>临时表

 不支持以 ## 开头的全局临时表。  
  
 本地临时表具有以下限制和约束：  
  
-   它们仅对当前会话可见。 在会话末尾 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 会自动删除它们。 若要显式删除它们，请使用 DROP TABLE 语句。   
-   无法重命名它们。 
-   它们不得有分区或视图。  
-   无法更改它们的权限。 `GRANT`、`DENY` 和 `REVOKE` 语句无法用于本地临时表。   
-   为临时表阻止数据库控制台命令。   
-   如果批处理中使用多个本地临时表，每个临时表都必须具有唯一的名称。 如果多个会话正在运行同一批处理并创建相同的本地临时表，[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 会以内部方式为本地临时表名追加一个数字后缀，为每个本地临时表保留唯一的名称。  
    
<a name="LockingBehavior"></a>   
## <a name="locking-behavior"></a>锁定行为  
 对表采用排他锁。 在 DATABASE、SCHEMA、SCHEMARESOLUTION 对象上采用共享锁。  
 

<a name="ExamplesColumn"></a>   
## <a name="examples-for-columns"></a>列的示例

### <a name="a-specify-a-column-collation"></a><a name="ColumnCollation"></a> A. 指定一个列排序规则 
 在以下示例中，使用两种不同的列排序规则创建表 `MyTable`。 默认情况下，列 `mycolumn1` 具有默认的排序规则 Latin1_General_100_CI_AS_KS_WS。 列 `mycolumn2` 具有排序规则 Frisian_100_CS_AS。  
  
```sql
CREATE TABLE MyTable   
  (  
    mycolumnnn1 nvarchar,  
    mycolumn2 nvarchar COLLATE Frisian_100_CS_AS )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```  
  
### <a name="b-specify-a-default-constraint-for-a-column"></a><a name="DefaultConstraint"></a> B. 指定列的 DEFAULT 约束

 以下示例显示了为列指定默认值的语法。 colA 列有一个名为 constraint_colA 的默认约束以及一个默认值 0。  
  
```sql
CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX )  
;  
```

<a name="ExamplesTemporaryTables"></a> 
## <a name="examples-for-temporary-tables"></a>临时表的示例

### <a name="c-create-a-local-temporary-table"></a><a name="TemporaryTable"></a> C. 创建本地临时表  
 下面的示例创建名为“#myTable”的本地临时表。 此表的指定名称包含三个部分（以 # 开头）。
  
```sql
CREATE TABLE AdventureWorks.dbo.#myTable
  (  
   id int NOT NULL,  
   lastName varchar(20),  
   zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```

<a name="ExTableStructure"></a>  
## <a name="examples-for-table-structure"></a>表结构的示例

### <a name="d-create-a-table-with-a-clustered-columnstore-index"></a><a name="ClusteredColumnstoreIndex"></a> D. 创建一个具有聚集列存储索引的表  
 以下示例创建一个具有聚集列存储索引的分布式表。 将每个分发存储为一个列存储。  
  
 聚集列存储索引不影响数据的分发方式；数据始终按行分发。 聚集列存储索引影响数据在每个分发中的存储方式。  
  
```sql
  CREATE TABLE MyTable
  (  
    colA int CONSTRAINT constraint_colA DEFAULT 0,  
    colB nvarchar COLLATE Frisian_100_CS_AS
  )  
WITH   
  (   
    DISTRIBUTION = HASH ( colB ),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  

### <a name="e-create-an-ordered-clustered-columnstore-index"></a><a name="OrderedClusteredColumnstoreIndex"></a> E. 创建有序聚集列存储索引

下面的示例展示了如何创建有序聚集列存储索引。 索引按 SHIPDATE 进行排序。

```sql
CREATE TABLE Lineitem  
WITH (DISTRIBUTION = ROUND_ROBIN, CLUSTERED COLUMNSTORE INDEX ORDER(SHIPDATE))  
AS  
SELECT * FROM ext_Lineitem
```

<a name="ExTableDistribution"></a> 
## <a name="examples-for-table-distribution"></a>表分发的示例

### <a name="f-create-a-round_robin-table"></a><a name="RoundRobin"></a> F. 创建 ROUND_ROBIN 表  
 以下示例创建 ROUND_ROBIN 表，其中包含三列并且没有分区。 数据分布在所有分发中。 该表是使用 CLUSTERED COLUMNSTORE INDEX 创建的，它能提供比堆或行存储聚集索引更好的性能和数据压缩。  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH ( CLUSTERED COLUMNSTORE INDEX );  
```  
  
### <a name="g-create-a-hash-distributed-table"></a><a name="HashDistributed"></a> G. 创建哈希分布式表

 以下示例创建与上面的示例相同的表。 但对于此表，分发行（位于 `id` 列），而不是像 ROUND_ROBIN 表一样随机分发。 该表是使用 CLUSTERED COLUMNSTORE INDEX 创建的，它能提供比堆或行存储聚集索引更好的性能和数据压缩。  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = HASH (id),   
    CLUSTERED COLUMNSTORE INDEX  
  );  
```  
  
### <a name="h-create-a-replicated-table"></a><a name="Replicated"></a> H. 创建已复制的表  
 以下示例创建一个类似于前面示例的已复制表。 将已复制表全部复制到每个 Compute 节点。 通过每个 Compute 节点上的副本，可以减少查询的数据移动。 此示例是使用 CLUSTERED INDEX 进行创建，可实现比堆更好的数据压缩。 堆可能包含的行不够，无法实现理想的 CLUSTERED COLUMNSTORE INDEX 压缩。  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode varchar(6)  
  )  
WITH  
  (   
    DISTRIBUTION = REPLICATE,
    CLUSTERED INDEX (lastName)  
  );  
```  

<a name="ExTablePartitions"></a> 
## <a name="examples-for-table-partitions"></a>表分区的示例

###  <a name="i-create-a-partitioned-table"></a><a name="PartitionedTable"></a> I. 创建已分区表

 以下示例创建与示例 A 中所示相同的表，并在 `id` 列上添加 RANGE LEFT 分区。 它指定了四个分区边界值，所以有五个分区。  
  
```sql
CREATE TABLE myTable
  (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
  (
  
    PARTITION ( id RANGE LEFT FOR VALUES (10, 20, 30, 40 )),  
    CLUSTERED COLUMNSTORE INDEX
  )  
;  
```  
  
 在此示例中，数据将分类到以下分区中：  
  
- 分区 1：列 <= 10
- 分区 2：10 < 列 < = 20
- 分区 3：20 < 列 < = 30
- 分区 4：30 < 列 < = 40
- 分区 5：40 < 列  
  
 如果将此同一个表分区为 RANGE RIGHT 而非 RANGE LEFT（默认），数据将分类到以下分区中：  
  
- 分区 1：列 < 10  
- 分区 2：10 < = 列 < 20
- 分区 3：20 < = 列 < 30
- 分区 4：30 < = 列 < 40
- 分区 5：40 < = 列  
  
### <a name="j-create-a-partitioned-table-with-one-partition"></a><a name="OnePartition"></a> J. 使用一个分区创建已分区表

 以下示例使用一个分区创建已分区表。 它不指定任何边界值，所以有一个分区。  
  
```sql
CREATE TABLE myTable (  
    id int NOT NULL,  
    lastName varchar(20),  
    zipCode int)  
WITH
    (
      PARTITION ( id RANGE LEFT FOR VALUES ( )),  
      CLUSTERED COLUMNSTORE INDEX  
    )  
;  
```  
  
### <a name="k-create-a-table-with-date-partitioning"></a><a name="DatePartition"></a> K. 创建具有日期分区的表

 以下示例创建一个名为 `myTable` 的新表，并在 `date` 列上进行分区。 使用 RANGE RIGHT 和日期作为边界值，它将在每个分区中放置一个月的数据。  
  
```sql
CREATE TABLE myTable (  
    l_orderkey      bigint,
    l_partkey       bigint,
    l_suppkey       bigint,
    l_linenumber    bigint,
    l_quantity      decimal(15,2),  
    l_extendedprice decimal(15,2),  
    l_discount      decimal(15,2),  
    l_tax           decimal(15,2),  
    l_returnflag    char(1),  
    l_linestatus    char(1),  
    l_shipdate      date,  
    l_commitdate    date,  
    l_receiptdate   date,  
    l_shipinstruct  char(25),  
    l_shipmode      char(10),  
    l_comment       varchar(44))  
WITH
  (
    DISTRIBUTION = HASH (l_orderkey),  
    CLUSTERED COLUMNSTORE INDEX,  
    PARTITION ( l_shipdate  RANGE RIGHT FOR VALUES
      (  
        '1992-01-01','1992-02-01','1992-03-01','1992-04-01','1992-05-01',
        '1992-06-01','1992-07-01','1992-08-01','1992-09-01','1992-10-01',
        '1992-11-01','1992-12-01','1993-01-01','1993-02-01','1993-03-01',
        '1993-04-01','1993-05-01','1993-06-01','1993-07-01','1993-08-01',
        '1993-09-01','1993-10-01','1993-11-01','1993-12-01','1994-01-01',
        '1994-02-01','1994-03-01','1994-04-01','1994-05-01','1994-06-01',
        '1994-07-01','1994-08-01','1994-09-01','1994-10-01','1994-11-01',
        '1994-12-01'  
      ))
  );  
```  
  
<a name="SeeAlso"></a>
## <a name="see-also"></a>另请参阅
 
[CREATE TABLE AS SELECT &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-as-select-azure-sql-data-warehouse.md)   
[DROP TABLE (Transact-SQL)](../../t-sql/statements/drop-table-transact-sql.md)   
[ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
[sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md) 
