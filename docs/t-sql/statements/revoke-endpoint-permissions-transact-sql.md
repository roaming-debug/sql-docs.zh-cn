---
description: REVOKE 端点权限 (Transact-SQL)
title: REVOKE 终结点权限 (Transact-SQL) | Microsoft Docs
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
- endpoints [SQL Server], permissions
- REVOKE statement, endpoints
- permissions [SQL Server], endpoints
ms.assetid: 826f513e-9ad0-46b9-87ad-7525713638c8
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7a672938bacfa9555ea988c9bc68621a4aab9155
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209891"
---
# <a name="revoke-endpoint-permissions-transact-sql"></a>REVOKE 端点权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  撤消对端点授予或拒绝的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ]   
    ON ENDPOINT :: endpoint_name  
    { FROM | TO } <server_principal> [ ,...n ]  
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
 指定可对端点授予的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON ENDPOINT ::endpoint_name  
 指定要授予对其的权限的端点。 需要使用作用域限定符 (::)。  
  
 { FROM | TO } \<server_principal> 指定要从中撤消权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
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
  
> [!CAUTION]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 AS SQL_Server_login  
 指定执行此查询的主体从哪个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名派生其撤消该权限的权限。  
  
## <a name="remarks"></a>备注  
 只有在当前数据库为 master 时，才可撤消服务器作用域内权限。  
  
 可以在 [sys.endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md) 目录视图中查看终结点的相关信息。 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中查看服务器权限的相关信息，在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中查看服务器主体的相关信息。  
  
 端点为服务器级安全对象。 下表列出了可撤消的对端点最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|端点权限|端点权限隐含的权限|服务器权限隐含的权限|  
|-------------------------|------------------------------------|----------------------------------|  
|ALTER|CONTROL|ALTER ANY ENDPOINT|  
|CONNECT|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对端点的 CONTROL 权限或对服务器的 ALTER ANY ENDPOINT 权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-revoking-view-definition-permission-on-an-endpoint"></a>A. 撤消对端点的 VIEW DEFINITION 权限  
 以下示例从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `VIEW DEFINITION` 中撤消对端点 `Mirror7` 的 `ZArifin` 权限。  
  
```sql  
USE master;  
REVOKE VIEW DEFINITION ON ENDPOINT::Mirror7 FROM ZArifin;  
GO  
```  
  
### <a name="b-revoking-take-ownership-permission-with-the-cascade-option"></a>B. 使用 CASCADE 选项撤消 TAKE OWNERSHIP 权限  
 以下示例从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户 `PKomosinski` 以及 `PKomosinski` 授予对 `Shipping83` 的 `TAKE OWNERSHIP` 权限的所有主体中撤消对终结点 `Shipping83` 的 `TAKE OWNERSHIP` 权限。  
  
```sql  
USE master;  
REVOKE TAKE OWNERSHIP ON ENDPOINT::Shipping83 FROM PKomosinski   
    CASCADE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [DENY 终结点权限 (Transact-SQL)](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [sys.endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

