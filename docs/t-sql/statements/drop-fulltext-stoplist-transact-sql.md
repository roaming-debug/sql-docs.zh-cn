---
description: DROP FULLTEXT STOPLIST (Transact-SQL)
title: DROP FULLTEXT STOPLIST (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_FULLTEXT_STOPLIST_TSQL
- DROP FULLTEXT STOPLIST
dev_langs:
- TSQL
helpviewer_keywords:
- DROP FULLTEXT STOPLIST statement
- stoplists [full-text search]
- full-text search [SQL Server], stoplists
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 3ee2a2bb-1dfb-4e7c-90e9-9d917cd84a15
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8591c42688d3b459a2c3bdb126c6c988e3bdb1e
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091937"
---
# <a name="drop-fulltext-stoplist-transact-sql"></a>DROP FULLTEXT STOPLIST (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的数据库中删除全文非索引字表。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
> [!IMPORTANT]  
>  只有在兼容级别为 100 或更高时，才支持 CREATE FULLTEXT STOPLIST。 在 80 和 90 兼容级别下，会始终为数据库分配系统非索引字表。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP FULLTEXT STOPLIST stoplist_name  
;  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 stoplist_name  
 要从数据库中删除的全文本非索引字表的名称。  
  
## <a name="remarks"></a>注解  
 如果有任何全文索引引用了已删除的全文本非索引字表，则 DROP FULLTEXT STOPLIST 会失败。  
  
## <a name="permissions"></a>权限  
 若要删除非索引字表，则必须对该非索引字表具有 DROP 权限，或者必须是 **db_owner** 或 **db_ddladmin** 固定数据库角色的成员。  
  
## <a name="examples"></a>示例  
 下面的示例将删除名为 `myStoplist` 的全文非索引字表。  
  
```sql 
DROP FULLTEXT STOPLIST myStoplist;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md)   
 [CREATE FULLTEXT STOPLIST (Transact-SQL)](../../t-sql/statements/create-fulltext-stoplist-transact-sql.md)   
 [sys.fulltext_stoplists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)  
  
  
