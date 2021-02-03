---
description: CREATE FULLTEXT STOPLIST (Transact-SQL)
title: CREATE FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STOPLIST_TSQL
- FULLTEXT STOPLIST
- STOPLIST
- FULLTEXT_STOPLIST_TSQL
- CREATE FULLTEXT STOPLIST
- CREATE_FULLTEXT_STOPLIST_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- CREATE FULLTEXT STOPLIST statement
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 0669b1d0-46cc-4fac-8df7-5f7fa7af5db4
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: feef9cf677b1d897a1a9cfc681791c0aba8fdf60
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191037"
---
# <a name="create-fulltext-stoplist-transact-sql"></a>CREATE FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在当前数据库中创建新的全文非索引字表。  
  
 通过使用称为“非索引字表”的对象在数据库中管理非索引字。 “非索引字表”是一个由非索引字组成的列表，这些非索引字在与全文索引关联时会应用于该索引的全文查询。 有关详细信息，请参阅 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)。  
  
> [!IMPORTANT]  
>  仅在兼容级别为 100 时才支持 CREATE FULLTEXT STOPLIST、ALTER FULLTEXT STOPLIST 和 DROP FULLTEXT STOPLIST。 兼容级别为 80 和 90 时，将不支持这些语句。 不过，在所有兼容级别下，系统非索引字表将会自动与新的全文索引相关联。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
CREATE FULLTEXT STOPLIST stoplist_name  
[ FROM { [ database_name.]source_stoplist_name } | SYSTEM STOPLIST ]  
[ AUTHORIZATION owner_name ]  
;  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 stoplist_name  
 非索引字表的名称。 stoplist_name 最多可以包含 128 个字符。 stoplist_name 在当前数据库中的所有非索引字表中必须是唯一的，并且符合标识符规则。  
  
 创建全文检索时将使用 stoplist_name。  
  
 *database_name*  
 source_stoplist_name 指定的非索引字表所在的数据库的名称。 如果未指定，则 database_name 默认为当前数据库。  
  
 source_stoplist_name  
 指定通过复制现有的非索引字表来创建新的非索引字表。 如果不存在 source_stoplist_name，或者数据库用户没有正确的权限，CREATE FULLTEXT STOPLIST 将失败，并返回错误。 如果源非索引字表的停止词中指定的任何语言未在当前数据库中注册，CREATE FULLTEXT STOPLIST 将成功，但会返回警告且不会添加相应的停止词。  
  
 SYSTEM STOPLIST  
 指定用默认存在于[资源数据库](../../relational-databases/databases/resource-database.md)中的非索引字表创建新的非索引字表。  
  
 AUTHORIZATION owner_name   
 指定拥有此非索引字表的数据库主体的名称。 owner_name 必须是当前用户所属的主体的名称，或者当前用户必须具有对 owner_name 的 IMPERSONATE 权限。 如果未指定，则所有权授予当前用户。  
  
## <a name="remarks"></a>备注  
 非索引字表的创建者也是其所有者。  
  
## <a name="permissions"></a>权限  
 若要创建 STOPLIST，需要拥有 CREATE FULLTEXT CATALOG 权限。 非索引字表的所有者可显式授予对非索引字表的 CONTROL 权限，以允许用户添加和删除词，以及删除该非索引字表。  
  
> [!NOTE]  
>  若要对全文索引使用非索引字表，需要拥有 REFERENCE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-new-full-text-stoplist"></a>A. 创建新的全文非索引字表  
 下面的示例创建一个名为 `myStoplist` 的新的全文非索引字表。  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist;  
GO  
```  
  
### <a name="b-copying-a-full-text-stoplist-from-an-existing-full-text-stoplist"></a>B. 从现有的全文非索引字表复制全文非索引字表  
 下面的示例通过复制一个名为 `myStoplist2` 的现有 AdventureWorks 非索引字表来创建一个名为 `Customers.otherStoplist` 的新全文非索引字表。  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist2 FROM AdventureWorks.otherStoplist;  
GO  
```  
  
### <a name="c-copying-a-full-text-stoplist-from-the-system-full-text-stoplist"></a>C. 从系统全文非索引字表复制全文非索引字表  
 下面的示例通过从系统非索引字表进行复制来创建一个名为 `myStoplist3` 的新全文非索引字表。  
  
```sql  
CREATE FULLTEXT STOPLIST myStoplist3 FROM SYSTEM STOPLIST;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)   
 [DROP FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/drop-fulltext-stoplist-transact-sql.md)   
 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [sys.fulltext_stoplists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [为全文搜索配置和管理非索引字和非索引字表](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
