---
description: sys.dm_os_memory_cache_hash_tables (Transact-SQL)
title: sys.dm_os_memory_cache_hash_tables (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.dm_os_memory_cache_hash_tables_TSQL
- sys.dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables
- dm_os_memory_cache_hash_tables_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_cache_hash_tables dynamic management view
ms.assetid: 68b94f35-8f80-4d2b-bcde-7a21934219af
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 433385ec8bdacfca034026ca24a47a9e7efcd9c5
ms.sourcegitcommit: 2991ad5324601c8618739915aec9b184a8a49c74
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97326735"
---
# <a name="sysdm_os_memory_cache_hash_tables-transact-sql"></a>sys.dm_os_memory_cache_hash_tables (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中的每个活动缓存返回一行。  
  
> [!NOTE]  
>  若要从或调用此 [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] ，请使用名称 **sys.dm_pdw_nodes_os_memory_cache_hash_tables**。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**cache_address**|**varbinary(8)**|缓存条目的地址（主键）。 不可为 null。|  
|name|**nvarchar(256)**|缓存的名称。 不可为 null。|  
|type|**nvarchar(60)**|缓存类型。 不可为 null。|  
|**table_level**|**int**|哈希表编号。 某个特定缓存可能有多个对应于不同哈希函数的哈希表。 不可为 null。|  
|**buckets_count**|**int**|哈希表中的存储桶数。 不可为 null。|  
|**buckets_in_use_count**|**int**|当前使用的存储桶数。 不可为 null。|  
|**buckets_min_length**|**int**|存储桶中的最小缓存条目数。 不可为 null。|  
|**buckets_max_length**|**int**|存储桶中的最大缓存条目数。 不可为 null。|  
|**buckets_avg_length**|**int**|每个存储桶中的平均缓存条目数。 不可为 null。|  
|**buckets_max_length_ever**|**int**|自服务器启动以来，哈希存储桶中用于该哈希表的最大已缓存条目数。 不可为 null。|  
|**hits_count**|**bigint**|缓存命中次数。 不可为 null。|  
|**misses_count**|**bigint**|缓存未命中次数。 不可为 null。|  
|**buckets_avg_scan_hit_length**|**int**|在找到搜索项之前，存储桶中已检查条目的平均数。 不可为 null。|  
|**buckets_avg_scan_miss_length**|**int**|在搜索未成功结束之前，存储桶中已检查条目的平均数。 不可为 null。|  
|pdw_node_id|**int**|此分发所在的节点的标识符。<br /><br /> **适用** 于： [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 、 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]|  
  
## <a name="permissions"></a>权限 

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标以及弹性池中的数据库上， `Server admin` `Azure Active Directory admin` 需要或帐户。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   

## <a name="see-also"></a>另请参阅  
 
  [&#40;Transact-sql 的与操作系统相关的动态管理视图 SQL Server&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
  


