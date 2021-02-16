---
description: ALTER AUTHORIZATION (Transact-SQL)
title: ALTER AUTHORIZATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/01/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_AUTHORIZATION_TSQL
- ALTER AUTHORIZATION
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], transferring
- modifying entity ownership
- full-text search [SQL Server], permissions
- owners [SQL Server], entities
- ALTER AUTHORIZATION statement
- entity ownership [SQL Server]
- transferring ownership
- search property lists [SQL Server], permissions
- TAKE OWNERSHIP
ms.assetid: 8c805ae2-91ed-4133-96f6-9835c908f373
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 95a1787412fa84c30ac0b14d670f94c72af10d30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100270500"
---
# <a name="alter-authorization-transact-sql"></a>ALTER AUTHORIZATION (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  更改安全对象的所有权。

 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>语法

```syntaxsql
-- Syntax for SQL Server
ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
      OBJECT | ASSEMBLY | ASYMMETRIC KEY | AVAILABILITY GROUP | CERTIFICATE
    | CONTRACT | TYPE | DATABASE | ENDPOINT | FULLTEXT CATALOG
    | FULLTEXT STOPLIST | MESSAGE TYPE | REMOTE SERVICE BINDING
    | ROLE | ROUTE | SCHEMA | SEARCH PROPERTY LIST | SERVER ROLE
    | SERVICE | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for SQL Database

ALTER AUTHORIZATION
    ON [ <class_type>:: ] entity_name
    TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::=
     {
    OBJECT | ASSEMBLY | ASYMMETRIC KEY | CERTIFICATE
     | TYPE | DATABASE | FULLTEXT CATALOG
     | FULLTEXT STOPLIST
     | ROLE | SCHEMA | SEARCH PROPERTY LIST
     | SYMMETRIC KEY | XML SCHEMA COLLECTION
     }
```

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

    <class_type> ::= {
    SCHEMA
     | OBJECT
    }

    <entity_name> ::=
    {
    schema_name
     | [ schema_name. ] object_name
    }
```

```syntaxsql
-- Syntax for Parallel Data Warehouse

ALTER AUTHORIZATION ON
     [ <class_type> :: ] <entity_name>
     TO { principal_name | SCHEMA OWNER }
    [;]

<class_type> ::= {
    DATABASE
     | SCHEMA
     | OBJECT
    }

<entity_name> ::=
    {
    database_name
     | schema_name
     | [ schema_name. ] object_name
    }
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数

\<class_type>：更改其所有者的实体的安全对象类。 OBJECT 是默认值。

|类|Products|
|-|-|
|OBJECT|**适用范围**[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]、[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]、[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]。|
|ASSEMBLY|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|ASYMMETRIC KEY|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|AVAILABILITY GROUP |适用范围：SQL Server 2012 和更高版本。|
|CERTIFICATE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|CONTRACT|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。|
|DATABASE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。 有关详细信息，请参阅[用于数据库的 ALTER AUTHORIZATION](#alter-authorization-for-databases)。|
|ENDPOINT|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。|
|FULLTEXT CATALOG|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|FULLTEXT STOPLIST|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|MESSAGE TYPE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。|
|REMOTE SERVICE BINDING|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。|
|ROLE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|ROUTE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。|
|SCHEMA|**适用范围**[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]、[!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]、[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]。|
|SEARCH PROPERTY LIST|**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|SERVER ROLE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。|
|SERVICE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。|
|SYMMETRIC KEY|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|TYPE|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|
|XML SCHEMA COLLECTION|**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|

entity_name 是实体名称。

principal_name | SCHEMA OWNER：将拥有实体的安全主体的名称。 数据库对象必须为数据库主体、数据库用户或角色所拥有。 服务器对象（如数据库）必须为服务器主体（登录名）所拥有。 指定 SCHEMA OWNER 作为 *principal_name，以指明对象应该由拥有对象架构的主体拥有。

## <a name="remarks"></a>备注

ALTER AUTHORIZATION 可用于更改任何具有所有者的实体的所有权。 数据库包含的实体的所有权，可以转移给任何数据库级的主体。 服务器级实体的所有权只能转移给服务器级主体。

> [!IMPORTANT]
> 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，用户可以拥有由另一个数据库用户拥有的架构所包含的 OBJECT 或 TYPE。 这是对早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的行为的更改。 有关详细信息，请参阅 [OBJECTPROPERTY (Transact-SQL)](../../t-sql/functions/objectproperty-transact-sql.md) 和 [TYPEPROPERTY (Transact-SQL)](../../t-sql/functions/typeproperty-transact-sql.md)。

以下包含在架构中、类型为“object”的实体的所有权可以转移：表、视图、函数、过程、队列和同义词。

不能传输以下实体的所有权：链接服务器、统计信息、约束、规则、默认值、触发器、[!INCLUDE[ssSB](../../includes/sssb-md.md)] 队列、凭据、分区函数、分区方案、数据库主密钥、服务主密钥和事件通知。

以下安全对象类的成员所有权不能进行转移：服务器、登录、用户、应用程序角色和列。

仅当转移架构包含的实体的所有权时，SCHEMA OWNER 选项才有效。 SCHEMA OWNER 将实体所有权转移给它所在的架构所有者。 只有类 OBJECT、TYPE 或 XML SCHEMA COLLECTION 的实体是架构包含的。

如果目标实体不是数据库，且该实体正被转移给新的所有者，则该目标的所有权限将被删除。

> [!CAUTION]
> 在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中，架构的行为与早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的行为不同。 假设架构与数据库用户等效的代码可能不会返回正确的结果。 旧目录视图（包括 sysobjects）不应用于曾使用下列任何 DDL 语句的数据库中：CREATE SCHEMA、ALTER SCHEMA、DROP SCHEMA、CREATE USER、ALTER USER、DROP USER、CREATE ROLE、ALTER ROLE、DROP ROLE、CREATE APPROLE、ALTER APPROLE、DROP APPROLE、ALTER AUTHORIZATION。 在曾经使用过这些语句中的任意一个语句的数据库中，必须使用新的目录视图。 新目录视图将采用在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的使主体和架构分离的方法。 有关目录视图的详细信息，请参阅[目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)。

 另请注意下列事项：

> [!IMPORTANT]
> 查找对象所有者的唯一可靠的方式是查询 **sys.objects** 目录视图。 查找类型所有者的唯一可靠的方式是使用 TYPEPROPERTY 函数。

## <a name="special-cases-and-conditions"></a>特殊事例和条件

下表列出了适用于更改授权的特殊事例、异常和条件。

|类|条件|
|-----------|---------------|
|OBJECT|无法更改触发器、约束、规则、默认值、统计信息、系统对象、队列、索引视图或具有索引视图的表的所有权。|
|SCHEMA|转移所有权时，将删除没有显式所有者的架构包含对象的权限。 无法更改 sys、dbo 或 information_schema 的所有者。|
|TYPE|无法更改属于 sys 或 information_schema 的 TYPE 的所有权。|
|CONTRACT、MESSAGE TYPE 或 SERVICE|无法更改系统实体的所有权。|
|SYMMETRIC KEY|无法更改全局临时密钥的所有权。|
|CERTIFICATE 或 ASYMMETRIC KEY|无法将这些实体的所有权转移给角色或组。|
|ENDPOINT|主体必须为登录名。|

## <a name="alter-authorization-for-databases"></a>对数据库执行 ALTER AUTHORIZATION

### <a name="for-sql-server"></a>对于 SQL Server

对新所有者的要求：新所有者主体必须是以下项之一：

- SQL Server 身份验证登录名。
- 表示 Windows 用户（而不是组）的 Windows 身份验证登录名。
- 表示 Windows 组的 Windows 用户，通过 Windows 身份验证登录名进行身份验证。

**对执行 ALTER AUTHORIZATION 语句的人员的要求：** 如果不是 **sysadmin** 固定服务器角色的成员，则必须至少对数据库具有 TAKE OWNERSHIP 权限和对新所有者用户名具有 IMPERSONATE 权限。

### <a name="for-azure-sql-database"></a>对于 Azure SQL 数据库

对新所有者的要求：新所有者主体必须是以下项之一：

- SQL Server 身份验证登录名。
- 存在于 Azure AD 中的联合用户（而不是组）。
- 存在于 Azure AD 中的托管用户（而不是组）或应用程序。

如果新所有者是 Azure Active Directory 用户，则在新所有者将成为新 DBO 的数据库中，该新所有者不能在其中作为用户而存在。 执行用于将数据库所有权更改到新用户的 ALTER AUTHORIZATION 语句前，必须首先从数据库中删除此类 Azure AD 用户。 有关使用 SQL 数据库配置 Azure Active Directory 用户的详细信息，请参阅[使用 Azure Active Directory 身份验证连接到 SQL 数据库或 [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/)。

**对执行 ALTER AUTHORIZATION 语句的人员的要求：** 必须连接到目标数据库才能更改数据库的所有者。

以下类型的帐户可以更改数据库的所有者。

- 服务级别主体登录名。 （创建 SQL 数据库服务器时预配的 SQL Azure 管理员。）
- Azure SQL Server 的 Azure Active Directory 管理员。
- 数据库的当前所有者。

下表概述了这些要求：

执行者  |目标  |结果
---------|---------|---------
SQL Server 身份验证登录名  |SQL Server 身份验证登录名|Success
SQL Server 身份验证登录名  |Azure AD 用户|失败
Azure AD 用户  |SQL Server 身份验证登录名|Success
Azure AD 用户  |Azure AD 用户|Success

若要验证数据库的 Azure AD 所有者，请在用户数据库中执行以下 Transact-SQL 命令（在此示例中为 `testdb`）。

```sql
SELECT CAST(owner_sid as uniqueidentifier) AS Owner_SID
FROM sys.databases
WHERE name = 'testdb';
```

输出将是一个标识符（如 6D8B81F6-7C79-444C-8858-4AF896C03C67），它对应于分配给 `richel@cqclinic.onmicrosoft.com` 的 Azure AD ObjectID。如果 SQL Server 身份验证登录用户是数据库所有者，请在 master 数据库中执行以下语句来验证数据库所有者：

```sql
SELECT d.name, d.owner_sid, sl.name
FROM sys.databases AS d
JOIN sys.sql_logins AS sl
ON d.owner_sid = sl.sid;

```

### <a name="best-practice"></a>最佳做法

将 Azure AD 组用作 db_owner 固定数据库角色的成员，而不是将 Azure AD 用户用作数据库的单个所有者。 下面的步骤演示如何将禁用登录名配置为数据库所有者，并将 Azure Active Directory 组 (`mydbogroup`) 设为 db_owner 角色的成员。

1. 以 Azure AD 管理员身份登录 SQL Server，将数据库的所有者更改为禁用的 SQL Server 身份验证登录名。 例如，在用户数据库中执行：

   ```sql
   ALTER AUTHORIZATION ON database::testdb TO DisabledLogin;
   ```

1. 创建应拥有数据库的 Azure AD 组，将其作为用户添加到用户数据库。 例如：

   ```sql
   CREATE USER [mydbogroup] FROM EXTERNAL PROVIDER;
   ```

1. 在用户数据库中，将表示 Azure AD 组的用户添加到 db_owner 固定数据库角色。 例如：

   ```sql
   ALTER ROLE db_owner ADD MEMBER mydbogroup;
   ```

现在，`mydbogroup` 成员可将数据库作为 db_owner 角色的成员进行集中管理。

- 从 Azure AD 组中移除此组的成员时，他们会自动失去此数据库的 dbo 权限。
- 同样，如果向 `mydbogroup` Azure AD 组中添加新成员，他们将自动获得此数据库的 dbo 权限。

若要检查特定用户是否具有有效的 dbo 权限，请让该用户执行以下语句：

```sql
SELECT IS_MEMBER ('db_owner');
```

返回值 1 表示该用户是角色的成员。

## <a name="permissions"></a>权限

要求具有实体的 TAKE OWNERSHIP 权限。 如果新所有者不是执行该语句的用户，那么：1) 如果新所有者是用户或登录名，则要求具有该所有者的 IMPERSONATE 权限；2) 如果新所有者是角色，则要求具有该角色的成员身份或该角色的 ALTER 权限；3) 如果新所有者是应用程序角色，则要求具有该应用程序角色的 ALTER 权限。

## <a name="examples"></a>示例

### <a name="a-transfer-ownership-of-a-table"></a>A. 转移表的所有权

以下示例将 `Sprockets` 表的所有权转移给 `MichikoOsada` 用户。 该表位于 `Parts` 架构内。

```sql
ALTER AUTHORIZATION ON OBJECT::Parts.Sprockets TO MichikoOsada;
GO
```

该查询可能如下所示：

```sql
ALTER AUTHORIZATION ON Parts.Sprockets TO MichikoOsada;
GO
```

如果语句中不包含对象架构，[!INCLUDE[ssDE](../../includes/ssde-md.md)] 将在用户默认架构中查找对象。 例如：

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
```

### <a name="b-transfer-ownership-of-a-view-to-the-schema-owner"></a>B. 将视图的所有权转移给架构所有者

以下示例将 `ProductionView06` 视图的所有权转移给包含它的架构的所有者。 该视图位于 `Production` 架构内。

```sql
ALTER AUTHORIZATION ON OBJECT::Production.ProductionView06 TO SCHEMA OWNER;
GO
```

### <a name="c-transfer-ownership-of-a-schema-to-a-user"></a>C. 将架构所有权转移给用户

以下示例将 `SeattleProduction11` 架构的所有权转移给 `SandraAlayo` 用户。

```sql
ALTER AUTHORIZATION ON SCHEMA::SeattleProduction11 TO SandraAlayo;
GO
```

### <a name="d-transfer-ownership-of-an-endpoint-to-a-sql-server-login"></a>D. 将端点的所有权转移给 SQL Server 登录名

以下示例将 `CantabSalesServer1` 端点的所有权转移给 `JaePak`。 由于该端点是服务器级安全对象，因此只能将它转移给服务器级别主体。

**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本。

```sql
ALTER AUTHORIZATION ON ENDPOINT::CantabSalesServer1 TO JaePak;
GO
```

### <a name="e-changing-the-owner-of-a-table"></a>E. 更改表所有者

下面的每个示例都将 `Parts` 数据库中 `Sprockets` 表的所有者更改为数据库用户 `MichikoOsada`。

```sql
ALTER AUTHORIZATION ON Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON dbo.Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::Sprockets TO MichikoOsada;
ALTER AUTHORIZATION ON OBJECT::dbo.Sprockets TO MichikoOsada;
```

### <a name="f-changing-the-owner-of-a-database"></a>F. 更改数据库所有者

 **适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]及更高版本、[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]、[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]。

 以下示例将 `Parts` 数据库的所有者更改为登录名 `MichikoOsada`。

```sql
ALTER AUTHORIZATION ON DATABASE::Parts TO MichikoOsada;
```

### <a name="g-changing-the-owner-of-a-sql-database-to-an-azure-ad-user"></a>G. 将 SQL 数据库的所有者更改为 Azure AD 用户

在下面的示例中，组织（拥有名为 `cqclinic.onmicrosoft.com` 的 Active Directory ）中的 SQL Server 的 Azure Active Directory 管理员可以更改数据库 `targetDB` 的当前所有权，还将使用以下命令将 AAD 用户 `richel@cqclinic.onmicorsoft.com` 设为新的数据库所有者：

```sql
ALTER AUTHORIZATION ON database::targetDB TO [rachel@cqclinic.onmicrosoft.com];
```

Azure AD 要求用括号 `[]` 括住用户名。

## <a name="see-also"></a>另请参阅
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md) [TYPEPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/typeproperty-transact-sql.md) [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)

