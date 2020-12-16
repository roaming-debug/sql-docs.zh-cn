---
description: 在系统版本控制的临时表中修改数据
title: 在系统版本控制的临时表中修改数据 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 5f398470-c531-47b5-84d5-7c67c27df6e5
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7acb5296f5cfcefd5c39c9ceb643a1076c11bfbd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484469"
---
# <a name="modifying-data-in-a-system-versioned-temporal-table"></a>修改由系统控制版本的时态表中的数据


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


系统版本控制的临时表中的数据使用常规的 DML 语句进行修改，但有一个重要的区别：无法直接修改时间段列数据。 当数据更新时，它就是版本控制的，每个已更新行的以前版本都将插入到历史记录表中。 当删除数据时，删除是逻辑的，行将从当前表格移动到历史记录表中 - 不会被永久删除。

## <a name="inserting-data"></a>插入数据

当插入新数据时，如果它们不是 **HIDDEN** ，你需要对 **PERIOD** 列作出说明。 你还可以对系统版本控制的临时表使用分区切换。

### <a name="insert-new-data-with-visible-period-columns"></a>使用可见的时间段列插入新数据

当你有如下可见的 **PERIOD** 列用来说明新的 **PERIOD** 列时，则可以构建你的 **INSERT** 语句：

- 如果你在 **INSERT** 语句中指定列列表，则可以省略 **PERIOD** 列，因为系统将为这些列自动生成值。

  ```sql
  -- Insert with column list and without period columns
  INSERT INTO [dbo].[Department]
    (  [DeptID]
          , [DeptName]
          , [ManagerID]
          ,[ParentDeptID]
    )
       VALUES
         (  10
         , 'Marketing'
         , 101
         , 1
         ) ;
   ```

- 如果你确实在 INSERT 语句中的列列表中指定了 PERIOD 列，则需要指定 DEFAULT 作为它们的值。

  ```sql
  INSERT INTO [dbo].[Department]
    (  [DeptID]
          , [DeptName]
          , [ManagerID]
          , [ParentDeptID]
          , SysStartTime
          , SysEndTime
    )
       VALUES
         (  11
          , 'Sales'
          , 101
          , 1
          , default
          , default) ;
   ```

- 如果你未在 **INSERT** 语句中指定列列表，则为 **PERIOD** 列指定 **DEFAULT** 。

  ```sql
    -- Insert without column list and DEFAULT values for period columns
    INSERT INTO [dbo].[Department]
    VALUES(12, 'Production', 101, 1, default, default);

  ```

### <a name="insert-data-into-a-table-with-hidden-period-columns"></a>将数据插入到具有 HIDDEN 时间段列的表中

如果 **PERIOD** 列被指定为 HIDDEN，那么当你使用 INSERT 且没有指定列列表时，你仅需要为可见的列指定值。 不需要对你的 **INSERT** 语句中的新的 **PERIOD** 列作出说明。 此行为保证当你对从版本控制中受益的表启用系统版本控制时，你的旧版应用程序将继续工作。

```sql
CREATE TABLE [dbo].[CompanyLocation]
(
     [LocID] [int] IDENTITY(1,1) NOT NULL PRIMARY KEY
   , [LocName] [varchar](50) NOT NULL
   , [City] [varchar](50) NOT NULL
   , [SysStartTime] [datetime2] GENERATED ALWAYS AS ROW START HIDDEN NOT NULL
   , [SysEndTime] [datetime2] GENERATED ALWAYS AS ROW END HIDDEN NOT NULL
   , PERIOD FOR SYSTEM_TIME ([SysStartTime], [SysEndTime])
)
WITH ( SYSTEM_VERSIONING = ON );
GO
INSERT INTO [dbo].[CompanyLocation]
VALUES ('Headquarters', 'New York');

```

### <a name="inserting-data-using-partition-switch"></a>使用 PARTITION SWITCH 插入数据

如果当前表已分区，可以使用分区切换作为将数据加载到空分区或并行加载到多个分区的有效机制。

在 **PARTITION SWITCH IN** 语句中与系统版本控制的临时表配合使用的暂存表必须定义了 **SYSTEM_TIME PERIOD** ，但它不需要成为系统版本控制的临时表。
这可确保在数据插入到暂存表期间或将 SYSTEM_TIME 时间段添加到预填充的暂存表时执行了临时的一致性检查。

```sql
/*Create staging table with period definition for SWITCH IN temporal table*/
CREATE TABLE [dbo].[Staging_Department_Partition2]
(
     [DeptID] [int] NOT NULL
   , [DeptName] [varchar](50) NOT NULL
   , [ManagerID] [int] NULL
   , [ParentDeptID] [int] NULL
   , [SysStartTime] [datetime2] GENERATED ALWAYS AS ROW START NOT NULL
   , [SysEndTime] [datetime2] GENERATED ALWAYS AS ROW END NOT NULL
   , PERIOD FOR SYSTEM_TIME ( [SysStartTime], [SysEndTime] )
) ON [PRIMARY]

/*Create aligned primary key*/
ALTER TABLE [dbo].[Staging_Department_Partition2]
ADD CONSTRAINT [Staging_Department_Partition2_PK]
   PRIMARY KEY CLUSTERED
   ( [DeptID] ASC )
   ON [PRIMARY]
  
/*
Create and enforce constraints for partition boundaries.
Partition 2 contains rows with DeptID > 100 and DeptID <=200
*/
ALTER TABLE [dbo].[Staging_Department_Partition2]
   WITH CHECK ADD  CONSTRAINT [chk_staging_Department_partition_2]
   CHECK  ([DeptID]>N'100' AND [DeptID]<=N'200')
ALTER TABLE [dbo].[Staging_Department_Partition2]
   CHECK CONSTRAINT [chk_staging_Department_partition_2]

/*Load data into staging table*/
INSERT INTO [dbo].[staging_Department] ([DeptID],[DeptName],[ManagerID],[ParentDeptID])
VALUES (101,'D101',1,NULL)

/*Use PARTITION SWITCH IN to efficiently add data to current table */
ALTER TABLE [Staging_Department]
SWITCH TO [dbo].[Department] PARTITION 2;
```

如果你尝试在没有时间段定义的情况下从表中执行 PARTITION SWITCH，将收到错误消息：`Msg 13577, Level 16, State 1, Line 25 ALTER TABLE SWITCH statement failed on table 'MyDB.dbo.Staging_Department_2015_09_26' because target table has SYSTEM_TIME PERIOD while source table does not have it.`

## <a name="updating-data"></a>更新数据

用常规的 **UPDATE** 语句在当前表中更新数据。 可以针对小的意外根据历史记录表来更新当前表中的数据。  但是，无法更新 PERIOD 列且当 SYSTEM_VERSIONING = ON 时不能直接在历史记录表中更新数据。

设置 **SYSTEM_VERSIONING = OFF** 并从当前和历史记录表中更新行，但请记住，这样的话系统将不会保留更改的历史记录。

### <a name="updating-the-current-table"></a>更新当前表

在此示例中，对 DeptID = 10 的每一行都将更新 ManagerID 列。 **PERIOD** 列不会以任何方式引用。

```sql
UPDATE [dbo].[Department] SET [ManagerID] = 501 WHERE [DeptID] = 10
```

但是，你不能更新 **PERIOD** 列且不能更新历史记录表。 在此示例中，尝试更新 **PERIOD** 列会生成错误。

```sql
UPDATE [dbo].[Department]
SET SysStartTime = '2015-09-23 23:48:31.2990175'
WHERE DeptID = 10 ;

Msg 13537, Level 16, State 1, Line 3
Cannot update GENERATED ALWAYS columns in table 'TmpDev.dbo.Department'.
```

### <a name="updating-the-current-table-from-the-history-table"></a>从历史记录表更新当前表

可以对当前表使用 UPDATE 以便在过去的特定时间点将实际行状态恢复为有效状态（恢复为“上次已知完好的行版本”）。 下面的示例显示了恢复为截至 2015-04-25 的历史记录表中的值，其中的 DeptID = 10。

```sql
UPDATE Department
SET DeptName = History.DeptName
FROM Department
FOR SYSTEM_TIME AS OF '2015-04-25' AS History
WHERE History.DeptID = 10
AND Department.DeptID = 10 ;
```

## <a name="deleting-data"></a>删除数据

用常规的 **DELETE** 语句在当前表中删除数据。 已删除行的结束时间段列将填充底层事物的开始时间。 当 **SYSTEM_VERSIONING = ON** 时不能直接从历史记录表删除行。 设置 **SYSTEM_VERSIONING = OFF** 并从当前和历史记录表中删除行，但请记住，这样的话系统将不会保留更改的历史记录。 **SYSTEM_VERSIONING = ON** 时，不支当前表的 **TRUNCATE** 和 **SWITCH PARTITION OUT** ，以及历史记录表的 **SWITCH PARTITION IN**。

## <a name="using-merge-to-modify-data-in-temporal-table"></a>使用 MERGE 在临时表中修改数据

支持 **MERGE** 操作，有关 **INSERT** 列方面的限制与 **INSERT** 和 **UPDATE** 语句的相同。

```sql
CREATE TABLE DepartmentStaging (DeptId INT, DeptName varchar(50));
GO
INSERT INTO DepartmentStaging VALUES (1, 'Company Management');
INSERT INTO DepartmentStaging VALUES (10, 'Science & Research');
INSERT INTO DepartmentStaging VALUES (15, 'Process Management');

MERGE dbo.Department AS target
USING (SELECT DeptId, DeptName FROM DepartmentStaging) AS source (DeptId, DeptName)
ON (target.DeptId = source.DeptId)
WHEN MATCHED THEN
    UPDATE
   SET DeptName = source.DeptName
WHEN NOT MATCHED THEN
   INSERT (DeptName)
   VALUES (source.DeptName);
```

## <a name="next-steps"></a>后续步骤

- [临时表](../../relational-databases/tables/temporal-tables.md)
- [创建由系统控制版本的临时表](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
- [在系统版本控制临时表中查询数据](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [更改系统版本控制的临时表架构](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)
- [停止对系统版本的临时表的系统版本控制](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)
