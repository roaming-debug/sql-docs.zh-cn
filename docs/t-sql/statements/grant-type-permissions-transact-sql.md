---
description: GRANT 类型权限 (Transact-SQL)
title: GRANT 类型权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], types
- granting permissions [SQL Server], types
- GRANT statement, types
- type permissions [SQL Server]
ms.assetid: 14bd2fb3-1446-49c0-be87-c6a670317ed0
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c562aac8a5b0922c235ec8c24d170fdf19c4d088
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180152"
---
# <a name="grant-type-permissions-transact-sql"></a>GRANT 类型权限 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  授予对类型的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GRANT permission  [ ,...n ] ON TYPE :: [ schema_name . ] type_name  
    TO <database_principal> [ ,...n ]  
    [ WITH GRANT OPTION ]  
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
 指定可以授予的类型权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON TYPE :: [ schema_name.  ] type_name  
 指定要授予其权限的类型。 需要使用作用域限定符 (::)。 如果未指定 schema_name，则使用默认架构。 如果指定了 schema_name，则需要使用架构作用域限定符 (.)。  
  
 TO \<database_principal> 指定要向其授予权限的主体。  
  
 WITH GRANT OPTION  
 指示该主体还可以向其他主体授予所指定的权限。  
  
 AS \<database_principal>：指定一个主体，执行该查询的主体从中获得授予权限的权利。  
  
 Database_user  
 指定数据库用户。  
  
 Database_role  
 指定数据库角色。  
  
 Application_role  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
 指定应用程序角色。  
  
 Database_user_mapped_to_Windows_User  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 指定映射到 Windows 用户的数据库用户。  
  
 Database_user_mapped_to_Windows_Group  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 指定映射到 Windows 组的数据库用户。  
  
 Database_user_mapped_to_certificate  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 指定映射到证书的数据库用户。  
  
 Database_user_mapped_to_asymmetric_key  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 指定映射到非对称密钥的数据库用户。  
  
 Database_user_with_no_login  
 指定无相应服务器级主体的数据库用户。  
  
## <a name="remarks"></a>备注  
 类型是架构级的安全对象，包含于权限层次结构中作为其父级的架构中。  
  
> [!IMPORTANT]  
>  GRANT、DENY 和 REVOKE 权限不适用于系统类型  。 可以为用户定义类型授予权限。 有关用户定义类型的详细信息，请参阅[使用 SQL Server 中的用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)。  
  
 下表列出了可授予的对类型最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|类型权限|类型权限隐含的权限|架构权限隐含的权限|  
|---------------------|--------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|EXECUTE|CONTROL|EXECUTE|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 授权者（或用 AS 选项指定的主体）必须具有带 GRANT OPTION 的相同权限，或具有隐含所授予权限的更高权限。  
  
 若要使用 AS 选项，还必须满足以下附加要求：  
  
|AS|所需的其他权限|  
|--------|------------------------------------|  
|数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到 Windows 登录名的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到 Windows 组的数据库用户|Windows 组的成员身份、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到证书的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到非对称密钥的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|未映射到任何服务器主体的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|数据库角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|应用程序角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
  
## <a name="examples"></a>示例  
 以下示例使用 `VIEW DEFINITION` 授予用户 `GRANT OPTION` 对用户定义类型 `PhoneNumber` 的 `KhalidR` 权限。 `PhoneNumber` 位于架构 `Telemarketing` 中。  
  
```sql  
GRANT VIEW DEFINITION ON TYPE::Telemarketing.PhoneNumber   
    TO KhalidR WITH GRANT OPTION;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [DENY 类型权限 (Transact-SQL)](../../t-sql/statements/deny-type-permissions-transact-sql.md)   
 [REVOKE 类型权限 (Transact-SQL)](../../t-sql/statements/revoke-type-permissions-transact-sql.md)   
 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [安全对象](../../relational-databases/security/securables.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

