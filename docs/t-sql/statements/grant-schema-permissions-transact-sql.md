---
description: GRANT 架构权限 (Transact-SQL)
title: GRANT 架构权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- schemas [SQL Server], permissions
- permissions [SQL Server], schemas
- GRANT statement, schemas
- granting permissions [SQL Server], schemas
ms.assetid: b2aa1fc8-e7af-45d2-9f80-737543c8aa95
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fbef817bba2cd7b8f682d86300a5348fd0879dfb
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "90570555"
---
# <a name="grant-schema-permissions-transact-sql"></a>GRANT 架构权限 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  授予对架构的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
GRANT permission  [ ,...n ] ON SCHEMA :: schema_name  
    TO database_principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
    [ AS granting_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 permission  
 指定可授予架构的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ON SCHEMA :: schema_name  
 指定将对其授予权限的架构。 需要使用作用域限定符 ::。  
  
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
  
> [!IMPORTANT]  
>  在某些情况下，如果同时拥有 ALTER 权限和 REFERENCE 权限，被授权者将可以查看数据或执行未经授权的函数。 例如：对表拥有 ALTER 权限和对函数拥有 REFERENCE 权限的用户可对函数创建计算列并执行该函数。 在此情况下，用户必须还对计算列具有 SELECT 权限。  
  
 架构是一个数据库级的安全对象，包含于权限层次结构中作为其父级的数据库中。 下面列出了可对架构授予的最特定、最有限的权限，以及暗含这些权限的更一般的权限。  
  
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
  
> [!CAUTION]  
>  对架构拥有 ALTER 权限的用户可以使用所有权链接来访问其他架构中的安全对象，包括显式拒绝用户访问的安全对象。 原因是当主体拥有被引用对象而且主体拥有的对象引用这些被引用对象时，所有权链接对这些被引用对象绕过权限检查。 对架构拥有 ALTER 权限的用户可以创建该架构的所有者拥有的过程、同义词和视图。 这些对象将有权访问（通过所有权链接）该架构所有者拥有的其他架构中的信息。 如果架构所有者还拥有其他架构，则应尽可能避免授予对该架构的 ALTER 权限。  
  
 例如，在以下情框下可能发生此问题。 这些情况假定用户（称为 U1）对 S1 架构拥有 ALTER 权限。 U1 用户被拒绝访问架构 S2 中的表对象（称为 T1）。 S1 架构和 S2 架构由同一所有者拥有。  
  
 U1 用户对数据库拥有 CREATE PROCEDURE 权限，并对 S1 架构拥有 EXECUTE 权限。 因此，U1 用户可以创建一个存储过程，然后在该存储过程中访问被拒绝的对象 T1。  
  
 U1 用户对数据库拥有 CREATE SYNONYM 权限，并对 S1 架构拥有 SELECT 权限。 因此，U1 用户可以在 S1 架构中为被拒绝的对象 T1 创建同义词，然后使用该同义词访问被拒绝的对象 T1。  
  
 U1 用户对数据库拥有 CREATE VIEW 权限，并对 S1 架构拥有 SELECT 权限。 因此，U1 用户可以在 S1 架构中创建视图，以便从被拒绝的对象 T1 中查询数据，然后使用该视图访问被拒绝的对象 T1。
  
## <a name="permissions"></a>权限  
 授权者（或用 AS 选项指定的主体）必须具有带 GRANT OPTION 的相同权限，或具有隐含所授予权限的更高权限。  
  
 如果使用 AS 选项，还必须满足以下附加要求：  
  
|AS granting_principal|所需的其他权限|  
|------------------------------|------------------------------------|  
|数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到 Windows 登录名的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到 Windows 组的数据库用户|Windows 组的成员身份、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到证书的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|映射到非对称密钥的数据库用户|db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|未映射到任何服务器主体的数据库用户|对用户的 IMPERSONATE 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|数据库角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
|应用程序角色|对角色的 ALTER 权限、db_securityadmin 固定数据库角色的成员身份、db_owner 固定数据库角色的成员身份或 sysadmin 固定服务器角色的成员身份。|  
  
 对象所有者可以授予对其所拥有的对象的权限。 对某安全对象具有 CONTROL 权限的主体可以授予对该安全对象的权限。  
  
 被授予 CONTROL SERVER 权限的用户（例如，sysadmin 固定服务器角色的成员）可以授予对相应服务器中任一个安全对象的任意权限。 将某一数据库的 CONTROL 权限授予用户（例如，db_owner 固定数据库角色的成员）后，该用户就可授予对该数据库中任何一个安全对象的任意权限。 被授权 CONTROL 权限的用户可以授予对相应架构中任一个对象的任意权限。  
  
## <a name="examples"></a>示例  
  
### <a name="a-granting-insert-permission-on-schema-humanresources-to-guest"></a>A. 将对架构 HumanResources 的 INSERT 权限授予 Guest  
  
```sql  
GRANT INSERT ON SCHEMA :: HumanResources TO guest;  
```  
  
### <a name="b-granting-select-permission-on-schema-person-to-database-user-wiljo"></a>B. 将对架构 Person 的 SELECT 权限授予数据库用户 WilJo  
  
```sql  
GRANT SELECT ON SCHEMA :: Person TO WilJo WITH GRANT OPTION;  
```  
  
## <a name="see-also"></a>另请参阅  
 [DENY 架构权限 (Transact-SQL)](../../t-sql/statements/deny-schema-permissions-transact-sql.md)   
 [REVOKE 架构权限 (Transact-SQL)](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [CREATE CERTIFICATE (Transact-SQL)](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE APPLICATION ROLE (Transact-SQL)](../../t-sql/statements/create-application-role-transact-sql.md)   
 [加密层次结构](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME (Transact-SQL)](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
  
