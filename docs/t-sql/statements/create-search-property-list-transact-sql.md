---
description: CREATE SEARCH PROPERTY LIST (Transact-SQL)
title: CREATE SEARCH PROPERTY LIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SEARCH_PROPERTY_LIST_TSQL
- CREATE SEARCH
- CREATE_SEARCH_TSQL
- CREATE_SEARCH_PROPERTY_TSQL
- CREATE SEARCH PROPERTY
- CREATE SEARCH PROPERTY LIST
- sql13.swb.spl.newsearchpropertylist.f1
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- search property lists [SQL Server], creating
- CREATE SEARCH PROPERTY LIST statement
ms.assetid: 5440cbb8-3403-4d27-a2f9-8e1f5a1bc12b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: afd220a9893f77c79d5677af135c9a640611abf2
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091979"
---
# <a name="create-search-property-list-transact-sql"></a>CREATE SEARCH PROPERTY LIST (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  创建新的搜索属性列表。 搜索属性列表用于指定要在全文检索中包括的一个或多个搜索属性。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
CREATE SEARCH PROPERTY LIST new_list_name  
   [ FROM [ database_name. ] source_list_name ]  
   [ AUTHORIZATION owner_name ]  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 new_list_name  
 新搜索属性列表的名称。 new_list_name 是最多包含 128 个字符的标识符。 new_list_name 在当前数据库的所有属性列表中必须是唯一的，并且符合标识符规则。 创建全文检索时将使用 new_list_name。  
  
 *database_name*  
 source_list_name 指定的属性列表所在的数据库的名称。 如果未指定，则 database_name 默认为当前数据库。  
  
 database_name 须指定现有数据库的名称。 当前连接的登录名必须与 database_name 所指定的数据库中的现有用户 ID 关联。 还必须拥有针对数据库的必需[权限](#Permissions)。  
  
 source_list_name  
 指定通过从 database_name 中复制现有属性列表来创建新属性列表。 如果 source_list_name 不存在，CREATE SEARCH PROPERTY LIST 失败并出现错误。 source_list_name 中的搜索属性由 new_list_name 继承。  
  
 AUTHORIZATION owner_name   
 指定要拥有属性列表的用户或角色的名称。 owner_name 必须是当前用户所属的角色的名称，或当前用户必须具有对 owner_name 的 IMPERSONATE 权限。 如果未指定，则所有权授予当前用户。  
  
> [!NOTE]  
>  可通过 [ALTER AUTHORIZATION](../../t-sql/statements/alter-authorization-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句更改所有者。  
  
## <a name="remarks"></a>备注  
  
> [!NOTE]  
>  有关属性列表的一般信息，请参阅[使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)。  
  
 默认情况下，新的搜索属性列表为空，必须手动对其进行修改，以添加一个或多个搜索属性。 或者，您也可以复制现有的搜索属性列表。 在此情况下，新列表将继承其源的搜索属性，但您可以修改新列表，以添加或删除搜索属性。 在下一次完全填充时搜索属性列表中的所有属性都将包括在全文检索中。  
  
 在以下任一情况下，CREATE SEARCH PROPERTY LIST 语句均会失败：  
  
-   database_name 指定的数据库不存在。  
  
-   source_list_name 指定的列表不存在。  
  
-   没有适当的权限。  
  
 **在列表中添加或删除属性**  
  
-   [ALTER SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/alter-search-property-list-transact-sql.md)  
  
-   **删除属性列表**  
  
-   [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)  
  
##  <a name="permissions"></a><a name="Permissions"></a> 权限  
 要求在当前数据库中拥有 CREATE FULLTEXT CATALOG 权限，并对从中复制源属性列表的任何数据库拥有 REFERENCES 权限。  
  
> [!NOTE]  
>  若要将列表与全文检索关联，需要拥有 REFERENCE 权限。 若要添加和移除属性或删除列表，需要拥有 CONTROL 权限。 属性列表所有者可以授予对列表的 REFERENCE 或 CONTROL 权限。 拥有 CONTROL 权限的用户还可以向其他用户授予 REFERENCES 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-an-empty-property-list-and-associating-it-with-an-index"></a>A. 创建空属性列表并将其与索引关联  
 以下示例创建名为 `DocumentPropertyList` 的新搜索属性列表。 该示例然后使用 [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) 语句将新属性列表与 `AdventureWorks` 数据库中 `Production.Document` 表的全文检索关联，但不开始填充。  
  
> [!NOTE]  
>  有关将多个预定义的知名搜索属性添加到此搜索属性列表的示例，请参阅 [ALTER SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/alter-search-property-list-transact-sql.md)。 在向列表中添加搜索属性后，数据库管理员将需要使用另一个带有 START FULL POPULATION 子句的 ALTER FULLTEXT INDEX 语句。  
  
```sql 
CREATE SEARCH PROPERTY LIST DocumentPropertyList;  
GO  
USE AdventureWorks2012;  
ALTER FULLTEXT INDEX ON Production.Document   
   SET SEARCH PROPERTY LIST DocumentPropertyList  
   WITH NO POPULATION;   
GO   
```  
  
### <a name="b-creating-a-property-list-from-an-existing-one"></a>B. 基于现有属性列表创建属性列表  
 以下示例基于示例 A 创建的列表 `JobCandidateProperties`（它与 `DocumentPropertyList` 数据库中的全文检索关联）创建一个新的搜索属性列表 `AdventureWorks2012`。 然后，该示例使用 ALTER FULLTEXT INDEX 语句将新属性列表与 `HumanResources.JobCandidate` 数据库中 `AdventureWorks2012` 表的全文检索关联。 此 ALTER FULLTEXT INDEX 语句启动完全填充，这是 SET SEARCH PROPERTY LIST 子句的默认行为。  
  
```sql  
CREATE SEARCH PROPERTY LIST JobCandidateProperties 
FROM AdventureWorks2012.DocumentPropertyList;  
GO  
ALTER FULLTEXT INDEX ON HumanResources.JobCandidate   
   SET SEARCH PROPERTY LIST JobCandidateProperties;  
GO
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/alter-search-property-list-transact-sql.md)   
 [DROP SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/drop-search-property-list-transact-sql.md)   
 [sys.registered_search_properties (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_property (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)   
 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [查找搜索属性的属性集 GUID 和属性整数 ID](../../relational-databases/search/find-property-set-guids-and-property-integer-ids-for-search-properties.md)  
  
  
