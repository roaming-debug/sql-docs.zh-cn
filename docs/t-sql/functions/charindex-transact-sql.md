---
title: CHARINDEX (Transact-SQL) | Microsoft Docs
description: CHARINDEX 函数的 Transact-SQL 参考。
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CHARINDEX
- CHARINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], pattern searching
- CHARINDEX function
- pattern searching [SQL Server]
- starting point of expression in character string
ms.assetid: 78c10341-8373-4b30-b404-3db20e1a3ac4
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1ec1919c5b9b60e4109b7f76463dab21fd1e50b3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749997"
---
# <a name="charindex-transact-sql"></a>CHARINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

此函数会在第二个字符表达式中搜索一个字符表达式，这将返回第一个表达式（如果发现存在）的开始位置。
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
expressionToFind   
一个字符[表达式](../../t-sql/language-elements/expressions-transact-sql.md)，其中包含要查找的序列。 expressionToFind 限制为 8000 个字符  。
  
expressionToSearch   
要搜索的字符表达式。
  
start_location   
表示搜索开始位置的 integer 或 bigint 表达式   。 如果 start_location 未指定、具有负数值或 0，搜索将从 expressionToSearch 的开头开始   。
  
## <a name="return-types"></a>返回类型
如果 expressionToSearch 具有一个 nvarchar(max)、varbinary(max) 或 varchar(max) 数据类型，则为 bigint；否则为 int       。
  
## <a name="remarks"></a>备注  
如果 expressionToFind 或 expressionToSearch 表达式具有一个 Unicode 数据类型（nchar 或 nvarchar），而其他的表达式不具有，CHARINDEX 函数则会将其他表达式转换为一个 Unicode 数据类型     。 CHARINDEX 不能与 image、ntext 和 text 数据类型一起使用    。
  
如果 expressionToFind 或 expressionToSearch 表达式具有 NULL 值，CHARINDEX 则返回 NULL   。
  
如果 CHARINDEX 在 expressionToSearch 中找不到 expressionToFind，CHARINDEX 则返回 0   。
  
CHARINDEX 根据输入排序规则执行比较操作。 若要以指定的排序规则执行比较，可以使用 COLLATE 将显式排序规则应用于输入。
  
返回的起始位置从 1 开始，而不是从 0 开始。
  
0x0000 (char(0)) 是 Windows 排序规则中未定义的字符，不能包括在 CHARINDEX 中  。
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
在使用 SC 排序规则时，start_location 和返回值将代理项对计为一个字符，而不是计为两个字符  。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。
  
## <a name="examples"></a>示例  
  
### <a name="a-returning-the-starting-position-of-an-expression"></a>A. 返回表达式的起始位置  
此示例将在搜索的字符串值变量 `@document` 中搜索 `bicycle`。
  
```sql
DECLARE @document VARCHAR(64);  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bicycle', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
48            
```  
  
### <a name="b-searching-from-a-specific-position"></a>B. 从特定位置中搜索  
此示例使用可选的 start_location 参数在搜索的字符串值变量 `@document` 的第五个字符处开始搜索 `vital` 。
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('vital', @document, 5);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------   
16            
  
(1 row(s) affected)  
```  
  
### <a name="c-searching-for-a-nonexistent-expression"></a>C. 搜索不存在的表达式  
此示例显示 CHARINDEX 在 expressionToSearch 中找不到 expressionToFind 时的结果集   。
  
```sql
DECLARE @document VARCHAR(64);  
  
SELECT @document = 'Reflectors are vital safety' +  
                   ' components of your bicycle.';  
SELECT CHARINDEX('bike', @document);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
  
(1 row(s) affected)
```
  
### <a name="d-performing-a-case-sensitive-search"></a>D. 执行区分大小写的搜索  
此示例在搜索的字符串 `'This is a Test``'` 中执行区分大小写的字符串 `'TEST'` 搜索。
  
```sql
USE tempdb;  
GO  
--perform a case sensitive search  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
0
```  
  
此示例在 `'This is a Test'` 中执行区分大小写的字符串 `'Test'` 搜索。
  
```sql
  
USE tempdb;  
GO  
SELECT CHARINDEX ( 'Test',  
       'This is a Test'  
       COLLATE Latin1_General_CS_AS);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
### <a name="e-performing-a-case-insensitive-search"></a>E. 执行不区分大小写的搜索  
此示例在 `'This is a Test'` 中执行不区分大小写的字符串 `'TEST'` 搜索。
  
```sql
USE tempdb;  
GO  
SELECT CHARINDEX ( 'TEST',  
       'This is a Test'  
       COLLATE Latin1_General_CI_AS);  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
-----------
11
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="f-searching-from-the-start-of-a-string-expression"></a>F. 从字符串表达式的开头搜索  
此示例返回字符串 `This is a string` 中字符串 `is` 的第一个位置，从 `This is a string` 的位置 1（第一个字符）开始。
  
```sql
SELECT CHARINDEX('is', 'This is a string');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
3
```  
  
### <a name="g-searching-from-a-position-other-than-the-first-position"></a>G. 从第一个位置以外的位置搜索  
此示例返回字符串 `This is a string` 中字符串 `is` 的第一个位置，从位置 4（第四个字符）开始进行搜索。
  
```sql
SELECT CHARINDEX('is', 'This is a string', 4);  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
 6
 ```  
  
### <a name="h-results-when-the-string-is-not-found"></a>H. 未找到字符串时的结果  
此示例显示 CHARINDEX 在搜索的字符串中找不到字符串 string_pattern 时的返回值  。
  
```sql
SELECT TOP(1) CHARINDEX('at', 'This is a string') FROM dbo.DimCustomer;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
---------
0
```  
  
## <a name="see-also"></a>另请参阅
 [LEN (Transact-SQL)](../../t-sql/functions/len-transact-sql.md)  
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)  
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
 [+（字符串串联）(Transact-SQL)](../../t-sql/language-elements/string-concatenation-transact-sql.md)  
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  


