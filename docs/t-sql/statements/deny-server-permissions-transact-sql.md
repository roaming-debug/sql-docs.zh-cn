---
description: DENY 服务器权限 (Transact-SQL)
title: DENY 服务器权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], servers
- denying permissions [SQL Server], servers
- servers [SQL Server], permissions
- DENY statement, servers
ms.assetid: 68d6b2a9-c36f-465a-9cd2-01d43a667e99
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 906e0a6d860b288a2f3eeda199564e54f61650e0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177697"
---
# <a name="deny-server-permissions-transact-sql"></a>DENY 服务器权限 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  拒绝服务器的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS <grantor_principal> ]   
  
<grantee_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
  
<grantor_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可对服务器拒绝的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 CASCADE  
 指示拒绝授予指定主体该权限，同时，对该主体授予了该权限的所有其他主体，也拒绝授予该权限。 当主体具有带 GRANT OPTION 的权限时，为必选项。 
  
 TO \<server_principal>  
 指定对其拒绝权限的主体。  
  
 AS \<grantor_principal>  
 指定执行此查询的主体要从哪个主体派生其拒绝该权限的权利。
使用 AS principal 子句指示：记录为权限的拒绝者的主体应为执行该语句的用户以外的主体。 例如，假设用户 Mary 是 principal_id 12，用户 Raul 是主体 15。 Mary 执行 `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;` 现在，即使语句的实际执行者是用户 13 (Mary)，sys.database_permissions 表仍将指示 deny 语句的 grantor_prinicpal_id 为 15 (Raul)。
  
在此语句中使用 AS 并不意味着能够模拟其他用户。    
  
 SQL_Server_login  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login_mapped_to_Windows_login  
 指定映射到 Windows 登录名的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login_mapped_to_Windows_group  
 指定映射到 Windows 组的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login_mapped_to_certificate  
 指定映射到证书的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login_mapped_to_asymmetric_key  
 指定映射到非对称密钥的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 server_role  
 指定服务器角色。  
  
## <a name="remarks"></a>备注  
 只有在当前数据库为 master 时，才可拒绝其服务器范围内的权限。  
  
 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中查看有关服务器权限的信息；在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中查看有关服务器主体的信息。 以及在 [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) 目录视图中查看有关服务器角色成员身份的信息。  
  
 服务器是权限层次结构的最高级别。 下表列出了可拒绝的对服务器最为具体的限定权限。  
  
|服务器权限|服务器权限隐含的权限|  
|-----------------------|----------------------------------|  
|ADMINISTER BULK OPERATIONS|CONTROL SERVER|  
|ALTER ANY AVAILABILITY GROUP<br /><br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|CONTROL SERVER|  
|ALTER ANY CONNECTION|CONTROL SERVER|  
|ALTER ANY CREDENTIAL|CONTROL SERVER|  
|ALTER ANY DATABASE|CONTROL SERVER|  
|ALTER ANY ENDPOINT|CONTROL SERVER|  
|ALTER ANY EVENT NOTIFICATION|CONTROL SERVER|  
|ALTER ANY EVENT SESSION|CONTROL SERVER|  
|ALTER ANY LINKED SERVER|CONTROL SERVER|  
|ALTER ANY LOGIN|CONTROL SERVER|  
|ALTER ANY SERVER AUDIT|CONTROL SERVER|  
|ALTER ANY SERVER ROLE<br /><br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|CONTROL SERVER|  
|ALTER RESOURCES|CONTROL SERVER|  
|ALTER SERVER STATE|CONTROL SERVER|  
|ALTER SETTINGS|CONTROL SERVER|  
|ALTER TRACE|CONTROL SERVER|  
|AUTHENTICATE SERVER|CONTROL SERVER|  
|CONNECT ANY DATABASE<br /><br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|CONTROL SERVER|  
|CONNECT SQL|CONTROL SERVER|  
|CONTROL SERVER|CONTROL SERVER|  
|CREATE ANY DATABASE|ALTER ANY DATABASE|  
|CREATE AVAILABILITY GROUP<br /><br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|ALTER ANY AVAILABILITY GROUP|  
|CREATE DDL EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|CREATE ENDPOINT|ALTER ANY ENDPOINT|  
|CREATE SERVER ROLE<br /><br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|ALTER ANY SERVER ROLE|  
|CREATE TRACE EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|EXTERNAL ACCESS ASSEMBLY|CONTROL SERVER|  
|IMPERSONATE ANY LOGIN<br /><br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|CONTROL SERVER|  
|SELECT ALL USER SECURABLES<br /><br /> **适用范围**： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 [当前版本](/troubleshoot/sql/general/determine-version-edition-update-level)）。|CONTROL SERVER|  
|SHUTDOWN|CONTROL SERVER|  
|UNSAFE ASSEMBLY|CONTROL SERVER|  
|VIEW ANY DATABASE|VIEW ANY DEFINITION|  
|VIEW ANY DEFINITION|CONTROL SERVER|  
|VIEW SERVER STATE|ALTER SERVER STATE|  
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 中添加了以下三个服务器权限。  
  
 CONNECT ANY DATABASE 权限  
 将 CONNECT ANY DATABASE 授予某个登录名，该登录名必须连接到当前存在的所有数据库和将来可能创建的任何新数据库。 不要在任何数据库中授予超过连接的任何权限。 与 SELECT ALL USER SECURABLES 或 VIEW SERVER STATE 结合使用，可审核进程查看所有数据或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的所有数据库状态。   
  
 IMPERSONATE ANY LOGIN 权限  
 授予后，当连接到数据库时，允许中间层进程模拟连接到它的客户端帐户。 被拒绝时，高特权的登录名可以阻止模拟其他登录名。 例如，可通过模拟其他登录名来阻止具有 CONTROL SERVER 权限的登录名。  
  
 SELECT ALL USER SECURABLES 权限  
 授予后，作者等登录名可以查看用户可连接到的所有数据库中的数据。 被拒绝时，阻止访问对象，除非这些对象处于 sys 架构中。  
  
## <a name="permissions"></a>权限  
 要求具有 CONTROL SERVER 权限或者安全对象的所有权。 如果使用 AS 子句，则指定的主体必须拥有要对其拒绝权限的安全对象。  
  
## <a name="examples"></a>示例  
  
### <a name="a-denying-connect-sql-permission-to-a-sql-server-login-and-principals-to-which-the-login-has-regranted-it"></a>A. 对 SQL Server 登录名和该登录名重新授予 CONNECT SQL 权限的主体拒绝该权限  
 以下示例对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `CONNECT SQL` 以及该登录名授予 `Annika` 权限的主体拒绝该权限。  
  
```sql  
USE master;  
DENY CONNECT SQL TO Annika CASCADE;  
GO  
```  
  
### <a name="b-denying-create-endpoint-permission-to-a-sql-server-login-using-the-as-option"></a>B. 使用 AS 选项对 SQL Server 登录名拒绝 CREATE ENDPOINT 权限  
 以下示例对用户 `CREATE ENDPOINT` 拒绝 `ArifS` 权限。 该示例使用 `AS` 选项指定 `MandarP` 作为执行主体从中派生执行权限的主体。  
  
```sql  
USE master;  
DENY CREATE ENDPOINT TO ArifS AS MandarP;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [DENY 服务器权限 (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE 服务器权限 (Transact-SQL)](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
