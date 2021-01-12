---
description: sys.dm_xe_sessions (Transact-SQL)
title: sys.dm_xe_sessions (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_xe_sessions_TSQL
- dm_xe_sessions
- sys.dm_xe_sessions_TSQL
- sys.dm_xe_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_sessions dynamic management view
- extended events [SQL Server], views
ms.assetid: defd6efb-9507-4247-a91f-dc6ff5841e17
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3acce4d01f26c3fcc818201e05c33ab9a46116a8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099691"
---
# <a name="sysdm_xe_sessions-transact-sql"></a>sys.dm_xe_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关处于活动状态的扩展事件会话的信息。 此会话是事件、操作和目标的集合。  
    
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|address|**varbinary(8)**|会话的内存地址。 地址在本地系统中是唯一的。 不可为 null。|  
|name|**nvarchar(256)**|会话的名称。 名称在本地系统中是唯一的。 不可为 null。|  
|pending_buffers|**int**|正在挂起处理的已满缓冲区的数量。 不可为 null。|  
|total_regular_buffers|**int**|与会话相关联的常规缓冲区的总数。 不可为 null。<br /><br /> 注意：大多数时候都在使用常规缓冲区。 这类缓冲区很大，足以容纳大量事件。 每个会话通常有三个或更多缓冲区。 常规缓冲区的数目由服务器根据通过 MEMORY_PARTITION_MODE 选项设置的内存分区自动确定。 常规缓冲区的大小等于 MAX_MEMORY 选项的值（默认值为 4 MB）除以缓冲区的数目。 有关 MEMORY_PARTITION_MODE 和 MAX_MEMORY 选项的详细信息，请参阅 [CREATE EVENT SESSION &#40;transact-sql&#41;](../../t-sql/statements/create-event-session-transact-sql.md)。|  
|regular_buffer_size|**bigint**|常规缓冲区的大小（以字节为单位）。 不可为 null。|  
|total_large_buffers|**int**|大型缓冲区的总数。 不可为 null。<br /><br /> 注意：当事件大于常规缓冲区时，将使用较大的缓冲区。 为此，需要显式保留这类缓冲区。 大型缓冲区在事件会话启动时分配，并根据 MAX_EVENT_SIZE 选项确定大小。 有关 MAX_EVENT_SIZE 选项的详细信息，请参阅 [CREATE EVENT SESSION &#40;transact-sql&#41;](../../t-sql/statements/create-event-session-transact-sql.md)。|  
|large_buffer_size|**bigint**|大型缓冲区的大小（以字节为单位）。 不可为 null。|  
|total_buffer_size|**bigint**|用于存储会话事件的内存缓冲区的总大小（以字节为单位）。 不可为 null。|  
|buffer_policy_flags|**int**|指示会话事件缓冲区在所有缓冲区已满并且一个新事件被激发时的行为的位图。 不可为 null。|  
|buffer_policy_desc|**nvarchar(256)**|指示会话事件缓冲区在所有缓冲区已满并且一个新事件被激发时的行为的说明。  不可为 null。 buffer_policy_desc 可以是以下项之一：<br /><br /> Drop 事件<br /><br /> 不删除事件<br /><br /> 删除已满缓冲区<br /><br /> 分配新缓冲区|  
|flags|**int**|指示已对会话设置的标志的位图。 不可为 null。|  
|flag_desc|**nvarchar(256)**|关于对会话设置的标志的说明。  不可为 null。 flag_desc 可以是以下各项的任意组合：<br /><br /> 在关闭时刷新缓冲区<br /><br /> 专用调度程序<br /><br /> 允许递归事件|  
|dropped_event_count|**int**|缓冲区已满时删除的事件数。 如果缓冲区策略是 "删除已满缓冲区" 或 "不删除事件"，则此值为 **0** 。 不可为 null。|  
|dropped_buffer_count|**int**|缓冲区已满时删除的缓冲区数。 如果缓冲区策略设置为 "删除事件" 或 "不删除事件"，则此值为 **0** 。 不可为 null。|  
|blocked_event_fire_time|**int**|缓冲区已满时事件触发受阻的时间长度。 如果缓冲区策略是 "删除完全缓冲区" 或 "删除事件"，则此值为 **0** 。 不可为 null。|  
|create_time|**datetime**|会话的创建时间。 不可为 null。|  
|largest_event_dropped_size|**int**|不适合会话缓冲区的最大事件大小。 不可为 null。|  
  
## <a name="permissions"></a>权限  
 要求具有服务器的 VIEW SERVER STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

