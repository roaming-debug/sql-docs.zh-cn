---
description: DROP VIEW (Transact-SQL)
title: DROP VIEW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/11/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DROP_VIEW_TSQL
- DROP VIEW
dev_langs:
- TSQL
helpviewer_keywords:
- dropping views
- DROP VIEW statement
- deleting views
- indexed views [SQL Server], removing
- views [SQL Server], removing
- removing views
ms.assetid: 03cea355-e39c-46e1-b7db-8832038669dd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b19f8a8a3f4a1f739d6db9a7e2f365d20b6df981
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465938"
---
# <a name="drop-view-transact-sql"></a>DROP VIEW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从当前数据库中删除一个或多个视图。 可对索引视图执行 DROP VIEW。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server, Azure SQL Database, and Azure Synapse Analytics
  
DROP VIEW [ IF EXISTS ] [ schema_name . ] view_name [ ...,n ] [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
DROP VIEW [ schema_name . ] view_name [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 **适用范围**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 到 [当前版本](https://go.microsoft.com/fwlink/p/?LinkId=299658)[!INCLUDE[sssds](../../includes/sssds-md.md)]）。|  
  
 只有在视图已存在时才对其进行有条件地删除。  
  
 *schema_name*  
 视图所属架构的名称。  
  
 view_name  
 要删除的视图的名称。  
  
## <a name="remarks"></a>注解  
 删除视图时，将从系统目录中删除视图的定义和有关视图的其他信息。 还将删除视图的所有权限。  
  
 使用 DROP TABLE 删除的表上的任何视图都必须使用 DROP VIEW 显式删除。  
  
 对索引视图执行 DROP VIEW 时，将自动删除视图上的所有索引。 若要显示视图上的所有索引，请使用 [sp_helpindex](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md)。  
  
 通过视图进行查询时，[!INCLUDE[ssDE](../../includes/ssde-md.md)]将进行检查以确保语句中引用的所有数据库对象都存在，这些对象在语句的上下文中有效，以及数据修改语句没有违反任何数据完整性规则。 如果检查失败，将返回错误消息。 如果检查成功，则将操作转换为对基础表的操作。 如果基础表或视图自最初创建视图以来已发生更改，则删除并重新创建视图可能很有用。  
  
 有关确定特定视图的依赖关系的详细信息，请参阅 [sys.sql_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-dependencies-transact-sql.md)。  
  
 有关查看视图文本的详细信息，请参阅 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 需要对视图拥有 **CONTROL** 权限，对包含视图的架构拥有 **ALTER** 权限，或者拥有 **db_ddladmin** 固定服务器角色中的成员身份。  
  
## <a name="examples"></a>示例  
  
### <a name="a-drop-a-view"></a>A. 删除视图  
 以下示例删除视图 `Reorder`。  
  
```sql
DROP VIEW IF EXISTS dbo.Reorder ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/alter-view-transact-sql.md)   
 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects (Transact-SQL)](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [USE (Transact-SQL)](../../t-sql/language-elements/use-transact-sql.md)   
 [sys.sql_expression_dependencies (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)  
 
