---
description: CREATE SCHEMA (Transact-SQL)
title: CREATE SCHEMA (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SCHEMA_TSQL
- SCHEMA
- CREATE SCHEMA
- SCHEMA_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- owners [SQL Server], schemas
- table creation [SQL Server], CREATE SCHEMA statement
- permissions [SQL Server], schemas
- schemas [SQL Server], creating
- CREATE SCHEMA statement
ms.assetid: df74fc36-20da-4efa-b412-c4e191786695
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8665ec086aeb1a0072f9dcc10405a524b7f7dc1b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104734324"
---
# <a name="create-schema-transact-sql"></a>CREATE SCHEMA (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在当前数据库中创建架构。 CREATE SCHEMA 事务还可以在新架构内创建表和视图，并可对这些对象设置 GRANT、DENY 或 REVOKE 权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE SCHEMA schema_name_clause [ <schema_element> [ ...n ] ]  
  
<schema_name_clause> ::=  
    {  
    schema_name  
    | AUTHORIZATION owner_name  
    | schema_name AUTHORIZATION owner_name  
    }  
  
<schema_element> ::=   
    {   
        table_definition | view_definition | grant_statement |   
        revoke_statement | deny_statement   
    }  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
CREATE SCHEMA schema_name [ AUTHORIZATION owner_name ] [;]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 *schema_name*  
 在数据库内标识架构的名称。  
  
 AUTHORIZATION owner_name  
 指定将拥有架构的数据库级主体的名称。 此主体还可以拥有其他架构，并且可以不使用当前架构作为其默认架构。  
  
 table_definition  
 指定在架构内创建表的 CREATE TABLE 语句。 执行此语句的主体必须对当前数据库具有 CREATE TABLE 权限。  
  
 view_definition  
 指定在架构内创建视图的 CREATE VIEW 语句。 执行此语句的主体必须对当前数据库具有 CREATE VIEW 权限。  
  
 grant_statement  
 指定可对除新架构外的任何安全对象授予权限的 GRANT 语句。  
  
 revoke_statement  
 指定可对除新架构外的任何安全对象撤消权限的 REVOKE 语句。  
  
 deny_statement  
 指定可对除新架构外的任何安全对象拒绝授予权限的 DENY 语句。  
  
## <a name="remarks"></a>备注  
  
> [!NOTE]  
>  包含 CREATE SCHEMA AUTHORIZATION 但未指定名称的语句仅允许用于向后兼容性。 该语句未引起错误，但未创建一个架构。  
  
 CREATE SCHEMA 可以在单条语句中创建架构以及该架构所包含的表和视图，并授予对任何安全对象的 GRANT、REVOKE 或 DENY 权限。 此语句必须作为一个单独的批处理执行。 CREATE SCHEMA 语句所创建的对象将在要创建的架构内进行创建。  
  
 CREATE SCHEMA 事务是原子级的。 如果 CREATE SCHEMA 语句执行期间出现任何错误，则不会创建任何指定的安全对象，也不会授予任何权限。  
  
 由 CREATE SCHEMA 创建的安全对象可以任何顺序列出，但引用其他视图的视图除外。 在这种情况下，被引用的视图必须在引用它的视图之前创建。  
  
 因此，GRANT 语句可以在创建某个对象自身之前对该对象授予权限，CREATE VIEW 语句也可以出现在创建该视图所引用表的 CREATE TABLE 语句之前。 同样，CREATE TABLE 语句可以在 CREATE SCHEMA 语句定义表之前声明表的外键。  
  
> [!NOTE]  
>  支持在 CREATE SCHEMA 语句中使用 DENY 和 REVOKE。 DENY 和 REVOKE 子句将按照它们在 CREATE SCHEMA 语句中出现的顺序执行。  
  
 执行 CREATE SCHEMA 的主体可以将另一个数据库主体指定为要创建的架构的所有者。 完成此操作需要另外的权限，如本主题下文中的“权限”部分所述。  
  
 新架构由以下数据库级别主体之一拥有：数据库用户、数据库角色或应用程序角色。 在架构内创建的对象由架构所有者拥有，这些对象在 **sys.objects** 中的 **principal_id** 为 NULL。 架构所包含对象的所有权可转让给任何数据库级主体，但架构所有者始终保留对该架构内对象的 CONTROL 权限。  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
 **隐式架构和用户创建**  
  
 在某些情况下，用户可在没有数据库用户帐户（数据库中的数据库主体）的情况下使用数据库。 在以下情况下，可能会发生此错误：  
  
-   登录名具有 CONTROL SERVER 特权。  
  
-   Windows 用户没有单独的数据库用户帐户（数据库中的数据库主体），但以具有数据库用户帐户（Windows 组的数据库主体）的 Windows 组成员的身份访问数据库。  
  
 如果没有数据库用户帐户的用户在不指定现有架构的情况下创建对象，则将在数据库中自动为该用户创建数据库主体和默认架构。 创建的数据库主体和架构采用的名称将与连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时用户使用的名称（[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名或 Windows 用户名）相同。  
  
 若要允许基于 Windows 组的用户创建和拥有对象，此行为很有必要。 但这种行为可能将导致意外创建架构和用户。 为了避免隐式创建用户和架构，请尽可能显式创建数据库主体和分配默认架构。 或者，在数据库中创建对象时，使用由两部分或三部分组成的对象名称显式声明现有架构。  

> [!NOTE]
>  不能在 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] 上隐式创建 Azure Active Directory 用户。 从外部提供程序创建 Azure AD 用户必须检查 AAD 中的用户状态，因此创建用户将会失败，并出现错误 2760：指定的架构名称“\<user_name@domain>”不存在或你不具有使用它权限。 然后出现错误 2759：由于前面的错误，CREATE SCHEMA 失败。 要解决这些错误，请首先从外部提供程序创建 Azure AD 用户，然后重新运行创建该对象的语句。
 
  
## <a name="deprecation-notice"></a>不推荐使用的声明  
 当前支持不指定架构名称的 CREATE SCHEMA 语句，目的是为了向后兼容。 此类语句并不在数据库中实际创建架构，但它们会创建表和视图，并授予权限。 主体不需要 CREATE SCHEMA 权限来执行这一早期形式的 CREATE SCHEMA，因为不会创建任何架构。 此功能将从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未来版本中删除。  
  
## <a name="permissions"></a>权限  
 需要对数据库拥有 CREATE SCHEMA 权限。  
  
 若要创建在 CREATE SCHEMA 语句中指定的对象，用户必须拥有相应的 CREATE 权限。  
  
 若要指定其他用户作为所创建架构的所有者，则调用方必须具有对该用户的 IMPERSONATE 权限。 如果指定一个数据库角色作为所有者，则调用方必须拥有该角色的成员身份或对该角色拥有 ALTER 权限。  
  
> [!NOTE]  
>  对于向后兼容的语法，将不针对 CREATE SCHEMA 检查任何权限，因为不创建任何架构。  
  
## <a name="examples"></a>示例  
  
### <a name="a-creating-a-schema-and-granting-permissions"></a>A. 创建架构和授予权限  
 下面的示例将创建由 `Sprockets` 拥有的、包含表 `Annik` 的 `NineProngs` 架构。 该语句向 `SELECT` 授予 `Mandar` 权限，而对 `SELECT` 拒绝授予 `Prasanna` 权限。 请注意，`Sprockets` 和 `NineProngs` 在一个语句中创建。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE SCHEMA Sprockets AUTHORIZATION Annik  
    CREATE TABLE NineProngs (source int, cost int, partnumber int)  
    GRANT SELECT ON SCHEMA::Sprockets TO Mandar  
    DENY SELECT ON SCHEMA::Sprockets TO Prasanna;  
GO   
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-creating-a-schema-and-a-table-in-the-schema"></a>B. 创建架构并在架构中创建表  
 以下示例将创建架构 `Sales`，然后在该架构中创建表 `Sales.Region`。  
  
```sql  
CREATE SCHEMA Sales;  
GO
  
CREATE TABLE Sales.Region   
(Region_id INT NOT NULL,  
Region_Name CHAR(5) NOT NULL)  
WITH (DISTRIBUTION = REPLICATE);  
GO  
```  
  
### <a name="c-setting-the-owner-of-a-schema"></a>C. 设置架构的所有者  
 下面的示例将创建由 `Mary` 拥有的 `Production` 架构。  
  
```sql  
CREATE SCHEMA Production AUTHORIZATION [Contoso\Mary];  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ALTER SCHEMA (Transact-SQL)](../../t-sql/statements/alter-schema-transact-sql.md)   
 [DROP SCHEMA (Transact-SQL)](../../t-sql/statements/drop-schema-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)   
 [CREATE VIEW (Transact-SQL)](../../t-sql/statements/create-view-transact-sql.md)   
 [EVENTDATA (Transact-SQL)](../../t-sql/functions/eventdata-transact-sql.md)   
 [sys.schemas (Transact-SQL)](../../relational-databases/system-catalog-views/schemas-catalog-views-sys-schemas.md)   
 [创建数据库架构](../../relational-databases/security/authentication-access/create-a-database-schema.md)  
  
  


