---
description: IN (Transact-SQL)
title: IN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- IN_TSQL
- IN
dev_langs:
- TSQL
helpviewer_keywords:
- values [SQL Server], matching
- NOT IN keyword
- 8623 (Database Engine error)
- matching values in subquery or list [SQL Server]
- IN keyword
- 8632 (Database Engine error)
ms.assetid: 4419de73-96b1-4dfe-8500-f4507915db04
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0751b396deef4a8617b18e9555aae50fdb835010
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193377"
---
# <a name="in-transact-sql"></a>IN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  确定指定的值是否与子查询或列表中的值相匹配。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
test_expression [ NOT ] IN   
    ( subquery | expression [ ,...n ]  
    )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 test_expression  
 为任意有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
 subquery  
 包含某列结果集的子查询。 该列必须与 test_expression 具有相同的数据类型。  
  
 expression[ ,... n ]  
 一个表达式列表，用来测试是否匹配。 所有的表达式必须与 test_expression 具有相同的类型。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="result-value"></a>结果值  
 如果 test_expression 的值与 subquery 所返回的任何值相等，或与逗号分隔的列表中的任何 expression 相等，则结果值为 TRUE；否则，结果值为 FALSE。  
  
 使用 NOT IN 对 subquery 值或 expression 求反。  
  
> [!CAUTION]  
>  subquery 或 expression 使用 IN 或 NOT IN 与 test_expression 比较后返回的所有空值都将返回 UNKNOWN。 将空值与 IN 或 NOT IN 一起使用会产生意外结果。  
  
## <a name="remarks"></a>备注  
 在 IN 子句的括号中显式包括数量非常多的值（数以千计，以逗号分隔）可能会消耗资源并返回错误 8623 或 8632。 若要解决这一问题，请将这些项存储于某个表的 IN 列表中，并在 IN 子句中使用 SELECT 嵌套查询。  
  
 错误 8623：  
  
 `The query processor ran out of internal resources and could not produce a query plan. This is a rare event and only expected for extremely complex queries or queries that reference a very large number of tables or partitions. Please simplify the query. If you believe you have received this message in error, contact Customer Support Services for more information.`  
  
 错误 8632：  
  
 `Internal error: An expression services limit has been reached. Please look for potentially complex expressions in your query, and try to simplify them.`  
  
## <a name="examples"></a>示例  
  
### <a name="a-comparing-or-and-in"></a>A. 比较 OR 和 IN  
 以下示例选择设计工程师、工具设计人员或销售助理等雇员的姓名列表。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle = 'Design Engineer'   
   OR e.JobTitle = 'Tool Designer'   
   OR e.JobTitle = 'Marketing Assistant';  
GO  
```  
  
 但是，也可以使用 IN 检索相同的结果：  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, e.JobTitle  
FROM Person.Person AS p  
JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle IN ('Design Engineer', 'Tool Designer', 'Marketing Assistant');  
GO  
```  
  
 以下是上面任一查询的结果集。  
  
```  
FirstName   LastName      Title  
---------   ---------   ---------------------  
Sharon      Salavaria   Design Engineer                                     
Gail        Erickson    Design Engineer                                     
Jossef      Goldberg    Design Engineer                                     
Janice      Galvin      Tool Designer                                       
Thierry     D'Hers      Tool Designer                                       
Wanida      Benshoof    Marketing Assistant                                 
Kevin       Brown       Marketing Assistant                                 
Mary        Dempsey     Marketing Assistant                                 
  
(8 row(s) affected)  
```  
  
### <a name="b-using-in-with-a-subquery"></a>B. 带子查询使用 IN  
 以下示例查找 `SalesPerson` 表中所有销售人员的 ID 以获得当年销售额超过 $250,000 的雇员，然后从 `Employee` 表中选择 `EmployeeID` 与 `SELECT` 子查询的结果相匹配的所有雇员的姓名。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
FirstName   LastName                                             
---------   --------   
Tsvi         Reiter                                              
Michael      Blythe                                              
Tete         Mensa-Annan                                         
  
(3 row(s) affected)  
```  
  
### <a name="c-using-not-in-with-a-subquery"></a>C. 带子查询使用 NOT IN  
 以下示例查找销售额不超过 $250,000 的销售人员。 `NOT IN` 查找与值列表中的项不匹配的销售人员。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName  
FROM Person.Person AS p  
    JOIN Sales.SalesPerson AS sp  
    ON p.BusinessEntityID = sp.BusinessEntityID  
WHERE p.BusinessEntityID NOT IN  
   (SELECT BusinessEntityID  
   FROM Sales.SalesPerson  
   WHERE SalesQuota > 250000);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-in-and-not-in"></a>D. 使用 IN 和 NOT IN  
 下面的示例查找 `FactInternetSales` 表中与 `DimSalesReason` 表中的 `SalesReasonKey` 值相匹配的所有条目。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
IN (SELECT SalesReasonKey FROM DimSalesReason);   
```  
  
 下面的示例查找 `FactInternetSalesReason` 表中与 `DimSalesReason` 表中的 `SalesReasonKey` 值不匹配的所有条目。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT * FROM FactInternetSalesReason   
WHERE SalesReasonKey   
NOT IN (SELECT SalesReasonKey FROM DimSalesReason);  
```  
  
### <a name="e-using-in-with-an-expression-list"></a>E. 将 IN 与表达式列表结合使用  
 下面的示例查找 `DimEmployee` 表中销售人员的所有 ID，该表中员工的名字（不含姓氏）均为 `Mike` 或 `Michael`。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName  
FROM DimEmployee  
WHERE FirstName IN ('Mike', 'Michael');  
```  
  
## <a name="see-also"></a>另请参阅  
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)   
 [ALL (Transact-SQL)](../../t-sql/language-elements/all-transact-sql.md)   
 [SOME | ANY (Transact-SQL)](../../t-sql/language-elements/some-any-transact-sql.md)  
  
  



