---
description: REVOKE (Transact-SQL)
title: REVOKE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- REVOKE_TSQL
- REVOKE
dev_langs:
- TSQL
helpviewer_keywords:
- schema-level securables [SQL Server]
- REVOKE statement, Transact-SQL syntax
- removing permissions
- server-level securables [SQL Server]
- deleting permissions
- revoking permissions [SQL Server]
- REVOKE statement
- denying permissions [SQL Server], removing
- database-level securables [SQL Server]
- granting permissions [SQL Server], removing
- permissions [SQL Server], revoking
- dropping permissions
ms.assetid: 9d31d3e7-0883-45cd-bf0e-f0361bbb0956
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5a1b425a293dd811ec5202a94612a3dcfc815ef2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91227464"
---
# <a name="revoke-transact-sql"></a>REVOKE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  撤消以前授予或拒绝的权限。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for REVOKE  
REVOKE [ GRANT OPTION FOR ]  
      {   
        [ ALL [ PRIVILEGES ] ]  
        |  
                permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      }  
      [ ON [ class :: ] securable ]   
      { TO | FROM } principal [ ,...n ]   
      [ CASCADE] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
REVOKE   
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]   
    [ FROM | TO ] principal [ ,...n ]  
    [ CASCADE ]  
[;]  
  
<permission> ::=  
{ see the tables below }  
  
<class_type> ::=  
{  
      LOGIN  
    | DATABASE  
    | OBJECT  
    | ROLE  
    | SCHEMA  
    | USER  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 GRANT OPTION FOR  
 指示将撤消授予指定权限的能力。 在使用 CASCADE 参数时，需要具备该功能。  
  
> [!IMPORTANT]  
>  如果主体具有不带 GRANT 选项的指定权限，则将撤消该权限本身。  
  
 ALL  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 该选项不会撤消所有可能的权限。 撤消 ALL 相当于撤消以下权限。  
  
-   如果安全对象是数据库，则 ALL 对应 BACKUP DATABASE、BACKUP LOG、CREATE DATABASE、CREATE DEFAULT、CREATE FUNCTION、CREATE PROCEDURE、CREATE RULE、CREATE TABLE 和 CREATE VIEW。  
  
-   如果安全对象是标量函数，则 ALL 对应 EXECUTE 和 REFERENCES。  
  
-   如果安全对象是表值函数，则 ALL 对应 DELETE、INSERT、REFERENCES、SELECT 和 UPDATE。  
  
-   如果安全对象是存储过程，则 ALL 表示 EXECUTE。  
  
-   如果安全对象是表，则 ALL 对应 DELETE、INSERT、REFERENCES、SELECT 和 UPDATE。  
  
-   如果安全对象是视图，则 ALL 对应 DELETE、INSERT、REFERENCES、SELECT 和 UPDATE。  
  
> [!NOTE]  
>  不推荐使用 REVOKE ALL 语法。 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]应改为撤销特定权限。  
  
 PRIVILEGES  
 包含此参数是为了符合 ISO 标准。 请不要更改 ALL 的行为。  
  
 permission  
 权限的名称。 本主题后面的[特定于安全对象的语法](#securable)部分所列出的主题介绍了权限和安全对象之间的有效映射。  
  
 *column*  
 指定表中将撤消其权限的列的名称。 需要使用括号。  
  
 *class*  
 指定将撤消其权限的安全对象的类。 需要使用作用域限定符 ::。  
  
 securable  
 指定将撤消其权限的安全对象。  
  
 TO | FROM principal  
 主体的名称。 可撤消其对安全对象的权限的主体随安全对象而异。 有关有效组合的详细信息，请参阅本主题后面的[特定于安全对象的语法](#securable)部分所列出的主题。  
  
 CASCADE  
 指示当前正在撤消的权限也将从其他被该主体授权的主体中撤消。 使用 CASCADE 参数时，还必须同时指定 GRANT OPTION FOR 参数。  
  
> [!CAUTION]  
>  如果对授予了 WITH GRANT OPTION 权限的权限执行级联撤消，将同时撤消该权限的 GRANT 和 DENY 权限。  
  
 AS principal  
 使用 AS principal 子句指示将撤销由你以外的主体所授予的权限。 例如，假设用户 Mary 是 principal_id 12，用户 Raul 是 principal_id 15。 Mary 和 Raul 为用户 Steven 授予相同权限。 Sys.database_permissions 表两次指示该权限，但分别具有不同的 grantor_prinicpal_id 值。 Mary 可以使用 `AS RAUL` 子句撤销权限以移除 Raul 授予的权限。
 
在此语句中使用 AS 并不意味着能够模拟其他用户。  
  
## <a name="remarks"></a>备注  
 REVOKE 语句的完整语法非常复杂。 上面的语法关系图进行了简化以突出其结构。 本主题后面的[特定于安全对象的语法](#securable)部分中列出的主题介绍了用于撤销特定安全对象的权限的完整语句。  
  
 REVOKE 语句可用于删除已授予的权限，DENY 语句可用于防止主体通过 GRANT 获得特定权限。  
  
 授予权限将删除对所指定安全对象的相应权限的 DENY 或 REVOKE 权限。 如果在包含该安全对象的更高级别拒绝了相同的权限，则 DENY 优先。 但是，在更高级别撤销已授予权限的操作并不优先。  
  
> [!CAUTION]  
>  表级 DENY 并不优先于列级 GRANT。 保留了权限层次结构中的这种不一致性以保持向后兼容。 未来的版本会将其删除。  
  
 sp_helprotect 系统存储过程报告在数据库级安全对象上的权限。  
  
 在撤消通过指定 GRANT OPTION 为其赋予权限的主体的权限时，如果未指定 CASCADE，则将无法成功执行 REVOKE 语句。  
  
## <a name="permissions"></a>权限  
 对安全对象具有 CONTROL 权限的主体可以撤消该安全对象的权限。 对象所有者可以撤消他们所拥有的对象的权限。  
  
 具备 CONTROL SERVER 权限的被权限者（例如 sysadmin 固定服务器角色的成员）可以撤消对该服务器的任何安全对象所拥有的任何权限。 对数据库具有 CONTROL 权限的被授权者（例如 db_owner 固定数据库角色的成员）可以撤消对该数据库的任何安全对象所拥有的任何权限。 被授予架构的 CONTROL 权限的用户可以撤消针对该架构的任何对象的任何权限。  
  
##  <a name="securable-specific-syntax"></a><a name="securable"></a>特定于安全对象的语法  
 下表列出了安全对象以及描述特定于安全对象的语法的主题。  
  
|安全对象|主题|  
|---------------|-----------|  
|应用程序角色|[REVOKE 数据库主体权限 (Transact-SQL)](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|程序集|[REVOKE 程序集权限 (Transact-SQL)](../../t-sql/statements/revoke-assembly-permissions-transact-sql.md)|  
|非对称密钥|[REVOKE 非对称密钥权限 (Transact-SQL)](../../t-sql/statements/revoke-asymmetric-key-permissions-transact-sql.md)|  
|可用性组|[REVOKE 可用性组权限 (Transact-SQL)](../../t-sql/statements/revoke-availability-group-permissions-transact-sql.md)|  
|证书|[REVOKE 证书权限 (Transact-SQL)](../../t-sql/statements/revoke-certificate-permissions-transact-sql.md)|  
|合约|[REVOKE Service Broker 权限 (Transact-SQL)](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|数据库|[REVOKE 数据库权限 (Transact-SQL)](../../t-sql/statements/revoke-database-permissions-transact-sql.md)|  
|终结点|[REVOKE 终结点权限 (Transact-SQL)](../../t-sql/statements/revoke-endpoint-permissions-transact-sql.md)|  
|数据库作用域凭据|[REVOKE 数据库作用域凭据 (Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)|  
|全文目录|[REVOKE 全文权限 (Transact-SQL)](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|全文非索引字表|[REVOKE 全文权限 (Transact-SQL)](../../t-sql/statements/revoke-full-text-permissions-transact-sql.md)|  
|函数|[REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|登录|[撤消服务器主体权限 (Transact-SQL)](../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)|  
|消息类型|[REVOKE Service Broker 权限 (Transact-SQL)](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|Object|[REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|队列|[REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|远程服务绑定|[REVOKE Service Broker 权限 (Transact-SQL)](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|角色|[REVOKE 数据库主体权限 (Transact-SQL)](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|路由|[REVOKE Service Broker 权限 (Transact-SQL)](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|架构|[REVOKE 架构权限 (Transact-SQL)](../../t-sql/statements/revoke-schema-permissions-transact-sql.md)|  
|搜索属性列表|[REVOKE 搜索属性列表权限 (Transact-SQL)](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)|  
|服务器|[REVOKE 服务器权限 (Transact-SQL)](../../t-sql/statements/revoke-server-permissions-transact-sql.md)|  
|服务|[REVOKE Service Broker 权限 (Transact-SQL)](../../t-sql/statements/revoke-service-broker-permissions-transact-sql.md)|  
|存储过程|[REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|对称密钥|[REVOKE 对称密钥权限 (Transact-SQL)](../../t-sql/statements/revoke-symmetric-key-permissions-transact-sql.md)|  
|同义词|[REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|系统对象|[REVOKE 系统对象权限 (Transact-SQL)](../../t-sql/statements/revoke-system-object-permissions-transact-sql.md)|  
|表|[REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|类型|[REVOKE 类型权限 (Transact-SQL)](../../t-sql/statements/revoke-type-permissions-transact-sql.md)|  
|用户|[REVOKE 数据库主体权限 (Transact-SQL)](../../t-sql/statements/revoke-database-principal-permissions-transact-sql.md)|  
|视图|[REVOKE 对象权限 (Transact-SQL)](../../t-sql/statements/revoke-object-permissions-transact-sql.md)|  
|XML 架构集合|[REVOKE XML 架构集合权限 (Transact-SQL)](../../t-sql/statements/revoke-xml-schema-collection-permissions-transact-sql.md)|  
  
## <a name="see-also"></a>另请参阅  
 [权限层次结构（数据库引擎）](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT (Transact-SQL)](../../t-sql/statements/grant-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_adduser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)   
 [sp_changedbowner (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)   
 [sp_helprotect (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)   
 [sp_helpuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)  
  
  
