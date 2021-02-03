---
description: GRANT 服务器权限 (Transact-SQL)
title: GRANT 服务器权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- GRANT statement, servers
- permissions [SQL Server], servers
- servers [SQL Server], permissions
- granting permissions [SQL Server], servers
ms.assetid: 7e880a5a-3bdc-491f-a167-7a9ed338be7f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 436ede592487a7f70a01a5ef72f5b359b250ac4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161187"
---
# <a name="grant-server-permissions-transact-sql"></a>GRANT 服务器权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授予服务器的权限。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
GRANT permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ] [ WITH GRANT OPTION ]  
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
 指定可对服务器授予的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 TO \<grantee_principal> 指定要向其授予权限的主体。  
  
 AS \<grantor_principal> 指定执行此查询的主体要从哪个主体派生其授予该权限的权限。  
  
 WITH GRANT OPTION  
 指示该主体还可以向其他主体授予所指定的权限。  
  
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
 指定用户定义的服务器角色。  
  
## <a name="remarks"></a>备注  
 只有在当前数据库为 master 时，才可授予其服务器作用域内的权限。  
  
 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中查看有关服务器权限的信息；在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中查看有关服务器主体的信息。 以及在 [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md) 目录视图中查看有关服务器角色成员身份的信息。  
  
 服务器是权限层次结构的最高级别。 下表列出了可授予的对服务器最为具体的限定权限。  
  
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
 授权者（或使用 AS 选项指定的主体）必须具有使用 GRANT OPTION 授予的权限本身，或具有隐含授予该权限的更高权限。 sysadmin 固定服务器角色成员可以授予任何权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-granting-a-permission-to-a-login"></a>A. 为登录名授予权限  
 以下示例将 `CONTROL SERVER` 权限授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `TerryEminhizer`。  
  
```sql  
USE master;  
GRANT CONTROL SERVER TO TerryEminhizer;  
GO  
```  
  
### <a name="b-granting-a-permission-that-has-grant-permission"></a>B. 授予具有 GRANT 权限的权限  
 以下示例为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `ALTER ANY EVENT NOTIFICATION` 授予 `JanethEsteves` 权限以及为其他登录名授予该权限的权限。  
  
```sql  
USE master;  
GRANT ALTER ANY EVENT NOTIFICATION TO JanethEsteves WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-a-permission-to-a-server-role"></a>C. 为服务器角色授予权限  
 以下示例创建两个名为 `ITDevAdmin` 和 `ITDevelopers` 的服务器角色。 它为用户定义的服务器角色 `ALTER ANY DATABASE` 授予 `ITDevAdmin` 权限（包括 `WITH GRANT` 选项），以便 `ITDevAdmin` 服务器角色可以重新分配 `ALTER ANY DATABASE` 权限。 然后，该示例为 `ITDevelopers` 授予权限以使用 `ALTER ANY DATABASE` 服务器角色的 `ITDevAdmin` 权限。  
  
```sql  
USE master;  
CREATE SERVER ROLE ITDevAdmin ;  
CREATE SERVER ROLE ITDevelopers ;  
GRANT ALTER ANY DATABASE TO ITDevAdmin WITH GRANT OPTION ;  
GRANT ALTER ANY DATABASE TO ITDevelopers AS ITDevAdmin ;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [DENY 服务器权限 (Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE 服务器权限 (Transact-SQL)](../../t-sql/statements/revoke-server-permissions-transact-sql.md)   
 [权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
