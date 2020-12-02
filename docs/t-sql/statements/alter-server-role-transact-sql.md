---
description: ALTER SERVER ROLE (Transact-SQL)
title: ALTER SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/16/2020
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ALTER_SERVER_ROLE_TSQL
- ALTER SERVER ROLE
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, ALTER
- ALTER SERVER ROLE statement
ms.assetid: 7a4db7bb-c442-4e12-9a8a-114da5bc7710
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c79b8ad73bc939354d3b9078c9486531ec8b01ec
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126233"
---
# <a name="alter-server-role-transact-sql"></a>ALTER SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

更改服务器角色的成员关系或更改用户定义的服务器角色的名称。 无法重命名固定服务器角色。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server  
  
ALTER SERVER ROLE server_role_name   
{  
    [ ADD MEMBER server_principal ]  
  | [ DROP MEMBER server_principal ]  
  | [ WITH NAME = new_server_role_name ]  
} [ ; ]  
```  
  
```syntaxsql
-- Syntax for Parallel Data Warehouse  
  
ALTER SERVER ROLE  server_role_name  ADD MEMBER login;  
  
ALTER SERVER ROLE  server_role_name  DROP MEMBER login;  
```  
  
## <a name="arguments"></a>参数  
*server_role_name*  
要更改的服务器角色的名称。  
  
ADD MEMBER server_principal  
将指定的服务器主体添加到服务器角色中。 server_principal 可以是登录名或用户定义的服务器角色。 server_principal 不能是固定服务器角色、数据库角色或 sa。  
  
DROP MEMBER server_principal  
从服务器角色中删除指定的服务器主体。 server_principal 可以是登录名或用户定义的服务器角色。 server_principal 不能是固定服务器角色、数据库角色或 sa。  
  
WITH NAME =new_server_role_name  
指定用户定义的服务器角色的新名称。 服务器中不能已存在此名称。  
  
## <a name="remarks"></a>备注  
更改用户定义的服务器角色名称并不会更改角色的 ID 号、所有者或权限。  
  
为了更改角色成员身份，`ALTER SERVER ROLE` 会替换 sp_addsrvrolemember 和 sp_dropsrvrolemember。 不推荐使用这些存储过程。  
  
您可以通过查询 `sys.server_role_members` 和 `sys.server_principals` 目录视图来查看服务器角色。  
  
若要更改用户定义的服务器角色的所有者，请使用 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
要求在服务器上具有 `ALTER ANY SERVER ROLE` 权限才能更改用户定义的服务器角色的名称。  
  
**固定服务器角色**  
  
若要为固定服务器角色添加成员，您必须是该固定服务器角色的成员，或者是 `sysadmin` 固定服务器角色的成员。  
  
> [!NOTE]  
>  `CONTROL SERVER` 和 `ALTER ANY SERVER ROLE` 权限不足以为固定服务器角色执行 `ALTER SERVER ROLE`，并且不能为固定服务器角色授予 `ALTER`权限。  
  
**用户定义的服务器角色**  
  
若要为用户定义的服务器角色添加成员，您必须是 `sysadmin` 固定服务器角色的成员，或者具有 `CONTROL SERVER` 或 `ALTER ANY SERVER ROLE` 权限。 否则，必须具有该角色的 `ALTER` 权限。  
  
> [!NOTE]  
>  与固定服务器角色不同，用户定义的服务器角色的成员本身并不具备为该同一角色添加成员的权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-changing-the-name-of-a-server-role"></a>A. 更改服务器角色的名称  
以下示例创建一个名为 `Product` 的服务器角色，然后将该服务器角色的名称更改为 `Production`。  
  
```sql
CREATE SERVER ROLE Product ;  
ALTER SERVER ROLE Product WITH NAME = Production ;  
GO  
```  
  
### <a name="b-adding-a-domain-account-to-a-server-role"></a>B. 在服务器角色中添加域帐户  
以下示例在名为 `Production` 的用户定义服务器角色中添加一个名为 `adventure-works\roberto0` 的域帐户。  
  
```sql
ALTER SERVER ROLE Production ADD MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="c-adding-a-sql-server-login-to-a-server-role"></a>C. 在服务器角色中添加 SQL Server 登录名  
以下示例在 `diskadmin` 固定服务器角色中添加一个名为 `Ted` 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
```sql
ALTER SERVER ROLE diskadmin ADD MEMBER Ted ;  
GO  
```  
  
### <a name="d-removing-a-domain-account-from-a-server-role"></a>D. 从服务器角色中删除域帐户  
以下示例从名为 `Production` 的用户定义服务器角色中删除一个名为 `adventure-works\roberto0` 的域帐户。  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER [adventure-works\roberto0] ;  
```  
  
### <a name="e-removing-a-sql-server-login-from-a-server-role"></a>E. 从服务器角色中删除 SQL Server 登录名  
以下示例从 `diskadmin` 固定服务器角色中删除 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `Ted`。  
  
```sql
ALTER SERVER ROLE Production DROP MEMBER Ted ;  
GO  
```  
  
### <a name="f-granting-a-login-the-permission-to-add-logins-to-a-user-defined-server-role"></a>F. 为登录名授予权限以将登录名添加到用户定义的服务器角色中  
以下示例允许 `Ted` 将其他登录名添加到名为 `Production` 的用户定义服务器角色中。  
  
```sql
GRANT ALTER ON SERVER ROLE::Production TO Ted ;  
GO  
```  
  
### <a name="g-to-view-role-membership"></a>G. 查看角色成员身份  
若要查看角色成员身份，请使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中的服务器角色（成员）页或执行以下查询：  
  
```sql
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
## <a name="examples-sspdw"></a>示例：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="h-basic-syntax"></a>H. 基本语法  
以下示例将登录名 `Anna` 添加到 `LargeRC` 服务器角色中。  
  
```sql
ALTER SERVER ROLE LargeRC ADD MEMBER Anna;  
```  
  
### <a name="i-remove-a-login-from-a-resource-class"></a>I. 从资源类中删除登录。  
以下示例在 `LargeRC` 服务器角色中删除 Anna 成员身份。  
  
```sql
ALTER SERVER ROLE LargeRC DROP MEMBER Anna;  
```  
  
## <a name="see-also"></a>另请参阅  
[CREATE SERVER ROLE (Transact-SQL)](../../t-sql/statements/create-server-role-transact-sql.md)   
[DROP SERVER ROLE (Transact-SQL)](../../t-sql/statements/drop-server-role-transact-sql.md)   
[CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
[ALTER ROLE (Transact-SQL)](../../t-sql/statements/alter-role-transact-sql.md)   
[DROP ROLE (Transact-SQL)](../../t-sql/statements/drop-role-transact-sql.md)   
[安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
[安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)   
[主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.server_role_members (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)   
[sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
