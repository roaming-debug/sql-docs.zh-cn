---
title: GRANT 可用性组权限
description: 授予对 Always On 可用性组的权限。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], permissions
- GRANT statement, availability groups
- granting permissions, [SQL Server], availability groups
- permissions [SQL Server], availability group
ms.assetid: 060eb839-666a-4046-9e1d-5edc9ea75a11
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: d1047980f83de32c5631c7d45673fe743607ebbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208167"
---
# <a name="grant-availability-group-permissions-transact-sql"></a>GRANT 可用性组权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授予对 Always On 可用性组的权限。  
  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GRANT permission  [ ,...n ] ON AVAILABILITY GROUP :: availability_group_name  
        TO < server_principal >  [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
        SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可以授予的对可用性组的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON AVAILABILITY GROUP ::availability_group_name  
 指定要授予权限的可用性组。 需要使用作用域限定符 (::)。  
  
 TO \<server_principal>  
 指定要对其授予权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_Windows_login  
 指定通过 Windows 登录帐户创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_certificate  
 指定映射到证书的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_AsymKey  
 指定映射到非对称密钥的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 WITH GRANT OPTION  
 指示该主体还可以向其他主体授予所指定的权限。  
  
 AS SQL_Server_login  
 指定执行此查询的主体要从哪个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名派生其授予该权限的权限。  
  
## <a name="remarks"></a>备注  
 只有在当前数据库为 master 时，才可授予其服务器作用域内的权限。  
  
 可以在 [sys.availability_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) 目录视图中查看可用性组的相关信息。 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中查看服务器权限的相关信息，在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中查看服务器主体的相关信息。  
  
 可用性组为服务器级安全对象。 下表列出了可授予的对可用性组最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|可用性组权限|可用性组权限隐含的权限|服务器权限隐含的权限|  
|-----------------------------------|----------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY AVAILABILITY GROUP|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
 有关所有[!INCLUDE[ssDE](../../includes/ssde-md.md)]权限的图表，请参阅[数据库引擎权限招贴](https://aka.ms/sql-permissions-poster)。  
  
## <a name="permissions"></a>权限  
 需要具有针对可用性组的 CONTROL 权限或针对服务器的 ALTER ANY AVAILABILITY GROUP 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-granting-view-definition-permission-on-an-availability-group"></a>A. 授予对可用性组的 VIEW DEFINITION 权限  
 以下示例将对可用性组 `VIEW DEFINITION` 的 `MyAg` 权限授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `ZArifin`。  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON AVAILABILITY GROUP::MyAg TO ZArifin;  
GO  
```  
  
### <a name="b-granting-take-ownership-permission-with-the-grant-option"></a>B. 使用 GRANT OPTION 授予 TAKE OWNERSHIP 权限  
 以下示例使用 `TAKE OWNERSHIP`，将对可用性组 `MyAg` 的 `PKomosinski` 权限授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户 `GRANT OPTION`。  
  
```sql  
USE master;  
GRANT TAKE OWNERSHIP ON AVAILABILITY GROUP::MyAg TO PKomosinski   
    WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-control-permission-on-an-availability-group"></a>C. 授予对可用性组的 CONTROL 权限  
 以下示例将对可用性组 `CONTROL` 的 `MyAg` 权限授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户 `PKomosinski`。 CONTROL 允许登录名完全控制可用性组，即使它们不是可用性组的所有者。 若要更改所有权，请参阅 [ALTER AUTHORIZATION (Transact-SQL)](../../t-sql/statements/alter-authorization-transact-sql.md)。  
  
```sql  
USE master;  
GRANT CONTROL ON AVAILABILITY GROUP::MyAg TO PKomosinski;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [REVOKE 可用性组权限 (Transact-SQL)](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)   
 [DENY 可用性组权限 (Transact-SQL)](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [sys.availability_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md) [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
