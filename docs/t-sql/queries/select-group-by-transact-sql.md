---
description: SELECT - GROUP BY- Transact-SQL
title: GROUP BY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GROUP
- CUBE
- ROLLUP
- GROUPING_SETS_TSQL
- GROUP BY
- GROUPING SETS
- GROUP_BY_TSQL
- GROUP_TSQL
- CUBE_TSQL
- ROLLUP_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GROUP BY clause, about GROUP BY clause
- dividing tables into groups
- GROUPING SETS
- GROUP BY clause
- table groups [SQL Server]
- groups [SQL Server], tables divided into groups
- summary values [SQL Server]
ms.assetid: 40075914-6385-4692-b4a5-62fe44ae6cb6
author: shkale-msft
ms.author: shkale
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f4bbf0df92834b3bec1e7314edb8dfba305f734d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207578"
---
# <a name="select---group-by--transact-sql"></a>SELECT - GROUP BY- Transact-SQL
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

将查询结果划分为多个行组的 SELECT 语句子句，通常用于在每个组上执行一个或多个聚合。 SELECT 语句每组返回一行。  
  
## <a name="syntax"></a>语法  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database   
-- ISO-Compliant Syntax  
  
GROUP BY {
      column-expression  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
    | GROUPING SETS ( <grouping_set> [ ,...n ]  )  
    | () --calculates the grand total 
} [ ,...n ] 
 
<group_by_expression> ::=  
      column-expression  
    | ( column-expression [ ,...n ] )    
   
<grouping_set> ::=  
      () --calculates the grand total  
    | <grouping_set_item>  
    | ( <grouping_set_item> [ ,...n ] )  
  
<grouping_set_item> ::=  
      <group_by_expression>  
    | ROLLUP ( <group_by_expression> [ ,...n ] )  
    | CUBE ( <group_by_expression> [ ,...n ] )  
  

-- For backward compatibility only.
-- Non-ISO-Compliant Syntax for SQL Server and Azure SQL Database 
  
GROUP BY 
      [ ALL ] column-expression [ ,...n ] 
    | column-expression [ ,...n ] [ WITH { CUBE | ROLLUP } ]   

```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics 
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
    | ROLLUP ( <group_by_expression> [ ,...n ] ) 
} [ ,...n ]

```

```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
GROUP BY {
      column-name [ WITH (DISTRIBUTED_AGG) ]  
    | column-expression
} [ ,...n ]

```  
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 
### <a name="column-expression"></a>*column-expression*  
指定列或列上的非聚合计算。 此列可以属于表、派生表或视图。 列必须出现在 SELECT 语句的 FROM 子句中，但不要求出现在 SELECT 列表中。 

关于有效的表达式，请参阅[表达式](~/t-sql/language-elements/expressions-transact-sql.md)。    

列必须出现在 SELECT 语句的 FROM 子句中，但不要求出现在 SELECT 列表中。 但是，\<select> 列表中任何非聚合表达式中的每个表列或视图列都必须包含在 GROUP BY 列表中：  
  
允许使用下面的语句：  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA, ColumnB;  
SELECT ColumnA + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + ColumnB + constant FROM T GROUP BY ColumnA, ColumnB;  
```  
  
不允许使用下面的语句：  
  
```sql  
SELECT ColumnA, ColumnB FROM T GROUP BY ColumnA + ColumnB;  
SELECT ColumnA + constant + ColumnB FROM T GROUP BY ColumnA + ColumnB;  
```  

列表达式不能包含：

- SELECT 列表中定义的列别名。 它可以使用 FROM 子句中定义的派生表的列别名。
- text、ntext 或 image 类型的列  。 但是，可以使用 text、ntext 或 image 列作为返回有效数据类型值的函数的参数。 例如，表达式可以使用 SUBSTRING() 和 CAST()。 这也适用于 HAVING 子句中的表达式。
- xml 数据类型方法。 它可以包含使用 xml 数据类型方法的用户定义函数。 它可以包含使用 xml 数据类型方法的计算列。 
- 子查询。 返回错误 144。 
- 来自索引视图中的列。 
 
### <a name="group-by-column-expression--n-"></a>GROUP BY column-expression [ ,...n ]  

根据一个或多个列表达式列表中的值对 SELECT 语句结果进行分组。 

例如，此查询创建包含 Country、Region 和 Sales 列的销量表。 它插入四行，其中两行具有与 Country 和 Region 匹配的值。  

```sql
CREATE TABLE Sales ( Country VARCHAR(50), Region VARCHAR(50), Sales INT );

INSERT INTO sales VALUES (N'Canada', N'Alberta', 100);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 200);
INSERT INTO sales VALUES (N'Canada', N'British Columbia', 300);
INSERT INTO sales VALUES (N'United States', N'Montana', 100);
```
销量表包含这些行：

| 国家/地区 | 区域 | Sales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 200 |
| Canada | British Columbia | 300 |
| United States | Montana | 100 |

下一个查询对 Country 和 Region 分组，并返回每个值组合的总和。  
 
```sql 
SELECT Country, Region, SUM(sales) AS TotalSales
FROM Sales
GROUP BY Country, Region;
```
因为 Country 和 Region 的值有 3 种组合，所以查询结果有 3 行。 Canada 和 British Columbia 的 TotalSales 是两行的总和。 

| 国家/地区 | 区域 | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| United States | Montana | 100 |

### <a name="group-by-rollup"></a>GROUP BY ROLLUP

为每个列表达式的组合创建一个组。 此外，它将结果“汇总”到小计和总计。 为此，它会从右向左减少创建的组和聚合的列表达式的数量。 

列的顺序会影响 ROLLUP 的输出，而且可能会影响结果集内的行数。  

例如，`GROUP BY ROLLUP (col1, col2, col3, col4)` 为以下列表中的每个列表达式组合创建组。  

- col1、col2、col3、col4 
- col1、col2、col3、NULL
- col1、col2、NULL、NULL
- col1、NULL、NULL、NULL
- NULL、NULL、NULL、NULL - 这是总计

此代码使用前面示例中的表格，运行 GROUP BY ROLLUP 操作而不是简单的 GROUP BY。

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region);
```

查询结果与没有 ROLLUP 的简单 GROUP BY 具有相同的聚合。 此外，它为 Country 的每个值创建小计。 最后，它为所有行提供总计。 结果类似以下形式：

| 国家/地区 | 区域 | TotalSales |
| :------ | :----- | ---------: |
| Canada | Alberta | 100 |
| Canada | British Columbia | 500 |
| Canada | Null | 600 |
| United States | Montana | 100 |
| United States | Null | 100 |
| Null | Null | 700 |

### <a name="group-by-cube--"></a>GROUP BY CUBE ( )  

GROUP BY CUBE 为所有可能的列组合创建组。 对于 GROUP BY CUBE (a, b)，结果具有 (a, b)、(NULL, b)、(a, NULL) 和 (NULL, NULL) 唯一值的组。

此代码使用前面示例中的表格，对 Country 和 Region 运行 GROUP BY CUBE 操作。 

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

查询结果具有 (Country, Region)、(NULL, Region)、(Country, NULL) 和 (NULL, NULL) 的唯一值的组。 结果如下所示：

| 国家/地区 | 区域 | TotalSales |
|---------|--------|-------|
| Canada | Alberta | 100 |
| Null | Alberta | 100 |
| Canada | British Columbia | 500 |
| Null | British Columbia | 500 |
| United States | Montana | 100 |
| Null | Montana | 100 |
| Null | Null | 700
| Canada | Null | 600 |
| United States | Null | 100 |
   
 ### <a name="group-by-grouping-sets--"></a>GROUP BY GROUPING SETS ( )  
 
GROUPING SETS 选项可将多个 GROUP BY 子句组合到一个 GROUP BY 子句中。 其结果与针对指定的组执行 UNION ALL 运算等效。 

例如，`GROUP BY ROLLUP (Country, Region)` 和 `GROUP BY GROUPING SETS ( ROLLUP (Country, Region) )` 返回相同的结果。 

当 GROUPING SETS 具有两个或多个元素时，结果是元素的联合。 本示例返回 Country 和 Region 的 ROLLUP 和 CUBE 结果的联合。

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( ROLLUP (Country, Region), CUBE (Country, Region) );
```

结果与返回两个 GROUP BY 语句的联合的查询相同。

```sql
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY ROLLUP (Country, Region)
UNION ALL
SELECT Country, Region, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY CUBE (Country, Region);
```

SQL 不会合并为 GROUPING SETS 列表生成的重复组。 例如，在 `GROUP BY ( (), CUBE (Country, Region) )` 中，两个元素都返回总计行并且这两行都会列在结果中。 

 ### <a name="group-by-"></a>GROUP BY ()  
指定生成总计的空组。 这作为 GROUPING SET 的元素之一来说非常有用。 例如，此语句给出每个国家/地区的总销售额，然后给出了所有国家/地区的总和。

```sql
SELECT Country, SUM(Sales) AS TotalSales
FROM Sales
GROUP BY GROUPING SETS ( Country, () );
```

### <a name="group-by--all--column-expression--n-"></a>GROUP BY [ ALL ] column-expression [ ,...n ] 

适用对象：SQL Server 和 Azure SQL 数据库

注意：提供此语法的目的只是为了实现后向兼容性。 未来的版本中会将其删除。 请避免在新的开发工作中使用该语法，并考虑修改当前使用该语法的应用程序。

指定将所有组包含在结果中，无论它们是否满足 WHERE 子句中的搜索条件。 不满足搜索条件的组的聚合为 NULL。 

GROUP BY ALL：
- 如果在访问远程表的查询中还有 WHERE 子句，则该查询不支持 GROUP BY ALL。
- 对于具有 FILESTREAM 属性的列，GROUP BY ALL 将失败。
  
### <a name="with-distributed_agg"></a>WITH (DISTRIBUTED_AGG)
适用范围：[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

DISTRIBUTED_AGG 查询提示强制大规模并行处理 (MPP) 系统以在执行聚合之前重新分发特定列上的表。 GROUP BY 子句中只有一列可以拥有 DISTRIBUTED_AGG 查询提示。 查询完成后，重新分发的表被删除。 不会更改原始表格。  

注意：提供 DISTRIBUTED_AGG 查询提示的目的是为了实现与早期 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 版本的后向兼容性，对大多数查询而言，并不会提高其性能。 默认情况下，MPP 已根据需要重新分发数据以提高聚合的性能。 
  
## <a name="general-remarks"></a>一般备注

### <a name="how-group-by-interacts-with-the-select-statement"></a>GROUP BY 如何与 SELECT 语句进行交互
SELECT 列表：
- 矢量聚合。 如果 SELECT 列表中包含聚合函数，则 GROUP BY 将计算每组的汇总值。 这些函数称为矢量聚合。 
- Distinct 聚合。 ROLLUP、CUBE 和 GROUPING SETS 支持聚合 AVG (DISTINCT column_name)、COUNT (DISTINCT column_name) 和 SUM (DISTINCT column_name)  。
  
WHERE 子句：
- 执行任何分组操作之前，SQL 会删除不满足 WHERE 子句中条件的行。  
  
HAVING 子句：
- SQL 使用 having 子句来筛选结果集内的组。 
  
ORDER BY 子句：
- 使用 ORDER BY 子句可以对结果集进行排序。 GROUP BY 子句不能对结果集进行排序。 
  
NULL 值：
- 如果组合列包含 NULL 值，则所有的 NULL 值都将被视为相等，并会置入一个组中。   
  
## <a name="limitations-and-restrictions"></a>限制和局限

适用对象：SQL Server（从 2008 版开始）和 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]

### <a name="maximum-capacity"></a>最大容量

对于使用 ROLLUP、CUBE 或 GROUPING SETS 的 GROUP BY 子句，表达式的最大数量为 32。 组的最大数量为 4096 (2<sup>12</sup>)。 下面的示例中，由于 GROUP BY 子句的组超过 4096 个，因此这些示例将失败。  
 
-   下面的示例生成 4097 (2<sup>12</sup> + 1) 个分组集，将会失败。  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), b )  
    ```  
  
-   下面的示例生成 4097 (2<sup>12</sup> + 1) 个组，将会失败。 `CUBE ()` 和 `()` 分组集会生成一个总计行，而且将不删除重复的分组集。  
  
    ```sql
    GROUP BY GROUPING SETS( CUBE(a1, ..., a12), ())  
    ```  

-   此示例使用向后兼容语法。 它生成 8192 (2<sup>13</sup>) 个分组集，将会失败。  
  
    ```sql
    GROUP BY CUBE (a1, ..., a13)   
    GROUP BY a1, ..., a13 WITH CUBE   
    ```    
    对于不包含 CUBE 或 ROLLUP 的向后兼容 GROUP BY 子句，group by 的项数受查询所涉及的 GROUP BY 列的大小、聚合列和聚合值的限制。 该限制从 8,060 字节的限制开始，对保存中间查询结果所需的中间级工作表有 8,060 字节的限制。 如果指定了 CUBE 或 ROLLUP，则最多只能有 12 个分组表达式。

### <a name="support-for-iso-and-ansi-sql-2006-group-by-features"></a>对 ISO 和 ANSI SQL-2006 GROUP BY 功能的支持

除了下面的语法，GROUP BY 子句支持 SQL-2006 标准中包括的所有 GROUP BY 功能：  
  
-   不允许在 GROUP BY 子句中使用分组集，除非它们是显式 GROUPING SETS 列表的一部分。 例如，在该标准中允许使用 `GROUP BY Column1, (Column2, ...ColumnN`)，但在 Transact-SQL 中不允许使用。  Transact-SQL 支持 `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))` 和 `GROUP BY Column1, Column2, ... ColumnN`，它们在语义上等效。 这些示例与上面的 `GROUP BY` 示例在语义上等效。 这是为了避免 `GROUP BY Column1, (Column2, ...ColumnN` 被误解为 `GROUP BY C1, GROUPING SETS ((Column2, ...ColumnN))`，它们在语义上不等效。  
  
-   不允许在分组集内部使用分组集。 例如，在 SQL-2006 标准中允许使用 `GROUP BY GROUPING SETS (A1, A2,...An, GROUPING SETS (C1, C2, ...Cn))`，但在 Transact-SQL 中不允许使用。 Transact-SQL 允许使用 `GROUP BY GROUPING SETS( A1, A2,...An, C1, C2, ...Cn )` 或 `GROUP BY GROUPING SETS( (A1), (A2), ... (An), (C1), (C2), ... (Cn) )`，它们与第一个 GROUP BY 示例在语义上等效，但其语法更清楚。  
  
-   仅允许在包含列表达式的简单 GROUP BY 子句中使用 GROUP BY [ALL/DISTINCT]。 不允许与 GROUPING SETS、ROLLUP、CUBE、WITH CUBE 或 WITH ROLLUP 构造一起使用。 ALL 是隐式默认值。 仅允许用在向后兼容语法中使用。
  
### <a name="comparison-of-supported-group-by-features"></a>对支持的 GROUP BY 功能的比较  
 下表描述了不同的 SQL 版本以及数据库兼容级别支持的 GROUP BY 功能。  
  
|Feature|SQL Server Integration Services|SQL Server 兼容级别 100 或更高|SQL Server 2008 或兼容级别为 90 的更高版本。|  
|-------------|-------------------------------------|--------------------------------------------------|-----------------------------------------------------------|  
|DISTINCT 聚合|WITH CUBE 或 WITH ROLLUP 不支持。|WITH CUBE、WITH ROLLUP、GROUPING SETS、CUBE 或 ROLLUP 支持。|与兼容级别 100 相同。|  
|GROUP BY 子句中具有 CUBE 或 ROLLUP 名称的用户定义函数|GROUP BY 子句中允许使用用户定义函数 dbo.cube(arg1,...argN) 或 dbo.rollup(arg1,...argN)  。<br /><br /> 例如： `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|GROUP BY 子句中不允许使用用户定义函数 dbo.cube (arg1,...argN) 或 dbo.rollup(arg1,...argN)   。<br /><br /> 例如： `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`<br /><br /> 返回以下错误消息：“Incorrect syntax near the keyword 'cube'&#124;'rollup'.”（关键字 'cube''&#124;'rollup' 附近有语法错误。）<br /><br /> 为了避免出现此问题，请将 `dbo.cube` 替换为 `[dbo].[cube]` 或将 `dbo.rollup` 替换为 `[dbo].[rollup]`。<br /><br /> 允许使用下面的示例：`SELECT SUM (x) FROM T  GROUP BY [dbo].[cube](y);`|GROUP BY 子句中允许使用用户定义函数 dbo.cube(arg1,...argN) 或 dbo.rollup(arg1,...argN)<br /><br /> 例如： `SELECT SUM (x) FROM T  GROUP BY dbo.cube(y);`|  
|GROUPING SETS|不支持|支持|支持|  
|CUBE|不支持|支持|不支持|  
|ROLLUP|不支持|支持|不支持|  
|总计，如 GROUP BY ()|不支持|支持|支持|  
|GROUPING_ID 函数|不支持|支持|支持|  
|GROUPING 函数|支持|支持|支持|  
|WITH CUBE|支持|支持|支持|  
|WITH ROLLUP|支持|支持|支持|  
|WITH CUBE 或 WITH ROLLUP“重复”分组删除|支持|支持|支持| 
 
  
## <a name="examples"></a>示例  
  
### <a name="a-use-a-simple-group-by-clause"></a>A. 使用简单 GROUP BY 子句  
 以下示例检索 `SalesOrderID` 表中各 `SalesOrderDetail` 的总数。 本示例使用 AdventureWorks。  
  
```sql  
SELECT SalesOrderID, SUM(LineTotal) AS SubTotal  
FROM Sales.SalesOrderDetail AS sod  
GROUP BY SalesOrderID  
ORDER BY SalesOrderID;  
```  
  
### <a name="b-use-a-group-by-clause-with-multiple-tables"></a>B. 将 GROUP BY 子句用于多个表  
 下面的示例检索与 `City` 表联接的 `Address` 表中的各 `EmployeeAddress` 的雇员数。 本示例使用 AdventureWorks。 
  
```sql  
SELECT a.City, COUNT(bea.AddressID) EmployeeCount  
FROM Person.BusinessEntityAddress AS bea   
    INNER JOIN Person.Address AS a  
        ON bea.AddressID = a.AddressID  
GROUP BY a.City  
ORDER BY a.City;  
```  
  
### <a name="c-use-a-group-by-clause-with-an-expression"></a>C. 将 GROUP BY 子句用于表达式  
 以下示例使用 `DATEPART` 函数检索每年的销售总额。 `SELECT` 列表和 `GROUP BY` 子句中必须存在相同的表达式。  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
### <a name="d-use-a-group-by-clause-with-a-having-clause"></a>D. 将 GROUP BY 子句与 HAVING 子句一起使用  
 下面的示例使用 `HAVING` 子句来指定应当将 `GROUP BY` 子句中生成的哪个组包括在结果集内。  
  
```sql  
SELECT DATEPART(yyyy,OrderDate) AS N'Year'  
    ,SUM(TotalDue) AS N'Total Order Amount'  
FROM Sales.SalesOrderHeader  
GROUP BY DATEPART(yyyy,OrderDate)  
HAVING DATEPART(yyyy,OrderDate) >= N'2003'  
ORDER BY DATEPART(yyyy,OrderDate);  
```  
  
## <a name="examples-azure-synapse-analytics-and-parallel-data-warehouse"></a>示例：Azure Synapse Analytics 和并行数据仓库  
  
### <a name="e-basic-use-of-the-group-by-clause"></a>E. GROUP BY 子句的基本用法  
 下面的示例查找每天所有销售的总金额。 返回每天包含所有销售额的总金额。  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales FROM FactInternetSales  
GROUP BY OrderDateKey ORDER BY OrderDateKey;  
```  
  
### <a name="f-basic-use-of-the-distributed_agg-hint"></a>F. DISTRIBUTED_AGG 提示的基本用法  
 此示例使用 DISTRIBUTED_AGG 查询提示强制设备在执行聚合之前对 `CustomerKey` 列上的表随意排布。  
  
```sql  
-- Uses AdventureWorksDW  
  
SELECT CustomerKey, SUM(SalesAmount) AS sas  
FROM FactInternetSales  
GROUP BY CustomerKey WITH (DISTRIBUTED_AGG)  
ORDER BY CustomerKey DESC;  
```  
  
### <a name="g-syntax-variations-for-group-by"></a>G. GROUP BY 的语法变体  
 SELECT 列表中没有聚合时，SELECT 列表中的每一列都必须包括在 GROUP BY 列表中。 SELECT 列表中的计算列可以在 GROUP BY 列表中列出，但不是必需的。 下面是语法上有效的 SELECT 语句的示例：  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName FROM DimCustomer GROUP BY LastName, FirstName;  
SELECT NumberCarsOwned FROM DimCustomer GROUP BY YearlyIncome, NumberCarsOwned;  
SELECT (SalesAmount + TaxAmt + Freight) AS TotalCost FROM FactInternetSales GROUP BY SalesAmount, TaxAmt, Freight;  
SELECT SalesAmount, SalesAmount*1.10 SalesTax FROM FactInternetSales GROUP BY SalesAmount;  
SELECT SalesAmount FROM FactInternetSales GROUP BY SalesAmount, SalesAmount*1.10;  
```  
  
### <a name="h-using-a-group-by-with-multiple-group-by-expressions"></a>H. 将 GROUP BY 与多个 GROUP BY 表达式一起使用  
 下面的示例使用多个 `GROUP BY` 条件对结果进行分组。 如果在每个 `OrderDateKey` 组中有可以通过 `DueDateKey` 进行区分的子组，则将为结果集定义一个新分组。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, DueDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales
GROUP BY OrderDateKey, DueDateKey   
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-a-group-by-clause-with-a-having-clause"></a>I. 将 GROUP BY 子句与 HAVING 子句一起使用  
 下面的示例使用 `HAVING` 子句来指定应当包含在结果集中的 `GROUP BY` 子句中生成的组。 仅具有 2004 年或以后的订单日期的组才会包含在结果中。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales   
FROM FactInternetSales  
GROUP BY OrderDateKey   
HAVING OrderDateKey > 20040000   
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>另请参阅  
 [GROUPING_ID (Transact-SQL)](~/t-sql/functions/grouping-id-transact-sql.md)   
 [GROUPING (Transact-SQL)](~/t-sql/functions/grouping-transact-sql.md)   
 [SELECT (Transact-SQL)](~/t-sql/queries/select-transact-sql.md)   
 [SELECT 子句 (Transact-SQL)](~/t-sql/queries/select-clause-transact-sql.md)  
  
  




