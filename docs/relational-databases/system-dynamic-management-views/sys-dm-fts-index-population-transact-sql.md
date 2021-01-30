---
description: sys.dm_fts_index_population (Transact-SQL)
title: sys.dm_fts_index_population (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_population
- dm_fts_index_population
- sys.dm_fts_index_population_TSQL
- dm_fts_index_population_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_population dynamic management view
ms.assetid: 82d1c102-efcc-4b60-9a5e-3eee299bcb2b
author: pmasl
ms.author: pelopes
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0da9010acff84b43fa66f44b6185706c756f96e2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211213"
---
# <a name="sysdm_fts_index_population-transact-sql"></a>sys.dm_fts_index_population (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中当前正在进行的全文索引填充和语义关键字短语填充的信息。  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|database_id|**int**|包含正在填充的全文索引的数据库 ID。|  
|**catalog_id**|**int**|包含此全文索引的全文目录的 ID。|  
|table_id|**int**|其全文索引正在被填充的表的 ID。|  
|**memory_address**|**varbinary(8)**|用于表示活动填充的内部数据结构的内存地址。|  
|**population_type**|**int**|填充类型。 下列类型作之一：<br /><br /> 1 = 完全填充<br /><br /> 2 = 基于时间戳的增量式填充<br /><br /> 3 = 对跟踪的更改进行手动更新<br /><br /> 4 = 对跟踪的更改进行后台更新。|  
|**population_type_description**|**nvarchar(120)**|对填充类型的说明。|  
|**is_clustered_index_scan**|**bit**|指示填充是否涉及对聚集索引的扫描。|  
|**range_count**|**int**|并行执行填充的子范围数。|  
|**completed_range_count**|**int**|处理完成的范围数。|  
|**outstanding_batch_count**|**int**|此填充当前未完成的批处理个数。 有关详细信息，请参阅 [&#40;transact-sql&#41;sys.dm_fts_outstanding_batches ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)。|  
|**status**|**int**|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本。<br /><br /> 此填充的状态。 说明：某些状态是暂时的。 下列类型作之一：<br /><br /> 3 = 正在启动<br /><br /> 5 = 正常处理<br /><br /> 7 = 已停止处理<br /><br /> 例如，正在进行自动合并时将出现此状态。<br /><br /> 11 = 已中止填充<br /><br /> 12 = 处理语义相似性提取|  
|**status_description**|**nvarchar(120)**|对填充状态的说明。|  
|**completion_type**|**int**|有关填充完成的状态。|  
|**completion_type_description**|**nvarchar(120)**|完成类型的说明。|  
|**worker_count**|**int**|此值始终为 0。|  
|**queued_population_type**|**int**|基于跟踪更改的填充类型，此类填充将在当前填充（如果存在）之后发生。|  
|**queued_population_type_description**|**nvarchar(120)**|对随后要进行的填充（如果存在）的说明。 例如，当 CHANGE TRACKING = AUTO 并且正在进行初始完全填充时，此列将显示“自动填充”。|  
|**start_time**|**datetime**|填充开始的时间。|  
|**incremental_timestamp**|**timestamp**|表示完全填充的开始时间戳。 对于所有其他填充类型，该值是表示填充进度的上次提交的检查点。|  
  
## <a name="remarks"></a>备注  
 如果除了全文索引之外还启用了统计语义索引，则关键字短语的语义提取和填充以及文档相似性数据的提取与全文索引同时发生。 文档相似性索引的填充在后面的第二阶段中发生。 有关详细信息，请参阅 [管理和监视语义搜索](../../relational-databases/search/manage-and-monitor-semantic-search.md)。  
  
## <a name="permissions"></a>权限  

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="physical-joins"></a>物理联接  
 ![此动态管理视图的重要联接](../../relational-databases/system-dynamic-management-views/media/join-dm-fts-index-population-1.gif "此动态管理视图的重要联接")  
  
## <a name="relationship-cardinalities"></a>关系基数  
  
|From|功能|关系|  
|----------|--------|------------------|  
|dm_fts_active_catalogs.database_id|dm_fts_index_population.database_id|一对一|  
|dm_fts_active_catalogs.catalog_id|dm_fts_index_population.catalog_id|一对一|  
|dm_fts_population_ranges.parent_memory_address|dm_fts_index_population.memory_address|多对一|  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
  

