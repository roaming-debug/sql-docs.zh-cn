---
description: 表达式（Transact-SQL)
title: 表达式 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- Boolean expressions
- expressions [SQL Server], about expressions
- combining expressions
- Transact-SQL expressions
- expressions [SQL Server], combining
- simple expressions [SQL Server]
- complex expressions [SQL Server]
ms.assetid: ee53c5c8-e36c-40f9-8cd1-d933791b98fa
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6db31166cf0931e7ace6a6f1366fed9776509568
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100373"
---
# <a name="expressions-transact-sql"></a>表达式（Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  符号和运算符的一种组合，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]将处理该组合以获得单个数据值。 简单表达式可以是一个常量、变量、列或标量函数。 可以用运算符将两个或更多的简单表达式联接起来组成复杂表达式。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
{ constant | scalar_function | [ table_name. ] column | variable   
    | ( expression ) | ( scalar_subquery )   
    | { unary_operator } expression   
    | expression { binary_operator } expression   
    | ranking_windowed_function | aggregate_windowed_function  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Expression in a SELECT statement  
<expression> ::=   
{  
    constant   
    | scalar_function   
    | column  
    | variable  
    | ( expression  )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE Windows_collation_name ]  
  
-- Scalar Expression in a DECLARE, SET, IF...ELSE, or WHILE statement  
<scalar_expression> ::=  
{  
    constant   
    | scalar_function   
    | variable  
    | ( expression  )  
    | (scalar_subquery )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE { Windows_collation_name ]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
  
|术语|定义|  
|----------|----------------|  
|*constant*|表示单个特定数据值的符号。 有关详细信息，请参阅[常量 (Transact-SQL)](../../t-sql/data-types/constants-transact-sql.md)。|  
|scalar_function|一个提供特定服务并返回单个值的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法单元。 scalar_function 可以是内置标量函数（如 SUM、GETDATE 或 CAST 函数），也可以是标量用户定义函数。|  
|[ table_name. ]|表的名称或别名。|  
|*column*|列的名称。 表达式中只允许列的名称。|  
|variable|变量或参数的名称。 有关详细信息，请参阅 [DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)。|  
| **(** _expression_  **)**|本主题中定义的任意一个有效表达式。 括号是分组运算符，用于确保先运算括号内表达式中的运算符，然后再将结果与别的表达式组合。|  
|**(** _scalar_subquery_ **)**|返回一个值的子查询。 例如：<br /><br /> `SELECT MAX(UnitPrice)`<br /><br /> `FROM Products`|  
|{ unary_operator }|一元运算符只能用于计算结果数据类型属于数字数据类型类别的表达式。 只有一个数字操作数的运算符：<br /><br /> ＋ 指示正数。<br /><br /> - 指示负数。<br /><br /> ~ 指示一的补数运算符。|  
|{ binary_operator }|用于定义如何组合两个表达式以得到一个结果的运算符。 binary_operator 可以是算术运算符、赋值运算符 (=)、位运算符、比较运算符、逻辑运算符、字符串连接运算符 (+) 或一元运算符。 有关运算符的详细信息，请参阅[运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)。|  
|ranking_windowed_function|任意 [!INCLUDE[tsql](../../includes/tsql-md.md)] 排名函数。 有关详细信息，请参阅[排名函数 (Transact-SQL)](../../t-sql/functions/ranking-functions-transact-sql.md)。|  
|aggregate_windowed_function|任意包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] OVER 子句的聚合函数。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。|  
  
## <a name="expression-results"></a>表达式结果  
 对于由单个常量、变量、标量函数或列名组成的简单表达式，其数据类型、排序规则、精度、小数位数和值就是它所引用的元素的数据类型、排序规则、精度、小数位数和值。  
  
 用比较运算符或逻辑运算符组合两个表达式时，生成的数据类型为 Boolean，并且值为下列类型之一：TRUE、FALSE 或 UNKNOWN。 有关 Boolean 数据类型的详细信息，请参阅[比较运算符 (Transact SQL)](../../t-sql/language-elements/comparison-operators-transact-sql.md)。  
  
 用算术运算符、位运算符或字符串运算符组合两个表达式时，生成的数据类型取决于运算符。  
  
 由多个符号和运算符组成的复杂表达式的计算结果为单值结果。 生成的表达式的数据类型、排序规则、精度和值由进行组合的两个表达式决定，并按每次两个表达式的顺序递延，直到得出最后结果。 表达式中元素组合的顺序由表达式中运算符的优先级决定。  
  
## <a name="remarks"></a>注解  
 两个表达式可以由一个运算符组合起来，只要它们具有该运算符支持的数据类型，并且满足至少下列一个条件：  
  
-   两个表达式有相同的数据类型。  
  
-   优先级低的数据类型可以隐式转换为优先级高的数据类型。  
  
 如果表达式不满足这些条件，则可以使用 CAST 或 CONVERT 函数将优先级低的数据类型显式转化为优先级高的数据类型，或者转换为一种可以隐式转化成优先级高的数据类型的中间数据类型。  
  
 如果没有支持的隐式或显式转换，则两个表达式将无法组合。  
  
 任何计算结果为字符串的表达式的排序规则都应遵循排序规则优先顺序规则。 有关详细信息，请参阅[排序规则优先顺序 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)。  
  
 在 C 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 这类编程语言中，表达式的计算结果始终为单值结果。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 选择列表中的表达式按以下规则进行变体：分别对结果集中的每一行计算表达式的值。 同一个表达式对结果集内的每一行可能会有不同的值，但该表达式在每一行的值是唯一的。 例如，在 `SELECT` 语句中，对 `ProductID` 的引用以及选择列表中的术语 `1+2` 都是表达式：  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, 1+2  
FROM Production.Product;  
GO  
```  
  
 结果集中的每个行的表达式 `1+2` 的计算结果都为 `3`。 虽然表达式 `ProductID` 在结果集的每一行中产生一个唯一值，但每一行只有一个 `ProductID` 值。  
 
- [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 为每个线程分配固定的最大内存量，因此，任何线程都无法占用所有内存。  其中一些内存用于存储查询的表达式。  如果查询包含太多表达式，并且其所需内存超出内部限制，则引擎不会执行该查询。  若要避免发生此问题，用户可以将查询更改为多个查询，使每个查询包含较少的表达式。 例如，在 WHERE 子句中有一个具有许多表达式的查询： 

```sql
DELETE FROM dbo.MyTable 
WHERE
(c1 = '0000001' AND c2 = 'A000001') or
(c1 = '0000002' AND c2 = 'A000002') or
(c1 = '0000003' AND c2 = 'A000003') or
...

```
将此查询更改为：

```sql
DELETE FROM dbo.MyTable WHERE (c1 = '0000001' AND c2 = 'A000001');
DELETE FROM dbo.MyTable WHERE (c1 = '0000002' AND c2 = 'A000002');
DELETE FROM dbo.MyTable WHERE (c1 = '0000003' AND c2 = 'A000003');
...
```

## <a name="see-also"></a>另请参阅  
 [AT TIME ZONE (Transact-SQL)](../../t-sql/queries/at-time-zone-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [COALESCE (Transact-SQL)](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [NULLIF (Transact-SQL)](../../t-sql/language-elements/nullif-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
  
  
