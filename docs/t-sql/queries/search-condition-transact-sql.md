---
description: 搜索条件 (Transact-SQL)
title: 搜索条件 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- search
- Search Condition
- condition
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator [Transact-SQL]
- CONTAINS predicate (Transact-SQL)
- ESCAPE keyword
- operators [Transact-SQL], search condition
- search conditions [SQL Server]
- WHERE clause, search conditions
- ALL keyword
- FREETEXT predicate (Transact-SQL)
- EXISTS keyword
- search conditions [SQL Server], about search conditions
- NOT operator [Transact-SQL]
- BETWEEN operator
- SOME | ANY keyword
- predicates [full-text search]
- AND, about search conditions
- logical operators [SQL Server], about search conditions
- precedence [SQL Server], logical operators
- logical operators [SQL Server], precedence
- LIKE comparisons
ms.assetid: 09974469-c5d2-4be8-bc5a-78e404660b2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f7878341361f74c9e8e4aa65b6619aebde9780a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460890"
---
# <a name="search-condition-transact-sql"></a>搜索条件 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  使用逻辑运算符 AND、OR 和 NOT 的一个或多个谓词的组合。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<search_condition> ::=  
    MATCH(<graph_search_pattern>) | <search_condition_without_match> | <search_condition> AND <search_condition>

<search_condition_without_match> ::= 
    { [ NOT ] <predicate> | ( <search_condition_without_match> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition_without_match> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | ! > | < | < = | ! < } expression   
    | string_expression [ NOT ] LIKE string_expression   
  [ ESCAPE 'escape_character' ]   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | CONTAINS   
  ( { column | * } , '<contains_search_condition>' )   
    | FREETEXT ( { column | * } , 'freetext_string' )   
    | expression [ NOT ] IN ( subquery | expression [ ,...n ] )   
    | expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
  { ALL | SOME | ANY} ( subquery )   
    | EXISTS ( subquery )     }   
    
<graph_search_pattern> ::=
    { <node_alias> { 
                      { <-( <edge_alias> )- } 
                    | { -( <edge_alias> )-> }
                    <node_alias> 
                   } 
    }
  
<node_alias> ::=
    node_table_name | node_table_alias 

<edge_alias> ::=
    edge_table_name | edge_table_alias
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
< search_condition > ::=   
    { [ NOT ] <predicate> | ( <search_condition> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | < | < = } expression   
    | string_expression [ NOT ] LIKE string_expression   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | expression [ NOT ] IN (subquery | expression [ ,...n ] )   
    | expression [ NOT ] EXISTS (subquery)     }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 \<search_condition>  
 指定要在 SELECT 语句、查询表达式或子查询的结果集中返回的行的条件。 对于 UPDATE 语句，指定要更新的行。 对于 DELETE 语句，指定要删除的行。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句搜索条件中可以包含任意多个谓词。  
  
 \<graph_search_pattern>  
 指定图匹配模式。 有关此子句的参数的详细信息，请参阅 [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md)
 
 NOT  
 对谓词指定的布尔表达式求反。 有关详细信息，请参阅 [NOT (Transact-SQL)](../../t-sql/language-elements/not-transact-sql.md)。  
  
 AND  
 组合两个条件，并在两个条件都为 TRUE 时取值为 TRUE。 有关详细信息，请参阅 [AND (Transact-SQL)](../../t-sql/language-elements/and-transact-sql.md)。  
  
 OR  
 组合两个条件，并在任何一个条件为 TRUE 时取值为 TRUE。 有关详细信息，请参阅 [OR (Transact-SQL)](../../t-sql/language-elements/or-transact-sql.md)。  
  
 \< predicate >  
 返回 TRUE、FALSE 或 UNKNOWN 的表达式。  
  
 *expression*  
 列名、常量、函数、变量、标量子查询，或者是通过运算符或子查询连接的列名、常量和函数的任意组合。 表达式还可以包含 CASE 表达式。  
  
> [!NOTE]  
>  非 Unicode 字符串常量和变量使用与数据库的默认排序规则相对应的代码页。 仅使用非 Unicode 字符数据并引用 char、varchar 和 text 类型的非 Unicode 字符数据时，可能发生代码页转换  。 如果该代码页与数据库的默认排序规则对应的代码页不同，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将把非 Unicode 字符串常量和变量转换为与引用列的排序规则或使用 COLLATE 指定的排序规则相对应的代码页。 如果可以找到[最佳匹配映射](https://www.unicode.org/Public/MAPPINGS/VENDORS/MICSFT/WindowsBestFit/)，则无法在新代码页中找到的任何字符都将转换为相似字符，否则将转换为默认替换字符“?”。  
>  
> 使用多个代码页时，字符常量可能以大写字母“N”为前缀；为避免发生代码页转换，还可能使用 Unicode 变量。  
  
 =  
 用于测试两个表达式是否相等的运算符。  
  
 <>  
 用于测试两个表达式是否彼此不相等的运算符。  
  
 !=  
 用于测试两个表达式是否彼此不相等的运算符。  
  
 \>  
 用于测试一个表达式是否大于另一个表达式的运算符。  
  
 \>=  
 用于测试一个表达式是否大于或等于另一个表达式的运算符。  
  
 !>  
 用于测试一个表达式是否不大于另一个表达式的运算符。  
  
 <  
 用于测试一个表达式是否小于另一个表达式的运算符。  
  
 <=  
 用于测试一个表达式是否小于或等于另一个表达式的运算符。  
  
 !<  
 用于测试一个表达式是否不小于另一个表达式的运算符。  
  
 string_expression  
 字符串和通配符。  
  
 [ NOT ] LIKE  
 指示后续字符串使用时要进行模式匹配。 有关详细信息，请参阅 [LIKE (Transact-SQL)](../../t-sql/language-elements/like-transact-sql.md)。  
  
 ESCAPE **'** _escape_ character_ *_'_*  
 允许在字符串中搜索通配符，而不是将其作为通配符使用。 escape_character 是放在通配符前表示此特殊用法的字符。  
  
 [ NOT ] BETWEEN  
 指定值的包含范围。 使用 AND 分隔开始值和结束值。 有关详细信息，请参阅 [BETWEEN (Transact-SQL)](../../t-sql/language-elements/between-transact-sql.md)。  
  
 IS [ NOT ] NULL  
 根据使用的关键字，指定是否搜索空值或非空值。 如果有任何一个操作数为 NULL，则包含位运算符或算术运算符的表达式的计算结果为 NULL。  
  
 CONTAINS  
 在包含基于字符的数据的列中，搜索单个词和短语的精确或不精确（“模糊”）匹配项、在一定范围内相同的近似词以及加权匹配项。 此选项只能与 SELECT 语句一起使用。 有关详细信息，请参阅 [CONTAINS (Transact-SQL)](../../t-sql/queries/contains-transact-sql.md)。  
  
 FREETEXT  
 在包含基于字符的数据的列中，搜索与谓词中的词的含义相符而非精确匹配的值，从而提供一种形式简单的自然语言查询。 此选项只能与 SELECT 语句一起使用。 有关详细信息，请参阅 [FREETEXT (Transact-SQL)](../../t-sql/queries/freetext-transact-sql.md)。  
  
 [ NOT ] IN  
 根据是在列表中包含还是排除某表达式，指定对该表达式的搜索。 搜索表达式可以是常量或列名，而列表可以是一组常量，更常用的是子查询。 将一组值用圆括号括起来。 有关详细信息，请参阅 [IN (Transact-SQL)](../../t-sql/language-elements/in-transact-sql.md)。  
  
 subquery  
 可以视为受限的 SELECT 语句，与 SELECT 语句中的 \<query_expression> 相似。 不允许使用 ORDER BY 子句和 INTO 关键字。 有关详细信息，请参阅 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)。  
  
 ALL  
 与比较运算符和子查询一起使用。 如果为子查询检索的所有值都满足比较运算，则为 \<predicate> 返回 TRUE；如果并非所有值都满足比较运算或子查询未向外部语句返回行，则返回 FALSE。 有关详细信息，请参阅 [ALL (Transact SQL)](../../t-sql/language-elements/all-transact-sql.md)。  
  
 { SOME | ANY }  
 与比较运算符和子查询一起使用。 如果为子查询检索的任何值都满足比较运算，则为 \<predicate> 返回 TRUE；如果子查询内没有值满足比较运算或子查询未向外部语句返回行，则返回 FALSE。 其他情况下，表达式为 UNKNOWN。 有关详细信息，请参阅 [SOME | ANY (Transact-SQL)](../../t-sql/language-elements/some-any-transact-sql.md)。  
  
 EXISTS  
 与子查询一起使用，用于测试是否存在子查询返回的行。 有关详细信息，请参阅 [EXISTS (Transact-SQL)](../../t-sql/language-elements/exists-transact-sql.md)。  
  
## <a name="remarks"></a>备注  
 逻辑运算符的优先顺序是 NOT（最高），然后是 AND，最后是 OR。 不过，可以在搜索条件内使用括号来表示优于此优先顺序的运算符。 根据查询优化器所做的选择，逻辑运算符的求值顺序可能有所不同。 有关逻辑运算符如何对逻辑值进行运算的详细信息，请参阅 [AND (Transact-SQL)](../../t-sql/language-elements/and-transact-sql.md)、[OR (Transact-SQL)](../../t-sql/language-elements/or-transact-sql.md) 和 [NOT (Transact-SQL)](../../t-sql/language-elements/not-transact-sql.md)。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-where-with-like-and-escape-syntax"></a>A. 在 WHERE 中使用 LIKE 和 ESCAPE 语法  
 下面的示例将搜索以下行，其中的 `LargePhotoFileName` 列包含字符 `green_`；由于 _ 是通配符，因此使用了 `ESCAPE` 选项。 如果不指定 `ESCAPE` 选项，则查询搜索到的任何说明值中将包含后跟一个非 _ 字符的 `green` 一词。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT *   
FROM Production.ProductPhoto  
WHERE LargePhotoFileName LIKE '%greena_%' ESCAPE 'a' ;  
```  
  
### <a name="b-using-where-and-like-syntax-with-unicode-data"></a>B. 对 Unicode 数据使用 WHERE 和 LIKE 语法  
 下面的示例将使用 `WHERE` 子句检索美国 (`US`) 以外且所在城市的名称以 `Pa` 开头的所有公司的邮件地址。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT AddressLine1, AddressLine2, City, PostalCode, CountryRegionCode    
FROM Person.Address AS a  
JOIN Person.StateProvince AS s ON a.StateProvinceID = s.StateProvinceID  
WHERE CountryRegionCode NOT IN ('US')  
AND City LIKE N'Pa%' ;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-where-with-like"></a>C. 将 WHERE 与 LIKE 一起使用  
 下面的示例将搜索以下行，其中的 `LastName` 列包含字符 `and`。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE '%and%';  
```  
  
### <a name="d-using-where-and-like-syntax-with-unicode-data"></a>D. 对 Unicode 数据使用 WHERE 和 LIKE 语法  
 下面的示例使用 `WHERE` 子句对 `LastName` 列执行 Unicode 搜索。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE N'%and%';  
```  
  
## <a name="see-also"></a>另请参阅  
 [聚合函数 (Transact-SQL)](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [CASE (Transact-SQL)](../../t-sql/language-elements/case-transact-sql.md)   
 [CONTAINSTABLE (Transact-SQL)](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [游标 (Transact-SQL)](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FREETEXTTABLE (Transact-SQL)](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)   
 [运算符 (Transact-SQL)](../../t-sql/language-elements/operators-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)  
  
  

