---
description: REVOKE 对称密钥权限 (Transact-SQL)
title: REVOKE 对称密钥权限 (Transact-SQL) | Microsoft Docs
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
- symmetric keys [SQL Server], permissions
- permissions [SQL Server], symmetric keys
- REVOKE statement, symmetric keys
ms.assetid: 091da030-a768-4aa3-9509-cc23bd719cea
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ec4926f6f41eff66770fc74f6bb4fa9f940a4364
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210951"
---
# <a name="revoke-symmetric-key-permissions-transact-sql"></a>REVOKE 对称密钥权限 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  撤消对对称密钥授予和拒绝的权限。  
   
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] permission [ ,...n ]    
    ON SYMMETRIC KEY :: symmetric_key_name   
        { TO | FROM } <database_principal> [ ,...n ]   
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
 指定可对对称密钥撤消的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON SYMMETRIC KEY ::asymmetric_key_name  
 指定要对其撤消权限的对称密钥。 需要作用域限定符 ::。  
  
 GRANT OPTION  
 指示要撤消向其他主体授予指定权限的权限。 不会撤消该权限本身。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
 CASCADE  
 指示要撤消的权限也会从此主体授予或拒绝该权限的其他主体中撤消。  
  
> [!CAUTION]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 { TO | FROM } \<*database_principal*>  
 指定要从中撤消权限的主体。  
  
 AS \<database_principal>：指定一个主体，执行该查询的主体从中获得撤消权限的权利。  
  
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
 可以在 [sys.symmetric_keys](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md) 目录视图中查看对称密钥的有关信息。  
  
 如果从通过指定 GRANT OPTION 被授权的主体中撤消该权限，但是未指定 CASCADE，则语句将失败。  
  
 对称密钥是一个数据库级的安全对象，包含于权限层次结构中作为其父级的数据库中。 下表列出了可授予的对对称密钥最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|对称密钥权限|对称密钥权限隐含的权限|数据库权限隐含的权限|  
|------------------------------|-----------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SYMMETRIC KEY|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对对称密钥的 CONTROL 权限或对数据库的 ALTER ANY SYMMETRIC KEY 权限。 若要使用 AS 选项，则指定的主体必须拥有对称密钥。  
  
## <a name="examples"></a>示例  
 以下示例从用户 `ALTER` 以及 `SamInventory42` 已授予 `HamidS` 权限的其他主体中撤消对对称密钥 `HamidS` 的 `ALTER` 权限。  
  
```sql  
USE AdventureWorks2012;  
REVOKE ALTER ON SYMMETRIC KEY::SamInventory42 TO HamidS CASCADE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.symmetric_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-symmetric-keys-transact-sql.md)   
 [GRANT 对称密钥权限 (Transact-SQL)](../../t-sql/statements/grant-symmetric-key-permissions-transact-sql.md)   
 [DENY 对称密钥权限 (Transact-SQL)](../../t-sql/statements/deny-symmetric-key-permissions-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  

