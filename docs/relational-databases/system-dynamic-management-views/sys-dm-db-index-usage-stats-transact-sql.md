---
description: sys.dm_db_index_usage_stats (Transact-SQL)
title: sys.dm_db_index_usage_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_index_usage_stats_TSQL
- sys.dm_db_index_usage_stats
- sys.dm_db_index_usage_stats_TSQL
- dm_db_index_usage_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_index_usage_stats dynamic management view
ms.assetid: d06a001f-0f72-4679-bc2f-66fff7958b86
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23907a0b37bbc4804b9972d830c0c64f0f0d1712
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171289"
---
# <a name="sysdm_db_index_usage_stats-transact-sql"></a>sys.dm_db_index_usage_stats (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  返回不同类型索引操作的计数以及上次执行每种操作的时间。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，动态管理视图不能公开将影响数据库包含的信息，也不能公开有关用户可以访问的其他数据库的信息。 为了避免公开此信息，每个包含不属于所连接的租户的数据的行都将被筛选掉。  
  
> [!NOTE]  
>  **sys.dm_db_index_usage_stats** 不返回有关内存优化索引或空间索引的信息。 有关内存优化索引使用的信息，请参阅 [&#40;transact-sql&#41;sys.dm_db_xtp_index_stats ](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-index-stats-transact-sql.md)。  
  
> [!NOTE]  
>  若要从或调用此视图 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用 **sys.dm_pdw_nodes_db_index_usage_stats**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**smallint**|在其中定义表或视图的数据库的 ID。|  
|object_id|**int**|为其定义索引的表或视图的 ID。|  
|index_id|**int**|索引的 ID。|  
|**user_seeks**|**bigint**|通过用户查询执行的搜索次数。|  
|**user_scans**|**bigint**|用户查询执行的扫描次数，未使用 "seek" 谓词。|  
|**user_lookups**|**bigint**|由用户查询执行的书签查找次数。|  
|**user_updates**|**bigint**|通过用户查询执行的更新次数。 这包括 Insert、Delete 和 Update，它们表示所做的操作的数目不受影响的实际行数。 例如，如果在一个语句中删除1000行，则此计数将递增1|  
|**last_user_seek**|**datetime**|用户上次执行搜索的时间。|  
|**last_user_scan**|**datetime**|用户上次执行扫描的时间。|  
|**last_user_lookup**|**datetime**|用户上次执行查找的时间。|  
|**last_user_update**|**datetime**|用户上次执行更新的时间。|  
|**system_seeks**|**bigint**|通过系统查询执行的搜索次数。|  
|**system_scans**|**bigint**|通过系统查询执行的扫描次数。|  
|**system_lookups**|**bigint**|通过系统查询执行的查找次数。|  
|**system_updates**|**bigint**|通过系统查询执行的更新次数。|  
|**last_system_seek**|**datetime**|系统上次执行搜索的时间。|  
|**last_system_scan**|**datetime**|系统上次执行扫描的时间。|  
|**last_system_lookup**|**datetime**|系统上次执行查找的时间。|  
|**last_system_update**|**datetime**|系统上次执行更新的时间。|  
|pdw_node_id|**int**|**适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> 此分发所在的节点的标识符。|  
  
## <a name="remarks"></a>备注  
 由一个查询执行对指定索引所进行的每个单独的搜索、扫描、查找或更新都被计为对该索引的一次使用，并使此视图中的相应计数器递增。 对于由用户提交的查询所引发的操作以及由内部生成的查询所引发的操作（例如为收集统计信息而进行的扫描），都将报告相应的信息。  
  
 **user_updates** 计数器指示由基础表或视图上的插入、更新或删除操作所引起的索引维护级别。 可以使用此视图确定应用程序极少使用的索引。 还可以使用此视图确定引发维护开销的索引。 您可能要删除引发维护开销但不用于查询或只是偶尔用于查询的索引。  
  
 只要启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER) 服务，计数器就初始化为空。 而且，当分离或关闭数据库时（例如，由于 AUTO_CLOSE 设置为 ON），便会删除与该数据库关联的所有行。  
  
 使用索引时，如果某行并未针对该索引而存在，则将该行添加到 **sys.dm_db_index_usage_stats** 中。 当添加该行时，它的计数器会初始设置为零。  
  
 升级到 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 、 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或时 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] ，sys.dm_db_index_usage_stats 中的条目将被删除。 从开始 [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] ，项将保留在之前 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 。  
  
## <a name="permissions"></a>权限  
在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。  
  
## <a name="see-also"></a>另请参阅  

 [与索引相关的动态管理视图和函数 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/index-related-dynamic-management-views-and-functions-transact-sql.md)   
 [sys.dm_db_index_physical_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-physical-stats-transact-sql.md)   
 [sys.dm_db_index_operational_stats (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-index-operational-stats-transact-sql.md)   
 [sys.indexes (Transact-SQL)](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [监视和优化性能](../../relational-databases/performance/monitor-and-tune-for-performance.md)  
  
  


