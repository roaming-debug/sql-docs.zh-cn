---
title: 更改内存优化表 | Microsoft Docs
description: 了解 ALTER TABLE 语句如何执行内存优化表的架构和索引更改。 在单个语句中结合使用 ADD、DROP 和 ALTER 操作。
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 690b70b7-5be1-4014-af97-54e531997839
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da12d83efd43c7ca6113348adee8e3bde14d9472
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465468"
---
# <a name="altering-memory-optimized-tables"></a>更改内存优化表

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

可以使用 ALTER TABLE 语句来更改内存优化表的架构和索引。 在 SQL Server 2016 和 Azure SQL 数据库中，对内存优化表的 ALTER TABLE 操作属于 OFFLINE，表示操作正在进行时，表格不能用于查询。 数据库应用程序可以继续运行，并且访问该表的任何操作都将被阻止，直到更改过程完成。 可以将多个 ADD、DROP 或 ALTER 操作合并为单个 ALTER TABLE 语句。

> [!IMPORTANT]
> Azure SQL 托管实例不支持常规用途服务层中的内存优化表。
  
## <a name="alter-table"></a>ALTER TABLE  

使用 ALTER TABLE 语法更改表架构，以及添加、删除和重新生成索引。 索引被视为表定义的一部分：  
  
- 语法 ALTER TABLE...内存优化表仅支持 ADD/DROP/ALTER INDEX。  
  
- 如果不使用 ALTER TABLE 语句，内存优化表的索引不支持 [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)、[DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 和 [PAD_INDEX](../../t-sql/statements/alter-table-index-option-transact-sql.md) 语句。  
  
支持下列更改类型：  
  
- 更改桶计数  
  
- 添加和删除索引  
  
- 更改、添加和删除列  
  
- 添加和删除约束  
  
 有关 ALTER TABLE 功能和完整语法的详细信息，请参阅 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)  
  
## <a name="schema-bound-dependency"></a>架构绑定依赖关系

 本机编译的存储过程都需要绑定到架构，即是说它们对所访问的内存优化表和所引用的列有一种绑定到架构的依赖关系。 架构绑定依赖关系是一种两个实体之间的关系，只要引用实体存在，这种关系就可以防止对被引用的实体进行删除或不兼容地更改。  
  
 例如，如果绑定到架构的本机编译存储过程从表 *mytable* 中引用了 *c1* 列，则 *c1* 列不能被删除。 同样，如果一个过程包含一个没有列列表的 INSERT 语句（如 `INSERT INTO dbo.mytable VALUES (...)`），那么表中没有可被删除的列。  

## <a name="logging-of-alter-table-on-memory-optimized-tables"></a>记录内存优化表上的 ALTER TABLE

在一个内存优化表中，大多数 ALTER TABLE 方案可同时运行，从而优化了向事务日志的写入。 这种优化是通过只将元数据更改记入事务日志中来实现的。 但是，以下 ALTER TABLE 操作将以单线程运行，而未经过日志优化。

本示例中的单线程操作会将改变表的整个内容都记录到事务日志中。 单线程操作的列表如下：

- 更改或添加列以使用大型对象 (LOB) 类型：nvarchar(max)、varchar(max) 或 varbinary(max)。

- 添加或删除列存储索引。

- 几乎所有内容都将影响 [行外列](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)。

  - 导致行上列移动行外列。
  - 导致行外列移动行上列。
  - 创建新的行外列。
  - 异常：已使用优化方式记录了延长行外列。
  
## <a name="examples"></a>示例

下面的示例将更改现有哈希索引的桶计数。 此操作将重新生成具有新的桶计数的哈希索引，并且哈希索引的其他属性保持不变。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem
       ALTER INDEX imPK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID  
              REBUILD WITH (BUCKET_COUNT=67108864);  
GO
```

下面的示例将添加一个带有 NOT NULL 约束和 DEFAULT 定义的列，并使用 WITH VALUES 为表中的各个现有行提供值。 如果没有使用 WITH VALUES，那么每一行的新列中都将包含 NULL 值。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD Comment NVARCHAR(100) NOT NULL DEFAULT N'' WITH VALUES;  
GO
```

下面的示例将在现有列中添加一个主键约束。  

```sql
CREATE TABLE dbo.UserSession (
   SessionId int not null,
   UserId int not null,
   CreatedDate datetime2 not null,
   ShoppingCartId int,
   index ix_UserId nonclustered hash (UserId) with (bucket_count=400000)
)
WITH (MEMORY_OPTIMIZED=ON, DURABILITY=SCHEMA_ONLY) ;  
GO  
  
ALTER TABLE dbo.UserSession  
       ADD CONSTRAINT PK_UserSession PRIMARY KEY NONCLUSTERED (SessionId);  
GO
```

下面的示例将删除一个索引。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       DROP INDEX ix_ModifiedDate;  
GO
```  

下面的示例将添加一个索引。  

```sql  
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD INDEX ix_ModifiedDate (ModifiedDate);  
GO  
```  

下面的示例将添加多个包含索引和约束的列。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD    CustomerID int NOT NULL DEFAULT -1 WITH VALUES,  
              ShipMethodID int NOT NULL DEFAULT -1 WITH VALUES,  
              INDEX ix_Customer (CustomerID);  
GO  
```

<a name="logging-of-alter-table-on-memory-optimized-tables-124"></a>

## <a name="see-also"></a>另请参阅  

[Memory-Optimized Tables](./sample-database-for-in-memory-oltp.md)