---
description: 排序规则优先级
title: 排序规则优先级 | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- coercible-default collation label
- precedence [SQL Server], collations
- collation sensitive
- collations [SQL Server], precedence
- explicit collation label [SQL Server]
- implicit collation label
- no-collation label
- collation insensitive
- operators [Transact-SQL], collations
- collation labels
- collation coercion rules
- rules [SQL Server], collations
ms.assetid: 58c4e64b-5634-4c29-aa22-33193282dd27
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 13a960ed74fe9947d82e6dee3701ce1ba6af7648
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124154"
---
# <a name="collation-precedence"></a>排序规则优先级
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  排序规则优先顺序也称为排序规则强制规则，用于确定：  
  
-   计算结果为字符串的表达式的最终结果排序规则。  
  
-   区分排序规则的运算符所使用的排序规则，这些运算符使用字符串输入但不返回字符串，如 LIKE 和 IN。  
  
排序规则的优先顺序规则只应用于下列字符串数据类型：char、varchar、text、nchar、nvarchar 和 ntext。 具有其他数据类型的对象不参与排序规则计算。  
  
## <a name="collation-labels"></a>排序规则标签  
下表列出并说明了四个用于标识所有对象的排序规则的类别。 每个类别的名称叫做排序规则标签。  
  
|排序规则标签|对象类型|  
|---------------------|----------------------|  
|强制默认|任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 字符串变量、参数、文字、目录内置函数的输出或不使用字符串输入但生成字符串输出的内置函数。<br /><br /> 如果在用户定义函数、存储过程或触发器中声明对象，则为该对象分配创建函数、存储过程或触发器所采用的数据库默认排序规则。 如果在批处理中声明对象，则为该对象分配用于连接的当前数据库的默认排序规则。|  
|隐式 X|列引用。 从为表或视图中的列定义的排序规则得到表达式 (X) 的排序规则。<br /><br /> 即使使用 CREATE TABLE 或 CREATE VIEW 语句中的 COLLATE 子句为列显式分配了排序规则，该列引用仍归为隐式。|  
|显式 X|使用表达式中的 COLLATE 子句显式转换为特定排序规则 (X) 的表达式。|  
|无排序规则|指示表达式的值是两个字符串之间的运算结果，而这两个字符串具有隐式排序规则标签的冲突排序规则。 表达式的结果被定义为不具有排序规则。|  
  
## <a name="collation-rules"></a>排序规则  
只引用一个字符串对象的简单表达式的排序规则标签是被引用对象的排序规则标签。  
  
如果复杂表达式被引用两个操作数表达式的排序规则标签相同，则该复杂表达式的排序规则标签为操作数表达式的排序规则标签。  
  
如果复杂表达式被引用两个操作数表达式的排序规则不同，则该复杂表达式最终结果的排序规则标签基于下列规则：  
  
-   显式优先于隐式。 隐式优先于强制默认：  
  
     显式 > 隐式 > 强制默认  
  
-   组合两个已被分配有不同排序规则的显式表达式将生成错误：  
  
     显式 X + 显式 Y = 错误  
  
-   组合两个具有不同排序规则的隐式表达式将生成无排序规则的结果：  
  
     隐式 X + 隐式 Y = 无排序规则  
  
-   将无排序规则的表达式与除显式排序规则（参阅下一个规则）之外任何标签表达式组合都将生成无排序规则标签的结果：  
  
     无排序规则 + 任何内容 = 无排序规则  
  
-   将无排序规则的表达式与具有显式排序规则的表达式组合将生成具有显式标签的表达式：  
  
     无排序规则 + 显式 X = 显式  
  
下表概述了这些规则。  
  
|操作数强制标签|显式 X|隐式 X|强制默认|无排序规则|  
|----------------------------|----------------|----------------|------------------------|-------------------|  
|**显式 Y**|生成错误|结果为显式 Y|结果为显式 Y|结果为显式 Y|  
|**隐式 Y**|结果为显式 X|结果为无排序规则|结果为隐式 Y|结果为无排序规则|  
|**强制默认**|结果为显式 X|结果为隐式 X|结果为强制默认|结果为无排序规则|  
|**无排序规则**|结果为显式 X|结果为无排序规则|结果为无排序规则|结果为无排序规则|  
  
下列附加规则也适用于排序规则优先顺序：  
  
-   在已经是显式表达式的表达式上不能有多个 COLLATE 子句。 例如，下面的 `WHERE` 子句无效，因为已经为显式表达式指定了 `COLLATE` 子句：  
  
     `WHERE ColumnA = ( 'abc' COLLATE French_CI_AS) COLLATE French_CS_AS`  
  
-   不允许进行 text 数据类型的代码页转换。 如果排序规则的代码页不同，则不能将 text 表达式从一种排序规则转换为另一种排序规则。 如果右边文本操作数的排序规则代码页与左边文本操作数的排序规则代码页不同，则不能为赋值运算符赋值。  
  
在数据类型转换之后确定排序规则优先顺序。 生成结果排序规则的操作数可以与提供最终结果数据类型的操作数不同。 例如，请看下面的批处理：  
  
```sql  
CREATE TABLE TestTab  
   (PrimaryKey int PRIMARY KEY,  
    CharCol char(10) COLLATE French_CI_AS  
   )  
  
SELECT *  
FROM TestTab  
WHERE CharCol LIKE N'abc'  
```  
  
简单表达式 `N'abc'` 的 Unicode 数据类型有更高的数据类型优先级。 因此，所生成的表达式将 Unicode 数据类型分配给 `N'abc'`。 但是，表达式 `CharCol` 具有隐式排序规则标签，而 `N'abc'` 具有级别更低的强制标签，即强制默认。 因此，所使用的排序规则是 `French_CI_AS` 的 `CharCol` 排序规则。  
  
### <a name="examples-of-collation-rules"></a>排序规则示例  
 以下示例显示排序规则如何工作。 若要运行该示例，请创建以下测试表。  
  
```sql  
USE tempdb;  
GO  
  
CREATE TABLE TestTab (  
   id int,   
   GreekCol nvarchar(10) collate greek_ci_as,   
   LatinCol nvarchar(10) collate latin1_general_cs_as  
   )  
INSERT TestTab VALUES (1, N'A', N'a');  
GO  
```  
  
#### <a name="collation-conflict-and-error"></a>排序规则冲突和错误  
 下面查询中的谓词具有排序规则冲突，因此会产生错误。  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 448, Level 16, State 9, Line 2  
Cannot resolve collation conflict between 'Latin1_General_CS_AS' and 'Greek_CI_AS' in equal to operation.  
```  
  
#### <a name="explicit-label-vs-implicit-label"></a>显式标签与隐式标签  
 由于右表达式有显式标签，因此采用排序规则 `greek_ci_as` 计算以下查询中的谓词。 它的优先级高于左表达式的隐式标签。  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol COLLATE greek_ci_as;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
id          GreekCol             LatinCol  
----------- -------------------- --------------------  
          1 A                    a  
  
(1 row affected)  
```  
  
#### <a name="no-collation-labels"></a>无排序规则标签  
下列查询中的 `CASE` 表达式具有无排序规则标签，所以它们不能出现在选择列表中，也不能由区分排序规则的运算符进行运算。 不过，这些表达式可由不区分排序规则的运算符进行运算。  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END)   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 451, Level 16, State 1, Line 1  
Cannot resolve collation conflict for column 1 in SELECT statement.  
```  
  
```sql  
SELECT PATINDEX((CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END), 'a')  
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 446, Level 16, State 9, Server LEIH2, Line 1  
Cannot resolve collation conflict for patindex operation.  
```  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END) COLLATE Latin1_General_CI_AS   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------  
a  
  
(1 row affected)  
```  
  
## <a name="collation-sensitive-and-collation-insensitive"></a>区分排序规则与不区分排序规则  
运算符和函数可以区分排序规则，也可以不区分排序规则。  
  
区分排序规则  
这表示指定无排序规则操作数是一个编译时错误。 表达式结果不能无排序规则。  
  
不区分排序规则  
这表示操作数和结果可以无排序规则。  
  
### <a name="operators-and-collation"></a>运算符和排序规则  
比较运算符以及 MAX、MIN、BETWEEN、LIKE 和 IN 运算符都区分排序规则。 运算符所使用的字符串被赋以具有较高优先顺序的操作数的排序规则标签。 UNION 语句也区分排序规则，且所有字符串操作数和最终结果被赋以具有最高优先顺序的操作数的排序规则。 按列评估 UNION 操作数和结果的排序规则优先顺序。  
  
赋值运算符不区分排序规则，右边的表达式转换到左边的排序规则上。  
  
字符串串联运算符区分排序规则，两个字符串操作数和结果被赋以排序规则优先级最高的操作数的排序规则标签。 UNION ALL 和 CASE 语句不区分排序规则，所有的字符串操作数和最终结果都被赋以具有最高优先顺序的操作数的排序规则标签。 按列评估 UNION ALL 操作数和结果的排序规则优先顺序。  
  
### <a name="functions-and-collation"></a>函数和排序规则  
对于 char、varchar 和 text 数据类型，CAST、CONVERT 和 COLLATE 函数区分排序规则。 如果 CAST 和 CONVERT 函数的输入和输出是字符串，则输出字符串具有输入字符串的排序规则标签。 如果输入不是字符串，则输出字符串为强制默认并被赋以连接所使用的当前数据库的排序规则，或是包含引用 CAST 或 CONVERT 的用户定义函数、存储过程或触发器的数据库的排序规则。  
  
 对于返回字符串但不使用字符串输入的内置函数，结果字符串为强制默认并被赋以当前数据库的排序规则，或是包含引用该函数的用户定义函数、存储过程或触发器的数据库的排序规则。  
  
 下列函数区分排序规则，并且它们的输出字符串具有输入字符串的排序规则标签：  

:::row:::
    :::column:::
        CHARINDEX
    :::column-end:::
    :::column:::
        REPLACE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DIFFERENCE
    :::column-end:::
    :::column:::
        REVERSE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ISNUMERIC
    :::column-end:::
    :::column:::
        RIGHT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEFT
    :::column-end:::
    :::column:::
        SOUNDEX
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEN
    :::column-end:::
    :::column:::
        STUFF
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LOWER
    :::column-end:::
    :::column:::
        SUBSTRING
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        PATINDEX
    :::column-end:::
    :::column:::
        UPPER
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>另请参阅  
 [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md)   
 [数据类型转换（数据库引擎）](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
