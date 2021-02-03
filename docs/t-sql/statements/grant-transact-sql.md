---
description: GRANT (Transact-SQL)
title: GRANT (Transact-SQL)
ms.custom: ''
ms.date: 06/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- GRANT_TSQL
- GRANT
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], GRANT statement
- schema-level securables [SQL Server]
- GRANT statement
- cross-database permissions
- GRANT statement, about GRANT statement
- server-level securables [SQL Server]
- database-level securables [SQL Server]
- permissions [SQL Server], granting
ms.assetid: a760c16a-4d2d-43f2-be81-ae9315f38185
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cb24af4f9740f5471a05e17898969a9f39624822
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180199"
---
# <a name="grant-transact-sql"></a>GRANT (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为主体授予安全对象的权限。  一般概念是 GRANT \<some permission> ON \<some object> TO \<some user, login, or group>。 有关权限的一般讨论，请参阅[权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)。  
  
 ![文章链接图标](../../database-engine/configure-windows/media/topic-link.gif "文章链接图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
-- Simplified syntax for GRANT  
GRANT { ALL [ PRIVILEGES ] }  
      | permission [ ( column [ ,...n ] ) ] [ ,...n ]  
      [ ON [ class :: ] securable ] TO principal [ ,...n ]   
      [ WITH GRANT OPTION ] [ AS principal ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
GRANT
    <permission> [ ,...n ]  
    [ ON [ <class_type> :: ] securable ]
    TO principal [ ,...n ]  
    [ WITH GRANT OPTION ]  
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

ALL  
不推荐使用此选项，保留此选项仅用于向后兼容。 它不会授予所有可能的权限。 授予 ALL 等同于授予下列权限：

  - 如果安全对象是数据库，则 ALL 对应 BACKUP DATABASE、BACKUP LOG、CREATE DATABASE、CREATE DEFAULT、CREATE FUNCTION、CREATE PROCEDURE、CREATE RULE、CREATE TABLE 和 CREATE VIEW。  
  
  - 如果安全对象是标量函数，则 ALL 对应 EXECUTE 和 REFERENCES。  
  
  - 如果安全对象是表值函数，则 ALL 对应 DELETE、INSERT、REFERENCES、SELECT 和 UPDATE。  
  
  - 如果安全对象是存储过程，则 ALL 表示 EXECUTE。  
  
  - 如果安全对象是表，则 ALL 对应 DELETE、INSERT、REFERENCES、SELECT 和 UPDATE。  
  
  - 如果安全对象是视图，则 ALL 对应 DELETE、INSERT、REFERENCES、SELECT 和 UPDATE。  

PRIVILEGES  
包含此参数是为了符合 ISO 标准。 请不要更改 ALL 的行为。  
  
permission  
权限的名称。 下面列出的子主题介绍了不同权限与安全对象之间的有效映射。  
  
*column*  
指定表中将授予权限的列的名称。 需要使用圆括号 ()。  
  
class  
指定将授予权限的安全对象的类。 需要使用作用域限定符 ::。  
  
securable  
指定将授予权限的安全对象。  
  
TO principal  
主体的名称。 可为其授予安全对象权限的主体随安全对象而异。 有关有效的组合，请参阅下面列出的子主题。  
  
GRANT OPTION  
指示被授权者在获得指定权限的同时还可以将指定权限授予其他主体。  
  
AS principal  
使用 AS principal 子句指示：记录为权限授予者的主体应为执行该语句的用户以外的主体。 例如，假设用户 Mary 是 principal_id 12，用户 Raul 是主体 15。 Mary 执行 `GRANT SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;` 现在，即使语句的实际执行者是用户 13 (Mary)，sys.database_permissions 表仍将指示 grantor_prinicpal_id 为 15 (Raul)。  

通常不建议使用 AS 子句，除非需要显式定义权限链。 有关详细信息，请参阅[权限（数据库引擎）](../../relational-databases/security/permissions-database-engine.md)中的“权限检查算法摘要”部分。

在此语句中使用 AS 并不意味着能够模拟其他用户。

## <a name="remarks"></a>备注

GRANT 语句的完整语法非常复杂。 上面的语法关系图进行了简化以突出其结构。 下面列出的文章介绍了在授予特定安全对象权限时使用的完整语法。  

REVOKE 语句可用于删除已授予的权限，DENY 语句可用于防止主体通过 GRANT 获得特定权限。  

授予权限将删除对所指定安全对象的相应权限的 DENY 或 REVOKE 权限。 如果在包含该安全对象的更高级别拒绝了相同的权限，则 DENY 优先。 但是，在更高级别撤消已授予权限的操作并不优先。  

数据库级权限在指定的数据库范围内授予。 如果用户需要另一个数据库中的对象的权限，请在该数据库中创建用户帐户，或者授权用户帐户访问该数据库以及当前数据库。  

> [!CAUTION]  
> 表级 DENY 并不优先于列级 GRANT。 保留了权限层次结构中这种不一致性以保持向后兼容。 未来的版本会将其删除。  

sp_helprotect 系统存储过程可报告对数据库级安全对象的权限。  

## <a name="with-grant-option"></a>WITH GRANT OPTION

**GRANT** ...WITH GRANT OPTION 指定向接收权限的安全主体提供向其他安全帐户授予指定权限的能力。 当接收权限的主体是某一角色或某一 Windows 组时，如果需要进一步将对象权限授予不是该组成员或该角色的用户，则必须使用 AS 子句。 因为只有用户（而非某个组或角色）才能执行 GRANT 语句，所以，在授予权限时，该组或角色的特定成员必须使用 AS 子句显式调用该角色或组成员身份 。 下面的示例说明如何在授予角色或 Windows 组时使用 WITH GRANT OPTION。  
  
```sql
-- Execute the following as a database owner  
GRANT EXECUTE ON TestProc TO TesterRole WITH GRANT OPTION;  
EXEC sp_addrolemember TesterRole, User1;  
-- Execute the following as User1  
-- The following fails because User1 does not have the permission as the User1  
GRANT EXECUTE ON TestMe TO User2;  
-- The following succeeds because User1 invokes the TesterRole membership  
GRANT EXECUTE ON TestMe TO User2 AS TesterRole;  
```

## <a name="chart-of-sql-server-permissions"></a>SQL Server 权限图表

若要获取 pdf 格式的所有 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 权限的海报大小的图表，请参阅 [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster)。  

## <a name="permissions"></a>权限

授权者（或用 AS 选项指定的主体）必须具有带 GRANT OPTION 的相同权限，或具有隐含所授予权限的更高权限。 如果使用 AS 选项，则还应满足其他要求。 有关详细信息，请参阅特定于安全对象的文章。

对象所有者可以授予对其所拥有的对象的权限。 对某安全对象具有 CONTROL 权限的主体可以授予对该安全对象的权限。

被授予 CONTROL SERVER 权限的用户（例如，sysadmin 固定服务器角色的成员）可以授予对相应服务器中任一个安全对象的任意权限。 将某一数据库的 CONTROL 权限授予用户（例如，db_owner 固定数据库角色的成员）后，该用户就可授予对该数据库中任何一个安全对象的任意权限。 被授权 CONTROL 权限的用户可以授予对相应架构中任一个对象的任意权限。

## <a name="examples"></a>示例

下表列出了安全对象以及描述特定于安全对象的语法的文章。  

| 安全对象 | GRANT 语法|
| ---------| ------ |
|应用程序角色|[GRANT 数据库主体权限 (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|Assembly|[GRANT 程序集权限 (Transact-SQL)](../../t-sql/statements/grant-assembly-permissions-transact-sql.md)|  
|非对称密钥|[GRANT 非对称密钥权限 (Transact-SQL)](../../t-sql/statements/grant-asymmetric-key-permissions-transact-sql.md)|  
|可用性组|[GRANT 可用性组权限 (Transact-SQL)](../../t-sql/statements/grant-availability-group-permissions-transact-sql.md)|  
|证书|[GRANT 证书权限 (Transact-SQL)](../../t-sql/statements/grant-certificate-permissions-transact-sql.md)|  
|合约|[GRANT Service Broker 权限 (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|数据库|[GRANT 数据库权限 (Transact-SQL)](../../t-sql/statements/grant-database-permissions-transact-sql.md)|
|数据库作用域凭据|[GRANT 数据库作用域凭据 (Transact-SQL)](../../t-sql/statements/grant-database-scoped-credential-transact-sql.md)|  
|端点|[GRANT 终结点权限 (Transact-SQL)](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)|  
|全文目录|[GRANT 全文权限 (Transact-SQL)](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|全文非索引字表|[GRANT 全文权限 (Transact-SQL)](../../t-sql/statements/grant-full-text-permissions-transact-sql.md)|  
|函数|[GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|登录|[GRANT 服务器主体权限 (Transact-SQL)](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)|  
|消息类型|[GRANT Service Broker 权限 (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|对象|[GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|队列|[GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|远程服务绑定|[GRANT Service Broker 权限 (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|角色|[GRANT 数据库主体权限 (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|路由|[GRANT Service Broker 权限 (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|架构|[GRANT 架构权限 (Transact-SQL)](../../t-sql/statements/grant-schema-permissions-transact-sql.md)|  
|搜索属性列表|[GRANT 搜索属性列表权限 (Transact-SQL)](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)|  
|服务器|[GRANT 服务器权限 (Transact-SQL)](../../t-sql/statements/grant-server-permissions-transact-sql.md)|  
|服务|[GRANT Service Broker 权限 (Transact-SQL)](../../t-sql/statements/grant-service-broker-permissions-transact-sql.md)|  
|存储过程|[GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|对称密钥|[GRANT 对称密钥权限 (Transact-SQL)](../../t-sql/statements/grant-symmetric-key-permissions-transact-sql.md)|  
|同义词|[GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|系统对象|[GRANT 系统对象权限 (Transact-SQL)](../../t-sql/statements/grant-system-object-permissions-transact-sql.md)|  
|表|[GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|类型|[GRANT 类型权限 (Transact-SQL)](../../t-sql/statements/grant-type-permissions-transact-sql.md)|  
|用户|[GRANT 数据库主体权限 (Transact-SQL)](../../t-sql/statements/grant-database-principal-permissions-transact-sql.md)|  
|查看|[GRANT 对象权限 (Transact-SQL)](../../t-sql/statements/grant-object-permissions-transact-sql.md)|  
|XML 架构集合|[GRANT XML 架构集合权限 (Transact-SQL)](../../t-sql/statements/grant-xml-schema-collection-permissions-transact-sql.md)|  

## <a name="see-also"></a>另请参阅

- [DENY (Transact-SQL)](../../t-sql/statements/deny-transact-sql.md)
- [REVOKE (Transact-SQL)](../../t-sql/statements/revoke-transact-sql.md)
- [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)
- [sp_adduser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-adduser-transact-sql.md)
- [sp_changedbowner (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)
- [sp_dropuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-dropuser-transact-sql.md)
- [sp_helprotect (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)
- [sp_helpuser (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)
