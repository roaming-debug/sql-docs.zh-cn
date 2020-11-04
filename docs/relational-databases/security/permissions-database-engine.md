---
title: 权限（数据库引擎）| Microsoft Docs
description: 请参阅此 SQL Server 权限完整列表，了解哪些权限适用于你使用的平台。
ms.custom: ''
ms.date: 10/30/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseuser.permissions.database.f1--May use common.permissions
- sql13.swb.databaseuser.permissions.object.f1--May use common.permissions
helpviewer_keywords:
- REFERENCES permission
- permissions [SQL Server]
- security [SQL Server], permissions
- naming conventions [SQL Server]
ms.assetid: f28e3dea-24e6-4a81-877b-02ec4c7e36b9
author: AndreasWolter
ms.author: anwolter
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5da1bad65cf04093be339e1f2e55bddd30efffbf
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243611"
---
# <a name="permissions-database-engine"></a>权限（数据库引擎）
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

每个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全对象都有可以授予主体的关联权限。 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中的权限在分配给登录名和服务器角色的服务器级别，以及分配给数据库用户和数据库角色的数据库级别进行管理。 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 的模型拥有与数据库权限相同的系统，但服务器级别权限不可用。 本主题包含权限的完整列表。 有关典型的权限实现，请参阅 [Getting Started with Database Engine Permissions](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。  
  
[!INCLUDE[ssSQLv15_md](../../includes/sssqlv15-md.md)] 的权限总数是 248。 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 公布有 254 个权限。 大多数权限适用于所有平台，但有些则不适用。 例如，无法对 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 授予服务器级别权限，并且只有几个权限对 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 有意义。
新权限将随着新版本逐步引入。 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 公布有 238 个权限。 [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 公布有 230 个权限。 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 公布有 219 个权限。 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 公布有 214 个权限。 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 公布有 195 个权限。 [sys.fn_builtin_permissions](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md) 主题指定哪些权限时近期版本中的新权限。

了解权限后，通过 [GRANT](../../t-sql/statements/grant-transact-sql.md)、 [REVOKE](../../t-sql/statements/revoke-transact-sql.md)和 [DENY](../../t-sql/statements/deny-transact-sql.md) 语句，将服务器级别权限应用于登录名和数据库级别权限用户。 例如：   
```sql
GRANT SELECT ON OBJECT::HumanResources.Employee TO Larry;
REVOKE SELECT ON OBJECT::HumanResources.Employee TO Larry;
```   
有关计划权限系统的相关提示，请参阅 [数据库引擎权限入门](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。
  
##  <a name="permissions-naming-conventions"></a><a name="_conventions"></a> 权限命名约定  
 下面介绍命名权限时遵循的一般约定：  
  
-   CONTROL  
  
     为被授权者授予类似所有权的功能。 被授权者实际上对安全对象具有所定义的所有权限。 也可以为已被授予 CONTROL 权限的主体授予对安全对象的权限。 因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安全模型是分层的，所以 CONTROL 权限在特定范围内隐含着对该范围内的所有安全对象的 CONTROL 权限。 例如，对数据库的 CONTROL 权限隐含着对数据库的所有权限、对数据库中所有组件的所有权限、对数据库中所有架构的所有权限以及对数据库的所有架构中的所有对象的权限。  
  
-   ALTER  
  
     授予更改特定安全对象的属性（所有权除外）的权限。 当授予对某个范围的 ALTER 权限时，也授予更改、创建或删除该范围内包含的任何安全对象的权限。 例如，对架构的 ALTER 权限包括在该架构中创建、更改和删除对象的权限。  
  
-   ALTER ANY \<*Server Securable*>，其中 Server Securable 可为任何服务器安全对象。  
  
     授予创建、更改或删除“服务器安全对象” 的各个实例的权限。 例如，ALTER ANY LOGIN 将授予创建、更改或删除实例中的任何登录名的权限。  
  
-   ALTER ANY \<*Database Securable*>，其中 Database Securable 可为任何数据库级别的安全对象。  
  
     授予创建、更改或删除“数据库安全对象” 的各个实例的权限。 例如，ALTER ANY SCHEMA 将授予创建、更改或删除数据库中的任何架构的权限。  
  
-   TAKE OWNERSHIP  
  
     允许被授权者获取所授予的安全对象的所有权。  
  
-   IMPERSONATE \<*Login*>  
  
     允许被授权者模拟该登录名。  
  
-   IMPERSONATE \<*User*>  
  
     允许被授权者模拟该用户。  
  
-   CREATE \<*Server Securable*>  
  
     授予被授权者创建“服务器安全对象” 的权限。  
  
-   CREATE \<*Database Securable*>  
  
     授予被授权者创建“数据库安全对象” 的权限。  
  
-   CREATE \<*Schema-contained Securable*>  
  
     授予创建包含在架构中的安全对象的权限。 但是，若要在特定架构中创建安全对象，必须对该架构具有 ALTER 权限。  
  
-   VIEW DEFINITION  
  
     允许被授权者访问元数据。  
  
-   REFERENCES  
  
     表的 REFERENCES 权限是创建引用该表的外键约束时所必需的。  
  
     对象的 REFERENCES 权限是使用引用该对象的 `WITH SCHEMABINDING` 子句创建 FUNCTION 或 VIEW 时所必需的。  
  
## <a name="chart-of-sql-server-permissions"></a>SQL Server 权限图表  
[!INCLUDE[database-engine-permissions](../../includes/paragraph-content/database-engine-permissions.md)]
  
##  <a name="permissions-applicable-to-specific-securables"></a><a name="_securables"></a> 适用于特定安全对象的权限  
 下表列出了主要的权限类别以及可应用这些权限的安全对象的种类。  
  
|权限|适用于|  
|----------------|----------------|  
|ALTER|除 TYPE 外的所有对象类。|  
|CONTROL|所有对象类： <br />AGGREGATE、<br />APPLICATION ROLE、<br />ASSEMBLY、<br />ASYMMETRIC KEY、<br />AVAILABILITY GROUP、<br />CERTIFICATE、<br />CONTRACT、<br />CREDENTIALS、DATABASE、<br />DATABASE SCOPED CREDENTIAL、<br /> DEFAULT、<br />ENDPOINT、<br />FULLTEXT CATALOG、<br />FULLTEXT STOPLIST、<br />FUNCTION、<br />LOGIN、<br />MESSAGE TYPE、<br />PROCEDURE、<br />QUEUE、 <br />REMOTE SERVICE BINDING、<br />ROLE、<br />ROUTE、<br />RULE、<br />SCHEMA、<br />SEARCH PROPERTY LIST、<br />SERVER、<br />SERVER ROLE、<br />SERVICE、<br />SYMMETRIC KEY、<br />SYNONYM、<br />TABLE、<br />TYPE、<br /> USER、<br />VIEW 和<br />XML SCHEMA COLLECTION|  
|DELETE|除 DATABASE SCOPED CONFIGURATION、SERVER 和 TYPE 外的所有对象类。|  
|EXECUTE|CLR 类型、外部脚本、过程（[!INCLUDE[tsql](../../includes/tsql-md.md)] 和 CLR）、标量和聚合函数（[!INCLUDE[tsql](../../includes/tsql-md.md)] 和 CLR）以及同义词|  
|IMPERSONATE|登录名和用户|  
|INSERT|同义词、表和列、视图和列。 可以在数据库、架构或对象级别授予权限。|  
|RECEIVE|[!INCLUDE[ssSB](../../includes/sssb-md.md)] 队列|  
|REFERENCES|AGGREGATE、<br />ASSEMBLY、<br />ASYMMETRIC KEY、<br />CERTIFICATE、<br />CONTRACT、<br />DATABASE、<br />DATABASE SCOPED CREDENTIAL、<br />FULLTEXT CATALOG、<br />FULLTEXT STOPLIST、<br />FUNCTION、<br />MESSAGE TYPE、<br />PROCEDURE、<br />QUEUE、 <br />RULE、<br />SCHEMA、<br />SEARCH PROPERTY LIST、<br />SEQUENCE OBJECT、 <br />SYMMETRIC KEY、<br />TABLE、<br />TYPE、<br />VIEW 和<br />XML SCHEMA COLLECTION|  
|SELECT|同义词、表和列、视图和列。 可以在数据库、架构或对象级别授予权限。|  
|TAKE OWNERSHIP|除 DATABASE SCOPED CONFIGURATION、LOGIN、SERVER 和 USER 外的所有对象类。|  
|UPDATE|同义词、表和列、视图和列。 可以在数据库、架构或对象级别授予权限。|  
|VIEW CHANGE TRACKING|架构和表|  
|VIEW DEFINITION|除 DATABASE SCOPED CONFIGURATION 和 SERVER 外的所有对象类。|  
  
> [!CAUTION]  
>  安装期间授予系统对象的默认权限已针对可能的威胁进行了仔细评估，并且作为强化 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的一部分，无需进行更改。 对系统对象权限的任何更改都可能限制或破坏功能，并且可能让你的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装处于不受支持的状态。  
  
##  <a name="sql-server-permissions"></a><a name="_permissions"></a> SQL Server 权限  
 下表提供了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 权限的完整列表。 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 权限仅适用于受支持的基本安全对象。 不能在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]中授予服务器级别权限，但是在某些情况下，可改为授予数据库权限。  
  
|基础安全对象|对基础安全对象的粒度权限|权限类型代码|包含基础安全对象的安全对象|对容器安全对象的权限隐含着对基础安全对象的粒度权限|  
|--------------------|--------------------------------------------|--------------------------|--------------------------------------------|------------------------------------------------------------------------------------------|  
|APPLICATION ROLE|ALTER|AL|DATABASE|ALTER ANY APPLICATION ROLE|  
|APPLICATION ROLE|CONTROL|CL|DATABASE|CONTROL|  
|APPLICATION ROLE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ASSEMBLY|ALTER|AL|DATABASE|ALTER ANY ASSEMBLY|  
|ASSEMBLY|CONTROL|CL|DATABASE|CONTROL|  
|ASSEMBLY|REFERENCES|RF|DATABASE|REFERENCES|  
|ASSEMBLY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ASSEMBLY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ASYMMETRIC KEY|ALTER|AL|DATABASE|ALTER ANY ASYMMETRIC KEY|  
|ASYMMETRIC KEY|CONTROL|CL|DATABASE|CONTROL|  
|ASYMMETRIC KEY|REFERENCES|RF|DATABASE|REFERENCES|  
|ASYMMETRIC KEY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ASYMMETRIC KEY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|AVAILABILITY GROUP|ALTER|AL|SERVER|ALTER ANY AVAILABILITY GROUP|  
|AVAILABILITY GROUP|CONTROL|CL|SERVER|CONTROL SERVER|  
|AVAILABILITY GROUP|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|AVAILABILITY GROUP|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|CERTIFICATE|ALTER|AL|DATABASE|ALTER ANY CERTIFICATE|  
|CERTIFICATE|CONTROL|CL|DATABASE|CONTROL|  
|CERTIFICATE|REFERENCES|RF|DATABASE|REFERENCES|  
|CERTIFICATE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|CERTIFICATE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|CONTRACT|ALTER|AL|DATABASE|ALTER ANY CONTRACT|  
|CONTRACT|CONTROL|CL|DATABASE|CONTROL|  
|CONTRACT|REFERENCES|RF|DATABASE|REFERENCES|  
|CONTRACT|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|CONTRACT|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|DATABASE|ADMINISTER DATABASE BULK OPERATIONS|DABO|SERVER|CONTROL SERVER|
|DATABASE|ALTER|AL|SERVER|ALTER ANY DATABASE|  
|DATABASE|ALTER ANY APPLICATION ROLE|ALAR|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ASSEMBLY|ALAS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ASYMMETRIC KEY|ALAK|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY CERTIFICATE|ALCF|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY COLUMN ENCRYPTION KEY|ALCK<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY COLUMN MASTER KEY|ALCM<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY CONTRACT|ALSC|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATABASE AUDIT|ALDA|SERVER|ALTER ANY SERVER AUDIT|  
|DATABASE|ALTER ANY DATABASE DDL TRIGGER|ALTG|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATABASE EVENT NOTIFICATION|ALED|SERVER|ALTER ANY EVENT NOTIFICATION|  
|DATABASE|ALTER ANY DATABASE EVENT SESSION|AADS<br /><br /> 适用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|ALTER ANY EVENT SESSION|  
|DATABASE|ALTER ANY DATABASE SCOPED CONFIGURATION|ALDC<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY DATASPACE|ALDS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY EXTERNAL DATA SOURCE|AEDS|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY EXTERNAL FILE FORMAT|AEFF|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY FULLTEXT CATALOG|ALFT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY MASK|AAMK<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY MESSAGE TYPE|ALMT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY REMOTE SERVICE BINDING|ALSB|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ROLE|ALRL|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY ROUTE|ALRT|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SCHEMA|ALSM|SERVER|CONTROL SERVER|  
|DATABASE|更改任何安全策略|ALSP<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|CONTROL SERVER|  
|DATABASE|更改任何敏感度分类|ALSP<br />适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]（SQL Server 2019 (15.x) 至当前版本）、[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|DATABASE|CONTROL SERVER|
|DATABASE|ALTER ANY SERVICE|ALSV|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY SYMMETRIC KEY|ALSK|SERVER|CONTROL SERVER|  
|DATABASE|ALTER ANY USER|ALUS|SERVER|CONTROL SERVER|  
|DATABASE|AUTHENTICATE|AUTH|SERVER|AUTHENTICATE SERVER|  
|DATABASE|BACKUP DATABASE|BADB|SERVER|CONTROL SERVER|  
|DATABASE|BACKUP LOG|BALO|SERVER|CONTROL SERVER|  
|DATABASE|CHECKPOINT|CP|SERVER|CONTROL SERVER|  
|DATABASE|CONNECT|CO|SERVER|CONTROL SERVER|  
|DATABASE|CONNECT REPLICATION|CORP|SERVER|CONTROL SERVER|  
|DATABASE|CONTROL|CL|SERVER|CONTROL SERVER|  
|DATABASE|CREATE AGGREGATE|CRAG|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ASSEMBLY|CRAS|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ASYMMETRIC KEY|CRAK|SERVER|CONTROL SERVER|  
|DATABASE|CREATE CERTIFICATE|CRCF|SERVER|CONTROL SERVER|  
|DATABASE|CREATE CONTRACT|CRSC|SERVER|CONTROL SERVER|  
|DATABASE|CREATE DATABASE|CRDB|SERVER|CREATE ANY DATABASE|  
|DATABASE|CREATE DATABASE DDL EVENT NOTIFICATION|CRED|SERVER|CREATE DDL EVENT NOTIFICATION|  
|DATABASE|CREATE DEFAULT|CRDF|SERVER|CONTROL SERVER|  
|DATABASE|CREATE FULLTEXT CATALOG|CRFT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE FUNCTION|CRFN|SERVER|CONTROL SERVER|  
|DATABASE|CREATE MESSAGE TYPE|CRMT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE PROCEDURE|CRPR|SERVER|CONTROL SERVER|  
|DATABASE|CREATE QUEUE|CRQU|SERVER|CONTROL SERVER|  
|DATABASE|CREATE REMOTE SERVICE BINDING|CRSB|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ROLE|CRRL|SERVER|CONTROL SERVER|  
|DATABASE|CREATE ROUTE|CRRT|SERVER|CONTROL SERVER|  
|DATABASE|CREATE RULE|CRRU|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SCHEMA|CRSM|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SERVICE|CRSV|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SYMMETRIC KEY|CRSK|SERVER|CONTROL SERVER|  
|DATABASE|CREATE SYNONYM|CRSN|SERVER|CONTROL SERVER|  
|DATABASE|CREATE TABLE|CRTB|SERVER|CONTROL SERVER|  
|DATABASE|CREATE TYPE|CRTY|SERVER|CONTROL SERVER|  
|DATABASE|CREATE VIEW|CRVW|SERVER|CONTROL SERVER|  
|DATABASE|CREATE XML SCHEMA COLLECTION|CRXS|SERVER|CONTROL SERVER|  
|DATABASE|DELETE|DL|SERVER|CONTROL SERVER|  
|DATABASE|EXECUTE|EX|SERVER|CONTROL SERVER|  
|DATABASE|EXECUTE ANY EXTERNAL SCRIPT|EAES<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）。|SERVER|CONTROL SERVER|  
|DATABASE|INSERT|IN|SERVER|CONTROL SERVER|  
|DATABASE|KILL DATABASE CONNECTION|KIDC<br /><br /> 仅适用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中使用 ALTER ANY CONNECTION。|SERVER|ALTER ANY CONNECTION|  
|DATABASE|REFERENCES|RF|SERVER|CONTROL SERVER|  
|DATABASE|SELECT|SL|SERVER|CONTROL SERVER|  
|DATABASE|SHOWPLAN|SPLN|SERVER|ALTER TRACE|  
|DATABASE|SUBSCRIBE QUERY NOTIFICATIONS|SUQN|SERVER|CONTROL SERVER|  
|DATABASE|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|DATABASE|UNMASK|UMSK<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|CONTROL SERVER|  
|DATABASE|UPDATE|UP|SERVER|CONTROL SERVER|  
|DATABASE|VIEW ANY COLUMN ENCRYPTION KEY DEFINITION|VWCK<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW ANY COLUMN MASTER KEY DEFINITION|vWCM<br /><br /> 适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] （至[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] 当前版本）和 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]。|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW DATABASE STATE|VWDS|SERVER|VIEW SERVER STATE|  
|DATABASE|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|DATABASE SCOPED CREDENTIAL|ALTER|AL|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|CONTROL|CL|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|REFERENCES|RF|DATABASE|REFERENCES|
|DATABASE SCOPED CREDENTIAL|TAKE OWNERSHIP|TO|DATABASE|CONTROL|
|DATABASE SCOPED CREDENTIAL|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|
|ENDPOINT|ALTER|AL|SERVER|ALTER ANY ENDPOINT|  
|ENDPOINT|CONNECT|CO|SERVER|CONTROL SERVER|  
|ENDPOINT|CONTROL|CL|SERVER|CONTROL SERVER|  
|ENDPOINT|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|ENDPOINT|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|FULLTEXT CATALOG|ALTER|AL|DATABASE|ALTER ANY FULLTEXT CATALOG|  
|FULLTEXT CATALOG|CONTROL|CL|DATABASE|CONTROL|  
|FULLTEXT CATALOG|REFERENCES|RF|DATABASE|REFERENCES|  
|FULLTEXT CATALOG|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|FULLTEXT CATALOG|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|FULLTEXT STOPLIST|ALTER|AL|DATABASE|ALTER ANY FULLTEXT CATALOG|  
|FULLTEXT STOPLIST|CONTROL|CL|DATABASE|CONTROL|  
|FULLTEXT STOPLIST|REFERENCES|RF|DATABASE|REFERENCES|  
|FULLTEXT STOPLIST|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|FULLTEXT STOPLIST|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|LOGIN|ALTER|AL|SERVER|ALTER ANY LOGIN|  
|LOGIN|CONTROL|CL|SERVER|CONTROL SERVER|  
|LOGIN|IMPERSONATE|IM|SERVER|CONTROL SERVER|  
|LOGIN|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|MESSAGE TYPE|ALTER|AL|DATABASE|ALTER ANY MESSAGE TYPE|  
|MESSAGE TYPE|CONTROL|CL|DATABASE|CONTROL|  
|MESSAGE TYPE|REFERENCES|RF|DATABASE|REFERENCES|  
|MESSAGE TYPE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|MESSAGE TYPE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|OBJECT|ALTER|AL|SCHEMA|ALTER|  
|OBJECT|CONTROL|CL|SCHEMA|CONTROL|  
|OBJECT|DELETE|DL|SCHEMA|DELETE|  
|OBJECT|EXECUTE|EX|SCHEMA|EXECUTE|  
|OBJECT|INSERT|IN|SCHEMA|INSERT|  
|OBJECT|RECEIVE|RC|SCHEMA|CONTROL|  
|OBJECT|REFERENCES|RF|SCHEMA|REFERENCES|  
|OBJECT|SELECT|SL|SCHEMA|SELECT|  
|OBJECT|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|OBJECT|UPDATE|UP|SCHEMA|UPDATE|  
|OBJECT|VIEW CHANGE TRACKING|VWCT|SCHEMA|VIEW CHANGE TRACKING|  
|OBJECT|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
|REMOTE SERVICE BINDING|ALTER|AL|DATABASE|ALTER ANY REMOTE SERVICE BINDING|  
|REMOTE SERVICE BINDING|CONTROL|CL|DATABASE|CONTROL|  
|REMOTE SERVICE BINDING|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|REMOTE SERVICE BINDING|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ROLE|ALTER|AL|DATABASE|ALTER ANY ROLE|  
|ROLE|CONTROL|CL|DATABASE|CONTROL|  
|ROLE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ROLE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|ROUTE|ALTER|AL|DATABASE|ALTER ANY ROUTE|  
|ROUTE|CONTROL|CL|DATABASE|CONTROL|  
|ROUTE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|ROUTE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SEARCH PROPERTY LIST|ALTER|AL|SERVER|ALTER ANY FULLTEXT CATALOG|  
|SEARCH PROPERTY LIST|CONTROL|CL|SERVER|CONTROL|  
|SEARCH PROPERTY LIST|REFERENCES|RF|SERVER|REFERENCES|  
|SEARCH PROPERTY LIST|TAKE OWNERSHIP|TO|SERVER|CONTROL|  
|SEARCH PROPERTY LIST|VIEW DEFINITION|VW|SERVER|VIEW DEFINITION|  
|SCHEMA|ALTER|AL|DATABASE|ALTER ANY SCHEMA|  
|SCHEMA|CONTROL|CL|DATABASE|CONTROL|  
|SCHEMA|CREATE SEQUENCE|CRSO|DATABASE|CONTROL|  
|SCHEMA|DELETE|DL|DATABASE|DELETE|  
|SCHEMA|EXECUTE|EX|DATABASE|EXECUTE|  
|SCHEMA|INSERT|IN|DATABASE|INSERT|  
|SCHEMA|REFERENCES|RF|DATABASE|REFERENCES|  
|SCHEMA|SELECT|SL|DATABASE|SELECT|  
|SCHEMA|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SCHEMA|UPDATE|UP|DATABASE|UPDATE|  
|SCHEMA|VIEW CHANGE TRACKING|VWCT|DATABASE|VIEW CHANGE TRACKING|  
|SCHEMA|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SERVER|ADMINISTER BULK OPERATIONS|ADBO|不适用|不适用|  
|SERVER|ALTER ANY AVAILABILITY GROUP|ALAG|不适用|不适用|  
|SERVER|ALTER ANY CONNECTION|ALCO|不适用|不适用|  
|SERVER|ALTER ANY CREDENTIAL|ALCD|不适用|不适用|  
|SERVER|ALTER ANY DATABASE|ALDB|不适用|不适用|  
|SERVER|ALTER ANY ENDPOINT|ALHE|不适用|不适用|  
|SERVER|ALTER ANY EVENT NOTIFICATION|ALES|不适用|不适用|  
|SERVER|ALTER ANY EVENT SESSION|AAES|不适用|不适用|  
|SERVER|ALTER ANY LINKED SERVER|ALLS|不适用|不适用|  
|SERVER|ALTER ANY LOGIN|ALLG|不适用|不适用|  
|SERVER|ALTER ANY SERVER AUDIT|ALAA|不适用|不适用|  
|SERVER|ALTER ANY SERVER ROLE|ALSR|不适用|不适用|  
|SERVER|ALTER RESOURCES|ALRS|不适用|不适用|  
|SERVER|ALTER SERVER STATE|ALSS|不适用|不适用|  
|SERVER|ALTER SETTINGS|ALST|不适用|不适用|  
|SERVER|ALTER TRACE|ALTR|不适用|不适用|  
|SERVER|AUTHENTICATE SERVER|AUTH|不适用|不适用|  
|SERVER|CONNECT ANY DATABASE|CADB|不适用|不适用|  
|SERVER|CONNECT SQL|COSQ|不适用|不适用|  
|SERVER|CONTROL SERVER|CL|不适用|不适用|  
|SERVER|CREATE ANY DATABASE|CRDB|不适用|不适用|  
|SERVER|CREATE AVAILABILITY GROUP|CRAC|不适用|不适用|  
|SERVER|CREATE DDL EVENT NOTIFICATION|CRDE|不适用|不适用|  
|SERVER|CREATE ENDPOINT|CRHE|不适用|不适用|  
|SERVER|CREATE SERVER ROLE|CRSR|不适用|不适用|  
|SERVER|CREATE TRACE EVENT NOTIFICATION|CRTE|不适用|不适用|  
|SERVER|EXTERNAL ACCESS ASSEMBLY|XA|不适用|不适用|  
|SERVER|IMPERSONATE ANY LOGIN|IAL|不适用|不适用|  
|SERVER|SELECT ALL USER SECURABLES|SUS|不适用|不适用|  
|SERVER|SHUTDOWN|SHDN|不适用|不适用|  
|SERVER|UNSAFE ASSEMBLY|XU|不适用|不适用|  
|SERVER|VIEW ANY DATABASE|VWDB|不适用|不适用|  
|SERVER|VIEW ANY DEFINITION|VWAD|不适用|不适用|  
|SERVER|VIEW SERVER STATE|VWSS|不适用|不适用|  
|SERVER ROLE|ALTER|AL|SERVER|ALTER ANY SERVER ROLE|  
|SERVER ROLE|CONTROL|CL|SERVER|CONTROL SERVER|  
|SERVER ROLE|TAKE OWNERSHIP|TO|SERVER|CONTROL SERVER|  
|SERVER ROLE|VIEW DEFINITION|VW|SERVER|VIEW ANY DEFINITION|  
|SERVICE|ALTER|AL|DATABASE|ALTER ANY SERVICE|  
|SERVICE|CONTROL|CL|DATABASE|CONTROL|  
|SERVICE|SEND|SN|DATABASE|CONTROL|  
|SERVICE|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SERVICE|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|SYMMETRIC KEY|ALTER|AL|DATABASE|ALTER ANY SYMMETRIC KEY|  
|SYMMETRIC KEY|CONTROL|CL|DATABASE|CONTROL|  
|SYMMETRIC KEY|REFERENCES|RF|DATABASE|REFERENCES|  
|SYMMETRIC KEY|TAKE OWNERSHIP|TO|DATABASE|CONTROL|  
|SYMMETRIC KEY|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|TYPE|CONTROL|CL|SCHEMA|CONTROL|  
|TYPE|EXECUTE|EX|SCHEMA|EXECUTE|  
|TYPE|REFERENCES|RF|SCHEMA|REFERENCES|  
|TYPE|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|TYPE|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
|USER|ALTER|AL|DATABASE|ALTER ANY USER|  
|USER|CONTROL|CL|DATABASE|CONTROL|  
|USER|IMPERSONATE|IM|DATABASE|CONTROL|  
|USER|VIEW DEFINITION|VW|DATABASE|VIEW DEFINITION|  
|XML SCHEMA COLLECTION|ALTER|AL|SCHEMA|ALTER|  
|XML SCHEMA COLLECTION|CONTROL|CL|SCHEMA|CONTROL|  
|XML SCHEMA COLLECTION|EXECUTE|EX|SCHEMA|EXECUTE|  
|XML SCHEMA COLLECTION|REFERENCES|RF|SCHEMA|REFERENCES|  
|XML SCHEMA COLLECTION|TAKE OWNERSHIP|TO|SCHEMA|CONTROL|  
|XML SCHEMA COLLECTION|VIEW DEFINITION|VW|SCHEMA|VIEW DEFINITION|  
  
##  <a name="summary-of-the-permission-check-algorithm"></a><a name="_algorithm"></a> 权限检查算法摘要  
 检查权限可能很复杂。 权限检查算法包括重叠的组成员关系和所有权链接、显式和隐式权限，并且会受包含安全实体的安全类的权限影响。 该算法的一般过程是收集所有相关权限。 如果未找到阻止性 DENY，该算法将搜索提供足够访问权限的 GRANT。 该算法包含三个基本元素： **安全上下文** 、 **权限空间** 和 **必需的权限** 。  
  
> [!NOTE]  
>  无法对 sa、dbo、实体所有者、information_schema、sys 或您自己授予、拒绝或撤消权限。  
  
-   **安全上下文**  
  
     这是提供进行访问权限检查的权限的一组主体。 这些是与当前登录名或用户有关的权限，除非使用 EXECUTE AS 语句将安全上下文更改为其他登录名或用户。 安全上下文包括以下主体：  
  
    -   登录名  
  
    -   用户  
  
    -   角色成员资格  
  
    -   Windows 组成员身份  
  
    -   如果使用模块签名，则指证书的任何登录名或用户帐户（该证书用于对用户当前正在执行的模块进行签名），以及该主体的相关角色成员资格。  
  
-   **权限空间**  
  
     这是安全实体和所有包含安全实体的安全类。 例如，表（安全实体）包含在架构安全类和数据库安全类中。 访问权限会受表级、架构级、数据库级和服务器级权限影响。 有关详细信息，请参阅[权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)。  
  
-   **必需的权限**  
  
     必需的权限种类。 例如，INSERT、UPDATE、DELETE、SELECT、EXECUTE、ALTER、CONTROL 等等。  
  
     访问可能需要多个权限，如下面的示例中所示：  
  
    -   存储过程可能既需要针对存储过程的 EXECUTE 权限，也需要针对该存储过程引用的每个表的 INSERT 权限。  
  
    -   动态管理视图可能同时需要针对该视图的 VIEW SERVER STATE 和 SELECT 权限。  
  
### <a name="general-steps-of-the-algorithm"></a>算法的通用步骤  
 算法确定是否允许访问某个安全对象时，使用的具体步骤可能会变化，这取决于涉及的主体和安全对象。 但是，算法会执行以下通用步骤：  
  
1.  如果登录名是 sysadmin 固定服务器角色的成员或用户是当前数据库中的 dbo 用户，则绕过权限检查。  
  
2.  如果所有权链接适用且以前针对链中对象的访问权限检查通过了安全检查，则允许访问。  
  
3.  聚合与调用方关联的服务器级、数据库级和已签名模块的标识，以创建 **安全上下文** 。  
  
4.  对于该 **安全上下文** ，收集为 **权限空间** 授予或拒绝的所有权限。 权限可以明确表述为 GRANT、GRANT WITH GRANT 或 DENY，也可以是隐含或涵盖的权限 GRANT 或 DENY。 例如，针对架构的 CONTROL 权限隐含对表的 CONTROL， 对表的 CONTROL 则隐含 SELECT。 因此，如果授予了针对架构的 CONTROL 权限，也就授予了对表的 SELECT 权限。 如果拒绝了对表的 CONTROL 权限，也就拒绝了对表的 SELECT 权限。  
  
    > [!NOTE]  
    >  列级权限的 GRANT 覆盖对象级的 DENY。  
  
5.  标识 **必需的权限** 。  
  
6.  如果对于 **权限空间** 中的对象，直接或隐式拒绝授予 **安全上下文** 中任何标识 **必需的权限** ，则权限检查失败。  
  
7.  如果对于 **权限空间** 中的任何对象，没有拒绝 **必需的权限** 且 **必需的权限** 包含对 **安全上下文** 中任何标识直接或隐式的 GRANT 或 GRANT WITH GRANT 权限，则通过权限检查。  

## <a name="special-considerations-for-column-level-permissions"></a>列级权限的特殊注意事项

使用语法 <table_name>(\<column _name>) 授予列级权限。 例如：
```sql
GRANT SELECT ON OBJECT::Customer(CustomerName) TO UserJoe;
```
表上的 DENY 被列中的 GRANT 替代。 但是，表上的后续 DENY 将删除 GRANT 列。 
  
##  <a name="examples"></a><a name="_examples"></a> 示例  
 本节中的以下示例说明如何检索权限信息。  
  
### <a name="a-returning-the-complete-list-of-grantable-permissions"></a>A. 返回可授予权限的完整列表  
 下列语句使用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 函数返回所有 `fn_builtin_permissions` 权限。 有关详细信息，请参阅 [sys.fn_builtin_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)。  
  
```sql  
SELECT * FROM fn_builtin_permissions(default);  
GO  
```  
  
### <a name="b-returning-the-permissions-on-a-particular-class-of-objects"></a>B. 返回针对某类对象的权限  
 下面的示例使用 `fn_builtin_permissions` 查看可用于安全对象类别的所有权限。 此示例将返回对程序集的权限。  
  
```sql  
SELECT * FROM fn_builtin_permissions('assembly');  
GO    
```  
  
### <a name="c-returning-the-permissions-granted-to-the-executing-principal-on-an-object"></a>C. 返回授予对象的执行主体的权限  
 以下示例使用 `fn_my_permissions` 返回指定安全对象的调用主体所具有的有效权限列表。 此示例将返回对名为 `Orders55` 的对象的权限。 有关详细信息，请参阅 [sys.fn_my_permissions (Transact-SQL)](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)。  
  
```sql  
SELECT * FROM fn_my_permissions('Orders55', 'object');  
GO  
```  
  
### <a name="d-returning-the-permissions-applicable-to-a-specified-object"></a>D. 返回适用于指定对象的权限  
 以下示例将返回适用于名为 `Yttrium`的对象的权限。 请注意，使用了内置函数 `OBJECT_ID` 来检索对象 `Yttrium`的 ID。  
  
```sql  
SELECT * FROM sys.database_permissions   
    WHERE major_id = OBJECT_ID('Yttrium');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.database_permissions (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
