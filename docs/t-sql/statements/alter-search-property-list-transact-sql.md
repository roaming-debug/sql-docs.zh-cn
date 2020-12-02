---
description: ALTER SEARCH PROPERTY LIST (Transact-SQL)
title: ALTER SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SEARCH_PROPERTY_TSQL
- ALTER_SEARCH_PROPERTY_LIST_TSQL
- ALTER SEARCH PROPERTY LIST
- ALTER SEARCH PROPERTY
- ALTER_SEARCH_TSQL
- ALTER SEARCH
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], altering
- ALTER SEARCH PROPERTY LIST statement
ms.assetid: 0436e4a8-ca26-4d23-93f1-e31e2a1c8bfb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5cf5d6bcda6670c6cfc9a142be0569bc4bfeaf8e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128050"
---
# <a name="alter-search-property-list-transact-sql"></a>ALTER SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  将指定的搜索属性添加到指定的搜索属性列表中或从列表中删除该属性。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ALTER SEARCH PROPERTY LIST list_name  
{  
   ADD 'property_name'  
     WITH   
      (   
          PROPERTY_SET_GUID = 'property_set_guid'  
        , PROPERTY_INT_ID = property_int_id  
      [ , PROPERTY_DESCRIPTION = 'property_description' ]  
      )  
 | DROP 'property_name'   
}  
;  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *list_name*  
 是正在更改的属性列表的名称。 list_name 是一个标识符。  
  
 若要查看现有属性列表的名称，请使用 [sys.registered_search_property_lists](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md) 目录视图，如下所示：  
  
```sql  
SELECT name FROM sys.registered_search_property_lists;  
```  
  
 ADD  
 将指定的搜索属性添加到 list_name 指定的属性列表中。 已为搜索属性列表注册该属性。 在新添加的属性可用于属性搜索之前，必须重新填充关联的全文检索。 有关详细信息，请参阅 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)。  
  
> [!NOTE]  
>  若要将给定搜索属性添加到搜索属性列表中，必须提供其属性集 GUID (property_set_guid) 和属性整数 ID (property_int_id)。 有关详细信息，请参阅本主题后面的“获取属性集 GUIDS 和标识符”。  
  
 property_name  
 指定要用来标识全文查询中的属性的名称。 property_name 必须唯一标识属性集中的属性。 属性名称可以包含内部空格。 property_name 的最大长度为 256 个字符。 此名称可以是“作者”或“家庭地址”等此类用户友好名称，也可以是 Windows 的属性规范名称，如 System.Author 或 System.Contact.HomeAddress。  
  
 开发人员将需要使用你为 property_name 指定的值在 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 谓词中标识该属性。 因此，在添加属性时，务必指定一个在含义上可表示由指定的属性集 GUID (property_set_guid) 和属性标识符 (property_int_id) 定义属性的值。 有关属性名称的详细信息，请参阅本主题后面的“备注”。  
  
 若要查看当前数据库的搜索属性列表中当前存在的属性的名称，请使用 [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) 目录视图，如下所示：  
  
```sql  
SELECT property_name FROM sys.registered_search_properties;  
```  
  
 PROPERTY_SET_GUID ='property_set_guid'  
 指定属性所属的属性集的标识符。 这是全局唯一标识符 (GUID)。 有关获取此值的信息，请参阅本主题后面的“备注”。  
  
 若要查看当前数据库的搜索属性列表中存在的任何属性的属性集 GUID，请使用 [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) 目录视图，如下所示：  
  
```sql  
SELECT property_set_guid FROM sys.registered_search_properties;  
```  
  
 PROPERTY_INT_ID =property_int_id  
 指定用于在属性集内标识属性的整数。 有关获取此值的信息，请参阅“备注”。  
  
 若要查看当前数据库的搜索属性列表中存在的任何属性的整数标识符，请使用 [sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) 目录视图，如下所示：  
  
```sql  
SELECT property_int_id FROM sys.registered_search_properties;  
```  
  
> [!NOTE]  
>  property_set_guid 和 property_int_id 的给定组合在搜索属性列表内必须唯一。 如果试图添加一个现有组合，则 ALTER SEARCH PROPERTY LIST 操作将失败并且会发出错误。 这意味着，您只能为给定属性定义一个名称。  
  
 PROPERTY_DESCRIPTION ='property_description'  
 指定用户定义的属性说明。 property_description 是一个最多 512 个字符的字符串。 此选项是可选的。  
  
 DROP  
 从 list_name 指定的属性列表中删除指定属性。 删除属性会撤消注册该属性，因此它不再可搜索。  
  
## <a name="remarks"></a>备注  
 每个全文检索只能有一个搜索属性列表。  
  
 若要对给定的搜索属性启用查询，您必须将其添加到全文检索的搜索属性列表中，然后重新填充该索引。  
  
 指定属性时，可按任何顺序将 PROPERTY_SET_GUID、PROPERTY_INT_ID 和 PROPERTY_DESCRIPTION 子句排列为括在括号内的逗号分隔列表，例如：  
  
```sql  
ALTER SEARCH PROPERTY LIST CVitaProperties  
ADD 'System.Author'   
WITH (   
      PROPERTY_DESCRIPTION = 'Author or authors of a given document.',  
      PROPERTY_SET_GUID   = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9',   
      PROPERTY_INT_ID = 4   
      );  
```  
  
> [!NOTE]  
>  此示例使用属性名称 `System.Author`，该名称与 Windows Vista（Windows 规范名称）中引入的规范属性名称的概念相似。  
  
## <a name="obtaining-property-values"></a>获取属性值  
 全文搜索使用搜索属性的属性集 GUID 和属性整数 ID 将其映射为全文检索。 有关如何获取已由 Microsoft 定义的属性的这些值的信息，请参阅 [查找搜索属性的属性集 GUID 和属性整数 ID](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)。 有关由独立软件供应商 (ISV) 定义的属性的信息，请参阅该供应商提供的文档。  
  
## <a name="making-added-properties-searchable"></a>使已添加的属性可搜索  
 将搜索属性添加到搜索属性列表中会注册该属性。 可在 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 查询中立即指定新添加的属性。 但是，对新添加的属性进行的属性范围的全文查询将不返回文档，直到重新填充关联的全文检索为止。 例如，对新添加的属性 new_search_property 执行的属性范围的查询将不返回任何文档，直到重新填充与目标表 (table_name) 关联的全文索引为止：  
  
```sql  
SELECT column_name  
FROM table_name  
WHERE CONTAINS( PROPERTY( column_name, 'new_search_property' ), 
               'contains_search_condition');  
GO   
```  
  
 若要启动完全填充，请使用以下 [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md) 语句：  
  
```sql  
USE database_name;  
GO  
ALTER FULLTEXT INDEX ON table_name START FULL POPULATION;  
GO  
```  
  
> [!NOTE]  
>  由于只有保留在搜索属性列表中的属性可用于全文查询，因此在从属性列表中删除属性后，不需要重新填充。  
  
## <a name="related-references"></a>相关参考  
 **创建属性列表**  
  
-   [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)  
  
 **删除属性列表**  
  
-   [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
 **对全文检索添加或删除属性列表**  
  
-   [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
 **对全文检索运行填充**  
  
-   [ALTER FULLTEXT INDEX (Transact-SQL)](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 需要拥有对属性列表的 CONTROL 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-adding-a-property"></a>A. 添加属性  
 下面的示例将多个属性（`Title`、`Author` 和 `Tags`）添加到名为 `DocumentPropertyList` 的属性列表中。  
  
> [!NOTE]  
>  有关创建 `DocumentPropertyList` 属性列表的示例，请参阅 [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)。  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
   ADD 'Title'   
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 2,   
      PROPERTY_DESCRIPTION = 'System.Title - Title of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Author'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 4,   
      PROPERTY_DESCRIPTION = 'System.Author - Author or authors of the item.' );  
  
ALTER SEARCH PROPERTY LIST DocumentPropertyList   
    ADD 'Tags'  
   WITH ( PROPERTY_SET_GUID = 'F29F85E0-4FF9-1068-AB91-08002B27B3D9', PROPERTY_INT_ID = 5,   
      PROPERTY_DESCRIPTION = 
          'System.Keywords - Set of keywords (also known as tags) assigned to the item.' );  
```  
  
> [!NOTE]  
>  必须先将给定的搜索属性列表与全文检索关联，才能将其用于属性范围的查询。 为此，请使用 [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) 语句并指定 SET SEARCH PROPERTY LIST 子句。  
  
### <a name="b-dropping-a-property"></a>B. 删除属性  
 以下示例从 `Comments` 属性列表中删除 `DocumentPropertyList` 属性。  
  
```sql  
ALTER SEARCH PROPERTY LIST DocumentPropertyList  
DROP 'Comments' ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [查找搜索属性的属性集 GUID 和属性整数 ID](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
