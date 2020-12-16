---
description: 更改系统版本控制的临时表架构
title: 更改系统版本控制的临时表架构 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 9dbe5a21-9335-4f8b-85fd-9da83df79946
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4b57082f1ce4f76e191c0237e80f404199a9ac4a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462638"
---
# <a name="changing-the-schema-of-a-system-versioned-temporal-table"></a>更改由系统控制版本的时态表的架构


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

使用 **ALTER TABLE** 语句添加、更改或删除列。

## <a name="examples"></a>示例

下面是一些关于更改临时表架构的示例。

```sql
ALTER TABLE dbo.Department
    ALTER COLUMN DeptName varchar(100);

ALTER TABLE dbo.Department
    ADD WebAddress nvarchar(255) NOT NULL
    CONSTRAINT DF_WebAddress DEFAULT 'www.mycompany.com';

ALTER TABLE dbo.Department
    ADD TempColumn INT;

GO

ALTER TABLE dbo.Department
    DROP COLUMN TempColumn;

/* Setting IsHidden property for period columns.
Use ALTER COLUMN <period_column> DROP HIDDEN to clear IsHidden flag */

ALTER TABLE dbo.Department
    ALTER COLUMN SysStartTime ADD HIDDEN;

ALTER TABLE dbo.Department
    ALTER COLUMN SysEndTime ADD HIDDEN;
```

### <a name="important-remarks"></a>重要提示

- 若要更改临时表的架构，需要具有对当前和历史记录表的 **CONTROL** 权限。
- 在 **ALTER TABLE** 操作过程中，系统持有这两个表的架构锁。
- 指定的架构更改会以合适的方式（具体取决于更改的类型）传播到历史记录表。
- 如果你添加不可为 null 的列，或将现有列更改为不可为 null，则必须指定现有行的默认值。 系统将使用相同的值生成额外的默认值，并将其应用于历史记录表。 将 **DEFAULT** 添加到非空表是在所有除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition（在此版本上为元数据操作）以外的版本上的数据操作的大小。
- 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的所有版本上，使用默认值添加 varchar(max)、nvarchar(max)、varbinary(max) 或 XML 列将被视为更新数据操作。
- 如果添加列后的行大小超出行大小限制，则不能联机添加新列。
- 使用新的 NOT NULL 列扩展表后，请考虑删除对历史记录表的默认约束，因为系统将自动填充此表中的所有列。
- 对于系统版本控制的临时表，Online 选项 (**WITH (ONLINE = ON**) 对 **ALTER TABLE ALTER COLUMN** 不起任何作用。 无论为 ONLINE 选项指定的值是什么，都不会联机执行 ALTER 列。
- 你可以使用 **ALTER COLUMN** 更改 period 列的 **IsHidden** 属性。
- 不能使用直接 **ALTER** 进行以下架构更改。 对于这些类型的更改，请设置 **SYSTEM_VERSIONING = OFF**。

  - 添加计算列
  - 添加 **IDENTITY** 列
  - 当历史记录表设置为 **DATA_COMPRESSION = PAGE** 或 **DATA_COMPRESSION = ROW**（历史记录表的默认值）时，添加 **SPARSE** 列或将现有列更改为 **SPARSE**。
  - 添加 **COLUMN_SET**
  - 添加 **ROWGUIDCOL** 列或将现有列更改为 **ROWGUIDCOL**

下例演示了如何在仍然需要 **SYSTEM_VERSIONING = OFF** 的情况下更改架构（添加 **IDENTITY** 列）。 此示例禁用了数据一致性检查。 在不可以更改任何并发数据的情况下，当在某个事务内进行架构更改时，可以省略此检查。

```sql
    BEGIN TRAN
        ALTER TABLE [dbo].[CompanyLocation] SET (SYSTEM_VERSIONING = OFF);
        ALTER TABLE [CompanyLocation] ADD Cntr INT IDENTITY (1,1);
        ALTER TABLE [dbo].[CompanyLocationHistory] ADD Cntr INT NOT NULL DEFAULT 0;
        ALTER TABLE [dbo].[CompanyLocation]
    SET
         (
            SYSTEM_VERSIONING = ON
           ( HISTORY_TABLE = [dbo].[CompanyLocationHistory])
         );
    COMMIT ;
```

## <a name="next-steps"></a>后续步骤

- [时态表](../../relational-databases/tables/temporal-tables.md)
 [由系统控制版本的时态表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)
- [创建由系统控制版本的临时表](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
- [在系统版本控制的临时表中修改数据](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [在系统版本控制临时表中查询数据](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [停止对系统版本的临时表的系统版本控制](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)
