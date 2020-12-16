---
description: 在系统版本控制临时表中查询数据
title: 在系统版本控制临时表中查询数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 2d358c2e-ebd8-4eb3-9bff-cfa598a39125
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1efbdca9b0d3d7919cf3e3f54837decd37bdf5b0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484439"
---
# <a name="querying-data-in-a-system-versioned-temporal-table"></a>在系统控制版本的时态表中查询数据


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


如果想要获取临时表中数据的最新（实际）状态，完全可以像查询非临时表一样进行查询。 如果 PERIOD 列未隐藏，其值将出现在 SELECT \* 查询中。 如果已将“PERIOD”列指定为隐藏，其值将不会出现在 SELECT \* 查询中。 当 **PERIOD** 列隐藏时，可在 SELECT 子句中明确引用 **PERIOD** 列以返回这些列的值。

若要执行任何一种基于时间的分析，请将新的 FOR SYSTEM_TIME 子句和四个特定时态的子子句结合使用，以便跨当前表和历史记录表查询数据。 有关这些子句的详细信息，请参阅[临时表](../../relational-databases/tables/temporal-tables.md)和 [FROM (Transact SQL)](../../t-sql/queries/from-transact-sql.md)

- AS OF <date_time>
- FROM <start_date_time> TO <end_date_time>
- BETWEEN <start_date_time> AND <end_date_time>
- CONTAINED IN (<start_date_time> , <end_date_time>)
- ALL

可以在查询中为每个表单独指定 **FOR SYSTEM_TIME** 。 它可以在公用表表达式、表值函数和存储过程内使用。 将表别名与时态表配合使用时，时态表名称和别名之间必须包含 FOR SYSTEM_TIME 子句（请参阅下面“使用 AS OF 子句查询特定时间”中的第二个示例）。

## <a name="query-for-a-specific-time-using-the-as-of-sub-clause"></a>使用 AS OF 子子句查询特定时间

当因为数据处于过去的任意特定时间而需要重新构造数据状态时，可使用 AS OF 子子句。 你可以使用 **PERIOD** 列定义中指定的 datetime2 类型的精度来重新构造数据。

AS OF 子子句可以与常数文本或变量结合使用，从而让你动态指定时间条件。 所提供的值解释为 UTC 时间。

第一个示例返回 dbo.Department 表从过去某个特定日期起的状态。

```sql
/*State of entire table AS OF specific date in the past*/
SELECT [DeptID], [DeptName], [SysStartTime],[SysEndTime]
FROM [dbo].[Department]
FOR SYSTEM_TIME AS OF '2015-09-01 T10:00:00.7230011' ;
```

第二个示例对行子集两个时间点之间的值进行比较。

```sql
DECLARE @ADayAgo datetime2
SET @ADayAgo = DATEADD (day, -1, sysutcdatetime())
/*Comparison between two points in time for subset of rows*/
SELECT D_1_Ago.[DeptID], D.[DeptID],
D_1_Ago.[DeptName], D.[DeptName],
D_1_Ago.[SysStartTime], D.[SysStartTime],
D_1_Ago.[SysEndTime], D.[SysEndTime]
FROM [dbo].[Department] FOR SYSTEM_TIME AS OF @ADayAgo AS D_1_Ago
JOIN [Department] AS D ON D_1_Ago.[DeptID] = [D].[DeptID]
AND D_1_Ago.[DeptID] BETWEEN 1 and 5 ;
```

### <a name="using-views-with-as-of-sub-clause-in-temporal-queries"></a>在临时查询中将视图与 AS-OF 子子句结合使用

在需要进行复杂时间点分析的情况下，使用视图非常有用。 常见的示例是使用上个月的值生成现在的业务报表。

通常情况下，客户会有一个规范化的数据库模型，该模型涉及多个具有外键关系的表。 如何让该规范化模型中的数据看上去像处于过去的某个时间点，要回答这个问题可能非常难，因为所有表都按自己的频率独立进行更改。

在这种情况下，最好的选择是创建一个视图并将 **AS OF** 子子句应用于整个视图。 利用此方法，你可以将数据访问层建模从时间点分析分离，因为 SQL Server 会将 **AS OF** 子句透明地应用于参与视图定义的所有临时表。 此外，你还可以将临时表和非临时表组合在同一个视图中， **AS OF** 将仅应用于临时表。 如果视图不引用任何临时表，那么，对其应用临时查询子句将失败并出现错误。

```sql
/* Create view that joins three temporal tables: Department, CompanyLocation, LocationDepartments */
CREATE VIEW [dbo].[vw_GetOrgChart]
AS
SELECT
    [CompanyLocation].LocID
   , [CompanyLocation].LocName
   , [CompanyLocation].City
   , [Department].DeptID
   , [Department].DeptName
FROM [dbo].[CompanyLocation]
LEFT JOIN [dbo].[LocationDepartments]
   ON [CompanyLocation].LocID = LocationDepartments.LocID
LEFT JOIN [dbo].[Department]
   ON LocationDepartments.DeptID = [Department].DeptID ;
GO
/* Querying view AS OF */
SELECT * FROM [vw_GetOrgChart]
FOR SYSTEM_TIME AS OF '2015-09-01 T10:00:00.7230011' ;
```

## <a name="query-for-changes-to-specific-rows-over-time"></a>查询一段时间内特定行的更改

如果想要执行数据审核，即，需要获取当前表中特定行的所有历史更改，临时子子句 **FROM...TO**, **BETWEEN...AND** 和 **CONTAINED IN** 将非常有用。

前两个子子句返回与指定时间段重叠的行版本（即，在给定时间段之前启动并在其之后结束的行版本），CONTAINED IN 则仅返回指定时间段边界内存在的行版本。

> [!IMPORTANT]
> 如果仅搜索非最新行版本，建议直接查询历史记录表，因为这样查询性能最佳。 如果需要在没有任何限制的情况下查询当前数据和历史数据，则使用 **ALL** 。

```sql
/* Query using BETWEEN...AND sub-clause*/
SELECT
     [DeptID]
   , [DeptName]
   , [SysStartTime]
   , [SysEndTime]
   , IIF (YEAR(SysEndTime) = 9999, 1, 0) AS IsActual
FROM [dbo].[Department]
FOR SYSTEM_TIME BETWEEN '2015-01-01' AND '2015-12-31'
WHERE DeptId = 1
ORDER BY SysStartTime DESC;

/* Query using CONTAINED IN sub-clause */
SELECT [DeptID], [DeptName], [SysStartTime],[SysEndTime]
FROM [dbo].[Department]
FOR SYSTEM_TIME CONTAINED IN ('2015-04-01', '2015-09-25')
WHERE DeptId = 1
ORDER BY SysStartTime DESC ;

/* Query using ALL sub-clause */
SELECT
     [DeptID]
   , [DeptName]
   , [SysStartTime]
   , [SysEndTime]
   , IIF (YEAR(SysEndTime) = 9999, 1, 0) AS IsActual
FROM [dbo].[Department] FOR SYSTEM_TIME ALL
ORDER BY [DeptID], [SysStartTime] Desc
```

## <a name="next-steps"></a>后续步骤

- [临时表](../../relational-databases/tables/temporal-tables.md)
- [FROM (Transact-SQL)](../../t-sql/queries/from-transact-sql.md)
- [创建由系统控制版本的临时表](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
- [在系统版本控制的临时表中修改数据](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [更改系统版本控制的临时表架构](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)
- [停止对系统版本的临时表的系统版本控制](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)
