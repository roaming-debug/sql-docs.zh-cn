---
description: DENY 架构权限 (Transact-SQL)
title: DENY 架构权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- denying permissions [SQL Server], schemas
- schemas [SQL Server], permissions
- permissions [SQL Server], schemas
- DENY statement, schemas
ms.assetid: 300a67c4-d226-4653-9e9f-7ae4d53fcf33
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f27ddae862eb4cfc3b84db6d945818ca706e1034
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177718"
---
# <a name="deny-schema-permissions-transact-sql"></a>DENY 架构权限 (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

拒绝授予架构权限。  
  

![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
DENY permission  [ ,...n ] } ON SCHEMA :: schema_name  
    TO database_principal [ ,...n ]   
    [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
permission  
指定可拒绝授予架构的权限。 有关这些权限的列表，请参阅本文后面的“备注”部分。  
  
ON SCHEMA :: schema_name  
指定权限在其中被拒绝的架构。 需要使用作用域限定符 ::。  
  
database_principal  
指定对其拒绝权限的主体。 *database_principal* 可以为以下主体之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户  
  
CASCADE  
对指定 *database_principal* 授予权限的任何其他主体拒绝授予权限。
  
denying_principal  
指定一个主体，执行该查询的主体从该主体获得拒绝授予该权限的权利。 *denying_principal* 可以为以下主体之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户  
  
## <a name="remarks"></a>注解  
架构是数据库级别的安全对象。 它由权限层次结构中属于其父级的数据库进行约束。 下表列出了可在架构上拒绝的最为具体的限定权限。 该表显示通过暗示包含它们的更一般的权限。  
  
|架构权限|架构权限隐含的权限|数据库权限隐含的权限|  
|-----------------------|----------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY SCHEMA|  
|CONTROL|CONTROL|CONTROL|  
|CREATE SEQUENCE|ALTER|ALTER ANY SCHEMA|  
|DELETE|CONTROL|DELETE|  
|EXECUTE|CONTROL|EXECUTE|  
|INSERT|CONTROL|INSERT|  
|REFERENCES|CONTROL|REFERENCES|  
|SELECT|CONTROL|SELECT|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|UPDATE|CONTROL|UPDATE|  
|VIEW CHANGE TRACKING|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
需要对架构具有 CONTROL 权限。 如果要使用 AS 选项，则指定的主体必须拥有架构。  
  
## <a name="see-also"></a>另请参阅  
[CREATE SCHEMA (Transact-SQL)](../../t-sql/statements/create-schema-transact-sql.md)   
[DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
[权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
[主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
[sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
[sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
[HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  
