---
description: sys.dm_fts_index_keywords_by_property (Transact-SQL)
title: sys.dm_fts_index_keywords_by_property (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords_by_property
- dm_fts_index_keywords_by_property_TSQL
- sys.dm_fts_index_keywords_by_property
- sys.dm_fts_index_keywords_by_property_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], troubleshooting
- search property lists [SQL Server], viewing keywords by property
- full-text search [SQL Server], viewing keywords
- sys.dm_fts_index_keywords_by_property dynamic management view
ms.assetid: fa41e052-a79a-4194-9b1a-2885f7828500
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 9052c7d64ab78c2e6eca1388ed08903365b6e129
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196258"
---
# <a name="sysdm_fts_index_keywords_by_property-transact-sql"></a>sys.dm_fts_index_keywords_by_property (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在给定表的全文索引中返回与属性相关的所有内容。 其中包括属于与全文索引关联的搜索属性列表注册的任何属性的所有数据。  
  
 sys.dm_fts_index_keywords_by_property 是一种动态管理功能，通过该功能，您可以查看 Ifilter 在索引时发出了哪些已注册的属性，以及每个索引文档中每个属性的确切内容。  
  
 **查看所有文档级内容（包括属性相关的内容）**  
  
-   [sys.dm_fts_index_keywords_by_document (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
 **查看高级全文索引信息**  
  
-   [sys.dm_fts_index_keywords (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
> [!NOTE]  
>  有关搜索属性列表的信息，请参阅 [用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)。  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.dm_fts_index_keywords_by_property  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>参数  
 db_id ( "*database_name*" )   
 调用 [DB_ID ( # B1 ](../../t-sql/functions/db-id-transact-sql.md) 函数。 此函数接受数据库名称并返回数据库 ID，该 ID sys.dm_fts_index_keywords_by_property 使用来查找指定的数据库。 如果省略 database_name，则返回当前数据库 ID。  
  
 object_id ( "*table_name*" )   
 调用 [OBJECT_ID ( # B1 ](../../t-sql/functions/object-id-transact-sql.md) 函数。 此函数接受表名，并返回包含要检查的全文索引的表的表 ID。  
  
## <a name="table-returned"></a>返回的表  
  
|列|数据类型|说明|  
|------------|---------------|-----------------|  
|关键字 (keyword)|**nvarchar(4000)**|存储在全文索引中的关键字的十六进制表示形式。<br /><br /> 注意： OxFF 表示特殊字符，该字符指示文件或数据集的结尾。|  
|display_term|**nvarchar(4000)**|关键字的可读格式。 此格式是从全文索引中存储的内部格式派生的。<br /><br /> 注意： OxFF 表示特殊字符，该字符指示文件或数据集的结尾。|  
|column_id|**int**|从中对当前关键字进行全文索引的列的 ID。|  
|document_id|**int**|从中对当前字词进行全文索引的文档或行的 ID。 此 ID 对应于该文档或行的全文键值。|  
|property_id|**int**|在 OBJECT_ID ( "*table_name*" ) 参数中指定的表的全文索引内搜索属性的内部属性 ID。<br /><br /> 在将给定属性添加到搜索属性列表时，全文引擎会注册该属性并为其指定特定于该属性列表的内部属性 ID。 内部属性 ID 是一个整数，对给定的搜索属性列表唯一。 如果为多个搜索属性列表注册给定属性，则可能会为每个搜索属性列表指定不同的内部属性 ID。<br /><br /> 注意：内部属性 ID 不同于将属性添加到搜索属性列表时指定的属性整数标识符。 有关详细信息，请参阅 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)。<br /><br /> 查看 property_id 和属性名称之间的关联：<br />                    [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)|  
  
## <a name="remarks"></a>备注  
 此动态管理视图可以回答诸如以下的各个问题：  
  
-   哪些内容存储在给定 DocID 的给定属性中？  
  
-   给定属性在索引文档中的几率有多大？  
  
-   哪些文档确实包含给定属性？ 如果查询给定的搜索属性时未返回您要查找的文档，则此视图很有用。  
  
 如果全文键列是建议的整数数据类型，则 document_id 直接映射到基表中的全文键值。  
  
 相反，如果全文键列使用非整数数据类型，document_id 并不表示基表中的全文键。 在这种情况下，若要标识 dm_fts_index_keywords_by_property 返回的基表中的行，需要使用 [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)返回的结果来联接此视图。 在联接它们之前，您必须将存储过程的输出存储在临时表中。 然后，可以使用此存储过程返回的 DocId 列联接 dm_fts_index_keywords_by_property 的 document_id 列。 请注意， **时间戳** 列无法在插入时接收值，因为它们是由自动生成的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 因此， **timestamp** 列必须转换为 **varbinary (8)** 列。 下面的示例说明了这些步骤。 在此示例中， *table_id* 是表的 id， *database_name* 是数据库的名称， *table_name* 是表的名称。  
  
```  
USE database_name;  
GO  
CREATE TABLE #MyTempTable   
   (  
      docid INT PRIMARY KEY ,  
      [key] INT NOT NULL  
   );  
DECLARE @db_id int = db_id(N'database_name');  
DECLARE @table_id int = OBJECT_ID(N'table_name');  
INSERT INTO #MyTempTable EXEC sp_fulltext_keymappings @table_id;  
SELECT * FROM sys.dm_fts_index_keywords_by_property   
   ( @db_id, @table_id ) kbd  
   INNER JOIN #MyTempTable tt ON tt.[docid]=kbd.document_id;  
GO  
  
```  
  
## <a name="permissions"></a>权限  
 要求具有全文索引涵盖的列的 SELECT 权限以及 CREATE FULLTEXT CATALOG 权限。  
  
## <a name="examples"></a>示例  
 以下示例从 `Author` 示例数据库的 `Production.Document` 表的全文索引中的 `AdventureWorks` 属性中返回关键字。 该示例使用 `KWBPOP` **sys.dm_fts_index_keywords_by_property** 返回的表的别名。 该示例使用内部联接来合并 [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) 和 [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)中的列。  
  
```  
-- Once the full-text index is configured to support property searching  
-- on the Author property, return any keywords indexed for this property.  
USE AdventureWorks2012;  
GO   
SELECT KWBPOP.* FROM   
   sys.dm_fts_index_keywords_by_property( DB_ID(),   
   object_id('Production.Document') ) AS KWBPOP  
   INNER JOIN  
      sys.registered_search_properties AS RSP ON(   
         (KWBPOP.property_id = RSP.property_id)   
         AND (RSP.property_name = 'Author') )  
   INNER JOIN  
      sys.fulltext_indexes AS FTI ON(   
         (FTI.[object_id] = object_id('Production.Document'))   
         AND (RSP.property_list_id = FTI.property_list_id) );  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [全文搜索](../../relational-databases/search/full-text-search.md)   
 [提高 Full-Text 索引的性能](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [sp_fulltext_keymappings &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)   
 [sys.dm_fts_index_keywords &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)   
 [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
