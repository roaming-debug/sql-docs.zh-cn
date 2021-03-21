---
description: sys.dm_exec_compute_node_status (Transact-SQL)
title: sys.dm_exec_compute_node_status (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 11/04/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DM_EXEC_COMPUTE_NODE_STATUS_TSQL
- DM_EXEC_COMPUTE_NODE_STATUS
dev_langs:
- TSQL
helpviewer_keywords:
- PolyBase,views
- PolyBase
- dm_exec_compute_node_status
- sys.dm_exec_compute_node_status management view
ms.assetid: b606f91f-3a08-4a4f-bb57-32ae155b3738
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5878301bed9e18f2ae21f6706ad0eefa1318dbd0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750287"
---
# <a name="sysdm_exec_compute_node_status-transact-sql"></a>sys.dm_exec_compute_node_status (Transact-SQL)
[!INCLUDE [sqlserver2016-asa-pdw](../../includes/applies-to-version/sqlserver2016-asa-pdw.md)]

  保存有关所有 PolyBase 节点的性能和状态的附加信息。 为每个节点列出一行。  
  
|列名|数据类型|说明|范围|  
|-----------------|---------------|-----------------|-----------|  
|compute_node_id|`int`|与节点关联的唯一数字 id。|在扩展群集中唯一，而不考虑类型。|  
|process_id|`int`|||  
|process_name|`nvarchar(255)`|节点的逻辑名称。|任何适当长度的字符串。|  
|allocated_memory|`bigint`|此节点上已分配的内存总量。||  
|available_memory|`bigint`|此节点上的可用内存总量。||  
|process_cpu_usage|`bigint`|总进程 CPU 使用率（以计时周期为单位）。||  
|total_cpu_usage|`bigint`|总 CPU 使用率（以刻度为单位）。||  
|thread_count|`bigint`|在此节点上使用的线程的总数。||  
|handle_count|`bigint`|在此节点上使用的句柄总数。||  
|total_elapsed_time|`bigint`|自系统启动或重新启动以来经过的总时间。|自系统启动或重新启动以来经过的总时间。 如果 total_elapsed_time 超过24.8 天（以毫秒为单位） (整数的最大值) ，则会导致具体化失败，因为溢出。最大值（以毫秒为单位）等效于24.8 天。|  
|is_available|`bit`|指示此节点是否可用的标志。||  
|sent_time|`datetime`|此网络包的上次发送时间||  
|received_time|`datetime`|此节点上次发送网络包的时间。||  
|error_id|`nvarchar(36)`|此节点上发生的最后一个错误的唯一标识符。||
|compute_pool_id|`int`|池的唯一标识符。|

## <a name="see-also"></a>另请参阅  
 [通过动态管理视图进行 PolyBase 故障排除](/previous-versions/sql/sql-server-2016/mt146389(v=sql.130))   
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与数据库相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/database-related-dynamic-management-views-transact-sql.md)  
  
