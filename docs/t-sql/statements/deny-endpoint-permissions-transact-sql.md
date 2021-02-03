---
description: DENY 端点权限 (Transact-SQL)
title: DENY 终结点权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- endpoints [SQL Server], permissions
- DENY statement, endpoints
- denying permissions [SQL Server], endpoints
- permissions [SQL Server], endpoints
ms.assetid: 3ac40457-7529-4eda-95a4-5247345cc8cf
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2fc6110c828ec008a132d1ecfc2daf8dc6159237
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177703"
---
# <a name="deny-endpoint-permissions-transact-sql"></a>DENY 端点权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  拒绝对端点的权限。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission  [ ,...n ] ON ENDPOINT :: endpoint_name  
    TO < server_principal >  [ ,...n ]  
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
 指定可对端点拒绝的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON ENDPOINT ::endpoint_name  
 指定要对其拒绝权限的端点。 需要使用作用域限定符 (::)。  
  
 TO \<server_principal>  
 指定要拒绝权限的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名。  
  
 SQL_Server_login  
 指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_Windows_login  
 指定通过 Windows 登录帐户创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_certificate  
 指定映射到证书的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 SQL_Server_login_from_AsymKey  
 指定映射到非对称密钥的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录帐户的名称。  
  
 CASCADE  
 指示要拒绝的权限也会被对此主体授予该权限的其他主体拒绝。  
  
 AS SQL_Server_login  
 指定执行此查询的主体要从哪个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名派生其拒绝该权限的权限。  
  
## <a name="remarks"></a>备注  
 只有在当前数据库为 master 时，才可拒绝其服务器作用域内的权限。  
  
 可以在 [sys.endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md) 目录视图中查看终结点的相关信息。 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中查看服务器权限的相关信息，在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中查看服务器主体的相关信息。  
  
 端点为服务器级安全对象。 下表列出了可拒绝的对端点最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
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
  
### <a name="a-denying-view-definition-permission-on-an-endpoint"></a>A. 拒绝对端点的 VIEW DEFINITION 权限  
 以下示例拒绝 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `ZArifin` 对终结点 `Mirror7` 的 `VIEW DEFINITION` 权限。  
  
```sql  
USE master;  
DENY VIEW DEFINITION ON ENDPOINT::Mirror7 TO ZArifin;  
GO  
```  
  
### <a name="b-denying-take-ownership-permission-with-cascade-option"></a>B. 使用 CASCADE 选项拒绝 TAKE OWNERSHIP 权限  
 以下示例拒绝 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户 `TAKE OWNERSHIP` 以及 `Shipping83` 授予 `PKomosinski` 权限的主体对端点 `PKomosinski` 的 `TAKE OWNERSHIP` 权限。  
  
```sql  
USE master;  
DENY TAKE OWNERSHIP ON ENDPOINT::Shipping83 TO PKomosinski   
    CASCADE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [REVOKE 终结点权限 (Transact-SQL)](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [sys.endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
