---
description: 比较运算符 (Transact-SQL)
title: 比较运算符 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], testing
- operators [Transact-SQL], comparison
- testing expressions
- Boolean data type
- Boolean expressions
- comparing expressions
- comparison operators [SQL Server]
ms.assetid: b0cc68ef-3029-484c-a917-0c15dcbc230d
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0c59b82a018a7d58c1eafdd607a1cf79e0003600
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208195"
---
# <a name="comparison-operators-transact-sql"></a>比较运算符 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  比较运算符测试两个表达式是否相同。 除 text、ntext 或 image 数据类型的表达式外，比较运算符可以用于所有其他表达式。 下表列出了 [!INCLUDE[tsql](../../includes/tsql-md.md)] 比较运算符。  
  
|运算符|含义|  
|--------------|-------------|  
|[=（等于）](../../t-sql/language-elements/equals-transact-sql.md)|等于|  
|[>（大于）](../../t-sql/language-elements/greater-than-transact-sql.md)|大于|  
|[<（小于）](../../t-sql/language-elements/less-than-transact-sql.md)|小于|  
|[>=（大于或等于）](../../t-sql/language-elements/greater-than-or-equal-to-transact-sql.md)|大于等于|  
|[<=（小于或等于）](../../t-sql/language-elements/less-than-or-equal-to-transact-sql.md)|小于或等于|  
|[<>（不等于）](../../t-sql/language-elements/not-equal-to-transact-sql-traditional.md)|不等于|  
|[!=（不等于）](../../t-sql/language-elements/not-equal-to-transact-sql-exclamation.md)|不等于（非 ISO 标准）|  
|[\!<（不小于）](../../t-sql/language-elements/not-less-than-transact-sql.md)|不小于（非 ISO 标准）|  
|[\!>（不大于）](../../t-sql/language-elements/not-greater-than-transact-sql.md)|不大于（非 ISO 标准）|  
  
## <a name="boolean-data-type"></a>布尔数据类型  
 具有 Boolean 数据类型的比较运算符的结果。 它有三个值：TRUE、FALSE 和 UNKNOWN。 返回 Boolean 数据类型的表达式称为布尔表达式。  
  
 与其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型不同，Boolean 数据类型不能被指定为表列或变量的数据类型，也不能在结果集中返回。  
  
 当 SET ANSI_NULLS 为 ON 时，带有一个或两个 NULL 表达式的运算符返回 UNKNOWN。 当 SET ANSI_NULLS 为 OFF 时，除了等于 (=) 和不等于 (<>) 运算符外，相同的规则适用。 当 SET ANSI_NULLS 为 OFF 时，这些运算符将 NULL 视为已知值，等效于任何其他 NULL，且只返回 TRUE 或 FALSE（绝不会返回 UNKNOWN）。  
  
 在 WHERE 子句中使用数据类型为 Boolean 的表达式，可以筛选出符合搜索条件的行，也可以在控制流语言语句（例如 IF 和 WHILE）中使用这种表达式。例如：  
  
```syntaxsql  
-- Uses AdventureWorks  
  
DECLARE @MyProduct INT;  
SET @MyProduct = 750;  
IF (@MyProduct <> 0)  
   SELECT ProductID, Name, ProductNumber  
   FROM Production.Product  
   WHERE ProductID = @MyProduct;  
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)  
  
  
