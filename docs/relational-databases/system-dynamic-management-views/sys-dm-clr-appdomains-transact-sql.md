---
description: sys.dm_clr_appdomains (Transact-SQL)
title: sys.dm_clr_appdomains (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_clr_appdomains
- sys.dm_clr_appdomains
- dm_clr_appdomains_TSQL
- sys.dm_clr_appdomains_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_appdomains dynamic management dynamic management view
ms.assetid: 9fe0d4fd-950a-4274-a493-85e776278045
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 7579a0648bbc5eee90512bb70dfd667728b56834
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208105"
---
# <a name="sysdm_clr_appdomains-transact-sql"></a>sys.dm_clr_appdomains (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  为服务器中的每个应用程序域返回一行。 应用程序域 (**AppDomain**) 是公共语言运行时中的一个构造， [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] (CLR) ，它是应用程序的隔离单元。 您可以使用此视图来了解和排查在中执行的 CLR 集成对象的问题 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 有多种类型的 CLR 集成托管数据库对象。 有关这些对象的常规信息，请参阅 [通过公共语言运行时生成数据库对象 (CLR) 集成](../../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md)。 每次执行这些对象时，将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 创建一个 **AppDomain** ，以便它能够加载和执行所需的代码。 **Appdomain** 的隔离级别为每个所有者每个数据库一个 **appdomain** 。 也就是说，用户拥有的所有 CLR 对象始终在每个数据库的同一 **AppDomain** 中执行 (如果用户将 clr 数据库对象注册到不同的数据库中，则 clr 数据库对象将在不同的应用程序域中运行) 。 代码完成执行后， **AppDomain** 不会销毁。 而是缓存在内存中以备将来执行， 从而可以提高性能。  
  
 有关详细信息，请参阅 [应用程序域](../../sql-server/what-s-new-in-sql-server-2016.md)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**appdomain_address**|**varbinary(8)**|**AppDomain** 的地址。 用户拥有的所有托管数据库对象始终加载到相同的 **AppDomain** 中。 您可以使用此列查找 **sys.dm_clr_loaded_assemblies****中当前加载的所有** 程序集。|  
|**appdomain_id**|**int**|**AppDomain** 的 ID。 每个 **AppDomain** 都具有唯一的 ID。|  
|**appdomain_name**|**varchar (386)**|由分配的 **AppDomain** 的名称 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|**creation_time**|**datetime**|创建 **AppDomain** 的时间。 由于 **appdomain** 已缓存并可重复使用以提高性能，因此 **creation_time** 不一定是代码的执行时间。|  
|**db_id**|**int**|在其中创建此 **AppDomain** 的数据库的 ID。 存储在两个不同数据库中的代码不能共享一个 **AppDomain**。|  
|**user_id**|**int**|其对象可以在此 **AppDomain** 中执行的用户的 ID。|  
|**state**|**nvarchar(128)**|**AppDomain** 的当前状态的描述符。 AppDomain 可以处于从创建到删除的不同状态中。 有关详细信息，请参阅本主题的“备注”部分。|  
|**strong_refcount**|**int**|对此 **AppDomain** 的强引用数。 这反映了使用此 **AppDomain** 的当前正在执行的批处理的数目。 请注意，此视图的执行将创建一个 **强引用计数**;即使当前没有正在执行的代码， **strong_refcount** 的值也为1。|  
|**weak_refcount**|**int**|对此 **AppDomain** 的弱引用的数目。 这表示将缓存 **AppDomain** 中的对象数。 当你执行托管数据库对象时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 会将其缓存在 **AppDomain** 内以备将来重复使用。 从而可以提高性能。|  
|**cost**|**int**|**AppDomain** 的成本。 成本越高，在内存压力下卸载该 **AppDomain** 的可能性就越大。 开销通常取决于重新创建此 **AppDomain** 所需的内存量。|  
|**value**|**int**|**AppDomain** 的值。 值越低，则在内存压力下卸载该 **AppDomain** 的可能性就越大。 值通常取决于使用此 **AppDomain** 的连接数或批处理数。|  
|**total_processor_time_ms**|**bigint**|自进程启动后在当前应用程序域中执行的过程中所有线程所用的处理器总时间（毫秒）。 这等效于 **MonitoringTotalProcessorTime**。|  
|**total_allocated_memory_kb**|**bigint**|应用程序域自创建以来进行的所有内存分配的总大小 (KB)（不减去已收集的内存大小）。 这等效于 **MonitoringTotalAllocatedMemorySize**。|  
|**survived_memory_kb**|**bigint**|在已知当前应用程序域所引用的最近一次全面的阻塞收集之后仍剩余的字节数 (KB)。 这等效于 **MonitoringSurvivedMemorySize**。|  
  
## <a name="remarks"></a>备注  
 Appdomain_address 和 **dm_clr_loaded_assemblies** 之间存在一对一的关系。 **dm_clr_appdomains**  
  
 下表列出了可能的 **状态值** 、它们的说明以及它们在 **AppDomain** 生命周期中发生的时间。 您可以使用此信息来跟踪 **appdomain** 的生命周期，并监视是否有可疑或重复的 **AppDomain** 实例卸载，而不必分析 Windows 事件日志。  
  
## <a name="appdomain-initialization"></a>AppDomain 初始化  
  
|状态|说明|  
|-----------|-----------------|  
|E_APPDOMAIN_CREATING|正在创建 **AppDomain** 。|  
  
## <a name="appdomain-usage"></a>AppDomain 使用情况  
  
|状态|说明|  
|-----------|-----------------|  
|E_APPDOMAIN_SHARED|运行时 **AppDomain** 已准备就绪，可供多个用户使用。|  
|E_APPDOMAIN_SINGLEUSER|**AppDomain** 可以在 DDL 操作中使用。 此行为不同于 E_APPDOMAIN_SHARED，在后者中，共享的 AppDomain 用于 CLR 集成执行，而非 DDL 操作。 此类 AppDomain 独立于其他并发操作。|  
|E_APPDOMAIN_DOOMED|已计划卸载 **AppDomain** ，但当前正在其中执行线程。|  
  
## <a name="appdomain-cleanup"></a>AppDomain 的清除  
  
|状态|说明|  
|-----------|-----------------|  
|E_APPDOMAIN_UNLOADING|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已请求 CLR 卸载 **AppDomain**，通常是因为包含托管数据库对象的程序集已更改或删除。|  
|E_APPDOMAIN_UNLOADED|CLR 已卸载 **AppDomain**。 这通常是由于 **ThreadAbort**、 **OutOfMemory** 或用户代码中未经处理的异常导致的升级过程的结果。|  
|E_APPDOMAIN_ENQUEUE_DESTROY|已在 CLR 中卸载了 **AppDomain** ，并将其设置为被销毁 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|E_APPDOMAIN_DESTROY|**AppDomain** 正在被销毁 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|E_APPDOMAIN_ZOMBIE|**Appdomain** 已由销毁 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ; 但是，并非所有对 **appdomain** 的引用都已清除。|  
  
## <a name="permissions"></a>权限  
 需要对数据库具有 VIEW SERVER STATE 权限。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何查看给定程序集的 **AppDomain** 的详细信息：  
  
```  
select appdomain_id, creation_time, db_id, user_id, state  
from sys.dm_clr_appdomains a  
where appdomain_address =   
(select appdomain_address   
 from sys.dm_clr_loaded_assemblies  
   where assembly_id = 500);  
```  
  
 下面的示例演示如何查看给定 **AppDomain** 中的所有程序集：  
  
```  
select a.name, a.assembly_id, a.permission_set_desc, a.is_visible, a.create_date, l.load_time   
from sys.dm_clr_loaded_assemblies as l   
inner join sys.assemblies as a  
on l.assembly_id = a.assembly_id  
where l.appdomain_address =   
(select appdomain_address   
from sys.dm_clr_appdomains  
where appdomain_id = 15);  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.dm_clr_loaded_assemblies &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-clr-loaded-assemblies-transact-sql.md)   
 [与公共语言运行时相关的动态管理视图 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
