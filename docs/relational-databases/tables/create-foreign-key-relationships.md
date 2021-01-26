---
description: 创建外键关系
title: 创建外键关系 | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- relationships [SQL Server], creating
ms.assetid: 867a54b8-5be4-46e6-9702-49ae6dabf67c
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 230a1a00d66f1839be7a8135ff9c45022f0a2511
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765658"
---
# <a name="create-foreign-key-relationships"></a>创建外键关系


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

本文介绍了如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 在 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中创建外键关系。 当希望将一个表的行与另一个表的行相关联时，您可在这两个表之间创建关系。

## <a name="permissions"></a>权限

使用外键创建新表需要在数据库中具有 [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md) 权限，并对在其中创建表的架构具有 [ALTER](../../t-sql/statements/alter-schema-transact-sql.md) 权限。

在某一现有表中创建外键需要对该表具有 [ALTER](../../t-sql/statements/alter-table-transact-sql.md) 权限。

## <a name="limits-and-restrictions"></a><a name="BeforeYouBegin"></a> 限制和局限

- 外键约束不一定要链接到另一个表中的主键约束。 外键还可以定义为引用另一个表中 UNIQUE 约束的列。
- 如果在 FOREIGN KEY 约束的列中输入非 NULL 值，则此值必须在被引用列中存在。 否则，将返回外键冲突错误消息。 若要确保验证了组合外键约束的所有值，请对所有参与列指定 NOT NULL。
- FOREIGN KEY 约束仅能引用位于同一服务器上的同一数据库中的表。 跨数据库的引用完整性必须通过触发器实现。 有关详细信息，请参阅 [CREATE TRIGGER](../../t-sql/statements/create-trigger-transact-sql.md)。
- FOREIGN KEY 约束可引用同一表中的其他列，并称之为自引用。
- 在列级指定的 FOREIGN KEY 约束只能列出一个引用列。 此列的数据类型必须与定义约束的列的数据类型相同。
- 在表级指定的 FOREIGN KEY 约束所具有的引用列数目必须与约束列列表中的列数相同。 每个引用列的数据类型也必须与列表中相应列的数据类型相同。
- 对于表可包含的引用其他表的 FOREIGN KEY 约束的数目，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 没有预定义的限制。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 也不限制由引用特定表的其他表所拥有的 FOREIGN KEY 约束的数目。 但是，使用的 FOREIGN KEY 约束的实际数目受硬件配置以及数据库和应用程序设计的限制。 表最多可以将 253 个其他表和列作为外键引用（传出引用）。 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本将可在单独的表中引用的其他表和列（传入引用）的数量限制从 253 提高至 10,000。 （兼容性级别至少必须为 130。）数量限制的提高带来了下列约束：

  - DELETE 和 UPDATE DML 操作支持大于 253 个外键引用。 不支持 MERGE 操作。
  - 对自身进行外键引用的表仍只能进行 253 个外键引用。
  - 列存储索引、内存优化表和 Stretch Database 暂不支持进行超过 253 个外键引用。

- 对于临时表不强制 FOREIGN KEY 约束。
- 如果在 CLR 用户定义类型的列上定义外键，则该类型的实现必须支持二进制排序。 有关详细信息，请参阅 [CLR 用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md)。
- 仅当 FOREIGN KEY 约束引用的主键也定义为类型 **varchar(max)** 时，才能在此约束中使用类型为 **varchar(max)** 的列。

## <a name="create-a-foreign-key-relationship-in-table-designer"></a>在表设计器中创建外键关系

### <a name="using-sql-server-management-studio"></a>使用 SQL Server Management Studio

1. 在对象资源管理器中，右键单击将位于关系的外键方的表，再单击“设计”。

   此时，将在[表设计器](../../ssms/visual-db-tools/design-tables-visual-database-tools.md)中打开该表。
2. 在 **表设计器** 菜单上，单击 **“关系”** 。
3. 在“外键关系”对话框中，单击“添加”。

   “选定的关系”列表中将显示关系以及系统提供的名称，格式为 FK_\<*tablename*>_\<*tablename*>，其中 tablename 是外键表的名称。
4. 在 **“选定的关系”** 列表中单击该关系。
5. 单击右侧网格中的“表和列规范”，再单击该属性右侧的省略号 (…) 。
6. 在“表和列”对话框中，从“主键”下拉列表中选择要位于关系主键方的表。
7. 在下方的网格中，选择要分配给表的主键的列。 在每列右侧的相临网格单元格中，选择外键表的相应外键列。

   **表设计器** 将为此关系提供一个建议名称。 若要更改此名称，请编辑 **“关系名”** 文本框的内容。
8. 选择 **“确定”** 以创建该关系。
9. 关闭表设计器窗口，并保存更改，使外键关系更改生效。

## <a name="create-a-foreign-key-in-a-new-table"></a>在新表中创建外键

### <a name="using-transact-sql"></a>“使用 Transact-SQL”

下面的示例创建一个表，并对列 `TempID` 定义外键约束，以引用 AdventureWorks 数据库中 `Sales.SalesReason` 表内的列 `SalesReasonID`。 ON DELETE CASCADE 和 ON UPDATE CASCADE 子句用于确保对 `Sales.SalesReason` 表的更改自动传播到 `Sales.TempSalesReason` 表。    

```sql
CREATE TABLE Sales.TempSalesReason 
   (
      TempID int NOT NULL, Name nvarchar(50)
      , CONSTRAINT PK_TempSales PRIMARY KEY NONCLUSTERED (TempID)
      , CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
        REFERENCES Sales.SalesReason (SalesReasonID)
        ON DELETE CASCADE
        ON UPDATE CASCADE
   )
;
```

## <a name="create-a-foreign-key-in-an-existing-table"></a>在现有表中创建外键

### <a name="using-transact-sql"></a>“使用 Transact-SQL”
下面的示例对列 `TempID` 创建外键，并引用 AdventureWorks 数据库中 `Sales.SalesReason` 表内的列 `SalesReasonID`。

```sql
ALTER TABLE Sales.TempSalesReason
   ADD CONSTRAINT FK_TempSales_SalesReason FOREIGN KEY (TempID)
      REFERENCES Sales.SalesReason (SalesReasonID)
      ON DELETE CASCADE
      ON UPDATE CASCADE
;
```

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅：

- [主键和外键约束](primary-and-foreign-key-constraints.md)
- [GRANT 数据库权限](../../t-sql/statements/grant-database-permissions-transact-sql.md)
- [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md)
- [CREATE TABLE](../../t-sql/statements/create-table-transact-sql.md)
- [ALTER TABLE table_constraint](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)。
