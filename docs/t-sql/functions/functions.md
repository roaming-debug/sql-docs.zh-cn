---
description: Microsoft SQL 数据库函数有哪些？
title: Microsoft SQL 数据库函数有哪些？ | Microsoft Docs
ms.custom: ''
ms.date: 06/28/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- built-in functions [SQL Server]
- function [SQL Server] See functions [SQL Server]
- functions [Transact-SQL]
- functions [SQL Server], about functions
- scalar functions
- functions [SQL Server]
ms.assetid: 17186213-5ab5-40b0-b470-b660af1ec44c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1364ff8238e8aea9f303b7a6aa7ef114b4544d3e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179230"
---
# <a name="what-are-the-sql-database-functions"></a>SQL 数据库函数有哪些？
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

了解可在 SQL 数据库中使用的内置函数类别。 可以使用内置函数，或创建自己的用户定义函数。
  
## <a name="aggregate-functions"></a>聚合函数

聚合函数对一组值执行计算，并返回单个值。 在 select 列表或 SELECT 语句的 HAVING 子句中允许使用它们。 可以将聚合与 GROUP BY 子句结合使用，来计算行类别的聚合。 使用 OVER 子句来计算特定范围内的值的聚合。 OVER 子句不能跟在 GROUPING 或 GROUPING_ID 聚合后。

所有聚合函数都是确定性的，这意味着对相同的输入值进行运算时，它们始终返回相同的值。 有关详细信息，请参阅 [确定性函数和不确定性函数](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。|

## <a name="analytic-functions"></a>分析函数
解析函数基于一组行计算聚合值。 不过，与聚合函数不同，分析函数可能针对每个组返回多行。 可以使用分析函数来计算移动平均线、运行总计、百分比或一个组内的前 N 个结果。

## <a name="ranking-functions"></a>排名函数
排名函数为分区中的每一行返回一个排名值。 根据所用函数的不同，某些行可能与其他行接收到相同的值。 排名函数具有不确定性。

## <a name="rowset-functions"></a>行集函数
行集函数 返回可在 SQL 语句中像表引用一样使用的对象。

## <a name="scalar-functions"></a>标量函数
对单一值进行运算，然后返回单一值。 只要表达式有效，即可使用标量函数。

### <a name="categories-of-scalar-functions"></a>标量函数类别
  
|函数类别|说明|  
|-----------------------|-----------------|  
|[配置函数](configuration-functions-transact-sql.md)|返回当前配置信息。|  
|[转换函数](conversion-functions-transact-sql.md)|支持数据类型强制转换和转换。|  
|[游标函数](cursor-functions-transact-sql.md)|返回游标信息。|  
|[日期和时间数据类型及函数](date-and-time-data-types-and-functions-transact-sql.md)|对日期和时间输入值执行运算，然后返回字符串、数字或日期和时间值。|  
|[JSON 函数](json-functions-transact-sql.md)|验证、查询或更改 JSON 数据。|  
|[逻辑函数](logical-functions-choose-transact-sql.md)|执行逻辑运算。|  
|[数学函数](mathematical-functions-transact-sql.md)|基于作为函数的参数提供的输入值执行运算，然后返回数字值。|  
|[元数据函数](metadata-functions-transact-sql.md)|返回有关数据库和数据库对象的信息。|  
|[安全函数](security-functions-transact-sql.md)|返回有关用户和角色的信息。|  
|[字符串函数](string-functions-transact-sql.md)|对字符串（char 或 varchar）输入值执行运算，然后返回一个字符串或数字值   。|  
|[系统函数](../../relational-databases/system-functions/system-functions-category-transact-sql.md)|执行运算后返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中有关值、对象和设置的信息。|  
|[系统统计函数](system-statistical-functions-transact-sql.md)|返回系统的统计信息。|  
|[文本和图像函数](./text-and-image-functions-textptr-transact-sql.md)|对文本或图像输入值或列执行运算，然后返回有关值的信息。|  
  
## <a name="function-determinism"></a>函数确定性  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内置函数可以是确定的或是不确定的。 如果任何时候用一组特定的输入值调用内置函数，返回的结果总是相同的，则这些内置函数为确定的。 如果每次调用内置函数时，即使用的是同一组特定输入值，也总返回不同结果，则这些内置函数为不确定的。 有关详细信息，请参阅 [确定性函数和不确定性函数](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)  
  
## <a name="function-collation"></a>函数排序规则  
 使用字符串输入并返回字符串输出的函数，对输出使用输入字符串的排序规则。  
  
 使用非字符输入并返回字符串的函数对输出使用当前数据库的默认排序规则。  
  
 使用多个字符串输入并返回字符串的函数，使用排序规则的优先顺序规则设置输出字符串的排序规则。 有关详细信息，请参阅[排序规则优先顺序 (Transact-SQL)](../../t-sql/statements/collation-precedence-transact-sql.md)。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE FUNCTION (Transact-SQL)](../../t-sql/statements/create-function-transact-sql.md)   
 [确定性函数和不确定性函数](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)   
 [使用存储过程 &#40;MDX&#41;](../../mdx/using-stored-procedures-mdx.md)  
  
