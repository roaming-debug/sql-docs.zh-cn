---
title: OVER Clause (Transact-SQL) | Microsoft Docs
description: OVER 子句的 Transact-SQL 参考，该函数定义查询结果集内的用户指定的行集。
ms.date: 08/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OVER_TSQL
- OVER
dev_langs:
- TSQL
helpviewer_keywords:
- order of rowsets [SQL Server]
- rowsets [SQL Server], windowing
- window function
- partitions [SQL Server], rowsets
- clauses [SQL Server], OVER
- rowsets [SQL Server], partitioning
- rowsets [SQL Server], ordering
- OVER clause
ms.assetid: ddcef3a6-0341-43e0-ae73-630484b7b398
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3b78f603517e6f754039d66572c88ea77b81098a
ms.sourcegitcommit: c83c17e44b5e1e3e2a3b5933c2a1c4afb98eb772
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2021
ms.locfileid: "100525221"
---
# <a name="select---over-clause-transact-sql"></a>SELECT - OVER 子句 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在应用关联的开窗函数前确定行集的分区和排序。 也就是说，OVER 子句定义查询结果集内的窗口或用户指定的行集。 然后，开窗函数将计算窗口中每一行的值。 您可以将 OVER 子句与函数一起使用，以便计算各种聚合值，例如移动平均值、累积聚合、运行总计或每组结果的前 N 个结果。  
  
-   [排名函数](../../t-sql/functions/ranking-functions-transact-sql.md)  
  
-   [聚合函数](../../t-sql/functions/aggregate-functions-transact-sql.md)  
  
-   [分析函数](../../t-sql/functions/analytic-functions-transact-sql.md)  
  
-   [NEXT VALUE FOR 函数](../../t-sql/functions/next-value-for-transact-sql.md)  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure Synapse Analytics  
  
OVER (   
       [ <PARTITION BY clause> ]  
       [ <ORDER BY clause> ]   
       [ <ROW or RANGE clause> ]  
      )  
  
<PARTITION BY clause> ::=  
PARTITION BY value_expression , ... [ n ]  
  
<ORDER BY clause> ::=  
ORDER BY order_by_expression  
    [ COLLATE collation_name ]   
    [ ASC | DESC ]   
    [ ,...n ]  
  
<ROW or RANGE clause> ::=  
{ ROWS | RANGE } <window frame extent>  
  
<window frame extent> ::=   
{   <window frame preceding>  
  | <window frame between>  
}  
<window frame between> ::=   
  BETWEEN <window frame bound> AND <window frame bound>  
  
<window frame bound> ::=   
{   <window frame preceding>  
  | <window frame following>  
}  
  
<window frame preceding> ::=   
{  
    UNBOUNDED PRECEDING  
  | <unsigned_value_specification> PRECEDING  
  | CURRENT ROW  
}  
  
<window frame following> ::=   
{  
    UNBOUNDED FOLLOWING  
  | <unsigned_value_specification> FOLLOWING  
  | CURRENT ROW  
}  
  
<unsigned value specification> ::=   
{  <unsigned integer literal> }  
  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
OVER ( [ PARTITION BY value_expression ] [ order_by_clause ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

开窗函数在其 `OVER` 子句中具有以下参数：
- [PARTITION BY](#partition-by)：将查询结果集分为多个分区。
- [ORDER BY](#order-by)：定义结果集的每个分区中行的逻辑顺序。 
- [ROWS/RANGE](#rows-or-range)：通过指定分区中的起点和终点来限制分区中的行数。 它需要 `ORDER BY` 参数，如果指定了 `ORDER BY` 参数，则默认值是从分区起点到当前元素。

如果未指定任何参数，则将对整个结果集应用开窗函数。
```sql
select 
      object_id
    , [min] = min(object_id) over()
    , [max] = max(object_id) over()
from sys.objects
```
 
|object_id | 分钟 | max |
|---|---|---|
| 3 | 3 | 2139154666 |
| 5 | 3 | 2139154666 |
| ... | ... | ... |
| 2123154609 |  3 | 2139154666 |
| 2139154666 |  3 | 2139154666 |

### <a name="partition-by"></a>PARTITION BY  
 将查询结果集分为多个分区。 开窗函数分别应用于每个分区，并为每个分区重新启动计算。  

```syntaxsql
PARTITION BY *value_expression* 
```
 
 如果未指定 PARTITION BY，则此函数将查询结果集的所有行视为单个分区。
如果未指定 `ORDER BY` 子句，则将对分区中的所有行应用函数。
  
#### <a name="partition-by-value_expression"></a>PARTITION BY value_expression   
 指定行集按其分区的列。 value_expression 只能引用可供 FROM 子句使用的列。 value_expression 不能引用选择列表中的表达式或别名。 value_expression 可以是列表达式、标量子查询、标量函数或用户定义的变量。 
 
 ```sql
 select 
      object_id, type
    , [min] = min(object_id) over(partition by type)
    , [max] = max(object_id) over(partition by type)
from sys.objects
```

|object_id | type | 分钟 | max |
|---|---|---|---|
| 68195293  | PK    | 68195293  | 711673583 |
| 631673298 | PK    | 68195293  | 711673583 |
| 711673583 | PK    | 68195293  | 711673583 |
| ... | ... | ... |
| 3 | S | 3 | 98 |
| 5 | S |   3   | 98 |
| ... | ... | ... |
| 98    | S |   3   | 98 |
| ... | ... | ... |
  
### <a name="order-by"></a>ORDER BY  

```syntaxsql
ORDER BY *order_by_expression* [COLLATE *collation_name*] [ASC|DESC]  
```

 定义结果集的每个分区中行的逻辑顺序。 也就是说，它指定按其执行开窗函数计算的逻辑顺序。 
 - 如果未指定，则默认顺序为 `ASC`，且开窗函数将使用分区中的所有行。
 - 如果指定了此参数，但未指定 ROWS/RANGE，则默认值 `RANGE UNBOUNDED PRECEDING AND CURRENT ROW` 将由可接受可选 ROWS/RANGE 规范（例如 `min` 或 `max`）的函数作为窗口帧的默认值。 
 
```sql
select 
      object_id, type
    , [min] = min(object_id) over(partition by type order by object_id)
    , [max] = max(object_id) over(partition by type order by object_id)
from sys.objects
```

|object_id | type | 分钟 | max |
|---|---|---|---|
| 68195293  | PK    | 68195293  | 68195293 |
| 631673298 | PK    | 68195293  | 631673298 |
| 711673583 | PK    | 68195293  | 711673583 |
| ... | ... | ... |
| 3 | S | 3 | 3 |
| 5 | S |   3 | 5 |
| 6 | S |   3 | 6 |
| ... | ... | ... |
| 97    | S |   3 | 97 |
| 98    | S |   3 | 98 |
| ... | ... | ... |


 order_by_expression  
 指定用于进行排序的列或表达式。 order_by_expression 只能引用可供 FROM 子句使用的列。 不能将整数指定为表示列名或别名。  
  
 COLLATE collation_name  
 指定应该根据在 collation_name 中指定的排序规则执行 ORDER BY 操作。 collation_name 既可以是 Windows 排序规则名称，也可以是 SQL 排序规则名称。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。 COLLATE 仅适用于 char、nchar、varchar 和 nvarchar 类型的列   。  
  
 **ASC** | DESC  
 指定按升序或降序排列指定列中的值。 ASC 是默认排序顺序。 Null 值被视为最低的可能值。  
  
### <a name="rows-or-range"></a>ROWS 或 RANGE  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。 
  
 通过指定分区中的起点和终点，进一步限制分区中的行数。 这是通过按照逻辑关联或物理关联对当前行指定某一范围的行实现的。 物理关联通过使用 ROWS 子句实现。  
  
 ROWS 子句通过指定当前行之前或之后的固定数目的行，限制分区中的行数。 此外，RANGE 子句通过指定针对当前行中的值的某一范围的值，从逻辑上限制分区中的行数。 基于 ORDER BY 子句中的顺序对之前和之后的行进行定义。 窗口框架“RANGE …CURRENT ROW ...”包括在 ORDER BY 表达式中与当前行具有相同值的所有行。 例如，ROWS BETWEEN 2 PRECEDING AND CURRENT ROW 意味着该函数对其操作的行的窗口在大小上是 3 行，开头为之前的 2 行，再包括当前行。  
 
```sql
select
      object_id
    , [preceding]   = count(*) over(order by object_id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW )
    , [central] = count(*) over(order by object_id ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING )
    , [following]   = count(*) over(order by object_id ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
from sys.objects
order by object_id asc
```

|object_id | 前面 | 中心 | 后面 |
|---|---|---|---|
| 3 | 1 | 3 | 156 |
| 5 | 2 | 4 | 155 |
| 6 | 3 | 5 | 154 |
| 7 | 4 | 5 | 153 |
| 8 | 5 | 5 | 152 |
| ...   | ...   | ...   | ... |
| 2112726579    | 153   | 5 | 4 |
| 2119678599    | 154   | 5 | 3 |
| 2123154609    | 155   | 4 | 2 |
| 2139154666    | 156   | 3 | 1 | 
  
> [!NOTE]  
>  ROWS 或 RANGE 要求指定 ORDER BY 子句。 如果 ORDER BY 包含多个顺序表达式，则 CURRENT ROW FOR RANGE 在确定当前行时将考虑 ORDER BY 列表中的所有列。  
  
#### <a name="unbounded-preceding"></a>UNBOUNDED PRECEDING  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 指定窗口在分区中的第一行开始。 UNBOUNDED PRECEDING 只能指定为窗口起点。  
  
 \<unsigned value specification> PRECEDING  
 使用 \<unsigned value specification> 指定，用以指示要置于当前行之前的行或值的数目。 对于 RANGE 则不允许这样指定。  
  
#### <a name="current-row"></a>CURRENT ROW  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。 
  
 在与 ROWS 一起使用时指定窗口在当前行开始或结束，或者在与 RANGE 一起使用时指定当前值。 CURRENT ROW 可指定为既是起点，又是终点。  
  
#### <a name="between-and"></a>BETWEEN AND  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。 
  
```syntaxsql
BETWEEN <window frame bound > AND <window frame bound >  
```
 与 ROWS 或 RANGE 一起使用，以便指定窗口的下（开始）边界和上（结束）边界点。 \<window frame bound> 定义边界起点，\<window frame bound> 定义边界终点。 上限不能小于下限。  
  
#### <a name="unbounded-following"></a>UNBOUNDED FOLLOWING  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。 
  
 指定窗口在分区的最后一行结束。 UNBOUNDED FOLLOWING 只能指定为窗口终点。 例如，RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING 定义以当前行开始、以分区的最后一行结束的窗口。  
  
 \<unsigned value specification> FOLLOWING  
 使用 \<unsigned value specification> 指定，用以指示要置于当前行之后的行或值的数目。 当 \<unsigned value specification> FOLLOWING 指定为窗口起点时，终点必须为 \<unsigned value specification>FOLLOWING。 例如，ROWS BETWEEN 2 FOLLOWING AND 10 FOLLOWING 定义一个窗口，该窗口以跟随在当前行之后的第二行开头、以跟随在当前行之后的第十行结尾。 对于 RANGE 则不允许这样指定。  
  
 无符号整数文字  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 一个正整数文字（包括 0），它指定要置于当前行或值之前或之后的行或值的数目。 这一指定仅对于 ROWS 有效。  
  
## <a name="general-remarks"></a>一般备注  
 可以在单个查询中将多个开窗函数与单个 FROM 子句一起使用。 每个函数的 OVER 子句在分区和排序上可能不同。  
  
 如果未指定 PARTITION BY，则此函数将查询结果集的所有行视为单个组。 
 
### <a name="important"></a>重要说明！

如果指定 ROWS/RANGE 并且 \<window frame preceding> 用于 \<window frame extent>（简短语法），则这一指定用于窗口框架边界起点并且 CURRENT ROW 用于边界终点。 例如，“ROWS 5 PRECEDING”等于“ROWS BETWEEN 5 PRECEDING AND CURRENT ROW”。  
  
> [!NOTE]
> 如果未指定 ORDER BY，则整个分区将用于窗口框架。 这仅适用于不要求 ORDER BY 子句的函数。 如果未指定 ROWS/RANGE，但指定了 ORDER BY，则将 RANGE UNBOUNDED PRECEDING AND CURRENT ROW 用作窗口框架的默认值。 这仅适用于可接受可选 ROWS/RANGE 指定的函数。 例如，排名函数无法接受 ROWS/RANGE，因此，此窗口框架不适用，甚至在存在 ORDER BY 而不存在 ROWS/RANGE 时也是如此。  
    
## <a name="limitations-and-restrictions"></a>限制和局限  
 OVER 子句不能与 CHECKSUM 聚合函数结合使用。  
  
 RANGE 不能用于 \<unsigned value specification> PRECEDING 或 \<unsigned value specification> FOLLOWING。  
  
 根据用于 OVER 子句的排名、聚合或分析函数，可能不支持 \<ORDER BY clause> 和/或 \<ROWS and RANGE clause>。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-over-clause-with-the-row_number-function"></a>A. 将 OVER 子句与 ROW_NUMBER 函数结合使用  
 下面的示例说明如何将 OVER 子句与 ROW_NUMBER 函数一起使用来显示分区内各行的行号。 在 OVER 子句中指定的 ORDER BY 子句按 `SalesYTD` 列对每个分区中的行进行排序。 SELECT 语句中的 ORDER BY 子句确定按其返回整个查询结果集的顺序。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ROW_NUMBER() OVER(PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS "Row Number",   
    p.LastName, s.SalesYTD, a.PostalCode  
FROM Sales.SalesPerson AS s   
    INNER JOIN Person.Person AS p   
        ON s.BusinessEntityID = p.BusinessEntityID  
    INNER JOIN Person.Address AS a   
        ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0  
ORDER BY PostalCode;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Row Number      LastName                SalesYTD              PostalCode 
 --------------- ----------------------- --------------------- ---------- 
 1               Mitchell                4251368.5497          98027 
 2               Blythe                  3763178.1787          98027 
 3               Carson                  3189418.3662          98027 
 4               Reiter                  2315185.611           98027 
 5               Vargas                  1453719.4653          98027  
 6               Ansman-Wolfe            1352577.1325          98027  
 1               Pak                     4116871.2277          98055  
 2               Varkey Chudukatil       3121616.3202          98055  
 3               Saraiva                 2604540.7172          98055  
 4               Ito                     2458535.6169          98055  
 5               Valdez                  1827066.7118          98055  
 6               Mensa-Annan             1576562.1966          98055  
 7               Campbell                1573012.9383          98055  
 8               Tsoflias                1421810.9242          98055
 ```  
  
### <a name="b-using-the-over-clause-with-aggregate-functions"></a>B. 将 OVER 子句与聚合函数结合使用  
 下面的示例对于查询返回的所有行将 `OVER` 子句与聚合函数一起使用。 在这个示例中，使用 `OVER` 子句与使用子查询相比，可以更高效地派生聚合值。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,AVG(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Avg"  
    ,COUNT(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Count"  
    ,MIN(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Min"  
    ,MAX(OrderQty) OVER(PARTITION BY SalesOrderID) AS "Max"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
SalesOrderID ProductID   OrderQty Total       Avg         Count       Min    Max  
------------ ----------- -------- ----------- ----------- ----------- ------ ------  
43659        776         1        26          2           12          1      6  
43659        777         3        26          2           12          1      6  
43659        778         1        26          2           12          1      6  
43659        771         1        26          2           12          1      6  
43659        772         1        26          2           12          1      6  
43659        773         2        26          2           12          1      6  
43659        774         1        26          2           12          1      6  
43659        714         3        26          2           12          1      6  
43659        716         1        26          2           12          1      6  
43659        709         6        26          2           12          1      6  
43659        712         2        26          2           12          1      6  
43659        711         4        26          2           12          1      6  
43664        772         1        14          1           8           1      4  
43664        775         4        14          1           8           1      4  
43664        714         1        14          1           8           1      4  
43664        716         1        14          1           8           1      4  
43664        777         2        14          1           8           1      4  
43664        771         3        14          1           8           1      4  
43664        773         1        14          1           8           1      4  
43664        778         1        14          1           8           1      4  
```  
  
 以下示例显示在计算所得值中将 `OVER` 子句与聚合函数结合使用。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT SalesOrderID, ProductID, OrderQty  
    ,SUM(OrderQty) OVER(PARTITION BY SalesOrderID) AS Total  
    ,CAST(1. * OrderQty / SUM(OrderQty) OVER(PARTITION BY SalesOrderID)   
        *100 AS DECIMAL(5,2))AS "Percent by ProductID"  
FROM Sales.SalesOrderDetail   
WHERE SalesOrderID IN(43659,43664);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]注意，聚合由 `SalesOrderID` 计算，并会为每个 `Percent by ProductID` 的每一行计算 `SalesOrderID`（ProductID 的百分比）。  
  
```  
SalesOrderID ProductID   OrderQty Total       Percent by ProductID  
------------ ----------- -------- ----------- ---------------------------------------  
43659        776         1        26          3.85  
43659        777         3        26          11.54  
43659        778         1        26          3.85  
43659        771         1        26          3.85  
43659        772         1        26          3.85  
43659        773         2        26          7.69  
43659        774         1        26          3.85  
43659        714         3        26          11.54  
43659        716         1        26          3.85  
43659        709         6        26          23.08  
43659        712         2        26          7.69  
43659        711         4        26          15.38  
43664        772         1        14          7.14  
43664        775         4        14          28.57  
43664        714         1        14          7.14  
43664        716         1        14          7.14  
43664        777         2        14          14.29  
43664        771         3        14          21.4  
43664        773         1        14          7.14  
43664        778         1        14          7.14  
  
 (20 row(s) affected)  
```  
  
### <a name="c-producing-a-moving-average-and-cumulative-total"></a>C. 生成移动平均值和累计合计  
 下面的示例将 AVG 和 SUM 函数与 OVER 子句结合使用，以便为 `Sales.SalesPerson` 表中的每个地区提供年度销售额的累计合计。 数据按 `TerritoryID` 分区并在逻辑上按 `SalesYTD` 排序。 这意味着，将基于年度销售额为各区域计算 AVG 函数。 请注意，对于 `TerritoryID` 1，为 2005 销售年度存在两行，分别表示在该年度有销售业绩的两个销售人员。 将计算这两行的平均销售额，然后在计算中包括表示 2006 年销售额的第三行。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS  SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                           ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                            ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY TerritoryID,SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           559,697.56           559,697.56  
287              NULL        2006        519,905.93           539,801.75           1,079,603.50  
285              NULL        2007        172,524.45           417,375.98           1,252,127.95  
283              1           2005        1,573,012.94         1,462,795.04         2,925,590.07  
280              1           2005        1,352,577.13         1,462,795.04         2,925,590.07  
284              1           2006        1,576,562.20         1,500,717.42         4,502,152.27  
275              2           2005        3,763,178.18         3,763,178.18         3,763,178.18  
277              3           2005        3,189,418.37         3,189,418.37         3,189,418.37  
276              4           2005        4,251,368.55         3,354,952.08         6,709,904.17  
281              4           2005        2,458,535.62         3,354,952.08         6,709,904.17  
  
(10 row(s) affected)  
  
```  
  
 在这个例子中，OVER 子句未包含 PARTITION BY。 这意味着该函数将应用于查询所返回的所有行。 在 OVER 子句中指定的 ORDER BY 子句将确定应用 AVG 函数的逻辑顺序。 该查询将按年为在 WHERE 子句中指定的所有销售区域返回销售额的移动平均值。 在 SELECT 语句中指定的 ORDER BY 子句将确定显示查询行的顺序。  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
   ,CONVERT(VARCHAR(20),AVG(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS MovingAvg  
   ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (ORDER BY DATEPART(yy,ModifiedDate)   
                                            ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5  
ORDER BY SalesYear;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYear   SalesYTD             MovingAvg            CumulativeTotal  
---------------- ----------- ----------- -------------------- -------------------- --------------------  
274              NULL        2005        559,697.56           2,449,684.05         17,147,788.35  
275              2           2005        3,763,178.18         2,449,684.05         17,147,788.35  
276              4           2005        4,251,368.55         2,449,684.05         17,147,788.35  
277              3           2005        3,189,418.37         2,449,684.05         17,147,788.35  
280              1           2005        1,352,577.13         2,449,684.05         17,147,788.35  
281              4           2005        2,458,535.62         2,449,684.05         17,147,788.35  
283              1           2005        1,573,012.94         2,449,684.05         17,147,788.35  
284              1           2006        1,576,562.20         2,138,250.72         19,244,256.47  
287              NULL        2006        519,905.93           2,138,250.72         19,244,256.47  
285              NULL        2007        172,524.45           1,941,678.09         19,416,780.93  
(10 row(s) affected)  
```  
  
### <a name="d-specifying-the-rows-clause"></a>D. 指定 ROWS 子句  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。  
  
 以下示例使用 ROWS 子句定义其行将作为当前行以及后随的 N 行（在此示例中为 1 行）计算的窗口。  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS BETWEEN CURRENT ROW AND 1 FOLLOWING ),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        1,079,603.50  
287              NULL        519,905.93           2006        692,430.38  
285              NULL        172,524.45           2007        172,524.45  
283              1           1,573,012.94         2005        2,925,590.07  
280              1           1,352,577.13         2005        2,929,139.33  
284              1           1,576,562.20         2006        1,576,562.20  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        6,709,904.17  
281              4           2,458,535.62         2005        2,458,535.62  
```  
  
 在下面的示例中，使用 UNBOUNDED PRECEDING 指定 ROWS 子句。 结果为窗口在分区中的第一行开始。  
  
```sql  
SELECT BusinessEntityID, TerritoryID   
    ,CONVERT(VARCHAR(20),SalesYTD,1) AS SalesYTD  
    ,DATEPART(yy,ModifiedDate) AS SalesYear  
    ,CONVERT(VARCHAR(20),SUM(SalesYTD) OVER (PARTITION BY TerritoryID   
                                             ORDER BY DATEPART(yy,ModifiedDate)   
                                             ROWS UNBOUNDED PRECEDING),1) AS CumulativeTotal  
FROM Sales.SalesPerson  
WHERE TerritoryID IS NULL OR TerritoryID < 5;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
BusinessEntityID TerritoryID SalesYTD             SalesYear   CumulativeTotal  
---------------- ----------- -------------------- ----------- --------------------  
274              NULL        559,697.56           2005        559,697.56  
287              NULL        519,905.93           2006        1,079,603.50  
285              NULL        172,524.45           2007        1,252,127.95  
283              1           1,573,012.94         2005        1,573,012.94  
280              1           1,352,577.13         2005        2,925,590.07  
284              1           1,576,562.20         2006        4,502,152.27  
275              2           3,763,178.18         2005        3,763,178.18  
277              3           3,189,418.37         2005        3,189,418.37  
276              4           4,251,368.55         2005        4,251,368.55  
281              4           2,458,535.62         2005        6,709,904.17  
  
```  
  
## <a name="examples-sspdw"></a>示例：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-the-over-clause-with-the-row_number-function"></a>E. 将 OVER 子句与 ROW_NUMBER 函数结合使用  
 以下示例根据销售代表所分配的销售配额返回各自的 ROW_NUMBER。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ROW_NUMBER() OVER(ORDER BY SUM(SalesAmountQuota) DESC) AS RowNumber,  
    FirstName, LastName,   
CONVERT(VARCHAR(13), SUM(SalesAmountQuota),1) AS SalesQuota   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq  
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE e.SalesPersonFlag = 1  
GROUP BY LastName, FirstName;  
```  
  
 以下为部分结果集。  
  
 ```
 RowNumber  FirstName  LastName            SalesQuota  
 ---------  ---------  ------------------  -------------  
 1          Jillian    Carson              12,198,000.00  
 2          Linda      Mitchell            11,786,000.00  
 3          Michael    Blythe              11,162,000.00  
 4          Jae        Pak                 10,514,000.00  
 ```
 
### <a name="f-using-the-over-clause-with-aggregate-functions"></a>F. 将 OVER 子句与聚合函数结合使用  
 以下示例显示了将 OVER 子句与聚合函数结合使用。 在此示例中，使用 OVER 子句比使用子查询的效率高。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       AVG(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Avg,  
       COUNT(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Count,  
       MIN(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Min,  
       MAX(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Max  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 OrderNumber  Product  Qty  Total  Avg  Count  Min  Max  
 -----------  -------  ---  -----  ---  -----  ---  ---  
 SO43659      218      6    16     3    5      1    6  
 SO43659      220      4    16     3    5      1    6  
 SO43659      223      2    16     3    5      1    6  
 SO43659      229      3    16     3    5      1    6  
 SO43659      235      1    16     3    5      1    6  
 SO43664      229      1     2     1    2      1    1  
 SO43664      235      1     2     1    2      1    1  
 ```
 
 以下示例显示在计算所得值中将 OVER 子句与聚合函数结合使用。 注意，聚合由 `SalesOrderNumber` 计算，并会为每个 `SalesOrderNumber` 的每一行计算总销售订单的百分比。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT SalesOrderNumber AS OrderNumber, ProductKey AS Product,   
       OrderQuantity AS Qty,   
       SUM(OrderQuantity) OVER(PARTITION BY SalesOrderNumber) AS Total,  
       CAST(1. * OrderQuantity / SUM(OrderQuantity)   
        OVER(PARTITION BY SalesOrderNumber)   
            *100 AS DECIMAL(5,2)) AS PctByProduct  
FROM dbo.FactResellerSales   
WHERE SalesOrderNumber IN(N'SO43659',N'SO43664') AND  
      ProductKey LIKE '2%'  
ORDER BY SalesOrderNumber,ProductKey;  
```  
  
 此结果集的开始部分为：  
  
 ```
 OrderNumber  Product  Qty  Total  PctByProduct  
 -----------  -------  ---  -----  ------------  
 SO43659      218      6    16     37.50  
 SO43659      220      4    16     25.00  
 SO43659      223      2    16     12.50  
 SO43659      229      2    16     18.75  
 ```
 
## <a name="see-also"></a>另请参阅  
 [聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [分析函数 (Transact-SQL)](../../t-sql/functions/analytic-functions-transact-sql.md)   
 [Itzik Ben-Gan 在 sqlmag.com 上撰写的关于窗口函数和 OVER 的优秀博客文章](https://www.itprotoday.com/sql-server/how-use-microsoft-sql-server-2012s-window-functions-part-1)  
  
  
