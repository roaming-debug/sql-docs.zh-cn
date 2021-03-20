---
description: DIFFERENCE (Transact-SQL)
title: DIFFERENCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DIFFERENCE
- DIFFERENCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DIFFERENCE function
- comparing SOUNDEX values
- SOUNDEX values
ms.assetid: c58ca25d-d6ea-48fa-93bb-c9374b0b2a2b
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6c5f24b82ba8b0b0ad75fcb9846048409cae50f0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752037"
---
# <a name="difference-transact-sql"></a>DIFFERENCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

该函数返回一个整数值，用于度量两个不同字符表达式的 [SOUNDEX()](./soundex-transact-sql.md) 值之间的差异。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DIFFERENCE ( character_expression , character_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*character_expression*  
字符数据的字母数字[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 character_expression 可以是常量、变量或列。  
  
## <a name="return-types"></a>返回类型  
**int**  
 
## <a name="remarks"></a>备注  
`DIFFERENCE` 比较两个不同的 `SOUNDEX` 值，并返回一个整数值。 该值用于度量 `SOUNDEX` 值匹配的程度，范围为 0 到 4。 值为 0 表示 SOUNDEX 值之间的相似性较弱或不相似；4 表示与 SOUNDEX 值非常相似，甚至完全相同。  
  
`DIFFERENCE` 和 `SOUNDEX` 具有排序规则敏感度。  
  
## <a name="examples"></a>示例  
本示例的第一部分比较两个非常相似的字符串的 `SOUNDEX` 值。 对于 Latin1_General 排序规则，`DIFFERENCE` 返回的值为 `4`。 示例的第二部分比较两个差异较大的字符串的 `SOUNDEX` 值，对于 Latin1_General 排序规则，`DIFFERENCE` 返回的值为 `0`。  
  
```sql  
-- Returns a DIFFERENCE value of 4, the least possible difference.  
SELECT SOUNDEX('Green'), SOUNDEX('Greene'), DIFFERENCE('Green','Greene');  
GO  
-- Returns a DIFFERENCE value of 0, the highest possible difference.  
SELECT SOUNDEX('Blotchet-Halls'), SOUNDEX('Greene'), DIFFERENCE('Blotchet-Halls', 'Greene');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----- ----- -----------   
G650  G650  4             
  
(1 row(s) affected)  
  
----- ----- -----------   
B432  G650  0             
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [SOUNDEX (Transact-SQL)](../../t-sql/functions/soundex-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

