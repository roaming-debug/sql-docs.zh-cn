---
description: sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)
title: sys.dm_db_xtp_nonclustered_index_stats (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- dm_db_xtp_nonclustered_index_stats_TSQL
- dm_db_xtp_nonclustered_index_stats
- sys.dm_db_xtp_nonclustered_index_stats_TSQL
- sys.dm_db_xtp_nonclustered_index_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_nonclustered_index_stats
ms.assetid: d55ba31c-296c-419b-9c4b-c126e0a3d156
author: markingmyname
ms.author: maghan
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3688330769cbac60841f29d98efb5219cfc965a3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474938"
---
# <a name="sysdm_db_xtp_nonclustered_index_stats-transact-sql"></a>sys.dm_db_xtp_nonclustered_index_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  sys.dm_db_xtp_nonclustered_index_stats 包含有关对内存优化表中的非聚集索引执行操作的统计信息。 sys.dm_db_xtp_nonclustered_index_stats 针对当前数据库中内存优化表上的每个非聚集索引都包含一行。  
  
 在创建内存中索引结构时，会收集 sys.dm_db_xtp_nonclustered_index_stats 中反映的统计信息。 数据库重新启动时，会重新创建内存中索引结构。  
  
 使用 sys.dm_db_xtp_nonclustered_index_stats 可了解和监视 DML 操作过程中以及数据库进入联机状态时的索引活动。 重新启动具有内存优化表的数据库时，通过一次向内存插入一行来生成索引。 页拆分、合并和整合的计数可以帮助您了解在数据库进入联机状态时为生成索引而进行的工作。 还可以在一系列 DML 操作之前以及之后查看这些计数。  
  
 大量重试表示存在并发问题；请致电 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 支持部门。  
  
 有关内存优化的非聚集索引的详细信息，请参阅 page 17 [In-Memory OLTP 内部概述 SQL Server](https://t.co/T6zToWc6y6)。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|对象的 ID。|  
|xtp_object_id|**bigint**|内存优化表的 ID。|  
|index_id|**int**|索引的 ID。|  
|delta_pages|**bigint**|树中此索引的增量页总数。|  
|internal_pages|**bigint**|供内部使用。 树中此索引的内部页总数。|  
|leaf_pages|**bigint**|树中此索引的叶级页总数。|  
|outstanding_retired_nodes|**bigint**|供内部使用。 内部结构中此索引的节点总数。|  
|page_update_count|**bigint**|对索引中的页进行更新的累计操作数。|  
|page_update_retry_count|**bigint**|对索引中的页进行更新的累计操作重试次数。|  
|page_consolidation_count|**bigint**|索引中的累计页合并数。|  
|page_consolidation_retry_count|**bigint**|累计页合并操作重试次数。|  
|page_split_count|**bigint**|索引中的累计页拆分操作数。|  
|page_split_retry_count|**bigint**|累计页拆分操作重试次数。|  
|key_split_count|**bigint**|索引中的累计键拆分数。|  
|key_split_retry_count|**bigint**|累计键拆分操作重试次数。|  
|page_merge_count|**bigint**|索引中的累计页合并操作数。|  
|page_merge_retry_count|**bigint**|累计页合并操作重试次数。|  
|key_merge_count|**bigint**|索引中的累计键合并操作数。|  
|key_merge_retry_count|**bigint**|累计键合并操作重试次数。|  
  
## <a name="permissions"></a>权限  
 要求对当前数据库拥有 VIEW DATABASE STATE 权限。  
  
## <a name="see-also"></a>另请参阅  
 [内存优化表动态管理视图 &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
