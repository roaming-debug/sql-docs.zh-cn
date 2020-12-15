---
description: 获取有关 DML 触发器的信息
title: 获取有关 DML 触发器的信息 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- metadata [SQL Server], triggers
- viewing DML triggers
- DML triggers, metadata
- displaying DML triggers
- status information [SQL Server], triggers
- DML triggers, viewing
ms.assetid: 37574aac-181d-4aca-a2cc-8abff64237dc
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 39a4d0a6136990cdb583a432757503099f61cfb6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97403393"
---
# <a name="get-information-about-dml-triggers"></a>获取有关 DML 触发器的信息
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主题说明如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中获取有关 DML 触发器的信息。 此信息可包含针对某个表的触发器的类型、触发器的名称、其所有者以及创建或修改日期。 如果在创建触发器时未加密，您可获取该触发器的定义。 通过该定义，可以帮助您了解该触发器如何影响对其定义该触发器的表。 此外，您可以找出特定触发器使用的对象。 通过该信息，可在数据库中影响触发器的对象发生更改或删除它们时确定这些对象。  
  
 **本主题内容**  
  
-   **开始之前：**  
  
     [安全性](#Security)  
  
-   **若要获取有关 DML 触发器的信息，请使用：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 开始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 权限  
 **sys.sql.modules**, **sys.object**, **sys.triggers**, **sys.events**, **sys.trigger_events**  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
 OBJECT_DEFINITION, OBJECTPROPERTY, **sp_helptext**  
 要求 **公共** 角色具有成员身份。 用户对象的定义对于对象所有者或具有下列任一权限的被授权者可见：ALTER、CONTROL、TAKE OWNERSHIP 或 VIEW DEFINITION。 **db_owner**、 **db_ddladmin** 和 **db_securityadmin** 固定数据库角色的成员隐式具有这些权限。  
  
 **sys.sql_expression_dependencies**  
 要求对数据库具有 VIEW DEFINITION 权限，并对数据库的 **sys.sql_expression_dependencies** 具有 SELECT 权限。 默认情况下，SELECT 权限仅授予 **db_owner** 固定数据库角色的成员。 将 SELECT 和 VIEW DEFINITION 权限授予其他用户时，被授权者可以查看数据库中的所有依赖关系。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-the-definition-of-a-dml-trigger"></a>查看 DML 触发器的定义  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开您所需的数据库，再展开 **“表”** ，然后展开包含要查看其定义的触发器的表。  
  
3.  展开“触发器”，右键单击需要的触发器，然后单击“修改”。 将在查询窗口中显示 DML 触发器的定义。  

#### <a name="to-view-the-dependencies-of-a-dml-trigger"></a>查看 DML 触发器的依赖关系  
  
1.  在 **“对象资源管理器”** 中，连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的实例，然后展开该实例。  
  
2.  展开您所需的数据库，再展开 **“表”** ，然后展开包含要查看的触发器及其依赖关系的表。  
  
3.  展开“触发器”，右键单击需要的触发器，然后单击“查看依赖关系”。  
  
4.  在“对象依赖关系”窗口中，若要查看依赖于 DML 触发器的对象，请选择“依赖于 \<DML trigger name> 的对象”。 将在 **“依赖关系”** 区域显示这些对象。  
  
     若要查看 DML 所依赖的对象，请选择“\<DML trigger name> 所依赖的对象”。 将在 **“依赖关系”** 区域显示这些对象。 展开每个节点以查看所有对象。  
  
5.  若要获取有关 **“依赖关系”** 区域中显示的对象的信息，请单击该对象。 在 **“所选对象”** 字段中，在 **“名称”** 、 **“类型”** 和 **“依赖关系类型”** 框中提供信息。  
  
6.  若要关闭 **“对象依赖关系”** 窗口，请单击 **“确定”** 。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-view-the-definition-of-a-dml-trigger"></a>查看 DML 触发器的定义  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例之一复制并粘贴到查询窗口中，然后单击 **“执行”** 。 每个示例显示您如何查看 `iuPerson` 触发器的定义。  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT definition   
FROM sys.sql_modules  
WHERE object_id = OBJECT_ID(N'Person.iuPerson');   
GO  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT OBJECT_DEFINITION (OBJECT_ID(N'Person.iuPerson')) AS ObjectDefinition;   
GO  
  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
EXEC sp_helptext 'Person.iuPerson'  
GO  
  
```  
  
#### <a name="to-view-the-dependencies-of-a-dml-trigger"></a>查看 DML 触发器的依赖关系  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例之一复制并粘贴到查询窗口中，然后单击 **“执行”** 。 每个示例显示您如何查看 `iuPerson` 触发器的依赖关系。  
  
```  
USE AdventureWorks2012;   
GO  
SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc, referenced_class_desc,   
    referenced_server_name, referenced_database_name, referenced_schema_name,   
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,   
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referencing_id = OBJECT_ID(N'Person.iuPerson');   
GO  
  
```  
  
#### <a name="to-view-information-about-dml-triggers-in-the-database"></a>查看有关数据库中的 DML 触发器的信息  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例之一复制并粘贴到查询窗口中，然后单击 **“执行”** 。 每个示例显示您如何查看数据库中有关 DML 触发器 (`TR`) 的信息。  
  
```  
USE AdventureWorks2012;   
GO  
SELECT  name, parent_id, create_date, modify_date, is_instead_of_trigger  
FROM sys.triggers  
WHERE type = 'TR';   
GO  
  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT  name, object_id, schema_id, parent_object_id, type_desc, create_date, modify_date, is_published  
FROM sys.objects  
WHERE type = 'TR';   
GO  
  
```  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT OBJECTPROPERTY(OBJECT_ID(N'Person.iuPerson'), 'ExecIsInsteadOfTrigger');   
GO  
  
```  
  
#### <a name="to-view-information-about-events-that-fire-a-dml-trigger"></a>查看有关激发 DML 触发器的事件的信息  
  
1.  连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在标准菜单栏上，单击 **“新建查询”** 。  
  
3.  将以下示例之一复制并粘贴到查询窗口中，然后单击 **“执行”** 。 每个示例显示如何查看激发 `iuPerson` 触发器的事件。  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT object_id, type, type_desc, is_trigger_event, event_group_type, event_group_type_desc   
FROM sys.events  
WHERE object_id = OBJECT_ID('Person.iuPerson');   
GO  
```  
  
```sql  
USE AdventureWorks2012;   
GO   
SELECT object_id, type,is_first, is_last  
FROM sys.trigger_events  
WHERE object_id = OBJECT_ID('Person.iuPerson');   
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [ENABLE TRIGGER (Transact-SQL)](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_rename (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
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
 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md)   
 [OBJECT_DEFINITION (Transact-SQL)](../../t-sql/functions/object-definition-transact-sql.md)  
  
  
