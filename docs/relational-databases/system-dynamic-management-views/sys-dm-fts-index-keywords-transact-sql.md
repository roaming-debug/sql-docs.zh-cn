---
description: sys.dm_fts_index_keywords (Transact-SQL)
title: sys.dm_fts_index_keywords (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords
- sys.dm_fts_index_keywords
- sys.dm_fts_index_keywords_TSQL
- dm_fts_index_keywords_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords dynamic management function
- full-text search [SQL Server], viewing keywords
- troubleshooting [SQL Server], full-text search
ms.assetid: fce7b2a1-7e74-4769-86a8-c77c7628decd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 55bd43c5d4704d145618401230be62e59f3cd3c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211208"
---
# <a name="sysdm_fts_index_keywords-transact-sql"></a>sys.dm_fts_index_keywords (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回有关指定表的全文索引内容的信息。  
  
 **sys.dm_fts_index_keywords** 是动态管理功能。  
  
> [!NOTE]  
>  若要查看较低级别的全文检索信息，请在文档级别使用 [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md) 动态管理函数。  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.dm_fts_index_keywords( DB_ID('database_name'), OBJECT_ID('table_name') )  
```  
  
## <a name="arguments"></a>参数  
 db_id ( "*database_name*" )   
 调用 [DB_ID ( # B1 ](../../t-sql/functions/db-id-transact-sql.md) 函数。 此函数接受数据库名称并返回数据库 ID，该 ID **sys.dm_fts_index_keywords** 使用来查找指定的数据库。 如果省略 database_name，则返回当前数据库 ID。  
  
 object_id ( "*table_name*" )   
 调用 [OBJECT_ID ( # B1 ](../../t-sql/functions/object-id-transact-sql.md) 函数。 此函数接受表名，并返回包含要检查的全文索引的表的表 ID。  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**keyword**|**nvarchar(4000)**|全文索引中存储的关键字的十六进制表示形式。<br /><br /> 注意： OxFF 表示特殊字符，该字符指示文件或数据集的结尾。|  
|**display_term**|**nvarchar(4000)**|关键字的可读格式。 这种格式是从十六进制格式派生的。<br /><br /> 注意： OxFF 的 **display_term** 值为 "文件结尾"。|  
|column_id|**int**|从中对当前关键字进行全文索引的列的 ID。|  
|**document_count**|**int**|包含当前字词的文档或行的数目。|  
  
## <a name="remarks"></a>备注  
 **Sys.dm_fts_index_keywords** 返回的信息对于查找以下内容很有用：  
  
-   关键字是否为全文索引的一部分。  
  
-   有多少文档或行包含给定关键字。  
  
-   全文索引中最常见的关键字：  
  
    -   每个 *keyword_value* 的 **document_count** 与 **document_count** 总大小（0xff 的文档数）相比较。  
  
    -   通常，最常见的关键字可能适于声明为非索引字。  
  
> [!NOTE]  
>  与 **sys.dm_fts_index_keywords_by_document** 或 **CONTAINS** 查询返回的计数相比， **sys.dm_fts_index_keywords** 返回的 **document_count** 对于特定文档而言可能不太准确。 这一可能的不精确估计小于 1%。 出现这种导致的原因是，如果 document_id 在索引片段中的多行间连续发生两次，或在同一行中多次出现，则可能会对该计数两次。 若要为特定文档获取更准确的计数，请使用 **sys.dm_fts_index_keywords_by_document** 或 **CONTAINS** 查询。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定服务器角色的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-displaying-high-level-full-text-index-content"></a>A. 显示高级全文索引内容  
 以下示例显示有关 `HumanResources.JobCandidate` 表中高级全文索引内容的信息。  
  
```  
SELECT * FROM sys.dm_fts_index_keywords(db_id('AdventureWorks2012'), object_id('HumanResources.JobCandidate'))  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [全文搜索和语义搜索动态管理视图和函数 &#40;Transact-sql&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [全文搜索](../../relational-databases/search/full-text-search.md)   
 [sys.dm_fts_index_keywords_by_document (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
