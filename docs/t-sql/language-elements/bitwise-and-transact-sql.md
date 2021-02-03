---
description: '&amp;（位与）(Transact-SQL)'
title: '&amp;（位与）(Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- bitwise
- '&'
- '&_TSQL'
dev_langs:
- TSQL
helpviewer_keywords:
- AND, bitwise AND
- '& (bitwise AND)'
- bitwise AND (&)
ms.assetid: 20275755-4fa7-47b1-a9be-ac85606d63b0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 99d07e7e957c9447f493acee1a40c8a8a96064a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159198"
---
# <a name="amp-bitwise-and-transact-sql"></a>&amp;（位与）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在两个整数值之间执行“逻辑位与”运算。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
expression & expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 整数数据类型类别中的任何一种数据类型、bit、binary 或 varbinary 数据类型的任何有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)  。 对于位运算，expression 被视为二进制数字。  
  
> [!NOTE]  
>  位运算中，只有一个 expression 可以是 binary 或 varbinary 的其中任意一种数据类型 。  
  
## <a name="result-types"></a>结果类型  
 如果输入值为 int，则结果为 int 。  
  
 如果输入值为 smallint，则结果为 smallint 。  
  
 如果输入值为 tinyint 或 bit，则结果为 tinyint  。  
  
## <a name="remarks"></a>备注  
 & 位运算符将在两个表达式之间执行位与逻辑运算，从两个表达式取对应的位。 当且仅当输入表达式中两个位（正在被解析的当前位）的值都为 1 时，结果中的位才被设置为 1；否则，结果中的位被设置为 0。  
  
 如果左侧和右侧的表达式具有不同的整数数据类型（例如，左侧的表达式的数据类型为 smallint，右侧的表达式的数据类型为 int），则会将较小数据类型的参数转换为较大数据类型。 此示例中，smallint 表达式转换成了 int。  
  
## <a name="examples"></a>示例  
 以下示例将使用 int 数据类型创建一个表，用于存储值，并将两个值插入到一行中。  
  
```sql
CREATE TABLE bitwise (   
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 下面的查询将在 `a_int_value` 和 `b_int_value` 列之间执行位与运算。  
  
```sql  
SELECT a_int_value & b_int_value  
FROM bitwise;  
GO  
```  
  
 下面是结果集：  
  
```  
-----------   
10            
  
(1 row(s) affected)  
```  
  
 170（`a_int_value` 或 `A`）的二进制表示形式是 `0000 0000 1010 1010`。 75（`b_int_value` 或 `B`）的二进制表示形式是 `0000 0000 0100 1011`。 对上述两个值执行“位与”运算将产生二进制结果 `0000 0000 0000 1010`，即十进制数 10。  
  
```  
(A & B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 0000 1010  
```  
  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [位运算符 (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&=（位与赋值）(Transact-SQL)](../../t-sql/language-elements/bitwise-and-equals-transact-sql.md)   
 [复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


