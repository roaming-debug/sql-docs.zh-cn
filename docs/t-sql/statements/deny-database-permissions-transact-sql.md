---
description: DENY 数据库权限 (Transact-SQL)
title: DENY 数据库权限 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/21/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, databases
- permissions [SQL Server], databases
- database permissions [SQL Server], denying
- denying permissions [SQL Server], databases
ms.assetid: 36cc4e2c-5a24-4975-9920-9305f12c6e7c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8aa88e0acbdec1b9672b6f26d985f823a2ee08ac
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489508"
---
# <a name="deny-database-permissions-transact-sql"></a>DENY 数据库权限 (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

拒绝对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中数据库的权限。

![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
DENY <permission> [ ,...n ]
    TO <database_principal> [ ,...n ] [ CASCADE ]
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

permission 指定可对数据库拒绝的权限。 有关权限的列表，请参阅本主题后面的“备注”部分。

ALL 该选项不拒绝所有可能权限。 拒绝 ALL 等同于拒绝下列权限：BACKUP DATABASE、BACKUP LOG、CREATE DATABASE、CREATE DEFAULT、CREATE FUNCTION、CREATE PROCEDURE、CREATE RULE、CREATE TABLE 和 CREATE VIEW。

PRIVILEGES 包含此参数是为了符合 ISO 标准。 请不要更改 ALL 的行为。

CASCADE 指示要拒绝的权限也会被对指定主体授予权限的主体拒绝。

AS \<database_principal> 指定一个主体，执行该查询的主体从该主体获得拒绝授予该权限的权利。

Database_user 指定数据库用户。

Database_role 指定数据库角色。

Application_role
适用于：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

指定应用程序角色。

Database_user_mapped_to_Windows_User 指定映射到 Windows 用户的数据库用户。

Database_user_mapped_to_Windows_Group 指定映射到 Windows 组的数据库用户。

Database_user_mapped_to_certificate 指定映射到证书的数据库用户。

Database_user_mapped_to_asymmetric_key 指定映射到非对称密钥的数据库用户。

Database_user_with_no_login 指定无相应服务器级主体的数据库用户。

## <a name="remarks"></a>备注

数据库是安全对象，包含于权限层次结构中作为其父级的服务器中。 下表列出了可拒绝的对数据库最为具体的限定权限，以及隐含这些权限的更为通用的权限。

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
|ALTER ANY DATABASE SCOPED CONFIGURATION<br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDS](../../includes/sssds-md.md)]。|CONTROL|CONTROL SERVER|
|ALTER ANY DATASPACE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL DATA SOURCE|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL FILE FORMAT|ALTER|CONTROL SERVER|
|ALTER ANY EXTERNAL LIBRARY <br /> **适用于**： [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]。|CONTROL|CONTROL SERVER |
|ALTER ANY FULLTEXT CATALOG|ALTER|CONTROL SERVER|
|ALTER ANY MASK|CONTROL|CONTROL SERVER|
|ALTER ANY MESSAGE TYPE|ALTER|CONTROL SERVER|
|ALTER ANY REMOTE SERVICE BINDING|ALTER|CONTROL SERVER|
|ALTER ANY ROLE|ALTER|CONTROL SERVER|
|ALTER ANY ROUTE|ALTER|CONTROL SERVER|
|更改任何安全策略<br /> **适用于**：[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|CONTROL|CONTROL SERVER|
|ALTER ANY SCHEMA|ALTER|CONTROL SERVER|
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
|EXECUTE ANY EXTERNAL SCRIPT <br /> **适用于**： [!INCLUDE[ssSQL15_md](../../includes/sssql15-md.md)]。|CONTROL|CONTROL SERVER|
|INSERT|CONTROL|CONTROL SERVER|
|KILL DATABASE CONNECTION<br /> **适用于**： [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|CONTROL|ALTER ANY CONNECTION|
|REFERENCES|CONTROL|CONTROL SERVER|
|SELECT|CONTROL|CONTROL SERVER|
|SHOWPLAN|CONTROL|ALTER TRACE|
|SUBSCRIBE QUERY NOTIFICATIONS|CONTROL|CONTROL SERVER|
|TAKE OWNERSHIP|CONTROL|CONTROL SERVER|
|UNMASK|CONTROL|CONTROL SERVER|
|UPDATE|CONTROL|CONTROL SERVER|
|VIEW ANY COLUMN ENCRYPTION KEY|CONTROL|VIEW ANY DEFINITION|
|VIEW ANY MASTER KEY DEFINITION|CONTROL|VIEW ANY DEFINITION|
|VIEW DATABASE STATE|CONTROL|VIEW SERVER STATE|
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|

## <a name="permissions"></a>权限

执行此语句的主体（或用 AS 选项指定的主体）必须具有对数据库的 CONTROL 权限，或具有隐含对数据库的 CONTROL 权限的更高权限。

若要使用 AS 选项，则指定的主体必须拥有数据库。

## <a name="examples"></a>示例

### <a name="a-denying-permission-to-create-certificates"></a>A. 拒绝创建证书的权限

以下示例拒绝用户 `CREATE CERTIFICATE` 对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的 `MelanieK` 权限。

```sql
USE AdventureWorks2012;
DENY CREATE CERTIFICATE TO MelanieK;
GO
```

### <a name="b-denying-references-permission-to-an-application-role"></a>B. 对应用程序角色拒绝 REFERENCES 权限

以下示例拒绝应用程序角色 `REFERENCES` 对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的 `AuditMonitor` 权限。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]。

```sql
USE AdventureWorks2012;
DENY REFERENCES TO AuditMonitor;
GO
```

### <a name="c-denying-view-definition-with-cascade"></a>C. 使用 CASCADE 拒绝 VIEW DEFINITION

以下示例拒绝用户 `CarmineEs` 以及 `CarmineEs` 已授予 `VIEW DEFINITION` 权限的所有主体对 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库的 `VIEW DEFINITION` 权限。

```sql
USE AdventureWorks2012;
DENY VIEW DEFINITION TO CarmineEs CASCADE;
GO
```

## <a name="see-also"></a>另请参阅

- [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)
- [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [GRANT](../../t-sql/statements/grant-transact-sql.md)
- [权限](../../relational-databases/security/permissions-database-engine.md)
- [主体](../../relational-databases/security/authentication-access/principals-database-engine.md)
