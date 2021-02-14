---
description: 'sys.dm_hadr_ag_threads (Transact-sql) '
title: sys.dm_hadr_ag_threads (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_hadr_ag_threads_TSQL
- sys.dm_hadr_ag_threads
- dm_hadr_ag_threads_TSQL
- dm_hadr_ag_threads
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- Availability Groups [SQL Server], WSFC clusters
- sys.dm_hadr_ag_threads catalog view
ms.assetid: ''
author: kfarlee
ms.author: kfarlee
monikerRange: ''
ms.openlocfilehash: c8487dd07e424b7edccc17b8a9587cfc91f74268
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100354134"
---
# <a name="sysdm_hadr_ag_threads-transact-sql"></a>sys.dm_hadr_ag_threads (Transact-sql) 

HADR 线程遥测 Dmv (**sys.dm_hadr_ag_threads** 和 [sys.dm_hadr_db_threads](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-db-threads-transact-sql.md)) 允许用户通过可用性组和高可用性数据库快速深入了解线程使用情况。 了解此线程使用情况是优化可用性组的重要基准。

此 DMV 报告可用性组级别的线程使用情况。

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|可用性组的标识符。|
|name |**nvarchar(128)**|可用性组的名称。|
|**num_databases**|**int**|可用性组中的数据库数。|
|**num_capture_threads**|**int**|此可用性组中的所有数据库使用的日志捕获线程数。|
|**num_redo_threads**|**int**|此可用性组中的所有数据库使用的重做线程数。|
|**num_parallel_redo_threads**|**int**|此可用性组中的所有数据库使用的并行重做线程数。|
|**num_hadr_threads**|**int**|此可用性组中所有数据库使用的所有 always on 线程的数目。|

## <a name="permissions"></a>权限  

 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  

 [Always On 可用性组动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/always-on-availability-groups-dynamic-management-views-functions.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [&#40;Transact-sql 监视可用性组&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [AlwaysOn 可用性组 &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  