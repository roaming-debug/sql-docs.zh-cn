---
description: 使用表值参数（数据库引擎）
title: 使用表值参数（数据库引擎）| Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- table-valued parameters
- table-valued parameters, about table-valued parameters
- parameters [SQL Server], table-valued
- TVP See table-valued parameters
ms.assetid: 5e95a382-1e01-4c74-81f5-055612c2ad99
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ca1d6cee1a989f8a035831b0e42a5a6ba672c101
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748907"
---
# <a name="use-table-valued-parameters-database-engine"></a>使用表值参数（数据库引擎）

[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

表值参数是使用用户定义的表类型来声明的。 使用表值参数，可以不必创建临时表或许多参数，即可向 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句或例程（如存储过程或函数）发送多行数据。

表值参数与 OLE DB 和 ODBC 中的参数数组类似，但具有更高的灵活性，且与 [!INCLUDE[tsql](../../includes/tsql-md.md)] 的集成更紧密。 表值参数的另一个优势是能够参与基于数据集的操作。

[!INCLUDE[tsql](../../includes/tsql-md.md)] 通过引用向例程传递表值参数，以避免创建输入数据的副本。 可以使用表值参数创建和执行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 例程，并且可以使用任何托管语言从 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码、托管客户端以及本机客户端调用它们。

 **本主题内容：**

[优点](#Benefits)

[限制](#Restrictions)

[表值参数与 BULK INSERT 操作](#BulkInsert)

[示例](#Example)

## <a name="benefits"></a><a name="Benefits"></a> 优势

就像其他参数一样，表值参数的作用域也是存储过程、函数或动态 [!INCLUDE[tsql](../../includes/tsql-md.md)] 文本。 同样，表类型变量也与使用 DECLARE 语句创建的其他任何局部变量一样具有作用域。 可以在动态 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句内声明表值变量，并且可以将这些变量作为表值参数传递到存储过程和函数。

表值参数具有更高的灵活性，在某些情况下，可比临时表或其他传递参数列表的方法提供更好的性能。 表值参数具有以下优势：

- 首次从客户端填充数据时，不获取锁。
- 提供简单的编程模型。
- 允许在单个例程中包括复杂的业务逻辑。
- 减少到服务器的往返。
- 可以具有不同基数的表结构。
- 是强类型。
- 使客户端可以指定排序顺序和唯一键。
- 在用于存储过程时像临时表一样被缓存。 从 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]开始，对于参数化查询，表值参数也被缓存。

## <a name="permissions"></a><a name="Permissions"></a> 权限
若要创建用户定义的表类型的实例或使用表值参数调用存储过程，用户必须对该类型或包含该类型的架构或数据库具有 EXECUTE 权限。

## <a name="restrictions"></a><a name="Restrictions"></a> 限制

表值参数有下面的限制：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不维护表值参数列的统计信息。
- 表值参数必须作为输入 READONLY 参数传递到 [!INCLUDE[tsql](../../includes/tsql-md.md)] 例程。 不能在例程体中对表值参数执行诸如 UPDATE、DELETE 或 INSERT 这样的 DML 操作。
- 不能将表值参数用作 SELECT INTO 或 INSERT EXEC 语句的目标。 表值参数可以在 SELECT INTO 的 FROM 子句中，也可以在 INSERT EXEC 字符串或存储过程中。

## <a name="table-valued-parameters-vs-bulk-insert-operations"></a><a name="BulkInsert"></a> 表值参数与 BULK INSERT 操作

表值参数的使用方法与其他基于数据集的变量的使用方法相似；但是，频繁使用表值参数将比大型数据集要快。 大容量操作的启动开销比表值参数大，与之相比，表值参数在插入数目少于 1000 的行时具有很好的执行性能。

重用的表值参数可从临时表缓存中受益。 这一表缓存功能可比对等的 BULK INSERT 操作提供更好的伸缩性。 使用小型行插入操作时，可以通过使用参数列表或批量语句（而不是 BULK INSERT 操作或表值参数）来获得小的性能改进。 但是，这些方法在编程上不太方便，并且随着行的增加，性能会迅速下降。

表值参数在执行性能上与对等的参数阵列实现相当甚至更好。

## <a name="example"></a><a name="Example"></a> 示例

下面的示例使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 并展示了如何执行以下操作：创建表值参数类型，声明变量来引用它，填充参数列表，然后将值传递到 AdventureWorks 数据库中的存储过程。

```sql
/* Create a table type. */
CREATE TYPE LocationTableType 
   AS TABLE
      ( LocationName VARCHAR(50)
      , CostRate INT );
GO
/* Create a procedure to receive data for the table-valued parameter. */
CREATE PROCEDURE dbo. usp_InsertProductionLocation
   @TVP LocationTableType READONLY
      AS
      SET NOCOUNT ON
      INSERT INTO AdventureWorks2012.Production.Location
         (
            Name
            , CostRate
            , Availability
            , ModifiedDate
         )
      SELECT *, 0, GETDATE()
      FROM @TVP;
GO
/* Declare a variable that references the type. */
DECLARE @LocationTVP AS LocationTableType;
/* Add data to the table variable. */
INSERT INTO @LocationTVP (LocationName, CostRate)
   SELECT Name, 0.00
   FROM AdventureWorks2012.Person.StateProvince;
  
/* Pass the table variable data to a stored procedure. */
EXEC usp_InsertProductionLocation @LocationTVP;
```

## <a name="see-also"></a>另请参阅

- [CREATE TYPE](../../t-sql/statements/create-type-transact-sql.md)
- [DECLARE @local_variable](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
- [sys.types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
- [sys.parameters](../../relational-databases/system-catalog-views/sys-parameters-transact-sql.md)
- [sys.parameter_type_usages](../../relational-databases/system-catalog-views/sys-parameter-type-usages-transact-sql.md)
- [CREATE PROCEDURE](../../t-sql/statements/create-procedure-transact-sql.md)
- [CREATE FUNCTION](../../t-sql/statements/create-function-transact-sql.md)  
