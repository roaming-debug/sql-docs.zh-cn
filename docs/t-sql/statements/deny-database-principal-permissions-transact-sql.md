---
title: DENY 数据库主体权限
description: 拒绝对数据库用户、数据库角色或应用程序角色的权限。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 05/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- database roles [SQL Server], permissions
- denying permissions [SQL Server], database roles
- denying permissions [SQL Server], database users
- permissions [SQL Server], database roles
- DENY statement, database roles
- database user permissions [SQL Server]
- permissions [SQL Server], application roles
- permissions [SQL Server], database users
- database permissions [SQL Server], denying
- DENY statement, application roles
- DENY statement, database users
- denying permissions [SQL Server], application roles
- application roles [SQL Server], permissions
ms.assetid: e2429a5d-e9be-4c05-be20-414d1038a63a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ff2c2feec318fb8bc53d3218ca81adcc6a3aaf5a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177741"
---
# <a name="deny-database-principal-permissions-transact-sql"></a>DENY 数据库主体权限 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  拒绝授予的对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中数据库用户、数据库角色或应用程序角色的权限。  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission [ ,...n ]    
    ON   
    {  [ USER :: database_user ]  
     | [ ROLE :: database_role ]  
     | [ APPLICATION ROLE :: application_role ]  
    }  
    TO <database_principal> [ ,...n ]  
      [ CASCADE ]  
      [ AS <database_principal> ]  
  
<database_principal> ::=  
    Database_user   
  | Database_role   
  | Application_role   
  | Database_user_mapped_to_Windows_User   
  | Database_user_mapped_to_Windows_Group   
  | Database_user_mapped_to_certificate   
  | Database_user_mapped_to_asymmetric_key   
  | Database_user_with_no_login   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可以拒绝的对数据库主体的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 USER ::database_user  
 指定要拒绝对其权限的用户的类和名称。 需要使用作用域限定符 (::)。  
  
 ROLE ::database_role  
 指定要拒绝对其权限的角色的类和名称。 需要使用作用域限定符 (::)。  
  
 APPLICATION ROLE ::application_role  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 指定要拒绝对其权限的应用程序角色的类和名称。 需要使用作用域限定符 (::)。  
  
 CASCADE  
 指示要拒绝的权限也会被对此主体授予该权限的其他主体拒绝。  
  
 AS \<database_principal>  
 指定一个主体，执行该查询的主体从该主体获得撤消该权限的权利。  
  
 Database_user  
 指定数据库用户。  
  
 Database_role  
 指定数据库角色。  
  
 Application_role  
 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
 指定应用程序角色。  
  
 Database_user_mapped_to_Windows_User  
 指定映射到 Windows 用户的数据库用户。  
  
 Database_user_mapped_to_Windows_Group  
 指定映射到 Windows 组的数据库用户。  
  
 Database_user_mapped_to_certificate  
  指定映射到证书的数据库用户。  
  
 Database_user_mapped_to_asymmetric_key  
  指定映射到非对称密钥的数据库用户。  
  
 Database_user_with_no_login  
 指定无相应服务器级主体的数据库用户。  
  
## <a name="remarks"></a>备注  
  
## <a name="database-user-permissions"></a>数据库用户权限  
 数据库用户是一个数据库级的安全对象，包含于权限层次结构中作为其父级的数据库中。 下表列出了可拒绝的对数据库用户最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|数据库用户权限|数据库用户权限隐含的权限|数据库权限隐含的权限|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|IMPERSONATE|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY USER|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="database-role-permissions"></a>数据库角色权限  
 数据库角色是一个数据库级的安全对象，包含于权限层次结构中作为其父级的数据库中。 下表列出了可以拒绝的对数据库角色的最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|数据库角色权限|数据库角色权限隐含的权限|数据库权限隐含的权限|  
|------------------------------|-----------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="application-role-permissions"></a>应用程序角色权限  
 应用程序角色是一个数据库级的安全对象，包含于权限层次结构中作为其父级的数据库中。 下表列出了可以拒绝的对应用程序角色的最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|应用程序角色权限|应用程序角色权限隐含的权限|数据库权限隐含的权限|  
|---------------------------------|--------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY APPLICATION ROLE|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对指定主体的 CONTROL 权限或隐含 CONTROL 权限的更高权限。  
  
 数据库的 CONTROL 权限的被授权者（例如，db_owner 固定数据库角色的成员）可以拒绝对数据库中任一个安全对象的任意权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-denying-control-permission-on-a-user-to-another-user"></a>A. 拒绝一个用户对另一个用户的 CONTROL 权限  
 以下示例拒绝用户 `CONTROL` 对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 用户 `Wanida` 的 `RolandX` 权限。  
  
```sql 
USE AdventureWorks2012;  
DENY CONTROL ON USER::Wanida TO RolandX;  
GO  
```  
  
### <a name="b-denying-view-definition-permission-on-a-role-to-a-user-to-which-it-was-granted-with-grant-option"></a>B. 拒绝被通过 GRANT OPTION 授予了 VIEW DEFINITION 权限的用户对角色的该权限  
 以下示例拒绝数据库用户 `VIEW DEFINITION` 对 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 角色 `SammamishParking` 的 `JinghaoLiu` 权限。 指定了 `CASCADE` 选项，因为用户 `JinghaoLiu` 被通过 WITH GRANT OPTION 授予了 VIEW DEFINITION 权限。  
  
```sql  
USE AdventureWorks2012;  
DENY VIEW DEFINITION ON ROLE::SammamishParking   
    TO JinghaoLiu CASCADE;  
GO  
```  
  
### <a name="c-denying-impersonate-permission-on-a-user-to-an-application-role"></a>C. 拒绝应用程序角色对用户的 IMPERSONATE 权限  
 以下示例拒绝 `IMPERSONATE` 应用程序角色 `HamithaL` 对用户 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 的 `AccountsPayable17` 权限。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。  
  
```sql  
USE AdventureWorks2012;  
DENY IMPERSONATE ON USER::HamithaL TO AccountsPayable17;  
GO    
```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT 数据库主体权限 (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)   
 [REVOKE 数据库主体权限 (Transact-SQL)](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ROLE (Transact-SQL)](../../t-sql/statements/create-role-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
