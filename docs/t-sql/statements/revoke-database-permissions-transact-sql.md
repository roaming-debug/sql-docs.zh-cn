---
description: REVOKE 数据库权限 (Transact-SQL)
title: REVOKE 数据库权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], databases
- database permissions [SQL Server], revoking
- REVOKE statement, databases
ms.assetid: 442acfc6-af97-40a3-b546-91cd485ee2be
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 23a419631ac1490d7434aef23ad8ea1c8d92efd2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "100350002"
---
# <a name="revoke-database-permissions-transact-sql"></a>REVOKE 数据库权限 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  撤消对数据库授予和拒绝的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
REVOKE [ GRANT OPTION FOR ] <permission> [ ,...n ]    
    { TO | FROM } <database_principal> [ ,...n ]   
        [ CASCADE ]  
    [ AS <database_principal> ]  
  
<permission> ::=    
permission | ALL [ PRIVILEGES ]  
  
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
 指定可对数据库拒绝的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。  
  
 ALL  
 该选项不会撤消所有可能的权限。 撤消 ALL 等同于撤消下列权限：BACKUP DATABASE、BACKUP LOG、CREATE DATABASE、CREATE DEFAULT、CREATE FUNCTION、CREATE PROCEDURE、CREATE RULE、CREATE TABLE 和 CREATE VIEW。  
  
 PRIVILEGES  
 包含此参数是为了符合 ISO 标准。 请不要更改 ALL 的行为。  
  
 GRANT OPTION  
 指示要撤消向其他主体授予指定权限的权限。 不会撤消该权限本身。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
 CASCADE  
 指示要撤消的权限也会从此主体授予或拒绝该权限的其他主体中撤消。  
  
> [!CAUTION]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 AS \<database_principal>：指定一个主体，执行该查询的主体从中获得撤消权限的权利。  
  
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
 如果您撤消对通过 GRANT OPTION 被授予权限的主体的权限，但是未指定 CASCADE，则语句将失败。  
  
 数据库是安全对象，包含于权限层次结构中作为其父级的服务器中。 下表列出了可撤消的对数据库最为具体的限定权限，以及隐含这些权限的更为通用的权限。  
  
|数据库权限|数据库权限隐含的权限|服务器权限隐含的权限|  
|-------------------------|------------------------------------|----------------------------------|  
|ADMINISTER DATABASE BULK OPERATIONS<br/>适用对象：[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。|CONTROL|CONTROL SERVER|
|ALTER|CONTROL|ALTER ANY DATABASE|  
|ALTER ANY APPLICATION ROLE|ALTER|CONTROL SERVER|  
|ALTER ANY ASSEMBLY|ALTER|CONTROL SERVER|  
|ALTER ANY ASYMMETRIC KEY|ALTER|CONTROL SERVER|  
|ALTER ANY CERTIFICATE|ALTER|CONTROL SERVER|  
|ALTER ANY COLUMN ENCRYPTION KEY|ALTER|CONTROL SERVER|  
|ALTER ANY COLUMN MASTER KEY DEFINITION|ALTER|CONTROL SERVER|  
|ALTER ANY CONTRACT|ALTER|CONTROL SERVER|  
|ALTER ANY DATABASE AUDIT|ALTER|ALTER ANY SERVER AUDIT|  
|ALTER ANY DATABASE DDL TRIGGER|ALTER|CONTROL SERVER|  
|ALTER ANY DATABASE EVENT NOTIFICATION|ALTER|ALTER ANY EVENT NOTIFICATION|  
|ALTER ANY DATABASE EVENT SESSION<br /> **适用于**： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|ALTER|ALTER ANY EVENT SESSION|  
|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **适用于**：[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 及更高版本、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]。|CONTROL|CONTROL SERVER|  
|ALTER ANY DATASPACE|ALTER|CONTROL SERVER|  
|ALTER ANY EXTERNAL DATA SOURCE|ALTER|CONTROL SERVER|  
|ALTER ANY EXTERNAL FILE FORMAT|ALTER|CONTROL SERVER|  
|ALTER ANY EXTERNAL LIBRARY <br /> **适用于**： [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)]。|CONTROL|CONTROL SERVER |    
|ALTER ANY FULLTEXT CATALOG|ALTER|CONTROL SERVER|
|ALTER ANY MASK|CONTROL|CONTROL SERVER|  
|ALTER ANY MESSAGE TYPE|ALTER|CONTROL SERVER|  
|ALTER ANY REMOTE SERVICE BINDING|ALTER|CONTROL SERVER|  
|ALTER ANY ROLE|ALTER|CONTROL SERVER|  
|ALTER ANY ROUTE|ALTER|CONTROL SERVER|  
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|  
|更改任何安全策略<br />**适用于**： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|CONTROL|CONTROL SERVER|  
|ALTER ANY SERVICE|ALTER|CONTROL SERVER|  
|ALTER ANY SYMMETRIC KEY|ALTER|CONTROL SERVER|  
|ALTER ANY USER|ALTER|CONTROL SERVER|  
|AUTHENTICATE|CONTROL|AUTHENTICATE SERVER|  
|BACKUP DATABASE|CONTROL|CONTROL SERVER|  
|BACKUP LOG|CONTROL|CONTROL SERVER|  
|CHECKPOINT|CONTROL|CONTROL SERVER|  
|CONNECT|CONNECT REPLICATION|CONTROL SERVER|  
|CONNECT REPLICATION|CONTROL|CONTROL SERVER|  
|CONTROL|CONTROL|CONTROL SERVER|  
|CREATE AGGREGATE|ALTER|CONTROL SERVER|  
|CREATE ASSEMBLY|ALTER ANY ASSEMBLY|CONTROL SERVER|  
|CREATE ASYMMETRIC KEY|ALTER ANY ASYMMETRIC KEY|CONTROL SERVER|  
|CREATE CERTIFICATE|ALTER ANY CERTIFICATE|CONTROL SERVER|  
|CREATE CONTRACT|ALTER ANY CONTRACT|CONTROL SERVER|  
|CREATE DATABASE|CONTROL|CREATE ANY DATABASE|  
|CREATE DATABASE DDL EVENT NOTIFICATION|ALTER ANY DATABASE EVENT NOTIFICATION|CREATE DDL EVENT NOTIFICATION|  
|CREATE DEFAULT|ALTER|CONTROL SERVER|  
|CREATE FULLTEXT CATALOG|ALTER ANY FULLTEXT CATALOG|CONTROL SERVER|  
|CREATE FUNCTION|ALTER|CONTROL SERVER|  
|CREATE MESSAGE TYPE|ALTER ANY MESSAGE TYPE|CONTROL SERVER|  
|CREATE PROCEDURE|ALTER|CONTROL SERVER|  
|CREATE QUEUE|ALTER|CONTROL SERVER|  
|CREATE REMOTE SERVICE BINDING|ALTER ANY REMOTE SERVICE BINDING|CONTROL SERVER|  
|CREATE ROLE|ALTER ANY ROLE|CONTROL SERVER|  
|CREATE ROUTE|ALTER ANY ROUTE|CONTROL SERVER|  
|CREATE RULE|ALTER|CONTROL SERVER|  
|CREATE SCHEMA|ALTER ANY SCHEMA|CONTROL SERVER|  
|CREATE SERVICE|ALTER ANY SERVICE|CONTROL SERVER|  
|CREATE SYMMETRIC KEY|ALTER ANY SYMMETRIC KEY|CONTROL SERVER|  
|CREATE SYNONYM|ALTER|CONTROL SERVER|  
|CREATE TABLE|ALTER|CONTROL SERVER|  
|CREATE TYPE|ALTER|CONTROL SERVER|  
|CREATE VIEW|ALTER|CONTROL SERVER|  
|CREATE XML SCHEMA COLLECTION|ALTER|CONTROL SERVER|  
|DELETE|CONTROL|CONTROL SERVER|  
|EXECUTE|CONTROL|CONTROL SERVER|  
|EXECUTE ANY EXTERNAL SCRIPT <br /> **适用于**： [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)]。|CONTROL|CONTROL SERVER|   
|INSERT|CONTROL|CONTROL SERVER|  
|KILL DATABASE CONNECTION<br /> **适用于**： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|CONTROL|ALTER ANY CONNECTION|  
|REFERENCES|CONTROL|CONTROL SERVER|  
|SELECT|CONTROL|CONTROL SERVER|  
|SHOWPLAN|CONTROL|ALTER TRACE|  
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|  
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|  
|UNMASK|CONTROL|CONTROL SERVER|  
|UPDATE|CONTROL|CONTROL SERVER|  
|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|VIEW ANY COLUMN MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
  
## <a name="permissions"></a>权限  
 执行此语句的主体（或用 AS 选项指定的主体）必须具有对数据库的 CONTROL 权限，或具有隐含对数据库的 CONTROL 权限的更高权限。  
  
 若要使用 AS 选项，则指定的主体必须拥有数据库。  
  
## <a name="examples"></a>示例  
  
### <a name="a-revoking-permission-to-create-certificates"></a>A. 撤消创建证书的权限  
 以下示例从用户 `CREATE CERTIFICATE` 中撤消对 `AdventureWorks2012` 数据库的 `MelanieK` 权限。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
```sql  
USE AdventureWorks2012;  
REVOKE CREATE CERTIFICATE FROM MelanieK;  
GO  
```  
  
### <a name="b-revoking-references-permission-from-an-application-role"></a>B. 从应用程序角色中撤消 REFERENCES 权限  
 以下示例从应用程序角色 `REFERENCES` 中撤消对 `AdventureWorks2012` 数据库的 `AuditMonitor` 权限。  
  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
```sql  
USE AdventureWorks2012;  
REVOKE REFERENCES FROM AuditMonitor;  
GO  
```  
  
### <a name="c-revoking-view-definition-with-cascade"></a>C. 使用 CASCADE 撤消 VIEW DEFINITION  
 以下示例从用户 `VIEW DEFINITION` 以及 `AdventureWorks2012` 已授予 `CarmineEs` 权限的所有主体中撤消对 `CarmineEs` 数据库的 `VIEW DEFINITION` 权限。  
  
```sql  
USE AdventureWorks2012;  
REVOKE VIEW DEFINITION FROM CarmineEs CASCADE;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)   
 [DENY 数据库权限 (Transact-SQL)](../../t-sql/statements/deny-database-permissions-transact-sql.md)   
 [权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)   
 [主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  

