---
description: 'sys.dm_fts_index_keywords_position_by_document (Transact-sql) '
title: sys.dm_fts_index_keywords_position_by_document (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document
- sys.dm_fts_index_keywords_position_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords_position_by_document dynamic management view
ms.assetid: 0d70184f-baa2-411b-a32d-a4c5af890edd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 099574becb70e3812864c4acc7dc0a395f3169b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211232"
---
# <a name="sysdm_fts_index_keywords_position_by_document-transact-sql"></a>sys.dm_fts_index_keywords_position_by_document (Transact-sql) 
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回索引文档中的关键字位置信息。  
  
## <a name="syntax"></a>语法  
  
```  
sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>参数  
 db_id ( "*database_name*" )   
 调用 [DB_ID ( # B1 ](../../t-sql/functions/db-id-transact-sql.md) 函数。 此函数接受数据库名称并返回数据库 ID，该 ID sys.dm_fts_index_keywords_position_by_document 使用来查找指定的数据库。  
  
 object_id ( "*table_name*" )   
 调用 [OBJECT_ID ( # B1 ](../../t-sql/functions/object-id-transact-sql.md) 函数。 此函数接受表名，并返回包含要检查的全文索引的表的表 ID。  
  
## <a name="table-returned"></a>返回的表  
  
|列|数据类型|说明|  
|------------|---------------|-----------------|  
|关键字 (keyword)|**varbinary(128)**|表示关键字的二进制字符串。|  
|display_term|**nvarchar(4000)**|关键字的可读格式。 此格式是从全文索引中存储的内部格式派生的。|  
|column_id|**int**|从中对当前关键字进行全文索引的列的 ID。|  
|document_id|**bigint**|从中对当前字词进行全文索引的文档或行的 ID。 此 ID 对应于该文档或行的全文键值。|  
|position|**int**|关键字在文档中的位置。|  
  
## <a name="remarks"></a>备注  
 使用 DMV 标识索引文档中的索引字的位置。 当 **sys.dm_fts_index_keywords_by_document** 指示全文索引中的单词时，此 DMV 可用于解决问题，但当使用这些单词运行查询时，不会返回文档。  
  
## <a name="permissions"></a>权限  
 要求具有全文索引涵盖的列的 SELECT 权限以及 CREATE FULLTEXT CATALOG 权限。  
  
## <a name="examples"></a>示例  
 下面的示例从示例数据库的表的全文索引中返回关键字 `Production.Document` `AdventureWorks` 。  
  
```  
USE AdventureWorks2012;  
GO   
  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
);   
GO  
```  
  
 可以在其他 columns_id 上添加谓词，如以下示例查询中所示，进一步隔离位置。  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
)  
WHERE document_id = 7 AND display_term = 'performance';  
```  
  
## <a name="see-also"></a>另请参阅  
 [全文搜索](../../relational-databases/search/full-text-search.md)   
 [提高 Full-Text 索引的性能](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [&#40;Transact-sql 的全文搜索和语义搜索函数&#41;](../../relational-databases/system-functions/full-text-search-and-semantic-search-functions-transact-sql.md)   
 [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [&#40;Transact-sql&#41;的全文搜索和语义搜索存储过程 ](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [sys.dm_fts_index_keywords_by_document (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
