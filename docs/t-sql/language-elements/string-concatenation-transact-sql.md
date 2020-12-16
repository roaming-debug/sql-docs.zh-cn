---
description: + （字符串串联）(Transact-SQL)
title: + （字符串串联）(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/06/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- concatenation
- +
- string
dev_langs:
- TSQL
helpviewer_keywords:
- concatenation [SQL Server]
- string concatenation operators
- + (string concatenation)
ms.assetid: 35cb3d7a-48f5-4b13-926c-a9d369e20ed7
author: rothja
ms.author: jroth
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1e8263bb5fdf0862019456f20e7a52a342834a78
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464278"
---
# <a name="-string-concatenation-transact-sql"></a>+（字符串串联）(Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  字符串表达式中的运算符，它将两个或多个字符串或二进制字符串、列或字符串和列名的组合串联到一个表达式中（字符串运算符）。  例如，`SELECT 'book'+'case';` 返回 `bookcase`。
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
expression + expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *expression*  
 字符和二进制数据类型类别中的任何一个数据类型的有效[表达式](../../t-sql/language-elements/expressions-transact-sql.md)，但 image、ntext 或 text 数据类型除外。 两个表达式必须具有相同的数据类型，或者其中一个表达式必须能够隐式转换为另一个表达式的数据类型。  
  
 在二进制字符串之间串联二进制字符串和任何字符串时，必须显式转换字符数据。 以下示例显示了对于二进制串联，何时必须使用 `CONVERT` 或 `CAST`，何时不需要使用 `CONVERT` 或 `CAST`。  
  
```sql
DECLARE @mybin1 VARBINARY(5), @mybin2 VARBINARY(5)  
SET @mybin1 = 0xFF  
SET @mybin2 = 0xA5  
-- No CONVERT or CAST function is required because this example   
-- concatenates two binary strings.  
SELECT @mybin1 + @mybin2  
-- A CONVERT or CAST function is required because this example  
-- concatenates two binary strings plus a space.  
SELECT CONVERT(VARCHAR(5), @mybin1) + ' '   
   + CONVERT(VARCHAR(5), @mybin2)  
-- Here is the same conversion using CAST.  
SELECT CAST(@mybin1 AS VARCHAR(5)) + ' '   
   + CAST(@mybin2 AS VARCHAR(5))  
```  
  
## <a name="result-types"></a>结果类型  
 返回优先级最高的参数的数据类型。 有关详细信息，请参阅[数据类型优先级 (Transact-SQL)](../../t-sql/data-types/data-type-precedence-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 +（字符串串联）运算符在用于长度为零的空字符串时的作用与用于 NULL 或未知值时不同。 长度为零的字符串可以指定为两个引号，引号内没有任何字符。 长度为零的二进制字符串可以指定为不带以十六进制常量指定的任何字节值的 0x。 串联长度为零的字符串始终要串联上述两个指定的字符串。 处理具有空值的字符串时，串联结果取决于会话设置。 与对空值执行的算术运算一样，当将空值添加到已知值时，结果通常是未知值，对空值执行的字符串串联运算也会产生空的结果。 但是，可以通过更改当前会话的 `CONCAT_NULL_YIELDS_NULL` 设置来更改此行为。 有关详细信息，请参阅 [SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md)。  
  
 如果字符串串联的结果超出 8,000 字节的限值，则结果将被截断。 但是，如果至少其中一个串联的字符串是大值类型，就不会进行截断。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-string-concatenation"></a>A. 使用字符串串联  
 以下示例在列标头 `Name` 下使用多个字符列创建一个列，人员的姓氏后跟随逗号、一个空格，然后是人员的名字。 结果集是按照姓氏，然后按照名字以字母顺序升序排列的。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ', ' + FirstName) AS Name  
FROM Person.Person  
ORDER BY LastName ASC, FirstName ASC;  
```  
  
### <a name="b-combining-numeric-and-date-data-types"></a>B. 组合数值和日期数据类型  
 以下示例使用 `CONVERT` 函数串联 numeric 和 date 数据类型。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT 'The order is due on ' + CONVERT(VARCHAR(12), DueDate, 101)  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderID = 50001;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 ------------------------------------------------  
 The order is due on 04/23/2007  
 (1 row(s) affected)
 ```  
  
### <a name="c-using-multiple-string-concatenation"></a>C. 使用多个字符串串联  
 以下示例串联多个字符串，形成一个长字符串，显示 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 的副总裁的姓氏和名字的首字母。 逗号加在姓氏后，句点加在名字首字母后。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ',' + SPACE(1) + SUBSTRING(FirstName, 1, 1) + '.') AS Name, e.JobTitle  
FROM Person.Person AS p  
    JOIN HumanResources.Employee AS e  
    ON p.BusinessEntityID = e.BusinessEntityID  
WHERE e.JobTitle LIKE 'Vice%'  
ORDER BY LastName ASC;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Name               Title  
 -------------      ---------------`  
 Duffy, T.          Vice President of Engineering  
 Hamilton, J.       Vice President of Production  
 Welcker, B.        Vice President of Sales  

 (3 row(s) affected)
 ```  
 
### <a name="d-using-large-strings-in-concatenation"></a>D. 在串联中使用大型字符串
以下示例将多个字符串串联成一个长字符串，然后尝试计算最终字符串的长度。 结果集的最后长度为 16000，因为表达式计算从左开始，即 @x + @z + @y => (@x + @z) + @y。 在本例中，(@x + @z) 的结果在 8000 字节处截断，然后将 @y 添加到结果集中，这使得最终的字符串长度为 16000。 由于 @y 为大型值类型字符串时，所以不会发生截断。

```sql
DECLARE @x VARCHAR(8000) = REPLICATE('x', 8000)
DECLARE @y VARCHAR(max) = REPLICATE('y', 8000)
DECLARE @z VARCHAR(8000) = REPLICATE('z',8000)
SET @y = @x + @z + @y
-- The result of following select is 16000
SELECT LEN(@y) AS y
GO
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 y        
 -------  
 16000  
  
 (1 row(s) affected)
 ```  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-multiple-string-concatenation"></a>E. 使用多个字符串串联  
 以下示例串联多个字符串，形成一个长字符串，显示一个示例数据库中的副总裁的姓氏和名字的首字母。 逗号加在姓氏后，句点加在名字首字母后。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT (LastName + ', ' + SUBSTRING(FirstName, 1, 1) + '.') AS Name, Title  
FROM DimEmployee  
WHERE Title LIKE '%Vice Pres%'  
ORDER BY LastName ASC;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Name               Title                                           
-------------      ---------------  
Duffy, T.          Vice President of Engineering  
Hamilton, J.       Vice President of Production  
Welcker, B.        Vice President of Sales  
```  
  
## <a name="see-also"></a>另请参阅  
 [+=（字符串串联赋值）(Transact-SQL)](../../t-sql/language-elements/string-concatenation-equal-transact-sql.md)   
 [ALTER DATABASE (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql.md)   
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)   
  
  



