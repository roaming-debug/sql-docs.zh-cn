---
description: SUM (Transact-SQL)
title: SUM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SUM
- SUM_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], sum of all
- SUM function [Transact-SQL]
- values [SQL Server]
- summation [SQL Server]
- expressions [SQL Server], sum of all values
- DISTINCT keyword
- totals [SQL Server], SUM
- summary values [SQL Server]
ms.assetid: 9af94d0f-55d4-428f-a840-ec530160f379
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2782e81e97a12d2783ba3705475b52fc97538c53
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104754027"
---
# <a name="sum-transact-sql"></a>SUM (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回表达式中所有值的和或仅非重复值的和。 SUM 只能用于数字列。 Null 值会被忽略。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Aggregate Function Syntax    
SUM ( [ ALL | DISTINCT ] expression )  

-- Analytic Function Syntax   
SUM ([ ALL ] expression) OVER ( [ partition_by_clause ] order_by_clause)  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 ALL  
 向所有值应用此聚合函数。 ALL 为默认值。  
  
 DISTINCT  
 指定 SUM 返回唯一值的总和。  
  
 *expression*  
 常量、列、函数以及算术运算符、位运算符和字符串运算符的任意组合。 expression 是精确数值或近似数值数据类型类别（bit 数据类型除外）的表达式。 不允许使用聚合函数和子查询。 有关详细信息，请参阅[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
 OVER **(** [ _partition\_by\_clause_ ] _order\_by\_clause_ **)**  
 partition_by_clause 将 FROM 子句生成的结果集划分为要应用函数的分区  。 如果未指定，则此函数将查询结果集的所有行视为单个组。 _order\_by\_clause_ 确定执行操作的逻辑顺序。 _order\_by\_clause_ 是必需的。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
 以最精确的 expression 数据类型返回所有 expression 值的和。  
  
|表达式结果|返回类型|  
|-----------------------|-----------------|  
|**tinyint**|**int**|  
|**smallint**|**int**|  
|**int**|**int**|  
|**bigint**|**bigint**|  
|decimal 类别 (p, s)|**decimal(38, s)**|  
|money 和 smallmoney 类别|**money**|  
|float 和 real 类别|**float**|  
  
## <a name="remarks"></a>注解  
 SUM 不与 OVER 和 ORDER BY 子句配合使用时为确定性函数。 与 OVER 和 ORDER BY 子句一同指定时，它具有不确定性。 有关详细信息，请参阅 [Deterministic and Nondeterministic Functions](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-sum-to-return-summary-data"></a>A. 使用 SUM 返回汇总数据  
 下面示例显示了使用 SUM 函数返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中汇总数据的过程。  
  
```sql
SELECT Color, SUM(ListPrice), SUM(StandardCost)  
FROM Production.Product  
WHERE Color IS NOT NULL   
    AND ListPrice != 0.00   
    AND Name LIKE 'Mountain%'  
GROUP BY Color  
ORDER BY Color;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
Color
--------------- --------------------- ---------------------
Black           27404.84              5214.9616
Silver          26462.84              14665.6792
White           19.00                 6.7926

(3 row(s) affected)
 ```  
  
### <a name="b-using-the-over-clause"></a>B. 使用 OVER 子句  
 以下示例将 SUM 函数与 OVER 子句结合使用，以便为 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Sales.SalesPerson` 表中的每个地区提供年度销售额的累计合计。 数据按 `TerritoryID` 分区并在逻辑上按 `SalesYTD` 排序。 这意味着将基于年度销售额为每个地区计算 SUM 函数。 请注意，对于 `TerritoryID` 1，为 2005 销售年度存在两行，分别表示在该年度有销售业绩的两个销售人员。 将计算这两行的累积销售总额，然后在计算中包括表示 2006 年销售额的第三行。  
  
```sql
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
  
 在这个例子中，OVER 子句未包含 PARTITION BY。 这意味着该函数将应用于查询所返回的所有行。 在 OVER 子句中指定的 ORDER BY 子句将确定应用 SUM 函数的逻辑顺序。 该查询将按年为在 WHERE 子句中指定的所有销售区域返回销售额的累计合计。 在 SELECT 语句中指定的 ORDER BY 子句将确定显示查询行的顺序。  
  
```sql
SELECT BusinessEntityID, TerritoryID   
   ,DATEPART(yy,ModifiedDate) AS SalesYear  
   ,CONVERT(VARCHAR(20),SalesYTD,1) AS  SalesYTD  
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
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-a-simple-sum-example"></a>C. 一个简单的 SUM 示例  
 以下示例返回 2003 年销售的每件产品的总数。  
  
```sql
-- Uses AdventureWorks  
  
SELECT ProductKey, SUM(SalesAmount) AS TotalPerProduct  
FROM dbo.FactInternetSales  
WHERE OrderDateKey >= '20030101'   
      AND OrderDateKey < '20040101'  
GROUP BY ProductKey  
ORDER BY ProductKey;  
  
```  
  
 以下为部分结果集。  
  
 ```
ProductKey  TotalPerProduct
----------  ---------------
214         31421.0200
217         31176.0900
222         29986.4300
225          7956.1500
 ```
  
### <a name="d-calculating-group-totals-with-more-than-one-column"></a>D. 计算多列的组合计  
 以下示例针对 `ListPrice` 表中列出的每种颜色计算 `StandardCost` 与 `Product` 的和。  
  
```sql
-- Uses AdventureWorks  
  
SELECT Color, SUM(ListPrice)AS TotalList,   
       SUM(StandardCost) AS TotalCost  
FROM dbo.DimProduct  
GROUP BY Color  
ORDER BY Color;  
```  
  
 结果集的第一部分如下所示：  
  
 ```
Color       TotalList      TotalCost
----------  -------------  --------------
Black       101295.7191    57490.5378
Blue         24082.9484    14772.0524
Grey           125.0000       51.5625
Multi          880.7468      526.4095
NA            3162.3564     1360.6185
 ```  
  
## <a name="see-also"></a>另请参阅  
 [聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)  
  
  

