---
title: 监视内存使用量
description: 监视 SQL Server 实例以确认内存使用量在正常范围内。 使用内存：可用字节和内存：Pages/sec 计数器。
ms.custom: ''
ms.date: 01/20/2021
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- tuning databases [SQL Server], memory
- monitoring server performance [SQL Server], memory usage
- isolating memory [SQL Server]
- paging rate [SQL Server]
- memory [SQL Server], monitoring usage
- monitoring [SQL Server], memory usage
- low-memory conditions
- database monitoring [SQL Server], memory usage
- available memory [SQL Server]
- page faults [SQL Server]
- monitoring performance [SQL Server], memory usage
- server performance [SQL Server], memory
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8954573b0c32eef45ec655b27d26e03e72be96ed
ms.sourcegitcommit: 713e5a709e45711e18dae1e5ffc190c7918d52e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/22/2021
ms.locfileid: "98688756"
---
# <a name="monitor-memory-usage"></a>监视内存使用量
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  定期监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例以确认内存使用量在正常范围内。 

## <a name="configuring-ssnoversion-max-memory"></a>配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 最大内存

默认情况下，随着时间的推移，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可能会消耗服务器中大部分可用的 Windows 操作系统内存。 获取内存后，除非检测到内存压力，否则将不会释放内存。 这是由设计决定的，并不表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 进程中存在内存泄漏。 使用[“最大服务器内存”选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)以限制 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在大多数情况下可获取的内存量。 有关详细信息，请参阅[内存管理体系结构指南](../../relational-databases/memory-management-architecture-guide.md#changes-to-memory-management-starting-with-)。

在 Linux 上的 SQL Server 中，通过 mssql-conf 工具和 [memory.memorylimitmb 设置](../../linux/sql-server-linux-configure-mssql-conf.md#memorylimit)来[内存限制](../../linux/sql-server-linux-performance-best-practices.md#advanced-configuration)。  

## <a name="monitor-operating-system-memory"></a>监视操作系统内存   
 若要监视内存不足的情况，请使用下列 Windows Server 计数器。 许多操作系统内存计数器都可通过动态管理视图 [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) 和 [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md) 进行查询。

-   Memory:Available Bytes  
此计数器指示当前有多少内存（以字节为单位）可供进程使用。 Available Bytes 计数器的低值可指示操作系统内存总体不足。 此值可通过 T-SQL 使用 [sys.dm_os_sys_memory](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md).available_physical_memory_kb 进行查询。

-   Memory:Pages/sec  
此计数器指示由于硬页错误而从磁盘取回的页数，或由于页错误而写入磁盘以释放工作集空间的页数。 **Pages/sec** 计数器的比率高表示分页过多。  

-   Memory:Page Faults/sec 此计数器指示所有进程（包括系统进程）的页错误率。 到磁盘的分页率偏低但非零（以及由此产生的页错误）是正常的，即使计算机有大量的可用内存。 Microsoft Windows 虚拟内存管理器 (VMM) 在剪裁 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和其他进程的工作集大小时会收走这些进程的页。 此 VMM 活动会导致页错误。  

-   Process:Page Faults/sec 此计数器指示给定用户进程的页错误率。 监视 Process:Page Faults/sec 以确定磁盘活动是否由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分页导致。 若要确定分页过多是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 还是由另一个进程导致，请监视 **Process:** Page Faults/sec 计数器，该计数器属于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 处理实例。  
  
 有关如何解决分页过多的详细信息，请参阅操作系统文档。  
  
## <a name="isolating-memory-used-by-ssnoversion"></a>隔离 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所用的内存 

 若要监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存使用量，请使用以下 [SQL Server 对象计数器](use-sql-server-objects.md)。 许多 SQL Server 对象计数器都可通过动态管理视图 [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) 或 [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) 进行查询。

 默认情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据可用的系统资源动态管理其内存要求。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 需要更多内存，它会查询操作系统以确定是否有可用的空闲物理内存，然后使用可用内存。 如果 OS 的空闲内存不足，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将内存释放回操作系统，直到内存不足的情况得以缓解，或者直到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 达到“最小服务器内存”限制。 但是，你可以覆盖此选项通过“最小服务器内存”和“最大服务器内存”服务器配置选项来动态使用内存 。 有关详细信息，请参阅 [服务器内存选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)。  
  
 若要监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用的内存量，请检查下列性能计数器：  
  
-   SQL Server：**Memory Manager:Total Server Memory (KB)**  
此计数器指示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器当前已提交到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的操作系统内存量。 此数字预计会根据实际活动的需要增长，并且在 SQL Server 启动后将会增长。 使用 [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) 动态管理视图查询此计数器，观察 committed_kb 列。

-   SQL Server：**Memory Manager:Target Server Memory (KB)**  
此计数器指示根据最近的工作负载，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能使用的理想内存量。 在一段时间的典型操作后比较“总服务器内存”，以确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是否分配了所需的内存量。 典型操作后，“总服务器内存”和“目标服务器内存”应类似 。 如果“总服务器内存”明显低于“目标服务器内存”，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可能遇到内存不足的情况 。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 启动后的一段时间内，随着“总服务器内存”的增长，“总服务器内存”预计会低于“目标服务器内存”  。 使用 [sys.dm_os_sys_info](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md) 动态管理视图查询此计数器，观察 committed_target_kb 列。 有关配置内存的详细信息和最佳做法，请参阅[服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually)。

-   Process:Working Set  
此计数器指示当前进程正在使用的物理内存量（取决于操作系统）。 观察此计数器的 sqlservr.exe 实例。 使用 [sys.dm_os_process_memory](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md) 动态管理视图查询此计数器，观察 physical_memory_in_use_kb 列。

-   Process:Private Bytes  
此计数器指示进程为自身使用而向操作系统请求的内存量。 观察此计数器的 sqlservr.exe 实例。 由于此计数器包含 sqlservr.exe 请求的所有内存分配（包括那些不受[“最大服务器内存”选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)限制的内存分配），因此，此计数器可报告大于[“最大服务器内存”选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)的值 。

-   SQL Server：**Buffer Manager:Database pages**  
此计数器指示缓冲池中包含数据库内容的页数。 不包括 SQL Server 进程中的其他非缓冲池内存。 使用 [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) 动态管理视图查询此计数器。

-   SQL Server：**Buffer Manager:Buffer Cache Hit Ratio**  
此计数器特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 需要 90 或更高的比率。 大于 90 的值表示内存中的数据缓存满足所有数据请求中 90% 以上的请求，无需从磁盘读取。 有关 SQL Server Buffer Manager 的详细信息，请参阅 [SQL Server Buffer Manager 对象](sql-server-buffer-manager-object.md)。 使用 [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) 动态管理视图查询此计数器。  
 
-   SQL Server：**Buffer Manager:Page life expectancy**  
此计数器测量最早的页面在缓冲池中停留的时间（以秒为单位）。 对于使用 NUMA 体系结构的系统，这是所有 NUMA 节点的平均值。 此值会不断增加，越高越好。 突然下降表示在进出缓冲池的过程中有大量数据流失，这表明工作负载无法充分利用内存中已有的数据。 每个 NUMA 节点都有自己的缓冲池节点。 在具有多个 NUMA 节点的服务器上，使用 SQL Server:Buffer Node:Page life expectancy 查看每个缓冲池节点的页生存期。 使用 [sys.dm_os_performance_counters](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md) 动态管理视图查询此计数器。

  
## <a name="examples"></a>示例 

### <a name="determining-current-memory-allocation"></a>确定当前内存分配  
 以下查询返回有关当前分配内存的信息。  
  
```  
SELECT
(total_physical_memory_kb/1024) AS Total_OS_Memory_MB,
(available_physical_memory_kb/1024)  AS Available_OS_Memory_MB
FROM sys.dm_os_sys_memory;

SELECT  
(physical_memory_in_use_kb/1024) AS Memory_used_by_Sqlserver_MB,  
(locked_page_allocations_kb/1024) AS Locked_pages_used_by_Sqlserver_MB,  
(total_virtual_address_space_kb/1024) AS Total_VAS_in_MB,
process_physical_memory_low,  
process_virtual_memory_low  
FROM sys.dm_os_process_memory;  
```  

### <a name="determining-current-sql-server-memory-utilization"></a>确定当前的 SQL Server 内存利用率   
 以下查询返回有关当前 SQL Server 内存利用率的信息。  
```  
SELECT
sqlserver_start_time,
(committed_kb/1024) AS Total_Server_Memory_MB,
(committed_target_kb/1024)  AS Target_Server_Memory_MB
FROM sys.dm_os_sys_info;
```   

### <a name="determining-page-life-expectancy"></a>确定页生存期
 下面的查询使用 sys.dm_os_performance_counters 在整个缓冲区管理器级别和每个 NUMA 节点级别观察 SQL Server 实例当前的“页生存期”值 。
```
SELECT
CASE instance_name WHEN '' THEN 'Overall' ELSE instance_name END AS NUMA_Node, cntr_value AS PLE_s
FROM sys.dm_os_performance_counters    
WHERE counter_name = 'Page life expectancy';
```

## <a name="see-also"></a>另请参阅
- [sys.dm_os_sys_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-memory-transact-sql.md)
- [sys.dm_os_process_memory (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-process-memory-transact-sql.md)
- [sys.dm_os_sys_info (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)
- [sys.dm_os_performance_counters (Transact-SQL)](../system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)
- [SQL Server - Memory Manager 对象](sql-server-memory-manager-object.md)
- [SQL Server - Buffer Manager 对象](sql-server-buffer-manager-object.md)   
- [服务器内存配置选项](../../database-engine/configure-windows/server-memory-server-configuration-options.md)
- [内存管理体系结构指南](../../relational-databases/memory-management-architecture-guide.md)
