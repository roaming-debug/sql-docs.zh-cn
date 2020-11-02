---
description: SELECT (Transact-SQL)
title: SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SELECT_TSQL
- SELECT
dev_langs:
- TSQL
helpviewer_keywords:
- retrieving rows
- SELECT statement [SQL Server]
- SELECT statement [SQL Server], about SELECT statement
- row retrieval [SQL Server], SELECT statement
- DML [SQL Server], SELECT statement
- data manipulation language [SQL Server], SELECT statement
- row retrieval [SQL Server]
- queries [SQL Server], results
ms.assetid: dc85caea-54d1-49af-b166-f3aa2f3a93d0
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: faece054ca8f125e8c3594eb588ffa8cf97ddc16
ms.sourcegitcommit: 5f3e0eca9840db20038f0362e5d88a84ff3424af
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2020
ms.locfileid: "92344906"
---
# <a name="select-transact-sql"></a>SELECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从数据库中检索行，并允许从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的一个或多个表中选择一个或多个行或列。 虽然 SELECT 语句的完整语法较复杂，但其主要子句可归纳如下：  
  
[ WITH { [[ XMLNAMESPACES ,]](../../t-sql/xml/with-xmlnamespaces.md) [[ \<common_table_expression> ]](../../t-sql/queries/with-common-table-expression-transact-sql.md) } ]
  
 [SELECT *select_list*](../../t-sql/queries/select-clause-transact-sql.md) [ [INTO *new_table*](../../t-sql/queries/select-into-clause-transact-sql.md) ]  
  
 [ [FROM *table_source*](../../t-sql/queries/from-transact-sql.md) ] [ [WHERE *search_condition*](../../t-sql/queries/where-transact-sql.md) ]  
  
 [ [GROUP BY *group_by_expression*](../../t-sql/queries/select-group-by-transact-sql.md) ]  
  
 [ [HAVING *search_condition*](../../t-sql/queries/select-having-transact-sql.md) ]  
  
 [ [ORDER BY *order_expression* [ ASC | DESC ] ](../../t-sql/queries/select-order-by-clause-transact-sql.md)]  
  
 可在查询之间使用 [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md)、[EXCEPT 和 INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md) 运算符，以便将各个查询的结果合并到一个结果集中或进行比较。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<SELECT statement> ::=    
    [ WITH { [ XMLNAMESPACES ,] [ <common_table_expression> [,...n] ] } ]  
    <query_expression>   
    [ ORDER BY <order_by_expression> ] 
    [ <FOR Clause>]   
    [ OPTION ( <query_hint> [ ,...n ] ) ]   
<query_expression> ::=   
    { <query_specification> | ( <query_expression> ) }   
    [  { UNION [ ALL ] | EXCEPT | INTERSECT }  
        <query_specification> | ( <query_expression> ) [...n ] ]   
<query_specification> ::=   
SELECT [ ALL | DISTINCT ]   
    [TOP ( expression ) [PERCENT] [ WITH TIES ] ]   
    < select_list >   
    [ INTO new_table ]   
    [ FROM { <table_source> } [ ,...n ] ]   
    [ WHERE <search_condition> ]   
    [ <GROUP BY> ]   
    [ HAVING < search_condition > ]   
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
[ WITH <common_table_expression> [ ,...n ] ]  
SELECT <select_criteria>  
[;]  
  
<select_criteria> ::=  
    [ TOP ( top_expression ) ]   
    [ ALL | DISTINCT ]   
    { * | column_name | expression } [ ,...n ]   
    [ FROM { table_source } [ ,...n ] ]  
    [ WHERE <search_condition> ]   
    [ GROUP BY <group_by_clause> ]   
    [ HAVING <search_condition> ]   
    [ ORDER BY <order_by_expression> ]  
    [ OPTION ( <query_option> [ ,...n ] ) ]  
  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注  
 由于 SELECT 语句的复杂性，下面按子句说明详细的语法元素和参数：  

:::row:::
    :::column:::
        [WITH XMLNAMESPACES](../../t-sql/xml/with-xmlnamespaces.md)
    :::column-end:::
    :::column:::
        [HAVING](../../t-sql/queries/select-having-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WITH common_table_expression](../../t-sql/queries/with-common-table-expression-transact-sql.md)
    :::column-end:::
    :::column:::
        [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [SELECT 子句](../../t-sql/queries/select-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [EXCEPT 和 INTERSECT](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [INTO 子句](../../t-sql/queries/select-into-clause-transact-sql.md)
    :::column-end:::
    :::column:::
        [ORDER BY](../../t-sql/queries/select-order-by-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [FROM](../../t-sql/queries/from-transact-sql.md)
    :::column-end:::
    :::column:::
        [FOR 子句](../../t-sql/queries/select-for-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [WHERE](../../t-sql/queries/where-transact-sql.md)
    :::column-end:::
    :::column:::
        [OPTION 子句](../../t-sql/queries/option-clause-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [GROUP BY](../../t-sql/queries/select-group-by-transact-sql.md)
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

 SELECT 语句中的子句顺序非常重要。 可以省略可选子句，但这些子句在使用时必须按适当的顺序出现。  
  
 只有当这些语句的选择列表包含给函数的局部变量赋值的表达式时，在用户定义函数中才允许有 SELECT 语句。  
  
 对于由四部分组成的名称，若其中的服务器名称使用的是 OPENDATASOURCE 函数，则该名称可以在表名能够在 SELECT 语句内出现的任何位置作为表源使用。 无法为 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 指定由四部分组成的名称。  
  
 某些应用于 SELECT 语句的语法限制涉及到远程表。  
  
## <a name="logical-processing-order-of-the-select-statement"></a>SELECT 语句的逻辑处理顺序  
 以下步骤显示 SELECT 语句的逻辑处理顺序（即绑定顺序）。 此顺序确定在一个步骤中定义的对象何时可用于后续步骤中的子句。 例如，如果查询处理器可以绑定到（访问）在 FROM 子句中定义的表或视图，则这些对象及其列可用于所有后续步骤。 相反，因为 SELECT 子句处于步骤 8，所以，在该子句中定义的任何列别名或派生列都无法被之前的子句引用。 不过，它们可由 ORDER BY 子句之类的后续子句引用。 该语句的实际物理执行由查询处理器确定，因此顺序可能与此列表不同。  
  
1.  FROM  
2.  ON  
3.  JOIN  
4.  WHERE  
5.  GROUP BY  
6.  WITH CUBE 或 WITH ROLLUP  
7.  HAVING  
8.  SELECT  
9. DISTINCT  
10. ORDER BY  
11. TOP  

> [!WARNING]
> 上述顺序是常规顺序。 但是仍然存在不同于此顺序的少见情况。
>
> 例如，假设视图中具有聚集索引，该视图排除了一些表行，并且视图的 SELECT 列列表使用可将数据类型从 varchar 更改为 integer 的 CONVERT 。 在此情况下，CONVERT 可能会先于 WHERE 语句执行。 这种情况确实少见。 如果需要解决此问题，通常可通过某种方法修改视图来避免出现顺序不同的情况。 

## <a name="permissions"></a>权限  
 选择数据要求对表或视图具有 **SELECT** 权限，该权限可从更高级别的权限继承，例如针对架构的 **SELECT** 权限或针对表的 **CONTROL** 权限。 或者要求具有 db_datareader 或 db_owner 固定数据库角色或 sysadmin 固定服务器角色的成员身份  。 使用 SELECT INTO 创建新表既要求 CREATE TABLE 权限，还要求拥有新表的架构上的 ALTER SCHEMA 权限  。  
  
## <a name="examples"></a>示例：   
下面的示例使用 [!INCLUDE[ssawPDW](../../includes/ssawpdw-md.md)] 数据库。
  
### <a name="a-using-select-to-retrieve-rows-and-columns"></a>A. 使用 SELECT 检索行和列  
 本部分演示三个代码示例。 此第一个代码示例返回 `DimEmployee` 表中的所有行（未指定 WHERE 子句）和所有列（使用了 `*`）。  
  
```sql  
SELECT *  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
 此第二个示例使用表别名实现相同结果。  
  
```sql  
SELECT e.*  
FROM DimEmployee AS e  
ORDER BY LastName;  
```  
  
 此示例返回 `AdventureWorksPDW2012` 数据库的 `DimEmployee` 表中的所有行（未指定 WHERE 子句）以及列（`FirstName`、`LastName`、`StartDate`）的子集。 第三个列标题重命名为 `FirstDay`。  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
ORDER BY LastName;  
```  
  
 此示例仅返回 `DimEmployee` 的行，其中包含非 NULL 的 `EndDate` 和为“M”（已婚）的 `MaritalStatus`。  
  
```sql  
SELECT FirstName, LastName, StartDate AS FirstDay  
FROM DimEmployee   
WHERE EndDate IS NOT NULL   
AND MaritalStatus = 'M'  
ORDER BY LastName;  
```  
  
### <a name="b-using-select-with-column-headings-and-calculations"></a>B. 将 SELECT 与列标题和列计算一起使用  
 下面的示例返回 `DimEmployee` 表中的所有行，并基于每位员工的 `BaseRate` 和 40 小时工作周计算他们的总工资。  
  
```sql  
SELECT FirstName, LastName, BaseRate, BaseRate * 40 AS GrossPay  
FROM DimEmployee  
ORDER BY LastName;  
```  
  
### <a name="c-using-distinct-with-select"></a>C. 将 DISTINCT 与 SELECT 一起使用  
 下面的示例使用 `DISTINCT` 为 `DimEmployee` 表中的所有唯一标题生成列表。  
  
```sql  
SELECT DISTINCT Title  
FROM DimEmployee  
ORDER BY Title;  
```  
  
### <a name="d-using-group-by"></a>D. 使用 GROUP BY  
 下面的示例查找每天所有销售的总金额。  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
 由于使用了 `GROUP BY` 子句，因此每天只返回一行销售总额。  
  
### <a name="e-using-group-by-with-multiple-groups"></a>E. 对多个组使用 GROUP BY  
 下面的示例查找平均价格和每天的互联网销售总额（按订单日期和促销关键字进行分组）。  
  
```sql  

SELECT OrderDateKey, PromotionKey, AVG(SalesAmount) AS AvgSales, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey, PromotionKey  
ORDER BY OrderDateKey;   
```  
  
### <a name="f-using-group-by-and-where"></a>F. 使用 GROUP BY 和 WHERE  
 下面的示例在只检索订单日期晚于 2002 年 8 月 1 日的行后对结果进行分组。  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
WHERE OrderDateKey > '20020801'  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="g-using-group-by-with-an-expression"></a>G. 将 GROUP BY 与表达式一起使用  
 以下示例按表达式进行分组。 如果表达式不包含聚合函数，则可以按表达式进行分组。  
  
```sql  
SELECT SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY (OrderDateKey * 10);  
```  
  
### <a name="h-using-group-by-with-order-by"></a>H. 将 GROUP BY 与 ORDER BY 一起使用  
 下面的示例查找每天的总销售额以及当天的订单。  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
ORDER BY OrderDateKey;  
```  
  
### <a name="i-using-the-having-clause"></a>I. 使用 HAVING 子句  
 此查询使用 `HAVING` 子句限制结果。  
  
```sql  
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales  
FROM FactInternetSales  
GROUP BY OrderDateKey  
HAVING OrderDateKey > 20010000  
ORDER BY OrderDateKey;  
```  
  
## <a name="see-also"></a>另请参阅  
 [SELECT 示例 (Transact-SQL)](../../t-sql/queries/select-examples-transact-sql.md)  
 [提示 (Transact-SQL)](../../t-sql/queries/hints-transact-sql.md)
  

