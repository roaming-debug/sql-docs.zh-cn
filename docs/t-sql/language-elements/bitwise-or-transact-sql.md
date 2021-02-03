---
description: '|（位或）(Transact-SQL)'
title: '|（位或） (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- '|'
- '|_TSQL'
- Bitwise OR
- bitwise
- OR
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator
- bitwise OR (|)
- '| (bitwise OR operator)'
ms.assetid: 86a3b87f-9688-4eaf-a552-29f1b01d880a
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ff76c7ce5b98084aaf6f833a3e1b85d9fa8f68be
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193384"
---
# <a name="-bitwise-or-transact-sql"></a>|（位或）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句中，将两个指定的整数值转换为二进制表达式后执行逻辑位或运算。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql   
expression | expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 整数数据类型类别、bit、binary 或 varbinary 数据类型的任何有效[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 对于位运算，expression 被视为二进制数字。  
  
> [!NOTE]  
>  位运算中，只有一个 expression 可以是 binary 或 varbinary 数据类型 。  
  
## <a name="result-types"></a>结果类型  
 如果输入值为 int，则返回 int，如果输入值为 smallint，则返回 smallint，或者如果输入值为 tinyint，则返回 tinyint。  
  
## <a name="remarks"></a>备注  
 位运算符 | 取两个表达式的每个对应位，在两个表达式之间执行逻辑位或运算。 如果在输入表达式中有一个位为 1 或两个位均为 1（对于正在解析的当前位），那么结果中的位将被设置为 1；如果输入表达式中的两个位都不为 1，则结果中的位将被设置为 0。  
  
 如果左侧和右侧的表达式具有不同的整数数据类型（例如，左侧的表达式的数据类型为 smallint，右侧的表达式的数据类型为 int），则会将较小数据类型的参数转换为较大数据类型。 此示例中，smallint 表达式转换成了 int。  
  
## <a name="examples"></a>示例  
 以下示例将创建一个包含 int 数据类型的表以显示原始的值，并将该表放入一个行中。  
  
```sql  
CREATE TABLE bitwise (  
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);  
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 以下查询对 a_int_value 列和 b_int_value 列执行位或运算。  
  
```sql  
SELECT a_int_value | b_int_value  
FROM bitwise;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-----------   
235           
  
(1 row(s) affected)  
```  
  
 170（以下的 a_int_value 或 `A`）的二进制表示形式为 `0000 0000 1010 1010`。 75（以下的 b_int_value 或 `B`）的二进制表示形式为 `0000 0000 0100 1011`。 对这两个值执行位或运算产生的二进制结果为 `0000 0000 1110 1011`，即十进制数 235。  
  
```  
(A | B)  
0000 0000 1010 1010  
0000 0000 0100 1011  
-------------------  
0000 0000 1110 1011  
```  
  
## <a name="see-also"></a>另请参阅  
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [位运算符 (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [&#124;=（位异或赋值）(Transact-SQL)](../../t-sql/language-elements/bitwise-or-equals-transact-sql.md)   
 [复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


