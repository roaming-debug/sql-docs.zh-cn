---
title: Linux 上 SQL Server 的性能功能入门
description: 本文为刚接触 SQL Server 的 Linux 用户介绍 SQL Server 的性能功能。 其中许多示例适用于所有平台，但本文的上下文为 Linux 环境。
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 60036d26-4797-4872-9a9e-3552841c61be
ms.openlocfilehash: 24794910e53f730b16975714c514eb717b41ce49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350966"
---
# <a name="walkthrough-for-the-performance-features-of-sql-server-on-linux"></a>Linux 上 SQL Server 的性能功能演练

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

如果你是刚接触 SQL Server 的 Linux 用户，以下任务可帮助你了解某些性能功能。 虽然这些并非 Linux 独有或特定的任务，但能有助于你了解需要深入了解的领域。 每个示例中均提供了该领域的详细文档链接。

> [!NOTE]
> 以下示例使用 AdventureWorks 示例数据库。 有关如何获取并安装此示例数据库的说明，请参阅[将 SQL Server 数据库从 Windows 还原到 Linux](sql-server-linux-migrate-restore-database.md)。

## <a name="create-a-columnstore-index"></a>创建列存储索引
列存储索引是使用列式数据格式（称为列存储）存储和查询大量数据的技术。  

1. 通过执行以下 Transact-SQL 命令将列存储索引添加到 SalesOrderDetail 表：

   ```sql
   CREATE NONCLUSTERED COLUMNSTORE INDEX [IX_SalesOrderDetail_ColumnStore]
      ON Sales.SalesOrderDetail
      (UnitPrice, OrderQty, ProductID)
   GO
   ```

2. 执行以下查询，使用列存储索引扫描表：

   ```sql
   SELECT ProductID, SUM(UnitPrice) SumUnitPrice, AVG(UnitPrice) AvgUnitPrice,
      SUM(OrderQty) SumOrderQty, AVG(OrderQty) AvgOrderQty
   FROM Sales.SalesOrderDetail
      GROUP BY ProductID
      ORDER BY ProductID
   ```

3. 查找列存储索引的 object_id，然后确认它出现在 SalesOrderDetail 表的使用情况统计信息中，以验证是否使用了列存储索引：

   ```sql
   SELECT * FROM sys.indexes WHERE name = 'IX_SalesOrderDetail_ColumnStore'
   GO

   SELECT * 
   FROM sys.dm_db_index_usage_stats
      WHERE database_id = DB_ID('AdventureWorks')
      AND object_id = OBJECT_ID('AdventureWorks.Sales.SalesOrderDetail');
   ```
   
## <a name="use-in-memory-oltp"></a>使用内存中 OLTP
SQL Server 提供的内存中 OLTP 功能可极大提升应用程序系统的性能。  《评估指南》中的本节内容将介绍如何创建存储在内存中的内存优化表，以及创建无需编译或解释即可访问表的本机编译的存储过程。

### <a name="configure-database-for-in-memory-oltp"></a>配置内存中 OLTP 的数据库
1. 建议将数据库的兼容级别至少设置为 130，以使用内存中 OLTP。  使用以下查询检查 AdventureWorks 的当前兼容级别：  

   ```sql
   USE AdventureWorks
   GO
   SELECT d.compatibility_level
   FROM sys.databases as d
     WHERE d.name = Db_Name();
   GO
   ```
   
   如有必要，将级别更新为 130：

   ```sql
   ALTER DATABASE CURRENT
   SET COMPATIBILITY_LEVEL = 130;
   GO
   ```

2. 当事务同时涉及基于磁盘的表和内存优化表时，事务的内存优化部分必须在名为“快照”的事务隔离级别运行。  若要可靠地对跨容器事务中的内存优化表强制执行此级别，请执行以下脚本：

   ```sql
   ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
   GO
   ```

3. 需要先创建 Memory Optimized FILEGROUP 和数据文件的容器，然后才能创建内存优化表：

   ```sql
   ALTER DATABASE AdventureWorks ADD FILEGROUP AdventureWorks_mod CONTAINS memory_optimized_data
   GO  
   ALTER DATABASE AdventureWorks ADD FILE (NAME='AdventureWorks_mod', FILENAME='/var/opt/mssql/data/AdventureWorks_mod') TO FILEGROUP AdventureWorks_mod
   GO
   ```

### <a name="create-a-memory-optimized-table"></a>创建内存优化表
内存优化表的主存储是主内存，因此与基于磁盘的表不同，不必从磁盘将数据读取到内存缓冲区。  若要创建内存优化表，请使用 MEMORY_OPTIMIZED = ON 子句。

1. 执行以下查询创建内存优化表 dbo.ShoppingCart。  默认情况下，数据将保留在磁盘上以获得持续性（请注意，还可将 DURABILITY 设置为仅保留架构）。 

   ```sql
   CREATE TABLE dbo.ShoppingCart ( 
   ShoppingCartId INT IDENTITY(1,1) PRIMARY KEY NONCLUSTERED,
   UserId INT NOT NULL INDEX ix_UserId NONCLUSTERED HASH WITH (BUCKET_COUNT=1000000), 
   CreatedDate DATETIME2 NOT NULL, 
   TotalPrice MONEY
   ) WITH (MEMORY_OPTIMIZED=ON) 
   GO
   ```

2. 在表中插入一些记录：

   ```sql
   INSERT dbo.ShoppingCart VALUES (8798, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (23, SYSDATETIME(), 45.4) 
   INSERT dbo.ShoppingCart VALUES (80, SYSDATETIME(), NULL) 
   INSERT dbo.ShoppingCart VALUES (342, SYSDATETIME(), 65.4) 
   ```

### <a name="natively-compiled-stored-procedure"></a>本机编译的存储过程
SQL Server 支持访问内存优化表的本机编译的存储过程。 T-SQL 语句编译为机器代码并存储为本机 DLL，能够比传统 T-SQL 更快地访问数据，更高效地执行查询。   对使用 NATIVE_COMPILATION 来标记的存储过程执行本机编译。 

1. 执行以下脚本创建本机编译的存储过程，该过程在 ShoppingCart 表中插入大量记录：


   ```sql
   CREATE PROCEDURE dbo.usp_InsertSampleCarts @InsertCount int 
    WITH NATIVE_COMPILATION, SCHEMABINDING AS 
   BEGIN ATOMIC 
   WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')

   DECLARE @i int = 0

   WHILE @i < @InsertCount 
    BEGIN 
     INSERT INTO dbo.ShoppingCart VALUES (1, SYSDATETIME() , NULL) 
     SET @i += 1 
    END
   END 
   ```
2. 插入 1,000,000 行：

   ```sql
   EXEC usp_InsertSampleCarts 1000000 
   ```

3. 验证是否已插入这些行：

   ```sql
   SELECT COUNT(*) FROM dbo.ShoppingCart 
   ```

### <a name="learn-more-about-in-memory-oltp"></a>了解有关内存中 OLTP 的详细信息
有关内存中 OLTP 的详细信息，请参阅以下主题：

- [快速入门 1：可提高 Transact SQL 性能的内存中 OLTP 技术](../relational-databases/in-memory-oltp/survey-of-initial-areas-in-in-memory-oltp.md)
- [迁移到内存中 OLTP](../relational-databases/in-memory-oltp/plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)
- [通过使用内存优化获得更快的临时表和表变量](../relational-databases/in-memory-oltp/faster-temp-table-and-table-variable-by-using-memory-optimization.md)
- [内存使用情况的监视和故障排除](../relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage.md)
- [内存中 OLTP（内存中优化）](../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)

## <a name="use-query-store"></a>使用查询存储
查询存储区收集有关查询、执行计划和运行时统计信息的详细性能信息。

默认情况下，查询存储不处于活动状态，可通过 ALTER DATABASE 启用：

   ```sql
   ALTER DATABASE AdventureWorks SET QUERY_STORE = ON;
   ```

运行以下查询，返回查询存储中有关查询和计划的信息： 

   ```sql
   SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
   FROM sys.query_store_plan AS Pl
      JOIN sys.query_store_query AS Qry
         ON Pl.query_id = Qry.query_id
      JOIN sys.query_store_query_text AS Txt
         ON Qry.query_text_id = Txt.query_text_id ;
   ```

## <a name="query-dynamic-management-views"></a>查询动态管理视图
动态管理视图返回可用于监视服务器实例的运行状况、诊断故障以及优化性能的服务器状态信息。

要查询 dm_os_wait 统计信息动态管理视图：

   ```sql
   SELECT wait_type, wait_time_ms
   FROM sys.dm_os_wait_stats;
   ```