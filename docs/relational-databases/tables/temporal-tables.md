---
description: 临时表
title: 时态表 | Microsoft Docs
ms.custom: ''
ms.date: 07/11/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: e442303d-4de1-494e-94e4-4f66c29b5fb9
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 203522bf6cc70e5df2c55d9a179081c92775f92f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97426858"
---
# <a name="temporal-tables"></a>临时表


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


SQL Server 2016 以数据库功能的形式引入了对时态表（也称为由系统控制版本的时态表）的支持，其附带的内置支持可以提供表中存储的数据在任意时间点的相关信息，而不仅仅是数据在当前时刻正确的信息。 临时表是 ANSI SQL 2011 中引入的数据库功能。

**快速入门**

- **入门：**

  - [系统版本控制临时表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
  - [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
  - [临时表使用方案](../../relational-databases/tables/temporal-table-usage-scenarios.md)

  - [Azure SQL 数据库中的临时表入门](/azure/azure-sql/temporal-tables)

- **示例：**

  - [创建由系统控制版本的临时表](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
  - [使用带有系统版本的内存优化临时表](../../relational-databases/tables/working-with-memory-optimized-system-versioned-temporal-tables.md)
  - [在系统版本控制的临时表中修改数据](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
  - [在系统版本控制临时表中查询数据](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
  - **下载 Adventure Works 示例数据库：** 若要开始使用时态表，请下载包含脚本示例的 [适用于 SQL Server 的 AdventureWorks 数据库](https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2016_EXT.bak)，并遵循文件夹“Temporal”中的说明

- **语法：**

  - [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)
  - [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)
  - [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)
- 视频：有关时态表的 20 分钟讨论，请参阅 [Temporal in SQL Server 2016](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)（SQL Server 2016 中的时态表）。

## <a name="what-is-a-system-versioned-temporal-table"></a>什么是由系统控制版本的时态表

带有系统版本的时态表是用户表的一种类型，旨在保留完整的数据更改历史记录，并实现轻松的时间点分析。 这种类型的临时表之所以称为版本由系统控制的临时表，是因为每一行的有效期由系统（即数据库引擎）管理。

每个临时表有两个显式定义的列，其中每个列都有一个 **datetime2** 数据类型。 这些列称为期限列。 每当修改了某行后，系统将以独占方式使用这些期限列来记录每行的有效期。

除了这些期限列以外，临时表还包含对使用镜像架构的另一个表的引用。 每当更新或删除了临时表中的某行后，系统将使用此表来自动存储该行的先前版本。 此附加表称为历史记录表，而存储当前（实际）行版本的主表称为当前表，或直接称为临时表。 在创建临时表期间，用户可以指定现有的历史记录表（必须与架构相符），或者让系统创建默认的历史记录表。

## <a name="why-temporal"></a>为何使用时态表

实际的数据源是动态的，业务决策多半依赖于分析师从数据演变中获得的见解。 临时表的用例包括：

- 在必要时审核所有数据变更并执行数据取证
- 重构数据在过去任意时间之前的状态
- 计算各时间段的趋势
- 为决策支持应用程序保持一个慢速变化的维度
- 在发生意外的数据更改和应用程序错误后进行恢复

## <a name="how-does-temporal-work"></a>时态表的工作原理是什么

 表的系统版本控制是以一对表（当前表和历史记录表）的形式实现的。 在其中每个表中，以下两个附加 **datetime2** 列用于定义每行的有效期：

- 期限开始时间列：系统在此列（通常表示为 **SysStartTime** 列）中记录行的开始时间。
- 期限结束时间列：系统在此列（通常表示为 SysEndTime 列）中记录行的结束时间。

当前表包含每个行的当前值。 历史记录表包含每个行的每个先前值（如果有），以及该行生效的开始时间和结束时间。

![显示临时表工作方式的示意图。](../../relational-databases/tables/media/temporal-howworks.PNG "Temporal-HowWorks")

以下简单示例演示了在虚构 HR 数据库中包含员工信息的方案：

```sql
CREATE TABLE dbo.Employee
(
  [EmployeeID] int NOT NULL PRIMARY KEY CLUSTERED
  , [Name] nvarchar(100) NOT NULL
  , [Position] varchar(100) NOT NULL
  , [Department] varchar(100) NOT NULL
  , [Address] nvarchar(1024) NOT NULL
  , [AnnualSalary] decimal (10,2) NOT NULL
  , [ValidFrom] datetime2 GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.EmployeeHistory));
```

- **INSERTS：** 在 INSERT 上，系统基于系统时钟将 SysStartTime 列（在本例中，它名为 ValidFrom）的值设置为当前事务的开始时间（位于 UTC 时区），并将 SysEndTime 列（在本例中，它名为 ValidTo）的值指定为最大值 9999-12-31    。 这会将行标记为已打开。
- **UPDATES：** 对于 **UPDATE**，系统将行的先前值存储在历史记录表中，并基于系统时钟将 **SysEndTime** 列的值设置为当前事务的开始时间（位于 UTC 时区）。 这会将行标记为已关闭，并记录该行有效的期限。 在当前表中，将使用新值更新行，同时，系统会基于系统时钟将 **SysStartTime** 列的值设置为事务的开始时间（位于 UTC 时区）。 在当前表中， **SysEndTime** 列的更新行值将保留最大值 9999-12-31。
- **DELETES：** 对于 **DELETE**，系统将行的先前值存储在历史记录表中，并基于系统时钟将 **SysEndTime** 列的值设置为当前事务的开始时间（位于 UTC 时区）。 这会将行标记为已关闭，并记录前一行有效的期限。 在当前表中，该行将被删除。 对当前表的查询不会返回此行。 处理历史记录数据的查询将返回已关闭的行的数据。
- **MERGE：** 对于 MERGE，根据 MERGE 语句中被指定为操作的内容，该操作的行为与最多执行了三个语句（INSERT、UPDATE 和/或 DELETE）完全一样    。

> [!IMPORTANT]
> 系统 datetime2 列中记录的时间基于事务本身的开始时间。 例如，在单个事务中插入的所有行具有对应于 **SYSTEM_TIME** 的开始时间段列中记录的相同 UTC 时间。

## <a name="how-do-i-query-temporal-data"></a>如何查询时态数据

SELECT 语句 FROM\<table\> 子句提供新的 FOR SYSTEM_TIME 子句和五个特定于临时表的子子句，用于跨当前表和历史记录表查询数据 。 支持对通过多个联接传播的，以及通过多个临时表顶层的视图传播的单个表直接使用这种新的 **SELECT** 语句语法。

![显示临时查询工作方式的示意图。](../../relational-databases/tables/media/temporal-querying.PNG "Temporal-Querying")

以下查询将搜索 EmployeeID = 1000 且在 2014 年 1 月 1 日至 2015 年 1 月 1 日的某段时间（包括上限）内保持活动状态的员工行的行版本：

```sql
SELECT * FROM Employee
  FOR SYSTEM_TIME
    BETWEEN '2014-01-01 00:00:00.0000000' AND '2015-01-01 00:00:00.0000000'
      WHERE EmployeeID = 1000 ORDER BY ValidFrom;
```

> [!NOTE]
> FOR SYSTEM_TIME 筛选出有效期持续时间为零 (SysStartTime = SysEndTime)   的行。
> 如果你对同一事务中的同一个主键执行多项更新，将生成这些行。
> 在这种情况下，临时查询只会返回发生事务之前的行版本，以及发生事务之后实际生成的行版本。
> 如果需要在分析中包括这些行，请直接查询历史记录表。

在下表中，“符合条件的行”列中的 SysStartTime 表示正在查询的表中 **SysStartTime** 列的值， **SysEndTime** 表示正在查询的表中 **SysEndTime** 列的值。 有关完整语法和示例的信息，请参阅 [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md) 和 [在系统版本控制临时表中查询数据](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)的支持。

|表达式|符合条件的行|说明|
|----------------|---------------------|-----------------|
|**AS OF**<date_time>|SysStartTime \<= date_time AND SysEndTime > date_time|返回一个表，其行中包含过去指定时间点的实际（当前）值。 在内部，时态表及其历史记录表之间将进行联合，然后筛选结果以返回在 <date_time> 参数指定的时间点有效的行中的值。 如果 system_start_time_column_name 值小于或等于 <date_time> 参数值，并且 system_end_time_column_name 值大于 <date_time> 参数值，则此行的值视为有效   。|
|**FROM**<start_date_time>**TO**<end_date_time>|SysStartTime < end_date_time AND SysEndTime > start_date_time|返回一个表，其中包含在指定的时间范围内保持活动状态的所有行版本的值，不管这些版本是在 FROM 自变量的 <start_date_time> 参数之前开始活动，还是在 TO 自变量的 <end_date_time> 参数值之后停止活动 。 在内部，将在临时表及其历史记录表之间进行联合，然后筛选结果，以返回在指定时间范围内任意时间保持活动状态的所有行版本的值。 正好在 FROM 终结点定义的下限时间停止活动的行将被排除，正好在 TO 终结点定义的上限时间开始活动的记录也将被排除。|
|**BETWEEN**<start_date_time>**AND**<end_date_time>|SysStartTime \<= end_date_time AND SysEndTime > start_date_time|与上面的 **FOR SYSTEM_TIME FROM** <start_date_time>**TO** <end_date_time> 描述相同，不过，返回的行表包括在 <end_date_time> 终结点定义的上限时间激活的行。|
|**CONTAINED IN** (<start_date_time> , <end_date_time>)|SysStartTime >= start_date_time AND SysEndTime \<= end_date_time|返回一个表，其中包含在 CONTAINED IN 参数的两个日期时间值定义的时间范围内打开和关闭的所有行版本的值。 正好在下限时间激活的记录，或者在上限时间停止活动的行将包括在内。|
|**ALL**|所有行|返回属于当前表和历史记录表的行的联合。|

> [!NOTE]
> （可选）可以选择隐藏这些期限列，以便不显式引用这些期限列的查询不会返回这些列（SELECT \* FROM\<table\> 方案）。 若要返回隐藏的列，只需在查询中显式引用隐藏的列。 同样，如果这些新的期限列不存在， **INSERT** 和 **BULK INSERT** 语句将会继续（并且列值将自动填充）。 有关使用 **HIDDEN** 子句的详细信息，请参阅 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md) 和 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)的支持。

## <a name="next-steps"></a>后续步骤

- [系统版本控制临时表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [临时表使用方案](../../relational-databases/tables/temporal-table-usage-scenarios.md)
- [临时表注意事项和限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [管理版本由系统控制的临时表中历史数据的保留期](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [临时表安全性](../../relational-databases/tables/temporal-table-security.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)