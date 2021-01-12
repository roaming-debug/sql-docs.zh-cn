---
description: DISABLE TRIGGER (Transact-SQL)
title: DISABLE TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- DISABLE_TSQL
- DISABLE
- DISABLE TRIGGER
- DISABLE_TRIGGER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DML triggers, disabling
- DDL triggers, disabling
- DISABLE TRIGGER statement
- triggers [SQL Server], disabling
- disabling triggers
ms.assetid: e6529f06-e442-437e-a7bf-41790bc092c5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 41976059d1deb47fcbfc1c939240db415ee8cf64
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98081392"
---
# <a name="disable-trigger-transact-sql"></a>DISABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  禁用触发器。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DISABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 触发器所属架构的名称。 不能为 DDL 或登录触发器指定 schema_name。  
  
 trigger_name  
 要禁用的触发器的名称。  
  
 ALL  
 指示禁用在 ON 子句作用域中定义的所有触发器。  
  
> [!CAUTION]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在为合并复制发布的数据库中创建触发器。 在已发布数据库中指定 ALL 可禁用这些触发器，这样会中断复制。 在指定 ALL 之前，请验证没有为合并复制发布当前数据库。  
  
 *object_name*  
 对其创建了要执行的 DML 触发器 *trigger_name* 的表或视图的名称。  
  
 DATABASE  
 对于 DDL 触发器，指示所创建或修改的 *trigger_name* 将在数据库作用域内执行。  
  
 ALL SERVER  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
 对于 DDL 触发器，指示所创建或修改的 *trigger_name* 将在服务器作用域内执行。 ALL SERVER 也适用于登录触发器。  
  
> [!NOTE]  
>  此选项在包含数据库中不可用。  
  
## <a name="remarks"></a>备注  
 默认情况下，创建触发器后会启用触发器。 禁用触发器不会删除该触发器。 该触发器仍然作为对象存在于当前数据库中。 但是，当执行编写触发器程序所用的任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时，不会激发触发器。 可以使用 [ENABLE TRIGGER](../../t-sql/statements/enable-trigger-transact-sql.md) 重新启用触发器。 还可以通过使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 来禁用或启用为表定义的 DML 触发器。  
  
 使用 **ALTER TRIGGER** 语句更改触发器将启用此触发器。  
  
## <a name="permissions"></a>权限  
 若要禁用 DML 触发器，用户必须至少对为其创建触发器的表或视图具有 ALTER 权限。  
  
 若要禁用具有服务器范围 (ON ALL SERVER) 的 DDL 触发器或登录触发器，用户必须对服务器拥有 CONTROL SERVER 权限。 若要禁用数据库范围 (ON DATABASE) 中的 DDL 触发器，用户必须至少对当前数据库具有 ALTER ANY DATABASE DDL TRIGGER 权限。  
  
## <a name="examples"></a>示例  
AdventureWorks2012 数据库中介绍了以下示例。
  
### <a name="a-disabling-a-dml-trigger-on-a-table"></a>A. 禁用对表的 DML 触发器  
 以下示例禁用对表 `uAddress` 创建的触发器 `Person`。  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-disabling-a-ddl-trigger"></a>B. 禁用 DDL 触发器  
 以下示例在数据库范围创建 DDL 触发器 `safety`，然后禁用该触发器。  
  
```sql  
CREATE TRIGGER safety   
ON DATABASE   
FOR DROP_TABLE, ALTER_TABLE   
AS   
   PRINT 'You must disable Trigger "safety" to drop or alter tables!'   
   ROLLBACK;  
GO  
DISABLE TRIGGER safety ON DATABASE;  
GO  
```  
  
### <a name="c-disabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. 禁用以同一作用域定义的所有触发器  
 下例禁用在服务器范围内创建的所有 DDL 触发器。  
  
```sql  
DISABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ENABLE TRIGGER (Transact-SQL)](../../t-sql/statements/enable-trigger-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  
