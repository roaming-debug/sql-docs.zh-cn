---
description: NEXT VALUE FOR (Transact-SQL)
title: NEXT VALUE FOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NEXT_VALUE_TSQL
- NEXT
- NEXT VALUE
- NEXT VALUE FOR
- NEXT_TSQL
- NEXT_VALUE_FOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NEXT VALUE FOR function
- sequence number object, NEXT VALUE FOR function
ms.assetid: 92632ed5-9f32-48eb-be28-a5e477ef9076
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 02eaf8552bdbd25788d5a436230b58e3d14d6f9e
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91116666"
---
# <a name="next-value-for-transact-sql"></a>NEXT VALUE FOR (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  通过指定的序列对象生成序列号。  
  
 有关创建和使用序列的完整讨论，请参阅[序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。 可以使用 [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) 保留一定范围内的序列号。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
NEXT VALUE FOR [ database_name . ] [ schema_name . ]  sequence_name  
   [ OVER (<over_order_by_clause>) ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *database_name*  
 包含序列对象的数据库的名称。  
  
 *schema_name*  
 包含序列对象的架构的名称。  
  
 sequence_name  
 生成该编号的序列对象的名称。  
  
 *over_order_by_clause*  
 确定将序列值分配给分区中的行的顺序。 有关详细信息，请参阅 [OVER 子句 (Transact-SQL)](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
## <a name="return-types"></a>返回类型  
 使用序列类型返回一个数字。  
  
## <a name="remarks"></a>注解  
 可以在存储过程和触发器中使用 **NEXT VALUE FOR** 函数。  
  
 在查询或默认约束中使用 **NEXT VALUE FOR** 函数时，如果多次使用相同的序列对象，或者在提供这些值的语句以及执行的默认约束中使用相同的序列对象，则为结果集的行中引用同一序列的所有列返回相同的值。  
  
 **NEXT VALUE FOR** 函数具有不确定性，只允许在明确定义了生成的序列值数目的上下文中使用。 下面定义了给定语句中的每个引用的序列对象使用的值数目：  
  
-   **SELECT** - 对于每个引用的序列对象，将为语句结果中的每一行生成一次新值。  
  
-   INSERT ... **VALUES** - 对于每个引用的序列对象，将为语句中的每一个插入行生成一个新值。  
  
-   **UPDATE** - 对于每个引用的序列对象，将为语句所更新的每一行生成一个新值。  
  
-   过程语句（如 **DECLARE**、**SET** 等）- 对于每个引用的序列对象，将为每个语句生成一个新值。  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 不能在下列情况下使用 **NEXT VALUE FOR** 函数：  
  
-   数据库处于只读模式时。  
  
-   作为表值函数的参数。  
  
-   作为聚合函数的参数。  
  
-   在子查询中，包括公用表表达式和派生表。  
  
-   在视图、用户定义的函数或计算列中。  
  
-   在使用 **DISTINCT****UNION****UNION ALL****EXCEPT** 或 **INTERSECT** 运算符的语句中。  
  
-   在使用 ORDER BY 子句的语句中，除非使用了 NEXT VALUE FOR … OVER (ORDER BY …) 。  
  
-   在以下子句中：**FETCH****OVER****OUTPUT****ON****PIVOT****UNPIVOT****GROUP BY****HAVING****COMPUTE****COMPUTE BY** 或 **FOR XML**。  
  
-   在使用 **CASE****CHOOSE****COALESCE****IIF****ISNULL** 或 **NULLIF** 的条件表达式中。  
  
-   在不属于 **INSERT** 语句的 **VALUES** 子句中。  
  
-   在检查约束的定义中。  
  
-   在规则或默认对象的定义中。 （它可用于默认约束。）  
  
-   作为用户定义表类型中的默认值。  
  
-   在使用 **TOP**、**OFFSET** 的语句中，或在设置 **ROWCOUNT** 选项时。  
  
-   在语句的 **WHERE** 子句中。  
  
-   在 **MERGE** 语句中。 （在目标表的默认约束中使用 **NEXT VALUE FOR** 函数并且在 **MERGE** 语句的 **CREATE** 语句中使用默认值的情况下例外。）  
  
## <a name="using-a-sequence-object-in-a-default-constraint"></a>在默认约束中使用序列对象  
 在默认约束中使用 **NEXT VALUE FOR** 函数时，下列规则适用：  
  
-   可以从多个表的默认约束中引用单个序列对象。  
  
-   表和序列对象必须位于同一数据库中。  
  
-   添加默认约束的用户必须对序列对象具有 REFERENCES 权限。  
  
-   在删除默认约束之前，无法删除从默认约束中引用的序列对象。  
  
-   如果多个默认约束使用相同的序列对象，或者在提供这些值的语句以及执行的默认约束中使用相同的序列对象，则为行中的所有列返回相同的序列号。  
  
-   默认约束中对 **NEXT VALUE FOR** 函数的引用不能指定 **OVER** 子句。  
  
-   可以更改默认约束中引用的序列对象。  
  
-   如果 `INSERT ... SELECT` 或 `INSERT ... EXEC` 语句中插入的数据来自使用 **ORDER BY** 子句的查询，则按照 **ORDER BY** 子句指定的顺序生成 **NEXT VALUE FOR** 函数返回的值。  
  
## <a name="using-a-sequence-object-with-an-over-order-by-clause"></a>通过 OVER ORDER BY 子句使用序列对象  
 通过将 **OVER** 子句应用于 **NEXT VALUE FOR** 调用，**NEXT VALUE FOR** 函数支持生成排序的序列值。 通过使用 OVER 子句，可以向用户保证返回的值是按照 OVER 子句的 ORDER BY 子句的顺序生成的。 将 **NEXT VALUE FOR** 函数与 **OVER** 子句一起使用时，下列附加规则适用：  
  
-   如果在单个语句中为相同序列生成器多次调用 **NEXT VALUE FOR** 函数，这些调用必须使用相同的 **OVER** 子句定义。  
  
-   如果在单个语句中多次调用 **NEXT VALUE FOR** 函数以引用不同的序列生成器，则这些调用可以具有不同的 **OVER** 子句定义。  
  
-   为 **NEXT VALUE FOR** 函数应用的 **OVER** 子句不支持 **PARTITION BY** 子子句。  
  
-   如果 **SELECT** 语句中对 **NEXT VALUE FOR** 函数的所有调用均指定 **OVER** 子句，则可以在 **SELECT** 语句中使用 **ORDER BY** 子句。  
  
-   在 **SELECT** 语句或 `INSERT ... SELECT ...` 语句中使用时，允许将 **OVER** 子句与 **NEXT VALUE FOR** 函数一起使用。 不允许在 **UPDATE** 或 **MERGE** 语句中将 **OVER** 子句与 **NEXT VALUE FOR** 函数一起使用。  
  
-   如果另一个进程同时访问序列对象，则返回的编号可能会出现间断。  
  
## <a name="metadata"></a>Metadata  
 有关序列的信息，请查询 [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md) 目录视图。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 要求对序列对象或序列的架构具有 **UPDATE** 权限。 有关授予权限的示例，请参阅本主题后面的示例 F。  
  
### <a name="ownership-chaining"></a>所有权链接  
 序列对象支持所有权链接。 如果序列对象具有与调用存储过程、触发器或表相同的所有者（将序列对象作为默认约束），则不需要对序列对象进行权限检查。 如果序列对象与调用存储过程、触发器或表归不同的用户所有，则需要对序列对象进行权限检查。  
  
 如果在表中将 **NEXT VALUE FOR** 函数作为默认值，则用户需要对表具有 **INSERT** 权限和对序列对象具有 **UPDATE** 权限，才能使用默认值插入数据。  
  
-   如果默认约束具有与序列对象相同的所有者，则在调用默认约束时不需要具有序列对象的权限。  
  
-   如果默认约束和序列对象归不同的用户所有，即使通过默认约束调用序列对象，也需要具有序列对象的权限。  
  
### <a name="audit"></a>审核  
 若要审核 **NEXT VALUE FOR** 函数，请监视 SCHEMA_OBJECT_ACCESS_GROUP。  
  
## <a name="examples"></a>示例  
 有关创建序列和使用 NEXT VALUE FOR 函数生成序列号的示例，请参阅[序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
 以下示例在名为 `CountBy1` 的架构中使用名为 `Test` 的序列。 将执行以下语句以创建 `Test.CountBy1` 序列。 示例 C 和 E 使用 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库，因此，将在该数据库中创建 `CountBy1` 序列。  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
CREATE SCHEMA Test;  
GO  
  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="a-using-a-sequence-in-a-select-statement"></a>A. 在 SELECT 语句中使用序列  
 以下示例创建一个名为 `CountBy1` 的序列，每次使用该序列时将增加 1。  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1 AS FirstUse;  
SELECT NEXT VALUE FOR Test.CountBy1 AS SecondUse;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstUse  
1  
  
SecondUse  
2
```  
  
### <a name="b-setting-a-variable-to-the-next-sequence-value"></a>B. 将变量设置为下一个序列值  
 以下示例介绍了三种将变量设置为下一个序列号值的方法。  
  
```sql  
DECLARE @myvar1 BIGINT = NEXT VALUE FOR Test.CountBy1  
DECLARE @myvar2 BIGINT ;  
DECLARE @myvar3 BIGINT ;  
SET @myvar2 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar3 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar1 AS myvar1, @myvar2 AS myvar2, @myvar3 AS myvar3 ;  
GO  
```  
  
### <a name="c-using-a-sequence-with-a-ranking-window-function"></a>C. 将序列与排名窗口函数一起使用  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 OVER (ORDER BY LastName) AS ListNumber,  
    FirstName, LastName  
FROM Person.Contact ;  
GO  
```  
  
### <a name="d-using-the-next-value-for-function-in-the-definition-of-a-default-constraint"></a>D. 在默认约束定义中使用 NEXT VALUE FOR 函数  
 支持在默认约束定义中使用 **NEXT VALUE FOR** 函数。 有关在 **CREATE TABLE** 语句中使用 **NEXT VALUE FOR** 的示例，请参阅 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)中的示例 C。 以下示例使用 `ALTER TABLE` 将序列作为默认值添加到当前表中。  
  
```sql
CREATE TABLE Test.MyTable  
(  
    IDColumn NVARCHAR(25) PRIMARY KEY,  
    name VARCHAR(25) NOT NULL  
) ;  
GO  
  
CREATE SEQUENCE Test.CounterSeq  
    AS INT  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
ALTER TABLE Test.MyTable  
    ADD   
        DEFAULT N'AdvWorks_' +   
        CAST(NEXT VALUE FOR Test.CounterSeq AS NVARCHAR(20))   
        FOR IDColumn;  
GO  
  
INSERT Test.MyTable (name)  
VALUES ('Larry') ;  
GO  
  
SELECT * FROM Test.MyTable;  
GO  
```  
  
### <a name="e-using-the-next-value-for-function-in-an-insert-statement"></a>E. 在 INSERT 语句中使用 NEXT VALUE FOR 函数  
 以下示例创建一个名为 `TestTable` 的表，然后使用 `NEXT VALUE FOR` 函数插入一行。  
  
```sql  
CREATE TABLE Test.TestTable  
     (CounterColumn INT PRIMARY KEY,  
    Name NVARCHAR(25) NOT NULL) ;   
GO  
  
INSERT Test.TestTable (CounterColumn,Name)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Syed') ;  
GO  
  
SELECT * FROM Test.TestTable;   
GO  
```  
  
### <a name="e-using-the-next-value-for-function-with-select--into"></a>E. 将 NEXT VALUE FOR 函数与 SELECT … 一起使用INTO  
 以下示例使用 `SELECT ... INTO` 语句创建一个名为 `Production.NewLocation` 的表，然后使用 `NEXT VALUE FOR` 函数为每一行编号。  
  
```sql  
USE AdventureWorks2012 ;   
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 AS LocNumber, Name   
    INTO Production.NewLocation  
    FROM Production.Location ;  
GO  
  
SELECT * FROM Production.NewLocation ;  
GO  
```  
  
### <a name="f-granting-permission-to-execute-next-value-for"></a>F. 授予执行 NEXT VALUE FOR 的权限  
 以下示例为名为 `AdventureWorks\Larry` 的用户授予 **UPDATE** 权限以使用 `Test.CounterSeq` 序列执行 `NEXT VALUE FOR`。  
  
```sql  
GRANT UPDATE ON OBJECT::Test.CounterSeq TO [AdventureWorks\Larry] ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
