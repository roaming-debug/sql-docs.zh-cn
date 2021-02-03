---
title: DENY 搜索属性列表权限
description: 拒绝对搜索属性列表的权限。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], permissions
- DENY statement, search property list permissions
- denying permissions [SQL Server], search property lists
- search property lists [SQL Server], permissions
ms.assetid: 96513cb4-a9c0-4834-97a4-ddc0777b8415
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6e5ec634f87010b012bcc05a17d0711415770383
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177768"
---
# <a name="deny-search-property-list-permissions-transact-sql"></a>拒绝搜索属性列表权限 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  拒绝搜索属性列表的权限。  
 
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission [ ,...n ] ON  
        SEARCH PROPERTY LIST :: search_property_list_name  
    TO database_principal [ ,...n ] [ CASCADE ]  
    [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission   
 权限的名称。 本主题后面的“备注”部分中介绍了不同权限与安全对象之间的有效映射。  
  
ON SEARCH PROPERTY LIST ::search_property_list_name    
 指定要拒绝权限的搜索属性列表。 需要使用作用域限定符 ::。  
  
database_principal   
 指定要对其拒绝权限的主体。 可以是下列主体之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户。  
  
CASCADE  
 指示要拒绝的权限也会被对此主体授予该权限的其他主体拒绝。  
  
denying_principal   
 指定一个主体，执行该查询的主体从该主体获得拒绝授予该权限的权利。 可以是下列主体之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户。  
  
## <a name="remarks"></a>备注  
  
## <a name="search-property-list-permissions"></a>SEARCH PROPERTY LIST（搜索属性列表权限）  
 搜索属性列表是一个数据库级安全对象，包含在权限层次结构中作为其父级的数据库中。 下表列出了可拒绝的最具体的搜索属性列表限定权限，以及隐含这些权限的更一般的权限。  
  
|搜索属性列表权限|搜索属性列表权限隐含的权限|数据库权限隐含的权限|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对全文目录的 CONTROL 权限。 如果使用 AS 选项，则指定的主体必须拥有全文目录。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE SEARCH PROPERTY LIST (Transact-SQL)](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT 搜索属性列表权限 (Transact-SQL)](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVOKE 搜索属性列表权限 (Transact-SQL)](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.registered_search_property_lists (Transact-SQL)](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [使用搜索属性列表搜索文档属性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
