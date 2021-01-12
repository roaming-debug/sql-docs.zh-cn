---
description: 'sys.dm_db_column_store_row_group_operational_stats (Transact-sql) '
title: sys.dm_db_column_store_row_group_operational_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 31b71c68-50a0-4fd8-a7fe-2d2292be1163
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: e8ff8ab3db826d483f8f6df09277e1a5510aaf48
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097707"
---
# <a name="sysdm_db_column_store_row_group_operational_stats-transact-sql"></a>sys.dm_db_column_store_row_group_operational_stats (Transact-sql) 

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  为列存储索引中的压缩行组返回当前行级 i/o、锁定和访问方法活动。 使用 **sys.dm_db_column_store_row_group_operational_stats** 可以跟踪用户查询必须等待读取或写入列存储索引的压缩行组或分区的时间长度，并标识遇到重要 i/o 活动或热点的行组。  
  
 内存中列存储索引不会出现在此 DMV 中。  
 
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|具有列存储索引的表的 ID。|  
|index_id|**int**|columnstore 索引的 ID。|  
|**partition_number**|**int**|索引或堆中从 1 开始的分区号。|  
|**row_group_id**|**int**|列存储索引中的行组 ID。 这在分区中是唯一的。|  
|**scan_count**|**int**|自上次 SQL 重新启动以来通过行组扫描的次数。|  
|**delete_buffer_scan_count**|**int**|删除缓冲区用于确定此行组中的已删除行的次数。 这包括访问内存中的哈希表和基础 btree。|  
|**index_scan_count**|**int**|扫描列存储索引分区的次数。 这对于分区中的所有行组都是相同的。|  
|**rowgroup_lock_count**|**bigint**|自上次 SQL 重新启动以来此行组的锁请求的累计计数。|  
|**rowgroup_lock_wait_count**|**bigint**|自上次重新启动 SQL 以来，数据库引擎在此行组锁上等待的累积次数。|  
|**rowgroup_lock_wait_in_ms**|**bigint**|自上次重新启动 SQL 以来，数据库引擎在此行组锁上等待的总毫秒数。|  
  
## <a name="permissions"></a>权限  
 需要下列权限：  
  
-   Object_id 指定的表的 CONTROL 权限。  
  
-   VIEW DATABASE STATE 权限，以便通过使用对象通配符 @*object_id* = NULL 返回有关数据库中所有对象的信息  
  
 授予 VIEW DATABASE STATE 权限允许返回数据库中的所有对象，而不考虑对特定对象拒绝的任何 CONTROL 权限。  
  
 拒绝 VIEW DATABASE STATE 将禁止返回数据库中的所有对象，而不管对特定对象授予的任何 CONTROL 权限。 此外，当指定数据库通配符 @*database_id*= NULL 时，将忽略数据库。  
  
 有关详细信息，请参阅 [&#40;transact-sql&#41;中的动态管理视图和函数 ](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与索引相关的动态管理视图和函数 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_usage_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-usage-stats-transact-sql.md)   
 [sys.dm_os_latch_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-latch-stats-transact-sql.md)   
 [sys.dm_db_partition_stats &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-partition-stats-transact-sql.md)   
 [sys.allocation_units &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-allocation-units-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  

