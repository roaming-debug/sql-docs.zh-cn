---
description: ^（位异或）(Transact-SQL)
title: ^（位异或）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ^_TSQL
- Bitwise Exclusive OR
- Exclusive
- ^
- bitwise
- OR
dev_langs:
- TSQL
helpviewer_keywords:
- ^ (bitwise exclusive OR operator)
- OR operator
- exclusive OR mathematical operations
- bitwise exclusive OR (^)
ms.assetid: f38f0ad4-46d0-40ea-9851-0f928fda5293
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 274f54de1fe1c73f311348ac20bc2d946d7907fd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439192"
---
# <a name="-bitwise-exclusive-or-transact-sql"></a>^（位异或）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在两个整数值之间执行“位异或”运算。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
expression ^ expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 整数数据类型类别中的任何一种数据类型、bit、binary 或 varbinary 数据类型的任何有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)  。 对于位运算，expression 被视为二进制数字。  
  
> [!NOTE]  
>  位运算中，只有一个 expression 可以是 binary 或 varbinary 数据类型 。  
  
## <a name="result-types"></a>结果类型  
 如果输入值为 int，则结果为 int 。  
  
 如果输入值为 smallint，则结果为 smallint 。  
  
 如果输入值为 tinyint，则结果为 tinyint 。  
  
## <a name="remarks"></a>备注  
 通过从两个表达式中取对应的位，^ 位运算符对两个表达式执行按位逻辑异或运算。 如果在输入表达式的正在被解析的对应位中，任意一位（但不是两个位）的值为 1，则结果中该位的值被设置为 1。 如果相对应的两个位的值都为 0 或者都为 1，那么结果中该位的值被清除为 0。  
  
 如果左侧和右侧的表达式具有不同的整数数据类型（例如，左侧的表达式的数据类型为 smallint，右侧的表达式的数据类型为 int），则会将较小数据类型的参数转换为较大数据类型。 此示例中，smallint 表达式转换成了 int。  
  
## <a name="examples"></a>示例  
 以下示例创建一个表，其中使用 int 数据类型存储原始值，并将两个值插入一行。  
  
```sql  
CREATE TABLE bitwise (   
  a_int_value INT NOT NULL,  
  b_int_value INT NOT NULL);
GO  
INSERT bitwise VALUES (170, 75);  
GO  
```  
  
 下面的查询对 `a_int_value` 和 `b_int_value` 列执行位异或运算。  
  
```sql  
SELECT a_int_value ^ b_int_value  
FROM bitwise;  
GO  
```  
  
 下面是结果集：  
  
```  
-----------   
225           
  
(1 row(s) affected)  
```  
  
 170（`a_int_value` 或 `A`）的二进制表示形式是 `0000 0000 1010 1010`。 75（`b_int_value` 或 `B`）的二进制表示形式是 `0000 0000 0100 1011`。 在这两个值之间执行位异或运算所产生的二进制结果是 `0000 0000 1110 0001`，即十进制数 225。  
  
```  
(A ^ B)     
         0000 0000 1010 1010  
         0000 0000 0100 1011  
         -------------------  
         0000 0000 1110 0001  
```  
  

  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [位运算符 (Transact-SQL)](../../t-sql/language-elements/bitwise-operators-transact-sql.md)   
 [^=（位异或赋值）(Transact-SQL)](../../t-sql/language-elements/bitwise-exclusive-or-equals-transact-sql.md)   
 [复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)  
  
  


