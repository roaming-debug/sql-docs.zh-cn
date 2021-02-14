---
description: sys.dm_os_memory_pools (Transact-SQL)
title: sys.dm_os_memory_pools (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_pools_TSQL
- dm_os_memory_pools
- dm_os_memory_pools_TSQL
- sys.dm_os_memory_pools
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_pools dynamic management view
ms.assetid: 1ef053f3-c6f3-456e-82b6-26e4bd630d46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5ea86721a9a9b0543bf32f0c2aba17ac5be66368
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100339455"
---
# <a name="sysdm_os_memory_pools-transact-sql"></a>sys.dm_os_memory_pools (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中存储的每个对象分别返回一行。 可以使用此视图来监视缓存内存使用情况，并识别错误的缓存行为。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_memory_pools**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**memory_pool_address**|**varbinary(8)**|代表内存池的项的内存地址。 不可为 null。|  
|**pool_id**|**int**|在一组池中的特定池的 ID。 不可为 null。|  
|type |**nvarchar(60)**|对象池的类型。 不可为 null。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.dm_os_memory_clerks ](../../relational-databases/system-dynamic-management-views/sys-dm-os-memory-clerks-transact-sql.md)。|  
|name |**nvarchar(256)**|系统为此内存对象分配的名称。 不可为 null。|  
|**max_free_entries_count**|**bigint**|池可以拥有的最大可用项的个数。 不可为 null。|  
|**free_entries_count**|**bigint**|池中当前可用项的个数。 不可为 null。|  
|**removed_in_all_rounds_count**|**bigint**|自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启动以来从池中删除的项数。 不可为 null。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件有时使用公用池框架来缓存同种、无状态类型的数据。 池框架比缓存框架更简单。 池中的所有项都被视为是等同的。 在内部，池是内存分配器，并且可以用在使用内存分配器的地方。  
  
## <a name="see-also"></a>另请参阅  
 
  [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


