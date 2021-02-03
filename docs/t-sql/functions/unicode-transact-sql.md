---
description: UNICODE (Transact-SQL)
title: UNICODE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UNICODE
- UNICODE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- first character of input expression [SQL Server]
- UNICODE function
- Unicode [SQL Server], UNICODE function
ms.assetid: 5e3c40b2-8401-4741-9f2a-bae70eaa4da6
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b20be14a154789c5afc26f52b1e2e02e491f9fc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210673"
---
# <a name="unicode-transact-sql"></a>UNICODE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  按照 Unicode 标准的定义，返回输入表达式的第一个字符的整数值。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
UNICODE ( 'ncharacter_expression' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
**'** *ncharacter_expression* **'**  
是 nchar 或 nvarchar 表达式。  
  
## <a name="return-types"></a>返回类型  
**int**  
  
## <a name="remarks"></a>备注  
在 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 之前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本以及 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，UNICODE 函数返回范围 000000 - 00FFFF（它能够表示 Unicode 基本多文种平面 (BMP) 中的 65,535 个字符）内的 UCS-2 码位。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 开始，若使用启用了[补充字符 (SC)](../../relational-databases/collations/collation-and-unicode-support.md#Supplementary_Characters) 的排序规则时，UNICODE 会返回一个在 000000 到 10FFFF 范围内的 UTF-16 码位。 有关 [!INCLUDE[ssde_md](../../includes/ssde_md.md)] 中的 Unicode 支持的详细信息，请参阅[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md#Unicode_Defn)。 
  
## <a name="examples"></a>示例  
  
### <a name="a-using-unicode-and-the-nchar-function"></a>A. 使用 UNICODE 和 NCHAR 函数  
 以下示例使用 `UNICODE` 和 `NCHAR` 函数输出 `Åkergatan` 24 字符串中第一个字符的 UNICODE 值，并输出实际的第一个字符 `Å`。  
  
```sql  
DECLARE @nstring NCHAR(12);  
SET @nstring = N'Åkergatan 24';  
SELECT UNICODE(@nstring), NCHAR(UNICODE(@nstring));  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
----------- -   
197         Å  
```  
  
### <a name="b-using-substring-unicode-and-convert"></a>B. 使用 SUBSTRING、UNICODE 和 CONVERT  
 下面的示例使用 `SUBSTRING`、`UNICODE` 和 `CONVERT` 函数输出 `Åkergatan 24` 字符串中每个字符的字符号、Unicode 字符和 UNICODE 值。  
  
```sql  
-- The @position variable holds the position of the character currently  
-- being processed. The @nstring variable is the Unicode character   
-- string to process.  
DECLARE @position INT, @nstring NCHAR(12);  
-- Initialize the current position variable to the first character in   
-- the string.  
SET @position = 1;  
-- Initialize the character string variable to the string to process.   
-- Notice that there is an N before the start of the string, which   
-- indicates that the data following the N is Unicode data.  
SET @nstring = N'Åkergatan 24';  
-- Print the character number of the position of the string you are at,   
-- the actual Unicode character you are processing, and the UNICODE   
-- value for this particular character.  
PRINT 'Character #' + ' ' + 'Unicode Character' + ' ' + 'UNICODE Value';  
WHILE @position <= LEN(@nstring)  
-- While these are still characters in the character string,  

BEGIN;  
   SELECT @position AS [position],   
      SUBSTRING(@nstring, @position, 1) AS [character],  
      UNICODE(SUBSTRING(@nstring, @position, 1)) AS [code_point];  
   SET @position = @position + 1;  
END; 
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Character # Unicode Character UNICODE Value  
  
----------- ----------------- -----------   
1           Å                 197           
  
----------- ----------------- -----------   
2           k                 107           
  
----------- ----------------- -----------   
3           e                 101           
  
----------- ----------------- -----------   
4           r                 114           
  
----------- ----------------- -----------   
5           g                 103           
  
----------- ----------------- -----------   
6           a                 97            
  
----------- ----------------- -----------   
7           t                 116           
  
----------- ----------------- -----------   
8           a                 97            
  
----------- ----------------- -----------   
9           n                 110           
  
----------- ----------------- -----------   
10                            32            
  
----------- ----------------- -----------   
11          2                 50            
  
----------- ----------------- -----------   
12          4                 52  
```  
  
## <a name="see-also"></a>另请参阅  
 [ASCII (Transact-SQL)](../../t-sql/functions/ascii-transact-sql.md)  
 [CHAR (Transact-SQL)](../../t-sql/functions/char-transact-sql.md)  
 [NCHAR (Transact-SQL)](../../t-sql/functions/nchar-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)  
  
  

