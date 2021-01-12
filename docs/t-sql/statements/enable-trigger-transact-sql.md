---
description: ENABLE TRIGGER (Transact-SQL)
title: ENABLE TRIGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENABLE TRIGGER
- ENABLE_TSQL
- ENABLE_TRIGGER_TSQL
- ENABLE
dev_langs:
- TSQL
helpviewer_keywords:
- DDL triggers, enabling
- triggers [SQL Server], enabling
- DML triggers, enabling
- ENABLE TRIGGER statement
ms.assetid: 6e21f0ad-68d0-432f-9c7c-a119dd2d3fc9
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3ed2081a912291eed4682ba0e853df66f36e1801
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98091924"
---
# <a name="enable-trigger-transact-sql"></a>ENABLE TRIGGER (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

启用 DML、DDL 或登录触发器。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
ENABLE TRIGGER { [ schema_name . ] trigger_name [ ,...n ] | ALL }  
ON { object_name | DATABASE | ALL SERVER } [ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
*schema_name*  
触发器所属架构的名称。 不能为 DDL 或登录触发器指定 schema_name。  
  
trigger_name  
要启用的触发器的名称。  
  
ALL  
指示启用在 ON 子句作用域中定义的所有触发器。  
  
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
启用触发器并不是要重新创建它。 禁用的触发器仍以对象形式存在于当前数据库中，但并不触发。 启用触发器将导致在运行触发器最初编程时所针对的任何 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句时触发。 可以使用 [DISABLE TRIGGER](../../t-sql/statements/disable-trigger-transact-sql.md) 禁用触发器。 此外，还可以通过使用 [ALTER TABLE](../../t-sql/statements/alter-table-transact-sql.md) 来禁用或启用为表定义的 DML 触发器。  
  
## <a name="permissions"></a>权限  
若要启用 DML 触发器，用户需要至少对于创建触发器所在的表或视图拥有 ALTER 权限。  
  
若要启用具有服务器作用域 (ON ALL SERVER) 的 DDL 触发器或登录触发器，用户需要对服务器具有 CONTROL SERVER 权限。 若要启用具有数据库范围 (ON DATABASE) 的 DDL 触发器，用户至少需要在当前数据库中拥有 ALTER ANY DATABASE DDL TRIGGER 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-enabling-a-dml-trigger-on-a-table"></a>A. 在表中启用 DML 触发器  
以下示例禁用在 AdventureWorks 数据库的表 `Address` 中创建的触发器 `uAddress`，然后再启用它。  
  
```sql  
DISABLE TRIGGER Person.uAddress ON Person.Address;  
GO  
ENABLE Trigger Person.uAddress ON Person.Address;  
GO  
```  
  
### <a name="b-enabling-a-ddl-trigger"></a>B. 启用 DDL 触发器  
以下示例会创建具有数据库作用域的 DDL 触发器 `safety`，并且先禁用然后再启用该触发器。  
  
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
ENABLE TRIGGER safety ON DATABASE;  
GO  
```  
  
### <a name="c-enabling-all-triggers-that-were-defined-with-the-same-scope"></a>C. 启用以同一作用域定义的所有触发器  
以下示例启用在服务器作用域级别创建的所有 DDL 触发器。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。  
  
```sql  
ENABLE Trigger ALL ON ALL SERVER;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DISABLE TRIGGER (Transact-SQL)](../../t-sql/statements/disable-trigger-transact-sql.md)   
 [ALTER TRIGGER (Transact-SQL)](../../t-sql/statements/alter-trigger-transact-sql.md)   
 [CREATE TRIGGER (Transact-SQL)](../../t-sql/statements/create-trigger-transact-sql.md)   
 [DROP TRIGGER (Transact-SQL)](../../t-sql/statements/drop-trigger-transact-sql.md)   
 [sys.triggers (Transact-SQL)](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)  
  
  
