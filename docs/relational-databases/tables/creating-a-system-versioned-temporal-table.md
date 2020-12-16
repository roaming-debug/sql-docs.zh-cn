---
description: 创建由系统控制版本的临时表
title: 创建由系统控制版本的时态表 | Microsoft Docs
ms.custom: ''
ms.date: 11/15/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 21e6d74f-711f-40e6-a8b7-85f832c5d4b3
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2b795f2fc65cade53dac533795d41ae8013e90cb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439385"
---
# <a name="creating-a-system-versioned-temporal-table"></a>创建由系统控制版本的时态表


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


根据指定历史记录表的方式，有三种方法可创建由系统版本的临时表：

- 使用匿名历史记录表创建临时表：指定当前表的架构，让系统使用自动生成的名称创建相应的历史记录表。
- 使用默认历史记录表创建临时表：指定历史记录表架构名称和表名，让系统在该架构中创建历史记录表。
- 使用事先创建的用户定义的历史记录表创建临时表：创建最满足你的需求的历史记录表，然后在临时表创建过程中引用该表。

## <a name="creating-a-temporal-table-with-an-anonymous-history-table"></a>使用匿名历史记录表创建临时表

使用“匿名”历史记录表创建临时表是一个可快速创建对象的方便选项，特别是在原型环境和测试环境中。 它也是创建时态表的最简单方法，因为它不需要在 SYSTEM_VERSIONING 子句中指定任何参数  。 下面的示例将在不定义历史记录表名称的情况下，创建启用了系统版本控制的新表。

```sql
CREATE TABLE Department
(
    DeptID INT NOT NULL PRIMARY KEY CLUSTERED
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL
  , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL
  , PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON);
```

### <a name="important-remarks"></a>重要提示

- 由系统版本的临时表必须已定义主键，并且已定义且只定义了一个 **PERIOD FOR SYSTEM_TIME** （其中包含两个 datetime2 列，声明为 **GENERATED ALWAYS AS ROW START / END**）
- **PERIOD** 列始终不可为 null，即使未指定是否为 null，也是如此。 如果将 PERIOD 列显式定义为可为 null，则 CREATE TABLE 语句将失败   。
- 历史记录表必须在列数、列名、排序和数据类型方面始终与当前表或临时表架构一致。
- 将在当前表或临时表所在的架构中自动创建匿名历史记录表。
- 匿名历史记录表名采用以下格式：MSSQL_TemporalHistoryFor_<current_temporal_table_object_id>_[suffix]  . 后缀是可选的，仅当表名的第一部分不唯一时才会添加它。
- 历史记录表将创建为行存储表。 如果可能，将应用页压缩，否则历史记录表将不会进行压缩。 例如，某些表配置（如稀疏列）不允许压缩。
- 将为历史记录表（名称自动生成，格式为 *IX_<history_table_name>* ）创建一个默认聚集索引。 聚集索引包含 **PERIOD** 列（结束、开始）。
- 若要将当前表创建为内存优化表，请参阅[系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)。

## <a name="creating-a-temporal-table-with-a-default-history-table"></a>使用默认历史记录表创建临时表

如果想要控制命名并仍依赖于系统创建具有默认配置的历史记录表，使用默认历史记录表创建临时表是一个方便的选项。 在下面的示例中，将在显式定义历史记录表的名称的情况下，创建启用了系统版本控制的新表。

```sql
CREATE TABLE Department
(
    DeptID INT NOT NULL PRIMARY KEY CLUSTERED
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL
  , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL
  , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DepartmentHistory));
```

### <a name="important-remarks"></a>重要提示

将使用适用于创建“匿名”历史记录表的相同规则来创建历史记录表，而以下规则专门适用于命名的历史记录表。

- 对于 **HISTORY_TABLE** 参数，架构名称是必需的。
- 如果指定的架构不存在， **CREATE TABLE** 语句将失败。
- 如果 **HISTORY_TABLE** 参数指定的表已存在，则将根据新创建的临时表就 [schema consistency and temporal data consistency](./temporal-tables.md)（架构一致性和临时数据一致性）对其进行验证。 如果指定的历史记录表无效，CREATE TABLE 语句将失败  。

## <a name="creating-a-temporal-table-with-a-user-defined-history-table"></a>使用用户定义的历史记录表创建临时表

如果用户想要指定具有特定存储选项和其他索引的历史记录表，则使用用户定义的历史记录表创建临时表是一个方便的选项。 在下面的示例中，将使用与要创建的临时表一致的架构创建用户定义的历史记录表。 将为此用户定义的历史记录表创建聚集列存储索引和其他非聚集行存储（B 树）索引，以便进行点查找。 创建此用户定义的历史记录表后，将通过将用户定义的历史记录表指定为默认历史记录表，创建由系统控制版本的临时表。

```sql
CREATE TABLE DepartmentHistory
(
    DeptID INT NOT NULL
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 NOT NULL
  , SysEndTime DATETIME2 NOT NULL
);
GO

CREATE CLUSTERED COLUMNSTORE INDEX IX_DepartmentHistory
    ON DepartmentHistory;
CREATE NONCLUSTERED INDEX IX_DepartmentHistory_ID_PERIOD_COLUMNS
    ON DepartmentHistory (SysEndTime, SysStartTime, DeptID);
GO

CREATE TABLE Department
(
    DeptID int NOT NULL PRIMARY KEY CLUSTERED
  , DeptName VARCHAR(50) NOT NULL
  , ManagerID INT NULL
  , ParentDeptID INT NULL
  , SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START NOT NULL
  , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END NOT NULL
  , PERIOD FOR SYSTEM_TIME (SysStartTime,SysEndTime)
)
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DepartmentHistory));
```

### <a name="important-remarks"></a>重要提示

- 如果打算对历史数据运行使用聚合或窗口函数的分析查询，则强烈建议创建聚集列存储作为主索引，因为这样会有非常好的数据压缩和查询性能。
- 如果主要用例是数据审核（即从当前表中搜索单个行的历史更改），则创建具有聚集索引的行存储历史记录表是个不错的选择
- 历史记录表不能具有一个主键、外键、唯一索引、表约束或触发器。 不能配置它以进行更改数据捕获、更改跟踪、事务复制或合并复制。

## <a name="alter-non-temporal-table-to-be-a-system-versioned-temporal-table"></a>将非时态表更改为由系统控制版本的时态表

当你需要使用现有表启用系统版本控制时（例如当你希望将自定义的时态解决方案迁移到内置支持时），适合使用此方法。
例如，你可能有一组表使用触发器实现了版本控制。 使用临时系统版本控制并不太复杂，并提供了其他好处，包括：

- 不可变的历史记录
- 用于按时间顺序查询的新语法
- 更好的 DML 性能
- 最低的维护成本

 转换现有表时，建议使用 HIDDEN  子句来隐藏新的 PERIOD  列（datetime2 类型列 SysStartTime  和 SysEndTime  ），以免影响不显式指定列名（例如，不含列列表的 SELECT * 或 INSERT）且不旨在处理新列的现有应用程序。

### <a name="adding-versioning-to-non-temporal-tables"></a>将版本控制添加到非临时表

如果你想要开始跟踪包含数据的非临时表的更改，则需要添加 **PERIOD** 定义，并可以选择为 SQL Server 将为你创建的空历史记录表提供名称：

```sql
CREATE SCHEMA History;
GO

ALTER TABLE InsurancePolicy
    ADD
        SysStartTime DATETIME2 GENERATED ALWAYS AS ROW START HIDDEN
            CONSTRAINT DF_SysStart DEFAULT SYSUTCDATETIME()
      , SysEndTime DATETIME2 GENERATED ALWAYS AS ROW END HIDDEN
            CONSTRAINT DF_SysEnd DEFAULT CONVERT(DATETIME2, '9999-12-31 23:59:59.9999999'),
        PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);
GO

ALTER TABLE InsurancePolicy
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = History.InsurancePolicy));
```

> [!IMPORTANT]
> DATETIME2 的精度必须与基础表的精度一致 - 请参阅以下备注。

#### <a name="important-remarks"></a>重要提示

- 对 SQL Server Enterprise Edition 以外的所有版本而言，将具有默认值的非 null 列添加到一张包含数据的现有表是一种关于数据大小的操作（对 Enterprise Edition 是一种元数据操作）。 对于 SQL Server Standard Edition 中包含数据的大型现有历史记录表而言，添加非 null 列代价高昂。
- 必须小心选择期间开始和期间结束列的约束：
  - 开始列的默认值指定从哪个时间点起应考虑现有行有效。 不能将它指定为将来的日期时间点。
  - 结束时间必须指定为一个给定 datetime2 精度的最大值，例如 `9999-12-31 23:59:59` 或 `9999-12-31 23:59:59.9999999`。
- 添加期间将对当前表执行数据一致性检查，以确保期间列的默认值有效。
- 如果在启用 **SYSTEM_VERSIONING** 时指定了现有历史记录表，则将对当前表和历史记录表同时执行临时数据一致性检查。 如果你将 DATA_CONISTENCY_CHECK = OFF 指定为一个附加参数，则可跳过此项检查  。

### <a name="migrate-existing-tables-to-built-in-support"></a>将现有表迁移到内置支持

此示例演示如何基于触发器将现有解决方案迁移到内置临时支持。 对于此示例，我们假定当前自定义解决方案将当前数据和历史数据拆分为两个单独的用户表（**ProjectTaskCurrent** 和 **ProjectTaskHistory**）。 如果现有解决方案使用单个表来存储实际行和历史行，则应在执行此示例所示的此迁移步骤前将数据拆分为两个表：

```sql
/*Drop trigger on future temporal table*/
DROP TRIGGER ProjectCurrent_OnUpdateDelete;
/*Make sure that future period columns are non-nullable*/
ALTER TABLE ProjectTaskCurrent ALTER COLUMN [ValidFrom] datetime2 NOT NULL;
ALTER TABLE ProjectTaskCurrent ALTER COLUMN [ValidTo] datetime2 NOT NULL;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [ValidFrom] datetime2 NOT NULL;
ALTER TABLE ProjectTaskHistory ALTER COLUMN [ValidTo] datetime2 NOT NULL;
ALTER TABLE ProjectTaskCurrent
    ADD PERIOD FOR SYSTEM_TIME ([ValidFrom], [ValidTo])
ALTER TABLE ProjectTaskCurrent
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProjectTaskHistory, DATA_CONSISTENCY_CHECK = ON));
```

#### <a name="important-remarks"></a>重要提示

- 引用 **PERIOD** 定义中的现有列会隐式将 generated_always_type 更改为这些列的 **AS_ROW_START** 和 **AS_ROW_END** 。
- 添加 **PERIOD** 将对当前表执行数据一致性检查，以确保期间列的现有值有效
- 强烈建议使用 **DATA_CONSISTENCY_CHECK = ON** 设置 **SYSTEM_VERSIONING** 来对现有数据强制执行数据一致性检查。
- 如果首选隐藏列，请使用命令 `ALTER TABLE [tableName] ALTER COLUMN [columnName] ADD HIDDEN;`。

## <a name="next-steps"></a>后续步骤

- [时态表](../../relational-databases/tables/temporal-tables.md)
 [由系统控制版本的时态表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)
- [在系统版本控制的临时表中修改数据](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [在系统版本控制临时表中查询数据](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [更改系统版本控制的临时表架构](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)
- [停止对系统版本的临时表的系统版本控制](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)