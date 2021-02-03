---
description: COUNT_BIG (Transact-SQL)
title: COUNT_BIG (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COUNT_BIG_TSQL
- COUNT_BIG
dev_langs:
- TSQL
helpviewer_keywords:
- totals [SQL Server], COUNT_BIG function
- counting items in group
- groups [SQL Server], number of items in
- number of group items
- COUNT_BIG function
ms.assetid: f2e3601f-487e-4917-bb01-47b1047908cd
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 639ee47ccf91d456c4f5a34b4b53a4cfb85ed0dc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184168"
---
# <a name="count_big-transact-sql"></a>COUNT_BIG (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数返回组中找到的项数量。 `COUNT_BIG` 的操作与 [COUNT](../../t-sql/functions/count-transact-sql.md) 函数类似。 这些函数区别只在于其返回的值的数据类型。 `COUNT_BIG` 始终返回“bigint”数据类型值。 `COUNT` 始终返回“int”数据类型值。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql

-- Aggregation Function Syntax  
COUNT_BIG ( { [ [ ALL | DISTINCT ] expression ] | * } )  
  
-- Analytic Function Syntax  
COUNT_BIG ( [ ALL ] { expression | * } ) OVER ( [ <partition_by_clause> ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
ALL  
向所有值应用此聚合函数。 ALL 充当默认值。
  
DISTINCT  
指定 `COUNT_BIG` 返回唯一非 Null 值的数量。
  
*expression*  
任何类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 `COUNT_BIG` 不支持表达式中的聚合函数或子查询。
  
*\**  
指定 `COUNT_BIG` 应对所有行计数，以确定要返回的总表行计数。 `COUNT_BIG(*)` 不采用任何参数，也不支持使用 DISTINCT。 `COUNT_BIG(*)` 不需要“expression”参数，因为根据定义，该函数不使用有关任何特定列的信息。 `COUNT_BIG(*)` 返回指定表中的行数，但保留副本行。 它会单独为每一行计数，包括包含 null 值的行。
  
OVER **(** [ partition_by_clause ] [ order_by_clause ] **)**  
“partition_by_clause”将 `FROM` 子句生成的结果集划分为要应用 `COUNT_BIG` 函数的分区。 如果未指定，则此函数将查询结果集的所有行视为单个组。 “order_by_clause”确定操作的逻辑顺序。 请参阅 [OVER Clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md) 获取详细信息。
  
## <a name="return-types"></a>返回类型
**bigint**
  
## <a name="remarks"></a>备注  
COUNT_BIG(\*) 返回组中的项数。 包括 NULL 值和重复项。
  
COUNT_BIG (ALL *expression*) 计算组中每行的 *expression*，然后返回非 null 值的数量。
  
COUNT_BIG (DISTINCT *expression*) 计算组中每行的 *expression*，然后返回独一无二的非 null 值的数量。
  
COUNT_BIG 不与 OVER 和 ORDER BY 子句配合使用时为确定性函数。 与 OVER 和 ORDER BY 子句一同指定时，COUNT_BIG 具有不确定性 ****。 请参阅[确定性函数和不确定性函数](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)获取详细信息。
  
## <a name="examples"></a>示例  
请参阅 [COUNT &#40;Transact-SQL&#41;](../../t-sql/functions/count-transact-sql.md) 获取示例。
  
## <a name="see-also"></a>另请参阅
[聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)  
[COUNT (Transact-SQL)](../../t-sql/functions/count-transact-sql.md)  
[int、bigint、smallint 和 tinyint (Transact-SQL)](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
[OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)
  
  
