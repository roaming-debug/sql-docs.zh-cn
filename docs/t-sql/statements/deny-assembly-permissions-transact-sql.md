---
description: DENY 程序集权限 (Transact-SQL)
title: DENY 程序集权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- assemblies [CLR integration], permissions
- DENY statement, assemblies
- denying permissions [SQL Server], assemblies
ms.assetid: be0896b3-f793-44c9-9390-1c6e58f33b2b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 51a45ac7c65df9dd6ed28b8d76aaf2d59a627e53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177806"
---
# <a name="deny-assembly-permissions-transact-sql"></a>DENY 程序集权限 (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  拒绝对程序集的权限。  
  

  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
  
DENY { permission [ ,...n ] } ON ASSEMBLY :: assembly_name  
    TO database_principal [ ,...n ]   
        [ CASCADE ]  
        [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可拒绝授予程序集的权限。 如下所列。  
  
 ON ASSEMBLY ::assembly_name  
 指定拒绝将其权限授予他人的程序集。 需要使用作用域限定符“::”。  
  
 database_principal  
 指定要对其拒绝权限的主体。 下列类型作之一：  
  
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
 指定一个主体，执行该查询的主体从该主体获得拒绝授予该权限的权利。 下列类型作之一：  
  
-   数据库用户  
  
-   数据库角色  
  
-   应用程序角色  
  
-   映射到 Windows 登录名的数据库用户  
  
-   映射到 Windows 组的数据库用户  
  
-   映射到证书的数据库用户  
  
-   映射到非对称密钥的数据库用户  
  
-   未映射到服务器主体的数据库用户。  
  
## <a name="remarks"></a>注解  
 程序集是一个数据库级的安全对象，包含于权限层次结构中作为其父级的数据库中。 下面列出了可拒绝授予程序集的最特定和最受限的权限，以及隐含这些权限的更常用权限。  
  
|程序集权限|程序集权限隐含的权限|数据库权限隐含的权限|  
|-------------------------|------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASSEMBLY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 需要对程序集具有 CONTROL 权限。 如果使用 AS 选项，则指定的主体必须拥有程序集。  
  
## <a name="see-also"></a>另请参阅  
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASSEMBLY (Transact-SQL)](../../t-sql/statements/create-assembly-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
