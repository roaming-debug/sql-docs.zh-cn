---
description: LEN (Transact-SQL)
title: LEN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/03/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- LEN
- LEN_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- LEN function
- characters [SQL Server], number of
- number of characters
ms.assetid: fa20fee4-884d-4301-891a-c03e901345ae
author: pmasl
ms.author: pelopes
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bdcfe164aa08677887c1c494309bcf1d9af4c6d9
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88459689"
---
# <a name="len-transact-sql"></a>LEN (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

返回指定字符串表达式的字符数，其中不包含尾随空格。  
  
> [!NOTE]  
> 若要返回用于表示表达式的字节数，请使用 [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) 函数。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
LEN ( string_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 string_expression  
 要计算的字符串[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。 string_expression 可以是常量、变量，也可以是字符列或二进制数据列。  
  
## <a name="return-types"></a>返回类型  
 bigint（如果 expression 的数据类型为 varchar(max)、nvarchar (max) 或 varbinary(max)）；否则为 int。  
  
 如果使用 SC 排序规则，则返回的整数值将 UTF-16 代理项对作为单个字符计数。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="remarks"></a>备注  
LEN 不包括尾随空格。 如果这是个问题，请考虑使用 [DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md) 函数，该函数不会修整字符串。 如果处理的是 unicode 字符串，DATALENGTH 会返回可能不等于字符数的数字。 以下示例演示 LEN 和带有尾随空格的 DATALENGTH。  
  
```sql  
  DECLARE @v1 VARCHAR(40),  
    @v2 NVARCHAR(40);  
SELECT   
@v1 = 'Test of 22 characters ',   
@v2 = 'Test of 22 characters ';  
SELECT LEN(@v1) AS [VARCHAR LEN] , DATALENGTH(@v1) AS [VARCHAR DATALENGTH];  
SELECT LEN(@v2) AS [NVARCHAR LEN], DATALENGTH(@v2) AS [NVARCHAR DATALENGTH];  
```  

> [!NOTE]
> 使用 [LEN](../../t-sql/functions/len-transact-sql.md) 返回编码到给定字符串表达式中的字符数，使用 [DATALENGTH](../../t-sql/functions/datalength-transact-sql.md) 返回给定字符串表达式的字节大小。 这些输出可能会因列中使用的数据类型和编码类型而异。 若要详细了解不同编码类型的存储区别，请参阅[排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。

## <a name="examples"></a>示例  
 以下示例在 `FirstName` 地区的人的 `Australia` 中选择字符数和数据。 本示例使用 AdventureWorks 数据库。  
  
```sql  
SELECT LEN(FirstName) AS Length, FirstName, LastName   
FROM Sales.vIndividualCustomer  
WHERE CountryRegionName = 'Australia';  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 以下示例返回 `FirstName` 列中的字符数，以及位于 `Australia` 中的员工的姓氏和名字。  
  
```sql  
USE AdventureWorks2016  
GO  
SELECT DISTINCT LEN(FirstName) AS FNameLength, FirstName, LastName   
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.DimGeography AS g   
    ON e.SalesTerritoryKey = g.SalesTerritoryKey   
WHERE EnglishCountryRegionName = 'Australia';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
FNameLength  FirstName  LastName  
-----------  ---------  ---------------  
4            Lynn       Tsoflias
```  
  
## <a name="see-also"></a>另请参阅  
 [DATALENGTH (Transact-SQL)](../../t-sql/functions/datalength-transact-sql.md)   
 [CHARINDEX (Transact-SQL)](../../t-sql/functions/charindex-transact-sql.md)  
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)  
 [LEFT (Transact-SQL)](../../t-sql/functions/left-transact-sql.md)   
 [RIGHT (Transact-SQL)](../../t-sql/functions/right-transact-sql.md)  
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)   
  
  
