---
description: 'sys.dm_resource_governor_resource_pool_volumes (Transact-sql) '
title: sys.dm_resource_governor_resource_pool_volumes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_resource_governor_resource_pool_volumes_TSQL
- dm_resource_governor_resource_pool_volumes_TSQL
- dm_resource_governor_resource_pool_volumes
- sys.dm_resource_governor_resource_pool_volumes
dev_langs:
- TSQL
helpviewer_keywords:
- dm_resource_governor_resource_pool_volumes
- sys.dm_resource_governor_resource_pool_volumes
ms.assetid: fa692e56-c561-4533-97c5-bc12c600553f
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9c6707bfd8b1c29073239821cd0fa3c0425ef089
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093972"
---
# <a name="sysdm_resource_governor_resource_pool_volumes-transact-sql"></a>sys.dm_resource_governor_resource_pool_volumes (Transact-sql) 
[!INCLUDE[sqlserver](../../includes/applies-to-version/sqlserver.md)]

  对于每个磁盘卷返回有关当前资源池 IO 统计信息的信息。 此信息也可在 [sys.dm_resource_governor_resource_pools &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-resource-pools-transact-sql.md)的资源池级别使用。  
  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|pool_id|**int**|资源池的 ID。 不可为 null。|  
|volume_name|**sysname**|磁盘卷的名称。 不可为 null。|  
|read_io_queued_total|**int**|自重置资源调控器以来排队的总读 IO 数。 不可为 null。|  
|read_io_issued_total|**int**|自重置资源调控器统计信息以来发出的读取 IO 总数。 不可为 null。|  
|read_ios_completed_total|**int**|自重置资源调控器统计信息以来完成的读取 IO 总数。 不可为 null。|  
|read_ios_throttled_total|**int**|自重置资源调控器统计信息以来限制的读取 IO 总数。 不可为 null。|  
|read_bytes_total|**bigint**|自重置资源调控器统计信息以来读取的总字节数。 不可为 null。|  
|read_io_stall_total_ms|**bigint**|读 IO 到达和完成之间的总时间（毫秒）。 不可为 null。|  
|read_io_stall_queued_ms|**bigint**|读 IO 到达和发出之间的总时间（毫秒）。 这是 IO 资源调控所引入的延迟。 不可为 null。|  
|write_io_queued_total|**int**|自重置资源调控器统计信息以来排队的写入 IO 总数。 不可为 null。|  
|write_io_issued_total|**int**|自重置资源调控器统计信息以来发出的写入 IO 总数。 不可为 null。|  
|write_io_completed_total|**int**|自重置资源调控器统计信息以来完成的写入 IO 总数。 不可为 Null。|  
|write_io_throttled_total|**int**|自重置资源调控器统计信息以来限制的写入 IO 总数。 不可为 Null。|  
|write_bytes_total|**bigint**|自重置资源调控器统计信息以来写入的总字节数。 不可为 null。|  
|write_io_stall_total_ms|**bigint**|写 IO 发出和完成之间的总时间（毫秒）。 不可为 null。|  
|write_io_stall_queued_ms|**bigint**|写 IO 到达和发出之间的总时间（毫秒）。 这是 IO 资源调控所引入的延迟。 不可为 null。|  
|io_issue_violations_total|**int**|总 IO 发出违反数。 即 IO 发出率低于保留比率时的次数。 不可为 null。|  
|io_issue_delay_total_ms|**bigint**|预定发出 IO 和实际发出 IO 之间的总时间（毫秒）。 不可为 null。|  
  
## <a name="permissions"></a>权限  
 需要 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [sys.dm_resource_governor_workload_groups (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-resource-governor-workload-groups-transact-sql.md)   
 [sys.resource_governor_resource_pools &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-resource-governor-resource-pools-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md)  
  
  

