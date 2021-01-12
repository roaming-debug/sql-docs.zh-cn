---
description: _（通配符 - 匹配一个字符）(Transact-SQL)
title: _（通配符 - 匹配一个字符）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Match
- wildcard
- _TSQL
- Match One
- _
dev_langs:
- TSQL
helpviewer_keywords:
- wildcard characters [SQL Server]
- _ (wildcard - match one character)
ms.assetid: 11a2ed36-9e21-4bdf-ae20-a31db1434b97
author: cawrites
ms.author: chadam
ms.openlocfilehash: 38398adeb20a6a9f70ab225d4ead8b8504635323
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98095857"
---
# <a name="_-wildcard---match-one-character-transact-sql"></a>_（通配符 - 匹配一个字符）(Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

下划线字符 _ 用于匹配涉及模式匹配的字符串比较操作（如 `LIKE` 和 `PATINDEX`）中的任何单个字符。  
  
## <a name="examples"></a>示例  

## <a name="a-simple-example"></a>A：简单示例   

以下示例返回以字母 `m` 开头且第三个字母为 `d` 的所有数据库名称。 下划线字符指定名称的第二个字符可以是任何字母。 `model` 数据库和 `msdb` 数据库均符合此条件。 `master` 数据库则不符合。

```sql
SELECT name FROM sys.databases
WHERE name LIKE 'm_d%';
```   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   
```
name
-----
model
msdb
```   
你可能有其他符合此条件的数据库。

可使用多个下划线来表示多个字符。 将 `LIKE` 条件更改为包含两个下划线 `'m__%` 会将 master 数据库包含在结果中。

### <a name="b-more-complex-example"></a>B：更复杂的示例
 以下示例使用 _ 运算符在 `Person` 表中查找名字由三个字母组成且以 `an` 结尾的所有人。  
  
```sql  
-- USE AdventureWorks2012
  
SELECT FirstName, LastName  
FROM Person.Person  
WHERE FirstName LIKE '_an'  
ORDER BY FirstName;  
```  
## <a name="c-escaping-the-underscore-character"></a>C：对下划线字符进行转义   
以下示例会返回 `db_owner`、`db_ddladmin` 等固定数据库角色的名称，但它也会返回 `dbo` 用户。 

```sql
SELECT name FROM sys.database_principals
WHERE name LIKE 'db_%';
```

第三个字符位置处的下划线被视为通配符，它无法仅筛选出以字母 `db_` 开头的主体。 若要对下划线进行转义，请将它括在括号中 `[_]`。 

```sql
SELECT name FROM sys.database_principals
WHERE name LIKE 'db[_]%';
```   
现在已排除 `dbo` 用户。   
[!INCLUDE[ssResult_md](../../includes/ssresult-md.md)]   
```
name
-------------
db_owner
db_accessadmin
db_securityadmin
...
```

  
## <a name="see-also"></a>另请参阅  
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)   
  [%（通配符 - 需匹配的字符）](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)   
  [[ ]（通配符 - 需匹配的字符）](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [[^]（通配符 - 无需匹配的字符）](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)     
  
