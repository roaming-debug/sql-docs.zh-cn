---
description: '&lt;（小于）(Transact-SQL)'
title: '&lt;（小于）(Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- < (Less Than)
- <_TSQL
- Than
- Less
- <
- Less Than
dev_langs:
- TSQL
helpviewer_keywords:
- less than (<)
- < (less than operator)
ms.assetid: 54f50bdd-bb62-4593-9af9-4c49edecab75
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e73ac51d769c939371b51b07df25c86d758eeae8
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749827"
---
# <a name="lt-less-than-transact-sql"></a>&lt;（小于）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  比较两个表达式（比较运算符）。 比较非空表达式时，如果左操作数的值小于右操作数，则结果为 TRUE；否则结果为 FALSE。 如果任何一个操作数为 NULL 或两个都为 NULL，则请参阅主题 [SET ANSI_NULLS (Transact-SQL)](../../t-sql/statements/set-ansi-nulls-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
expression < expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 为任意有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 两个表达式都必须包含可隐式转换的数据类型。 转换方式取决于[数据类型优先级](../../t-sql/data-types/data-type-precedence-transact-sql.md)的相关规则。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="examples"></a>示例  
  
### <a name="a-using--in-a-simple-query"></a>A. 在简单查询中使用 <  
 下面的示例返回 `HumanResources.Department` 表中其 `DepartmentID` 的值小于 3 的所有行。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT DepartmentID, Name  
FROM HumanResources.Department  
WHERE DepartmentID < 3  
ORDER BY DepartmentID;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
DepartmentID Name  
------------ --------------------------------------------------  
1            Engineering  
2            Tool Design  
  
(2 row(s) affected)  
  
```  
  
### <a name="b-using--to-compare-two-variables"></a>B. 使用 < 比较两个变量  
  
```sql  
DECLARE @a INT = 45, @b INT = 40;  
SELECT IIF ( @a < @b, 'TRUE', 'FALSE' ) AS Result;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Result  
------  
FALSE  
  
(1 row(s) affected)  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [IIF (Transact-SQL)](../../t-sql/functions/logical-functions-iif-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
