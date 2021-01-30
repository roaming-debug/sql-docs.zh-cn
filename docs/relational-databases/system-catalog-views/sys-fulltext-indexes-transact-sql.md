---
description: sys.fulltext_indexes (Transact-SQL)
title: sys.fulltext_indexes (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fulltext_indexes
- fulltext_indexes_TSQL
- sys.fulltext_indexes_TSQL
- sys.fulltext_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_indexes catalog view
- full-text indexes [SQL Server], properties
ms.assetid: 7fc10fdc-370f-4927-bba0-b76108a7508e
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 46b65b9a9662df54a9945adfc78ca4f38d9343d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193875"
---
# <a name="sysfulltext_indexes-transact-sql"></a>sys.fulltext_indexes (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  表对象的每个全文索引各占一行。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|object_id|**int**|此全文索引所属的对象的 ID。|  
|**unique_index_id**|**int**|对应的唯一非全文索引的 ID，该索引用于将全文索引与行关联。|  
|**fulltext_catalog_id**|**int**|全文索引所在的全文目录的 ID。|  
|**is_enabled**|**bit**|1 = 当前已启用全文索引。|  
|**change_tracking_state**|**char(1)**|更改跟踪的状态。<br /><br /> M = 手动<br /><br /> A = 自动<br /><br /> O = 关闭|  
|**change_tracking_state_desc**|**nvarchar(60)**|对更改跟踪状态的说明。<br /><br /> MANUAL<br /><br /> AUTO<br /><br /> OFF|  
|**has_crawl_completed**|**bit**|全文索引完成的上一次爬网（填充）。|  
|**crawl_type**|**char(1)**|当前或上一次爬网的类型。<br /><br /> F = 完全爬网<br /><br /> I = 增量时间戳爬网<br /><br /> U = 基于通知的更新爬网<br /><br /> P = 完全爬网已暂停。|  
|**crawl_type_desc**|**nvarchar(60)**|对当前或上一次爬网类型的说明。<br /><br /> FULL_CRAWL<br /><br /> INCREMENTAL_CRAWL<br /><br /> UPDATE_CRAWL<br /><br /> PAUSED_FULL_CRAWL|  
|**crawl_start_date**|**datetime**|当前或上一次爬网的开始日期。<br /><br /> NULL = 无。|  
|**crawl_end_date**|**datetime**|当前或上一次爬网的结束日期。<br /><br /> NULL = 无。|  
|**incremental_timestamp**|**二进制 (8)**|要用于下一次增量爬网的时间戳值。<br /><br /> NULL = 无。|  
|**stoplist_id**|**int**|与此全文索引关联的 [非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) 的 ID。|  
|**data_space_id**|**int**|此全文索引所在的文件组。|  
|**property_list_id**|**int**|与此全文索引关联的搜索属性列表的 ID。 NULL 表示没有与全文索引关联的搜索属性列表。 若要获取有关此搜索属性列表的详细信息，请使用 [sys.registered_search_property_lists &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md) 目录视图。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="examples"></a>示例  
 以下示例在 `HumanResources.JobCandidate` 示例数据库的 `AdventureWorks2012` 表中使用全文索引。 该示例返回表的对象 ID、搜索属性列表 ID 以及全文索引使用的非索引字表的非索引字表 ID。  
  
> [!NOTE]  
>  有关创建此全文索引的代码示例，请参阅 [创建全文索引 &#40;transact-sql&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)中的 "示例" 部分。  
  
```  
USE AdventureWorks2012;  
GO  
SELECT object_id, property_list_id, stoplist_id FROM sys.fulltext_indexes  
    where object_id = object_id('HumanResources.JobCandidate');   
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.fulltext_index_fragments &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)   
 [sys.fulltext_index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)   
 [sys.fulltext_index_catalog_usages &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [创建和管理全文索引](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [DROP FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [CREATE FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
