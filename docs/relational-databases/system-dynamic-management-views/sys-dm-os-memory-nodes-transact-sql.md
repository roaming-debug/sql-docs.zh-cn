---
description: sys.dm_os_memory_nodes (Transact-SQL)
title: sys.dm_os_memory_nodes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_os_memory_nodes_TSQL
- sys.dm_os_memory_nodes
- sys.dm_os_memory_nodes_TSQL
- dm_os_memory_nodes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_nodes dynamic management view
ms.assetid: bf4032fe-7db1-40e9-a62e-d69cebff4b44
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 02d875cc2ee069c43ca04c16f374c5cb8afe2b31
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097598"
---
# <a name="sysdm_os_memory_nodes-transact-sql"></a>sys.dm_os_memory_nodes (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的内部分配使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器。 跟踪来自 **sys.dm_os_process_memory** 和内部计数器的进程内存计数器之间的差异可以指示内存空间中的外部组件的内存使用情况 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 针对每个物理 NUMA 内存节点都创建了节点。 它们可能与 **sys.dm_os_nodes** 中的 CPU 节点不同。  
  
 对于直接通过 Windows 内存分配例程进行的分配不会进行跟踪。 下表提供了仅通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 内存管理器接口进行的内存分配的相关信息。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_memory_nodes**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**memory_node_id**|**smallint**|指定内存节点的 ID。 与 sys.dm_os_memory_clerks **memory_node_id** 相关。 不可为 Null。|  
|**virtual_address_space_reserved_kb**|**bigint**|指示既未提交也未映射到物理页的预留虚拟地址数量（以 KB 为单位）。 不可为 Null。|  
|**virtual_address_space_committed_kb**|**bigint**|指定已提交或映射到物理页的虚拟地址数量（以 KB 为单位）。 不可为 Null。|  
|**locked_page_allocations_kb**|**bigint**|指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 锁定的物理内存数量（以 KB 为单位）。 不可为 Null。|  
|**single_pages_kb**|**bigint**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。<br /><br /> 通过使用单页分配器并按此节点上运行的线程分配的已提交内存量（以 KB 为单位）。 该内存是从缓冲池分配的。 该值指示的是发生分配请求的节点，而不是满足分配请求的物理位置。|  
|**pages_kb**|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指定已提交的内存数量（以 KB 为单位），这是由内存管理器页面分配器从此 NUMA 节点分配的。 不可为 Null。|  
|**multi_pages_kb**|**bigint**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。<br /><br /> 通过使用多页分配器并按此节点上运行的线程分配的已提交内存量（以 KB 为单位）。 该内存是从缓冲池之外分配的。 此值指示发生分配请求的节点，而不是满足分配请求的物理位置。|  
|**shared_memory_reserved_kb**|**bigint**|指定从此节点中保留的共享内存数量（以 KB 为单位）。 不可为 Null。|  
|**shared_memory_committed_kb**|**bigint**|指定在此节点上提交的共享内存数量（以 KB 为单位）。 不可为 Null。|  
|**cpu_affinity_mask**|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 仅限内部使用。 不可为 Null。|  
|**online_scheduler_mask**|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 仅限内部使用。 不可为 Null。|  
|**processor_group**|**smallint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 仅限内部使用。 不可为 Null。|  
|**foreign_committed_kb**|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 指定从其他内存节点中提交的内存数量（以 KB 为单位）。 不可为 Null。|  
|**target_kb** |**bigint** |**适用于**：[!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。<br /><br /> 指定内存节点的内存目标（KB）。 |   
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="see-also"></a>另请参阅  
  [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


