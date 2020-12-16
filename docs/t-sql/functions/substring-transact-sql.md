---
title: SUBSTRING (Transact-SQL) | Microsoft Docs
description: SUBSTRING 函数的 Transact-SQL 参考。 此函数返回指定字符、二进制、文本或图像表达式的一部分。
ms.date: 10/21/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SUBSTRING
- SUBSTRING_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- portion of expression returned [SQL Server]
- part of expression returned [SQL Server]
- SUBSTRING function
- offsets [SQL Server]
- binary [SQL Server], returning part of
- expressions [SQL Server], part returned
- characters [SQL Server], returning part of
ms.assetid: a19c808f-aaf9-4a69-af59-b1a5fc3e5c4c
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2dcdf0ed9fa0e62be66cde8773a741b37ffa3306
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461198"
---
# <a name="substring-transact-sql"></a>SUBSTRING (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的字符、二进制、文本或图像表达式的一部分。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SUBSTRING ( expression ,start , length )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 为 character、binary、text、ntext 或者 image [表达式](../../t-sql/language-elements/expressions-transact-sql.md)      。  
  
 *start*  
 指定返回字符的起始位置的整数或 bigint 表达式  。 （编号从 1 开始，意味着表达式中的第一个字符为 1）。 如果 start 小于 1，则返回的表达式的起始位置为表达式中指定的第一个字符   。 在这种情况下，返回的字符数是后两者中的较大值：*start* + *length* 之和减去 1，0。 如果 start 大于值表达式中的字符数，将返回一个零长度的表达式  。  
  
 *length*  
 是正整数或用于指定要返回的 expression 的字符数的 bigint 表达式   。 如果 length 是负数，会生成错误并终止语句  。 如果 start 和 length 的总和大于表达式中的字符数，则会返回从 start 开始的整个值表达式     。  
  
## <a name="return-types"></a>返回类型  
 如果 expression 是支持的字符数据类型之一，则返回字符数据  。 如果 expression 是 binary 支持的字符数据类型之一，则返回字符数据   。 返回的字符串类型与指定表达式的类型相同（表中显示的除外）。  
  
|指定的表达式|返回类型|  
|--------------------------|-----------------|  
|**char**/**varchar**/**text**|**varchar**|  
|**nchar**/**nvarchar**/**ntext**|**nvarchar**|  
|**binary**/**varbinary**/**image**|**varbinary**|  
  
## <a name="remarks"></a>备注  
 start 和 length 的值对于 ntext、char 或 varchar 数据类型必须以字符数指定，对于 text、image、binary 或 varbinary 数据类型，则以字节数指定          。  
  
 start 或 length 包含大于 2147483647 的值时，expression 必须是 varchar(max) 或 varbinary(max)      。  
  
## <a name="supplementary-characters-surrogate-pairs"></a>补充字符（代理项对）  
 在使用增补字符 (SC) 排序规则时，start 和 length 将 expression 中的每个代理项对计为一个字符    。 有关详细信息，请参阅 [排序规则和 Unicode 支持](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-substring-with-a-character-string"></a>A. 对字符串使用 SUBSTRING  
 以下示例说明如何仅返回字符串的一部分。 从 `sys.databases` 表中，此查询返回第一列中的系统数据库名称、第二列中的数据库的第一个字母和最后一列中的第三和第四个字符。  
  
```sql
SELECT name, SUBSTRING(name, 1, 1) AS Initial ,
SUBSTRING(name, 3, 2) AS ThirdAndFourthCharacters
FROM sys.databases  
WHERE database_id < 5;   
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

|name |初始 |ThirdAndFourthCharacters|
|---|--|--|
|主    |m    |st |
|tempdb    |t    |mp |
|模型    |m    |de |
|msdb    |m    |db |


  
 以下示例说明如何显示字符串常量 `abcdef` 中的第二个、第三个和第四个字符。  
  
```sql
SELECT x = SUBSTRING('abcdef', 2, 3);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x  
----------  
bcd  
  
(1 row(s) affected)
```  
  
### <a name="b-using-substring-with-text-ntext-and-image-data"></a>B. 对 text、ntext 和 image 数据使用 SUBSTRING  
  
> [!NOTE]  
>  若要运行以下示例，必须安装 pubs 数据库  。  
  
 以下示例说明如何返回 `pubs` 数据库的 `pub_info` 表内每个 text 和 image 数据列的前 10 个字符   。 text 数据返回为 varchar，而 image 数据返回为 varbinary     。  
  
```sql
USE pubs;  
SELECT pub_id, SUBSTRING(logo, 1, 10) AS logo,   
   SUBSTRING(pr_info, 1, 10) AS pr_info  
FROM pub_info  
WHERE pub_id = '1756';  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 pub_id logo    pr_info
------ ---------------------- ----------
1756   0x474946383961E3002500 This is sa

(1 row(s) affected)
```  
  
 以下示例说明 SUBSTRING 对 text 和 ntext 数据的影响   。 首先，该示例在 `pubs` 数据库内创建一个名为 `npub_info` 的新表。 接着，该示例使用 `pr_info` 列的前 80 个字符在 `npub_info` 表中创建 `pub_info.pr_info` 列，然后将添加 `ü` 为第一个字符。 最后，`INNER JOIN` 检索所有出版商标识号以及 text 和 ntext 出版商信息列的`SUBSTRING`  。  
  
```sql
IF EXISTS (SELECT table_name FROM INFORMATION_SCHEMA.TABLES   
      WHERE table_name = 'npub_info')  
   DROP TABLE npub_info;  
GO  
-- Create npub_info table in pubs database. Borrowed from instpubs.sql.  
USE pubs;  
GO  
CREATE TABLE npub_info  
(  
 pub_id CHAR(4) NOT NULL  
    REFERENCES publishers(pub_id)  
    CONSTRAINT UPKCL_npubinfo PRIMARY KEY CLUSTERED,  
pr_info ntext NULL  
);  
  
GO  
  
-- Fill the pr_info column in npub_info with international data.  
RAISERROR('Now at the inserts to pub_info...',0,1);  
  
GO  
  
INSERT npub_info VALUES('0736', N'üThis is sample text data for New Moon Books, publisher 0736 in the pubs database')  
,('0877', N'üThis is sample text data for Binnet & Hardley, publisher 0877 in the pubs databa')  
,('1389', N'üThis is sample text data for Algodata Infosystems, publisher 1389 in the pubs da')  
,('9952', N'üThis is sample text data for Scootney Books, publisher 9952 in the pubs database')  
,('1622', N'üThis is sample text data for Five Lakes Publishing, publisher 1622 in the pubs d')  
,('1756', N'üThis is sample text data for Ramona Publishers, publisher 1756 in the pubs datab')  
,('9901', N'üThis is sample text data for GGG&G, publisher 9901 in the pubs database. GGG&G i')  
,('9999', N'üThis is sample text data for Lucerne Publishing, publisher 9999 in the pubs data');  
GO  
-- Join between npub_info and pub_info on pub_id.  
SELECT pr.pub_id, SUBSTRING(pr.pr_info, 1, 35) AS pr_info,  
   SUBSTRING(npr.pr_info, 1, 35) AS npr_info  
FROM pub_info pr INNER JOIN npub_info npr  
   ON pr.pub_id = npr.pub_id  
ORDER BY pr.pub_id ASC;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-substring-with-a-character-string"></a>C. 对字符串使用 SUBSTRING  
 以下示例说明如何仅返回字符串的一部分。 该查询在一列中返回 `dbo.DimEmployee` 表中的姓氏，在另一列中只返回名字首字母。  
  
```sql
-- Uses AdventureWorks  
  
SELECT LastName, SUBSTRING(FirstName, 1, 1) AS Initial  
FROM dbo.DimEmployee  
WHERE LastName LIKE 'Bar%'  
ORDER BY LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName             Initial
-------------------- -------
Barbariol            A
Barber               D
Barreto de Mattos    P
```  
  
 以下示例显示如何返回字符串常量 `abcdef` 的第二个、第三个和第四个字符。  
  
```sql
USE ssawPDW;  
  
SELECT TOP 1 SUBSTRING('abcdef', 2, 3) AS x FROM dbo.DimCustomer;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
x
-----
bcd
```  
  
## <a name="see-also"></a>另请参阅  
 [LEFT (Transact-SQL)](../../t-sql/functions/left-transact-sql.md)  
 [LTRIM (Transact-SQL)](../../t-sql/functions/ltrim-transact-sql.md)  
 [RIGHT (Transact-SQL)](../../t-sql/functions/right-transact-sql.md)  
 [RTRIM (Transact-SQL)](../../t-sql/functions/rtrim-transact-sql.md)  
 [STRING_SPLIT (Transact-SQL)](../../t-sql/functions/string-split-transact-sql.md)  
 [TRIM (Transact-SQL)](../../t-sql/functions/trim-transact-sql.md)  
 [字符串函数 (Transact-SQL)](../../t-sql/functions/string-functions-transact-sql.md)  
  
  


