---
description: sys.dm_resource_governor_resource_pools (Transact-SQL)
title: sys.dm_resource_governor_resource_pools (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_resource_pools_TSQL
- dm_resource_governor_resource_pools_TSQL
- sys.dm_resource_governor_resource_pools
- dm_resource_governor_resource_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_resource_governor_resource_pools dynamic management view
ms.assetid: 9bfc926e-d8bc-40f8-9229-ab1f8a1e69c5
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2ac359e1b73d963e7c2d7f880be7443118f8bd2c
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101443"
---
# <a name="sysdm_resource_governor_resource_pools-transact-sql"></a>sys.dm_resource_governor_resource_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回有关当前资源池状态、资源池的当前配置以及资源池统计信息的信息。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_resource_governor_resource_pools**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|资源池的 ID。 不可为 null。|  
|name|**sysname**|资源池的名称。 不可为 null。|  
|statistics_start_time|**datetime**|为该池重置统计信息的时间。 不可为 null。|  
|total_cpu_usage_ms|**bigint**|自重置资源调控器统计信息以来的累计 CPU 使用量（毫秒）。 不可为 null。|  
|cache_memory_kb|**bigint**|当前的缓存内存总用量（以 KB 为单位）。 不可为 null。|  
|compile_memory_kb|**bigint**|当前总的内存盗用量（以 KB 为单位）。 这部分内存主要用于编译和优化，但也可能包括其他内存使用者所用的内存量。 不可为 null。|  
|used_memgrant_kb|**bigint**|当前通过内存授予使用（盗用）的内存总量。 不可为 null。|  
|total_memgrant_count|**bigint**|此资源池中的内存授予累计计数。 不可为 null。|  
|total_memgrant_timeout_count|**bigint**|此资源池中内存授予超时的累计计数。 不可为 null。|  
|active_memgrant_count|**int**|当前内存授予计数。 不可为 null。|  
|active_memgrant_kb|**bigint**|当前内存授予总量（以 KB 为单位）。 不可为 null。|  
|memgrant_waiter_count|**int**|内存授予过程中当前挂起的查询数。 不可为 null。|  
|max_memory_kb|**bigint**|该资源池可拥有的最大内存量（以 KB 为单位）。 这取决于当前设置和服务器状态。 不可为 null。|  
|used_memory_kb|**bigint**|此资源池已用的内存量（以 KB 为单位）。 不可为 null。|  
|target_memory_kb|**bigint**|此资源池试图获取的目标内存量（以 KB 为单位）。 这取决于当前设置和服务器状态。 不可为 null。|  
|out_of_memory_count|**bigint**|自重置资源调控器统计信息以来池中内存分配失败次数。 不可为 null。|  
|min_cpu_percent|**int**|存在 CPU 争用时此资源池中所有请求有保障的平均 CPU 带宽的当前配置。 不可为 null。|  
|max_cpu_percent|**int**|存在 CPU 争用时允许此资源池中的所有请求使用的最大平均 CPU 带宽的当前配置。 不可为 null。|  
|min_memory_percent|**int**|存在内存争用时此资源池中所有请求有保障的内存量的当前配置。 不与其他资源池共享这部分内存。 不可为 null。|  
|max_memory_percent|**int**|此资源池中的请求可使用的总服务器内存百分比的当前配置。 不可为 null。|  
|cap_cpu_percent|**int**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 资源池中的所有请求都将收到的 CPU 带宽硬性上限。 将 CPU 最大带宽级别限制为指定的级别。 允许的值范围为 1 到 100。 不可为 null。|  
|min_iops_per_volume|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 针对此池的每个磁盘卷设置的每秒最小 I/O 数 (IOPS)。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|max_iops_per_volume|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 针对此池的每个磁盘卷设置的每秒最大 I/O 数 (IOPS)。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置均为 0。|  
|read_io_queued_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器以来排队的读取 IO 总数。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|read_io_issued_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来发出的读取 IO 总数。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|read_io_completed_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来完成的读取 IO 总数。 不可为 null。|  
|read_io_throttled_total|**int**|自重置资源调控器统计信息以来限制的读取 IO 总数。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|read_bytes_total|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来读取的总字节数。 不可为 null。|  
|read_io_stall_total_ms|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 读 IO 到达和完成之间的总时间（毫秒）。 不可为 null。 |  
|read_io_stall_queued_ms|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 读 IO 到达和发出之间的总时间（毫秒）。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。<br /><br /> 若要确定池的 IO 设置是否导致延迟，请从 **read_io_stall_total_ms** 中减去 **read_io_stall_queued_ms** 。|  
|write_io_queued_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来排队的写入 IO 总数。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|write_io_issued_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来发出的写入 IO 总数。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|write_io_completed_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来完成的写入 IO 总数。 不可为 Null。|  
|write_io_throttled_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来限制的写入 IO 总数。 不可为 Null。|  
|write_bytes_total|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 自重置资源调控器统计信息以来写入的总字节数。 不可为 null。|  
|write_io_stall_total_ms|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 写 IO 到达和完成之间的总时间（毫秒）。 不可为 null。 |  
|write_io_stall_queued_ms|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 写 IO 到达和发出之间的总时间（毫秒）。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。<br /><br /> 这是 IO 资源调控所引入的延迟。|  
|io_issue_violations_total|**int**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 总 IO 发出违反数。 即 IO 发出率低于保留比率时的次数。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|io_issue_delay_total_ms|**bigint**|**适用于**：[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 及更高版本。<br /><br /> 预定发出 IO 和实际发出 IO 之间的总时间（毫秒）。 可以为 Null。 如果没有为 IO 调控资源池，则为 null。 也就是说，资源池 MIN_IOPS_PER_VOLUME 和 MAX_IOPS_PER_VOLUME 设置是0。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="remarks"></a>备注  
 资源调控器工作负荷组和资源调控器资源池具有多对一映射关系。 因此，许多资源池统计信息都是派生自工作负荷组统计信息。  
  
 此动态管理视图显示了内存中配置。 若要查看存储的配置元数据，请使用 sys.resource_governor_resource_pools 目录视图。  
  
## <a name="permissions"></a>权限  
 需要 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_workload_groups (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.resource_governor_resource_pools &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-resource-pools-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  



