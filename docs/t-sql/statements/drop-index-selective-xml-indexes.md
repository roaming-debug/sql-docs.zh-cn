---
description: DROP INDEX（选择性 XML 索引）
title: DROP INDEX（选择性 XML 索引）| Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP XML INDEX statement
dev_langs:
- TSQL
ms.assetid: 4779ae84-e5f4-4d04-8fc1-e24a6631b428
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 10a8dce89cff5a8201c583b9479a774d44a72ade
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096944"
---
# <a name="drop-index-selective-xml-indexes"></a>DROP INDEX（选择性 XML 索引）
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的现有选择性 XML 索引或辅助选择性 XML 索引。 有关详细信息，请参阅[选择性 XML 索引 (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DROP INDEX index_name ON <object>  
    [ WITH ( <drop_index_option> [ ,...n ] ) ]  
  
<object> ::=  
{ database_name.schema_name.table_or_view_name | schema_name.table_or_view_name | table_or_view_name }  
  
<drop_index_option> ::=  
{  
    MAXDOP = max_degree_of_parallelism  
    | ONLINE = { ON | OFF }  
}  
```  
  
##  <a name="arguments"></a><a name="Arguments"></a> 参数  
 index_name  
 要删除的现有索引的名称。  
  
 \< object>：包含已建立索引的 XML 列的表。 使用以下格式之一：  
  
-   `database_name.schema_name.table_name`  
  
-   `database_name..table_name`  
  
-   `schema_name.table_name`  
  
-   `table_name`  
  
 \<drop_index_option>：要了解“删除索引”选项，请参阅 [DROP INDEX (Transact-SQL)](../../t-sql/statements/drop-index-transact-sql.md)。  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 若要运行 DROP INDEX，需要对表或视图拥有 ALTER 权限。 默认情况下，此权限授予 sysadmin 固定服务器角色以及 db_ddladmin 和 db_owner 固定数据库角色。  
  
## <a name="example"></a>示例  
 下面的示例说明 DROP INDEX 语句。  
  
```sql  
DROP INDEX sxi_index ON tbl;  
```  
  
## <a name="see-also"></a>另请参阅  
 [选择性 XML 索引 (SXI)](../../relational-databases/xml/selective-xml-indexes-sxi.md)   
 [创建、更改和删除选择性 XML 索引](../../relational-databases/xml/create-alter-and-drop-selective-xml-indexes.md)  
  
  

