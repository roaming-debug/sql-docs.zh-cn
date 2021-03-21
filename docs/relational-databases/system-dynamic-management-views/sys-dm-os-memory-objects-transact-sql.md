---
description: sys.dm_os_memory_objects (Transact-SQL)
title: sys.dm_os_memory_objects (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_objects
- sys.dm_os_memory_objects
- sys.dm_os_memory_objects_TSQL
- dm_os_memory_objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_objects dynamic management view
ms.assetid: 5688bcf8-5da9-4ff9-960b-742b671d7096
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ee908ab13e7b6b34665e588b506a7862c8664493
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751007"
---
# <a name="sysdm_os_memory_objects-transact-sql"></a>sys.dm_os_memory_objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回当前由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分配的内存对象。 您可以使用 **sys.dm_os_memory_objects** 分析内存使用情况，并识别可能的内存泄漏。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**memory_object_address**|**varbinary(8)**|内存对象的地址。 不可为 null。|  
|**parent_address**|**varbinary(8)**|父内存对象的地址。 可以为 Null。|  
|**pages_allocated_count**|**int**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。<br /><br /> 由该对象分配的页数。 不可为 null。|  
|**pages_in_bytes**|**bigint**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 此内存对象实例分配的内存量（以字节为单位）。 不可为 null。|  
|**creation_options**|**int**|仅限内部使用。 可以为 Null。|  
|**bytes_used**|**bigint**|仅限内部使用。 可以为 Null。|  
|type |**nvarchar(60)**|内存对象的类型。<br /><br /> 它指示该内存对象所属的特定组件，或指示内存对象的函数。 可以为 Null。|  
|name |**varchar(128)**|仅限内部使用。 可以为 NULL。|  
|**memory_node_id**|**smallint**|该内存对象所用的内存节点的 ID。 不可为 null。|  
|**creation_time**|**datetime**|仅限内部使用。 可以为 Null。|  
|**max_pages_allocated_count**|**int**|**适用范围**： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]。<br /><br /> 由该内存对象分配的最大页数。 不可为 null。|  
|**page_size_in_bytes**|**int**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 由该对象分配的页的大小（以字节为单位）。 不可为 null。|  
|**max_pages_in_bytes**|**bigint**|该内存对象曾经使用的最大内存量。 不可为 null。|  
|**page_allocator_address**|**varbinary(8)**|页分配器的内存地址。 不可为 null。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.dm_os_memory_clerks ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)。|  
|**creation_stack_address**|**varbinary(8)**|仅限内部使用。 可以为 Null。|  
|**sequence_num**|**int**|仅限内部使用。 可以为 Null。|  
|**partition_type**|**int**|**适用于**：[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 分区类型：<br /><br /> 0-非分区内存对象<br /><br /> 1-分区内存对象，当前未分区<br /><br /> 2-分区内存对象，按 NUMA 节点进行分区。 在具有单个 NUMA 节点的环境中，此项等效于1。<br /><br /> 3-分区内存对象，按 CPU 分区。|  
|**contention_factor**|**real**|**适用于**：[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 一个值，该值指定此内存对象的争用，0表示无争用。 每当指定数量的内存分配在该时间段内反射争用时，该值就会更新。 仅适用于线程安全内存对象。|  
|**waiting_tasks_count**|**bigint**|**适用于**：[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 此内存对象上的等待次数。 从此内存对象分配内存时，此计数器将递增。 增量是当前等待访问此内存对象的任务数。 仅适用于线程安全内存对象。 这是最大努力，无需保证正确性。|  
|**exclusive_access_count**|**bigint**|**适用于**：[!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 及更高版本。<br /><br /> 指定以独占方式访问此内存对象的频率。 仅适用于线程安全内存对象。  这是最大努力，无需保证正确性。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
 中尚未实现 **partition_type**、 **contention_factor**、 **waiting_tasks_count** 和 **exclusive_access_count** [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 。  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="remarks"></a>备注  
 内存对象是指多个堆。 它们所提供的分配的粒度比内存分配器所提供的分配的粒度更精细。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件使用内存对象，而不使用内存分配器。 内存对象使用内存分配器的页分配器接口来分配页。 内存对象不使用虚拟内存接口或共享内存接口。 根据分配模式的不同，组件可以创建不同的内存对象类型来分配任意大小的区域。  
  
 内存对象的典型页面大小为 8 KB。 但是，增量内存对象的页大小的范围可以从 512 字节到 8 KB。  
  
> [!NOTE]  
>  页大小不是最大分配。 相反，页大小是受页分配器支持，并且由内存分配器实现的分配粒度。 可从内存对象中请求大于 8 KB 的分配。  
  
## <a name="examples"></a>示例  
 以下示例返回由每种内存对象类型分配的内存量。  
  
```  
SELECT SUM (pages_in_bytes) as 'Bytes Used', type   
FROM sys.dm_os_memory_objects  
GROUP BY type   
ORDER BY 'Bytes Used' DESC;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
  [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_memory_clerks &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)  
  
