---
description: 临时表使用方案
title: 临时表使用方案 | Microsoft Docs
ms.custom: ''
ms.date: 05/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 4b8fa2dd-1790-4289-8362-f11e6d63bb09
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28f1287ead4b3c84f5dcb65ec24564b9341d33f0
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384649"
---
# <a name="temporal-table-usage-scenarios"></a>时态表使用方案

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

临时表通常适用于需要跟踪数据更改历史记录的方案。 建议在以下用例中考虑使用时态表，以获得巨大的生产效率优势。

## <a name="data-audit"></a>数据审核

对存储关键信息的表使用临时系统版本控制，你需要跟踪对这些信息所做的更改和更改发生的时间，以及在任何时间点进行数据取证。

由系统控制版本的时态表允许你在开发周期的早期阶段规划数据审核方案，或者根据需要将数据审核添加到现有应用程序或解决方案。

下图显示了一个 Employee 表方案，其数据样本包括当前行版本（标记为蓝色）以及历史行版本（标记为灰色）。
图的右侧部分在时间轴上显示了行版本，以及在使用或不使用 SYSTEM_TIME 子句的情况下，你针对不同类型的基于临时表的查询选择了哪些行。

![显示第一个临时使用场景的示意图。](../../relational-databases/tables/media/temporalusagescenario1.png "TemporalUsageScenario1")

### <a name="enabling-system-versioning-on-a-new-table-for-data-audit"></a>对新表启用系统版本控制，以便进行数据审核

如果你确定了需要进行数据审核的信息，则请将数据库表创建为临时系统版本控制型的。 以下简单示例演示了在虚构 HR 数据库中包含员工信息的方案：

```sql
CREATE TABLE Employee
(
  [EmployeeID] int NOT NULL PRIMARY KEY CLUSTERED
  , [Name] nvarchar(100) NOT NULL
  , [Position] varchar(100) NOT NULL
  , [Department] varchar(100) NOT NULL
  , [Address] nvarchar(1024) NOT NULL
  , [AnnualSalary] decimal (10,2) NOT NULL
  , [ValidFrom] datetime2 (2) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (2) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )
 WITH (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.EmployeeHistory));
```

[创建由系统控制版本的时态表](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)中对创建由系统控制版本的时态表所需的各种选项进行了说明。

### <a name="enabling-system-versioning-on-an-existing-table-for-data-audit"></a>对现有表启用系统版本控制，以便进行数据审核

若需在现有数据库中执行数据审核，可使用 ALTER TABLE 扩展非临时表，使之成为系统版本控制型的。 为了避免在应用程序中进行中断性变更，可以 HIDDEN 方式添加时间段列，详见[将非时态表更改为由系统控制版本的时态表](./creating-a-system-versioned-temporal-table.md)中所述。 以下示例说明了如何在虚构 HR 数据库中针对现有 Employee 表启用系统版本控制。

```sql
/*
Turn ON system versioning in Employee table in two steps
(1) add new period columns (HIDDEN)
(2) create default history table
*/
ALTER TABLE Employee
ADD
    ValidFrom datetime2 (2) GENERATED ALWAYS AS ROW START HIDDEN
        constraint DF_ValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())  
    , ValidTo datetime2 (2) GENERATED ALWAYS AS ROW END HIDDEN
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);
  
ALTER TABLE Employee
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.Employee_History));
```

> [!IMPORTANT]
> Datetime2 数据类型的精度在源表和由系统控制版本的时态表中是相同的。

执行以上脚本以后，将会在历史记录表中以透明方式收集所有数据更改。 在典型的数据审核方案中，你需要查询在所关注时段内应用到单个行的所有数据更改。 可创建默认的历史记录表，使用聚合行存储 B 树来高效地解决这种用例问题。

### <a name="performing-data-analysis"></a>执行数据分析

使用上述方法之一启用系统版本控制以后，只需执行一个查询即可进行数据审核。 以下查询将搜索 EmployeeID = 1000 且在 2014 年 1 月 1 日至 2015 年 1 月 1 日的某段时间（包括上限）内保持活动状态的员工记录的行版本：

```sql
SELECT * FROM Employee
    FOR SYSTEM_TIME
      BETWEEN '2014-01-01 00:00:00.0000000' AND '2015-01-01 00:00:00.0000000'
        WHERE EmployeeID = 1000 ORDER BY ValidFrom;
```

将 FOR SYSTEM_TIME BETWEEN...AND 替换为 FOR SYSTEM_TIME ALL 即可分析针对该特定员工进行的数据更改的整个历史记录：

```sql
SELECT * FROM Employee
    FOR SYSTEM_TIME ALL WHERE
        EmployeeID = 1000 ORDER BY ValidFrom;
```

若要搜索仅在某个时间段内（不计该时间段外）处于活动状态的行版本，请使用 CONTAINED IN。 此查询非常高效，因为只查询历史记录表：

```sql
SELECT * FROM Employee FOR SYSTEM_TIME
    CONTAINED IN ('2014-01-01 00:00:00.0000000', '2015-01-01 00:00:00.0000000')
        WHERE EmployeeID = 1000 ORDER BY ValidFrom;
```

最后，在某些审核方案中，你可能需要了解整个表在过去任意时间点的情况：

```sql
SELECT * FROM Employee FOR SYSTEM_TIME AS OF '2014-01-01 00:00:00.0000000' ;
```

系统版本控制型临时表在存储时间段列的值时使用 UTC 时区，而筛选数据和显示结果时，使用本地时区总是更方便。 以下代码示例演示了如何应用筛选条件。该筛选条件最初使用本地时区指定，然后使用在 SQL Server 2016 中引入的 AT TIME ZONE 转换成了 UTC：

```sql
/*Add offset of the local time zone to current time*/
DECLARE @asOf DATETIMEOFFSET = GETDATE() AT TIME ZONE 'Pacific Standard Time'
/*Convert AS OF filter to UTC*/
SET @asOf = DATEADD (MONTH, -9, @asOf) AT TIME ZONE 'UTC';

SELECT
    EmployeeID
    , Name
    , Position
    , Department
    , [Address]
    , [AnnualSalary]
    , ValidFrom AT TIME ZONE 'Pacific Standard Time' AS ValidFromPT
    , ValidTo AT TIME ZONE 'Pacific Standard Time' AS ValidToPT
FROM Employee
    FOR SYSTEM_TIME AS OF @asOf where EmployeeId = 1000
```

所有其他使用系统版本控制型表的方案均可使用 AT TIME ZONE。

> [!TIP]
> 在带有 FOR SYSTEM_TIME 的临时子句中指定的筛选条件为 SARG 型（即 SQL Server 可以利用基础聚集索引以执行搜索而不是扫描操作。
> 如果直接查询历史记录表，请确保筛选条件也是 SARG 型的，即在指定筛选器时采用 \<period column> > {< | > | =, …} date_condition AT TIME ZONE 'UTC' 格式。
> 如果将 AT TIME ZONE 应用到时间段列，SQL Server 会执行开销可能很大的表/索引扫描。 在查询中要避免这种类型的条件：\<period column> AT TIME ZONE '\<your time zone>' > {< | > | =, ...} 的形式 date_condition。

另请参阅：[在系统版本控制时态表中查询数据](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)。

## <a name="point-in-time-analysis-time-travel"></a>时间点分析（按时间顺序查看）

在按时间顺序查看方案中，用户需要了解整个数据集在一定时段内的变化情况，这与关注点通常在单个记录的更改的数据审核不一样。 有时候，时程包括多个相关的临时表，每个表的变化模式都是独立的，而这也是你需要进行针对性分析的：

- 历史数据和当前数据中重要指标所指示的趋势
- “AS OF”子句所揭示的全部数据在过去任意时间点（昨天、一月前，等等）的精确快照
- 两个所关注时间点（例如，一月前和三月前）之间的差异

许多实际方案都需要时程分析。 为了说明此类使用方案，让我们看看带有自动生成的历史记录的 OLTP。

### <a name="oltp-with-auto-generated-data-history"></a>带有自动生成的数据历史记录的 OLTP

在事务处理系统中，经常需分析重要度量值在一定时段内的变化情况。 理想情况下，分析历史记录不会损害 OLTP 应用程序的性能，但前提是访问数据的最新状态时，必须尽量降低延迟并进行数据锁定。 设计系统版本控制型临时表的目的是让用户能够以透明方式保留所做更改的完整历史记录，以便以后进行分析。这些历史记录独立于当前数据，将对主 OLTP 工作负荷的影响降到最低。

对于需要进行高强度事务处理的工作负荷，建议你使用[由系统控制版本的时态表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)，使你能够以具有成本效益的方法将当前数据存储在内存中，并且将所做更改的完整历史记录存储在磁盘上。

对于历史记录表，建议你使用聚集列存储索引，原因如下：

- 聚集列存储索引可提高查询性能，这有益于典型的趋势分析。
- 在 OLTP 工作负荷很重的情况下，如果历史记录表使用聚集列存储索引，则内存优化表的数据刷新任务的执行效果最佳。
- 聚集列存储索引的压缩效果很好，尤其是在并非所有列的更改都同时发生的情况下。

将临时表与内存中 OLTP 一起使用，则不需总是将整个数据集保留在内存中，因此可轻松地辨识热数据和冷数据。

现实中此类情况的示例包括库存管理、货币贸易等。

下图显示了用于库存管理的简化数据模型：

![显示用于库存管理的简化数据模型的示意图。](../../relational-databases/tables/media/temporalusageinmemory.png "TemporalUsageInMemory")

以下代码示例创建 ProductInventory 作为内存中系统版本控制型临时表，所使用的聚集列存储索引基于历史记录表（此表实际上是替换了默认创建的行存储索引）：

> [!NOTE]
> 请确保你的数据库允许创建内存优化表。 请参阅 [创建内存优化表和本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)。

```sql
USE TemporalProductInventory
GO

BEGIN
    --If table is system-versioned, SYSTEM_VERSIONING must be set to OFF first
    IF ((SELECT temporal_type FROM SYS.TABLES WHERE object_id = OBJECT_ID('dbo.ProductInventory', 'U')) = 2)
    BEGIN
        ALTER TABLE [dbo].[ProductInventory] SET (SYSTEM_VERSIONING = OFF)
    END
    DROP TABLE IF EXISTS [dbo].[ProductInventory]
       DROP TABLE IF EXISTS [dbo].[ProductInventoryHistory]
END
GO

CREATE TABLE [dbo].[ProductInventory]
(
    ProductId int NOT NULL,
    LocationID INT NOT NULL,
    Quantity int NOT NULL CHECK (Quantity >=0),
  
    SysStartTime datetime2 GENERATED ALWAYS AS ROW START NOT NULL ,
    SysEndTime datetime2 GENERATED ALWAYS AS ROW END NOT NULL ,
    PERIOD FOR SYSTEM_TIME(SysStartTime,SysEndTime),

    --Primary key definition
    CONSTRAINT PK_ProductInventory PRIMARY KEY NONCLUSTERED (ProductId, LocationId)
)
WITH
(
    MEMORY_OPTIMIZED=ON,
    SYSTEM_VERSIONING = ON
    (
        HISTORY_TABLE = [dbo].[ProductInventoryHistory],
        DATA_CONSISTENCY_CHECK = ON
    )
)

CREATE CLUSTERED COLUMNSTORE INDEX IX_ProductInventoryHistory ON [ProductInventoryHistory]
WITH (DROP_EXISTING = ON);
```

对于上述模型来说，下面是库存维护过程的具体内容：

```sql
CREATE PROCEDURE [dbo].[spUpdateInventory]
@productId int,
@locationId int,
@quantityIncrement int

WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT, LANGUAGE=N'English')
    UPDATE dbo.ProductInventory
        SET Quantity = Quantity + @quantityIncrement
            WHERE ProductId = @productId AND LocationId = @locationId

/*If zero rows were updated than this is insert of the new product for a given location*/
    IF @@rowcount = 0
        BEGIN
            IF @quantityIncrement < 0
                SET @quantityIncrement = 0
            INSERT INTO [dbo].[ProductInventory]
                (
                    [ProductId]
                    ,[LocationID]
                    ,[Quantity]
                )
                VALUES
                   (
                        @productId
                       ,@locationId
                       ,@quantityIncrement
                   )
        END
END;
```

spUpdateInventory 存储过程可以将新产品插入库存中，也可以更新特定位置的产品数量。 业务逻辑很简单，其注重点是通过表更新对 Quantity 字段进行递增/递减操作，确保最新状态始终准确，而由系统控制版本的表则通过透明方式将历史记录维度添加到数据中，如下图所示。

![显示临时使用情况的示意图，当前使用情况在内存中，历史使用情况在聚集列存储中。](../../relational-databases/tables/media/temporalusageinmemory2b.png "TemporalUsageInMemory2b")

现在，通过本机编译模块即可高效地执行最新状态查询：

```sql
CREATE PROCEDURE [dbo].[spQueryInventoryLatestState]
WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT, LANGUAGE=N'English')
    SELECT ProductId, LocationID, Quantity, SysStartTime
        FROM dbo.ProductInventory
    ORDER BY ProductId, LocationId
END;
GO
EXEC [dbo].[spQueryInventoryLatestState];
```

使用 FOR SYSTEM_TIME ALL 子句进行一定时段内的数据更改分析变得相当容易，如以下示例所示：

```sql
DROP VIEW IF EXISTS vw_GetProductInventoryHistory;
GO
CREATE VIEW vw_GetProductInventoryHistory
AS
    SELECT ProductId, LocationId, Quantity, SysStartTime, SysEndTime
    FROM [dbo].[ProductInventory]
        FOR SYSTEM_TIME ALL;
GO
SELECT * FROM vw_GetProductInventoryHistory
    WHERE ProductId = 2;
```

下图显示了一个产品的数据历史记录，该记录可以通过将上述视图导入 Power Query、Power BI 或类似的商业智能工具来轻松地呈现：

![显示一种产品的数据历史记录的示意图。](../../relational-databases/tables/media/producthistoryovertime.png "ProductHistoryOverTime")

这种情况下可以使用临时表进行其他类型的时程分析，例如通过 AS OF 子句重新构造库存在过去任意时间点的状态，或者对属于不同时刻的快照进行比较。

就这种使用方案来说，你还可以将 Product 表和 Location 表扩展成为时态表，以便以后对 UnitPrice 和 NumberOfEmployee 的更改历史记录进行分析。

```sql
ALTER TABLE Product
ADD
    SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN
        constraint DF_ValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())
    , SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN
        constraint DF_ValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);

ALTER TABLE Product
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.ProductHistory));

ALTER TABLE [Location]
ADD
    SysStartTime datetime2 GENERATED ALWAYS AS ROW START HIDDEN
        constraint DFValidFrom DEFAULT DATEADD(second, -1, SYSUTCDATETIME())
    , SysEndTime datetime2 GENERATED ALWAYS AS ROW END HIDDEN
        constraint DFValidTo DEFAULT '9999.12.31 23:59:59.99'
    , PERIOD FOR SYSTEM_TIME (SysStartTime, SysEndTime);

ALTER TABLE [Location]
    SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.LocationHistory));
```

由于数据模型现在涉及多个临时表，因此进行 AS OF 分析时最好是创建一个可从相关表中提取必需数据的视图，然后将 FOR SYSTEM_TIME AS OF 应用到该视图，这样可以大大简化完整数据模型状态的重新构造过程：

```sql
DROP VIEW IF EXISTS vw_ProductInventoryDetails;
GO

CREATE VIEW vw_ProductInventoryDetails
AS
    SELECT PrInv.ProductId ,PrInv.LocationId, P.ProductName, L.LocationName, PrInv.Quantity
    , P.UnitPrice, L.NumberOfEmployees
    , P.SysStartTime AS ProductStartTime, P.SysEndTime AS ProductEndTime
    , L.SysStartTime AS LocationStartTime, L.SysEndTime AS LocationEndTime
    , PrInv.SysStartTime AS InventoryStartTime, PrInv.SysEndTime AS InventoryEndTime
FROM dbo.ProductInventory as PrInv
JOIN dbo.Product AS P ON PrInv.ProductId = P.ProductID
JOIN dbo.Location AS L ON PrInv.LocationId = L.LocationID;
GO
SELECT * FROM vw_ProductInventoryDetails
    FOR SYSTEM_TIME AS OF '2015.01.01';
```

下图显示了为 SELECT 查询生成的执行计划。 这表明，在处理临时关系时，不管过程怎么复杂，都可以通过 SQL Server 引擎来妥善处理：

![显示为 SELECT 查询生成的执行计划的示意图，其中说明了处理临时关系所面临的各种复杂性由 SQL Server 引擎完全处理。](../../relational-databases/tables/media/asofexecutionplan.png "ASOFExecutionPlan")

使用以下代码来比较两个时间点（一天前和一月前）的产品库存状态：

```sql
DECLARE @dayAgo datetime2 = DATEADD (day, -1, SYSUTCDATETIME());
DECLARE @monthAgo datetime2 = DATEADD (month, -1, SYSUTCDATETIME());

SELECT
    inventoryDayAgo.ProductId
    , inventoryDayAgo.ProductName
    , inventoryDayAgo.LocationName
    , inventoryDayAgo.Quantity AS QuantityDayAgo,inventoryMonthAgo.Quantity AS QuantityMonthAgo
    , inventoryDayAgo.UnitPrice AS UnitPriceDayAgo, inventoryMonthAgo.UnitPrice AS UnitPriceMonthAgo
FROM vw_ProductInventoryDetails
FOR SYSTEM_TIME AS OF @dayAgo AS inventoryDayAgo
JOIN vw_ProductInventoryDetails FOR SYSTEM_TIME AS OF @monthAgo AS inventoryMonthAgo
    ON inventoryDayAgo.ProductId = inventoryMonthAgo.ProductId AND inventoryDayAgo.LocationId = inventoryMonthAgo.LocationID;
```

## <a name="anomaly-detection"></a>异常检测

异常检测（或离群值检测）是指确定哪些项目不符合期望的模式或者不同于数据集中的其他项目。 可以使用系统版本控制型临时表来检测定期或不定期发生的异常，因为你可以利用临时查询来快速查找特定模式。 具体异常取决于所收集数据的类型以及业务逻辑。

以下示例显示了在销售数字中检测“峰值”所需的简化逻辑。 假定你要处理一个临时表，该表收集所购产品的历史记录：

```sql
CREATE TABLE [dbo].[Product]
                (
            [ProdID] [int] NOT NULL PRIMARY KEY CLUSTERED
        , [ProductName] [varchar](100) NOT NULL
        , [DailySales] INT NOT NULL
        , [ValidFrom] [datetime2] GENERATED ALWAYS AS ROW START NOT NULL
        , [ValidTo] [datetime2] GENERATED ALWAYS AS ROW END NOT NULL
        , PERIOD FOR SYSTEM_TIME ([ValidFrom], [ValidTo])
    )
    WITH( SYSTEM_VERSIONING = ON (HISTORY_TABLE = [dbo].[ProductHistory]
        , DATA_CONSISTENCY_CHECK = ON ))
```

下图显示了一定时段内的购买项目：

![显示一段时间内的购买项目的示意图。](../../relational-databases/tables/media/temporalanomalydetection.png "TemporalAnomalyDetection")

假定购买产品的数目在平常日子有一些小的差异，则可使用以下查询来确定单一离群值。单一离群值指的是与最近的邻居相比具有显著差异（2 倍），但与周围的示例相比没有显著差异（不到 20%）的示例：

```sql
WITH CTE (ProdId, PrevValue, CurrentValue, NextValue, ValidFrom, ValidTo)
AS
    (
        SELECT
            ProdId, LAG (DailySales, 1, 1) over (partition by ProdId order by ValidFrom) as PrevValue
            , DailySales, LEAD (DailySales, 1, 1) over (partition by ProdId order by ValidFrom) as NextValue
             , ValidFrom, ValidTo from Product
        FOR SYSTEM_TIME ALL
)

SELECT
    ProdId
    , PrevValue
    , CurrentValue
    , NextValue
    , ValidFrom
    , ValidTo
    , ABS (PrevValue - NextValue) / convert (float, (CASE WHEN NextValue > PrevValue THEN PrevValue ELSE NextValue END)) as PrevToNextDiff
    , ABS (CurrentValue - PrevValue) / convert (float, (CASE WHEN CurrentValue > PrevValue THEN PrevValue ELSE CurrentValue END)) as CurrentToPrevDiff
    , ABS (CurrentValue - NextValue) / convert (float, (CASE WHEN CurrentValue > NextValue THEN NextValue ELSE CurrentValue END)) as CurrentToNextDiff
FROM CTE
    WHERE
        ABS (PrevValue - NextValue) / (CASE WHEN NextValue > PrevValue THEN PrevValue ELSE NextValue END) < 0.2
            AND ABS (CurrentValue - PrevValue) / (CASE WHEN CurrentValue > PrevValue THEN PrevValue ELSE CurrentValue END) > 2
            AND ABS (CurrentValue - NextValue) / (CASE WHEN CurrentValue > NextValue THEN NextValue ELSE CurrentValue END) > 2;
```

> [!NOTE]
> 此示例已特意简化。 在生产方案中，你可能会使用高级统计方法来确定不遵循通用模式的样本。

## <a name="slowly-changing-dimensions"></a>缓慢变化的维度

数据仓库中的维度通常包含相对静态的与以下实体相关的数据：地理位置、客户或产品。 不过，某些方案会要求你也跟踪数据在维度表中的更改。 由于维度修改的发生频率要小得多、修改方式不可预测，并且超出常规的事实数据表更新计划，因此这些类型的维度表称为缓慢变化的维度 (SCD)。

根据更改历史记录的保留方式，可以将渐变维度分为多个类别：

- 类型 0：不保留历史记录。 维度属性反映原始值。
- 类型 1：维度属性反映最新值（覆盖以前的值）
- 类型 2：每个版本的维度成员在表中使用不同的行表示，通常使用列表示有效期
- 类型 3：在同一行中使用其他列保留所选属性的有限历史记录
- 类型 4：在单独的表中保留历史记录，而原始维度表则保留最新（当前）的维度成员版本

选择 SCD 策略时，由 ETL（提取-转换-加载）层确保维度表的准确性，通常这需要编写大量的代码并完成复杂的维护工作。

使用 SQL Server 2016 中的系统版本控制型临时表可以大幅降低代码的复杂性，因为会自动保留数据的历史记录。 由于是使用两个表来实现的，SQL Server 2016 中的时态表最接近于类型 4 SCD。 但是，由于临时查询只允许你引用当前表，因此也可以考虑在计划使用类型 2 SCD 的环境中使用临时表。

要将常规维度转换为 SCD，可以直接创建一个新表，也可以将现有表更改为由系统控制版本的时态表。 如果现有维度表包含历史数据，可创建一个单独的表，将历史数据移到其中，并将当前（实际）的维度版本保留在原始维度表中。 然后，使用 ALTER TABLE 语法通过预定义历史记录表将维度表转换为系统版本控制型临时表。

以下示例演示了该过程，并假定 DimLocation 维度表已经有 ValidFrom 和 ValidTo 作为不可为 null 的 datetime2 列，并通过 ETL 过程对这些列进行了填充：

```sql
/*Move "closed" row versions into newly created history table*/
SELECT * INTO DimLocationHistory
    FROM DimLocation
        WHERE ValidTo < '9999-12-31 23:59:59.99';
GO
/*Create clustered columnstore index which is a very good choice in DW scenarios*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_DimLocationHistory ON DimLocationHistory
/*Delete previous versions from DimLocation which will become current table in temporal-system-versioning configuration*/
DELETE FROM DimLocation
    WHERE ValidTo < '9999-12-31 23:59:59.99';
/*Add period definition*/
ALTER TABLE DimLocation ADD PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo);
/*Enable system-versioning and bind history table to the DimLocation*/
ALTER TABLE DimLocation SET (SYSTEM_VERSIONING = ON (HISTORY_TABLE = dbo.DimLocationHistory));
```

创建以后，就不需要在数据仓库加载过程中使用额外的代码来维护 SCD。

下图演示了如何在涉及 2 个 SCD（DimLocation 和 DimProduct）和 1 个事实数据表的简单方案中使用临时表。

![一张示意图，显示如何在涉及 2 个 SCD（DimLocation 和 DimProduct）和 1 个事实数据表的简单方案中使用临时表。](../../relational-databases/tables/media/temporalscd.png "TemporalSCD")

若要在报表中使用上述 SCD，需对查询进行有效调整。 例如，你可能需要计算过去六个月的总销售额和人均销售产品数。 请注意，这两个指标都需要对事实数据表和维度中的数据进行更正，这些数据可能更改了对分析来说很重要的属性（DimLocation.NumOfCustomers、DimProduct.UnitPrice）。 以下查询对所需指标进行了正确的计算：

```sql
DECLARE @now datetime2 = SYSUTCDATETIME()
DECLARE @sixMonthsAgo datetime2 SET
    @sixMonthsAgo = DATEADD (month, -12, SYSUTCDATETIME())

SELECT DimProduct_History.ProductId
   , DimLocation_History.LocationId
    , SUM(F.Quantity * DimProduct_History.UnitPrice) AS TotalAmount
    , AVG (F.Quantity/DimLocation_History.NumOfCustomers) AS AverageProductsPerCapita
FROM FactProductSales F
/* find corresponding record in SCD history in last 6 months, based on matching fact */
JOIN DimLocation FOR SYSTEM_TIME BETWEEN @sixMonthsAgo AND @now AS DimLocation_History
    ON DimLocation_History.LocationId = F.LocationId
        AND F.FactDate BETWEEN DimLocation_History.ValidFrom AND DimLocation_History.ValidTo
/* find corresponding record in SCD history in last 6 months, based on matching fact */
JOIN DimProduct FOR SYSTEM_TIME BETWEEN @sixMonthsAgo AND @now AS DimProduct_History
    ON DimProduct_History.ProductId = F.ProductId
        AND F.FactDate BETWEEN DimProduct_History.ValidFrom AND DimProduct_History.ValidTo
    WHERE F.FactDate BETWEEN @sixMonthsAgo AND @now
GROUP BY DimProduct_History.ProductId, DimLocation_History.LocationId ;
```

**注意事项：**

- 对 SCD 使用由系统控制版本的时态表是可以接受的，前提是根据数据库事务时间计算的有效期适用于你的业务逻辑。 如果加载数据时的延迟很严重，则事务时间可能是不可接受的。
- 默认情况下，系统版本控制型临时表不允许在加载后更改历史数据（将 SYSTEM_VERSIONING 设置为 OFF 后可以修改历史记录）。 在频繁更改历史数据的情况下，这可能导致功能受限。
- 只要列发生更改，系统版本控制型临时表就会生成行版本。 如果你想要在进行特定的列更改时禁止显示新版本，则需在 ETL 逻辑中纳入该限制。
- 如果你预计 SCD 表中会有大量的历史行，则可考虑使用聚集列存储索引作为历史记录表的主要存储选项。 这样会减少历史记录表占用的空间，加快分析查询速度。

## <a name="repairing-row-level-data-corruption"></a>修复行级数据损坏

你可以依赖由系统控制版本的时态表中的历史数据，将各个行快速修复到以前捕获的任何状态。 如果能够找到受影响的行并且/或者知道在何时进行了不需要的数据更改，则可利用临时表的这种属性来高效地执行修复，不需使用备份。

此方法有多种优点：

- 你可以非常精确地控制修复范围。 不受影响的记录需保持最新状态，这通常是一项很关键的要求。
- 操作很高效，数据库会保持联机状态，以便所有工作负荷使用数据。
- 修复操作本身也会进行版本控制。 你有修复操作本身的审核记录，因此可以在以后根据需要分析所发生的情况。

可以相对轻松地自动执行修复操作。 下面是针对数据审核方案中使用的 Employee 表执行数据修复的存储过程的代码示例。

```sql
DROP PROCEDURE IF EXISTS sp_RepairEmployeeRecord;
GO

CREATE PROCEDURE sp_RepairEmployeeRecord
    @EmployeeID INT,
    @versionNumber INT = 1
AS

;WITH History
AS
(
        /* Order historical rows by tehir age in DESC order*/
        SELECT ROW_NUMBER () OVER (PARTITION BY EmployeeID ORDER BY [ValidTo] DESC) AS RN, *
        FROM Employee FOR SYSTEM_TIME ALL WHERE YEAR (ValidTo) < 9999 AND Employee.EmployeeID = @EmployeeID
)

/*Update current row by using N-th row version from history (default is 1 - i.e. last version)*/
UPDATE Employee
    SET [Position] = H.[Position], [Department] = H.Department, [Address] = H.[Address], AnnualSalary = H.AnnualSalary
    FROM Employee E JOIN History H ON E.EmployeeID = H.EmployeeID AND RN = @versionNumber
    WHERE E.EmployeeID = @EmployeeID
```

此存储过程采用 @EmployeeID 和 @versionNumber 作为输入参数。 默认情况下，此过程将行状态还原到历史记录中的最后一个版本 (@versionNumber = 1)。

下图显示了过程调用前后的行状态。 红色矩形标记的是不正确的当前行版本，绿色矩形标记的是历史记录中的正确版本。

![显示过程调用前后的行状态的屏幕截图](../../relational-databases/tables/media/temporalusagerepair1.png "TemporalUsageRepair1")

```sql
EXEC sp_RepairEmployeeRecord @EmployeeID = 1, @versionNumber = 1
```

![显示所选行的屏幕截图。](../../relational-databases/tables/media/temporalusagerepair2.png "TemporalUsageRepair2")

可以将该修复存储过程定义为接受具体的时间戳而非行版本。 该过程会将行还原为在所提供的时间点（即 AS OF 时间点）处于活动状态的任何版本。

```sql
DROP PROCEDURE IF EXISTS sp_RepairEmployeeRecordAsOf;
GO

CREATE PROCEDURE sp_RepairEmployeeRecordAsOf
    @EmployeeID INT,
    @asOf datetime2
AS

/*Update current row to the state that was actual AS OF provided date*/
UPDATE Employee
    SET [Position] = History.[Position], [Department] = History.Department, [Address] = History.[Address], AnnualSalary = History.AnnualSalary
    FROM Employee AS E JOIN Employee FOR SYSTEM_TIME AS OF @asOf AS History ON E.EmployeeID = History.EmployeeID
    WHERE E.EmployeeID = @EmployeeID
```

对于同一数据样本，下图显示的是带有时间条件的修复方案。 突出显示的是 @asOf 参数、历史记录中选择的所提供时间点的实际行，以及进行修复操作后当前表中的全新行版本：

![显示带有时间条件的修复方案的屏幕截图。](../../relational-databases/tables/media/temporalusagerepair3.png "TemporalUsageRepair3")

在数据仓库和报表系统中自动加载数据时，可能会进行数据更正。 很多情况下，如果刚更新的值不正确，从历史记录中还原以前的版本即可。 下图显示了自动执行此过程的方法：

![显示可如何自动执行此过程的示意图。](../../relational-databases/tables/media/temporalusagerepair4.png "TemporalUsageRepair4")

## <a name="next-steps"></a>后续步骤

- [时态表](../../relational-databases/tables/temporal-tables.md)
 [由系统控制版本的时态表入门](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [临时表系统一致性检查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [临时表分区](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [临时表注意事项和限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [临时表安全性](../../relational-databases/tables/temporal-table-security.md)
- [系统版本控制临时表与内存优化表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [临时表元数据视图和函数](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)