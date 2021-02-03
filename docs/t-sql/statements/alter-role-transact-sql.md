---
description: ALTER ROLE (Transact-SQL)
title: ALTER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_ROLE_TSQL
- ALTER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database roles
- ALTER ROLE statement
- renaming database roles
- database roles [SQL Server], modifying
- names [SQL Server], database roles
ms.assetid: e1e83caa-17cc-4871-b2db-2711339fb64f
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ed5da4d728d142a4b80aa894a57ed342368ce09e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204091"
---
# <a name="alter-role-transact-sql"></a>ALTER ROLE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为数据库角色添加或删除成员，或更改用户定义的数据库角色的名称。  
  
> [!NOTE]  
>  若要更改在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 中添加或删除成员的角色，请使用 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) 和 [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server (starting with 2012) and Azure SQL Database  
  
ALTER ROLE  role_name  
{  
       ADD MEMBER database_principal  
    |  DROP MEMBER database_principal  
    |  WITH NAME = new_name  
}  
[;]  
```  
  
 
```syntaxsql
-- Syntax for SQL Server 2008, Azure Synapse Analytics and Parallel Data Warehouse
  
-- Change the name of a user-defined database role  
ALTER ROLE role_name   
    WITH NAME = new_name  
[;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 role_name  
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 2008 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定要更改的数据库角色。  
  
 ADD MEMBER database_principal  
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 2012 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定向数据库角色的成员身份添加数据库主体。  
  
-   database_principal 是数据库用户或用户定义的数据库角色。  
  
-   database_principal 不能是固定的数据库角色或是服务器主体。  
  
DROP MEMBER database_principal  
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 2012 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定从数据库角色的成员身份删除数据库主体。  
  
-   database_principal 是数据库用户或用户定义的数据库角色。  
  
-   database_principal 不能是固定的数据库角色或是服务器主体。  
  
WITH NAME = new_name  
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 2008 开始）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]  
  
 指定更改用户定义的数据库角色的名称。 数据库中必须尚未包含新名称。  
  
 更改数据库角色的名称不会更改角色的 ID 号、所有者或权限。  
  
## <a name="permissions"></a>权限  
 需具有以下一项或多项权限或成员身份才能运行此命令：  
  
-   对角色具有 ALTER 权限  
-   对数据库具有 ALTER ANY ROLE 权限  
-   具有 db_securityadmin 固定数据库角色的成员身份  
  
此外，若要更改固定数据库角色中的成员身份还需要：  
  
-   具有 db_owner 固定数据库角色的成员身份  
  
## <a name="limitations-and-restrictions"></a>限制和局限  
 不能更改固定数据库角色的名称。  
  
## <a name="metadata"></a>Metadata  
 这些系统视图中包含有关数据库角色和数据库主体的信息。  
  
-   [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)  
  
-   [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
## <a name="examples"></a>示例  
  
### <a name="a-change-the-name-of-a-database-role"></a>A. 更改数据库角色的名称  
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 2008 开始）、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
 以下示例将角色 `buyers` 的名称更改为 `purchasing`。   本示例可以在 [AdventureWorks](https://msftdbprodsamples.codeplex.com/) 示例数据库中执行。
  
```sql  
ALTER ROLE buyers WITH NAME = purchasing;  
```  
  
### <a name="b-add-or-remove-role-members"></a>B. 添加或删除角色成员  
 适用对象：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（从 2012 开始）、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
  
 此示例创建一个名为 `Sales` 的数据库角色。 示例向成员身份添加一个名为 Barry 的数据库用户，然后演示如何删除成员 Barry。   本示例可以在 [AdventureWorks](https://msftdbprodsamples.codeplex.com/) 示例数据库中执行。
  
```sql  
CREATE ROLE Sales;  
ALTER ROLE Sales ADD MEMBER Barry;  
ALTER ROLE Sales DROP MEMBER Barry;  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [DROP ROLE (Transact-SQL)](../../t-sql/statements/drop-role-transact-sql.md)   
 [sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
