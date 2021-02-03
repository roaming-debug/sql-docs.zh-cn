---
description: GRANT 端点权限 (Transact-SQL)
title: GRANT 终结点权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- endpoints [SQL Server], permissions
- granting permissions [SQL Server], endpoints
- GRANT statement, endpoints
- permissions [SQL Server], endpoints
ms.assetid: 9eda885c-fc3a-4c9d-8de6-ce07fb35a934
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 7c2a366e55f27475b2bda9e0189ed079f8b796d2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205776"
---
# <a name="grant-endpoint-permissions-transact-sql"></a>GRANT 端点权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  授予对端点的权限。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GRANT permission  [ ,...n ] ON ENDPOINT :: endpoint_name  
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
 指定可对端点授予的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON ENDPOINT ::endpoint_name  
 指定要授予对其的权限的端点。 需要使用作用域限定符 (::)。  
  
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
  
 可以在 [sys.endpoints](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md) 目录视图中查看终结点的相关信息。 可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 目录视图中查看服务器权限的相关信息，在 [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md) 目录视图中查看服务器主体的相关信息。  
  
 端点为服务器级安全对象。 下表列出了可授予的对端点最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
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
  
### <a name="a-granting-view-definition-permission-on-an-endpoint"></a>A. 授予对端点的 VIEW DEFINITION 权限  
 以下示例将对端点 `VIEW DEFINITION` 的 `Mirror7` 权限授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名 `ZArifin`。  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON ENDPOINT::Mirror7 TO ZArifin;  
GO  
```  
  
### <a name="b-granting-take-ownership-permission-with-the-grant-option"></a>B. 使用 GRANT OPTION 授予 TAKE OWNERSHIP 权限  
 以下示例使用 `TAKE OWNERSHIP`，将对端点 `Shipping83` 的 `PKomosinski` 权限授予 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户 `GRANT OPTION`。  
  
```sql  
USE master;  
GRANT TAKE OWNERSHIP ON ENDPOINT::Shipping83 TO PKomosinski   
    WITH GRANT OPTION;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DENY 终结点权限 (Transact-SQL)](../../t-sql/statements/deny-endpoint-permissions-transact-sql.md)   
 [REVOKE 终结点权限 (Transact-SQL)](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)   
 [CREATE ENDPOINT (Transact-SQL)](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)   
 [sys.endpoints (Transact-SQL)](../../relational-databases/system-catalog-views/sys-endpoints-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
