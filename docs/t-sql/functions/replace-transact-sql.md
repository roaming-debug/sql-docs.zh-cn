---
title: REPLACE (Transact-SQL) | Microsoft Docs
description: REPLACE 函数的 Transact-SQL 参考，该函数将出现的所有指定字符串值替换为另一个字符串值。
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REPLACE_TSQL
- REPLACE
dev_langs:
- TSQL
helpviewer_keywords:
- first string expression [SQL Server]
- replacing string expression
- third string expressions [SQL Server]
- second string expressions [SQL Server]
- REPLACE function
ms.assetid: 8a7aaaf2-62e3-46c0-8e44-fa22290dd86b
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbd685e29e9773eea41be391b1bdfd7b1c66fdb6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182298"
---
# <a name="replace-transact-sql"></a>REPLACE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

用另一个字符串值替换出现的所有指定字符串值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REPLACE ( string_expression , string_pattern , string_replacement )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 string_expression   
 是要搜索的字符串[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 string_expression 可以是字符或二进制数据类型  。  
  
 *string\_pattern*  
 是要查找的子字符串。 string_pattern 可以是字符或二进制数据类型  。 string_pattern 不能为空字符串 ('')，不能超过页容纳的最大字节数  。  
  
 *string\_replacement*  
 是替换字符串。 string_replacement 可以是字符或二进制数据类型  。  
  
## <a name="return-types"></a>返回类型  
 如果其中的一个输入参数数据类型为 nvarchar，则返回 nvarchar；否则 REPLACE 返回 varchar    。  
  
 如果任何一个参数为 NULL，则返回 NULL。  
  
 如果 string_expression 的类型不是 varchar(max) 或 nvarchar(max)，则 REPLACE 将返回值截断为 8000 个字节    。 若要返回大于 8,000 字节的值，则必须将 string_expression 显式转换为大值数据类型  。  
  
## <a name="remarks"></a>备注  
 REPLACE 根据输入的排序规则执行比较操作。 若要以指定排序规则进行比较，则可以使用 [COLLATE](~/t-sql/statements/collations.md) 将显式排序规则应用于输入。  
  
 0x0000 (char(0)) 是 Windows 排序规则中未定义的字符，不能包括在 REPLACE 中  。  
  
## <a name="examples"></a>示例  
 以下示例使用 `cde` 替换 `abcdefghi` 中的字符串 `xxx`。  
  
```sql  
SELECT REPLACE('abcdefghicde','cde','xxx');  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
abxxxfghixxx  
(1 row(s) affected)  
```  
  
 下面的示例使用 `COLLATE` 函数。  
  
```sql  
SELECT REPLACE('This is a Test'  COLLATE Latin1_General_BIN,  
'Test', 'desk' );  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
------------  
This is a desk  
(1 row(s) affected)  
```  

  
## <a name="see-also"></a>另请参阅  
 [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
 [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)  
 [FORMATMESSAGE (Transact-SQL)](../../t-sql/functions/formatmessage-transact-sql.md)  
 [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
 [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
 [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
 [STRING_ESCAPE (Transact-SQL)](../../t-sql/functions/string-escape-transact-sql.md)  
 [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
 [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
