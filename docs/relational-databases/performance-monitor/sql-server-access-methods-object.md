---
title: SQL Server - Access Methods 对象 | Microsoft Docs
description: 了解“访问方法”计数器，这些计数器还可用于监视 SQL Server 中数据库内的数据量、索引和可用空间。
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Access Methods object
- SQLServer:Access Methods
ms.assetid: 27558585-e780-48bb-a042-30d664662ebc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7d19e5469ea93c751076654ce5fa8e12cde4f274
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505949"
---
# <a name="sql-server-access-methods-object"></a>SQL Server Access Methods 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **中的** Access Methods [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象提供用于监视如何访问数据库中的逻辑数据的计数器。 用 **Buffer Manager** 计数器监视对磁盘上的数据库页的物理访问。 监视用于访问数据库中存储的数据的方法有助于确定是否可以通过添加或修改索引、添加或移动分区、添加文件或文件组、对索引进行碎片整理或者重写查询来提高查询性能。 **Access Methods** 计数器也可用于监视数据库中数据、索引和可用空间的数量，从而指示每个服务器实例的数据卷和碎片。 索引的碎片过多会降低性能。  
  
 有关数据卷、碎片和用法的详细信息，请使用下列动态管理视图：  
  
-   [sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)  
  
-   [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)  
  
-   [sys.dm_db_partition_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)  
  
-   [sys.dm_db_index_usage_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)  
  
 有关文件、任务和会话级的 **tempdb** 中的空间使用情况，请使用下列动态管理视图：  
  
-   [sys.dm_db_file_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-file-space-usage-transact-sql.md)  
  
-   [sys.dm_db_task_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md)  
  
-   [sys.dm_db_session_space_usage (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)  
  
 下表介绍了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Access Methods 计数器。  
  
|SQL Server Access Methods 计数器|说明|  
|----------------------------------------|-----------------|  
|**AU cleanup batches/sec**|负责清除延迟删除的分配单元的后台任务每秒成功完成的批处理数。|  
|**AU cleanups/sec**|负责清除延迟删除的分配单元的后台任务每秒成功删除的分配单元数。 删除每个分配单元需要多个批处理。|  
|**By-reference Lob Create Count**|由引用传递的大型对象 (LOB) 值的计数。 在有些大容量操作中将使用由引用传递的 LOB，以避免通过值传递这些对象所需的开销。|  
|**By-reference Lob Use Count**|已使用的由引用传递的 LOB 值的计数。 在有些大容量操作中将使用由引用传递的 LOB，以避免通过值传递这些对象所需的开销。|  
|**Count Lob Readahead**|执行提前读的 LOB 页的计数。|  
|**Count Pull In Row**|已从行外请求到行内的列值的计数。|  
|**Count Push Off Row**|已从行内推送到行外的列值的计数。|  
|**Deferred Dropped Aus**|等待由负责清除延迟删除的分配单元的后台任务删除的分配单元数。|  
|**Deferred Dropped rowsets**|等待由负责清除延迟删除的行集的后台任务删除的、由于联机索引生成操作中止而创建的行集数。|  
|**Dropped rowset cleanups/sec**|负责清除延迟删除的行集的后台任务每秒成功删除的、由于联机索引生成操作中止而创建的行集数。|  
|**Dropped rowsets skipped/sec**|负责清除延迟删除的行集的后台任务每秒跳过的、由于联机索引生成操作中止而创建的行集数。|  
|**Extent Deallocations/sec**|在此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的所有数据库中每秒释放的区数。|  
|**Extents Allocated/sec**|在此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的所有数据库中每秒分配的区数。|  
|**Failed AU cleanup batches/sec**|每秒失败并需要由负责清除延迟删除的分配单元的后台任务重试的批处理数。 失败可能是由于内存或磁盘空间不足、硬件故障和其他原因。|  
|**Failed leaf page cookie**|在索引搜索期间，自叶级页上发生更改以来无法使用叶级页 cookie 的次数。 Cookie 用于加快索引搜索。|  
|**Failed tree page cookie**|在索引搜索期间，自树页的父页上发生更改以来无法使用树页 cookie 的次数。 Cookie 用于加快索引搜索。|  
|**Forwarded Records/sec**|每秒通过正向记录指针提取的记录数。|  
|**FreeSpace Page Fetches/sec**|每秒通过可用空间扫描提取的页数。 这些扫描将在已分配给某个分配单元的页内搜索可用空间，以便满足插入或修改记录片段的请求。|  
|**FreeSpace Scans/sec**|每秒为在已分配给某个分配单元的页内搜索可用空间以插入或修改记录片段而启动的扫描数。 每次扫描可能会找到多个页。|  
|**Full Scans/sec**|每秒不受限制的完全扫描数。 这些扫描可以是基表扫描，也可以是全文索引扫描。|  
|**Index Searches/sec**|每秒索引搜索数。 索引搜索用于启动范围扫描、重新定位范围扫描、重新验证扫描点、提取单个索引记录以及向下搜索索引以确定新行的插入位置。|  
|**InSysXact 等待次数/秒**|读取器因为设置 InSysXact 位而需要等待某一页的次数。|  
|**LobHandle Create Count**|已创建的临时 LOB 计数。|  
|**LobHandle Destroy Count**|已破坏的临时 LOB 计数。|  
|**LobSS Provider Create Count**|已创建的 LOB 存储服务提供程序 (LobSSP) 计数。 对每个 LobSSP 创建一个工作表。|  
|**LobSS Provider Destroy Count**|已破坏的 LobSSP 计数。|  
|**LobSS Provider Truncation Count**|已截断的 LobSSP 计数。|  
|**Mixed page allocations/sec**|每秒从混合区分配的页数。 这些页可用于存储 IAM 页和分配给某个分配单元的前八页。|  
|**Page compression attempts/sec**|对页级别压缩计算的页数。 因为可以极大地节省空间，所以将包括未压缩的页。 包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中的所有对象。 有关特定对象的信息，请参阅 [sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)实例的所有数据库中每秒释放的区数。|  
|**Page Deallocations/sec**|在此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的所有数据库中每秒释放的页数。 其中包括来自混合区和统一区的页。|  
|**Page Splits/sec**|每秒由于索引页溢出而发生的页拆分数。|  
|**Pages Allocated/sec**|在此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例的所有数据库中每秒分配的页数。 这些页包括从混合区和统一区中分配的页。|  
|**Pages compressed/sec**|使用 PAGE 压缩压缩的数据页数。 包括 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例中的所有对象。 有关特定对象的信息，请参阅 [sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)实例的所有数据库中每秒释放的区数。|  
|**Probe Scans/sec**|每秒内用于直接在索引或基本表中查找最多一个限定行的探测扫描数。|  
|**Range Scans/sec**|每秒通过索引进行的限定范围的扫描数。|  
|**Scan Point Revalidations/sec**|每秒必须重新验证扫描点才能继续扫描的次数。|  
|**Skipped Ghosted Records/sec**|扫描期间每秒跳过的虚影记录数。|  
|**Table Lock Escalations/sec**|表上的锁升级到 TABLE 或 HoBT 粒度的次数。|  
|**Used leaf page cookie**|在索引搜索期间，自叶级页上未发生更改以来成功使用叶级页 cookie 的次数。 Cookie 用于加快索引搜索。|  
|**Used tree page cookie**|在索引搜索期间，自树页的父页上未发生更改以来成功使用树页 cookie 的次数。 Cookie 用于加快索引搜索。|  
|**Workfiles Created/sec**|每秒创建的工作文件数。 例如，工作文件可用于存储哈希联接和哈希聚合的临时结果。|  
|**Worktables Created/sec**|每秒创建的工作表数。 例如，工作表可用于存储查询假脱机、LOB 变量、XML 变量和游标的临时结果。|  
|**Worktables From Cache Base**|仅供内部使用。|  
|**Worktables From Cache Ratio**|其前两页虽未分配但可从工作表缓存中直接使用的已创建的工作表的百分比。 （工作表被删除后，这两页可能会保持分配状态并返回到工作表缓存中。 这将提高性能。）|  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
