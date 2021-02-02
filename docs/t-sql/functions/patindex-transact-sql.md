---
description: PATINDEX (Transact-SQL)
title: PATINDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- PATINDEX
- PATINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- first occurrence of pattern [SQL Server]
- searches [SQL Server], pattern starting position
- starting position of patten search
- pattern searching [SQL Server]
- PATINDEX function
ms.assetid: c0dfb17f-2230-4e36-98da-a9b630bab656
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2433b4bc3e83392bd77c58aed5c4fb48d71971df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99120961"
---
# <a name="patindex-transact-sql"></a>PATINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回模式在指定表达式中第一次出现的起始位置；如果在所有有效的文本和字符数据类型中都找不到该模式，则返回零。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
PATINDEX ( '%pattern%' , expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *pattern*  
 包含要查找的序列的字符表达式。 可以使用通配符；但 pattern 之前和之后必须有 % 字符（搜索第一个或最后一个字符时除外）。 pattern 是字符串数据类型类别的表达式。 pattern最多包含 8000 个字符。

 > [!NOTE]
 > 虽然传统正则表达式在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中不受本机支持，但可以使用各种通配符表达式来实现类似的复杂模式匹配。 有关通配符语法的更多详细信息，请参阅[字符串运算符](../../t-sql/language-elements/string-operators-transact-sql.md)文档。
  
 *expression*  
 是一个[expression](../../t-sql/language-elements/expressions-transact-sql.md)，通常是针对指定模式搜索的列。 expression 是字符串数据类型类别的表达式。  
  
## <a name="return-types"></a>返回类型  
bigint（如果 expression 的数据类型为 varchar(max) 或 nvarchar(max)）；否则为 int。  
  
## <a name="remarks"></a>注解  
如果 pattern 或 expression 为 NULL，则 PATINDEX 返回 NULL。  
 
PATINDEX 的起始位置为 1。
 
PATINDEX 基于输入的排序规则执行比较。 若要以指定排序规则进行比较，则可以使用 COLLATE 将显式排序规则应用于输入。  
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
在使用 SC 排序规则时，返回值将 expression 参数中的任何 UTF-16 代理项对计为一个字符。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
0x0000 (char(0)) 是 Windows 排序规则中未定义的字符，不能包括在 PATINDEX 中。  
  
## <a name="examples"></a>示例  
  
### <a name="a-simple-patindex-example"></a>A. 简单 PATINDEX 示例  
 以下示例检查字符 `ter` 起始位置的短字符串 (`interesting data`)。  
  
```sql  
SELECT position = PATINDEX('%ter%', 'interesting data');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
3
```
  
### <a name="b-using-a-pattern-with-patindex"></a>B. 在 PATINDEX 中使用模式  
以下示例查找模式 `ensure` 在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `DocumentSummary` 表中 `Document` 列特定行中的开始位置。  
  
```sql  
SELECT position = PATINDEX('%ensure%',DocumentSummary)  
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
GO   
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
64  
```  
  
如果未使用 `WHERE` 子句限制要搜索的行，查询将返回表中的所有行，对找到该模式的行报告非零值，对未找到该模式的行报告零值。  
  
### <a name="c-using-wildcard-characters-with-patindex"></a>C. 在 PATINDEX 中使用通配符  
 以下示例使用 % 和 _ 通配符查找模式 `'en'`（后跟任意一个字符和 `'ure'`）在指定字符串中的开始位置（索引从 1 开始）：  
  
```sql  
SELECT position = PATINDEX('%en_ure%', 'Please ensure the door is locked!');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
8  
```  
  
`PATINDEX` 的作用与 `LIKE` 类似，因此，您可以使用任何通配符。 不必将模式放在百分号之间。 `PATINDEX('a%', 'abc')` 将返回 1；`PATINDEX('%a', 'cba')` 将返回 3。  
  
 与 `LIKE` 不同的是，`PATINDEX` 返回一个位置，这与 `CHARINDEX` 类似。  

### <a name="d-using-complex-wildcard-expressions-with-patindex"></a>D. 将复杂的通配符表达式和 PATINDEX 结合使用 
下面的示例使用 `[^]` [字符串运算符](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)查找不是数字、字母或空格的字符的位置。

```sql
SELECT position = PATINDEX('%[^ 0-9A-z]%', 'Please ensure the door is locked!'); 
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
33
```

### <a name="e-using-collate-with-patindex"></a>E. 在 PATINDEX 中使用 COLLATE  
 以下示例使用 `COLLATE` 函数显式指定要搜索的表达式的排序规则。  
  
```sql  
USE tempdb;  
GO  
SELECT PATINDEX ( '%ein%', 'Das ist ein Test'  COLLATE Latin1_General_BIN) ;  
GO  
```  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
9
```

### <a name="f-using-a-variable-to-specify-the-pattern"></a>F. 使用变量指定模式  
下面的示例使用变量将值传递到 pattern 参数。 此示例使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库。  
  
```sql  
DECLARE @MyValue VARCHAR(10) = 'safety';   
SELECT position = PATINDEX('%' + @MyValue + '%', DocumentSummary)   
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
22
```  
  
## <a name="see-also"></a>另请参阅  
 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)   
 [CHARINDEX (Transact-SQL)](../../t-sql/functions/charindex-transact-sql.md)  
 [LEN (Transact-SQL)](../../t-sql/functions/len-transact-sql.md)  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
 [（通配符 - 需匹配的字符）(Transact-SQL)](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [（通配符 - 无需匹配的字符）(Transact-SQL)](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)   
 [_（通配符 - 匹配一个字符）(Transact-SQL)](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)   
 [百分比字符（通配符 - 需匹配的字符）(Transact-SQL)](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)  
  
  


