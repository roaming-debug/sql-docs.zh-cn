---
description: OUTPUT 子句 (Transact-SQL)
title: OUTPUT 子句 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/14/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OUTPUT_TSQL
- OUTPUT
dev_langs:
- TSQL
helpviewer_keywords:
- displaying updated rows
- INSERT statement [SQL Server], OUTPUT clause
- outputs [SQL Server]
- OUTPUT clause
- row additions [SQL Server], OUTPUT clause
- viewing updated rows
- row deletions [SQL Server], OUTPUT clause
- viewing deleted rows
- DELETE statement [SQL Server], OUTPUT clause
- row updates [SQL Server]
- displaying inserted rows
- viewing inserted rows
- displaying deleted rows
- UPDATE statement [SQL Server], OUTPUT clause
ms.assetid: 41b9962c-0c71-4227-80a0-08fdc19f5fe4
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f248f7a26dee135cbb68d7d0660ea8e467f76d15
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207670"
---
# <a name="output-clause-transact-sql"></a>OUTPUT 子句 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回受 INSERT、UPDATE、DELETE 或 MERGE 语句影响的各行中的信息，或返回基于受这些语句影响的各行的表达式。 这些结果可以返回到处理应用程序，以供在确认消息、存档以及其他类似的应用程序要求中使用。 也可以将这些结果插入表或表变量。 另外，您可以捕获嵌入的 INSERT、UPDATE、DELETE 或 MERGE 语句中 OUTPUT 子句的结果，然后将这些结果插入目标表或视图。  
  
> [!NOTE]  
>  对于具有 OUTPUT 子句的 UPDATE、INSERT 或 DELETE 语句，即使在遇到错误需要回滚时，也会将行返回到客户端。 如果在运行语句的过程中出现任何错误，都不应使用该结果。  
  
 **用于以下语句：**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [INSERT](../../t-sql/statements/insert-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 [MERGE](../../t-sql/statements/merge-transact-sql.md)  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
<OUTPUT_CLAUSE> ::=  
{  
    [ OUTPUT <dml_select_list> INTO { @table_variable | output_table } [ ( column_list ) ] ]  
    [ OUTPUT <dml_select_list> ]  
}  
<dml_select_list> ::=  
{ <column_name> | scalar_expression } [ [AS] column_alias_identifier ]  
    [ ,...n ]  
  
<column_name> ::=  
{ DELETED | INSERTED | from_table_name } . { * | column_name }  
    | $action
```
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 \@table_variable  
 指定 table 变量，返回的行将插入此变量，而不是返回给调用方。 \@table_variable 必须在 INSERT、UPDATE、DELETE 或 MERGE 语句前声明。  
  
 如果未指定 column_list，则 table 变量必须与 OUTPUT 结果集具有相同的列数。 标识列和计算列除外，这两种列必须跳过。 如果指定了 column_list，则任何省略的列都必须允许 NULL 值，或者都分配有默认值。  
  
 有关 table 变量的详细信息，请参阅 [table (Transact-SQL)](../../t-sql/data-types/table-transact-sql.md)。  
  
 output_table  
 指定一个表，返回的行将插入该表中而不是返回到调用方。 output_table 可以为临时表。  
  
 如果未指定 column_list，则 table 必须与 OUTPUT 结果集具有相同的列数。 标识列和计算列例外， 必须跳过这两种列。 如果指定了 column_list，则任何省略的列都必须允许 NULL 值，或者都分配有默认值。  
  
 output_table 无法：  
  
-   具有启用的对其定义的触发器。  
  
-   参与 FOREIGN KEY 约束的任意一方。  
  
-   具有 CHECK 约束或启用的规则。  
  
column_list  
 INTO 子句目标表上列名的可选列表。 它类似于 [INSERT](../../t-sql/statements/insert-transact-sql.md) 语句中允许使用的列列表。  
  
 *scalar_expression*  
 可取计算结果为单个值的任何符号和运算符的组合。 scalar_expression 中不允许使用聚合函数。  
  
 对修改的表中的列的任何引用都必须使用 INSERTED 或 DELETED 前缀限定。  
  
 column_alias_identifier  
 用于引用列名的代替名称。  
  
 DELETED  
 指定由更新或删除操作删除的值的列前缀。 以 DELETED 为前缀的列反映了 UPDATE、DELETE 或 MERGE 语句完成之前的值。  
  
 不能在 INSERT 语句中同时使用 DELETED 与 OUTPUT 子句。  
  
 INSERTED  
 列的前缀，指定由插入操作或更新操作添加的值。 以 INSERTED 为前缀的列反映了在 UPDATE、INSERT 或 MERGE 语句完成之后但在触发器执行之前的值。  
  
 INSERTED 语句不能与 DELETE 语句的 OUTPUT 子句同时使用。  
  
 from_table_name  
 是一个列前缀，指定 DELETE、UPDATE 或 MERGE 语句（用于指定要更新或删除的行）的 FROM 子句中包含的表。  
  
 如果还在 FROM 子句中指定了要修改的表，则对该表中的列的任何引用都必须使用 INSERTED 或 DELETED 前缀限定。  
  
 \*  
 指定受删除、插入或更新操作影响的所有列都将按照它们在表中的顺序返回。  
  
 例如，以下 DELETE 语句中的 `OUTPUT DELETED.*` 将返回 `ShoppingCartItem` 表中所有已删除的列：  
  
```sql
DELETE Sales.ShoppingCartItem
    OUTPUT DELETED.*;
```
  
 column_name  
 显式列引用。 任何对正在修改的表的引用都必须使用相应的 INSERTED 或 DELETED 前缀正确限定，例如：INSERTED **.** _column\_name_。  
  
 $action  
 仅可用于 MERGE 语句。 在 MERGE 语句的 OUTPUT 子句中指定一个 **nvarchar(10)** 类型的列，该子句为每行返回以下三个值之一：“INSERT”、“UPDATE”或“DELETE”（具体视对相应行执行的操作而定）。  
  
## <a name="remarks"></a>备注  
 OUTPUT \<dml_select_list> 子句和 OUTPUT \<dml_select_list> INTO { **\@** _table\_variable_ | _output\_table_ } 子句可以在单个 INSERT、UPDATE、DELETE 或 MERGE 语句中进行定义。  
  
> [!NOTE]  
>  除非另行指定，否则，对 OUTPUT 子句的引用将同时引用 OUTPUT 子句和 OUTPUT INTO 子句。  
  
 OUTPUT 子句对于在 INSERT 或 UPDATE 操作之后检索标识列或计算列的值可能非常有用。  
  
 当 \<dml_select_list> 中包含计算列时，输出表或表变量中的相应列并不是计算列。 新列中的值是在执行该语句时计算出的值。  
  
 无法保证将更改应用于表的顺序与将行插入输出表或表变量的顺序相对应。  
  
 如果将参数或变量作为 UPDATE 语句的一部分进行了修改，则 OUTPUT 子句将始终返回语句执行之前的参数或变量的值而不是已修改的值。  
  
 在使用 WHERE CURRENT OF 语法通过游标定位的 UPDATE 或 DELETE 语句中，可以使用 OUTPUT。  
  
 以下语句中不支持 OUTPUT 子句：  
  
-   引用本地分区视图、分布式分区视图或远程表的 DML 语句。  
  
-   包含 EXECUTE 语句的 INSERT 语句。  
  
-   当数据库兼容级别设置为 100 时，不允许在 OUTPUT 子句中使用全文谓词。  
  
-   不能将 OUTPUT INTO 子句插入视图或行集函数。  
  
-   如果用户定义的函数包含一个以表为目标的 OUTPUT INTO 子句，则不能创建该函数。  
  
 若要防止出现不确定的行为，OUTPUT 子句不能包含以下引用：  
  
-   执行用户或系统数据访问的子查询或用户定义函数，或者被认定会执行此类访问的子查询或用户定义函数。 如果用户定义函数未绑定到架构，则认定它会执行数据访问。  
  
-   视图或内嵌表值函数中的一个列（如果该列由以下方法之一定义）：  
  
    -   子查询。  
  
    -   执行用户数据访问或系统数据访问或者被认为执行此种访问的用户定义函数。  
  
    -   定义中包含执行用户数据访问或系统数据访问的用户定义函数的计算列。  
  
     如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 OUTPUT 子句中检测到了此类列，将引发错误 4186。   
  
## <a name="inserting-data-returned-from-an-output-clause-into-a-table"></a>将从 OUTPUT 子句返回的数据插入表  
 在捕获嵌套的 INSERT、UPDATE、DELETE 或 MERGE 语句中 OUTPUT 子句的结果并将这些结果插入目标表时，请牢记以下信息：  
  
-   整个操作是原子的。 INSERT 语句和包含 OUTPUT 子句的嵌套 DML 语句要么都执行，要么整个语句都失败。  
  
-   以下限制适用于外层 INSERT 语句的目标：  
  
    -   目标不能为远程表、视图或公用表表达式。  
  
    -   目标不能有 FOREIGN KEY 约束，或者被 FOREIGN KEY 约束所引用。  
  
    -   不能对目标定义触发器。  
  
    -   目标不能参与合并复制或事务复制的可更新订阅。  
  
-   对于嵌套的 DML 语句有以下限制：  
  
    -   目标不能为远程表或分区视图。  
  
    -   源本身不能包含 \<dml_table_source> 子句。  
  
-   包含 \<dml_table_source> 子句的 INSERT 语句中不支持 OUTPUT INTO 子句。  
  
-   \@\@ROWCOUNT 返回仅由外部 INSERT 语句插入的行。  
  
-   \@\@IDENTITY、SCOPE_IDENTITY 和 IDENT_CURRENT 返回仅由嵌套的 DML 语句生成的标识值，而不返回外部 INSERT 语句生成的标识值。  
  
-   查询通知将语句作为单个实体进行处理，并且即使重大更改是来自外层 INSERT 语句本身，所创建的任何消息的类型也将是嵌套 DML 的类型。  
  
-   在 \<dml_table_source> 子句中，SELECT 和 WHERE 子句不能包括子查询、聚合函数、排名函数、全文谓词、执行数据访问的用户定义函数或 TEXTPTR 函数。  

## <a name="parallelism"></a>并行度
 可将结果返回客户端的 OUTPUT 子句将始终使用串行计划。

在兼容性级别设置为 130 或更高的数据库的上下文中，如果 INSERT...SELECT 操作使用 SELECT 语句的 WITH (TABLOCK) 提示，并且使用 OUTPUT…INTO 插入临时表或用户表，则 INSERT…SELECT 的目标表将可以进行并行操作（具体取决于子树成本）。  OUTPUT INTO 子句中引用的目标表不能进行并行操作。 
 
## <a name="triggers"></a>触发器  
 从 OUTPUT 中返回的列反映 INSERT、UPDATE 或 DELETE 语句完成之后但在触发器执行之前的数据。  
  
 对于 INSTEAD OF 触发器，即使没有因为触发器的操作而发生修改，也会如同实际执行 INSERT、UPDATE 或 DELETE 那样生成返回的结果。 如果在触发器的主体内使用包含 OUTPUT 子句的语句，则必须使用表别名来引用触发器 inserted 和 deleted 表，以免使用与 OUTPUT 关联的 INSERTED 和 DELETED 表复制列引用。  
  
 如果指定了 OUTPUT 子句但未同时指定 INTO 关键字，则对于给定的 DML 操作，DML 操作的目标不能启用对其定义的任何触发器。 例如，如果在 UPDATE 语句中定义了 OUTPUT 子句，则目标表不能具有任何启用的 UPDATE 触发器。  
  
 如果设置了 sp_configure 选项 disallow results from triggers，则从触发器内调用语句时，不带 INTO 子句的 OUTPUT 子句将导致该语句失败。  
  
## <a name="data-types"></a>数据类型  
 OUTPUT 子句支持大型对象数据类型：nvarchar(max)、varchar(max)、varbinary(max)、text、ntext、image 和 xml      。 当在 UPDATE 语句中使用 .WRITE 子句修改 nvarchar(max)、varchar(max) 或 varbinary(max) 列时，如果引用了值的全部前像和后像，则将其返回  。 在 OUTPUT 子句中，TEXTPTR( ) 函数不能作为 text、ntext 或 image 列的表达式的一部分出现  。  
  
## <a name="queues"></a>队列  
 可以在将表用作队列或将表用于保持中间结果集的应用程序中使用 OUTPUT。 换句话说，应用程序不断地在表中添加或删除行。 以下示例在 DELETE 语句中使用 OUTPUT 子句将已删除的行返回到执行调用的应用程序。  
  
```sql
USE AdventureWorks2012;
GO

DELETE TOP(1) dbo.DatabaseLog WITH (READPAST)  
OUTPUT deleted.*  
WHERE DatabaseLogID = 7;  
GO
```
  
 此示例从用作队列的表中删除一行，并使用单个操作将已删除的值返回到处理应用程序。 还可实现其他语义，例如使用表来实现堆栈。 但是，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并不保证由使用 OUTPUT 子句的 DML 语句处理和返回行的顺序。 应用程序负责包括可保证所需语义的适当 WHERE 子句，或者理解当针对 DML 操作可能限定多行时，没有保证的顺序。 以下示例使用子查询，并假定 `DatabaseLogID` 列具有唯一性特征以实现所需的排序语义。  
  
```sql
USE tempdb;
GO

CREATE TABLE dbo.table1  
(  
    id INT,  
    employee VARCHAR(32)  
);  
GO  
  
INSERT INTO dbo.table1 VALUES   
      (1, 'Fred')  
     ,(2, 'Tom')  
     ,(3, 'Sally')  
     ,(4, 'Alice');  
GO  
  
DECLARE @MyTableVar TABLE  
(  
    id INT,  
    employee VARCHAR(32)  
);  
  
PRINT 'table1, before delete'   
SELECT * FROM dbo.table1;  
  
DELETE FROM dbo.table1  
OUTPUT DELETED.* INTO @MyTableVar  
WHERE id = 4 OR id = 2;  
  
PRINT 'table1, after delete'  
SELECT * FROM dbo.table1;  
  
PRINT '@MyTableVar, after delete'  
SELECT * FROM @MyTableVar;  
  
DROP TABLE dbo.table1;  
```

```
--Results  
--table1, before delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--2           Tom  
--3           Sally  
--4           Alice  
--  
--table1, after delete  
--id          employee  
------------- ------------------------------  
--1           Fred  
--3           Sally  
--@MyTableVar, after delete  
--id          employee  
------------- ------------------------------  
--2           Tom  
--4           Alice
```
  
> [!NOTE]  
>  如果您的方案允许多个应用程序从一个表中执行析构性读取，请在 UPDATE 和 DELETE 语句中使用 READPAST 表提示。 这可防止在其他应用程序已经读取表中第一个限定记录的情况下出现锁定问题。  
  
## <a name="permissions"></a>权限  
 要求对通过 \<dml_select_list> 检索或在 \<scalar_expression> 中使用的任何列具有 SELECT 权限。  
  
 要求对 \<output_table> 中指定的任何表具有 INSERT 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-output-into-with-a-simple-insert-statement"></a>A. 将 OUTPUT INTO 与简单 INSERT 语句一起使用  
 下例向 `ScrapReason` 表插入一行，并使用 `OUTPUT` 子句将语句的结果返回给 `@MyTableVar``table` 变量。 由于 `ScrapReasonID` 列使用 IDENTITY 属性定义，因此未在 `INSERT` 语句中为该列指定一个值。 但请注意，将在列 `OUTPUT` 内的 `inserted.ScrapReasonID` 子句中返回由[!INCLUDE[ssDE](../../includes/ssde-md.md)]为该列生成的值。  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (
    NewScrapReasonID SMALLINT,  
    Name VARCHAR(50),  
    ModifiedDate DATETIME); 
    
INSERT Production.ScrapReason  
    OUTPUT INSERTED.ScrapReasonID, INSERTED.Name, INSERTED.ModifiedDate  
        INTO @MyTableVar  
VALUES (N'Operator error', GETDATE());  
  
--Display the result set of the table variable.  
SELECT NewScrapReasonID, Name, ModifiedDate FROM @MyTableVar;  
--Display the result set of the table.  
SELECT ScrapReasonID, Name, ModifiedDate   
FROM Production.ScrapReason;  
GO
```
  
### <a name="b-using-output-with-a-delete-statement"></a>B. 将 OUTPUT 与 DELETE 语句一起使用  
 以下示例将删除 `ShoppingCartItem` 表中的所有行。 子句 `OUTPUT deleted.*` 指定 `DELETE` 语句的结果（即已删除的行中的所有列）返回到执行调用的应用程序。 后面的 `SELECT` 语句验证对 `ShoppingCartItem` 表所执行的删除操作的结果。  
  
```sql
USE AdventureWorks2012;
GO

DELETE Sales.ShoppingCartItem  
OUTPUT DELETED.*   
WHERE ShoppingCartID = 20621;  
  
--Verify the rows in the table matching the WHERE clause have been deleted.  
SELECT COUNT(*) AS [Rows in Table] FROM Sales.ShoppingCartItem WHERE ShoppingCartID = 20621;  
GO
```
  
### <a name="c-using-output-into-with-an-update-statement"></a>C. 将 OUTPUT INTO 与 UPDATE 语句一起使用  
 下面的示例将 `VacationHours` 表中 `Employee` 列的前 10 行更新 25%。 `OUTPUT` 子句将返回 `VacationHours` 值，该值在将 `UPDATE` 列中的 `deleted.VacationHours` 语句和 `inserted.VacationHours` 列中的已更新值应用于 `@MyTableVar` 表变量之前存在。  
  
 在它后面的两个 `SELECT` 语句返回 `@MyTableVar` 中的值以及 `Employee` 表中更新操作的结果。  
  
```sql
USE AdventureWorks2012;
GO
  
DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()   
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours, ModifiedDate  
FROM @MyTableVar;  
GO  
--Display the result set of the table.  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO  
```

### <a name="d-using-output-into-to-return-an-expression"></a>D. 使用 OUTPUT INTO 返回表达式  
 下例建立在示例 C 的基础上，它在 `OUTPUT` 子句中定义一个表达式，作为更新后的 `VacationHours` 值与应用更新前的 `VacationHours` 值之间的差。 该表达式的值返回给列 `VacationHoursDifference` 中的 `@MyTableVar``table` 变量。  
  
```sql
USE AdventureWorks2012;  
GO

DECLARE @MyTableVar TABLE (  
    EmpID INT NOT NULL,  
    OldVacationHours INT,  
    NewVacationHours INT,  
    VacationHoursDifference INT,  
    ModifiedDate DATETIME);  
  
UPDATE TOP (10) HumanResources.Employee  
SET VacationHours = VacationHours * 1.25,  
    ModifiedDate = GETDATE()  
OUTPUT inserted.BusinessEntityID,  
       deleted.VacationHours,  
       inserted.VacationHours,  
       inserted.VacationHours - deleted.VacationHours,  
       inserted.ModifiedDate  
INTO @MyTableVar;  
  
--Display the result set of the table variable.  
SELECT EmpID, OldVacationHours, NewVacationHours,   
    VacationHoursDifference, ModifiedDate  
FROM @MyTableVar;  
GO  
SELECT TOP (10) BusinessEntityID, VacationHours, ModifiedDate  
FROM HumanResources.Employee;  
GO
```

### <a name="e-using-output-into-with-from_table_name-in-an-update-statement"></a>E. 在 UPDATE 语句中使用包含 from_table_name 的 OUTPUT INTO  
 以下示例使用指定的 `ProductID` 和 `ScrapReasonID`，针对 `WorkOrder` 表中的所有工作顺序更新 `ScrapReasonID` 列。 `OUTPUT INTO` 子句返回所更新表 (`WorkOrder`) 中的值以及 `Product` 表中的值。 在 `Product` 子句中使用 `FROM` 表来指定要更新的行。 由于 `WorkOrder` 表上定义了 `AFTER UPDATE` 触发器，因此需要 `INTO` 关键字。  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTestVar TABLE (  
    OldScrapReasonID INT NOT NULL,   
    NewScrapReasonID INT NOT NULL,   
    WorkOrderID INT NOT NULL,  
    ProductID INT NOT NULL,  
    ProductName NVARCHAR(50)NOT NULL);  
  
UPDATE Production.WorkOrder  
SET ScrapReasonID = 4  
OUTPUT deleted.ScrapReasonID,  
       inserted.ScrapReasonID,   
       inserted.WorkOrderID,  
       inserted.ProductID,  
       p.Name  
    INTO @MyTestVar  
FROM Production.WorkOrder AS wo  
    INNER JOIN Production.Product AS p   
    ON wo.ProductID = p.ProductID   
    AND wo.ScrapReasonID= 16  
    AND p.ProductID = 733;  
  
SELECT OldScrapReasonID, NewScrapReasonID, WorkOrderID,   
    ProductID, ProductName   
FROM @MyTestVar;  
GO
```

### <a name="f-using-output-into-with-from_table_name-in-a-delete-statement"></a>F. 在 DELETE 语句中使用包含 from_table_name 的 OUTPUT INTO  
 以下示例将按照在 `ProductProductPhoto` 语句的 `FROM` 子句中所定义的搜索条件删除 `DELETE` 表中的行。 `OUTPUT` 子句返回所删除表（`deleted.ProductID`、`deleted.ProductPhotoID`）中的列以及 `Product` 表中的列。 在 `FROM` 子句中使用该表来指定要删除的行。  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
    WHERE p.ProductModelID BETWEEN 120 and 130;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, ProductModelID, PhotoID   
FROM @MyTableVar  
ORDER BY ProductModelID;  
GO
```
  
### <a name="g-using-output-into-with-a-large-object-data-type"></a>G. 将 OUTPUT INTO 与大型对象数据类型一起使用  
 以下示例使用 `DocumentSummary` 子句更新 `nvarchar(max)` 表内 `Production.Document` 这一 `.WRITE` 列中的部分值。 通过指定替换单词、现有数据中要替换的单词的开始位置（偏移量）以及要替换的字符数（长度），将单词 `components` 替换为单词 `features`。 此示例使用 `OUTPUT` 子句将 `DocumentSummary` 列的前像和后像返回到 `@MyTableVar``table` 变量。 请注意，将返回 `DocumentSummary` 列的全部前像和后像。  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    SummaryBefore NVARCHAR(max),  
    SummaryAfter NVARCHAR(max));  
  
UPDATE Production.Document  
SET DocumentSummary .WRITE (N'features',28,10)  
OUTPUT deleted.DocumentSummary,   
       inserted.DocumentSummary   
    INTO @MyTableVar  
WHERE Title = N'Front Reflector Bracket Installation';  
  
SELECT SummaryBefore, SummaryAfter   
FROM @MyTableVar;  
GO
```
  
### <a name="h-using-output-in-an-instead-of-trigger"></a>H. 在 INSTEAD OF 触发器中使用 OUTPUT  
 下例在触发器中使用 `OUTPUT` 子句来返回触发器操作的结果。 首先，创建一个 `ScrapReason` 表的视图，然后对该视图定义 `INSTEAD OF INSERT` 触发器，从而使用户只修改基表的 `Name` 列。 由于 `ScrapReasonID` 列在基表中是 `IDENTITY` 列，因此触发器忽略用户提供的值。 这允许[!INCLUDE[ssDE](../../includes/ssde-md.md)]自动生成正确的值。 同样，用户为 `ModifiedDate` 提供的值也被忽略并设置为正确的日期。 `OUTPUT` 子句返回实际插入 `ScrapReason` 表中的值。  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID('dbo.vw_ScrapReason','V') IS NOT NULL  
    DROP VIEW dbo.vw_ScrapReason;  
GO  
CREATE VIEW dbo.vw_ScrapReason  
AS (SELECT ScrapReasonID, Name, ModifiedDate  
    FROM Production.ScrapReason);  
GO  
CREATE TRIGGER dbo.io_ScrapReason   
    ON dbo.vw_ScrapReason  
INSTEAD OF INSERT  
AS  
BEGIN  
--ScrapReasonID is not specified in the list of columns to be inserted   
--because it is an IDENTITY column.  
    INSERT INTO Production.ScrapReason (Name, ModifiedDate)  
        OUTPUT INSERTED.ScrapReasonID, INSERTED.Name,   
               INSERTED.ModifiedDate  
    SELECT Name, getdate()  
    FROM inserted;  
END  
GO  
INSERT vw_ScrapReason (ScrapReasonID, Name, ModifiedDate)  
VALUES (99, N'My scrap reason','20030404');  
GO
```
  
 这是在 2004 年 4 月 12 日 ('`2004-04-12'`) 生成的结果集。 请注意，`ScrapReasonIDActual` 和 `ModifiedDate` 列反映由触发器操作生成的值而不是 `INSERT` 语句中提供的值。  
  
 ```
 ScrapReasonID  Name             ModifiedDate  
 -------------  ---------------- -----------------------  
 17             My scrap reason  2004-04-12 16:23:33.050
 ```  
  
### <a name="i-using-output-into-with-identity-and-computed-columns"></a>I. 将 OUTPUT INTO 与标识列和计算列一起使用  
 下面的示例创建 `EmployeeSales` 表，然后使用 `INSERT` 语句向其中插入若干行，并使用 `SELECT` 语句从源表中检索数据。 `EmployeeSales` 表包含标识列 (`EmployeeID`) 和计算列 (`ProjectedSales`)。  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID ('dbo.EmployeeSales', 'U') IS NOT NULL  
    DROP TABLE dbo.EmployeeSales;  
GO  
CREATE TABLE dbo.EmployeeSales  
( EmployeeID   INT IDENTITY (1,5) NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales AS CurrentSales * 1.10   
);  
GO  
DECLARE @MyTableVar TABLE (  
  EmployeeID   INT NOT NULL,  
  LastName     NVARCHAR(20) NOT NULL,  
  FirstName    NVARCHAR(20) NOT NULL,  
  CurrentSales MONEY NOT NULL,  
  ProjectedSales MONEY NOT NULL  
  );  
  
INSERT INTO dbo.EmployeeSales (LastName, FirstName, CurrentSales)  
  OUTPUT INSERTED.EmployeeID,
         INSERTED.LastName,   
         INSERTED.FirstName,   
         INSERTED.CurrentSales,
         INSERTED.ProjectedSales
  INTO @MyTableVar  
    SELECT c.LastName, c.FirstName, sp.SalesYTD  
    FROM Sales.SalesPerson AS sp  
    INNER JOIN Person.Person AS c  
        ON sp.BusinessEntityID = c.BusinessEntityID  
    WHERE sp.BusinessEntityID LIKE '2%'  
    ORDER BY c.LastName, c.FirstName;  
  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM @MyTableVar;  
GO  
SELECT EmployeeID, LastName, FirstName, CurrentSales, ProjectedSales  
FROM dbo.EmployeeSales;  
GO
```
  
### <a name="j-using-output-and-output-into-in-a-single-statement"></a>J. 在单个语句中使用 OUTPUT 和 OUTPUT INTO  
 以下示例将按照在 `ProductProductPhoto` 语句的 `FROM` 子句中所定义的搜索条件删除 `DELETE` 表中的行。 `OUTPUT INTO` 子句将被删除表中的列（`deleted.ProductID`、`deleted.ProductPhotoID`）及 `Product` 表中的列返回给 `@MyTableVar``table` 变量。 在 `Product` 子句中使用 `FROM` 表来指定要删除的行。 `OUTPUT` 子句将 `deleted.ProductID` 表中的 `deleted.ProductPhotoID`、`ProductProductPhoto` 列以及行的删除日期和时间返回到执行调用的应用程序。  
  
```sql
USE AdventureWorks2012;
GO

DECLARE @MyTableVar TABLE (  
    ProductID INT NOT NULL,   
    ProductName NVARCHAR(50)NOT NULL,  
    ProductModelID INT NOT NULL,   
    PhotoID INT NOT NULL);  
  
DELETE Production.ProductProductPhoto  
OUTPUT DELETED.ProductID,  
       p.Name,  
       p.ProductModelID,  
       DELETED.ProductPhotoID  
    INTO @MyTableVar  
OUTPUT DELETED.ProductID, DELETED.ProductPhotoID, GETDATE() AS DeletedDate   
FROM Production.ProductProductPhoto AS ph  
JOIN Production.Product as p   
    ON ph.ProductID = p.ProductID   
WHERE p.ProductID BETWEEN 800 and 810;  
  
--Display the results of the table variable.  
SELECT ProductID, ProductName, PhotoID, ProductModelID   
FROM @MyTableVar;  
GO
```
  
### <a name="k-inserting-data-returned-from-an-output-clause"></a>K. 插入从 OUTPUT 子句返回的数据  
 下面的示例捕获从 `OUTPUT` 语句的 `MERGE` 子句返回的数据，并将这些数据插入另一个表。 `MERGE` 语句每天根据在 `Quantity` 表中处理的订单更新 `ProductInventory` 表的 `SalesOrderDetail` 列。 如果产品的库存降至 `0` 或更低，它还会删除与这些产品对应的行。 本示例捕获已删除的行并将这些行插入另一个表 `ZeroInventory` 中，该表跟踪没有库存的产品。  
  
```sql
USE AdventureWorks2012;
GO

IF OBJECT_ID(N'Production.ZeroInventory', N'U') IS NOT NULL  
    DROP TABLE Production.ZeroInventory;  
GO  
--Create ZeroInventory table.  
CREATE TABLE Production.ZeroInventory (DeletedProductID int, RemovedOnDate DateTime);  
GO  
  
INSERT INTO Production.ZeroInventory (DeletedProductID, RemovedOnDate)  
SELECT ProductID, GETDATE()  
FROM  
(   MERGE Production.ProductInventory AS pi  
    USING (SELECT ProductID, SUM(OrderQty) FROM Sales.SalesOrderDetail AS sod  
           JOIN Sales.SalesOrderHeader AS soh  
           ON sod.SalesOrderID = soh.SalesOrderID  
           AND soh.OrderDate = '20070401'  
           GROUP BY ProductID) AS src (ProductID, OrderQty)  
    ON (pi.ProductID = src.ProductID)  
    WHEN MATCHED AND pi.Quantity - src.OrderQty <= 0  
        THEN DELETE  
    WHEN MATCHED  
        THEN UPDATE SET pi.Quantity = pi.Quantity - src.OrderQty  
    OUTPUT $action, deleted.ProductID) AS Changes (Action, ProductID)  
WHERE Action = 'DELETE';  
IF @@ROWCOUNT = 0  
PRINT 'Warning: No rows were inserted';  
GO  
SELECT DeletedProductID, RemovedOnDate
FROM Production.ZeroInventory;
GO
```
  
## <a name="see-also"></a>另请参阅  
 [DELETE (Transact-SQL)](../../t-sql/statements/delete-transact-sql.md)   
 [INSERT (Transact-SQL)](../../t-sql/statements/insert-transact-sql.md)   
 [UPDATE (Transact-SQL)](../../t-sql/queries/update-transact-sql.md)   
 [表 (Transact-SQL)](../../t-sql/data-types/table-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
