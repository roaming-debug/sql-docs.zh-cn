---
description: BETWEEN (Transact-SQL)
title: BETWEEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/28/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BETWEEN
- BETWEEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- inclusive ranges
- testing range
- exclusive ranges
- NOT BETWEEN operator
- BETWEEN operator
- range to test [SQL Server]
ms.assetid: a5d5b050-203e-4355-ac85-e08ef5ca7823
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8f8df816ba1885e6dd713d8b77affcd4b1175c3c
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104747857"
---
# <a name="between-transact-sql"></a>BETWEEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  指定测试范围。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
test_expression [ NOT ] BETWEEN begin_expression AND end_expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 test_expression  
 要在由 begin_expression 和 end_expression 定义的范围内测试的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 test_expression 的数据类型必须与 begin_expression 和 end_expression 的数据类型相同。  
  
 NOT  
 指定谓词的结果被取反。  
  
 begin_expression  
 为任何有效的表达式。 begin_expression 的数据类型必须与 test_expression 和 end_expression 的数据类型相同。  
  
 end_expression  
 为任何有效的表达式。 end_expression 的数据类型必须与 test_expression 和 begin_expression 的数据类型相同。  
  
 AND  
 充当一个占位符，用于指示 test_expression 应该在 begin_expression 和 end_expression 指定的范围内。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="result-value"></a>结果值  
 如果 test_expression 的值大于或等于 begin_expression 的值，并且小于或等于 end_expression 的值，则 BETWEEN 返回 TRUE。  
  
 如果 test_expression 的值小于 begin_expression 的值，或大于 end_expression 的值，则 NOT BETWEEN 返回 TRUE。  
  
## <a name="remarks"></a>注解  
 若要指定排他范围，请使用大于 (>) 和小于 (<) 运算符。 如果任何 BETWEEN 或 NOT BETWEEN 谓词的输入为 NULL，则结果为 UNKNOWN。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-between"></a>A. 使用 BETWEEN  
 以下示例返回有关数据库中数据库角色的信息。 第一个查询返回所有角色。 第二个示例使用 `BETWEEN` 子句将角色限制为指定的 `database_id` 值。  
  
```sql  
SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R';

SELECT principal_id, name 
FROM sys.database_principals
WHERE type = 'R'
AND principal_id BETWEEN 16385 AND 16390;
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]   
```  
principal_id    name
------------  ---- 
0               public
16384           db_owner
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
16391           db_datawriter
16392           db_denydatareader
16393           db_denydatawriter
```  
```  
principal_id    name
------------  ---- 
16385           db_accessadmin
16386           db_securityadmin
16387           db_ddladmin
16389           db_backupoperator
16390           db_datareader
```  
  
### <a name="b-using--and--instead-of-between"></a>B. 使用 > 和 <，而不使用 BETWEEN  
 下面的示例使用大于 (`>`) 和小于 (`<`) 运算符，因为这些运算符是非包含的，所以该示例返回九行，而不是像上一个示例那样返回十行。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate > 27 AND ep.Rate < 30  
ORDER BY ep.Rate;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 FirstName   LastName             Rate  
 ---------   -------------------  ---------  
 Paula       Barreto de Mattos    27.1394  
 Janaina     Bueno                27.4038  
 Dan         Bacon                27.4038  
 Ramesh      Meyyappan            27.4038  
 Karen       Berg                 27.4038  
 David       Bradley              28.7500  
 Hazem       Abolrous             28.8462  
 Ovidiu      Cracium              28.8462  
 Rob         Walters              29.8462  
 ```    
  
### <a name="c-using-not-between"></a>C. 使用 NOT BETWEEN  
 下面的示例查找处于指定范围 `27` 到 `30` 以外的所有行。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.FirstName, e.LastName, ep.Rate  
FROM HumanResources.vEmployee e   
JOIN HumanResources.EmployeePayHistory ep   
    ON e.BusinessEntityID = ep.BusinessEntityID  
WHERE ep.Rate NOT BETWEEN 27 AND 30  
ORDER BY ep.Rate;  
GO  
```  
  
### <a name="d-using-between-with-datetime-values"></a>D. 使用带有日期时间值的 BETWEEN   
 以下示例检索 datetime 值介于 `'20011212'` 和 `'20020105'`（含）之间的行。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, RateChangeDate  
FROM HumanResources.EmployeePayHistory  
WHERE RateChangeDate BETWEEN '20011212' AND '20020105';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```  
 BusinessEntityID RateChangeDate  
 ----------- -----------------------  
 3           2001-12-12 00:00:00.000  
 4           2002-01-05 00:00:00.000  
 ```  
 
 由于指定查询中的日期值和 `RateChangeDate` 列中存储的 datetime 值时未指定日期的时间部分，因此该查询将检索预期行。 未指定时间部分时，将默认使用 12:00 A.M。 请注意，若某行的时间部分晚于 2002-01-05 12:00 A.M.， 则由于它处于范围之外，因此此查询不返回该行。  
  
  
## <a name="see-also"></a>另请参阅  
 [&#62;（大于）(Transact-SQL)](../../t-sql/language-elements/greater-than-transact-sql.md)   
 [&#60;（小于）(Transact-SQL)](../../t-sql/language-elements/less-than-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  


