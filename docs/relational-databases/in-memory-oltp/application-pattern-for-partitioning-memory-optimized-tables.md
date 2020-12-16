---
title: 应用程序模式 - 内存优化表分区
description: 了解内存中 OLTP 应用程序设计模式，该模式将当前的活动数据存储在内存优化表中，将旧数据存储在已分区表中。
ms.custom: seo-dt-2019,issue-PR=4700-14820
ms.date: 05/03/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 3f867763-a8e6-413a-b015-20e9672cc4d1
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8ff253b9164ca12d8d24dd5f7b362f4dfb47d548
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465438"
---
# <a name="application-pattern-for-partitioning-memory-optimized-tables"></a>用于对内存优化表进行分区的应用程序模式

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[hek_2](../../includes/hek-2-md.md)] 支持的应用程序设计模式是将性能资源消耗在相对较新的数据上。 当读取或更新当前数据的频率远高于旧数据时，可以应用此模式。 在这种情况下，我们说当前数据是活动的或热的，较旧的数据是冷的。

主要意图是将热数据存储在内存优化表中。 按每周或每月的频率将不太常用的旧数据移动到已分区表中。 已分区表将其数据存储在磁盘或其他硬盘驱动器上，而不是存储在内存中。

通常情况下，这种设计使用一个日期时间键，使移动过程能够有效地区分冷热数据。

## <a name="advanced-partitioning"></a>高级分区

该设计旨在模拟还具有一个内存优化分区的已分区表。 为了使这种设计有效，必须确保所有表都有一个共同的架构。 本文后面的代码示例演示了此方法。

根据定义，假定新数据是热的。 在内存优化表中插入热数据并更新。 冷数据是在传统的已分区表中维护的。 存储过程将定期添加新分区。 分区包含已从内存优化表中移出的最新冷数据。

如果一个操作只需要热数据，可以使用本机编译的存储过程来访问数据。 可能访问热数据或冷数据的操作必须使用经过解释的 [!INCLUDE[tsql](../../includes/tsql-md.md)]，将内存优化表与已分区表联接在一起。

### <a name="add-a-partition"></a>添加分区

最近已经变冷的数据必须移到已分区表中。 这种定期分区交换的步骤如下：

1. 对于内存优化表中的数据，确定作为热数据与新的冷数据之间的分界线或分界点的日期时间。
2. 将新的冷数据从内存中 OLTP 表插入 cold\_staging 表。
3. 从内存优化表中删除相同的冷数据。
4. 将 cold\_staging 表交换到分区中。
5. 添加分区。

#### <a name="maintenance-window"></a>维护时段

前面的一个步骤是从内存优化表中删除新的冷数据。 此删除与添加新分区的最后一个步骤之间存在时间间隔。 在此间隔期间，尝试读取新的冷数据的任何应用程序都会失败。

有关示例，请参阅 [应用程序级分区](../../relational-databases/in-memory-oltp/application-level-partitioning.md)。

## <a name="code-sample"></a>代码示例

下面的 Transact-SQL 示例以一系列较小的代码块显示，这只是为了便于演示。 可以将它们都附加到一个大的代码块中，以便进行测试。

从整体来看，T-SQL 示例展示了如何将内存优化表与经过分区的基于磁盘的表一起使用。

T-SQL 示例的第一阶段创建数据库，然后在数据库中创建对象（如表）。 后面的阶段展示如何将数据从内存优化表移至经过分区的表。

### <a name="create-a-database"></a>创建数据库

T-SQL 示例的此部分将创建一个测试数据库。 将数据库配置为同时支持内存优化表和已分区表。

```sql
CREATE DATABASE PartitionSample;
GO

-- Add a FileGroup, enabled for In-Memory OLTP.
-- Change file path as needed.

ALTER DATABASE PartitionSample
    ADD FILEGROUP PartitionSample_mod
    CONTAINS MEMORY_OPTIMIZED_DATA;

ALTER DATABASE PartitionSample
    ADD FILE(
        NAME = 'PartitionSample_mod',
        FILENAME = 'c:\data\PartitionSample_mod')
    TO FILEGROUP PartitionSample_mod;
GO
```

### <a name="create-a-memory-optimized-table-for-hot-data"></a>为热数据创建内存优化表

这部分创建的是内存优化表，它保存最新的数据，其中大部分仍是热数据。

```sql
USE PartitionSample;
GO

-- Create a memory-optimized table for the HOT Sales Order data.
-- Notice the index that uses datetime2.

CREATE TABLE dbo.SalesOrders_hot (
   so_id INT IDENTITY PRIMARY KEY NONCLUSTERED,
   cust_id INT NOT NULL,
   so_date DATETIME2 NOT NULL INDEX ix_date NONCLUSTERED,
   so_total MONEY NOT NULL,
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc)
) WITH (MEMORY_OPTIMIZED=ON);
GO
```

### <a name="create-a-partitioned-table-for-cold-data"></a>为冷数据创建已分区表

这部分创建的是已分区表，它保存冷数据。

```sql
-- Create a partition and table for the COLD Sales Order data.
-- Notice the index that uses datetime2.

CREATE PARTITION FUNCTION [ByDatePF](datetime2) AS RANGE RIGHT
   FOR VALUES();
GO

CREATE PARTITION SCHEME [ByDateRange]
   AS PARTITION [ByDatePF]
   ALL TO ([PRIMARY]);
GO

CREATE TABLE dbo.SalesOrders_cold (
   so_id INT NOT NULL,
   cust_id INT NOT NULL,
   so_date DATETIME2 NOT NULL,
   so_total MONEY NOT NULL,
   CONSTRAINT PK_SalesOrders_cold PRIMARY KEY (so_id, so_date),
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc)
) ON [ByDateRange](so_date);
GO
```

### <a name="create-a-table-to-store-cold-data-during-move"></a>创建一个在移动过程中存储冷数据的表

这部分创建的是 cold\_staging 表。 同时创建了一个视图，将两个表的热数据和冷数据结合在一起。

```sql
-- A table used to briefly stage the newly cold data, during moves to a partition.

CREATE TABLE dbo.SalesOrders_cold_staging (
   so_id INT NOT NULL,
   cust_id INT NOT NULL,
   so_date datetime2 NOT NULL,
   so_total MONEY NOT NULL,
   CONSTRAINT PK_SalesOrders_cold_staging PRIMARY KEY (so_id, so_date),
   INDEX ix_date_total NONCLUSTERED (so_date desc, so_total desc),
   CONSTRAINT CHK_SalesOrders_cold_staging CHECK (so_date >= '1900-01-01')
);
GO

-- A view, for retrieving the aggregation of hot plus cold data.

CREATE VIEW dbo.SalesOrders
AS SELECT so_id,
          cust_id,
          so_date,
          so_total,
          1 AS 'is_hot'
       FROM dbo.SalesOrders_hot
   UNION ALL
   SELECT so_id,
          cust_id,
          so_date,
          so_total,
          0 AS 'is_cold'
       FROM dbo.SalesOrders_cold;
GO
```

### <a name="create-the-stored-procedure"></a>创建存储过程

这部分创建的是定期运行的存储过程。 此过程将内存优化表中新的冷数据移到已分区表中。

```sql
-- A stored procedure to move all newly cold sales orders data
-- to its staging location.

CREATE PROCEDURE dbo.usp_SalesOrdersOffloadToCold @splitdate datetime2
   AS
   BEGIN
      BEGIN TRANSACTION;

      -- Insert the cold data as a temporary heap.
      INSERT INTO dbo.SalesOrders_cold_staging WITH (TABLOCKX)
      SELECT so_id , cust_id , so_date , so_total
         FROM dbo.SalesOrders_hot WITH (serializable)
         WHERE so_date <= @splitdate;

      -- Delete the moved data from the hot table.
      DELETE FROM dbo.SalesOrders_hot WITH (SERIALIZABLE)
         WHERE so_date <= @splitdate;

      -- Update the partition function, and switch in the new partition.
      ALTER PARTITION SCHEME [ByDateRange] NEXT USED [PRIMARY];

      DECLARE @p INT = (
        SELECT MAX(partition_number)
            FROM sys.partitions
            WHERE object_id = OBJECT_ID('dbo.SalesOrders_cold'));

      EXEC sp_executesql
        N'ALTER TABLE dbo.SalesOrders_cold_staging
            SWITCH TO dbo.SalesOrders_cold partition @i',
        N'@i int',
        @i = @p;

      ALTER PARTITION FUNCTION [ByDatePF]()
      SPLIT RANGE( @splitdate);

      -- Modify a constraint on the cold_staging table, to align with new partition.
      ALTER TABLE dbo.SalesOrders_cold_staging
         DROP CONSTRAINT CHK_SalesOrders_cold_staging;

      DECLARE @s nvarchar( 100) = CONVERT( nvarchar( 100) , @splitdate , 121);
      DECLARE @sql nvarchar( 1000) = N'alter table dbo.SalesOrders_cold_staging 
         add constraint CHK_SalesOrders_cold_staging check (so_date > ''' + @s + ''')';
      PRINT @sql;
      EXEC sp_executesql @sql;

      COMMIT;
END;
GO
```

### <a name="prepare-sample-data-and-demo-the-stored-procedure"></a>准备示例数据，并演示存储过程

这一部分生成并插入示例数据，然后运行存储过程作为演示。

```sql
-- Insert sample values into the hot table.
INSERT INTO dbo.SalesOrders_hot VALUES(1,SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(1, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(1, SYSDATETIME(), 1);
GO

-- Verify that the hot data is in the table, by selecting from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Treat all data in the hot table as cold data:
-- Run the stored procedure, to move (offload) all sales orders to date to cold storage.
DECLARE @t datetime2 = SYSDATETIME();
EXEC dbo.usp_SalesOrdersOffloadToCold @t;

-- Again, read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Retrieve the name of every partition.
SELECT OBJECT_NAME( object_id) , * FROM sys.dm_db_partition_stats ps
   WHERE object_id = OBJECT_ID( 'dbo.SalesOrders_cold');

-- Insert more data into the hot table.
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO
INSERT INTO dbo.SalesOrders_hot VALUES(2, SYSDATETIME(), 1);
GO

-- Read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Again, run the stored procedure, to move all sales orders to date to cold storage.
DECLARE @t datetime2 = SYSDATETIME();
EXEC dbo.usp_SalesOrdersOffloadToCold @t;

-- Read hot plus cold data from the view.
SELECT * FROM dbo.SalesOrders;
GO

-- Again, retrieve the name of every partition.
-- The stored procedure can modify the partitions.
SELECT OBJECT_NAME( object_id) , partition_number , row_count
  FROM sys.dm_db_partition_stats ps
  WHERE object_id = OBJECT_ID( 'dbo.SalesOrders_cold')
    AND index_id = 1;
```

### <a name="drop-all-the-demo-objects"></a>删除所有演示对象

请记得清理测试系统中的演示测试数据库。

```sql
-- You must first leave the context of the PartitionSample database.

-- USE <A-Database-Name-Here>;
GO

DROP DATABASE PartitionSample;
GO
```

## <a name="see-also"></a>另请参阅

[Memory-Optimized Tables](./sample-database-for-in-memory-oltp.md)