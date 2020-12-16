---
title: 子查询
description: Azure Synapse Analytics 和并行数据仓库中的子查询
ms.custom: seo-lt-2019
titleSuffix: Azure Synapse Analytics
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
ms.assetid: 0e8ebd60-1936-48c9-b2b9-e099c8269fcf
author: shkale-msft
ms.author: shkale
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: a739b44c673a20a157dd2ea03824ae9a8b70a30b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439042"
---
# <a name="subqueries-azure-synapse-analytics-parallel-data-warehouse"></a>子查询（Azure Synapse Analytics 和并行数据仓库）
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  本主题提供在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]或[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]中使用子查询的示例。  
  
 有关 SELECT 语句，请参阅 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)  
  
## <a name="contents"></a>目录  
  
-   [基础知识](#Basics)  
  
-   [示例：Azure Synapse Analytics 和并行数据仓库](#Examples)  
  
##  <a name="basics"></a><a name="Basics"></a> 基础知识  
 子查询  
 子查询是一个嵌套在 SELECT、INSERT、UPDATE 或 DELETE 语句或其他子查询中的查询。 子查询亦称内部查询或内部选择。  
  
 外部查询  
 包含子查询的语句， 亦称外部选择。  
  
 相关子查询  
 引用外部查询中的表的子查询。  
  
##  <a name="examples-sssdw-and-sspdw"></a><a name="Examples"></a> 示例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 本节提供 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]或[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]支持的子查询的示例。  
  
### <a name="a-top-and-order-by-in-a-subquery"></a>A. 子查询中的 TOP 和 ORDER BY  
  
```sql
SELECT * FROM tblA  
WHERE col1 IN  
    (SELECT TOP 100 col1 FROM tblB ORDER BY col1);
```  
  
### <a name="b-having-clause-with-a-correlated-subquery"></a>B. 包含相关子查询的 HAVING 子句  
  
```sql
SELECT dm.EmployeeKey, dm.FirstName, dm.LastName   
FROM DimEmployee AS dm   
GROUP BY dm.EmployeeKey, dm.FirstName, dm.LastName  
HAVING 5000 <=   
(SELECT sum(OrderQuantity)  
FROM FactResellerSales AS frs  
WHERE dm.EmployeeKey = frs.EmployeeKey)  
ORDER BY EmployeeKey;
```  
  
### <a name="c-correlated-subqueries-with-analytics"></a>C. 使用分析的相关子查询  
  
```sql
SELECT * FROM ReplA AS A   
WHERE A.ID IN   
    (SELECT sum(B.ID2) OVER() FROM ReplB AS B WHERE A.ID2 = B.ID);  
```  
  
### <a name="d-correlated-union-statements-in-a-subquery"></a>D. 子查询中的相关联合语句  
  
```sql
SELECT * FROM RA   
WHERE EXISTS   
    (SELECT 1 FROM RB WHERE RB.b1 = RA.a1   
     UNION ALL SELECT 1 FROM RC);  
```  
  
### <a name="e-join-predicates-in-a-subquery"></a>E. 子查询中的联接谓词  
  
```sql
SELECT * FROM RA INNER JOIN RB   
    ON RA.a1 = (SELECT COUNT(*) FROM RC);  
```  
  
### <a name="f-correlated-join-predicates-in-a-subquery"></a>F. 子查询中的相关联接谓词  
  
```sql
SELECT * FROM RA   
    WHERE RA.a2 IN   
    (SELECT 1 FROM RB INNER JOIN RC ON RA.a1=RB.b1+RC.c1);  
```  
  
### <a name="g-correlated-subselects-as-data-sources"></a>G. 作为数据源的相关嵌套 select 语句  
  
```sql
SELECT * FROM RA   
    WHERE 3 = (SELECT COUNT(*)   
        FROM (SELECT b1 FROM RB WHERE RB.b1 = RA.a1) X);  
```  
  
### <a name="h-correlated-subqueries-in-the-data-values--used-with-aggregates"></a>H. 用于聚合的数据值中的相关子查询  
  
```sql
SELECT Rb.b1, (SELECT RA.a1 FROM RA WHERE RB.b1 = RA.a1) FROM RB GROUP BY RB.b1;  
```  
  
### <a name="i-using-in-with-a-correlated-subquery"></a>I. 在相关子查询中使用 IN  
 以下示例在相关或重复子查询中使用 `IN`。 该查询依赖于外部查询来查询其值。 该内部查询为外部查询可能选择的每一行各重复运行一次。 该查询检索 `FactResellerSales` 表中 `OrderQuantity` 为 `5` 且雇员标识号在 `DimEmployee` 和 `FactResellerSales` 表中相匹配的每个雇员的 `EmployeeKey` 加姓名的一个实例。  
  
```sql
SELECT DISTINCT dm.EmployeeKey, dm.FirstName, dm.LastName   
FROM DimEmployee AS dm   
WHERE 5 IN   
    (SELECT OrderQuantity  
    FROM FactResellerSales AS frs  
    WHERE dm.EmployeeKey = frs.EmployeeKey)  
ORDER BY EmployeeKey;  
```  
  
### <a name="j-using-exists-versus-in-with-a-subquery"></a>J. 在子查询中使用 EXISTS 和 IN  
 以下示例展示了语义等效的查询，以说明使用 `EXISTS` 关键字和 `IN` 关键字的区别。 两者都是子查询示例，用于检索产品子类别为 `Road Bikes` 的每个产品名称的一个实例。 `ProductSubcategoryKey` 在 `DimProduct` 和 `DimProductSubcategory` 表之间进行匹配。  
  
```sql
SELECT DISTINCT EnglishProductName  
FROM DimProduct AS dp   
WHERE EXISTS  
    (SELECT *  
     FROM DimProductSubcategory AS dps   
     WHERE dp.ProductSubcategoryKey = dps.ProductSubcategoryKey  
           AND dps.EnglishProductSubcategoryName = 'Road Bikes')  
ORDER BY EnglishProductName;  
```  
  
 或  
  
```sql
SELECT DISTINCT EnglishProductName  
FROM DimProduct AS dp   
WHERE dp.ProductSubcategoryKey IN  
    (SELECT ProductSubcategoryKey  
     FROM DimProductSubcategory   
     WHERE EnglishProductSubcategoryName = 'Road Bikes')  
ORDER BY EnglishProductName;  
```  
  
### <a name="k-using-multiple-correlated-subqueries"></a>K. 使用多个相关子查询  
 此示例使用两个相关子查询查找售出过某种特定产品的雇员的姓名。  
  
```sql
SELECT DISTINCT LastName, FirstName, e.EmployeeKey  
FROM DimEmployee e JOIN FactResellerSales s ON e.EmployeeKey = s.EmployeeKey  
WHERE ProductKey IN  
(SELECT ProductKey FROM DimProduct WHERE ProductSubcategoryKey IN  
(SELECT ProductSubcategoryKey FROM DimProductSubcategory   
 WHERE EnglishProductSubcategoryName LIKE '%Bikes'))  
ORDER BY LastName;  
```  
  
  
