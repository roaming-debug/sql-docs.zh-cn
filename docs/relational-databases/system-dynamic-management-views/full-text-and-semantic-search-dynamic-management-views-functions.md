---
description: Full-Text 和语义搜索动态管理视图-函数
title: Full-Text 和语义搜索动态管理视图-函数 |Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- dynamic management objects [SQL Server], full-text search
- full-text search [SQL Server], dynamic management views
ms.assetid: 199dbd5a-29f6-4ef0-8e65-86e32c0aaa3a
author: pmasl
ms.author: pelopes
ms.openlocfilehash: cd9e072fa2c4aafceaad2d4aad57eb9dcc4bb38b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201361"
---
# <a name="full-text-and-semantic-search-dynamic-management-views---functions"></a>Full-Text 和语义搜索动态管理视图-函数
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本节包含以下与全文搜索和语义搜索相关的动态管理视图和函数。  
  
## <a name="full-text-search-dynamic-management-views-and-functions"></a>全文搜索动态管理视图和函数  
 [sys.dm_fts_active_catalogs (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
 返回在服务器上正在进行某些填充活动的全文目录的相关信息。  
  
 [sys.dm_fts_fdhosts](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
 返回有关服务器实例中筛选器后台程序宿主的当前活动的信息。  
  
 [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
 返回有关指定表的全文索引内容的信息。  
  
 [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
 返回有关指定表的文档级全文索引内容的信息。 给定关键字可以出现在几个文档中。  
  
 [sys.dm_fts_index_keywords_by_property](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
 在给定表的全文索引中返回与属性相关的所有内容。 其中包括属于与全文索引关联的搜索属性列表注册的任何属性的所有数据。  
  
 sys.dm_fts_index_keywords_position_by_document  
 返回文档中关键字的位置。  
  
 [sys.dm_fts_index_population](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
 返回有关当前正在进行的全文索引填充的信息。  
  
 [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
 返回有关属于特定内存池的内存缓冲区（作为全文爬网或全文爬网范围的一部分使用）的信息。  
  
 [sys.dm_fts_memory_pools](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
 返回有关可供全文爬网或全文爬网范围的全文收集器组件使用的共享内存池的信息。  
  
 [sys.dm_fts_outstanding_batches](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
 返回有关每个全文索引批次的信息。  
  
 [sys.dm_fts_parser](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)  
 返回将给定断字符、同义词库和非索引字表组合应用于查询字符串输入后生成的最终词汇切分结果。 此输出等效于将指定查询字符串发送到全文引擎后输出的结果。  
  
 [sys.dm_fts_population_ranges](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
 返回有关与当前正在进行的全文索引填充相关的特定范围的信息。  
  
## <a name="semantic-search-dynamic-management-views-and-functions"></a>语义搜索动态管理视图和函数  
 [sys.dm_fts_semantic_similarity_population (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
 为关联有语义索引的每个表中的每个相似性索引返回一行关于文档相似性索引填充状态的信息。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)  
  
