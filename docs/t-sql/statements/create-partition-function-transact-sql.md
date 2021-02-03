---
description: CREATE PARTITION FUNCTION (Transact-SQL)
title: CREATE PARTITION FUNCTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/19/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE PARTITION FUNCTION
- PARTITION
- PARTITION FUNCTION
- PARTITION_FUNCTION_TSQL
- PARTITION_TSQL
- CREATE_PARTITION_FUNCTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RANGE RIGHT partition functions
- RANGE LEFT partition functions
- partitioned indexes [SQL Server], functions
- functions [SQL Server], creating
- partition functions [SQL Server], creating
- partitioned tables [SQL Server], functions
- CREATE PARTITION FUNCTION statement
ms.assetid: 9dfe8b76-721e-42fd-81ae-14e22258c4f2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7e1c2b7c6c3b6e12ae3865a5c35ad8f9904cba4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188571"
---
# <a name="create-partition-function-transact-sql"></a>CREATE PARTITION FUNCTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在当前数据库中创建一个函数，该函数可根据指定列的值将表或索引的各行映射到分区。 使用 CREATE PARTITION FUNCTION 是创建已分区表或索引的第一步。 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中，一张表或一个索引最多可以有 15,000 个分区。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE PARTITION FUNCTION partition_function_name ( input_parameter_type )  
AS RANGE [ LEFT | RIGHT ]   
FOR VALUES ( [ boundary_value [ ,...n ] ] )   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 partition_function_name  
 是分区函数的名称。 分区函数名称在数据库内必须唯一，并且符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
 input_parameter_type  
 是用于分区的列的数据类型。 所有数据类型都可有效用作分区列，除了 **text**、 **ntext**、 **image**、 **xml**、 **timestamp**、 **varchar(max)**、 **nvarchar(max)**、 **varbinary(max)**、别名数据类型或 CLR 用户定义的数据类型。  
  
 实际列（也称为分区列）是在 CREATE TABLE 或 CREATE INDEX 语句中指定的。  
  
 boundary_value  
 为使用 partition_function_name 的已分区表或索引的每个分区指定边界值。 如果 boundary_value 为空，则分区函数使用 partition_function_name 将整个表或索引映射到单个分区。 只能使用 CREATE TABLE 或 CREATE INDEX 语句中指定的一个分区列。  
  
 boundary_value 是可以引用变量的常量表达式。 这包括用户定义类型变量，或函数以及用户定义函数。 它不能引用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 表达式。 boundary_value 必须匹配或可以隐式转换为 input_parameter_type 中提供的数据类型，并且当值的大小和小数位数不匹配其对应 input_parameter_type 时，将无法在隐式转换过程中被截断。  

> [!NOTE]  
>  如果 boundary_value 包含 datetime 或 smalldatetime 文字值，则为这些文字值在计算时假设会话语言为 us_english。 不推荐使用此行为。 要确保分区函数定义对于所有会话语言都具有预期的行为，建议使用对于所有语言设置都以相同方式进行解释的常量，例如 yyyymmdd 格式；或者将文字值显式转换为特定样式。 若要确定服务器的语言会话，请运行 `SELECT @@LANGUAGE`。
>
> 有关详细信息，请参阅[文字日期字符串转换为日期值的不确定性转换](../data-types/nondeterministic-convert-date-literals.md)。
  
 ...n  
 指定 boundary_value 提供的值的数目，不能超过 14,999。 创建的分区数等于 n + 1。 不必按顺序列出各值。 如果值未按顺序列出，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将对它们进行排序、创建函数并返回一个警告，说明未按顺序提供值。 如果 n 包括任何重复的值，则数据库引擎将返回错误。  
  
 LEFT | RIGHT  
 指定当间隔值由 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 按升序从左到右排序时，boundary_value [ ,...n ] 属于每个边界值间隔的哪一侧（左侧还是右侧）。 如果未指定，则默认值为 LEFT。  
  
## <a name="remarks"></a>备注  
 分区函数的作用域被限制为在其中创建该分区函数的数据库。 在该数据库内，分区函数驻留在与其他函数的命名空间不同的一个单独命名空间内。  
  
 分区列为空值的所有行都放在最左侧分区中，除非将 NULL 指定为边界值并指定了 RIGHT。 在这种情况下，最左侧分区为空分区，NULL 值被放置在后面的分区中。  
  
## <a name="permissions"></a>权限  
 可以使用下列任何一种权限执行 CREATE PARTITION FUNCTION：  
  
-   ALTER ANY DATASPACE 权限。 默认情况下，此权限授予 **sysadmin** 固定服务器角色和 **db_owner** 及 **db_ddladmin** 固定数据库角色的成员。  
  
-   在其中创建分区函数的数据库的 CONTROL 或 ALTER 权限。  
  
-   在其中创建分区函数的数据库所在服务器的 CONTROL SERVER 或 ALTER ANY DATABASE 权限。  
  
##  <a name="examples"></a><a name="BKMK_examples"></a> 示例  
  
### <a name="a-creating-a-range-left-partition-function-on-an-int-column"></a>A. 对 int 列创建 RANGE LEFT 分区函数  
 以下分区函数将表或索引分为四个分区。  
  
```sql  
CREATE PARTITION FUNCTION myRangePF1 (int)  
AS RANGE LEFT FOR VALUES (1, 100, 1000);  
```  
  
 下表显示对分区依据列 col1 使用此分区函数的表如何进行分区。  
  
|分区|1|2|3|4|  
|---------------|-------|-------|-------|-------|  
|**值**|**col1** <= `1`|col1  AND col1 > `1`   <= `100`|col1 > `100` AND col1 <=`1000` |**col1** > `1000`|  
  
### <a name="b-creating-a-range-right-partition-function-on-an-int-column"></a>B. 对 int 列创建 RANGE RIGHT 分区函数  
 以下分区函数与上一个示例使用相同的 boundary_value [ ,...n ] 值，但它指定 RANGE RIGHT。  
  
```sql  
CREATE PARTITION FUNCTION myRangePF2 (int)  
AS RANGE RIGHT FOR VALUES (1, 100, 1000);  
```  
  
 下表显示对分区依据列 col1 使用此分区函数的表如何进行分区。  
  
|分区|1|2|3|4|  
|---------------|-------|-------|-------|-------|  
|**值**|**col1** \< `1`|**col1** >= `1` AND **col1** \< `100`|**col1** >= `100` AND **col1** \< `1000`|**col1** >= `1000`| 
  
### <a name="c-creating-a-range-right-partition-function-on-a-datetime-column"></a>C. 对 datetime 列创建 RANGE RIGHT 分区函数  
 以下分区函数将表或索引分成 12 个分区，每个分区对应 datetime 列中的一年中一个月的值。  
  
```sql  
CREATE PARTITION FUNCTION [myDateRangePF1] (datetime)  
AS RANGE RIGHT FOR VALUES ('20030201', '20030301', '20030401',  
               '20030501', '20030601', '20030701', '20030801',   
               '20030901', '20031001', '20031101', '20031201');  
```  
  
 下表显示对分区依据列 datecol 上使用此分区函数的表或索引如何进行分区。  
  
|分区|1|2|...|11|12|  
|---------------|-------|-------|---------|--------|--------|  
|**值**|**datecol** \< `February 1, 2003`|**datecol** >= `February 1, 2003` AND **datecol** \< `March 1, 2003`||**datecol** >= `November 1, 2003` AND **col1** \< `December 1, 2003`|datecol  >= `December 1, 2003`| 
  
### <a name="d-creating-a-partition-function-on-a-char-column"></a>D. 对 char 列创建分区函数  
 以下分区函数将表或索引分为四个分区。  
  
```sql  
CREATE PARTITION FUNCTION myRangePF3 (char(20))  
AS RANGE RIGHT FOR VALUES ('EX', 'RXE', 'XR');  
```  
  
 下表显示对分区依据列 col1 使用此分区函数的表如何进行分区。  
  
|分区|1|2|3|4|  
|---------------|-------|-------|-------|-------|  
|**值**|**col1** \< `EX`...|**col1** >= `EX` AND **col1** \< `RXE`...|**col1** >= `RXE` AND **col1** \< `XR`...|**col1** >= `XR`| 
  
### <a name="e-creating-15000-partitions"></a>E. 创建 15,000 个分区  
 以下分区函数将表或索引分为 15,000 个分区。  
  
```sql  
--Create integer partition function for 15,000 partitions.  
DECLARE @IntegerPartitionFunction nvarchar(max) = 
    N'CREATE PARTITION FUNCTION IntegerPartitionFunction (int) 
    AS RANGE RIGHT FOR VALUES (';  
DECLARE @i int = 1;  
WHILE @i < 14999  
BEGIN  
SET @IntegerPartitionFunction += CAST(@i as nvarchar(10)) + N', ';  
SET @i += 1;  
END  
SET @IntegerPartitionFunction += CAST(@i as nvarchar(10)) + N');';  
EXEC sp_executesql @IntegerPartitionFunction;  
GO  
```  
  
### <a name="f-creating-partitions-for-multiple-years"></a>F. 为多个年度创建分区  
 以下分区函数将表或索引在 datetime2 列上分为 50 个分区。 对于 2007 年 1 月至 2011 年 1 月之间的每个月，都有一个分区。  
  
```sql  
--Create date partition function with increment by month.  
DECLARE @DatePartitionFunction nvarchar(max) = 
    N'CREATE PARTITION FUNCTION DatePartitionFunction (datetime2) 
    AS RANGE RIGHT FOR VALUES (';  
DECLARE @i datetime2 = '20070101';  
WHILE @i < '20110101'  
BEGIN  
SET @DatePartitionFunction += '''' + CAST(@i as nvarchar(10)) + '''' + N', ';  
SET @i = DATEADD(MM, 1, @i);  
END  
SET @DatePartitionFunction += '''' + CAST(@i as nvarchar(10))+ '''' + N');';  
EXEC sp_executesql @DatePartitionFunction;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [已分区表和已分区索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)   
 [$PARTITION (Transact-SQL)](../../t-sql/functions/partition-transact-sql.md)   
 [ALTER PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/alter-partition-function-transact-sql.md)   
 [DROP PARTITION FUNCTION (Transact-SQL)](../../t-sql/statements/drop-partition-function-transact-sql.md)   
 [CREATE PARTITION SCHEME (Transact-SQL)](../../t-sql/statements/create-partition-scheme-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE INDEX (Transact-SQL)](../../t-sql/statements/create-index-transact-sql.md)   
 [ALTER INDEX (Transact-SQL)](../../t-sql/statements/alter-index-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.partition_functions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-functions-transact-sql.md)   
 [sys.partition_parameters (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-parameters-transact-sql.md)   
 [sys.partition_range_values (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partition-range-values-transact-sql.md)   
 [sys.partitions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [sys.tables (Transact-SQL)](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-index-columns-transact-sql.md)  
  
  

