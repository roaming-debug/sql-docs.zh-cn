---
description: sys.server_event_sessions (Transact-SQL)
title: sys.server_event_sessions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_event_sessions
- server_event_sessions_TSQL
- sys.server_event_sessions_TSQL
- sys.server_event_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_event_sessions catalog view
- xe
ms.assetid: 796f3093-6a3e-4d67-8da6-b9810ae9ef5b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4eaa06349d7266fa44cecd69cbc7c56733e1a44f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211645"
---
# <a name="sysserver_event_sessions-transact-sql"></a>sys.server_event_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  列出所有存在于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的事件会话定义。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|event_session_id|**int**|事件会话的唯一 ID。 不可为 null。|  
|name|**sysname**|标识事件会话的用户定义名称。 名称是唯一的。 不可为 null。|  
|event_retention_mode|**nchar(1)**|确定如何处理事件丢失。 默认值为 S。不可为 Null 值。 可以是下列值之一：<br /><br /> S. 映射到 event_retention_mode_desc = ALLOW_SINGLE_EVENT_LOSS<br /><br /> M. 映射到 event_retention_mode_desc = ALLOW_MULTIPLE_EVENT_LOSS<br /><br /> N. 映射到 event_retention_mode_desc = NO_EVENT_LOSS|  
|event_retention_mode_desc|**sysname**|介绍了如何处理事件丢失。 默认为 ALLOW_SINGLE_EVENT_LOSS。 不可为 null。 可以是下列值之一：<br /><br /> ALLOW_SINGLE_EVENT_LOSS。 事件可能从会话中丢失。 所有事件缓冲区都已满时仅删除单个事件。 通过在缓冲区已满时丢失单个事件，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可实现足以满足要求的性能特征，同时还可使处理过的事件流中的事件丢失下降到最低程度。<br /><br /> ALLOW_MULTIPLE_EVENT_LOSS。 完整的事件缓冲区可能从会话中丢失。 事件丢失的数目取决于分配给会话的内存大小、内存的分区以及缓冲区中的事件大小。 在快速填充事件缓冲区时，此选项可将对服务器性能的影响降到最低。 不过，大量的事件可能从会话中丢失。<br /><br /> NO_EVENT_LOSS。 不允许事件丢失。 此选项可确保保留所有引发的事件。 使用此选项可强制所有可激发事件的任务都等到事件缓冲区中有可用空间时再执行。 事件会话处于活动状态时，这可能会导致可以察觉的性能下降。|  
|max_dispatch_latency|**int**|在事件用于会话目标之前将其缓存在内存中的时间（以毫秒为单位）。 有效值为从0到2147483648，0。 值0指示调度延迟为无限。 可以为 Null。|  
|max_memory|**int**|分配给会话的用于事件缓冲的内存量。 默认值为 4 MB。 可以为 Null。|  
|max_event_size|**int**|为不适合事件会话缓冲区的事件保留的内存量。 如果 max_event_size 超出了计算的缓冲区大小，max_event_size 的两个附加缓冲区将分配给事件会话。 可以为 Null。|  
|memory_partition_mode|**nchar(1)**|事件缓冲区在内存中的创建位置。 默认分区模式是 G。不可为 Null 值。 memory_partition_mode 是以下其中之一：<br /><br /> G - NONE<br /><br /> C - PER_CPU<br /><br /> N - PER_NODE|  
|memory_partition_mode_desc|**sysname**|默认值为 NONE。 不可为 null。 可以是下列值之一：<br /><br /> NONE。 在 SQL Server 实例中创建一组缓冲区。<br /><br /> PER_CPU。 为每个 CPU 创建一组缓冲区。<br /><br /> PER_NODE。 为每个非一致性内存访问 (NUMA) 节点创建一组缓冲区。|  
|track_causality|**bit**|启用或禁用因果关系跟踪。 如果设置为 1 (ON)，跟踪会被启用且不同服务器连接上的相关事件可以建立关联。 默认设置为 0 (OFF)。 不可为 null。|  
|startup_state|**bit**|值可以确定服务器启动时是否自动启动会话。 默认值为 0。 不可为 null。 是以下其中之一：<br /><br /> 0 (OFF)。 服务器启动时不启动会话。<br /><br /> 1 (ON)。 服务器启动时启动事件会话。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [&#40;Transact-sql&#41;的扩展事件目录视图 ](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)   
 [扩展事件](../../relational-databases/extended-events/extended-events.md)  
  
  
