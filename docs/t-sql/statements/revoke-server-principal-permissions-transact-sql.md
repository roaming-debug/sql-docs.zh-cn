---
title: REVOKE 服务器主体权限
description: 撤消对 SQL Server 登录名的权限。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- REVOKE statement, impersonation
- permissions [SQL Server], impersonation
- permissions [SQL Server], logins
- impersonate [SQL Server], revoking
- logins [SQL Server], revoking
- REVOKE statement, logins
ms.assetid: 75409024-f150-4326-af16-9d60e900df18
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 062096e77c7e907594f7c2b9a372e3eb1899324a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185936"
---
# <a name="revoke-server-principal-permissions-transact-sql"></a>撤消服务器主体权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  撤消为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名授予或拒绝的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    { FROM | TO } <server_principal> [ ,...n ]  
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
 指定可对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名撤消的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 LOGIN ::SQL_Server_login  
 指定要撤消权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。 需要使用作用域限定符 (::)。  
  
 SERVER ROLE **::** *server_role*  
 指定要撤消权限的服务器角色。 需要使用作用域限定符 (::)。  
  
 { FROM | TO } \<server_principal> 指定要撤消权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或服务器角色。  
  
 SQL_Server_login  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_Windows_login  
 指定通过 Windows 登录帐户创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_certificate  
 指定映射到证书的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_AsymKey  
 指定映射到非对称密钥的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 server_role  
 指定用户定义的服务器角色的名称。  
  
 GRANT OPTION  
 指示要撤消向其他主体授予指定权限的权限。 不会撤消该权限本身。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
 CASCADE  
 指示要撤消的权限也会从此主体授予或拒绝该权限的其他主体中撤消。  
  
> [!CAUTION]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 AS SQL_Server_login  
 指定执行此查询的主体从哪个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名派生其撤消该权限的权限。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名和服务器角色是服务器级安全对象。 下表列出了可为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或服务器角色撤消的最具体的限定权限，以及隐含这些权限的更一般的权限。  
  
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
  
### <a name="a-revoking-impersonate-permission-on-a-login"></a>A. 撤消登录名的 IMPERSONATE 权限  
 以下示例从通过 Windows 用户 `AdvWorks\YoonM` 创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名中撤消对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `WanidaBenshoof` 的 `IMPERSONATE` 权限。  
  
```sql  
USE master;  
REVOKE IMPERSONATE ON LOGIN::WanidaBenshoof FROM [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-revoking-view-definition-permission-with-cascade"></a>B. 使用 CASCADE 撤消 VIEW DEFINITION 权限  
 以下示例从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `VIEW DEFINITION` 中撤消对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `EricKurjan` 的 `RMeyyappan` 权限。 `CASCADE` 选项指示还会从 `VIEW DEFINITION` 授予 `EricKurjan` 权限的主体中撤消对 `RMeyyappan` 的该权限。  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON LOGIN::EricKurjan FROM RMeyyappan   
    CASCADE;  
GO   
```  
  
### <a name="c-revoking-view-definition-permission-on-a-server-role"></a>C. 撤消服务器角色的 VIEW DEFINITION 权限  
 以下示例撤消 `VIEW DEFINITION` 和 `Sales` 服务器角色的  `Auditors` 权限。  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [授予服务器主体权限 (Transact-SQL)](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [拒绝服务器主体权限 (Transact-SQL)](../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [安全函数 (Transact-SQL)](../../t-sql/functions/security-functions-transact-sql.md)   
 [安全存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  

