---
description: 'sys.dm_exec_session_wait_stats (Transact-sql) '
title: sys.dm_exec_session_wait_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 04/24/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_session_wait_stats
- sys.dm_exec_session_wait_stats_tsql
- dm_exec_session_wait_stats
- dm_exec_session_wait_stats_tsql
helpviewer_keywords:
- sys.dm_exec_session_wait_stats
ms.assetid: df84842a-71eb-4fda-b448-5953cf9985dc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: df522912f8c9fe07cd6aef495eddd90c66832e63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192971"
---
# <a name="sysdm_exec_session_wait_stats-transact-sql"></a>sys.dm_exec_session_wait_stats (Transact-sql) 

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  返回有关对每个会话执行的线程所遇到的所有等待的信息。 您可以使用此视图诊断会话的性能问题 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以及特定查询和批处理。  此视图返回的会话与 [sys.dm_os_wait_stats &#40;transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md) 聚合的信息相同，但也提供 **session_id** 号。  
  
**适用于**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] 及更高版本）。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|会话的 id。|  
|wait_type|**nvarchar(60)**|等待类型的名称。 有关详细信息，请参阅 [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)。|  
|waiting_tasks_count|**bigint**|该等待类型的等待数。 该计数器在每开始一个等待时便会增加。|  
|wait_time_ms|**bigint**|该等待类型的总等待时间（毫秒）。 该时间包括 signal_wait_time_ms。|  
|max_wait_time_ms|**bigint**|该等待类型的最长等待时间。|  
|signal_wait_time_ms|**bigint**|正在等待的线程从收到信号通知到其开始运行之间的时差。|  
  
## <a name="remarks"></a>备注  
 当会话打开时，此 DMV 会重置会话的信息，如果连接池) ，则重置会话 (  
  
 有关等待类型的信息，请参阅 [&#40;transact-sql&#41;sys.dm_os_wait_stats ](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 如果用户对服务器具有 **VIEW SERVER STATE** 权限，则用户将看到该实例上的所有正在执行的会话 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; 否则，用户将只看到当前会话。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_wait_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)  
 
