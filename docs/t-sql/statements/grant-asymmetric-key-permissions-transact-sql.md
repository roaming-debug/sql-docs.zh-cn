---
description: GRANT 非对称密钥权限 (Transact-SQL)
title: GRANT 非对称密钥权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], asymmetric keys
- permissions [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], permissions
- GRANT statement, asymmetric keys
ms.assetid: a70e2ee6-59b0-4543-b883-e9cbae6199be
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||= azure-sqldw-latest
ms.openlocfilehash: c0c2b21dc6c54ecba5d533aa8dc441758f2857cb
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421999"
---
# <a name="grant-asymmetric-key-permissions-transact-sql"></a>GRANT 非对称密钥权限 (Transact-SQL)
[!INCLUDE [sql-asdb-asa](../../includes/applies-to-version/sql-asdb-asa.md)]

  授予对非对称密钥的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/\synapse-analytics-od-unsupported-syntax.md)]  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GRANT { permission  [ ,...n ] }   
    ON ASYMMETRIC KEY :: asymmetric_key_name   
       TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可对非对称密钥授予的权限。 如下所列。  
  
 ON ASYMMETRIC KEY ::asymmetric_key_name  
 指定对其授予权限的非对称密钥。 需要使用作用域限定符“::”。  
  
 database_principal  
 指定要向其授予权限的主体。 下列类型作之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户。  
  
GRANT OPTION  
 指示该主体还可以向其他主体授予所指定的权限。  
  
AS granting_principal  
 指定一个主体，执行该查询的主体从该主体获得授予该权限的权利。 下列类型作之一：  
  
-   数据库用户  
-   数据库角色  
-   应用程序角色  
-   映射到 Windows 登录名的数据库用户  
-   映射到 Windows 组的数据库用户  
-   映射到证书的数据库用户  
-   映射到非对称密钥的数据库用户  
-   未映射到服务器主体的数据库用户。  
  
## <a name="remarks"></a>备注  
 非对称密钥是一个数据库级的安全对象，包含于权限层次结构中作为其父级的数据库中。 下面列出了可授予非对称密钥的最特定和最受限的权限，以及隐含这些权限的更常用权限。  
  
|非对称密钥权限|非对称密钥权限隐含的权限|数据库权限隐含的权限|  
|-------------------------------|------------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|ALTER ANY ASYMMETRIC KEY|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>权限  
 授权者（或用 AS 选项指定的主体）必须具有带 GRANT OPTION 的相同权限，或具有隐含所授予权限的更高权限。  
  
 如果使用 AS 选项，还必须满足以下附加要求：  
  
|AS granting_principal|所需的其他权限|  
|------------------------------|------------------------------------|  
|数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到 Windows 登录名的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到 Windows 组的数据库用户|Windows 组的成员身份、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到证书的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|映射到非对称密钥的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|未映射到任何服务器主体的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|数据库角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
|应用程序角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份  。|  
  
 对象所有者可以授予对其所拥有的对象的权限。 对某安全对象具有 CONTROL 权限的主体可以授予对该安全对象的权限。  
  
 被授予 CONTROL SERVER 权限的用户（例如，sysadmin 固定服务器角色的成员）可以授予对相应服务器中任一个安全对象的任意权限。 将某一数据库的 CONTROL 权限授予用户（例如，db_owner 固定数据库角色的成员）后，该用户就可授予对该数据库中任何一个安全对象的任意权限。 被授权 CONTROL 权限的用户可以授予对相应架构中任一个对象的任意权限。  
  
## <a name="see-also"></a>另请参阅  
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
