---
description: STUFF (Transact-SQL)
title: STUFF (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STUFF
- STUFF_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- deleting characters
- STUFF function
- length characters
- removing characters
- replacing characters
- characters [SQL Server], replacing
- inserting data
ms.assetid: abb0afa9-44f6-42a2-a871-5f471dfb222b
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a91badcdfc378bc072c95952e40904778b879ddd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207938"
---
# <a name="stuff-transact-sql"></a>STUFF (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  STUFF 函数将字符串插入到另一个字符串中。 它从第一个字符串的开始位置删除指定长度的字符；然后将第二个字符串插入到第一个字符串的开始位置。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
STUFF ( character_expression , start , length , replaceWith_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *character_expression*  
 字符数据的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 character_expression 可以是常量、变量，也可以是字符列或二进制数据列。  
  
 *start*  
 一个整数值，指定删除和插入的开始位置。 如果 start 为负或为零，则返回空字符串。 如果 start 的长度大于第一个 character_expression，则返回空字符串。 start 的类型可以是 bigint。  
  
 *length*  
 一个整数，指定要删除的字符数。 如果 length 为负，则返回空字符串。 如果 length 的长度大于第一个 character_expression，则最多可以删除到最后一个 character_expression 中的最后一个字符。  如果 *length* 为零，则插入在 *start* 位置发生，并且不会删除任何字符。 length 的类型可以是 bigint。

 replaceWith_expression  
 字符数据的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 character_expression 可以是常量、变量，也可以是字符列或二进制数据列。 此表达式从 start 开始替换 length 个字符的 character_expression。 如果 replaceWith_expression 为 `NULL`，则在不插入任何内容的情况下删除字符。   
  
## <a name="return-types"></a>返回类型  
 如果 character_expression 是支持的字符数据类型之一，则返回字符数据。 如果 character_expression 是支持的二进制数据类型之一，则返回二进制数据。  
  
## <a name="remarks"></a>备注  
 如果开始位置或长度值是负数，或者开始位置大于第一个字符串的长度，则返回 Null 字符串。 如果开始位置为 0，则返回 Null 值。 如果要删除的长度大于第一个字符串的长度，则删除到第一个字符串中的第一个字符。  

如果结果值大于返回类型支持的最大值，则会引发错误。  
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
 使用 SC 排序规则时，character_expression 和 replaceWith_expression 都可以包含代理项对。 length 参数将 character_expression 中的每个代理项计为一个字符。  
  
## <a name="examples"></a>示例  
 以下示例从第一个字符串 `abcdef` 的第 `2` 个位置 (`b`) 开始删除三个字符，然后在删除位置插入第二个字符串，从而创建并返回一个字符串。  
  
```sql  
SELECT STUFF('abcdef', 2, 3, 'ijklmn');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------   
aijklmnef   
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另请参阅  
 [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE (Transact-SQL)](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
