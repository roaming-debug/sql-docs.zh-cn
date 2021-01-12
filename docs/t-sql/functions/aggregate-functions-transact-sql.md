---
description: 聚合函数 (Transact-SQL)
title: 聚合函数 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- functions [SQL Server], aggregate
- aggregate functions [SQL Server], about aggregate functions
- summarizing functions
- aggregate functions [SQL Server]
ms.assetid: 0c06ae42-eb0a-4d77-9d74-aa1e7f344009
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 48e47f1f81744765c8fe76080a5c62ba70089327
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98090880"
---
# <a name="aggregate-functions-transact-sql"></a>聚合函数 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

聚合函数对一组值执行计算，并返回单个值。 除了 `COUNT(*)` 外，聚合函数都会忽略 Null 值。 聚合函数经常与 SELECT 语句的 GROUP BY 子句一起使用。
  
所有聚合函数均为确定性函数。 换言之，每次使用一组特定的输入值调用聚合函数时，它们所返回的值都是相同的。 有关函数确定性的详细信息，请参阅[确定性函数和不确定性函数](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。 [OVER 子句](../../t-sql/queries/select-over-clause-transact-sql.md)可以跟在除 STRING_AGG、GROUPING 或 GROUPING_ID 函数以外的所有聚合函数后面。
  
只能在以下位置将聚合函数作为表达式使用：
-   SELECT 语句的选择列表（子查询或外部查询）。  
-   HAVING 子句。  
  
[!INCLUDE[tsql](../../includes/tsql-md.md)] 提供下列聚合函数：

- [APPROX_COUNT_DISTINCT](../../t-sql/functions/approx-count-distinct-transact-sql.md)
- [AVG](../../t-sql/functions/avg-transact-sql.md)
- [CHECKSUM_AGG](../../t-sql/functions/checksum-agg-transact-sql.md)
- [COUNT](../../t-sql/functions/count-transact-sql.md)
- [COUNT_BIG](../../t-sql/functions/count-big-transact-sql.md)
- [GROUPING](../../t-sql/functions/grouping-transact-sql.md)
- [GROUPING_ID](../../t-sql/functions/grouping-id-transact-sql.md)
- [MAX](../../t-sql/functions/max-transact-sql.md)
- [MIN](../../t-sql/functions/min-transact-sql.md)
- [STDEV](../../t-sql/functions/stdev-transact-sql.md)
- [STDEVP](../../t-sql/functions/stdevp-transact-sql.md)
- [STRING_AGG](../../t-sql/functions/string-agg-transact-sql.md)
- [SUM](../../t-sql/functions/sum-transact-sql.md)
- [VAR](../../t-sql/functions/var-transact-sql.md)
- [VARP](../../t-sql/functions/varp-transact-sql.md)
  
## <a name="see-also"></a>另请参阅
[内置函数 (Transact-SQL)](../../t-sql/functions/functions.md)  
[OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
