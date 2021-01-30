---
description: 全文搜索和语义搜索存储过程 (Transact-SQL)
title: Full-Text 搜索和语义搜索存储过程 (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], stored procedures
- full-text search [SQL Server], stored procedures
- full-text catalogs [SQL Server], stored procedures
- system stored procedures [SQL Server], full-text search
ms.assetid: 0d185a16-2b16-4958-884f-efe675e2e551
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39e13966219ed8f08c5a5095c505a661e6923a8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165201"
---
# <a name="full-text-search-and-semantic-search-stored-procedures-transact-sql"></a>全文搜索和语义搜索存储过程 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支持下列系统存储过程，这些存储过程用于实现和查询全文索引和语义索引。  
  
## <a name="full-text-search-stored-procedures"></a>全文搜索存储过程  
 [sp_fulltext_catalog](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)  
 创建和删除全文目录，并启动和停止目录的索引操作。 可为每个数据库创建多个全文目录。  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 请改用 " [创建全文目录](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)"、" [更改全文](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)目录" 和 " [删除全文目录](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) "。  
  
 [sp_fulltext_column](../../relational-databases/system-stored-procedures/sp-fulltext-column-transact-sql.md)  
 指定表的某个特定列是否参与全文索引。  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [ALTER 全文索引](../../t-sql/statements/alter-fulltext-index-transact-sql.md) 。  
  
 [sp_fulltext_database](../../relational-databases/system-stored-procedures/sp-fulltext-database-transact-sql.md)  
 对 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更高版本中的全文目录没有影响，支持它只是为了向后兼容。  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)  
 返回文档标识符 (**DocId** s) 和全文键值之间的映射。  
  
 [sp_fulltext_load_thesaurus_file](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)  
 分析和加载与 LCID 对应的更新后的同义词库文件中的数据，并导致重新编译使用此同义词库的全文查询。  
  
 [sp_fulltext_pendingchanges](../../relational-databases/system-stored-procedures/sp-fulltext-pendingchanges-transact-sql.md)  
 针对正在使用更改跟踪的指定表返回未处理的更改，例如挂起的插入、更新和删除。  
  
 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
 更改 SQL Server 全文搜索的服务器属性。  
  
 [sp_fulltext_table](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)  
 标记或取消标记要编制全文索引的表。  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 请改用 " [创建全文索引](../../t-sql/statements/create-fulltext-index-transact-sql.md)"、" [更改全文](../../t-sql/statements/alter-fulltext-index-transact-sql.md)索引" 和 " [删除全文索引](../../t-sql/statements/drop-fulltext-index-transact-sql.md) "。  
  
 [sp_help_fulltext_catalog_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalog-components-transact-sql.md)  
 返回用于当前数据库中所有全文目录的所有组件（筛选器、断字符和协议处理程序）的列表。  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_help_fulltext_catalogs](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
 返回指定的全文目录的 ID、名称、根目录、状态以及全文索引表的数量。  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) 目录视图。  
  
 [sp_help_fulltext_catalogs_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)  
 使用游标返回指定的全文目录的 ID、名称、根目录、状态和全文索引表的数量。  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) 目录视图。  
  
 [sp_help_fulltext_columns](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)  
 返回为全文索引指定的列。  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) 目录视图。  
  
 [sp_help_fulltext_columns_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)  
 使用游标返回为全文索引指派的列。  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) 目录视图。  
  
 [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)  
 返回注册的断字器、筛选器和协议处理程序的信息，以及已经使用指定组件的数据库和全文目录的标识符列表。  
  
 [sp_help_fulltext_tables](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)  
 返回为全文索引注册的表的列表。  
  
 [sp_help_fulltext_tables_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)  
 返回为全文索引注册的表的列表。  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 请改用 [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) 目录视图。  
  
## <a name="semantic-search-stored-procedures"></a>语义搜索存储过程  
 [sp_fulltext_semantic_register_language_statistics_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-register-language-statistics-db-transact-sql.md)  
 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前实例中注册预先填充的语义语言统计数据库。  
  
 [sp_fulltext_semantic_unregister_language_statistics_db (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-unregister-language-statistics-db-transact-sql.md)  
 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的当前实例中撤消注册现有语义语言统计数据库并删除所有关联元数据。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Transact-sql&#41;的全文搜索和语义搜索目录视图 ](../../relational-databases/system-catalog-views/full-text-search-and-semantic-search-catalog-views-transact-sql.md)   
 [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [全文搜索](../../relational-databases/search/full-text-search.md)  
  
  
