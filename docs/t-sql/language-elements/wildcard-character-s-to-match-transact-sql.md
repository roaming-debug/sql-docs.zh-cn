---
title: 用于匹配字符的 [ ] 通配符
description: 使用通配符匹配一个或多个字符。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Match
- wildcard
- '[ ]'
- '[_]_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- '[ ] (wildcard - character(s) to match)'
ms.assetid: 57817576-0bf1-49ed-b05d-fac27e8fed7a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b0cc55631431246f87a64c9a17fe953e34c454f3
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095873"
---
# <a name="--wildcard---characters-to-match-transact-sql"></a>\[ \]（通配符 - 要匹配的字符）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

匹配指定范围内或者属于方括号 `[ ]` 所指定的集合中的任意单个字符。 可以在涉及模式匹配的字符串比较（例如，`LIKE` 和 `PATINDEX`）中使用这些通配符。  

 
## <a name="examples"></a>示例  
### <a name="a-simple-example"></a>A:简单示例   
以下示例返回以 `m` 字母开头的名称。 `[n-z]` 指定第二个字母必须是 `n` 到 `z` 范围内的某个字母。 百分号通配符 `%` 允许任何或不包含以 3 个字符开头的字符。 `model` 数据库和 `msdb` 数据库均符合此条件。 `master` 数据库不符合条件，并被排除在结果集外。
 
```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm[n-z]%';
```
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
name
-----
model
msdb
```   
 可能必须安装其他符合条件的数据库。


### <a name="b-more-complex-example"></a>B：更复杂的示例   
 以下示例使用 [] 运算符查找其地址中有四位邮政编码的所有 [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] 雇员的 ID 和姓名。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.BusinessEntityID, p.FirstName, p.LastName, a.PostalCode  
FROM HumanResources.Employee AS e  
INNER JOIN Person.Person AS p ON e.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.BusinessEntityAddress AS ea ON e.BusinessEntityID = ea.BusinessEntityID  
INNER JOIN Person.Address AS a ON a.AddressID = ea.AddressID  
WHERE a.PostalCode LIKE '[0-9][0-9][0-9][0-9]';  
```  
  
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  
  
```  
EmployeeID      FirstName      LastName      PostalCode  
----------      ---------      ---------     ----------  
290             Lynn           Tsoflias      3000  
```  

### <a name="c-using-a-set-that-combines-ranges-and-single-characters"></a>C:使用组合了范围和单字符的集

通配符集可包含单字符和范围。 以下示例使用 [] 运算符查找以数字或一系列特殊字符开头的字符串。

```sql
SELECT [object_id], OBJECT_NAME(object_id) AS [object_name], name, column_id 
FROM sys.columns 
WHERE name LIKE '[0-9!@#$.,;_]%';
```

[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]  

```
object_id     object_name                         name  column_id
---------     -----------                         ----  ---------
615673241     vSalesPersonSalesByFiscalYears      2002  5
615673241     vSalesPersonSalesByFiscalYears      2003  6
615673241     vSalesPersonSalesByFiscalYears      2004  7
1591676718    JunkTable                           _xyz  1
```
  
## <a name="see-also"></a>另请参阅  
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)   
  [%（通配符 - 需匹配的字符）(Transact-SQL)](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
 [[^]（通配符 - 无需匹配的字符）(Transact-SQL)](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
 [\_（通配符 - 匹配一个字符）(Transact-SQL)](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)  
    
  
