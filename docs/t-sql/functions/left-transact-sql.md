---
description: LEFT (Transact-SQL)
title: LEFT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- LEFT
- LEFT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- character strings [SQL Server], LEFT
- characters [SQL Server], leftmost
- LEFT function
- leftmost character of expression
ms.assetid: 44a8c71b-63d8-458b-8b5d-99d570067c3c
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92014df1a2c761690c0b5371f16f4f29f8b9e167
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749307"
---
# <a name="left-transact-sql"></a>LEFT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回字符串中从左边开始指定个数的字符。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
LEFT ( character_expression , integer_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *character_expression*  
 字符或二进制数据的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 character_expression 可以是常量、变量或列。 character_expression 可以是除 text 或 ntext 外的任何数据类型，可隐式转换为 varchar 或 nvarchar   。 否则，请使用 [CAST](../../t-sql/functions/cast-and-convert-transact-sql.md) 函数显式转换 character_expression。  
 
> [!NOTE]  
> 如果 string_expression 的类型为二进制或 varbinary，则 LEFT 将执行到 varchar 的隐式转换，因此不会保留二进制输入  。  
  
 *integer_expression*  
 指定要返回的 character_expression 的字符数的正整数。 如果 integer_expression 为负，则返回错误。 如果 integer_expression 的数据类型为 bigint，且包含较大的值，则 character_expression 必须是较大的数据类型，如 varchar(max)。  
  
 integer_expression 参数将 UTF-16 代理项字符计为一个字符。  
  
## <a name="return-types"></a>返回类型  
 character_expression 为非 Unicode 字符数据类型时，返回 varchar。  
  
 character_expression 为 Unicode 字符数据类型时，返回 nvarchar。  
  
## <a name="remarks"></a>备注  
 在使用 SC 排序规则时，integer_expression 参数将 UTF-16 代理项对计为一个字符。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-left-with-a-column"></a>A. 带列使用 LEFT  
 以下示例返回 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库的 `Product` 表中每个产品名称中最左边的五个字符。  
  
```sql  
SELECT LEFT(Name, 5)   
FROM Production.Product  
ORDER BY ProductID;  
GO  
```  
  
### <a name="b-using-left-with-a-character-string"></a>B. 带字符串使用 LEFT  
 以下示例使用 `LEFT` 函数返回字符串 `abcdefg` 中最左边的两个字符。  
  
```sql  
SELECT LEFT('abcdefg',2);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab   
  
(1 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-left-with-a-column"></a>C. 带列使用 LEFT  
 以下示例返回每个产品名中最左边的五个字符。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT(EnglishProductName, 5)   
FROM dbo.DimProduct  
ORDER BY ProductKey;  
```  
  
### <a name="d-using-left-with-a-character-string"></a>D. 带字符串使用 LEFT  
 以下示例使用 `LEFT` 函数返回字符串 `abcdefg` 中最左边的两个字符。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LEFT('abcdefg',2) FROM dbo.DimProduct;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--   
ab  
```  
  
## <a name="see-also"></a>另请参阅  
 [LTRIM (Transact-SQL)](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT (Transact-SQL)](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM (Transact-SQL)](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT (Transact-SQL)](../../t-sql/functions/string-split-transact-sql.md)  
 [SUBSTRING (Transact-SQL)](../../t-sql/functions/substring-transact-sql.md)  
 [TRIM (Transact-SQL)](../../t-sql/functions/trim-transact-sql.md)  
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
  

