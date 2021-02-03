---
description: 拒绝服务器主体权限 (Transact-SQL)
title: DENY 服务器主体权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, impersonate
- permissions [SQL Server], impersonate
- impersonate [SQL Server], denying
- DENY statement, logins
- permissions [SQL Server], logins
- denying permissions [SQL Server], logins
- servers [SQL Server], permissions
- logins [SQL Server], denying access
ms.assetid: 859affa7-0567-47d1-9490-57c1abbd619b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8e3ea7c43e664883324f87f031ba47f985ef78bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177686"
---
# <a name="deny-server-principal-permissions-transact-sql"></a>拒绝服务器主体权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  拒绝为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名授予的权限。  
  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    TO <server_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
    SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey   
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名拒绝的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 LOGIN ::SQL_Server_login  
 指定要对其拒绝权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 需要使用作用域限定符 (::)。  
  
 SERVER ROLE **::** *server_role*  
 指定要拒绝权限的服务器角色。 需要使用作用域限定符 (::)。  
  
 TO \<server_principal>  
 指定要授予权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或服务器角色。  
  
 TO SQL_Server_login  
 指定要拒绝权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_Windows_login  
 指定通过 Windows 登录帐户创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_certificate  
 指定映射到证书的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_AsymKey  
 指定映射到非对称密钥的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 server_role  
 指定服务器角色的名称。  
  
 CASCADE  
 指示要拒绝的权限也会被对此主体授予该权限的其他主体拒绝。  
  
 AS SQL_Server_login  
 指定执行此查询的主体要从哪个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名派生其拒绝该权限的权限。  
  
## <a name="remarks"></a>备注  
 只有在当前数据库为 master 时，才可拒绝其服务器范围内的权限。  
  
 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中获取服务器权限的相关信息。 可以在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中获取服务器主体的相关信息。  
  
 如果针对通过 GRANT OPTION 授予权限的主体拒绝该权限，但未指定 CASCADE，则 DENY 语句失败。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和服务器角色是服务器级安全对象。 下表列出了可为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或服务器角色拒绝的最具体的限定权限，以及隐含这些权限的更一般的权限。  
  
|SQL Server 登录名或服务器角色权限|SQL Server 登录名或服务器角色权限隐含的权限|服务器权限隐含的权限|  
|------------------------------------------------|-----------------------------------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL SERVER|  
|IMPERSONATE|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|ALTER|CONTROL|ALTER ANY LOGIN<br /><br /> ALTER ANY SERVER ROLE|  
  
## <a name="permissions"></a>权限  
 对于登录名，要求具有登录名的 CONTROL 权限或服务器上的 ALTER ANY LOGIN 权限。  
  
 对于服务器角色，要求具有服务器角色的 CONTROL 权限或服务器上的 ALTER ANY SERVER ROLE 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-denying-impersonate-permission-on-a-login"></a>A. 拒绝登录名的 IMPERSONATE 权限  
 以下示例对通过 Windows 用户 `AdvWorks\YoonM` 创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名拒绝对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `WanidaBenshoof` 的 `IMPERSONATE` 权限。  
  
```  
USE master;  
DENY IMPERSONATE ON LOGIN::WanidaBenshoof TO [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-denying-view-definition-permission-with-cascade"></a>B. 使用 CASCADE 拒绝 VIEW DEFINITION 权限  
 以下示例拒绝 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `VIEW DEFINITION` 对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `EricKurjan` 的 `RMeyyappan` 权限。 `CASCADE` 选项指示还会拒绝 `VIEW DEFINITION` 授予 `EricKurjan` 权限的主体对 `RMeyyappan` 的该权限。  
  
```sql  
USE master;  
DENY VIEW DEFINITION ON LOGIN::EricKurjan TO RMeyyappan   
    CASCADE;  
GO   
```  
  
### <a name="c-denying-view-definition-permission-on-a-server-role"></a>C. 拒绝服务器角色的 VIEW DEFINITION 权限  
 以下示例拒绝 `VIEW DEFINITION` 和 `Sales` 服务器角色的  `Auditors` 权限。  
  
```sql 
USE master;  
DENY VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [授予服务器主体权限 (Transact-SQL)](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [撤消服务器主体权限 (Transact-SQL)](../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)   
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  
