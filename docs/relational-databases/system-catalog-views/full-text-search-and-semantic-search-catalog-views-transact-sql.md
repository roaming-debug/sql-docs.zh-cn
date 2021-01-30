---
description: 全文搜索和语义搜索目录视图 (Transact-SQL)
title: Full-Text 搜索和语义搜索目录视图 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], full-text search
- full-text search [SQL Server], catalog views
- full-text indexes [SQL Server], catalog views
ms.assetid: b08ad2fd-e3d8-458f-96f1-678217e0f419
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 90a7bcde4caa40158bf55314c84054bb94a8426f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194613"
---
# <a name="full-text-search-and-semantic-search-catalog-views-transact-sql"></a>全文搜索和语义搜索目录视图 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本节介绍提供有关全文索引和语义索引的信息的目录视图。  
  
## <a name="full-text-search-catalog-views"></a>全文搜索目录视图  
 [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)  
 每个全文目录对应一行。  
  
 [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md)  
 为可用于全文索引操作的每个文档类型返回一行。 每行表示在 SQL Server 的实例中注册的 **IFilter** 接口。  
  
 [sys.fulltext_index_catalog_usages](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)  
 对于全文索引引用的每个全文目录，返回与其对应的一行。  
  
 [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)  
 对构成全文索引的每列都包含一行。  
  
 [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)  
 每个全文索引片断在每个包含全文索引的表中各占一行。  
  
 [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)  
 表对象的每个全文索引各占一行。  
  
 [sys.fulltext_languages](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)  
 在 SQL Server 中注册了断字符的每种语言各占一行。 每一行都显示了语言的 LCID 和名称。  
  
 [sys.fulltext_stoplists](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)  
 对于数据库中的每个全文非索引字表，均存在对应的一行。  
  
 [sys.fulltext_stopwords](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)  
 对于数据库中所有非索引字表中的每个非索引字，均包含对应的一行。  
  
 [sys.fulltext_system_stopwords](../../relational-databases/system-catalog-views/sys-fulltext-system-stopwords-transact-sql.md)  
 提供对系统非索引字表的访问。  
  
 [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)  
 当前数据库中的任何搜索属性列表包含的每个搜索属性各占一行。  
  
 [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
 当前数据库中的每个搜索属性列表各占一行。  
  
## <a name="semantic-search-catalog-views"></a>语义搜索目录视图  
 [sys.fulltext_semantic_language_statistics_database (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md)  
 返回有关在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前实例上安装的语义语言统计数据库的行。  
  
 [sys.fulltext_semantic_languages (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md)  
 为在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例中注册统计模型的每种语言返回一行。 注册语言模型后，则支持对该语言进行语义索引。  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;的系统视图 &#40;](../../t-sql/language-reference.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
