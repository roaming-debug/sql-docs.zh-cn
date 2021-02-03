---
description: DENY 类型权限 (Transact-SQL)
title: DENY 类型权限 (Transact-SQL) | Microsoft Docs
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
- DENY statement, types
- permissions [SQL Server], types
- type permissions [SQL Server]
- denying permissions [SQL Server], types
ms.assetid: 564e3500-c567-43dc-993b-9ab50e99cf3f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 18aa1ff5bbfdf44d17a5b107f07c623f60f2a4bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211682"
---
# <a name="deny-type-permissions-transact-sql"></a>DENY 类型权限 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  拒绝对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的类型的权限。  

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission  [ ,...n ] ON TYPE :: [ schema_name . ] type_name  
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
 指定可对类型拒绝的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON TYPE :: [ schema_name.  ] type_name  
 指定要对其拒绝权限的类型。 需要使用作用域限定符 (::)。 如果未指定 schema_name，则使用默认架构。 如果指定了 schema_name，则需要使用架构作用域限定符 (.)。  
  
 TO \<database_principal>  
 指定要对其拒绝权限的主体。  
  
 CASCADE  
 指示要拒绝的权限也会被对此主体授予该权限的其他主体拒绝。  
  
 AS \<database_principal>  
 指定一个主体，执行该查询的主体从该主体获得拒绝授予该权限的权利。  
  
 Database_user  
 指定数据库用户。  
  
 Database_role  
 指定数据库角色。  
  
 Application_role  
   
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
 类型是架构级的安全对象，包含于权限层次结构中作为其父级的架构中。  
  
> [!IMPORTANT]  
>  GRANT、DENY 和 REVOKE 权限不适用于系统类型  。 可以为用户定义类型授予权限。 有关用户定义类型的详细信息，请参阅[使用 SQL Server 中的用户定义类型](../../relational-databases/clr-integration-database-objects-user-defined-types/working-with-user-defined-types-in-sql-server.md)。  
  
 下表列出了可拒绝的对类型最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|类型权限|类型权限隐含的权限|架构权限隐含的权限|  
|---------------------|--------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|EXECUTE|CONTROL|EXECUTE|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对类型的 CONTROL 权限。 如果使用 AS 子句，则指定的主体必须拥有要对其拒绝权限的类型。  
  
## <a name="examples"></a>示例  
 以下示例使用 `VIEW DEFINITION` 拒绝 `CASCADE` 对用户定义类型 `PhoneNumber` 的 `KhalidR` 权限。 `PhoneNumber` 位于架构 `Telemarketing` 中。  
  
```sql 
DENY VIEW DEFINITION ON TYPE::Telemarketing.PhoneNumber   
    TO KhalidR CASCADE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [GRANT 类型权限 (Transact-SQL)](../../t-sql/statements/grant-type-permissions-transact-sql.md)   
 [REVOKE 类型权限 (Transact-SQL)](../../t-sql/statements/revoke-type-permissions-transact-sql.md)   
 [CREATE TYPE (Transact-SQL)](../../t-sql/statements/create-type-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [安全对象](../../relational-databases/security/securables.md)  
  
  
