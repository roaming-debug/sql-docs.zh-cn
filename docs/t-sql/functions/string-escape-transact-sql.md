---
description: STRING_ESCAPE (Transact-SQL)
title: STRING_ESCAPE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STRING_ESCAPE
- STRING_ESCAPE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_ESCAPE function
ms.assetid: 2163bc7a-3816-4304-9c40-8954804f5465
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: de472b06f2d6747ced93a36f3563188e3d537d36
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461218"
---
# <a name="string_escape-transact-sql"></a>STRING_ESCAPE (Transact-SQL)


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

对文本中的特殊字符进行转义并返回有转义字符的文本。 STRING_ESCAPE 是 SQL Server 2016 中引入的一个确定性的函数。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
STRING_ESCAPE( text , type )  
```  

## <a name="arguments"></a>参数

 *text*  
 表示应转义对象的 nvarchar [表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
 *type*  
 对将要应用的规则进行转义。 目前支持的值是 `'json'`。  
  
## <a name="return-types"></a>返回类型

 带有已转义的特殊和控制字符的 nvarchar(max) 文本。 当前，STRING_ESCAPE 仅能对下表中的 JSON 特殊字符进行转义。  
  
|特殊字符|编码的序列|  
|-----------------------|----------------------|  
|引号 (")|\\"|  
|反斜线号 (\\)| \\\\ |  
|斜线号 (/)|\\/|  
|Backspace|\b|  
|换页|\f|  
|换行|\n|  
|回车|\r|  
|水平制表符|\t|  
  
|控制字符|编码的序列|  
|-----------------------|----------------------|  
|CHAR(0)|\u0000|  
|CHAR(1)|\u0001|  
|...|...|  
|CHAR(31)|\u001f|  
  
## <a name="remarks"></a>备注  
  
## <a name="examples"></a>示例  
  
### <a name="a--escape-text-according-to-the-json-formatting-rules"></a>A.  根据 JSON 格式设置对文本进行转义

 以下查询使用 JSON 规则对特殊字符进行转义，并返回已转义的文本。  
  
```sql
SELECT STRING_ESCAPE('\   /  
\\    "     ', 'json') AS escapedText;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
escapedText  
-------------------------------------------------------------  
\\\t\/\n\\\\\t\"\t
```  
  
### <a name="b-format-json-object"></a>B. 设置 JSON 对象格式

 下面的查询从数字和字符串变量创建 JSON 文本，并对变量中的任意特殊 JSON 字符进行转义。  
  
```
SET @json = FORMATMESSAGE('{ "id": %d,"name": "%s", "surname": "%s" }',
    17, STRING_ESCAPE(@name,'json'), STRING_ESCAPE(@surname,'json') );  
```  
  
## <a name="see-also"></a>另请参阅

- [CONCAT (Transact-SQL)](../../t-sql/functions/concat-transact-sql.md)  
- [CONCAT_WS (Transact-SQL)](../../t-sql/functions/concat-ws-transact-sql.md)  
- [FORMATMESSAGE (Transact-SQL)](../../t-sql/functions/formatmessage-transact-sql.md)  
- [QUOTENAME (Transact-SQL)](../../t-sql/functions/quotename-transact-sql.md)  
- [REPLACE (Transact-SQL)](../../t-sql/functions/replace-transact-sql.md)  
- [REVERSE (Transact-SQL)](../../t-sql/functions/reverse-transact-sql.md)  
- [STRING_AGG (Transact-SQL)](../../t-sql/functions/string-agg-transact-sql.md)  
- [STUFF (Transact-SQL)](../../t-sql/functions/stuff-transact-sql.md)  
- [TRANSLATE (Transact-SQL)](../../t-sql/functions/translate-transact-sql.md)  
- [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)
