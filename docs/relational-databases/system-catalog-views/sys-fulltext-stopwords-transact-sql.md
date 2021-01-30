---
description: sys.fulltext_stopwords (Transact-SQL)
title: sys.fulltext_stopwords (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fulltext_stopwords_TSQL
- fulltext_stopwords
- sys.fulltext_stopwords
- sys.fulltext_stopwords_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- full-text search [SQL Server], stopwords
- sys.fulltext_stopwords catalog view
- stopwords [full-text search]
ms.assetid: 79787bb7-d729-448e-b56a-0a467bbb304f
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d6b2cc2feafb82ac1b0338807b2d601151c6c255
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191482"
---
# <a name="sysfulltext_stopwords-transact-sql"></a>sys.fulltext_stopwords (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  对于数据库中所有非索引字表中的每个非索引字，均包含对应的一行。  
 
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**stoplist_id**|**int**|**stopword** 所属非索引字表的 ID。 此 ID 在数据库中是唯一的。|  
|**非索引字**|**nvarchar (64)**|可视为非索引字匹配项的字词。|  
|**language**|**sysname**|[Sys.fulltext_languages](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)中的别名的值对应于区域设置标识符的值 (**Lcid**) ，或者是数字 LCID 的字符串表示形式。|  
|**language_id**|**int**|用于断字的 LCID。|  
  
## <a name="permissions"></a>权限  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>另请参阅  
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [对象目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)   
 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_system_stopwords (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-system-stopwords-transact-sql.md)  
  
  
