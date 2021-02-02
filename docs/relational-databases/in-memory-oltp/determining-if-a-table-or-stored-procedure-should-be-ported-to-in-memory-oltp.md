---
title: 表或存储过程是否应移植到内存中 OLTP
description: 使用 SQL Server Management Studio 中的事务性能分析报告来评估内存中 OLTP 能否提高数据库应用程序的性能。
ms.custom: seo-dt-2019
ms.date: 08/02/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
helpviewer_keywords:
- Analyze, Migrate, Report
- AMR
ms.assetid: c1ef96f1-290d-4952-8369-2f49f27afee2
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 48d4d4b9602049c0c09e9e60ac8991c5c9a74154
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049230"
---
# <a name="determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp"></a>确定表或存储过程是否应移植到内存中 OLTP
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  通过 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的事务性能分析报表，可帮助你评估内存中 OLTP 是否将改进数据库应用程序的性能。 该报表还能够指明在应用程序中启用内存中 OLTP 所必须完成的工作量。 在你标识了要移植到内存中 OLTP 的基于磁盘的表之后，可以使用 [内存优化顾问](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md)帮助你迁移表。 同样， [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md) 帮助您将存储过程移植到本机编译的存储过程。 有关迁移方法的信息，请参阅 [内存中 OLTP - 常见的工作负荷模式和迁移注意事项](/previous-versions/dn673538(v=msdn.10))。  
  
 事务性能分析报表将直接对生产数据库或具有类似于生产工作负载的活动工作负载的测试数据库运行。  
  
 报表和迁移顾问将帮助你完成以下任务：  
  
-   分析工作负载以确定在内存中 OLTP 可潜在帮助提高性能的热点。 事务性能分析报表会建议可从转换为内存中 OLTP 获益最多的表和存储过程。  
  
-   帮助您规划和执行到内存中 OLTP 的迁移。 从基于磁盘的表到内存优化表的迁移路径可能比较费时。 内存优化顾问可帮助您找到表中的不兼容之处（必须在将表迁移到内存中 OLTP 之前予以解决）。 此外，内存优化顾问还可帮助您了解将表迁移到内存优化表会对应用程序产生何种影响。  
  
     在规划到内存中 OLTP 的迁移时，或每当您需要将某些表或存储过程迁移到内存中 OLTP 时，都可了解应用程序是否可从内存中 OLTP 获益。  
  
    > [!IMPORTANT]  
    >  数据库的性能取决于多种因素，不是所有这些因素都能被事务性能收集器发现和度量。 因此，事务性能分析报告不保证实际性能收益会符合其预测（如果作出任何预测）。  
  
 安装 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 时选择“管理工具 - 基本”或“管理工具 - 高级”，或[下载 SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 时，事物性能分析报表和迁移顾问会作为 SQL Server Management Studio (SSMS) 的部分安装 。    
  
## <a name="transaction-performance-analysis-reports"></a>事务性能分析报表  
 通过右键单击数据库，然后依次选择“报表”、“标准报表”、“事务性能分析概述”，可以在“对象资源管理器”中生成事务性能分析报表。 数据库需要有活动的工作负载或最近运行的工作负载，才能生成有意义的分析报表。  
  
### <a name="tables"></a>表
  
 表的详细报告包含三个部分：  
  
-   扫描统计信息部分  
  
     本部分包含一个表，其中显示已收集的有关数据库表扫描的统计信息。 列包括：  
  
    -   总访问次数百分比。 对此表的扫描和查询次数相对于整个数据库的活动的百分比。 这个比例越高，使用此表的频率相对于数据库中的其他表就越大。  
  
    -   查找统计数据/范围扫描统计数据。 此列记录在探查期间对表执行的点查询和范围扫描（索引扫描和表扫描）次数。 每事务的平均值为估计值。  
    
-   争用统计数据部分  
  
     本部分包含一个表，其中显示数据库表的争用。 有关数据库闩锁和锁的详细信息，请参阅“锁定体系结构”。 这些列如下所示：  
  
    -   总等待百分比。 此数据库表上闩锁和锁等待数占数据库活动的百分比。 这个比例越高，使用此表的频率相对于数据库中的其他表就越大。  
  
    -   闩锁统计信息。 这些列记录涉及此表的查询的闩锁等待数。 有关闩锁的信息，请参阅“闩锁”。 此数值越高，表的闩锁争用就越多。  
  
    -   锁定统计信息。 这组列记录对此表的查询的页锁定获取和等待次数。 有关锁的详细信息，请参阅“了解 SQL Server 中的锁定”。 等待越多，对表的锁定争用就越多。  
  
-   迁移难度部分  
  
     本部分包含一个表，其中显示将此数据库表转换为内存优化表的困难程度。 难度等级越高，转换表的难度就越大。 要查看转换此数据库表的详细信息，请使用内存优化顾问。  
  
表扫描和争用统计信息详细报表从 sys.dm_db_index_operational_stats (Transact-SQL) 收集和聚合而成。  

### <a name="stored-procedures"></a>存储过程

 CPU 时间与占用时间的比值较高的存储过程适合迁移。 该报告显示所有表引用，因为本机编译的存储过程只能引用内存优化的表，这可能加大迁移成本。  
  
 存储过程的详细报告包含两个部分：  
  
-   执行统计信息部分  
  
     本部分包含一个表，其中显示与已收集的存储过程执行相关的统计信息。 这些列如下所示：  
  
    -   缓存的时间。 高速缓存此执行计划的时间。 如果存储过程删除计划高速缓存并重新输入，这里将有每个缓存的时间。  
  
    -   总 CPU 时间。 存储过程在探查期间使用的总 CPU 时间。 此数值越高，存储过程使用的 CPU 就越多。  
  
    -   总执行时间。 存储过程在探查期间使用的执行时间总量。 此数值与 CPU 时间之间的差值越高，存储过程使用 CPU 的效率就越低。  
  
    -   总高速缓存失误数。 探查期间由存储过程执行引起的高速缓存失误数（从物理存储读取）。  
  
    -   执行计数。 在探查期间，此存储过程执行的次数。  
  
-   表引用部分  
  
     本部分包含一个表，其中显示此存储过程引用的那些表。 在将存储过程转换为本机编译的存储过程前，所有这些表必须转换为内存优化表，且它们必须位于同一服务器和数据库上。  
  
 存储过程执行统计信息详细报表从 sys.dm_exec_procedure_stats (Transact-SQL) 收集和聚合而成。 从 sys.sql_expression_dependencies (Transact-SQL) 获取引用。  
  
 要详细了解如何将存储过程转换为本机编译存储过程，请使用本机编译顾问。  
  
## <a name="generating-in-memory-oltp-migration-checklists"></a>生成内存中 OLTP 迁移清单  
 迁移清单识别内存优化表或本机编译存储过程不支持的任何表或存储过程功能。 内存优化和本机编译顾问可为基于单个磁盘的表或解释 T-SQL 存储过程生成一个清单。 还有可能为数据库中的多个表和存储过生成迁移清单。  
  
 使用“生成内存中 OLTP 迁移清单”命令或使用 PowerShell，可以在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中生成迁移清单。  
  
**使用 UI 命令生成迁移清单**  
  
1.  在“对象资源管理器”中，右键单击除系统数据库以外的数据库，单击“任务”，然后单击“生成内存中 OLTP 迁移清单”。  
  
2.  在“生成内存中 OLTP 迁移清单”对话框中，单击“下一步”以导航到“配置清单生成选项”页。 在该页上，执行下列操作。  
  
    1.  在“将清单保存到”  框中，输入文件夹路径。  
  
    2.  验证“为特定的表和存储过程生成清单”  处于选中状态。  
  
    3.  展开选择框中的“表”  和“存储过程”  节点。  
  
    4.  在选择框中选择几个对象。  
  
3.  单击“下一步”  并确认任务列表列表与“配置清单生成选项”  页上的设置一致。  
  
4.  单击“完成” ，然后确认仅为选定的对象生成迁移清单报表。  

 将这些报表与内存优化顾问工具和本机编译顾问工具生成的报表进行比较，验证这些报表的准确性。 有关详细信息，请参阅 [Memory Optimization Advisor](../../relational-databases/in-memory-oltp/memory-optimization-advisor.md) 和 [Native Compilation Advisor](../../relational-databases/in-memory-oltp/native-compilation-advisor.md)。  
  
**使用 SQL Server PowerShell 生成迁移清单**  
  
1.  在“对象资源管理器” 中，单击数据库，然后单击“启动 PowerShell” 。 验证出现了下面的提示。  
  
    ```  
    PS SQLSERVER: \SQL\{Instance Name}\DEFAULT\Databases\{two-part DB Name}>  
    ```  
  
2.  输入以下命令。  
  
    ```  
    Save-SqlMigrationReport -FolderPath "<folder_path>"  
    ```  
  
3.  验证下列各项。  
  
    -   创建了文件夹路径（如果它尚不存在）。  
  
    -   为数据库中的所有表和存储过程生成迁移清单报表，且报表位于 folder_path 指定的位置。  
  
**使用 Windows PowerShell 生成迁移清单**  
  
1.  启动提升的 Windows PowerShell 会话。  
  
2.  输入下面的命令。 对象可以是表或存储过程。  
  
    ```  
    [System.Reflection.Assembly]::LoadWithPartialName('Microsoft.SqlServer.SMO')  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -FolderPath "<folder_path1>"  
  
    ```  
  
    ```  
    Save-SqlMigrationReport -Server "<instance_name>" -Database "<db_name>" -Object <object_name> -FolderPath "<folder_path2>"  
  
    ```  
  
3.  验证下列各项。  
  
    -   为数据库中的所有表和存储过程生成迁移清单报表，且报表位于 folder_path 指定的位置。  
  
    -   <object_name> 的迁移清单报表是 folder_path2 所指定位置的唯一报表。  
  
## <a name="see-also"></a>另请参阅  
 [迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
