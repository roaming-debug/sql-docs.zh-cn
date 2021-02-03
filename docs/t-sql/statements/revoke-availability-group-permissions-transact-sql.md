---
title: REVOKE 可用性组权限
description: 撤消对 Always On 可用性组的权限。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], permissions
- REVOKE statement, availability groups
- revoking permissions, [SQL Server], availability groups
- permissions [SQL Server], availability group
ms.assetid: 02c77378-a36d-4286-9235-d8867a2b92ad
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7ab08e1bded5d8171b7a07e49134277bfa508e07
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209909"
---
# <a name="revoke-availability-group-permissions-transact-sql"></a>REVOKE 可用性组权限 (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  撤消对 Always On 可用性组的权限。 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission  [ ,...n ]   
    ON AVAILABILITY GROUP :: availability_group_name  
    { FROM | TO } < server_principal >  [ ,...n ]  
    [ CASCADE ]  
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
 指定可以撤消的对可用性组的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON AVAILABILITY GROUP ::availability_group_name  
 指定要撤消权限的可用性组。 需要使用作用域限定符 (::)。  
  
 { FROM | TO } \<server_principal>：指定要撤消其权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_Windows_login  
 指定通过 Windows 登录帐户创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_certificate  
 指定映射到证书的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_AsymKey  
 指定映射到非对称密钥的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 GRANT OPTION  
 指示要撤消向其他主体授予指定权限的权限。 不会撤消该权限本身。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
 CASCADE  
 指示要撤消的权限也会从此主体授予或拒绝该权限的其他主体中撤消。  
  
> [!IMPORTANT]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 AS SQL_Server_login  
 指定执行此查询的主体从哪个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名派生其撤消该权限的权限。  
  
## <a name="remarks"></a>备注  
 只有在当前数据库为 master 时，才可撤消服务器作用域内权限。  
  
 可以在 [sys.availability_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md) 目录视图中查看可用性组的相关信息。 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中查看服务器权限的相关信息，在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中查看服务器主体的相关信息。  
  
 可用性组为服务器级安全对象。 下表列出了可撤消的对可用性组最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|可用性组权限|可用性组权限隐含的权限|服务器权限隐含的权限|  
|-----------------------------------|----------------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY AVAILABILITY GROUP|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要具有针对可用性组的 CONTROL 权限或针对服务器的 ALTER ANY AVAILABILITY GROUP 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-revoking-view-definition-permission-on-an-availability-group"></a>A. 撤消对可用性组的 VIEW DEFINITION 权限  
 以下示例从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `VIEW DEFINITION` 撤消对可用性组 `MyAg` 的 `ZArifin` 权限。  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON AVAILABILITY GROUP::MyAg TO ZArifin;  
GO  
```  
  
### <a name="b-revoking-take-ownership-permission-with-the-cascade"></a>B. 使用 CASCADE 撤消 TAKE OWNERSHIP 权限  
 以下示例从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户 `TAKE OWNERSHIP` 以及 `MyAg` 授予对 MyAg 的 TAKE OWNERSHIP 权限的所有主体撤消对可用性组 `PKomosinski` 的 `PKomosinski` 权限。  
  
```sql  
USE master;  
REVOKE TAKE OWNERSHIP ON AVAILABILITY GROUP::MyAg TO PKomosinski   
    CASCADE;  
GO  
```  
  
### <a name="c-revoking-a-previously-granted-with-grant-option-clause"></a>C. 撤消以前授予的 WITH GRANT OPTION 子句  
 如果使用 WITH GRANT OPTION 授予了权限，则使用 REVOKE GRANT OPTION FOR … 删除 WITH GRANT OPTION。 以下示例授予权限，然后删除权限的 WITH GRANT 部分。  
  
```sql  
USE master;  
GRANT CONTROL ON AVAILABILITY GROUP::MyAg TO PKomosinski   
    WITH GRANT OPTION;  
GO  
REVOKE GRANT OPTION FOR CONTROL ON AVAILABILITY GROUP::MyAg TO PKomosinski  
CASCADE  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT 可用性组权限 (Transact-SQL)](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)   
 [DENY 可用性组权限 (Transact-SQL)](../../t-sql/statements/deny-availability-group-permissions-transact-sql.md)   
 [CREATE AVAILABILITY GROUP (Transact-SQL)](../../t-sql/statements/create-availability-group-transact-sql.md)   
 [sys.availability_groups (Transact-SQL)](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [AlwaysOn 可用性组目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/always-on-availability-groups-catalog-views-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

