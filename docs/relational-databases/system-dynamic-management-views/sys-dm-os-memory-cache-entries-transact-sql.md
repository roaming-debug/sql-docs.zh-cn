---
description: sys.dm_os_memory_cache_entries (Transact-SQL)
title: sys.dm_os_memory_cache_entries (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_cache_entries
- sys.dm_os_memory_cache_entries
- dm_os_memory_cache_entries_TSQL
- sys.dm_os_memory_cache_entries_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_entries dynamic management view
ms.assetid: dd32be6b-10d1-4059-b4fd-0bf817f40d54
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 6d12296ea1ce831a9f1299b150d3e2a81a39de3d
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100342826"
---
# <a name="sysdm_os_memory_cache_entries-transact-sql"></a>sys.dm_os_memory_cache_entries (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中返回有关缓存中所有条目的信息。 使用此视图可对缓存条目进行跟踪，直至它们的关联对象。 还可使用此视图获取有关缓存条目的统计信息。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_memory_cache_entries**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|缓存的地址。 不可为 null。|  
|name |**nvarchar(256)**|缓存的名称。 不可为 null。|  
|type |**varchar(60)**|缓存类型。 不可为 null。|  
|**entry_address**|**varbinary(8)**|缓存条目的描述符地址。 不可为 null。|  
|**entry_data_address**|**varbinary(8)**|缓存条目中用户数据的地址。<br /><br /> 0x00000000 = 条目数据地址不可用。<br /><br /> 不可为 null。|  
|**in_use_count**|**int**|同时使用此缓存条目的用户数。 不可为 null。|  
|**is_dirty**|**bit**|指示是否将此缓存条目标记为待删除。 1 = 标记为待删除。 不可为 null。|  
|**disk_ios_count**|**int**|创建此条目时引发的 I/O 数。 不可为 null。|  
|**context_switches_count**|**int**|创建此条目时引发的上下文开关数。 不可为 null。|  
|**original_cost**|**int**|此条目的原始开销。 此值是引发的 I/O 数、CPU 指令开销以及条目占用的内存量等的近似值。 开销越大，从缓存中删除此条目的机会越小。 不可为 null。|  
|**current_cost**|**int**|缓存条目的当前开销。 此值将在条目清除过程中更新。 重用条目时，当前开销将重置为原始值。 不可为 null。|  
|**memory_object_address**|**varbinary(8)**|关联内存对象的地址。 可以为 Null。|  
|**pages_allocated_count**|**bigint**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。<br /><br /> 存储此缓存条目的 8 KB 页的数目。 不可为 null。|  
|**pages_kb**|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 此缓存条目使用的内存量 (KB)。  不可为 null。|  
|**entry_data**|**nvarchar(2048)**|缓存条目的序列化表示形式。 此信息与缓存存储相关。 可以为 Null。|  
|**pool_id**|**int**|**适用于**：[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 及更高版本。<br /><br /> 与条目关联的资源池 ID。 可以为 Null。<br /><br /> 不是 katmai|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限 

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="see-also"></a>另请参阅  
 
  [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


