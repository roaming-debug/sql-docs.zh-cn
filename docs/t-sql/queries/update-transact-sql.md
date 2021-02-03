---
description: UPDATE (Transact-SQL)
title: UPDATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATE_TSQL
- UPDATE
dev_langs:
- TSQL
helpviewer_keywords:
- DML [SQL Server], UPDATE statement
- data updates [SQL Server], UPDATE statement
- updating data [SQL Server]
- TOP clause, UPDATE
- OUTPUT clause
- large value data types
- data manipulation language [SQL Server], UPDATE statement
- user-defined types [SQL Server], modifying
- INSTEAD OF triggers
- modifying data [SQL Server], UPDATE statement
- data modifications [SQL Server], UPDATE statement
- large data, modifying
- .WRITE clause
- table modifications [SQL Server], UPDATE statement
- UDTs [SQL Server], modifying
- UPDATE statement [SQL Server], about UPDATE statement
- LOB data [SQL Server], modifying
- modifying LOB data
- UPDATE statement [SQL Server]
- FROM clause, UPDATE statement
- WHERE clause, UPDATE statement
ms.assetid: 40e63302-0c68-4593-af3e-6d190181fee7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4dde2d907d13a0f720f925fc33f9e7cb0ba708b5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237806"
---
# <a name="update-transact-sql"></a>UPDATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中更改表或视图中的现有数据。 有关示例，请参阅[示例](#UpdateExamples)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  

[ WITH <common_table_expression> [...n] ]  
UPDATE   
    [ TOP ( expression ) [ PERCENT ] ]   
    { { table_alias | <object> | rowset_function_limited   
         [ WITH ( <Table_Hint_Limited> [ ...n ] ) ]  
      }  
      | @table_variable      
    }  
    SET  
        { column_name = { expression | DEFAULT | NULL }  
          | { udt_column_name.{ { property_name = expression  
                                | field_name = expression }  
                                | method_name ( argument [ ,...n ] )  
                              }  
          }  
          | column_name { .WRITE ( expression , @Offset , @Length ) }  
          | @variable = expression  
          | @variable = column = expression  
          | column_name { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable { += | -= | *= | /= | %= | &= | ^= | |= } expression  
          | @variable = column { += | -= | *= | /= | %= | &= | ^= | |= } expression  
        } [ ,...n ]   
  
    [ <OUTPUT Clause> ]  
    [ FROM{ <table_source> } [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                  { { [ GLOBAL ] cursor_name }   
                      | cursor_variable_name   
                  }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]  
[ ; ]  
  
<object> ::=  
{   
    [ server_name . database_name . schema_name .   
    | database_name .[ schema_name ] .   
    | schema_name .  
    ]  
    table_or_view_name}  
```  
  
```syntaxsql 
-- Syntax for Azure Synapse Analysis

[ WITH <common_table_expression> [ ,...n ] ]
UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name
SET { column_name = { expression | NULL } } [ ,...n ]  
FROM [ database_name . [ schema_name ] . | schema_name . ] table_name   
JOIN {<join_table_source>}[ ,...n ] 
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

UPDATE [ database_name . [ schema_name ] . | schema_name . ] table_name   
SET { column_name = { expression | NULL } } [ ,...n ]  
[ FROM from_clause ]  
[ WHERE <search_condition> ]   
[ OPTION ( LABEL = label_name ) ]  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 WITH \<common_table_expression>  
 指定在 UPDATE 语句作用域内定义的临时命名结果集或视图，也称为公用表表达式 (CTE)。 CTE 结果集派生自简单查询并由 UPDATE 语句引用。  
  
 公用表表达式还可与 SELECT、INSERT、DELETE 和 CREATE VIEW 等语句一起使用。 有关详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。  
  
 TOP ( expression) [ PERCENT ]  
 指定更新的行数或行数百分比。 *expression* 可以是行数或行的百分比。  
  
 与 INSERT、UPDATE 或 DELETE 一起使用的 TOP 表达式中被引用行将不按任何顺序排列。  
  
 在 INSERT、UPDATE 和 DELETE 语句中，需要使用括号分隔 TOP 中的 expression。 有关详细信息，请参阅 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
  
 table_alias  
 在表示要从中更新行的表或视图的 FROM 子句中指定的别名。  
  
 server_name  
 是表或视图所在服务器的名称（使用链接服务器名称或 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 函数作为服务器名称）。 如果指定了 server_name，则需要 database_name 和 schema_name  。  
  
 *database_name*  
 数据库的名称。  
  
 *schema_name*  
 表或视图所属架构的名称。  
  
 table_or_view_name  
 要更新行的表或视图的名称。 table_or_view_name 引用的视图必须可更新，并且只在该视图的 FROM 子句中引用一个基表。 有关可更新视图的详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)。  
  
 rowset_function_limited  
 [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 或 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 函数，视提供程序的功能而定。  
  
 WITH ( \<Table_Hint_Limited> )   
 指定目标表允许的一个或多个表提示。 需要有 WITH 关键字和括号。 不允许 NOLOCK 和 READUNCOMMITTED。 有关表提示的信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
 @table_variable  
 将[表](../../t-sql/data-types/table-transact-sql.md) 变量指定为表源。  
  
 SET  
 指定要更新的列或变量名称的列表。  
  
 column_name  
 包含要更改的数据的列。 column_name 必须存在于 table_or view_name 中 。 不能更新标识列。  
  
 *expression*  
 返回单个值的变量、文字值、表达式或嵌套 select 语句（加括号）。 expression 返回的值替换 column_name 或 @ 变量中的现有值  。  
  
> [!NOTE]  
>  当引用 Unicode 字符数据类型 nchar、nvarchar 和 ntext 时，“expression”应采用大写字母“N”作为前缀  。 如果未指定“N”，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将字符串转换为与数据库或列的默认排序规则相对应的代码页。 此代码页中没有的字符都将丢失。  
  
 DEFAULT  
 指定用为列定义的默认值替换列中的现有值。 如果该列没有默认值并且定义为允许 Null 值，则该参数也可用于将列更改为 NULL。  
  
 { **+=** \| **-=** \| **\*=** \| **/=** \| **%=** \| **&=** \| **^=** \| **|=** }  
 复合赋值运算符：  
 += 相加并赋值  
 -= 相减并赋值  
 *= 相乘并赋值  
 /= 相除并赋值  
 %= 取模并赋值  
 &=                        “位与”并赋值  
 ^=              “位异或”并赋值  
 |=               “位或”并赋值  
  
 udt_column_name  
 用户定义类型列。  
  
 property_name | field_name   
 用户定义类型的公共属性或公共数据成员。  
  
 method_name ( argument [ ,... n] )  
 带一个或多个参数的 udt_column_name 的非静态公共赋值函数方法。  
  
 .WRITE (expression,@Offset,@Length)   
 指定要修改的 column_name 值的一部分。 expression 替换从 column_name 的 @Offset 开始的 @Length 单位   。 使用该子句只能指定 varchar(max)、nvarchar(max) 或 varbinary(max) 的列  。 column_name 不能为 NULL，也不能由表名或表别名限定。  
  
 expression 是复制到 column_name 的值 。 expression 的计算结果必须为 column_name 类型或者 expression 必须能够隐式强制转换为此类型 。 如果 expression 设置为 NULL，则忽略 @Length，并将 column_name 中的值按指定的 @Offset 截断   。  
  
 @Offset 是存储在 column_name 中的值的起点，从该点开始编写 expression  。 @Offset 是从 0 开始的序号字节位置，数据类型为 bigint，不能为负数。 如果 @Offset 为 NULL，则更新操作将在现有 column_name 值的结尾追加 expression，并忽略 @Length   。 如果 @Offset 大于 column_name 值的字节长度，则 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将返回错误 。 如果 @Offset 加上 @Length 超出了列中基础值的限度，则将删除到值的最后一个字符 。  
  
 @Length 是指列中某个部分的长度，从 @Offset 开始，该长度由 expression 替换  。 @Length 是 bigint 并且不能为负数。 如果 @Length 为 NULL，则更新操作将删除从 @Offset 到 column_name 值的结尾的所有数据  。  
  
 有关详细信息，请参阅[更新大值数据类型](#updating-lobs)。  
  
 @ variable  
 已声明的变量，该变量将设置为 expression 所返回的值。  
  
 SET **@** _variable_ = *column* = *expression* 将变量设置为与列相同的值。 这与 SET **@** _variable_ = _column_, _column_ = _expression_ 不同，后者将变量设置为列更新前的值。  
  
 \<OUTPUT_Clause>  
 在 UPDATE 操作中，返回更新后的数据或基于更新后的数据的表达式。 针对远程表或视图的任何 DML 语句都不支持 OUTPUT 子句。 有关该子句的参数和行为的详细信息，请参阅 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)。  
  
 FROM \<table_source>  
 指定将表、视图或派生表源用于为更新操作提供条件。 有关详细信息，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)。  
  
 如果所更新对象与 FROM 子句中的对象相同，并且在 FROM 子句中对该对象只有一个引用，则指定或不指定对象别名均可。 如果更新的对象在 FROM 子句中出现了不止一次，则对该对象的一个（并且只有一个）引用不能指定表别名。 FROM 子句中对该对象的所有其他引用都必须包含对象别名。  
  
 带 INSTEAD OF UPDATE 触发器的视图不能是含有 FROM 子句的 UPDATE 的目标。  
  
> [!NOTE]  
>  FROM 子句中对 OPENDATASOURCE、OPENQUERY 或 OPENROWSET 的任何调用与对用作更新目标的这些函数的任何调用都是分开独立计算的，即使为两个调用提供的参数相同也是如此。 具体而言，应用到上述任一调用的结果的筛选器或联接条件不会影响其他调用的结果。  
  
 WHERE  
 指定条件来限定所更新的行。 根据所使用的 WHERE 子句的形式，有两种更新形式：  
  
-   搜索更新指定搜索条件来限定要删除的行。  
  
-   定位更新使用 CURRENT OF 子句指定游标。 更新操作发生在游标的当前位置。  
  
\<search_condition>  
 为要更新的行指定需满足的条件。 搜索条件也可以是联接所基于的条件。 对搜索条件中可以包含的谓词数量没有限制。 有关谓词和搜索条件的详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  
  
CURRENT OF  
 指定更新在指定游标的当前位置进行。  
  
 使用 WHERE CURRENT OF 子句的定位更新将在游标的当前位置更新单行。 这比使用 WHERE \<search_condition> 子句限定所更新行的搜索更新更精确。 当搜索条件不唯一标识一行时，搜索更新将修改多行。  
  
GLOBAL  
 指定 cursor_name 是指全局游标。  
  
cursor_name  
 要从中进行提取的开放游标的名称。 如果同时存在名为 cursor_name 的全局游标和局部游标，那么，在指定了 GLOBAL 时，该参数是指全局游标；否则是指局部游标。 游标必须允许更新。  
  
cursor_variable_name  
 cursor 变量的名称。 cursor_variable_name 必须引用允许更新的游标。  
  
OPTION ( \<query_hint> [ ,... n ] )   
 指定优化器提示用于自定义[!INCLUDE[ssDE](../../includes/ssde-md.md)]处理语句的方式。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
## <a name="best-practices"></a>最佳做法  
 使用 `@@ROWCOUNT` 函数返回插入到客户端应用程序的行数。 有关详细信息，请参阅 [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md)。  
  
 可以在 UPDATE 语句中使用变量名称来显示受影响的旧值和新值，但仅当 UPDATE 语句影响单个记录时才应使用变量名称。 如果 UPDATE 语句影响多个记录，若要返回每个记录的旧值和新值，请使用 [OUTPUT 子句](../../t-sql/queries/output-clause-transact-sql.md)。  
  
 指定 FROM 子句为更新操作提供条件时务须小心。 如果 UPDATE 语句包含了未指定每个更新列的位置只有一个可用值的 FROM 子句（换句话说，如果 UPDATE 语句是不确定性的），则其结果将不明确。 例如，对于下面脚本中的 UPDATE 语句，`Table1` 中的全部两行都满足 UPDATE 语句中 FROM 子句的限定条件；但是，将使用 `Table1` 中的哪一行来更新 `Table2.` 中的行是不明确的。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT PRIMARY KEY NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;  
```  
  
 当结合使用 `FROM` 和 `WHERE CURRENT OF` 子句时，可能发生同样的问题。 在以下示例中，`Table2` 中的全部两行都满足 `FROM` 语句中 `UPDATE` 子句的限定条件。 将使用 `Table2` 的哪一行来更新 `Table1` 中的行是不明确的。  
  
```sql  
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ('dbo.Table1', 'U') IS NOT NULL  
    DROP TABLE dbo.Table1;  
GO  
IF OBJECT_ID ('dbo.Table2', 'U') IS NOT NULL  
    DROP TABLE dbo.Table2;  
GO  
CREATE TABLE dbo.Table1  
    (c1 INT PRIMARY KEY NOT NULL, c2 INT NOT NULL);  
GO  
CREATE TABLE dbo.Table2  
    (d1 INT PRIMARY KEY NOT NULL, d2 INT NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES (1, 10);  
INSERT INTO dbo.Table2 VALUES (1, 20), (2, 30);  
GO  
DECLARE abc CURSOR LOCAL FOR  
    SELECT c1, c2   
    FROM dbo.Table1;  
OPEN abc;  
FETCH abc;  
UPDATE dbo.Table1   
SET c2 = c2 + d2   
FROM dbo.Table2   
WHERE CURRENT OF abc;  
GO  
SELECT c1, c2 FROM dbo.Table1;  
GO  
```  
  
## <a name="compatibility-support"></a>兼容性支持  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中，将不再支持在 FROM 子句中使用应用于 UPDATE 或 DELETE 语句目标表的 READUNCOMMITTED 和 NOLOCK 提示。 请避免在新的开发工作上下文中使用这些提示，并计划修改当前使用它们的应用程序。  
  
## <a name="data-types"></a>数据类型  
 所有的 char 和 nchar 列向右填充至定义长度 。  
  
 如果 ANSI_PADDING 设置为 OFF，则会从插入 varchar 和 nvarchar 列的数据中删除所有尾随空格，但只包含空格的字符串除外 。 这些字符串被截断为空字符串。 如果 ANSI_PADDING 设置为 ON，则插入尾随空格。 Microsoft SQL Server ODBC 驱动程序和用于 SQL Server 的 OLE DB 访问接口自动对每个连接设置 ANSI_PADDING ON。 这可在 ODBC 数据源中进行配置，也可通过设置连接特性或属性进行配置。 有关详细信息，请参阅 [SET ANSI_PADDING (Transact-SQL)](../../t-sql/statements/set-ansi-padding-transact-sql.md)。  
  
### <a name="updating-text-ntext-and-image-columns"></a>更新 text、ntext 和 image 列  
 使用 UPDATE 修改 text、ntext 或 image 列时将对列进行初始化，向其列分配有效的文本指针，并且分配至少一个数据页（除非使用 NULL 更新该列）  。  
  
 若要替换或修改大型 text、ntext 或 image 数据块，请使用 [WRITETEXT](../../t-sql/queries/writetext-transact-sql.md) 或 [UPDATETEXT](../../t-sql/queries/updatetext-transact-sql.md)，而不使用 UPDATE 语句  。  
  
 如果 UPDATE 语句在更新聚集键以及一个或者多个 text、ntext 或 image 列时可以更改多个行，则对这些列的部分更新将作为替换所有值来执行  。  
  
> [!IMPORTANT]
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中将删除 ntext、text 和 image 数据类型  。 请避免在新开发工作中使用这些数据类型，并考虑修改当前使用这些数据类型的应用程序。 请改用 [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)、 [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)和 [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) 。  
  
### <a name="updating-large-value-data-types"></a><a name="updating-lobs"></a> 更新大值数据类型  
 使用 .WRITE (expression,@Offset,@Length) 子句执行 varchar(max)、nvarchar(max) 和 varbinary(max) 数据类型的部分或完整更新     。 
 
 例如，对 varchar(max) 列的部分更新可能只删除或修改该列的前 200 个字节（如果使用的是 ASCII 字符，则为前 200 个字符），而完整更新则删除或修改该列中的所有数据。 如果将数据库恢复模式设置为大容量日志模式或简单模式，则对插入或追加新数据的 .WRITE 更新进行最小日志记录。 在更新现有值时，不使用最小日志记录。 有关详细信息，请参阅 [事务日志 (SQL Server)](../../relational-databases/logs/the-transaction-log-sql-server.md)。  
  
 当 UPDATE 语句导致下列任一操作时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]便会将部分更新转换为完整更新：  
-   更改分区视图或表的键列。  
-   修改多行并且还将非唯一的聚集索引的键更新为非常量值。  
  
不能使用 .WRITE 子句更新 NULL 列或将 column_name 的值设置为 NULL。  
  
对于 varbinary 和 varchar 数据类型，以字节为单位指定 @Offset 和 @Length；对于 nvarchar 数据类型，则以字节对为单位进行指定   。 有关字符串数据类型长度的详细信息，请参阅 [char 和 varchar (Transact-SQL)](../../t-sql/data-types/char-and-varchar-transact-sql.md) 以及 [nchar 和 nvarchar (Transact-SQL)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)。
  
为了获得最佳性能，建议按照块区大小为 8040 字节倍数的方式插入或更新数据。  
  
如果在 OUTPUT 子句中引用了由 \.WRITE 子句修改的列，则该列的完整值（deleted.column\_name 中的前像或 inserted.column\_name 中的后像）返回到表变量中的指定列 。 请参阅后面的示例 R。  
  
若要针对其他字符或二进制数据类型获得相同的 \.WRITE 功能，请使用 [STUFF &#40;Transact-SQL&#41;](../../t-sql/functions/stuff-transact-sql.md)。  
  
### <a name="updating-user-defined-type-columns"></a>更新用户定义类型列  
 更新用户定义类型列中的值可以通过下列方式之一完成：  
  
-   提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型的值，条件是该用户定义类型支持该类型的隐式转换或显式转换。 以下示例显示如何通过从字符串显式转换来更新用户定义类型 `Point` 的列中的值。  
  
    ```sql  
    UPDATE Cities  
    SET Location = CONVERT(Point, '12.3:46.2')  
    WHERE Name = 'Anchorage';  
    ```  
  
-   调用标记为“赋值函数”的用户定义类型的方法执行更新。 以下示例调用类型 `Point` 的名为 `SetXY` 的赋值函数方法。 这将更新该类型的实例状态。  
  
    ```sql  
    UPDATE Cities  
    SET Location.SetXY(23.5, 23.5)  
    WHERE Name = 'Anchorage';  
    ```  
  
    > [!NOTE]  
    > 如果对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Null 值调用赋值函数方法，或者赋值函数方法产生的新值为 Null，则 [!INCLUDE[tsql](../../includes/tsql-md.md)] 将返回错误。  
  
-   修改用户定义类型的已注册属性或公共数据成员的值。 提供值的表达式必须可隐式转换为属性的类型。 以下示例修改用户定义类型 `X` 的属性 `Point` 的值。  
  
    ```sql  
    UPDATE Cities  
    SET Location.X = 23.5  
    WHERE Name = 'Anchorage';  
    ```  
  
     若要修改同一用户定义类型列的不同属性，请发出多个 UPDATE 语句，或者调用该类型的赋值函数方法。  
  
### <a name="updating-filestream-data"></a>更新 FILESTREAM 数据  
 您可以使用 UPDATE 语句将 FILESTREAM 字段更新为 null 值、空值或相对较小的内联数据量。 但是，使用 Win32 接口可以更有效地将大量数据以流的方式导入到文件中。 更新 FILESTREAM 字段时，即会修改文件系统中的基础 BLOB 数据。 将 FILESTREAM 字段设置为 NULL 即会删除与该字段相关联的 BLOB 数据。 不能使用 .WRITE(), 执行对 FILESTREAM 数据的部分更新。 有关详细信息，请参阅 [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md)。  
  
## <a name="error-handling"></a>错误处理。  
 如果对行的更新违反了某个约束或规则，或违反了对列的 NULL 设置，或者新值是不兼容的数据类型，则取消该语句、返回错误并且不更新任何记录。  
  
 当 UPDATE 语句在表达式求值过程中遇到算术错误（溢出、被零除或域错误）时，则不进行更新。 批处理的剩余部分不再执行，并且返回错误消息。  
  
 如果对参与聚集索引的一列或多列的更新导致聚集索引和行的大小超过 8,060 字节，则更新失败并且返回错误消息。  
  
## <a name="interoperability"></a>互操作性  
 仅当所修改的表是表变量时，才允许在用户定义函数的主体中使用 UPDATE 语句。  
  
 当对表的 UPDATE 操作定义 `INSTEAD OF` 触发器时，将运行触发器而不运行 UPDATE 语句。 早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 只支持对 UPDATE 和其他数据修改语句定义 AFTER 触发器。 不能在直接或间接引用定义有 `INSTEAD OF` 触发器的视图的 UPDATE 语句中指定 FROM 子句。 有关 INSTEAD OF 触发器的详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 不能在直接或间接引用定义有 `INSTEAD OF` 触发器的视图的 UPDATE 语句中指定 FROM 子句。 有关 `INSTEAD OF` 触发器的详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)。  
  
 在公用表表达式 (CTE) 是 UPDATE 语句的目标时，在该语句中对 CTE 的所有引用都必须匹配。 例如，如果在 FROM 子句中向 CTE 分配了一个别名，则该别名必须用于对 CTE 的所有其他引用。 需要明确的 CTE 引用，因为 CTE 没有对象 ID，而 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用对象 ID 来识别对象与其别名之间的隐式关系。 如果没有这一关系，查询计划可能会产生意外的联接行为和意外的查询结果。 以下示例演示在 CTE 是更新操作的目标对象时指定 CTE 的正确和不正确的方法。  
  
```sql  
USE tempdb;  
GO  
-- UPDATE statement with CTE references that are correctly matched.  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE x -- cte is referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;  
SELECT * FROM @x;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 ID     Value  
 ------ -----  
 1      100  
 2      200  
 (2 row(s) affected)  
```  

使用不正确匹配的 CTE 引用的 UPDATE 语句。  

```sql  
USE tempdb;  
GO  
DECLARE @x TABLE (ID INT, Value INT);  
DECLARE @y TABLE (ID INT, Value INT);  
INSERT @x VALUES (1, 10), (2, 20);  
INSERT @y VALUES (1, 100),(2, 200);  
  
WITH cte AS (SELECT * FROM @x)  
UPDATE cte   -- cte is not referenced by the alias.  
SET Value = y.Value  
FROM cte AS x  -- cte is assigned an alias.  
INNER JOIN @y AS y ON y.ID = x.ID;   
SELECT * FROM @x;   
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
ID     Value  
------ -----  
1      100  
2      100  
(2 row(s) affected)  
```  

## <a name="locking-behavior"></a>锁定行为  
 UPDATE 语句在其修改的任何行上获取排他 (X) 锁，并在事务完成之前持有这些锁。 根据 UPDATE 语句的查询计划、要修改的行数和事务的隔离级别，可以在 PAGE 级或 TABLE 级（而不是 ROW 级）获取这些锁。 为避免这些更高级别的锁，可以考虑将影响数千行或更多行的 update 语句分成若干个批次，并确保索引支持任何联接和筛选条件。 有关 SQL Server 中的锁定机制的详细信息，请参阅[数据库引擎中的锁定](../../relational-databases/sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine)一文。  
  
## <a name="logging-behavior"></a>日志记录行为  
 UPDATE 语句将记入日志；但是，对使用 \.WRITE 子句对较大值数据类型的部分更新进行最小日志记录。 有关详细信息，请参阅上一节“数据类型”中的“更新大值数据类型”。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求对目标表具有 `UPDATE` 权限。 如果 UPDATE 语句包含 WHERE 子句，或者 SET 子句中的 expression 使用了表中的某个列，则还要求对要更新的表具有 `SELECT` 权限。  
  
 UPDATE 权限默认授予 `sysadmin` 固定服务器角色、`db_owner` 和 `db_datawriter` 固定数据库角色的成员以及表所有者。 `sysadmin`、`db_owner` 和 `db_securityadmin` 角色的成员以及表所有者可以将权限转让给其他用户。  
  
##  <a name="examples"></a><a name="UpdateExamples"></a> 示例  
  
|类别|作为特征的语法元素|  
|--------------|------------------------------|  
|[基本语法](#BasicSyntax)|UPDATE|  
|[限制更新的行](#LimitingValues)|WHERE • TOP • WITH 公用表表达式 • WHERE CURRENT OF|  
|[设置列值](#ColumnValues)|计算值 • 复合运算符 • 默认值 • 子查询|  
|[指定目标对象，而非标准表](#TargetObjects)|视图 • 表变量 • 表别名|  
|[基于其他表中的数据更新数据](#OtherTables)|FROM|  
|[更新远程表中的行](#RemoteTables)|链接服务器 • OPENQUERY • OPENDATASOURCE|  
|[更新大型对象数据类型](#LOBValues)|.WRITE • OPENROWSET|  
|[更新用户定义类型](#UDTs)|用户定义类型|  
|[通过使用提示覆盖查询优化器的默认行为](#TableHints)|表提示 • 查询提示|  
|[捕获 UPDATE 语句的结果](#CaptureResults)|OUTPUT 子句|  
|[在其他语句中使用 UPDATE](#Other)|存储过程 • TRY…CATCH|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a>基本语法  
 本节中的示例说明了使用最低要求的语法的 UPDATE 语句的基本功能。  
  
#### <a name="a-using-a-simple-update-statement"></a>A. 使用简单 UPDATE 语句  
 以下示例对于 `Person.Address` 表中的所有行更新一列。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.Address  
SET ModifiedDate = GETDATE();  
```  
  
#### <a name="b-updating-multiple-columns"></a>B. 更新多个列  
 以下示例对于 `Bonus` 表中的所有行更新 `CommissionPct`、`SalesQuota` 和 `SalesPerson` 列中的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET Bonus = 6000, CommissionPct = .10, SalesQuota = NULL;  
GO  
```  
  
###  <a name="limiting-the-rows-that-are-updated"></a><a name="LimitingValues"></a> 限制更新的行  
 本节中的示例说明了可用于限制 UPDATE 语句所影响的行数的方法。  
  
#### <a name="c-using-the-where-clause"></a>C. 使用 WHERE 子句  
 以下示例使用 WHERE 子句指定要更新的行。 该语句对于 `Color` 列中已具有值“Red”且在以“Road-250”开头的 `Production.Product` 列中具有值的所有行更新 `Color` 表中 `Name` 列的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
SET Color = N'Metallic Red'  
WHERE Name LIKE N'Road-250%' AND Color = N'Red';  
GO  
```  
  
#### <a name="d-using-the-top-clause"></a>D. 使用 TOP 子句  
 以下示例使用 TOP 子句来限制 UPDATE 语句中修改的行数。 当 TOP (n) 子句与 UPDATE 一起使用时，将针对随机选择的“n”行执行更新操作 。 以下示例按照 `VacationHours` 表中 10 个随机行的 25% 更新 `Employee` 列。  
  
```sql  
USE AdventureWorks2012;
GO
UPDATE TOP (10) HumanResources.Employee
SET VacationHours = VacationHours * 1.25 ;
GO  
```  
  
 如果必须使用 TOP 来应用按有意义的时间顺序排列的更新，则必须在嵌套 select 语句中同时使用 TOP 和 ORDER BY。 下列示例更新了雇佣最早的 10 名雇员的假期小时数。  
  
```sql  
UPDATE HumanResources.Employee  
SET VacationHours = VacationHours + 8  
FROM (SELECT TOP 10 BusinessEntityID FROM HumanResources.Employee  
     ORDER BY HireDate ASC) AS th  
WHERE HumanResources.Employee.BusinessEntityID = th.BusinessEntityID;  
GO  
```  
  
#### <a name="e-using-the-with-common_table_expression-clause"></a>E. 使用 WITH common_table_expression 子句  
 以下示例为直接或间接用于创建 `PerAssemblyQty` 的所有部件和组件更新 `ProductAssemblyID 800` 值。 公用表表达式将返回用于直接生成 `ProductAssemblyID 800` 的部件和用于生成这些组件的部件等的层次结构列表。 只修改公用表表达式所返回的行。  
  
```sql  
USE AdventureWorks2012;  
GO  
WITH Parts(AssemblyID, ComponentID, PerAssemblyQty, EndDate, ComponentLevel) AS  
(  
    SELECT b.ProductAssemblyID, b.ComponentID, b.PerAssemblyQty,  
        b.EndDate, 0 AS ComponentLevel  
    FROM Production.BillOfMaterials AS b  
    WHERE b.ProductAssemblyID = 800  
          AND b.EndDate IS NULL  
    UNION ALL  
    SELECT bom.ProductAssemblyID, bom.ComponentID, p.PerAssemblyQty,  
        bom.EndDate, ComponentLevel + 1  
    FROM Production.BillOfMaterials AS bom   
        INNER JOIN Parts AS p  
        ON bom.ProductAssemblyID = p.ComponentID  
        AND bom.EndDate IS NULL  
)  
UPDATE Production.BillOfMaterials  
SET PerAssemblyQty = c.PerAssemblyQty * 2  
FROM Production.BillOfMaterials AS c  
JOIN Parts AS d ON c.ProductAssemblyID = d.AssemblyID  
WHERE d.ComponentLevel = 0;  
```  
  
#### <a name="f-using-the-where-current-of-clause"></a>F. 使用 WHERE CURRENT OF 子句  
 以下示例使用 WHERE CURRENT OF 子句来只更新游标位于其上的行。 如果游标基于某个联接，则只修改 UPDATE 语句中指定的 `table_name`。 其他参与该游标的表不会受到影响。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
UPDATE HumanResources.EmployeePayHistory  
SET PayFrequency = 2   
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
###  <a name="setting-column-values"></a><a name="ColumnValues"></a> 设置列值  
 本节中的示例说明了如何使用计算值、子查询和 DEFAULT 值来更新列。  
  
#### <a name="g-specifying-a-computed-value"></a>G. 指定计算值  
 以下示例在 UPDATE 语句中使用计算值。 该示例将 `Product` 表中所有行的 `ListPrice` 列的值加倍。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
UPDATE Production.Product  
SET ListPrice = ListPrice * 2;  
GO  
```  
  
#### <a name="h-specifying-a-compound-operator"></a>H. 指定复合运算符  
 下面的示例使用变量 `@NewPrice` 通过在当前价格基础上加 10 来提高所有红色自行车的价格。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NewPrice INT = 10;  
UPDATE Production.Product  
SET ListPrice += @NewPrice  
WHERE Color = N'Red';  
GO  
```  
  
 以下示例使用复合运算符 += 针对 `' - tool malfunction'` 为 10 到 12 的行将数据 `Name` 追加到列 `ScrapReasonID` 中的现有值之后。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ScrapReason   
SET Name += ' - tool malfunction'  
WHERE ScrapReasonID BETWEEN 10 and 12;  
```  
  
#### <a name="i-specifying-a-subquery-in-the-set-clause"></a>I. 在 SET 子句中指定子查询  
 以下示例使用 SET 子句中的子查询来确定用于更新列的值。 子查询必须只返回标量值（即每行返回一个值）。 此示例修改 `SalesYTD` 表中的 `SalesPerson` 列，以反映 `SalesOrderHeader` 表中记录的最近销售情况。 该子查询在 `UPDATE` 语句中聚合了每个销售人员的销售量。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
#### <a name="j-updating-rows-using-default-values"></a>J. 使用 DEFAULT 值更新行  
 以下示例针对 `CostRate` 值大于 `CostRate` 的所有行将 `20.00` 列设置为其默认值 (0.00)。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Location  
SET CostRate = DEFAULT  
WHERE CostRate > 20.00;  
```  
  
###  <a name="specifying-target-objects-other-than-standard-tables"></a><a name="TargetObjects"></a> 指定目标对象，而非标准表  
 本节中的示例说明了如何通过指定视图、表别名或表变量来更新行。  
  
#### <a name="k-specifying-a-view-as-the-target-object"></a>K. 将视图指定为目标对象  
 以下示例通过将视图指定为目标对象来更新表中的行。 该视图定义引用了多个表，但是 UPDATE 语句成功运行，因为它只引用了其中一个基础表中的列。 如果指定两个表中的列，UPDATE 语句将失败。 有关详细信息，请参阅[通过视图修改数据](../../relational-databases/views/modify-data-through-a-view.md)。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Person.vStateProvinceCountryRegion  
SET CountryRegionName = 'United States of America'  
WHERE CountryRegionName = 'United States';  
```  
  
#### <a name="l-specifying-a-table-alias-as-the-target-object"></a>L. 将表别名指定为目标对象  
 以下示例将更新 `Production.ScrapReason` 表中的行。 将分配给 FROM 子句中 `ScrapReason` 的表别名指定为 UPDATE 子句中的目标对象。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE sr  
SET sr.Name += ' - tool malfunction'  
FROM Production.ScrapReason AS sr  
JOIN Production.WorkOrder AS wo   
     ON sr.ScrapReasonID = wo.ScrapReasonID  
     AND wo.ScrappedQty > 300;  
```  
  
#### <a name="m-specifying-a-table-variable-as-the-target-object"></a>M. 将表变量指定为目标对象  
 以下示例将更新表变量中的行。  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create the table variable.  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
-- Populate the table variable with employee ID values from HumanResources.Employee.  
INSERT INTO @MyTableVar (EmpID)  
    SELECT BusinessEntityID FROM HumanResources.Employee;  
  
-- Update columns in the table variable.  
UPDATE @MyTableVar  
SET NewVacationHours = e.VacationHours + 20,  
    ModifiedDate = GETDATE()  
FROM HumanResources.Employee AS e   
WHERE e.BusinessEntityID = EmpID;  
  
-- Display the results of the UPDATE statement.  
SELECT EmpID, NewVacationHours, ModifiedDate FROM @MyTableVar  
ORDER BY EmpID;  
GO  
```  
  
###  <a name="updating-data-based-on-data-from-other-tables"></a><a name="OtherTables"></a> 基于其他表中的数据更新数据  
 本节中的示例说明了基于一个表中的信息更新另一个表中的行的方法。  
  
#### <a name="n-using-the-update-statement-with-information-from-another-table"></a>N. 将 UPDATE 语句用于来自其他表的信息  
 下面的示例修改 `SalesYTD` 表中的 `SalesPerson` 列，以反映 `SalesOrderHeader` 表中记录的最近销售情况。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD + SubTotal  
FROM Sales.SalesPerson AS sp  
JOIN Sales.SalesOrderHeader AS so  
    ON sp.BusinessEntityID = so.SalesPersonID  
    AND so.OrderDate = (SELECT MAX(OrderDate)  
                        FROM Sales.SalesOrderHeader  
                        WHERE SalesPersonID = sp.BusinessEntityID);  
GO  
```  
  
 上一个示例假定在特定日期只记录指定销售人员的一笔销售业务，并假定更新信息是最新的。 如果在同一天中可以记录指定销售人员的多笔销售业务，所示的示例将不能正常运行。 该示例运行时没有错误，但是每个 `SalesYTD` 值只能用一笔销售数据更新，而不管那一天实际发生了多少笔销售业务。 这是因为一条 UPDATE 语句永远不会对同一行更新两次。  
  
 对于特定销售人员在同一天可销售不止一批的情况，每个销售人员的所有销售量必须在 `UPDATE` 语句中合计在一起，如下例所示：  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Sales.SalesPerson  
SET SalesYTD = SalesYTD +   
    (SELECT SUM(so.SubTotal)   
     FROM Sales.SalesOrderHeader AS so  
     WHERE so.OrderDate = (SELECT MAX(OrderDate)  
                           FROM Sales.SalesOrderHeader AS so2  
                           WHERE so2.SalesPersonID = so.SalesPersonID)  
     AND Sales.SalesPerson.BusinessEntityID = so.SalesPersonID  
     GROUP BY so.SalesPersonID);  
GO  
```  
  
###  <a name="updating-rows-in-a-remote-table"></a><a name="RemoteTables"></a> 更新远程表中的行  
 本节中的示例说明了如何使用[链接服务器](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)或[行集函数](../functions/opendatasource-transact-sql.md)引用一个远程目标表，从而更新该表中的行。  
  
#### <a name="o-updating-data-in-a-remote-table-by-using-a-linked-server"></a>O. 使用链接服务器更新远程表中的数据  
 以下示例更新远程服务器上的表。 该示例从使用 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 创建指向远程数据源的链接开始。 然后，将链接服务器名称 `MyLinkedServer` 指定为 server.catalog.schema.object 形式的由四个部分组成的对象名称的一部分。 请注意，您必须为 `@datasrc` 指定有效的服务器名称。  
  
```sql  
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_nameinstance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkedServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI10',   
    @datasrc = N'<server name>',  
    @catalog = N'AdventureWorks2012';  
GO  
USE AdventureWorks2012;  
GO  
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
UPDATE MyLinkedServer.AdventureWorks2012.HumanResources.Department  
SET GroupName = N'Public Relations'  
WHERE DepartmentID = 4;  
```  
  
#### <a name="p-updating-data-in-a-remote-table-by-using-the-openquery-function"></a>P. 使用 OPENQUERY 函数更新远程表中的数据  
 以下示例通过指定 [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 行集函数来更新远程表中的行。 在之前例子中创建的链接服务器名称用于此示例。  
  
```sql  
UPDATE OPENQUERY (MyLinkedServer, 'SELECT GroupName FROM HumanResources.Department WHERE DepartmentID = 4')   
SET GroupName = 'Sales and Marketing';  
```  
  
#### <a name="q-updating-data-in-a-remote-table-by-using-the-opendatasource-function"></a>Q. 使用 OPENDATASOURCE 函数更新远程表中的数据  
 以下示例通过指定 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 行集函数来更新远程表中的行。 通过使用 server_name 或 server_name\instance_name 格式，为该数据源指定一个有效的服务器名称 。 您可能需要为即席分布式查询配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 有关详细信息，请参阅[即席分布式查询服务器配置选项](../../database-engine/configure-windows/ad-hoc-distributed-queries-server-configuration-option.md)。  

```sql
UPDATE OPENDATASOURCE('SQLNCLI', 'Data Source=<server name>;Integrated Security=SSPI').AdventureWorks2012.HumanResources.Department
SET GroupName = 'Sales and Marketing' WHERE DepartmentID = 4;  
```

###  <a name="updating-large-object-data-types"></a><a name="LOBValues"></a> 更新大型对象数据类型  
 本节中的示例说明了如何更新使用大型对象 (LOB) 数据类型定义的列中的值。  
  
#### <a name="r-using-update-with-write-to-modify-data-in-an-nvarcharmax-column"></a>R. 使用包含 .WRITE 的 UPDATE 来修改 nvarchar(max) 列中的数据  
 以下示例使用 .WRITE 子句更新 `DocumentSummary`（`Production.Document` 表内的 nvarchar(max) 列）中的部分值。 通过指定替换单词、现有数据中要替换的单词的开始位置（偏移量）以及要替换的字符数（长度），将单词 `components` 替换为单词 `features`。 此示例还使用 OUTPUT 子句将 `DocumentSummary` 列的前像和后像返回给 `@MyTableVar` 表变量。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO  
```  
  
#### <a name="s-using-update-with-write-to-add-and-remove-data-in-an-nvarcharmax-column"></a>S. 使用包含 .WRITE 的 UPDATE 在 nvarchar(max) 列中添加和删除数据  
 以下示例从当前值设置为 NULL 的 nvarchar(max) 列中添加和删除列。 由于不能使用 .WRITE 子句修改 NULL 列，因此先使用临时数据填充该列。 然后，使用 .WRITE 子句将该数据替换为正确的数据。 其他示例将数据追加到列值的结尾，从列中删除（截断）数据，最后从列中删除部分数据。 SELECT 语句显示由每个 UPDATE 语句生成的数据修改。  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Replacing NULL value with temporary data.  
UPDATE Production.Document  
SET DocumentSummary = N'Replacing NULL value'  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Replacing temporary data with the correct data. Setting @Length to NULL   
-- truncates all existing data from the @Offset position.  
UPDATE Production.Document  
SET DocumentSummary .WRITE(N'Carefully inspect and maintain the tires and crank arms.',0,NULL)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Appending additional data to the end of the column by setting   
-- @Offset to NULL.  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N' Appending data to the end of the column.', NULL, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing all data from @Offset to the end of the existing value by   
-- setting expression to NULL.   
UPDATE Production.Document  
SET DocumentSummary .WRITE (NULL, 56, 0)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
-- Removing partial data beginning at position 9 and ending at   
-- position 21.  
UPDATE Production.Document  
SET DocumentSummary .WRITE ('',9, 12)  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
SELECT DocumentSummary   
FROM Production.Document  
WHERE Title = N'Crank Arm and Tire Maintenance';  
GO  
```  
  
#### <a name="t-using-update-with-openrowset-to-modify-a-varbinarymax-column"></a>T. 使用包含 OPENROWSET 的 UPDATE 修改 varbinary(max) 列  
 以下示例将 varbinary(max) 列中存储的现有图像替换为新图像。 将 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 函数和 BULK 选项一起使用以将图像加载到列中。 此示例假定指定的文件路径中存在名为 `Tires.jpg` 的文件。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.ProductPhoto  
SET ThumbNailPhoto = (  
    SELECT *  
    FROM OPENROWSET(BULK 'c:Tires.jpg', SINGLE_BLOB) AS x )  
WHERE ProductPhotoID = 1;  
GO  
```  
  
#### <a name="u-using-update-to-modify-filestream-data"></a>U. 使用 UPDATE 来修改 FILESTREAM 数据  
 以下示例使用 UPDATE 语句来修改文件系统文件中的数据。 我们不建议使用此方法将大量数据以流方式传输到文件。 请使用适当的 Win32 接口。 下面的示例将文件记录中的所有文本替换为文本 `Xray 1`。 有关详细信息，请参阅 [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md)。  
  
```sql  
UPDATE Archive.dbo.Records  
SET [Chart] = CAST('Xray 1' as VARBINARY(max))  
WHERE [SerialNumber] = 2;  
```  
  
###  <a name="updating-user-defined-types"></a><a name="UDTs"></a> 更新用户定义类型  
 以下示例修改 CLR 用户定义类型 (UDT) 列中的值。 演示了三种方法。 有关用户定义的列的详细信息，请参阅 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。  
  
#### <a name="v-using-a-system-data-type"></a>V. 使用系统数据类型  
 通过提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 系统数据类型的值可以更新 UDT，条件是该用户定义类型支持该类型的隐式转换或显式转换。 以下示例显示如何通过从字符串显式转换来更新用户定义类型 `Point` 的列中的值。  
  
```sql  
UPDATE dbo.Cities  
SET Location = CONVERT(Point, '12.3:46.2')  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="w-invoking-a-method"></a>W. 调用方法  
 通过调用标记为“赋值函数”的用户定义类型的方法执行更新，可以更新 UDT。 以下示例调用类型 `Point` 的名为 `SetXY` 的赋值函数方法。 这将更新该类型的实例状态。  
  
```sql  
UPDATE dbo.Cities  
SET Location.SetXY(23.5, 23.5)  
WHERE Name = 'Anchorage';  
```  
  
#### <a name="x-modifying-the-value-of-a-property-or-data-member"></a>X. 修改属性或数据成员的值  
 通过修改用户定义类型的已注册属性或公共数据成员的值，可以更新 UDT。 提供值的表达式必须可隐式转换为属性的类型。 以下示例修改用户定义类型 `X` 的属性 `Point` 的值。  
  
```sql  
UPDATE dbo.Cities  
SET Location.X = 23.5  
WHERE Name = 'Anchorage';  
```  
  
###  <a name="overriding-the-default-behavior-of-the-query-optimizer-by-using-hints"></a><a name="TableHints"></a> 通过使用提示覆盖查询优化器的默认行为  
 本节中的示例说明了如何使用表提示和查询提示在处理 UPDATE 语句时暂时覆盖查询优化器的默认行为。  
  
> [!CAUTION]  
>  由于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查询优化器通常会为查询选择最佳执行计划，因此我们建议仅在最后迫不得已的情况下才可由资深的开发人员和数据库管理员使用提示。  
  
#### <a name="y-specifying-a-table-hint"></a>Y. 指定表提示  
 以下示例指定 [表提示](../../t-sql/queries/hints-transact-sql-table.md) TABLOCK。 此提示指定对 `Production.Product` 表采用共享锁，并保持到 UPDATE 语句结束。  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE Production.Product  
WITH (TABLOCK)  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE 'BK-%';  
GO  
```  
  
#### <a name="z-specifying-a-query-hint"></a>的授权请求。 指定查询提示  
 以下示例在 UPDATE 语句中指定[查询提示](../../t-sql/queries/hints-transact-sql-query.md)`OPTIMIZE FOR (@variable)`。 此提示指示查询优化器在编译和优化查询时对局部变量使用特定值。 仅在查询优化期间使用该值，在查询执行期间不使用该值。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE Production.uspProductUpdate  
@Product NVARCHAR(25)  
AS  
SET NOCOUNT ON;  
UPDATE Production.Product  
SET ListPrice = ListPrice * 1.10  
WHERE ProductNumber LIKE @Product  
OPTION (OPTIMIZE FOR (@Product = 'BK-%') );  
GO  
-- Execute the stored procedure   
EXEC Production.uspProductUpdate 'BK-%';  
```  
  
###  <a name="capturing-the-results-of-the-update-statement"></a><a name="CaptureResults"></a> 捕获 UPDATE 语句的结果  
 本节中的示例说明如何使用 [OUTPUT 子句](../../t-sql/queries/output-clause-transact-sql.md)从 UPDATE 语句影响的每一行返回信息（或基于的表达式）。 这些结果可以返回到处理应用程序，以供在确认消息、存档以及其他类似的应用程序要求中使用。  
  
#### <a name="aa-using-update-with-the-output-clause"></a>AA. 使用包含 OUTPUT 子句的 UPDATE  
 以下示例针对前 10 行的 25％ 更新 `VacationHours` 表中的列 `Employee` 并将 `ModifiedDate` 列中的值设置为当前日期。 `OUTPUT` 子句将返回 `VacationHours` 的值，该值在将 `UPDATE` 列中的 `deleted.VacationHours` 语句和 `inserted.VacationHours` 列中的已更新值应用于 `@MyTableVar` 表变量之前存在。  
  
 在它后面的两个 `SELECT` 语句返回 `@MyTableVar` 中的值以及 `Employee` 表中更新操作的结果。 有关使用 OUTPUT 子句的更多示例，请参阅 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate datetime);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
###  <a name="using-update-in-other-statements"></a><a name="Other"></a> 在其他语句中使用 UPDATE  
 本节中的示例说明了如何在其他语句中使用 UPDATE。  
  
#### <a name="ab-using-update-in-a-stored-procedure"></a>AB. 在存储过程中使用 UPDATE  
 下面的示例在一个存储过程中使用了 UPDATE 语句。 该过程采用一个输入参数 `@NewHours` 和一个输出参数 `@RowCount`。 在 UPDATE 语句中使用 `@NewHours` 参数值来更新表 `HumanResources.Employee` 中的列 `VacationHours`。 `@RowCount` 输出参数用于将影响的行数返回给一个局部变量。 在 SET 子句中使用 CASE 表达式，以便确定按条件确定为 `VacationHours` 设置的值。 在按每小时向员工付薪时 (`SalariedFlag` = 0)，`VacationHours` 设置为当前小时数加上 `@NewHours` 中指定的值；否则，`VacationHours` 设置为在 `@NewHours` 中指定的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE HumanResources.Update_VacationHours  
@NewHours SMALLINT  
AS   
SET NOCOUNT ON;  
UPDATE HumanResources.Employee  
SET VacationHours =   
    ( CASE  
         WHEN SalariedFlag = 0 THEN VacationHours + @NewHours  
         ELSE @NewHours  
       END  
    )  
WHERE CurrentFlag = 1;  
GO  
  
EXEC HumanResources.Update_VacationHours 40;  
```  
  
#### <a name="ac-using-update-in-a-trycatch-block"></a>AC. 在 TRY…CATCH 块中使用 UPDATE  
 以下示例在 TRY…CATCH 块中使用 UPDATE 语句来处理在执行更新操作期间可能发生的执行错误。  
  
```sql  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Intentionally generate a constraint violation error.  
    UPDATE HumanResources.Department  
    SET Name = N'MyNewName'  
    WHERE DepartmentID BETWEEN 1 AND 2;  
END TRY  
BEGIN CATCH  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>示例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="ad-using-a-simple-update-statement"></a>AD. 使用简单 UPDATE 语句  
 以下示例显示在未使用 WHERE 子句指定要更新的行时如何影响所有行。  
  
 此示例针对 `DimEmployee` 表中的所有行更新 `EndDate` 和 `CurrentFlag` 列中的值。  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET EndDate = '2010-12-31', CurrentFlag='False';  
```  
  
 也可以在 UPDATE 语句中使用计算值。 下面的示例将 `ListPrice` 表中所有行的 `Product` 列的值加倍。  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET BaseRate = BaseRate * 2;  
```  
  
### <a name="ae-using-the-update-statement-with-a-where-clause"></a>AE. 将 UPDATE 语句用于 WHERE 子句  
 以下示例使用 WHERE 子句指定要更新的行。  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimEmployee  
SET FirstName = 'Gail'  
WHERE EmployeeKey = 500;  
```  
  
### <a name="af-using-the-update-statement-with-label"></a>AF. 将 UPDATE 语句用于标签  
 以下示例演示 UPDATE 语句中 LABEL 的用法。  
  
```sql  
-- Uses AdventureWorks  
  
UPDATE DimProduct  
SET ProductSubcategoryKey = 2   
WHERE ProductKey = 313  
OPTION (LABEL = N'label1');  
```  
  
### <a name="ag-using-the-update-statement-with-information-from-another-table"></a>AG. 将 UPDATE 语句用于来自其他表的信息  
 此示例创建一个表，用于存储每年的总销售额。 通过对 FactInternetSales 表运行 SELECT 语句来更新 2004 年的总销售额。  
  
```sql  
-- Uses AdventureWorks  
  
CREATE TABLE YearlyTotalSales (  
    YearlySalesAmount MONEY NOT NULL,  
    Year SMALLINT NOT NULL )  
WITH ( DISTRIBUTION = REPLICATE );  
  
INSERT INTO YearlyTotalSales VALUES (0, 2004);  
INSERT INTO YearlyTotalSales VALUES (0, 2005);  
INSERT INTO YearlyTotalSales VALUES (0, 2006);  
  
UPDATE YearlyTotalSales  
SET YearlySalesAmount=  
(SELECT SUM(SalesAmount) FROM FactInternetSales WHERE OrderDateKey >=20040000 AND OrderDateKey < 20050000)  
WHERE Year=2004;  
  
SELECT * FROM YearlyTotalSales;   
```  

### <a name="ah-ansi-join-for-update-statements"></a>AH. 更新语句的 ANSI 联接
此示例演示了如何根据与其他表联接的结果来更新数据。

```sql
CREATE TABLE dbo.Table1   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
CREATE TABLE dbo.Table2   
    (ColA INT NOT NULL, ColB DECIMAL(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  
UPDATE dbo.Table2   
SET dbo.Table2.ColB = dbo.Table2.ColB + dbo.Table1.ColB  
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA);  
GO  
SELECT ColA, ColB   
FROM dbo.Table2;
GO
```

## <a name="see-also"></a>另请参阅  
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [游标 (Transact-SQL)](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [文本与图像函数 (Transact-SQL)](../functions/text-and-image-functions-textptr-transact-sql.md)   
 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [FILESTREAM (SQL Server)](../../relational-databases/blob/filestream-sql-server.md)  
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)    
 [单字节和多字节字符集](/cpp/c-runtime-library/single-byte-and-multibyte-character-sets)  
