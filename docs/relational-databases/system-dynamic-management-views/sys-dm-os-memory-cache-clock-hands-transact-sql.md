---
description: sys.dm_os_memory_cache_clock_hands (Transact-SQL)
title: sys.dm_os_memory_cache_clock_hands (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 12/21/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_cache_clock_hands_TSQL
- dm_os_memory_cache_clock_hands
- dm_os_memory_cache_clock_hands_TSQL
- sys.dm_os_memory_cache_clock_hands
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_clock_hands dynamic management view
ms.assetid: 0660eddc-691c-425f-9d43-71151d644de7
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: c87554c8c6707fc4527e0e2fe8c75868d56e486d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184868"
---
# <a name="sysdm_os_memory_cache_clock_hands-transact-sql"></a>sys.dm_os_memory_cache_clock_hands (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回特定缓存时钟的每个指针的状态。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_memory_cache_clock_hands**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|与时钟关联的缓存的地址。 不可为 null。|  
|name |**nvarchar(256)**|缓存的名称。 不可为 null。|  
|type |**nvarchar(60)**|缓存存储的类型。 可存在相同类型的多个缓存。 不可为 null。|  
|**clock_hand**|**nvarchar(60)**|手动类型。 这是以下各项之一：<br /><br /> 外部<br /><br /> 内部<br /><br /> 不可为 null。|  
|**clock_status**|**nvarchar(60)**|时钟的状态。 这是以下各项之一：<br /><br /> 已挂起<br /><br /> 运行<br /><br /> 不可为 null。|  
|**rounds_count**|**bigint**|通过缓存执行的、旨在删除项的清扫数。 不可为 null。|  
|**removed_all_rounds_count**|**bigint**|所有清扫删除的项数。 不可为 null。|  
|**updated_last_round_count**|**bigint**|上次清扫中更新的项数。 不可为 null。|  
|**removed_last_round_count**|**bigint**|上次清扫中删除的项数。 不可为 null。|  
|**last_tick_time**|**bigint**|移动时钟指针的最后时间，以毫秒表示。 不可为 null。|  
|**round_start_time**|**bigint**|上一次清扫的时间，以毫秒表示。 不可为 null。|  
|**last_round_start_time**|**bigint**|时钟完成上一往返花费的总时间，以毫秒表示。 不可为 null。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将信息存储在内存中一个称为内存缓存的结构中。 缓存中的信息可以是数据、索引条目、编译的过程计划以及其他各种类型的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 信息。 若要避免重新创建信息，尽可能将信息保留在内存缓存中，通常当信息太旧而失去用处或新信息需要使用内存空间时，会将旧信息从缓存中删除。 删除旧信息的过程称为内存清扫。 内存清扫是经常执行的操作，但不是连续执行的操作。 时钟算法控制内存缓存的清扫。 每个时钟能够控制几个内存清扫，称为指针。 内存缓存时钟指针是指一个内存清扫指针的当前位置。  

## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)    
 [sys.dm_os_memory_cache_counters &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-cache-counters-transact-sql.md)
  

