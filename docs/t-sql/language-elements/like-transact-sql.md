---
description: LIKE (Transact-SQL)
title: LIKE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ESCAPE
- LIKE
- ESCAPE_TSQL
- LIKE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ESCAPE keyword
- '% (wildcard - character(s) to match)'
- ASCII pattern matching
- pattern searching [SQL Server]
- wildcard characters [SQL Server]
- literals [SQL Server], using wildcards
- character strings [SQL Server], LIKE
- exact matches [SQL Server]
- Boolean functions
- LIKE comparisons
- matching patterns [SQL Server]
- NOT LIKE keyword
ms.assetid: 581fb289-29f9-412b-869c-18d33a9e93d5
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9bb44e61a98ca19424a39df9abd17a9c7b9e0be9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749817"
---
# <a name="like-transact-sql"></a>LIKE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  确定特定字符串是否与指定模式相匹配。 模式可以包含常规字符和通配符。 模式匹配过程中，常规字符必须与字符串中指定的字符完全匹配。 但是，通配符可以与字符串的任意部分相匹配。 与使用 = 和 != 字符串比较运算符相比，使用通配符可使 LIKE 运算符更加灵活。 如果任一参数都不属于字符串数据类型，[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]会尽量将它转换为使用字符串数据类型。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
match_expression [ NOT ] LIKE pattern [ ESCAPE escape_character ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
match_expression [ NOT ] LIKE pattern  
```  
>[!NOTE]
> [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中当前不支持 ESCAPE 和 STRING_ESCAPE。

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 match_expression  
 为任何有效的字符数据类型的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
 *pattern*  
 要在 match_expression 中搜索并且可以包括下列有效通配符的特定字符串。 pattern 的最大长度可达 8,000 字节。  
  
|通配符|描述|示例|  
|------------------------|-----------------|-------------|  
|%|包含零个或多个字符的任意字符串。|WHERE title LIKE '%computer%' 将查找在书名中任意位置包含单词 "computer" 的所有书名。|  
|_（下划线）|任何单个字符。|WHERE au_fname LIKE '_ean' 将查找以 ean 结尾的所有 4 个字母的名字（Dean、Sean 等）。|  
|[ ]|指定范围 ([a-f]) 或集合 ([abcdef]) 中的任何单个字符。|WHERE au_lname LIKE '[C-P]arsen' 将查找以 arsen 结尾并且以介于 C 与 P 之间的任何单个字符开始的作者姓氏，例如 Carsen、Larsen、Karsen 等。 在范围搜索中，范围包含的字符可能因排序规则的排序规则而异。|  
|[^]|不属于指定范围 ([a-f]) 或集合 ([abcdef]) 的任何单个字符。|WHERE au_lname LIKE 'de[^l]%' 查找以 de 开头且后跟字母不是 l 的所有作者姓氏。|  
  
 escape_character  
 位于通配符前的字符，用于指明应将通配符解释为常规字符，而不是通配符。 escape_character 是字符表达式，无默认值，并且计算结果必须仅为一个字符。  
  
## <a name="result-types"></a>结果类型  
 **布尔值**  
  
## <a name="result-value"></a>结果值  
 如果 match_expression 与指定的 pattern 相匹配，则 LIKE 返回 TRUE。  
  
## <a name="remarks"></a>备注  
 如果你使用 LIKE 执行字符串比较，模式字符串中的所有字符都有意义。 有意义的字符包括任何前导或尾随空格。 如果查询中的比较要返回包含字符串 LIKE 'abc '（abc 后跟一个空格）的所有行，则不会返回列值为 abc（abc 后没有空格）的行。 但是可以忽略模式所要匹配的表达式中的尾随空格。 如果查询中的比较要返回包含 "abc"（abc 后没有空格）的所有行，则返回以 "abc" 开始并且具有零个或多个尾随空格的所有行。  
  
 鉴于各数据类型的数据存储方式不同，如果使用包含 char 和 varchar 数据的模式进行字符串比较，可能无法传递 LIKE 比较。 下面的示例先将本地 char 变量传递给存储过程，再使用模式匹配来查找姓氏以一组指定字符开头的所有员工。  
  
```sql
-- Uses AdventureWorks  
  
CREATE PROCEDURE FindEmployee @EmpLName CHAR(20)  
AS  
SELECT @EmpLName = RTRIM(@EmpLName) + '%';  
SELECT p.FirstName, p.LastName, a.City  
FROM Person.Person p JOIN Person.Address a ON p.BusinessEntityID = a.AddressID  
WHERE p.LastName LIKE @EmpLName;  
GO  
EXEC FindEmployee @EmpLName = 'Barb';  
GO  
```  
  
 当名字中包含的字符数小于 20 时，char 变量 (`@EmpLName`) 将包含尾随空格，这导致 `FindEmployee` 过程中没有行返回。 由于 `LastName` 列为 varchar 类型，因此没有尾随空格。 因为尾随空格是有意义的，所以此过程失败。  
  
 但以下示例会成功，因为没有向 varchar 变量中添加尾随空格。  
  
```sql
-- Uses AdventureWorks  
  
CREATE PROCEDURE FindEmployee @EmpLName VARCHAR(20)  
AS  
SELECT @EmpLName = RTRIM(@EmpLName) + '%';  
SELECT p.FirstName, p.LastName, a.City  
FROM Person.Person p JOIN Person.Address a ON p.BusinessEntityID = a.AddressID  
WHERE p.LastName LIKE @EmpLName;  
GO  
EXEC FindEmployee @EmpLName = 'Barb';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
FirstName      LastName            City
----------     -------------------- --------------- 
Angela         Barbariol            Snohomish
David          Barber               Snohomish
(2 row(s) affected)  
```

## <a name="pattern-matching-by-using-like"></a>使用 LIKE 的模式匹配  
 LIKE 支持 ASCII 模式匹配和 Unicode 模式匹配。 如果所有参数（match_expression、pattern 和 escape_character，如果存在）均为 ASCII 字符数据类型，则执行 ASCII 模式匹配。 如果任何一个参数为 Unicode 数据类型，则所有参数都将转换为 Unicode，并执行 Unicode 模式匹配。 如果将 Unicode 数据（nchar 或 nvarchar 数据类型）与 LIKE 结合使用，尾随空格很重要；但对于非 Unicode 数据，尾随空格并不重要。 Unicode LIKE 与 ISO 标准兼容。 ASCII LIKE 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本兼容。  
  
 下面的一系列示例显示 ASCII LIKE 模式匹配与 Unicode LIKE 模式匹配所返回的行之间的差异。  
  
```sql  
-- ASCII pattern matching with char column  
CREATE TABLE t (col1 CHAR(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE col1 LIKE '% King';   -- returns 1 row  
  
-- Unicode pattern matching with nchar column  
CREATE TABLE t (col1 NCHAR(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE col1 LIKE '% King';   -- no rows returned  
  
-- Unicode pattern matching with nchar column and RTRIM  
CREATE TABLE t (col1 NCHAR(30));  
INSERT INTO t VALUES ('Robert King');  
SELECT *   
FROM t   
WHERE RTRIM(col1) LIKE '% King';   -- returns 1 row  
```
  
> [!NOTE]  
>  LIKE 比较受排序规则影响。 有关详细信息，请参阅[排序规则 (Transact-SQL)](~/t-sql/statements/collations.md)。  
  
## <a name="using-the--wildcard-character"></a>使用 % 通配符  
 如果指定 LIKE '5%' 符号，则[!INCLUDE[ssDE](../../includes/ssde-md.md)]将搜索后跟零个或多个任意字符的数字 5。  
  
 例如，以下查询显示 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的所有动态管理视图，因为它们全部以字母 `dm` 开始。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT Name  
FROM sys.system_views  
WHERE Name LIKE 'dm%';  
GO  
```  
  
 若要查看所有非动态管理视图的对象，请使用 `NOT LIKE 'dm%'`。 如果共有 32 个对象，且 LIKE 找到 13 个名称与模式匹配，那么 NOT LIKE 就会找到 19 个对象与 LIKE 模式不匹配。  
  
 使用 `LIKE '[^d][^m]%'` 之类的模式不一定每次找到的名称都相同。 可能仅找到 14 名称（而不是 19 个），除了动态管理视图名称外，所有以 `d` 开始或第二个字母为 `m` 的名称也都将从结果中消除。 之所以出现此行为是因为，用否定通配符匹配字符串是分步骤进行评估，一次评估一个通配符。 如果在评估过程中任一环节匹配失败，它就会被消除。  
  
## <a name="using-wildcard-characters-as-literals"></a>将通配符作为文字使用  
 可以将通配符模式匹配字符作为文字字符使用。 若要将通配符作为文字字符使用，请将通配符放在方括号中。 下表显示了几个使用 LIKE 关键字和 [ ] 通配符的示例。  
  
|符号|含义|  
|------------|-------------|  
|LIKE '5[%]'|5%|  
|LIKE '[_]n'|_n|  
|LIKE '[a-cdf]'|a、b、c、d 或 f|  
|LIKE '[-acdf]'|-、a、c、d 或 f|  
|LIKE '[ [ ]'|[|  
|LIKE ']'|]|  
|LIKE 'abc[_]d%'|abc_d 和 abc_de|  
|LIKE 'abc[def]'|abcd、abce 和 abcf|  
  
## <a name="pattern-matching-with-the-escape-clause"></a>使用 ESCAPE 子句的模式匹配  
 可搜索包含一个或多个特殊通配符的字符串。 例如，customers 数据库中的 discounts 表可能存储含百分号 (%) 的折扣值。 若要搜索作为字符而不是通配符的百分号，必须提供 ESCAPE 关键字和转义符。 例如，一个样本数据库包含名为 comment 的列，该列含文本 30%。 若要搜索在 comment 列中的任何位置包含字符串 30% 的任何行，请指定 `WHERE comment LIKE '%30!%%' ESCAPE '!'` 之类的 WHERE 子句。 如果未指定 ESCAPE 和转义字符，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 返回包含字符串 30! 的所有行。  
  
 如果 LIKE 模式中的转义字符后面没有字符，此模式无效，且 LIKE 返回 FALSE。 如果转义字符后面的字符不是通配符，转义字符会遭放弃，且后面的字符会被视为模式中的常规字符。 这些字符包括百分号 (%)、下划线 (_) 和左方括号 ([) 通配符（如果它们包含在双方括号 ([ ]) 中的话）。 可以在双方括号字符 ([ ]) 内使用转义字符，包括转义脱字号 (^)、连字符 (-) 或右方括号 (])。  
  
 0x0000 (char(0)) 是 Windows 排序规则中未定义的字符，无法添加到 LIKE 中。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-like-with-the--wildcard-character"></a>A. 使用带 % 通配符的 LIKE  
 以下示例在 `415` 表中查找区号为 `PersonPhone` 的所有电话号码。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, ph.PhoneNumber  
FROM Person.PersonPhone AS ph  
INNER JOIN Person.Person AS p  
ON ph.BusinessEntityID = p.BusinessEntityID  
WHERE ph.PhoneNumber LIKE '415%'  
ORDER by p.LastName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
 FirstName             LastName             Phone
 -----------------     -------------------  ------------
 Ruben                 Alonso               415-555-124  
 Shelby                Cook                 415-555-0121  
 Karen                 Hu                   415-555-0114  
 John                  Long                 415-555-0147  
 David                 Long                 415-555-0123  
 Gilbert               Ma                   415-555-0138  
 Meredith              Moreno               415-555-0131  
 Alexandra             Nelson               415-555-0174  
 Taylor                Patterson            415-555-0170  
 Gabrielle              Russell             415-555-0197  
 Dalton                 Simmons             415-555-0115  
 (11 row(s) affected)  
```

### <a name="b-using-not-like-with-the--wildcard-character"></a>B. 使用带 % 通配符的 NOT LIKE  
 以下示例在 `PersonPhone` 表中查找区号不是 `415` 的所有电话号码。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT p.FirstName, p.LastName, ph.PhoneNumber  
FROM Person.PersonPhone AS ph  
INNER JOIN Person.Person AS p  
ON ph.BusinessEntityID = p.BusinessEntityID  
WHERE ph.PhoneNumber NOT LIKE '415%' AND p.FirstName = 'Gail'  
ORDER BY p.LastName;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
FirstName              LastName            Phone
---------------------- -------------------- -------------------
Gail                  Alexander            1 (11) 500 555-0120  
Gail                  Butler               1 (11) 500 555-0191  
Gail                  Erickson             834-555-0132  
Gail                  Erickson             849-555-0139  
Gail                  Griffin              450-555-0171  
Gail                  Moore                155-555-0169  
Gail                  Russell              334-555-0170  
Gail                  Westover             305-555-0100  
(8 row(s) affected)  
```

### <a name="c-using-the-escape-clause"></a>C. 使用 ESCAPE 子句  
 以下示例使用 `ESCAPE` 子句和转义符在 `mytbl2` 表的列 `c1` 中查找精确字符串 `10-15%`。  
  
```sql
USE tempdb;  
GO  
IF EXISTS(SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'mytbl2')  
   DROP TABLE mytbl2;  
GO  
USE tempdb;  
GO  
CREATE TABLE mytbl2  
(  
 c1 sysname  
);  
GO  
INSERT mytbl2 VALUES ('Discount is 10-15% off'), ('Discount is .10-.15 off');  
GO  
SELECT c1   
FROM mytbl2  
WHERE c1 LIKE '%10-15!% off%' ESCAPE '!';  
GO  
```  
  
### <a name="d-using-the---wildcard-characters"></a>D. 使用 [ ] 通配符  
 以下示例将查找 `Person` 表中名字为 `Cheryl` 或 `Sheryl` 的员工。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT BusinessEntityID, FirstName, LastName   
FROM Person.Person   
WHERE FirstName LIKE '[CS]heryl';  
GO  
```  
  
 以下示例查找 `Person` 表中姓氏为 `Zheng` 或 `Zhang` 的员工所对应的行。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT LastName, FirstName  
FROM Person.Person  
WHERE LastName LIKE 'Zh[ae]ng'  
ORDER BY LastName ASC, FirstName ASC;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-like-with-the--wildcard-character"></a>E. 使用带 % 通配符的 LIKE  
 以下示例在 `DimEmployee` 表中查找电话号码以 `612` 开头的所有员工。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone LIKE '612%'  
ORDER by LastName;  
```  
  
### <a name="f-using-not-like-with-the--wildcard-character"></a>F. 使用带 % 通配符的 NOT LIKE  
 下面的示例在 `DimEmployee` 表中查找不以 `612` 开头的所有电话号码。  .  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone NOT LIKE '612%'  
ORDER by LastName;  
```  
  
### <a name="g-using-like-with-the-_-wildcard-character"></a>G. 使用带 _ 通配符的 LIKE  
 以下示例在 `DimEmployee` 表中查找区号以 `6` 开头、以 `2` 结尾的所有电话号码。 搜索模式的末尾包含 % 通配符，用于匹配电话列值中的所有后续字符。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT FirstName, LastName, Phone  
FROM DimEmployee  
WHERE phone LIKE '6_2%'  
ORDER by LastName;   
```  
  
## <a name="see-also"></a>另请参阅  
 [PATINDEX (Transact-SQL)](../../t-sql/functions/patindex-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [WHERE (Transact-SQL)](../../t-sql/queries/where-transact-sql.md)  
