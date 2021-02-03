---
description: DELETE (Transact-SQL)
title: DELETE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/19/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DELETE
- DELETE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- row removal [SQL Server]
- DELETE statement [SQL Server], about DELETE statement
- views [SQL Server], deleting rows
- removing rows
- tables [SQL Server], deleting rows
- DELETE statement [SQL Server]
- deleting rows
- row removal [SQL Server], DELETE statement
- deleting data
ms.assetid: ed6b2105-0f35-408f-ba51-e36ade7ad5b2
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 47dcac82e79c2b7d839a7072c995b252c0b45e8a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177863"
---
# <a name="delete-transact-sql"></a>DELETE (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的表或视图中删除一行或多行。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ WITH <common_table_expression> [ ,...n ] ]  
DELETE   
    [ TOP ( expression ) [ PERCENT ] ]   
    [ FROM ]   
    { { table_alias  
      | <object>   
      | rowset_function_limited   
      [ WITH ( table_hint_limited [ ...n ] ) ] }   
      | @table_variable  
    }  
    [ <OUTPUT Clause> ]  
    [ FROM table_source [ ,...n ] ]   
    [ WHERE { <search_condition>   
            | { [ CURRENT OF   
                   { { [ GLOBAL ] cursor_name }   
                       | cursor_variable_name   
                   }   
                ]  
              }  
            }   
    ]   
    [ OPTION ( <Query Hint> [ ,...n ] ) ]   
[; ]  
  
<object> ::=  
{   
    [ server_name.database_name.schema_name.   
      | database_name. [ schema_name ] .   
      | schema_name.  
    ]  
    table_or_view_name   
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics

[ WITH <common_table_expression> [ ,...n ] ] 
DELETE [database_name . [ schema ] . | schema. ] table_name  
FROM [database_name . [ schema ] . | schema. ] table_name 
JOIN {<join_table_source>}[ ,...n ]  
ON <join_condition>
[ WHERE <search_condition> ]   
[ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  

<join_table_source> ::=   
{  
    [ database_name . [ schema_name ] . | schema_name . ] table_or_view_name [ AS ] table_or_view_alias 
    [ <tablesample_clause>]  
    | derived_table [ AS ] table_alias [ ( column_alias [ ,...n ] ) ]  
}  
```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
DELETE 
    [ FROM [database_name . [ schema ] . | schema. ] table_name ]   
    [ WHERE <search_condition> ]   
    [ OPTION ( <query_options> [ ,...n ]  ) ]  
[; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 WITH \<common_table_expression>  
 指定在 DELETE 语句作用域内定义的临时命名结果集，也称为公用表表达式。 结果集源自 SELECT 语句。  
  
 公用表表达式还可与 SELECT、INSERT、UPDATE 和 CREATE VIEW 等语句一起使用。 有关详细信息，请参阅 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)。  
  
 TOP **(** _expression_ **)** [ PERCENT ]  
 指定将要删除的任意行数或任意行的百分比。 *expression* 可以是行数或行的百分比。 与 INSERT、UPDATE 或 DELETE 一起使用的 TOP 表达式中被引用行将不按任何顺序排列。 有关详细信息，请参阅 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
  
 FROM  
 一个可选关键字，可用在 DELETE 关键字与目标 table_or_view_name 或 rowset_function_limited 之间 。  
  
 table_alias  
 在表示要从中删除行的表或视图的 FROM *table_source* 子句中指定的别名。  
  
 server_name  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 表或视图所在服务器的名称（使用链接服务器名称或 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 函数作为服务器名称）。 如果指定了 server_name，则需要 database_name 和 schema_name  。  
  
 *database_name*  
 数据库的名称。  
  
 *schema_name*  
 表或视图所属架构的名称。  
  
 table_or_view_name  
 要从中删除行的表或视图的名称。  
  
 在其作用域内还可用作 DELETE 语句中的表源的表变量。  
  
 table_or_view_name 引用的视图必须可更新，并且只在视图定义的 FROM 子句中引用一个基表。 有关可更新视图的详细信息，请参阅 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)。  
  
 rowset_function_limited  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 或 [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) 函数，视提供程序的功能而定。  
  
 WITH **(** \<table_hint_limited> [... *n*] **)**  
 指定目标表允许的一个或多个表提示。 需要有 WITH 关键字和括号。 不允许 NOLOCK 和 READUNCOMMITTED。 有关表提示的详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
 \<OUTPUT_Clause>  
 将已删除行或基于这些行的表达式作为 DELETE 操作的一部分返回。 在针对视图或远程表的任何 DML 语句中都不支持 OUTPUT 子句。 有关该子句的参数和行为的详细信息，请参阅 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)。  
  
 FROM *table_source*  
 指定附加的 FROM 子句。 通过针对 DELETE 的该 [!INCLUDE[tsql](../../includes/tsql-md.md)] 扩展，可从 \<table_source> 指定数据并从第一个 FROM 子句内的表中删除相应的行。  
  
 这个扩展指定联接，可在 WHERE 子句中取代子查询来标识要删除的行。  
  
 有关详细信息，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)。  
  
 WHERE  
 指定用于限制删除行数的条件。 如果没有提供 WHERE 子句，则 DELETE 删除表中的所有行。  
  
 基于 WHERE 子句中所指定的条件，有两种形式的删除操作：  
  
-   搜索删除指定搜索条件以限定要删除的行。 例如，WHERE *column_name* = *value*。  
  
-   定位删除使用 CURRENT OF 子句指定游标。 删除操作在游标的当前位置执行。 这比使用 WHERE *search_condition* 子句限定待删除行的搜索 DELETE 语句更为精确。 如果搜索条件不唯一标识单行，则搜索 DELETE 语句删除多行。  
  
\<search_condition>  
 指定删除行的限定条件。 对搜索条件中可以包含的谓词数量没有限制。 有关详细信息，请参阅[搜索条件 (Transact-SQL)](../../t-sql/queries/search-condition-transact-sql.md)。  
  
 CURRENT OF  
 指定 DELETE 在指定游标的当前位置执行。  
  
 GLOBAL  
 指定 cursor_name 是指全局游标。  
  
 cursor_name  
 从其中进行提取的打开游标的名称。 如果同时存在名为 cursor_name 的全局游标和局部游标，那么，在指定了 GLOBAL 时，该参数是指全局游标；否则是指局部游标。 游标必须允许更新。  
  
 cursor_variable_name  
 游标变量的名称。 游标变量必须引用允许更新的游标。  
  
 OPTION **(** \<query_hint> [ **,** ... *n*] **)**  
 关键字，指示用于自定义[!INCLUDE[ssDE](../../includes/ssde-md.md)]处理语句的方式的优化器提示。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
## <a name="best-practices"></a>最佳实践  
 若要删除表中的所有行，请使用 TRUNCATE TABLE。 TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少。 TRUNCATE TABLE 具有限制，例如，表不能参与复制。 有关详细信息，请参阅 [TRUNCATE TABLE (Transact-SQL)](../../t-sql/statements/truncate-table-transact-sql.md)  
  
 使用 @@ROWCOUNT 函数将删除的行数返回给客户端应用程序。 有关详细信息，请参阅 [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md)。  
  
## <a name="error-handling"></a>错误处理  
 可以通过在 TRY…CATCH 构造函数中指定 DELETE 语句，实现对该语句的错误处理。  
  
 如果 DELETE 语句违反了触发器，或试图删除另一个有 FOREIGN KEY 约束的表内的数据被引用行，则可能会失败。 如果 DELETE 删除了多行，而在删除的行中有任何一行违反触发器或约束，则将取消该语句，返回错误且不删除任何行。  
  
 当 DELETE 语句遇到在表达式计算过程中发生的算术错误（溢出、被零除或域错误）时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将处理这些错误，就好象 SET ARITHABORT 设置为 ON。 将取消批处理中的其余部分并返回错误消息。  
  
## <a name="interoperability"></a>互操作性  
 如果所修改的对象是表变量，则 DELETE 可用在用户定义函数的正文中。  
  
 删除包含 FILESTREAM 列的行时，会同时删除其基础文件系统文件。 基础文件是由 FILESTREAM 垃圾回收器删除的。 有关详细信息，请参阅[使用 Transact-SQL 访问 FILESTREAM 数据](../../relational-databases/blob/access-filestream-data-with-transact-sql.md)。  
  
 不能在直接或间接引用对其定义 INSTEAD OF 触发器的视图的 DELETE 语句中指定 FROM 子句。 有关 INSTEAD OF 触发器的详细信息，请参阅 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 在将 TOP 与 DELETE 结合使用时，被引用行不按任何顺序排列，不能直接在此语句中指定 ORDER BY 子句。 如果需要使用 TOP 来删除按有意义的时间顺序排列的行，您必须同时在嵌套 select 语句中使用 TOP 和 ORDER BY 子句。 请参阅本主题后面的“示例”一节。  
  
 对于已分区视图，不能在 DELETE 语句中使用 TOP。  
  
## <a name="locking-behavior"></a>锁定行为  
 默认情况下，DELETE 语句始终在其修改的表对象上获取意向排他 (IX) 锁定，并保持该锁定直到事务完成。 通过意向排他 (IX) 锁定，其他任何事物都不能修改数据；读取操作只能通过使用 NOLOCK 提示或 read uncommitted（读取未提交内容）隔离级别来进行。 您可以指定表提示，以便通过指定其他锁定方法来覆盖 DELETE 语句的持续时间的这一默认行为，但只建议经验丰富的开发人员和数据库管理员将提示用作最后的手段来执行。 有关详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
 从堆删除行时， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 可以使用行锁定或页锁定进行操作。 结果，删除操作导致的空页将继续分配给堆。 未释放空页时，数据库中的其他对象将无法重用关联的空间。  
  
 若要删除堆中的行并释放页，请使用下列方法之一。  
  
-   在 DELETE 语句中指定 TABLOCK 提示。 使用 TABLOCK 提示会导致删除操作获取表的排他锁，而不是行锁或页锁。 这将允许释放页。 有关 TABLOCK 提示的详细信息，请参阅[表提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-table.md)。  
  
-   如果要删除表中的所有行，请使用 `TRUNCATE TABLE`。  
  
-   删除行之前，请为堆创建聚集索引。 删除行之后，可以删除聚集索引。 与先前的方法相比，此方法非常耗时，并且使用更多的临时资源。  
  
> [!NOTE]  
>  通过使用 `ALTER TABLE <table_name> REBUILD` 语句，可随时将空白页从堆中删除。  
  
## <a name="logging-behavior"></a>日志记录行为  
始终完全记录 DELETE 语句。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求对目标表具有 `DELETE` 权限。 如果语句包含 WHERE 字句，则还需要 `SELECT` 权限。  
  
 默认向 `sysadmin` 固定服务器角色、`db_owner` 和 `db_datawriter` 固定数据库角色以及表所有者的成员授予 DELETE 权限。 `sysadmin`、`db_owner` 和 `db_securityadmin` 角色以及表所有者的成员可以将权限转让给其他用户。  
  
## <a name="examples"></a>示例  
  
|类别|作为特征的语法元素|  
|--------------|------------------------------|  
|[基本语法](#BasicSyntax)|DELETE|  
|[限制删除的行数](#LimitRows)|WHERE • FROM • 游标 •|  
|[从远程表中删除行](#RemoteTables)|链接服务器 • OPENQUERY 行集函数 • OPENDATASOURCE 行集函数|  
|[捕获 DELETE 语句的结果](#CaptureResults)|OUTPUT 子句|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a> 基本语法  
 本节中的示例说明了使用最低要求的语法的 DELETE 语句的基本功能。  
  
#### <a name="a-using-delete-with-no-where-clause"></a>A. 使用不带 WHERE 子句的 DELETE  
 下面的示例从 `SalesPersonQuotaHistory` 数据库的 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 表中删除所有行，因为该例未使用 WHERE 子句限制删除的行数。  
  
```sql
DELETE FROM Sales.SalesPersonQuotaHistory;  
GO  
```  
  
###  <a name="limiting-the-rows-deleted"></a><a name="LimitRows"></a> 限制删除的行数  
 本节中的示例演示了如何限制将被删除的行数。  
  
#### <a name="b-using-the-where-clause-to-delete-a-set-of-rows"></a>B. 使用 WHERE 子句删除行集  
 以下示例从 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `ProductCostHistory` 表中删除 `StandardCost` 列的值大于 `1000.00` 的所有行。  
  
```sql
DELETE FROM Production.ProductCostHistory  
WHERE StandardCost > 1000.00;  
GO  
```  
  
 下面的示例演示一个更复杂的 WHERE 子句。 WHERE 子句定义要确定删除的行而必须满足的两个条件。 `StandardCost` 列中的值必须介于 `12.00` 与 `14.00` 之间，而 `SellEndDate` 列中的值必须为 Null。 该示例还将打印“\@\@ROWCOUNT”函数中的值，以返回已删除的行数。  
  
```sql
DELETE Production.ProductCostHistory  
WHERE StandardCost BETWEEN 12.00 AND 14.00  
      AND EndDate IS NULL;  
PRINT 'Number of rows deleted is ' + CAST(@@ROWCOUNT as char(3));  
```  
  
#### <a name="c-using-a-cursor-to-determine-the-row-to-delete"></a>C. 使用游标以确定要删除的行  
 以下示例使用名为 `complex_cursor` 的游标删除 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `EmployeePayHistory` 表中的单行。 删除操作只影响当前从游标提取的单行。  
  
```sql
DECLARE complex_cursor CURSOR FOR  
    SELECT a.BusinessEntityID  
    FROM HumanResources.EmployeePayHistory AS a  
    WHERE RateChangeDate <>   
         (SELECT MAX(RateChangeDate)  
          FROM HumanResources.EmployeePayHistory AS b  
          WHERE a.BusinessEntityID = b.BusinessEntityID) ;  
OPEN complex_cursor;  
FETCH FROM complex_cursor;  
DELETE FROM HumanResources.EmployeePayHistory  
WHERE CURRENT OF complex_cursor;  
CLOSE complex_cursor;  
DEALLOCATE complex_cursor;  
GO  
```  
  
#### <a name="d-using-joins-and-subqueries-to-data-in-one-table-to-delete-rows-in-another-table"></a>D. 对一个表中的数据使用联接和子查询，以删除另一个表中的行  
 下面的示例演示两种基于一个表中的数据删除另一个表中的行的方法。 这两个示例均基于 `SalesPerson` 表中存储的年初至今的销售业绩，从 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `SalesPersonQuotaHistory` 表中删除行。 第一个 `DELETE` 语句显示与 ISO 兼容的子查询解决方案，第二个 `DELETE` 语句显示联接这两个表的 [!INCLUDE[tsql](../../includes/tsql-md.md)] FROM 扩展。  
  
```sql
-- SQL-2003 Standard subquery  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
WHERE BusinessEntityID IN   
    (SELECT BusinessEntityID   
     FROM Sales.SalesPerson   
     WHERE SalesYTD > 2500000.00);  
GO  
```  
  
```sql
-- Transact-SQL extension  
  
DELETE FROM Sales.SalesPersonQuotaHistory   
FROM Sales.SalesPersonQuotaHistory AS spqh  
INNER JOIN Sales.SalesPerson AS sp  
ON spqh.BusinessEntityID = sp.BusinessEntityID  
WHERE sp.SalesYTD > 2500000.00;  
GO  
```  
  
```sql
-- No need to mention target table more than once.  
  
DELETE spqh  
  FROM  
        Sales.SalesPersonQuotaHistory AS spqh  
    INNER JOIN Sales.SalesPerson AS sp  
        ON spqh.BusinessEntityID = sp.BusinessEntityID  
  WHERE  sp.SalesYTD > 2500000.00;  
```  
  
#### <a name="e-using-top-to-limit-the-number-of-rows-deleted"></a>E. 使用 TOP 限制删除的行数  
 当 TOP (n) 子句与 DELETE 一起使用时，将针对随机选择的 n 行执行删除操作 。 以下示例从 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `PurchaseOrderDetail` 表中删除到期日期早于 2006 年 7 月 1 日的 `20` 个随机行。  
  
```sql
DELETE TOP (20)   
FROM Purchasing.PurchaseOrderDetail  
WHERE DueDate < '20020701';  
GO  
```  
  
 如果需要使用 TOP 来删除按有意义的时间顺序排列的行，您必须同时使用 TOP 和 ORDER BY 子句。 下面的查询从 `PurchaseOrderDetail` 表中删除了其到期日期最早的 10 行。 为了确保仅删除 10 行，嵌套 Select 语句 (`PurchaseOrderID`) 中指定的列将成为表的主键。 如果指定列包含重复的值，则在嵌套 Select 语句中使用非键列可能会导致删除的行超过 10 个。  
  
```sql
DELETE FROM Purchasing.PurchaseOrderDetail  
WHERE PurchaseOrderDetailID IN  
   (SELECT TOP 10 PurchaseOrderDetailID   
    FROM Purchasing.PurchaseOrderDetail   
    ORDER BY DueDate ASC);  
GO  
```  
  
###  <a name="deleting-rows-from-a-remote-table"></a><a name="RemoteTables"></a> 从远程表中删除行  
 本节中的示例说明如何使用[链接服务器](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)或[行集函数](../functions/opendatasource-transact-sql.md)引用一个远程表，以便从该表中删除行。 远程表存在于不同的服务器或 SQL Server 实例上。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
#### <a name="f-deleting-data-from-a-remote-table-by-using-a-linked-server"></a>F. 通过使用链接服务器从远程表删除数据  
 下面的示例将删除远程表中的行。 该示例从使用 [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 创建指向远程数据源的链接开始。 然后，将链接服务器名称 `MyLinkServer` 指定为 server.catalog.schema.object 形式的由四个部分组成的对象名称的一部分。  
  
```sql
USE master;  
GO  
-- Create a link to the remote data source.   
-- Specify a valid server name for @datasrc as 'server_name' or 'server_name\instance_name'.  
  
EXEC sp_addlinkedserver @server = N'MyLinkServer',  
    @srvproduct = N' ',  
    @provider = N'SQLNCLI',   
    @datasrc = N'server_name',  
    @catalog = N'AdventureWorks2012';  
GO  
```  
  
```sql
-- Specify the remote data source using a four-part name   
-- in the form linked_server.catalog.schema.object.  
  
DELETE MyLinkServer.AdventureWorks2012.HumanResources.Department 
WHERE DepartmentID > 16;  
GO  
```  
  
#### <a name="g-deleting-data-from-a-remote-table-by-using-the-openquery-function"></a>G. 通过使用 OPENQUERY 函数从远程表删除数据  
 以下示例通过指定 [OPENQUERY](../../t-sql/functions/openquery-transact-sql.md) 行集函数从远程表中删除行。 在之前例子中创建的链接服务器名称用于此示例。  
  
```sql
DELETE OPENQUERY (MyLinkServer, 'SELECT Name, GroupName 
FROM AdventureWorks2012.HumanResources.Department  
WHERE DepartmentID = 18');  
GO  
```  
  
#### <a name="h-deleting-data-from-a-remote-table-by-using-the-opendatasource-function"></a>H. 通过使用 OPENDATASOURCE 函数从远程表删除数据  
 以下示例通过指定 [OPENDATASOURCE](../../t-sql/functions/opendatasource-transact-sql.md) 行集函数从远程表中删除行。 通过使用 server_name 或 server_name\instance_name 格式，为该数据源指定一个有效的服务器名称 。  
  
```sql
DELETE FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source= <server_name>; Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Department   
WHERE DepartmentID = 17;
```  
  
###  <a name="capturing-the-results-of-the-delete-statement"></a><a name="CaptureResults"></a> 捕获 DELETE 语句的结果  
  
#### <a name="i-using-delete-with-the-output-clause"></a>I. 使用带有 OUTPUT 子句的 DELETE  
 以下示例演示如何将 `DELETE` 语句的结果保存到 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的表变量中。  
  
```sql
DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] 
FROM Sales.ShoppingCartItem 
WHERE ShoppingCartID = 20621;  
GO  
```  
  
#### <a name="j-using-output-with-from_table_name-in-a-delete-statement"></a>J. 在 DELETE 语句中同时使用 OUTPUT 与 <from_table_name>  
 以下示例根据 `DELETE` 语句的 `FROM` 子句中定义的搜索条件，删除 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `ProductProductPhoto` 表中的行。 `OUTPUT` 子句返回所删除表中的列（ `DELETED.ProductID`、 `DELETED.ProductPhotoID`）以及 `Product` 表中的列。 在 `FROM` 子句中使用该项来指定要删除的行。  
  
```sql
DECLARE @MyTableVar table (  
    ProductID int NOT NULL,   
    ProductName nvarchar(50)NOT NULL,  
    ProductModelID int NOT NULL,   
    PhotoID int NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="k-delete-all-rows-from-a-table"></a>K. 从表中删除所有行  
 下面的示例从 `Table1` 表中删除所有行，因为该例未使用 WHERE 子句限制删除的行数。  
  
```sql
DELETE FROM Table1;  
```  
  
### <a name="l-delete-a-set-of-rows-from-a-table"></a>L. 从表中删除一组行

 以下示例从 `Table1` 表中删除 `StandardCost` 列的值大于 1000.00 的所有行。  
  
```sql
DELETE FROM Table1  
WHERE StandardCost > 1000.00;  
```  
  
### <a name="m-using-label-with-a-delete-statement"></a>M. 通过 DELETE 语句使用 LABEL

 以下示例将标签与 DELETE 语句一起使用。  
  
```sql
DELETE FROM Table1  
OPTION ( LABEL = N'label1' );  
  
```  
  
### <a name="n-using-a-label-and-a-query-hint-with-the-delete-statement"></a>N. 通过 DELETE 语句使用标签和查询提示

 此查询显示通过 DELETE 语句使用查询联接提示的基本语法。 有关联接提示以及如何使用 OPTION 子句的详细信息，请参阅 [OPTION 子句 (Transact-SQL)](../queries/option-clause-transact-sql.md)。
  
```sql
-- Uses AdventureWorks  
  
DELETE FROM dbo.FactInternetSales  
WHERE ProductKey IN (   
    SELECT T1.ProductKey FROM dbo.DimProduct T1   
    JOIN dbo.DimProductSubcategory T2  
    ON T1.ProductSubcategoryKey = T2.ProductSubcategoryKey  
    WHERE T2.EnglishProductSubcategoryName = 'Road Bikes' )  
OPTION ( LABEL = N'CustomJoin', HASH JOIN ) ;  
```  

### <a name="o-delete-using-a-where-clause"></a>O. 使用 WHERE 子句进行删除

此查询显示如何使用 WHERE 子句而不是 FROM 子句进行删除。

```sql
DELETE tableA WHERE EXISTS (
SELECT TOP 1 1 FROM tableB tb WHERE tb.col1 = tableA.col1
)
```

### <a name="p-delete-based-on-the-result-of-joining-with-another-table"></a>P. 根据与其他表联接的结果进行删除

此示例演示了如何根据与其他表联接的结果从表中删除内容。

```sql
CREATE TABLE dbo.Table1   
    (ColA int NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  

CREATE TABLE dbo.Table2   
    (ColA int PRIMARY KEY NOT NULL, ColB decimal(10,3) NOT NULL);  
GO  
INSERT INTO dbo.Table1 VALUES(1, 10.0), (1, 20.0);  
INSERT INTO dbo.Table2 VALUES(1, 0.0);  
GO  

DELETE dbo.Table2   
FROM dbo.Table2   
    INNER JOIN dbo.Table1   
    ON (dbo.Table2.ColA = dbo.Table1.ColA)
    WHERE dboTable2.ColA = 1;  
```

## <a name="see-also"></a>另请参阅

 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [TRUNCATE TABLE (Transact-SQL)](../../t-sql/statements/truncate-table-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [WITH common_table_expression (Transact-SQL)](../../t-sql/queries/with-common-table-expression-transact-sql.md)   
 [@@ROWCOUNT (Transact-SQL)](../../t-sql/functions/rowcount-transact-sql.md)  
  
