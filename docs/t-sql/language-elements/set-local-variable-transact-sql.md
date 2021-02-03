---
description: SET @local_variable (Transact-SQL)
title: SET @local_variable (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- SET @local_variable
- variables [SQL Server], assigning
- SET statement, @local_variable
- local variables [SQL Server]
ms.assetid: d410e06e-061b-4c25-9973-b2dc9b60bd85
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0cf40a6b16c1a2750180a5646e17ad317fe3c1bf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194285"
---
# <a name="set-local_variable-transact-sql"></a>SET @local_variable (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

将先前使用 DECLARE @local_variable 语句创建的指定局部变量设置为指定值。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
SQL Server 和 Azure SQL 数据库的语法：

```syntaxsql
SET   
{ @local_variable  
    [ . { property_name | field_name } ] = { expression | udt_name { . | :: } method_name }  
}  
|  
{ @SQLCLR_local_variable.mutator_method  
}  
|  
{ @local_variable  
    {+= | -= | *= | /= | %= | &= | ^= | |= } expression  
}  
|   
  { @cursor_variable =   
    { @cursor_variable | cursor_name   
    | { CURSOR [ FORWARD_ONLY | SCROLL ]   
        [ STATIC | KEYSET | DYNAMIC | FAST_FORWARD ]   
        [ READ_ONLY | SCROLL_LOCKS | OPTIMISTIC ]   
        [ TYPE_WARNING ]   
    FOR select_statement   
        [ FOR { READ ONLY | UPDATE [ OF column_name [ ,...n ] ] } ]   
      }   
    }  
}   
```  
Azure Synapse Analytics 和并行数据仓库的语法：  
```syntaxsql
SET @local_variable { = | += | -= | *= | /= | %= | &= | ^= | |= } expression  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
**@** _local_variable_  
除 **cursor**、**text**、**ntext**、**image** 或 **table** 之外的任何类型的变量的名称。 变量名称必须以 at 符号 (@) 开头。 变量名称必须遵循有关[标识符](../../relational-databases/databases/database-identifiers.md)的规则。  
  
property_name  
用户定义类型的属性。  
  
field_name  
用户定义类型的公共字段。  
  
udt_name  
公共语言运行时 (CLR) 用户定义类型的名称。  
  
`{ . | :: }`  
指定 CLR 用户定义类型的方法。 对于实例（非静态）方法，请使用句点 (**.**)。 对于静态方法，请使用两个冒号 (::)。 若要调用 CLR 用户定义类型的方法、属性或字段，必须对类型具有 EXECUTE 权限。  
  
_method_name_ **(** _argument_ [ **,** ... *n* ] **)**  
用户定义类型的方法，它使用一个或多个参数来修改类型实例的状态。 静态方法必须是公共的。  
  
**@** _SQLCLR_local_variable_  
其类型位于程序集内的变量。 有关详细信息，请参阅[公共语言运行时 (CLR) 集成编程概念](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md)。  
  
mutator_method  
程序集中可更改对象状态的方法。 SQLMethodAttribute.IsMutator 会应用于此方法。  
  
`{ += | -= | *= | /= | %= | &= | ^= | |= }`  
复合赋值运算符：  
  
 += 相加并赋值  
  
 -=              相减并赋值  
  
 *= 相乘并赋值  
  
 /=              相除并赋值  
  
 %=              取模并赋值  
  
 &=“位与”并赋值  
  
 ^=“位异或”并赋值  
  
 |=              “位或”并赋值  
  
*expression*  
任何有效的[表达式](../../t-sql/language-elements/expressions-transact-sql.md)。  
  
cursor_variable  
游标变量的名称。 如果目标游标变量先前引用了不同游标，则删除先前的引用。  
  
cursor_name  
使用 DECLARE CURSOR 语句声明的游标名称。  
  
CURSOR  
指定 SET 语句包含游标的声明。  
  
SCROLL  
指定游标支持所有提取选项：FIRST、LAST、NEXT、PRIOR、RELATIVE 和 ABSOLUTE。 如果还指定了 FAST_FORWARD，则不能指定 SCROLL。  
  
FORWARD_ONLY  
指定游标仅支持 FETCH NEXT 选项。 仅以一个方向、从第一行到最后一行检索游标。 如果没有使用 STATIC、KEYSET 或 DYNAMIC 关键字指定 FORWARD_ONLY，游标将作为 DYNAMIC 实现。 如果 FORWARD_ONLY 和 SCROLL 均未指定，那么除非指定了 STATIC、KEYSET 或 DYNAMIC 关键字，否则默认值为 FORWARD_ONLY。 对于 STATIC、KEYSET 和 DYNAMIC 游标，SCROLL 为默认值。  
  
STATIC  
定义一个游标，以创建将由该游标使用的数据的临时副本。 对游标的所有请求都通过 tempdb 中的这个临时表进行答复。 因此，打开游标后对基表所做的修改不会在对游标进行提取操作返回的数据中反映。 而且，此游标不支持修改。  
  
KEYSET  
指定当游标打开时，游标中行的成员身份和顺序已经固定。 对行进行唯一标识的键集内置在 tempdb 内的 keysettable 中。 对基表中的非键值所做的更改（由游标所有者更改或其他用户提交）在游标所有者滚动游标时可见。 其他用户进行的插入不可见，且不能通过 [!INCLUDE[tsql](../../includes/tsql-md.md)] 服务器游标进行插入。  
  
如果删除某一行，则在尝试提取该行时返回的 @@FETCH_STATUS 为 -2。 从游标外部更新键值类似于删除旧行后再插入新行。 具有新值的行不可见，且尝试提取具有旧值的行时返回的 @@FETCH_STATUS 为 -2。 如果通过指定 WHERE CURRENT OF 子句来通过游标执行更新，则新值可见。  
  
DYNAMIC  
定义一个游标，以反映游标所有者滚动游标时对结果集内的行所做的所有数据更改。 行的数据值、顺序和成员身份在每次提取时都会更改。 动态游标不支持绝对和相对提取选项。  
  
FAST_FORWARD  
指定启用了优化的 FORWARD_ONLY 和 READ_ONLY 游标。 如果还指定了 SCROLL，则不能指定 FAST_FORWARD。  
  
READ_ONLY  
禁止通过该游标进行更新。 在 UPDATE 或 DELETE 语句的 WHERE CURRENT OF 子句中不能引用游标。 该选项优先于要更新的游标的默认功能。  
  
SCROLL LOCKS  
指定通过游标进行的定位更新或删除一定会成功。 将行读入游标时 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将锁定这些行，以确保随后可对它们进行修改。 如果还指定了 FAST_FORWARD，则不能指定 SCROLL_LOCKS。  
  
OPTIMISTIC  
指定如果行自读入游标以来已得到更新，则通过游标进行的定位更新或定位删除不成功。 将行读入游标时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不锁定这些行。 相反，它使用 timestamp 列值的比较，或者如果表没有 timestamp 列，则使用校验和值，以确定将行读入游标后是否已修改该行。 如果已修改该行，尝试进行的定位更新或定位删除将失败。 如果还指定了 FAST_FORWARD，则不能指定 OPTIMISTIC。  
  
TYPE_WARNING  
指定如果游标从所请求的类型隐式转换为另一种类型，则向客户端发送警告消息。  
  
FOR select_statement  
定义游标结果集的标准 SELECT 语句。 在游标声明的 *select_statement* 中不允许使用关键字 FOR BROWSE 和 INTO。  
  
如果使用 DISTINCT、UNION、GROUP BY 或 HAVING，或者 *select_list* 中包含聚合表达式，游标将创建为 STATIC。  
  
如果每个基础表都没有唯一索引和 ISO SCROLL 游标，或如果请求了 [!INCLUDE[tsql](../../includes/tsql-md.md)] KEYSET 游标，则游标将自动成为 STATIC 游标。  
  
如果 *select_statement* 包含 ORDER BY 子句，且其中的列不是唯一的行标识符，则 DYNAMIC 游标将转换为 KEYSET 游标，或如果 KEYSET 游标不能打开，则该游标转换为 STATIC 游标。 此进程同样适用于使用 ISO 语法定义但不带 STATIC 关键字的游标。  
  
READ ONLY  
禁止通过该游标进行更新。 在 UPDATE 或 DELETE 语句的 WHERE CURRENT OF 子句中不能引用游标。 该选项优先于要更新的游标的默认功能。 该关键字与早期的 READ_ONLY 关键字的不同之处是 READ 和 ONLY 之间是一个空格，而不是下划线。  
  
`UPDATE [OF column_name[ ,... n ] ]`  
定义游标中可更新的列。 如果提供了 OF column_name [,...n]，则只允许修改列出的列。 如果没有提供列表，则可更新所有列，除非已将游标定义为 READ_ONLY。  
  
## <a name="remarks"></a>备注  
声明一个变量后，该变量将初始化为 NULL。 使用 SET 语句将一个不是 NULL 的值赋予声明的变量。 给变量赋值的 SET 语句返回单值。 在初始化多个变量时，为每个局部变量使用单独的 SET 语句。  
  
只能在表达式中使用变量，而不能代替对象名或关键字。 若要构造动态 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，请使用 EXECUTE。  
  
SET **@** _cursor_variable_ 的语法规则不包含 LOCAL 和 GLOBAL 关键字。 使用 SET **@** _cursor_variable_ = CURSOR... 语法时，根据“默认使用本地游标数据库”选项的设置，游标将创建为 GLOBAL 或 LOCAL。  
  
即使游标变量引用全局游标，它们也始终是局部变量。 如果游标变量引用全局游标，则该游标既有全局游标引用，也有局部游标引用。 有关详细信息，请参阅示例 C。  
  
有关详细信息，请参阅 [DECLARE CURSOR (Transact-SQL)](../../t-sql/language-elements/declare-cursor-transact-sql.md)。  
  
可将复合赋值运算符用于存在赋值（在运算符的右侧有一个表达式，包括变量）的任何地方以及 UPDATE、SELECT 和 RECEIVE 语句的 SET 中。  
  
不要在 SELECT 语句中使用变量来连接值（即，计算聚合值）。 可能发生了意外查询结果。 因为，SELECT 列表中的所有表达式（包括赋值）不一定对于每个输出行仅执行一次。 有关详细信息，请参阅[此知识库文章](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/287515)。  
  
## <a name="permissions"></a>权限  
要求具有 public 角色的成员身份。 所有用户都可以使用 SET @local_variable。  
  
## <a name="examples"></a>示例  
  
### <a name="a-printing-the-value-of-a-variable-initialized-by-using-set"></a>A. 输出使用 SET 初始化的变量值  
以下示例创建 `@myvar` 变量，将字符串值放入该变量，然后输出 `@myvar` 变量的值。  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT @myvar;  
GO  
```  
  
### <a name="b-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>B. 在 SELECT 语句中使用由 SET 赋值的局部变量  
以下示例创建一个名为 `@state` 的局部变量，并在 `SELECT` 语句中使用该局部变量来查找位于 `Oregon` 州的所有雇员的姓氏与名字。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @state CHAR(25);  
SET @state = N'Oregon';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name, City  
FROM HumanResources.vEmployee  
WHERE StateProvinceName = @state;  
```  
  
### <a name="c-using-a-compound-assignment-for-a-local-variable"></a>C. 为局部变量使用复合赋值  
以下两个示例将产生相同的结果。 它们创建一个名为 `@NewBalance` 的局部变量，将其乘以 10 并在一个 `SELECT` 语句中显示该局部变量的新值。 第二个示例使用一个复合赋值运算符。  
  
```sql  
/* Example one */  
DECLARE  @NewBalance  INT ;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT  @NewBalance;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT @NewBalance;  
```  
  
### <a name="d-using-set-with-a-global-cursor"></a>D. 对全局游标使用 SET  
以下示例创建一个局部变量，然后将游标变量设置为全局游标名。  
  
```sql  
DECLARE my_cursor CURSOR GLOBAL   
FOR SELECT * FROM Purchasing.ShipMethod  
DECLARE @my_variable CURSOR ;  
SET @my_variable = my_cursor ;   
--There is a GLOBAL cursor declared(my_cursor) and a LOCAL variable  
--(@my_variable) set to the my_cursor cursor.  
DEALLOCATE my_cursor;   
--There is now only a LOCAL variable reference  
--(@my_variable) to the my_cursor cursor.  
```  
  
### <a name="e-defining-a-cursor-by-using-set"></a>E. 使用 SET 定义游标  
以下示例使用 `SET` 语句定义游标。  
  
```sql  
DECLARE @CursorVar CURSOR;  
  
SET @CursorVar = CURSOR SCROLL DYNAMIC  
FOR  
SELECT LastName, FirstName  
FROM AdventureWorks2012.HumanResources.vEmployee  
WHERE LastName like 'B%';  
  
OPEN @CursorVar;  
  
FETCH NEXT FROM @CursorVar;  
WHILE @@FETCH_STATUS = 0  
BEGIN  
    FETCH NEXT FROM @CursorVar  
END;  
  
CLOSE @CursorVar;  
DEALLOCATE @CursorVar;  
```  
  
### <a name="f-assigning-a-value-from-a-query"></a>F. 通过查询赋值  
以下示例使用查询为变量赋值。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM Sales.Customer);  
SELECT @rows;  
```  
  
### <a name="g-assigning-a-value-to-a-user-defined-type-variable-by-modifying-a-property-of-the-type"></a>G. 通过修改类型属性为用户定义类型的变量赋值  
以下示例通过修改类型的 `Point` 属性的值来设置用户定义类型 `X` 的值。  
  
```sql  
DECLARE @p Point;  
SET @p.X = @p.X + 1.1;  
SELECT @p;  
GO  
```  
  
### <a name="h-assigning-a-value-to-a-user-defined-type-variable-by-invoking-a-method-of-the-type"></a>H. 通过调用类型的方法为用户定义类型的变量赋值  
以下示例通过调用类型的 `SetXY` 方法设置用户定义类型 point 的值。  
  
```sql  
DECLARE @p Point;  
SET @p=point.SetXY(23.5, 23.5);  
```  
  
### <a name="i-creating-a-variable-for-a-clr-type-and-calling-a-mutator-method"></a>I. 为 CLR 类型创建变量并调用赋值函数方法  
以下示例为 `Point` 类型创建变量，然后在 `Point` 中执行赋值函数方法。  
  
```sql  
CREATE ASSEMBLY mytest FROM 'c:\test.dll' WITH PERMISSION_SET = SAFE  
CREATE TYPE Point EXTERNAL NAME mytest.Point  
GO  
DECLARE @p Point = CONVERT(Point, '')  
SET @p.SetXY(22, 23);  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="j-printing-the-value-of-a-variable-initialized-by-using-set"></a>J. 输出使用 SET 初始化的变量值  
以下示例创建 `@myvar` 变量，将字符串值放入该变量，然后输出 `@myvar` 变量的值。  
  
```sql  
DECLARE @myvar CHAR(20);  
SET @myvar = 'This is a test';  
SELECT TOP 1 @myvar FROM sys.databases;
```  
  
### <a name="k-using-a-local-variable-assigned-a-value-by-using-set-in-a-select-statement"></a>K. 在 SELECT 语句中使用由 SET 赋值的局部变量  
以下示例创建一个名为 `@dept` 的局部变量，并在 `SELECT` 语句中使用该局部变量来查找 `Marketing` 部门的所有雇员的姓氏与名字。  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @dept CHAR(25);  
SET @dept = N'Marketing';  
SELECT RTRIM(FirstName) + ' ' + RTRIM(LastName) AS Name  
FROM DimEmployee   
WHERE DepartmentName = @dept;  
```  
  
### <a name="l-using-a-compound-assignment-for-a-local-variable"></a>L. 为局部变量使用复合赋值  
以下两个示例将产生相同的结果。 它们创建一个名为 `@NewBalance` 的局部变量，将其乘以 `10` 并在一个 `SELECT` 语句中显示该局部变量的新值。 第二个示例使用一个复合赋值运算符。  
  
```sql  
/* Example one */  
DECLARE  @NewBalance INT;  
SET  @NewBalance  =  10;  
SET  @NewBalance  =  @NewBalance  *  10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
  
/* Example Two */  
DECLARE @NewBalance INT = 10;  
SET @NewBalance *= 10;  
SELECT TOP 1 @NewBalance FROM sys.tables;  
```  
  
### <a name="m-assigning-a-value-from-a-query"></a>M. 通过查询赋值  
以下示例使用查询为变量赋值。  
  
```sql  
-- Uses AdventureWorks  
  
DECLARE @rows INT;  
SET @rows = (SELECT COUNT(*) FROM dbo.DimCustomer);  
SELECT TOP 1 @rows FROM sys.tables;  
```  
  
## <a name="see-also"></a>另请参阅  
[复合运算符 (Transact-SQL)](../../t-sql/language-elements/compound-operators-transact-sql.md)   
[DECLARE @local_variable (Transact-SQL)](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
[EXECUTE (Transact-SQL)](../../t-sql/language-elements/execute-transact-sql.md)   
[SELECT (Transact-SQL)](../../t-sql/queries/select-transact-sql.md)   
[SET 语句 (Transact-SQL)](../../t-sql/statements/set-statements-transact-sql.md)  
  
  

