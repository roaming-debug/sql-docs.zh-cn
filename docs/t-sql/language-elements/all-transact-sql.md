---
description: ALL (Transact-SQL)
title: ALL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALL_TSQL
- ALL
dev_langs:
- TSQL
helpviewer_keywords:
- single-column set of values [SQL Server]
- ALL (Transact-SQL)
ms.assetid: 4b0c002e-1ffd-4425-a980-11fdc1f24af7
author: cawrites
ms.author: chadam
ms.openlocfilehash: f315735e5ad8b1c8fd9d5a54d581d66fe7e820b7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176366"
---
# <a name="all-transact-sql"></a>ALL (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  比较标量值和单列集中的值。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
scalar_expression { = | <> | != | > | >= | !> | < | <= | !< } ALL ( subquery )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *scalar_expression*  
 为任意有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 一个比较运算符。  
  
 subquery  
 返回单列结果集的子查询。 返回列的数据类型必须与 scalar_expression 的数据类型相同  。  
  
 受限的 SELECT 语句，其中不允许使用 ORDER BY 子句和 INTO 关键字。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="result-value"></a>结果值  
 如果指定的比较对于所有比较对 (_scalar_expression_ **,** _x)_ 均为 TRUE（其中 *x* 是单列集中的值），则返回 TRUE。 否则返回 FALSE。  
  
## <a name="remarks"></a>备注  
 ALL 要求 scalar_expression 与子查询返回的每个值进行比较时都应满足比较条件  。 例如，如果子查询返回的值为 2 和 3，则对于值为 2 的 scalar_expression，scalar_expression <= ALL（子查询）的计算结果为 TRUE   。 如果子查询返回值 2 和 3，*scalar_expression* = ALL（子查询）的计算结果为 FALSE，因为子查询的某些值（值 3）不符合表达式的条件。  
  
 有关要求 scalar_expression 只与子查询返回的某一个值比较时满足比较条件的语句，请参阅 [SOME | ANY (Transact-SQL)](../../t-sql/language-elements/some-any-transact-sql.md)  。  
  
 本文讨论了 ALL 用于子查询的情况。 ALL 也可以与 [UNION](../../t-sql/language-elements/set-operators-union-transact-sql.md) 和 [SELECT](../../t-sql/queries/select-transact-sql.md) 一起使用。  
  
## <a name="examples"></a>示例  
 以下示例创建一个存储过程，该过程确定是否能够在指定的天数中制造出 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中具有指定 `SalesOrderID` 的所有组件。 该示例使用子查询为具有特定 `DaysToManufacture` 的所有组件创建 `SalesOrderID` 值的列表，然后确认所有 `DaysToManufacture` 都在指定的天数内。  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE DaysToBuild @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays >= ALL  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'All items for this order can be manufactured in specified number of days or less.'  
ELSE   
PRINT 'Some items for this order can''t be manufactured in specified number of days or less.' ;  
```  
  
 若要测试该过程，请使用 `SalesOrderID 49080`（具有一个需要 `2` 天的组件和两个需要 0 天的组件）来执行该过程。 下面的第一个语句符合条件。 第二个查询不符合条件。  
  
```sql  
EXECUTE DaysToBuild 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in specified number of days or less.`  
  
```sql  
EXECUTE DaysToBuild 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `Some items for this order can't be manufactured in specified number of days or less.`  
  
## <a name="see-also"></a>另请参阅  
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [IN (Transact-SQL)](../../t-sql/language-elements/in-transact-sql.md)  
  
