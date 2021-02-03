---
description: 运算符优先级 (Transact-SQL)
title: 运算符优先级 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- operators [Transact-SQL], precedence
- operator precedence [Transact-SQL]
- order of operator execution [Transact-SQL]
- precedence [SQL Server], operators
ms.assetid: f04d2439-6fff-4e4c-801f-cc62faef510a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7ef9b05c0ecdbb0054c9ad0ef3e1a88b123fb87b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191909"
---
# <a name="operator-precedence-transact-sql"></a>运算符优先级 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  如果一个复杂表达式有多个运算符，则运算符优先级将确定操作序列。 执行顺序可能对结果值有明显的影响。  
  
 运算符的优先级别如下表中所示。 在较低级别的运算符之前先对较高级别的运算符进行求值。 在下表中，1 代表最高级别，8 代表最低级别。
  
|级别|运算符|  
|-----------|---------------|  
|1|~（位非）|  
|2|*（乘）、/（除）、%（取模）|  
|3|+（正）、-（负）、+（加）、+（串联）、-（减）、&（位与）、^（位异或）、&#124;（位或）|  
|4|=、>、\<, >=、>=、<=、<>、!=、!>、!<（比较运算符）|  
|5|NOT|  
|6|AND|  
|7|ALL、ANY、BETWEEN、IN、LIKE、OR、SOME|  
|8|=（赋值）|  
  
 如果一个表达式中的两个运算符有相同的优先级别，则按照它们在表达式中的位置对其从左到右进行求值。 例如，在下面的 `SET` 语句所使用的表达式中，在加运算符之前先对减运算符进行求值。  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 4 - 2 + 27;  
-- Evaluates to 2 + 27 which yields an expression result of 29.  
SELECT @MyNumber;  
```  
  
 在表达式中使用括号替代所定义的运算符的优先级。 对括号内的所有内容进行求值会得到一个单一的值。 该值可被括号外的任何运算符使用。  
  
 例如，在下面的 `SET` 语句所使用的表达式中，乘运算符具有比加运算符更高的优先级别。 首先计算乘法运算；表达式结果为 `13`。  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * 4 + 5;  
-- Evaluates to 8 + 5 which yields an expression result of 13.  
SELECT @MyNumber;  
```  
  
 在以下 `SET` 语句使用的表达式中，括号使加法先进行计算。 此表达式的结果为 `18`。  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + 5);  
-- Evaluates to 2 * 9 which yields an expression result of 18.  
SELECT @MyNumber;  
```  
  
 如果表达式有嵌套的括号，那么首先对嵌套最深的表达式求值。 以下示例中包含嵌套的括号，其中表达式 `5 - 3` 在嵌套最深的那对括号中。 该表达式产生一个值 `2`。 然后，加法运算符 (`+`) 将此结果与 `4` 相加，得到值 `6`。 最后将 `6` 与 `2` 相乘，生成表达式的结果 `12`。  
  
```sql  
DECLARE @MyNumber INT;  
SET @MyNumber = 2 * (4 + (5 - 3) );  
-- Evaluates to 2 * (4 + 2) which then evaluates to 2 * 6, and   
-- yields an expression result of 12.  
SELECT @MyNumber;  
```  
  
## <a name="see-also"></a>另请参阅  
 [逻辑运算符 (Transact-SQL)](../../t-sql/language-elements/logical-operators-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)  
  
