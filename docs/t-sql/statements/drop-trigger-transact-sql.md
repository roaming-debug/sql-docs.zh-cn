---
description: DROP TRIGGER (Transact-SQL)
title: DROP TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP TRIGGER
- DROP_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- renaming triggers
- triggers [SQL Server], removing
- DDL triggers, removing
- DROP TRIGGER statement
- deleting triggers
- dropping triggers
- removing triggers
- DML triggers, removing
ms.assetid: 092d0d71-9f1e-4e38-a1c4-2487adfa5b4e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0be25328507c6afa210cd4d51817551395f94770
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237757"
---
# <a name="drop-trigger-transact-sql"></a>DROP TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  从当前数据库中删除一个或多个 DML 或 DDL 触发器。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Trigger on an INSERT, UPDATE, or DELETE statement to a table or view (DML Trigger)  
  
DROP TRIGGER [ IF EXISTS ] [schema_name.]trigger_name [ ,...n ] [ ; ]  
  
-- Trigger on a CREATE, ALTER, DROP, GRANT, DENY, REVOKE or UPDATE statement (DDL Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON { DATABASE | ALL SERVER }   
[ ; ]  
  
-- Trigger on a LOGON event (Logon Trigger)  
  
DROP TRIGGER [ IF EXISTS ] trigger_name [ ,...n ]   
ON ALL SERVER  
```  

  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 IF EXISTS  
 **适用范围**：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)、[!INCLUDE[sssds](../../includes/sssds-md.md)]）。  
  
 有条件地删除触发器（仅当其已存在时）。  
  
 *schema_name*  
 DML 触发器所属架构的名称。 DML 触发器的作用域是为其创建该触发器的表或视图的架构。 不能为 DDL 或登录触发器指定 schema_name。  
  
 trigger_name  
 要删除的触发器的名称。 要查看当前已创建触发器的列表，请使用 [sys.server_assembly_modules](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) 或 [sys.server_triggers](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)。  
  
 DATABASE  
 指示 DDL 触发器的作用域应用于当前数据库。 如果在创建或修改触发器时也指定了 DATABASE，则必须指定 DATABASE。  
  
 ALL SERVER  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 指示 DDL 触发器的作用域应用于当前服务器。 如果在创建或修改触发器时也指定了 ALL SERVER，则必须指定 ALL SERVER。 ALL SERVER 也适用于登录触发器。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
## <a name="remarks"></a>备注  
 可以通过删除 DML 触发器或删除触发器表来删除 DML 触发器。 删除表时，将同时删除与表关联的所有触发器。  
  
 删除触发器时，会从 sys.objects、sys.triggers 和 sys.sql_modules 目录视图中删除有关该触发器的信息。  
  
 仅当所有触发器均使用相同的 ON 子句创建时，才能使用一个 DROP TRIGGER 语句删除多个 DDL 触发器。  
  
 若要重命名触发器，可使用 DROP TRIGGER 和 CREATE TRIGGER。 若要更改触发器的定义，可使用 ALTER TRIGGER。  
  
 有关确定特定触发器依赖关系的详细信息，请参阅 [sys.sql_expression_dependencies](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)、[sys.dm_sql_referenced_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md) 和 [sys.dm_sql_referencing_entities (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)。  
  
 有关查看触发器文本的详细信息，请参阅 [sp_helptext (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md) 和 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)。  
  
 有关查看现有触发器列表的详细信息，请参阅 [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md) 和 [sys.server_triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要删除 DML 触发器，需要具有对于定义该触发器所在的表或视图的 ALTER 权限。  
  
 若要删除定义了服务器范围 (ON ALL SERVER) 的 DDL 触发器或删除登录触发器，需要对服务器拥有 CONTROL SERVER 权限。 若要删除定义了数据库范围 (ON DATABASE) 的 DDL 触发器，要求在当前数据库中具有 ALTER ANY DATABASE DDL TRIGGER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-dropping-a-dml-trigger"></a>A. 删除 DML 触发器  
 以下示例将删除 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 数据库中的 `employee_insupd` 触发器。 （从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，可使用 DROP TRIGGER IF EXISTS 语句。）  
  
```sql  
IF OBJECT_ID ('employee_insupd', 'TR') IS NOT NULL  
   DROP TRIGGER employee_insupd;  
```  
  
### <a name="b-dropping-a-ddl-trigger"></a>B. 删除 DDL 触发器  
 以下示例将删除 DDL 触发器 `safety`。  
  
> [!IMPORTANT]  
>  因为 DDL 触发器不在架构范围内，所以不会在 sys.objects 目录视图中出现，无法使用 OBJECT_ID 函数来查询数据库中是否存在 DDL 触发器。 必须使用相应的目录视图来查询架构范围以外的对象。 对于 DDL 触发器，可使用 sys.triggers。  
  
```sql  
DROP TRIGGER safety  
ON DATABASE;  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [ENABLE TRIGGER (Transact-SQL)](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [获取有关 DML 触发器的信息](../../relational-databases/triggers/get-information-about-dml-triggers.md)   
 [sp_help (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)   
 [sp_helptrigger (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helptrigger-transact-sql.md)   
 [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [sys.trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-trigger-events-transact-sql.md)   
 [sys.sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.server_triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-triggers-transact-sql.md)   
 [sys.server_trigger_events (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-trigger-events-transact-sql.md)   
 [sys.server_sql_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-sql-modules-transact-sql.md)   
 [sys.server_assembly_modules (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-assembly-modules-transact-sql.md)  
