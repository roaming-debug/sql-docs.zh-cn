---
description: SELECT - ORDER BY 子句 (Transact-SQL)
title: ORDER BY 子句 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ORDER_TSQL
- BY
- ORDER_BY_TSQL
- BY_TSQL
- ORDER
- ORDER BY
dev_langs:
- TSQL
helpviewer_keywords:
- ad-hoc query paging
- OFFSET clause
- SELECT statement [SQL Server], FETCH clause
- clauses [SQL Server], ORDER BY
- SELECT statement [SQL Server], limiting the rows returned
- data [SQL Server], limiting the rows returned
- data [SQL Server], ad-hoc query paging
- sort orders [SQL Server]
- SELECT statement [SQL Server], OFFSET clause
- ORDER BY clause [Transact-SQL]
- LIMIT
- limiting result sets
- SELECT statement [SQL Server], ORDER BY clause
- query paging
- data [SQL Server], sorting
- limiting data returned in a query
- sort orders [SQL Server], ORDER BY clause
- FETCH clause
ms.assetid: bb394abe-cae6-4905-b5c6-8daaded77742
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a7903d3a208bd08c9ba22d251a0e4124e81725ea
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98711983"
---
# <a name="select---order-by-clause-transact-sql"></a>SELECT - ORDER BY 子句 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的查询返回的数据进行排序。 可以使用此子句执行以下操作：  
  
-   按指定的列列表对查询的结果集进行排序，并有选择地将返回的行限制为指定范围。 除非指定 ORDER BY 子句，否则，不能保证在结果集中返回的行的顺序。  
  
-   确定将[排名函数](../../t-sql/functions/ranking-functions-transact-sql.md)值应用于结果集的顺序。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!NOTE]  
>  [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中的 SELECT/INTO 或 CREATE TABLE AS SELECT (CTAS) 语句中不支持 ORDER BY。

## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]   
[ <offset_fetch> ]  
  
<offset_fetch> ::=  
{   
    OFFSET { integer_constant | offset_row_count_expression } { ROW | ROWS }  
    [  
      FETCH { FIRST | NEXT } {integer_constant | fetch_row_count_expression } { ROW | ROWS } ONLY  
    ]  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ ORDER BY   
    {  
    order_by_expression   
    [ ASC | DESC ]   
    } [ ,...n ]   
]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 order_by_expression  
 指定用于对查询结果集进行排序的列或表达式。 可以将排序列指定为一个名称或列别名，也可以指定一个表示列在选择列表中所处位置的非负整数。  
  
 可以指定多个排序列。 别名必须是唯一的。 ORDER BY 子句中的排序列的顺序定义了排序结果集的结构。 也就是说，按第一列对结果集进行排序，然后按第二列对排序列表进行排序，依此类推。  
  
 ORDER BY 子句中引用的列名必须明确对应于选择列表中的列或列别名，或对应于 FROM 子句中指定的表中定义的列。 如果 ORDER BY 子句引用选择列表中的列别名，则必须单独使用列别名，而不是作为 ORDER BY 子句中的某些表达式的一部分，例如：
 
```sql
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName; -- correct 
SELECT SCHEMA_NAME(schema_id) AS SchemaName FROM sys.objects 
ORDER BY SchemaName + ''; -- wrong
```
  
 COLLATE collation_name  
 指定应根据 collation_name 中指定的排序规则执行 ORDER BY 操作，而不是根据表或视图中所定义的列的排序规则  。 collation_name 既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称  。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。 COLLATE 仅适用于类型为 char、varchar、nchar 和 nvarchar 的列     。  
  
 **ASC** | DESC  
 指定按升序或降序排列指定列中的值。 ASC 按从最低值到最高值的顺序进行排序。 DESC 按从最高值到最低值的顺序进行排序。 ASC 是默认排序顺序。 Null 值被视为最低的可能值。  
  
 OFFSET { integer_constant *offset_row_count_expression } { ROW | ROWS }*  |   
 指定开始从查询表达式返回行之前跳过的行数。 该值可以是大于或等于零的整数常量或表达式。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].s  
  
 offset_row_count_expression 可以是变量、参数或常量标量子查询  。 在使用子查询时，它无法引用在外部查询范围中定义的任何列。 也就是说，它无法与外部查询相关联。  
  
 ROW 和 ROWS 是同义词，是为了与 ANSI 兼容而提供的。  
  
 在查询执行计划中，将在 TOP 查询运算符的 Offset 属性中显示偏移行数值  。  
  
 FETCH { FIRST \| NEXT } { integer_constant \| fetch_row_count_expression } { ROW \| ROWS } ONLY   
 指定在处理 OFFSET 子句后返回的行数。 该值可以是大于或等于 1 的整数常量或表达式。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
 fetch_row_count_expression 可以是变量、参数或常量标量子查询  。 在使用子查询时，它无法引用在外部查询范围中定义的任何列。 也就是说，它无法与外部查询相关联。  
  
 FIRST 和 NEXT 是同义词，是为了与 ANSI 兼容而提供的。  
  
 ROW 和 ROWS 是同义词，是为了与 ANSI 兼容而提供的。  
  
 在查询执行计划中，将在 TOP 查询运算符的 Rows 或 Top 属性中显示偏移行数值   。  
  
## <a name="best-practices"></a>最佳实践  
 避免将 ORDER BY 子句中的整数指定为选择列表中的列位置表示形式 例如，虽然 `SELECT ProductID, Name FROM Production.Production ORDER BY 2` 等语句是有效的，但与指定实际列名相比，其他人并不容易理解该语句。 此外，对选择列表的更改（如更改列顺序或添加新列）需要修改 ORDER BY 子句，以避免出现意外结果。  
  
 在 SELECT TOP (N) 语句中，请始终使用 ORDER BY 子句  。 这是以可预知的方式指明哪些行受 TOP 影响的唯一方法。 有关详细信息，请参阅 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)。  
  
## <a name="interoperability"></a>互操作性  
 在与 SELECT...INTO 或 INSERT...SELECT 语句一起使用以从另一来源插入行时，ORDER BY 子句不能保证按指定的顺序插入这些行。  
  
 在视图中使用 OFFSET 和 FETCH 并不会更改该视图的 Updateability 属性。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 ORDER BY 子句中的列数没有限制；但是，在 ORDER BY 子句中指定的列的总大小不能超过 8,060 个字节。  
  
 不能在 ORDER BY 子句中使用 ntext、text、image、geography、geometry 和 xml 类型的列       。  
  
 order_by_expression 出现在排名函数中时，无法指定整数或常量  。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
 如果已在 FROM 子句中指定了表名的别名，则在 ORDER BY 子句中只能使用该别名来限定其列。  
  
 如果 SELECT 语句包含以下子句或运算符之一，则必须在选择列表中定义在 ORDER BY 子句中指定的列名和别名：  
  
-   UNION 运算符  
  
-   EXCEPT 运算符  
  
-   INTERSECT 运算符  
  
-   SELECT DISTINCT  
  
 此外，当语句包含 UNION、EXCEPT 或 INTERSECT 运算符时，必须在第一个（左侧）查询的选择列表中指定列名或列别名。  
  
 在使用 UNION、EXCEPT 或 INTERSECT 运算符的查询中，只允许在语句末尾使用 ORDER BY。 只有在顶级查询而不是子查询中指定了 UNION、EXCEPT 和 INTERSECT 时，此限制才适用。 请参阅后面的“示例”一节。  
  
 除非还指定了 TOP 子句或 OFFSET 和 FETCH 子句，否则，视图、内联函数、派生表和子查询中的 ORDER BY 子句无效。 在这些对象中使用 ORDER BY 时，该子句仅用于确定由 TOP 子句或 OFFSET 和 FETCH 子句返回的行。 ORDER BY 不保证在查询这些构造时得到有序结果，除非在查询本身中也指定了 ORDER BY。  
  
 索引视图或使用 CHECK OPTION 子句定义的视图中不支持 OFFSET 和 FETCH。  
  
 可以在允许 TOP 和 ORDER BY 的任何查询中使用 OFFSET 和 FETCH，但具有以下限制：  
  
-   OVER 子句不支持 OFFSET 和 FETCH。  
  
-   无法在 INSERT、UPDATE、MERGE 和 DELETE 语句中直接指定 OFFSET 和 FETCH，但可以在这些语句定义的子查询中指定 OFFSET 和 FETCH。 例如，在 INSERT INTO SELECT 语句中，可以在 SELECT 语句中指定 OFFSET 和 FETCH。  
  
-   在使用 UNION、EXCEPT 或 INTERSECT 运算符的查询中，只能在指定查询结果顺序的最终查询中指定 OFFSET 和 FETCH。  
  
-   TOP 不能与 OFFSET 和 FETCH 在同一个查询表达式（同一个查询作用域）中结合使用。  
  
## <a name="using-offset-and-fetch-to-limit-the-rows-returned"></a>使用 OFFSET 和 FETCH 限制返回的行数  
 建议您使用 OFFSET 和 FETCH 子句而不是 TOP 子句实现查询分页解决方案，并限制发送到客户端应用程序的行数。  
  
 如果将 OFFSET 和 FETCH 作为分页解决方案，则需要为返回到客户端应用程序的每“页”数据运行一次查询。 例如，要以 10 行为增量返回查询结果，您必须执行一次查询以返回 1-10 行，然后再次运行查询以返回 11- 20 行，依此类推。 每个查询都是独立的，不会以任何方式与其他查询相关联。 这意味着，与使用执行一次查询并在服务器上保持状态的游标不同，将由客户端应用程序负责跟踪状态。 若要使用 OFFSET 和 FETCH 在查询请求之间获得稳定的结果，必须满足以下条件：  
  
1.  查询使用的基础数据不能发生变化。 即，不会更新查询处理的行，也不会在单个事务中使用快照或可序列化事务隔离执行查询中的所有页面请求。 有关这些事务隔离级别的详细信息，请参阅 [SET TRANSACTION ISOLATION LEVEL (Transact-SQL)](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)。  
  
2.  ORDER BY 子句包含保证是唯一的列或列组合。  
  
 请参阅本主题后面的“示例”部分中的“在单个事务中运行多个查询”示例。  
  
 如果一致的执行计划在分页解决方案中至关重要，应考虑使用 OFFSET 和 FETCH 参数的 OPTIMIZE FOR 查询提示。 请参阅本主题后面的“示例”部分中的“指定 OFFSET 和 FETCH 值的表达式”。 有关 OPTIMIZE FOR 的详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
## <a name="examples"></a>示例  
  
|类别|作为特征的语法元素|  
|--------------|------------------------------|  
|[基本语法](#BasicSyntax)|ORDER BY|  
|[指定升序和降序](#SortOrder)|DESC • ASC|  
|[指定排序规则](#Collation)|COLLATE|  
|[指定条件顺序](#Case)|CASE 表达式|  
|[在排名函数中使用 ORDER BY](#Rank)|排名函数|  
|[限制返回的行数](#Offset)|OFFSET • FETCH|  
|[将 ORDER BY 与 UNION、EXCEPT 和 INTERSECT 一起使用](#Union)|UNION|  
  
###  <a name="basic-syntax"></a><a name="BasicSyntax"></a>基本语法  
 本节中的示例说明了使用最低要求的语法的 ORDER BY 子句的基本功能。  
  
#### <a name="a-specifying-a-single-column-defined-in-the-select-list"></a>A. 指定在选择列表中定义的单个列  
 以下示例按数值 `ProductID` 列对结果集进行顺序。 由于未指定特定的排序顺序，将使用默认顺序（升序）。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID;  
```  
  
#### <a name="b-specifying-a-column-that-is-not-defined-in-the-select-list"></a>B. 指定未在选择列表中定义的列  
 以下示例按未包含在选择列表中的列对结果集进行排序，但 FROM 子句中指定的表中定义了该列。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name, Color  
FROM Production.Product  
ORDER BY ListPrice;  
```  
  
#### <a name="c-specifying-an-alias-as-the-sort-column"></a>C. 将别名指定为排序列  
 以下示例将列别名 `SchemaName` 指定为排序顺序列。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT name, SCHEMA_NAME(schema_id) AS SchemaName  
FROM sys.objects  
WHERE type = 'U'  
ORDER BY SchemaName;  
```  
  
#### <a name="d-specifying-an-expression-as-the-sort-column"></a>D. 将表达式指定为排序列  
 以下示例将表达式作为排序列。 表达式是使用 DATEPART 函数定义的，以便按员工的雇用年份对结果集进行排序。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, JobTitle, HireDate  
FROM HumanResources.Employee  
ORDER BY DATEPART(year, HireDate);  
```  
  
###  <a name="specifying-ascending-and-descending-sort-order"></a><a name="SortOrder"></a>指定升序和降序排序顺序  
  
#### <a name="a-specifying-a-descending-order"></a>A. 指定降序  
 以下示例按 `ProductID` 数值列降序对结果集进行排序。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY ProductID DESC;  
```  
  
#### <a name="b-specifying-an-ascending-order"></a>B. 指定升序  
 以下示例按 `Name` 列升序对结果集进行排序。 字符按字母顺序排序，而不是数值顺序。 也就是说，10 排在 2 之前。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, Name FROM Production.Product  
WHERE Name LIKE 'Lock Washer%'  
ORDER BY Name ASC ;  
```  
  
#### <a name="c-specifying-both-ascending-and-descending-order"></a>C. 同时指定升序和降序  
 以下示例按两个列对结果集进行排序。 先按 `FirstName` 列升序对查询结果集进行排序，然后按 `LastName` 列降序进行排序。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'R%'  
ORDER BY FirstName ASC, LastName DESC ;  
```  
  
###  <a name="specifying-a-collation"></a><a name="Collation"></a>指定排序规则  
 以下示例说明如何在 ORDER BY 子句中指定排序规则以更改查询结果的返回顺序。 将创建一个表，其中包含一个使用不区分大小写和重音的排序规则定义的列。 插入一些具有不同大小写和重音的值。 由于未在 ORDER BY 子句中指定排序规则，在对值进行排序时，第一个查询将使用列排序规则。 在第二个查询中，在 ORDER BY 子句中指定了区分大小写和重音的排序规则，这将改变行的返回顺序。  
  
```sql
USE tempdb;  
GO  
CREATE TABLE #t1 (name NVARCHAR(15) COLLATE Latin1_General_CI_AI)  
GO  
INSERT INTO #t1 VALUES(N'Sánchez'),(N'Sanchez'),(N'sánchez'),(N'sanchez');  
  
-- This query uses the collation specified for the column 'name' for sorting.  
SELECT name  
FROM #t1  
ORDER BY name;  
-- This query uses the collation specified in the ORDER BY clause for sorting.  
SELECT name  
FROM #t1  
ORDER BY name COLLATE Latin1_General_CS_AS;  
```  
  
###  <a name="specifying-a-conditional-order"></a><a name="Case"></a>指定条件顺序  
 以下示例在 ORDER BY 子句中使用 CASE 表达式，以根据给定的列值有条件地确定行的排序顺序。 在第一个示例中，会计算 `SalariedFlag` 表中 `HumanResources.Employee` 列的值。 `SalariedFlag` 设置为 1 的员工将按 `BusinessEntityID` 以降序顺序返回。 `SalariedFlag` 设置为 0 的员工将按 `BusinessEntityID` 以升序顺序返回。 在第二个示例中，当 `TerritoryName` 列等于“United States”时，结果集会按 `CountryRegionName` 列排序，对于所有其他行则按 `CountryRegionName` 排序。  
  
```sql
SELECT BusinessEntityID, SalariedFlag  
FROM HumanResources.Employee  
ORDER BY CASE SalariedFlag WHEN 1 THEN BusinessEntityID END DESC  
        ,CASE WHEN SalariedFlag = 0 THEN BusinessEntityID END;  
GO  
```  
  
```sql
SELECT BusinessEntityID, LastName, TerritoryName, CountryRegionName  
FROM Sales.vSalesPerson  
WHERE TerritoryName IS NOT NULL  
ORDER BY CASE CountryRegionName WHEN 'United States' THEN TerritoryName  
         ELSE CountryRegionName END;  
```  
  
###  <a name="using-order-by-in-a-ranking-function"></a><a name="Rank"></a>在排名函数中使用 ORDER BY  
 以下示例在 ROW_NUMBER、RANK、DENSE_RANK 和 NTILE 排名函数中使用 ORDER BY 子句。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT p.FirstName, p.LastName  
    ,ROW_NUMBER() OVER (ORDER BY a.PostalCode) AS "Row Number"  
    ,RANK() OVER (ORDER BY a.PostalCode) AS "Rank"  
    ,DENSE_RANK() OVER (ORDER BY a.PostalCode) AS "Dense Rank"  
    ,NTILE(4) OVER (ORDER BY a.PostalCode) AS "Quartile"  
    ,s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL AND SalesYTD <> 0;  
```  
  
###  <a name="limiting-the-number-of-rows-returned"></a><a name="Offset"></a>限制返回的行数  
 以下示例使用 OFFSET 和 FETCH 限制查询返回的行数。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。  
  
#### <a name="a-specifying-integer-constants-for-offset-and-fetch-values"></a>A. 指定整数常量以提供 OFFSET 和 FETCH 值  
 以下示例将一个整数常量指定为 OFFSET 和 FETCH 子句的值。 第一个查询返回所有按 `DepartmentID` 列排序的行。 将此查询返回的结果与后面的两个查询的结果进行比较。 下一个查询使用 `OFFSET 5 ROWS` 子句跳过前 5 行，然后返回所有其余行。 最终查询使用 `OFFSET 0 ROWS` 子句从第一行开始，然后使用 `FETCH NEXT 10 ROWS ONLY` 将返回的行限制为排序的结果集中的 10 行。  
  
```sql
USE AdventureWorks2012;  
GO  
-- Return all rows sorted by the column DepartmentID.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID;  
  
-- Skip the first 5 rows from the sorted result set and return all remaining rows.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID OFFSET 5 ROWS;  
  
-- Skip 0 rows and return only the first 10 rows from the sorted result set.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID   
    OFFSET 0 ROWS  
    FETCH NEXT 10 ROWS ONLY;  
```  
  
#### <a name="b-specifying-variables-for-offset-and-fetch-values"></a>B. 指定变量以提供 OFFSET 和 FETCH 值  
 下面的示例声明 `@RowsToSkip` 和 `@FetchRows` 变量，并在 OFFSET 和 FETCH 子句中指定这些变量。  
  
```sql
USE AdventureWorks2012;  
GO  
-- Specifying variables for OFFSET and FETCH values    
DECLARE @RowsToSkip TINYINT = 2
      , @FetchRows TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @RowsToSkip ROWS   
    FETCH NEXT @FetchRows ROWS ONLY;  
```  
  
#### <a name="c-specifying-expressions-for-offset-and-fetch-values"></a>C. 指定表达式以提供 OFFSET 和 FETCH 值  
 下面的示例使用 `@StartingRowNumber - 1` 表达式指定 OFFSET 值，并使用 `@EndingRowNumber - @StartingRowNumber + 1` 表达式指定 FETCH 值。 另外，还指定了查询提示 OPTIMIZE FOR。 在编译和优化查询时，可以使用此提示为局部变量提供特定的值。 仅在查询优化期间使用该值，在查询执行期间不使用该值。 有关详细信息，请参阅[查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)。  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Specifying expressions for OFFSET and FETCH values      
DECLARE @StartingRowNumber TINYINT = 1  
      , @EndingRowNumber TINYINT = 8;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @EndingRowNumber - @StartingRowNumber + 1 ROWS ONLY  
OPTION ( OPTIMIZE FOR (@StartingRowNumber = 1, @EndingRowNumber = 20) );  
```  
  
#### <a name="d-specifying-a-constant-scalar-subquery-for-offset-and-fetch-values"></a>D. 指定常数标量子查询以提供 OFFSET 和 FETCH 值  
 以下示例使用常数标量子查询定义 FETCH 子句的值。 该子查询从 `PageSize` 表的 `dbo.AppSettings` 列中返回单个值。  
  
```sql
-- Specifying a constant scalar subquery  
USE AdventureWorks2012;  
GO  
CREATE TABLE dbo.AppSettings (AppSettingID INT NOT NULL, PageSize INT NOT NULL);  
GO  
INSERT INTO dbo.AppSettings VALUES(1, 10);  
GO  
DECLARE @StartingRowNumber TINYINT = 1;  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber ROWS   
    FETCH NEXT (SELECT PageSize FROM dbo.AppSettings WHERE AppSettingID = 1) ROWS ONLY;  
```  
  
#### <a name="e-running-multiple-queries-in-a-single-transaction"></a>E. 在单个事务中运行多个查询  
 以下示例说明一种实现分页解决方案的方法，以确保在查询的所有请求中返回稳定的结果。 查询在使用快照隔离级别的单个事务中执行，同时，ORDER BY 语句中指定的列确保列的唯一性。  
  
```sql
USE AdventureWorks2012;  
GO  
  
-- Ensure the database can support the snapshot isolation level set for the query.  
IF (SELECT snapshot_isolation_state FROM sys.databases WHERE name = N'AdventureWorks2012') = 0  
    ALTER DATABASE AdventureWorks2012 SET ALLOW_SNAPSHOT_ISOLATION ON;  
GO  
  
-- Set the transaction isolation level  to SNAPSHOT for this query.  
SET TRANSACTION ISOLATION LEVEL SNAPSHOT;  
GO  
  
-- Beginning the transaction.
BEGIN TRANSACTION;  
GO  
-- Declare and set the variables for the OFFSET and FETCH values.  
DECLARE @StartingRowNumber INT = 1  
      , @RowCountPerPage INT = 3;  
  
-- Create the condition to stop the transaction after all rows have been returned.  
WHILE (SELECT COUNT(*) FROM HumanResources.Department) >= @StartingRowNumber  
BEGIN  
  
-- Run the query until the stop condition is met.  
SELECT DepartmentID, Name, GroupName  
FROM HumanResources.Department  
ORDER BY DepartmentID ASC   
    OFFSET @StartingRowNumber - 1 ROWS   
    FETCH NEXT @RowCountPerPage ROWS ONLY;  
  
-- Increment @StartingRowNumber value.  
SET @StartingRowNumber = @StartingRowNumber + @RowCountPerPage;  
CONTINUE  
END;  
GO  
COMMIT TRANSACTION;  
GO  
```  
  
###  <a name="using-order-by-with-union-except-and-intersect"></a><a name="Union"></a>将 ORDER BY 与 UNION、EXCEPT 和 INTERSECT 一起使用  
 当查询使用 UNION、EXCEPT 或 INTERSECT 运算符时，必须在语句末尾指定 ORDER BY 子句，并对合并的查询结果进行排序。 以下示例返回所有红色或黄色的产品，并按 `ListPrice` 列对合并的列表进行排序。  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Red'  
-- ORDER BY cannot be specified here.  
UNION ALL  
SELECT Name, Color, ListPrice  
FROM Production.Product  
WHERE Color = 'Yellow'  
ORDER BY ListPrice ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例演示按 `EmployeeKey` 数值列升序对结果集进行排序。  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey;  
```  
  
 以下示例按 `EmployeeKey` 数值列降序对结果集进行排序。  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY EmployeeKey DESC;  
```  
  
 以下示例按 `LastName` 列对结果集进行排序。  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName;  
```  
  
 以下示例按照两列进行排序。 此查询首先按 `FirstName` 列以升序排序，然后按 `FirstName` 列以降序对常见 `LastName` 值降序排序。  
  
```sql
-- Uses AdventureWorks  
  
SELECT EmployeeKey, FirstName, LastName FROM DimEmployee  
WHERE LastName LIKE 'A%'  
ORDER BY LastName, FirstName;  
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
 [排名函数 (Transact-SQL)](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [TOP (Transact-SQL)](../../t-sql/queries/top-transact-sql.md)   
 [查询提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql-query.md)   
 [EXCEPT 和 INTERSECT (Transact-SQL)](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UNION (Transact-SQL)](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)  
  
  

