---
title: CREATE DATABASE AUDIT SPECIFICATION
description: 使用 SQL Server 审核功能创建数据库审核规范对象。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 01/03/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE DATABASE AUDIT
- DATABASE_AUDIT_SPECIFICATION_TSQL
- DATABASE AUDIT SPECIFICATION
- CREATE_DATABASE_AUDIT_SPECIFICATION_TSQL
- CREATE_DATABASE_AUDIT_TSQL
- CREATE DATABASE AUDIT SPECIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- database audit specification
- CREATE DATABASE AUDIT SPECIFICATION statement
ms.assetid: 0544da48-0ca3-4a01-ba4c-940e23dc315b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: dfa2c988670df2c2b7b9176cbde4057c4b125c75
ms.sourcegitcommit: 2cc2e4e17ce88ef47cda32a60a02d929e617738e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103473202"
---
# <a name="create-database-audit-specification-transact-sql"></a>CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 审核功能创建数据库审核规范对象。 有关详细信息，请参阅 [SQL Server Audit（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
CREATE DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    FOR SERVER AUDIT audit_name   
        [ { ADD ( { <audit_action_specification> | audit_action_group_name } )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      action [ ,...n ]ON [ class :: ] securable BY principal [ ,...n ]  
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 audit_specification_name   
 是审核规范的名称。  
  
 audit_name   
 是应用此规范的审核的名称。  
  
 audit_action_specification   
 是主体对安全对象执行的应记录到审核中的操作的规范。  
  
 *action*  
 是一个或多个数据库级别可审核操作的名称。 要获取审核操作列表，请参阅 [SQL Server 审核操作组和操作](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)。  
  
 audit_action_group_name   
 是一个或多个数据库级别可审核操作组的名称。 要获取审核操作组列表，请参阅 [SQL Server 审核操作组和操作](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md)。  
  
 class   
 是安全对象上的类名（如果适用）。  
  
 securable   
 是应用审核操作或审核操作组的数据库中的表、视图或其他安全对象。 有关详细信息，请参阅 [Securables](../../relational-databases/security/securables.md)。  
  
 principal   
 应用审核操作或审核操作组的数据库主体的名称。 使用数据库主体 `public` 审核所有数据库主体。 有关详细信息，请参阅[主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)。  
  
 WITH ( STATE = { ON | OFF } )  
 允许或禁止审核收集此审核规范的记录。  
  
## <a name="remarks"></a>备注  
 数据库审核规范是驻留在给定数据库中的非安全对象。 数据库审核规范在创建之后处于禁用状态。  
  
## <a name="permissions"></a>权限  
 拥有 `ALTER ANY DATABASE AUDIT` 权限的用户可以创建数据库审核规范并将其绑定到任何审核。  
  
 在创建数据库审核规范之后，拥有 `CONTROL SERVER` 权限的用户或 `sysadmin` 帐户可查看该规范。  
  
## <a name="examples"></a>示例

### <a name="a-audit-select-and-insert-on-a-table-for-any-database-principal"></a>A. 审核任何数据库主体的表上的 SELECT 和 INSERT 
 下面的示例针对 `AdventureWorks2012` 数据库中的 `HumanResources.EmployeePayHistory` 表，创建名为 `Payrole_Security_Audit` 的服务器审核，然后创建可由 `public` 数据库角色的任何成员审核 `SELECT` 和 `INSERT` 语句的数据库审核规范，其名称为 `Payrole_Security_Audit`。 这样做的效果是，每个用户都会被审核，因为每个用户始终都是角色 `public` 的成员。
  
```sql  
USE master ;  
GO  
-- Create the server audit.  
CREATE SERVER AUDIT Payrole_Security_Audit  
    TO FILE ( FILEPATH =   
'D:\SQLAudit\' ) ;  -- make sure this path exists
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT Payrole_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
FOR SERVER AUDIT Payrole_Security_Audit  
ADD (SELECT , INSERT  
     ON HumanResources.EmployeePayHistory BY public )  
WITH (STATE = ON) ;  
GO  
``` 

### <a name="b-audit-any-dml-insert-update-or-delete-on-_all_-objects-in-the-_sales_-schema-for-a-specific-database-role"></a>B. 审核特定数据库角色“sales”架构中所有对象上的任何 DML（INSERT、UPDATE 或 DELETE）    
 下面的示例针对 `DataModification_Security_Audit` 数据库中 `Audit_Data_Modification_On_All_Sales_Tables` 架构的所有对象，创建名为 `INSERT` 的服务器审核，然后创建可由新数据库角色 `UPDATE` 中的用户审核 `DELETE`、`SalesUK` 和 `Sales` 语句的数据库审核规范，其名称为 `AdventureWorks2012`。  
  
```sql  
USE master ;  
GO  
-- Create the server audit.
-- Change the path to a path that the SQLServer Service has access to. 
CREATE SERVER AUDIT DataModification_Security_Audit  
    TO FILE ( FILEPATH = 
'D:\SQLAudit\' ) ;  -- make sure this path exists
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT DataModification_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
CREATE ROLE SalesUK
GO
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Data_Modification_On_All_Sales_Tables  
FOR SERVER AUDIT DataModification_Security_Audit  
ADD ( INSERT, UPDATE, DELETE  
     ON Schema::Sales BY SalesUK )  
WITH (STATE = ON) ;    
GO  
```  


## <a name="see-also"></a>另请参阅  
 [CREATE SERVER AUDIT (Transact-SQL)](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT (Transact-SQL)](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT (Transact-SQL)](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file (Transact-SQL)](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [创建服务器审核和服务器审核规范](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
