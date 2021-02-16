---
title: DECLARE @local_variable (Transact-SQL) | Microsoft Docs
description: 有关使用 DECLARE 定义要在批处理或过程中使用的局部变量的 Transact-SQL 参考。
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DECLARE
- DECLARE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- table-valued parameters
- variables [SQL Server], declaring
- DECLARE statement
- declaring variables
ms.assetid: d1635ebb-f751-4de1-8bbc-cae161f90821
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 46551c65a37fb4e2c5b98806835f9b0b0fb3701d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347560"
---
# <a name="declare-local_variable-transact-sql"></a>DECLARE @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  变量是在批处理或过程的主体中用 DECLARE 语句声明的，并用 SET 或 SELECT 语句赋值。 游标变量可使用此语句声明，并可用于其他与游标相关的语句。 除非在声明中提供值，否则声明之后所有变量将初始化为 NULL。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for SQL Server and Azure SQL Database  
  
DECLARE   
{   
    { @local_variable [AS] data_type  [ = value ] }  
  | { @cursor_variable_name CURSOR }  
} [,...n]   
| { @table_variable_name [AS] <table_type_definition> }   
  
<table_type_definition> ::=   
     TABLE ( { <column_definition> | <table_constraint> } [ ,...n] )   
  
<column_definition> ::=   
     column_name { scalar_data_type | AS computed_column_expression }  
     [ COLLATE collation_name ]   
     [ [ DEFAULT constant_expression ] | IDENTITY [ (seed ,increment ) ] ]   
     [ ROWGUIDCOL ]   
     [ <column_constraint> ]   
  
<column_constraint> ::=   
     { [ NULL | NOT NULL ]   
     | [ PRIMARY KEY | UNIQUE ]   
     | CHECK ( logical_expression )   
     | WITH ( <index_option > )  
     }   
  
<table_constraint> ::=   
     { { PRIMARY KEY | UNIQUE } ( column_name [ ,...n] )   
     | CHECK ( search_condition )   
     }   
  
<index_option> ::=  
See CREATE TABLE for index option syntax.  
  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
DECLARE   
{{ @local_variable [AS] data_type } [ =value [ COLLATE <collation_name> ] ] } [,...n]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
@local_variable  
 变量的名称。 变量名必须以 at 符 (@) 开头。 局部变量名称必须符合[标识符](../../relational-databases/databases/database-identifiers.md)规则。  
  
data_type  
 任何系统提供的公共语言运行时 (CLR) 用户定义表类型或别名数据类型。 变量的数据类型不能为 text、ntext 或 image  。  
  
 有关系统数据类型的详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。 有关 CLR 用户定义类型或别名数据类型的详细信息，请参阅 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)。  
  
 =value  
 以内联方式为变量赋值。 值可以是常量或表达式，但它必须与变量声明类型匹配，或者可隐式转换为该类型。 有关详细信息，请参阅[表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
@cursor_variable_name  
 cursor 变量的名称。 游标变量名称必须以 at 符 (@) 开头，并符合有关标识符的规则。  
  
CURSOR  
 指定变量是局部游标变量。  
  
@table_variable_name  
 表类型的变量的名称。 变量名称必须以 at 符 (@) 开头，并符合有关标识符的规则。  
  
<table_type_definition>  
定义表数据类型。 表声明包括列定义、名称、数据类型和约束。 允许的约束类型只包括 PRIMARY KEY、UNIQUE、NULL 和 CHECK。 如果类型绑定了规则或默认定义，则不能将别名数据类型用作列标量数据类型。
  
\<table_type_definiton> 是在 CREATE TABLE 中用于定义表的信息子集。 其中包含了元素和主要定义。 有关详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)。  
  
 *n*  
 指示可以指定多个变量并对变量赋值的占位符。 声明表变量时，表变量必须是 DECLARE 语句中声明的唯一变量 。  
  
 column_name  
 表中的列的名称。  
  
 scalar_data_type  
 指定列是标量数据类型。  
  
 computed_column_expression  
 定义计算列值的表达式。 计算列由同一表中的其他列通过表达式计算而得。 例如，计算列可以定义为 cost、AS、price \* qty 。表达式可以是非计算列名称、常量、内置函数、变量，也可以是用一个或多个运算符连接的上述元素的任意组合。 表达式不能为子查询或用户定义函数。 表达式不能引用 CLR 用户定义类型。  
  
 [ COLLATE collation_name]  
 指定列的排序规则。 collation_name 可以是 Windows 排序规则名称或 SQL 排序规则名称。只适用于 char、varchar、text、nchar、nvarchar 和 ntext 等数据类型列     。 如果未指定，则该列的排序规则是用户定义数据类型的排序规则（如果列为用户定义数据类型）或当前数据库的排序规则。  
  
 有关 Windows 和 SQL 排序规则名称的详细信息，请参阅 [COLLATE (Transact-SQL)](~/t-sql/statements/collations.md)。  
  
 DEFAULT  
 如果在插入过程中未显式提供值，则指定为列提供的值。 DEFAULT 定义可适用于除定义为 timestamp 或带 IDENTITY 属性的列以外的任何列。 删除表时，将删除 DEFAULT 定义。 只有常量值（如字符串）、系统函数（如 SYSTEM_USER()）或 NULL 可用作默认参数。 为了与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本兼容，可以为 DEFAULT 分配约束名称。  
  
 constant_expression  
 用作列的默认值的常量、NULL 或系统函数。  
  
 IDENTITY  
 指示新列是标识列。 在表中添加新行时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将为列提供一个唯一的增量值。 标识列通常与 PRIMARY KEY 约束一起使用，作为表的唯一行标识符。 可以将 IDENTITY 属性分配到 tinyint、smallint、int、decimal(p,0) 或 numeric(p,0) 列    。 每个表只能创建一个标识列。 不能对标识列使用绑定默认值和 DEFAULT 约束。 必须同时指定种子和增量，或者都不指定。 如果二者都未指定，则取默认值 (1,1)。  
  
 seed  
 是装入表的第一行所使用的值。  
  
 increment  
 添加到以前装载的列标识值的增量值。  
  
 ROWGUIDCOL  
 指示新列是行的全局唯一标识符列。 对于每个表，只能将其中的一个 uniqueidentifier 列指定为 ROWGUIDCOL 列。 ROWGUIDCOL 属性只能分配给 uniqueidentifier 列。  
  
 NULL | NOT NULL  
 指示变量中是否允许使用 Null。 默认值为 NULL。  
  
 PRIMARY KEY  
 通过唯一索引对给定的一列或多列强制实现实体完整性的约束。 每个表只能创建一个 PRIMARY KEY 约束。  
  
 UNIQUE  
 通过唯一索引为给定的一列或多列提供实体完整性的约束。 一个表可以有多个 UNIQUE 约束。  
  
 CHECK  
 一个约束，该约束通过限制可输入一列或多列中的可能值来强制实现域完整性。  
  
 logical_expression  
 返回 TRUE 或 FALSE 的逻辑表达式。  
  
## <a name="remarks"></a>备注  
 变量常用在批处理或过程中，作为 WHILE、LOOP 或 IF...ELSE 块的计数器。  
  
 变量只能用在表达式中，不能代替对象名或关键字。 若要构造动态 SQL 语句，请使用 EXECUTE。  
  
 局部变量的作用域是其被声明时所在批处理。  
 
 表变量不一定是内存驻留。 在内存压力下，可以将属于表变量的页推送到 tempdb。
  
 当前分配有游标的游标变量可在下列语句中作为源引用：  
  
-   CLOSE 语句。  
  
-   DEALLOCATE 语句。  
  
-   FETCH 语句。  
  
-   OPEN 语句。  
  
-   定位的 DELETE 或 UPDATE 语句。  
  
-   SET CURSOR 变量语句（在右侧）。  
  
 在所有上述语句中，如果存在被引用的游标变量，但是不具有当前分配给它的游标，那么 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将引发错误。 如果不存在被引用的游标变量，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将引发与其他类型的未声明变量引发的错误相同的错误。  
  
 游标变量：  
  
-   可以是游标类型或其他游标变量的目标。 有关详细信息，请参阅 [SET @local_variable (Transact-SQL)](../../t-sql/language-elements/set-local-variable-transact-sql.md)。  
  
-   如果当前没有给游标变量分配游标，则可在 EXECUTE 语句中作为输出游标参数的目标引用。  
  
-   应被看作是指向游标的指针。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-declare"></a>A. 使用 DECLARE  
 下例将使用名为 `@find` 的局部变量检索所有姓氏以 `Man` 开头的联系人信息。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @find VARCHAR(30);   
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';   
*/  
SET @find = 'Man%';   
SELECT p.LastName, p.FirstName, ph.PhoneNumber  
FROM Person.Person AS p   
JOIN Person.PersonPhone AS ph ON p.BusinessEntityID = ph.BusinessEntityID  
WHERE LastName LIKE @find;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName            FirstName               Phone
------------------- ----------------------- -------------------------
Manchepalli         Ajay                    1 (11) 500 555-0174
Manek               Parul                   1 (11) 500 555-0146
Manzanares          Tomas                   1 (11) 500 555-0178
  
(3 row(s) affected)
```  
  
### <a name="b-using-declare-with-two-variables"></a>B. 在 DECLARE 中使用两个变量  
 下例将检索北美销售区中年销售额至少为 $2,000,000 的 [!INCLUDE[ssSampleDBCoFull](../../includes/sssampledbcofull-md.md)] 销售代表的名字。  
  
```sql  
USE AdventureWorks2012;  
GO  
SET NOCOUNT ON;  
GO  
DECLARE @Group nvarchar(50), @Sales MONEY;  
SET @Group = N'North America';  
SET @Sales = 2000000;  
SET NOCOUNT OFF;  
SELECT FirstName, LastName, SalesYTD  
FROM Sales.vSalesPerson  
WHERE TerritoryGroup = @Group and SalesYTD >= @Sales;  
```  
  
### <a name="c-declaring-a-variable-of-type-table"></a>C. 声明一个表类型的变量  
 下例将创建一个 `table` 变量，用于储存 UPDATE 语句的 OUTPUT 子句中指定的值。 在它后面的两个 `SELECT` 语句返回 `@MyTableVar` 中的值以及 `Employee` 表中更新操作的结果。 请注意，`INSERTED.ModifiedDate` 列中的结果与 `Employee` 表的 `ModifiedDate` 列中的值不同。 这是因为对 `AFTER UPDATE` 表定义了 `ModifiedDate` 触发器，该触发器可以将 `Employee` 的值更新为当前日期。 不过，从 `OUTPUT` 返回的列可反映触发器激发之前的数据。 有关详细信息，请参阅 [OUTPUT 子句 (Transact-SQL)](../../t-sql/queries/output-clause-transact-sql.md)。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @MyTableVar TABLE(  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25   
OUTPUT INSERTED.BusinessEntityID,  
       DELETED.VacationHours,  
       INSERTED.VacationHours,  
       INSERTED.ModifiedDate  
INTO @MyTableVar;  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
--Note that ModifiedDate reflects the value generated by an  
--AFTER UPDATE trigger.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```  
  
### <a name="d-declaring-a-variable-of-user-defined-table-type"></a>D. 声明一个用户定义表类型的变量  
 下面的示例将创建一个名为 `@LocationTVP` 的表值参数或表变量。 这需要使用一个相应的名为 `LocationTableType` 的用户定义表类型。 有关如何创建用户定义表类型的详细信息，请参阅 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)。 有关表值参数的详细信息，请参阅[使用表值参数（数据引擎）](../../relational-databases/tables/use-table-valued-parameters-database-engine.md)。  
  
```sql  
DECLARE @LocationTVP   
AS LocationTableType;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-declare"></a>E. 使用 DECLARE  
 下例将使用名为 `@find` 的局部变量检索所有姓氏以 `Walt` 开头的联系人信息。  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @find VARCHAR(30);  
/* Also allowed:   
DECLARE @find VARCHAR(30) = 'Man%';  
*/  
SET @find = 'Walt%';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @find;  
```  
  
### <a name="f-using-declare-with-two-variables"></a>F. 在 DECLARE 中使用两个变量  
 以下示例检索使用变量来指定 `DimEmployee` 表中的第一个和最后一个雇员名称。  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @lastName VARCHAR(30), @firstName VARCHAR(30);  
  
SET @lastName = 'Walt%';  
SET @firstName = 'Bryan';  
  
SELECT LastName, FirstName, Phone  
FROM DimEmployee   
WHERE LastName LIKE @lastName AND FirstName LIKE @firstName;  
```  
  
## <a name="see-also"></a>另请参阅  
 [EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
 [内置函数 (Transact-SQL)](~/t-sql/functions/functions.md)   
 [SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
 [表 (Transact-SQL)](../../t-sql/data-types/table-transact-sql.md)   
 [类型化的 XML 与非类型化的 XML 的比较](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)  
  
  




