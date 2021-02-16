---
description: SOME | ANY (Transact-SQL)
title: SOME | ANY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SOME
- SOME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- scalar values
- comparing scalar with single-column set
- ANY operator
- SOME | ANY keyword
- single-column set of values [SQL Server]
ms.assetid: 1f717ad6-f67b-4980-9397-577ecb0e5789
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5f25709500f370b6083584b1321937b0484310bf
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342587"
---
# <a name="some--any-transact-sql"></a>SOME | ANY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  比较标量值和单列集中的值。 SOME 和 ANY 是等效的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
scalar_expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
     { SOME | ANY } ( subquery )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *scalar_expression*  
 为任意有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
 { = \| <> \| != \| > \| >= \| !> \| < \| <= \| !< }  
 任何有效的比较运算符。  
  
 SOME | ANY  
 指定应进行比较。  
  
 subquery  
 包含某列结果集的子查询。 所返回列的数据类型必须是与 scalar_expression 相同的数据类型  。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="result-value"></a>结果值  
 对于任何对 (scalar_expression,x)（其中 x 是单列集中的值），当指定的比较是 TRUE 时，SOME 或 ANY 返回 TRUE；否则返回 FALSE       。  
  
## <a name="remarks"></a>备注  
 SOME 要求 scalar_expression  与子查询返回的至少一个值比较时满足比较条件。 有关要求  scalar_expression 与子查询返回的每个值比较时都符合比较条件的语句，请参阅 [ALL (Transact-SQL)](../../t-sql/language-elements/all-transact-sql.md)。 例如，如果子查询返回的值为 2 和 3，则对于值为 2 的 scalar_express  ，  scalar_expression = SOME（子查询）的计算结果为 TRUE。 如果子查询返回值 2 和 3，*scalar_expression* = ALL（子查询）的计算结果为 FALSE，因为子查询的某些值（值 3）不符合表达式的条件。  
  
## <a name="examples"></a>示例  
  
### <a name="a-running-a-simple-example"></a>A. 运行简单示例  
 以下语句创建一个简单表，并向 `1` 列添加值 `2`、`3`、`4` 和 `ID`。  
  
```sql  
CREATE TABLE T1  
(ID INT) ;  
GO  
INSERT T1 VALUES (1) ;  
INSERT T1 VALUES (2) ;  
INSERT T1 VALUES (3) ;  
INSERT T1 VALUES (4) ;  
```  
  
 以下查询返回 `TRUE`，因为 `3` 小于表中的某些值。  
  
```sql  
IF 3 < SOME (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
 下面的查询返回 `FALSE`，因为 `3` 并不小于表中的所有值。  
  
```sql  
IF 3 < ALL (SELECT ID FROM T1)  
PRINT 'TRUE'   
ELSE  
PRINT 'FALSE' ;  
```  
  
### <a name="b-running-a-practical-example"></a>B. 运行实际示例  
 以下示例创建一个存储过程，该过程确定是否能够在指定的天数中制造出 `SalesOrderID` 数据库中具有指定 `AdventureWorks2012` 的所有组件。 该示例使用子查询为具有特定 `DaysToManufacture` 的所有组件创建 `SalesOrderID` 值的列表，然后测试子查询返回的值中是否有大于指定天数的值。 如果返回的所有 `DaysToManufacture` 的值都小于规定的天数，则条件为 TRUE，并输出第一个消息。  
  
```sql  
-- Uses AdventureWorks  
  
CREATE PROCEDURE ManyDaysToComplete @OrderID INT, @NumberOfDays INT  
AS  
IF   
@NumberOfDays < SOME  
   (  
    SELECT DaysToManufacture  
    FROM Sales.SalesOrderDetail  
    JOIN Production.Product   
    ON Sales.SalesOrderDetail.ProductID = Production.Product.ProductID   
    WHERE SalesOrderID = @OrderID  
   )  
PRINT 'At least one item for this order can''t be manufactured in specified number of days.'
ELSE   
PRINT 'All items for this order can be manufactured in the specified number of days or less.' ;  
  
```  
  
 若要测试该过程，请使用 `SalesOrderID``49080`（具有一个需要 `2` 天的组件和两个需要 0 天的组件）来执行该过程。 第一个语句符合条件。 第二个查询不符合条件。  
  
```sql  
EXECUTE ManyDaysToComplete 49080, 2 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `All items for this order can be manufactured in the specified number of days or less.`  
  
```  
EXECUTE ManyDaysToComplete 49080, 1 ;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `At least one item for this order can't be manufactured in specified number of days.`  
  
## <a name="see-also"></a>另请参阅  
 [ALL (Transact-SQL)](../../t-sql/language-elements/all-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [IN (Transact-SQL)](../../t-sql/language-elements/in-transact-sql.md)  
  
