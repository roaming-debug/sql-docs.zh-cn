---
title: 确定有效的数据库引擎权限 | Microsoft Docs
description: 了解如何确定谁对 SQL Server 数据库引擎中的各种对象拥有权限，包括当前和以前的权限系统。
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- permissions, effective
- effective permissions
ms.assetid: 273ea09d-60ee-47f5-8828-8bdc7a3c3529
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 3852c804ce8ab919072d8a2dbe23b4e0f11b8768
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463128"
---
# <a name="determining-effective-database-engine-permissions"></a>确定有效的数据库引擎权限
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

本文介绍如何确定谁对 SQL Server 数据库引擎中的各种对象拥有权限。 SQL Server 针对数据库引擎实现两个权限系统。 角色固定的旧系统拥有预先配置的权限。 从 SQL Server 2005 开始，提供了更灵活、更精确的系统。 （本文中的信息同样适用于 SQL Server 2005 及更高版本。 某些类型的权限在某些版本的 SQL Server 中不可用。）

> [!IMPORTANT]
>  * 有效权限是这两种权限系统的聚合。 
>  * 拒绝权限超控授予权限。 
>  * 如果某个用户是 sysadmin 固定服务器角色的成员，则不会进一步检查权限，因此不会强制执行拒绝。 
>  * 旧系统和新系统有类似之处。 例如，`sysadmin` 固定服务器角色中的成员身份就类似于拥有 `CONTROL SERVER` 权限。 但是，这两个系统并不完全相同。 例如，如果某个登录名仅拥有 `CONTROL SERVER` 权限，而某个存储过程检查 `sysadmin` 固定服务器角色中的成员身份，则权限检查将会失败。 反之亦然。 


## <a name="summary"></a>总结   
* 服务器级权限可以来自于固定服务器角色或用户定义的服务器角色中的成员身份。 每个人都属于 `public` 固定服务器角色，接收其中分配的任何权限。   
* 服务器级权限可以来自于授予登录名或用户定义的服务器角色的权限。   
* 数据库级权限可以来自于固定数据库角色中的成员身份，或者每个数据库中用户定义的数据库角色中的成员身份。 每个人都属于 `public` 固定数据库角色，接收其中分配的任何权限。   
* 数据库级权限可以来自于授予每个数据库中的用户角色或用户定义的数据库角色的权限。   
* 可从 `guest` 登录名或 `guest` 数据库用户（如果已启用）接收权限。 `guest` 登录名和用户默认已禁用。   
* Windows 用户可以是能够拥有登录名的 Windows 组的成员。 当 Windows 用户将 Windows 令牌连接到 Windows 组的安全标识符以及提供该令牌时，SQL Server 将会了解 Windows 组的成员身份。 由于 SQL Server 不会管理或接收有关 Windows 组成员身份的自动更新，因此，SQL Server 无法可靠地报告从 Windows 组成员身份接收的 Windows 用户的权限。   
* 可以通过切换到应用程序角色并提供密码来获取权限。   
* 可以通过执行包含 `EXECUTE AS` 子句的存储过程来获取权限。   
* 拥有 `IMPERSONATE` 权限的登录名或用户可以获取权限。   
* 本地计算机管理员组的成员始终可将其特权提升到 `sysadmin`。 （不适用于 SQL 数据库。）  
* `securityadmin` 固定服务器角色的成员可以提升其许多特权，在某些情况下，可将特权提升到 `sysadmin`。 （不适用于 SQL 数据库。）   
* SQL Server 管理员可以查看有关所有登录名和用户的信息。 特权较低的用户通常只能查看有关其自身标识的信息。

## <a name="older-fixed-role-permission-system"></a>旧的固定角色权限系统

固定服务器角色和固定数据库角色拥有不可更改的预先配置的权限。 若要确定谁是固定服务器角色的成员，请执行以下查询：    
> [!NOTE]
>  不适用于无法使用服务器级别权限的 SQL 数据库或 Azure Synapse Analytics。 SQL Server 2012 中已添加 `sys.server_principals` 的 `is_fixed_role` 列。 更低版本的 SQL Server 不需要此列。  
> ```sql
> SELECT SP1.name AS ServerRoleName, 
>  isnull (SP2.name, 'No members') AS LoginName   
>  FROM sys.server_role_members AS SRM
>  RIGHT OUTER JOIN sys.server_principals AS SP1
>    ON SRM.role_principal_id = SP1.principal_id
>  LEFT OUTER JOIN sys.server_principals AS SP2
>    ON SRM.member_principal_id = SP2.principal_id
>  WHERE SP1.is_fixed_role = 1 -- Remove for SQL Server 2008
>  ORDER BY SP1.name;
> ```
> [!NOTE]
>  * 所有登录名都是 public 角色的成员，不可删除。 
>  * 此查询将检查 master 数据库中的表，但可以在本地产品上的任何数据库中执行此查询。 

若要确定谁是固定数据库角色的成员，请在每个数据库中执行以下查询。
```sql
SELECT DP1.name AS DatabaseRoleName, 
   isnull (DP2.name, 'No members') AS DatabaseUserName 
 FROM sys.database_role_members AS DRM
 RIGHT OUTER JOIN sys.database_principals AS DP1
   ON DRM.role_principal_id = DP1.principal_id
 LEFT OUTER JOIN sys.database_principals AS DP2
   ON DRM.member_principal_id = DP2.principal_id
 WHERE DP1.is_fixed_role = 1
 ORDER BY DP1.name;
```
若要了解授予每个角色的权限，请参阅联机丛书（[服务器级角色](../../../relational-databases/security/authentication-access/server-level-roles.md)和[数据库级角色](../../../relational-databases/security/authentication-access/database-level-roles.md)）中演示的角色说明。

## <a name="newer-granular-permission-system"></a>新的粒度权限系统

此系统具有弹性，这意味着，如果将它设置得很精确，它可能会变得很复杂。 为了简化操作，它可以帮助创建角色、将权限分配给角色，然后将用户组添加到角色。 如果数据库开发团队按架构划分活动，然后向整个架构而不是单个表或过程授予角色权限，则操作就会更容易。 实际方案很复杂，且业务需求可创建意外的安全要求。   

[!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]


### <a name="security-classes"></a>安全类

可在服务器级别、数据库级别、架构级别或对象等等级别授予权限。共有 26 个级别（称为类）。 类的完整列表按字母顺序分别包括：`APPLICATION ROLE`、`ASSEMBLY`、`ASYMMETRIC KEY`、`AVAILABILITY GROUP`、`CERTIFICATE`、`CONTRACT`、`DATABASE`、`DATABASE` `SCOPED CREDENTIAL`、`ENDPOINT`、`FULLTEXT CATALOG`、`FULLTEXT STOPLIST`、`LOGIN`、`MESSAGE TYPE`、`OBJECT`、`REMOTE SERVICE BINDING`、`ROLE`、`ROUTE`、`SCHEMA`、`SEARCH PROPERTY LIST`、`SERVER`、`SERVER ROLE`、`SERVICE`、`SYMMETRIC KEY`、`TYPE`、`USER`、`XML SCHEMA COLLECTION`。 （某些类在某些类型的 SQL Server 中不可用。）若要提供有关每个类的完整信息，需要执行不同的查询。

### <a name="principals"></a>主体

权限将授予主体。 主体可以是服务器角色、登录名、数据库角色或用户。 登录名可以代表包含许多 Windows 用户的 Windows 组。 由于 Windows 组不由 SQL Server 维护，因此，SQL Server 不一定总知道谁是 Windows 组的成员。 当某个 Windows 用户连接到 SQL Server 时，登录数据包中包含该用户的 Windows 组成员身份令牌。

当某个 Windows 用户使用基于 Windows 组的登录名进行连接时，某些活动可能需要 SQL Server 创建登录名或用户来代表单个 Windows 用户。 例如，某个 Windows 组（“工程师”）包含用户（Mary、Todd、Pat），“工程师”组包含数据库用户帐户。 如果 Mary 拥有权限并创建了一个表，则可以创建用户 (Mary) 作为该表的所有者。 或者，如果拒绝 Todd 拥有“工程师”组中其他成员所拥有的权限，则必须创建用户 Todd 来跟踪权限拒绝。

请记住，一个 Windows 用户可以是多个 Windows 组（例如“工程师”和“经理”）的成员。 针对“工程师”登录名、“经理”登录名、单个用户、用户所属角色授予或拒绝的权限都将被聚合，并评估权限有效性。 `HAS_PERMS_BY_NAME` 函数可以揭示某个用户或登录名是否拥有特定的权限。 但是，没有任何确切的方法可以确定授予或拒绝权限的来源。 研究权限列表，也许还要使用试错方法进行试验。

## <a name="useful-queries"></a>有用的查询

### <a name="server-permissions"></a>服务器权限

以下查询返回在服务器级别授予或拒绝的权限列表。 只能在 master 数据库中执行此查询。   
> [!NOTE]
>  不能在 SQL 数据库或 Azure Synapse Analytics 中授予或查询服务器级权限。   
> ```sql
> SELECT pr.type_desc, pr.name, 
>  isnull (pe.state_desc, 'No permission statements') AS state_desc, 
>  isnull (pe.permission_name, 'No permission statements') AS permission_name 
>  FROM sys.server_principals AS pr
>  LEFT OUTER JOIN sys.server_permissions AS pe
>    ON pr.principal_id = pe.grantee_principal_id
>  WHERE is_fixed_role = 0 -- Remove for SQL Server 2008
>  ORDER BY pr.name, type_desc;
> ```

### <a name="database-permissions"></a>数据库权限

以下查询返回在数据库级别授予或拒绝的权限列表。 只能在每个数据库中执行此查询。   
```sql
SELECT pr.type_desc, pr.name, 
 isnull (pe.state_desc, 'No permission statements') AS state_desc, 
 isnull (pe.permission_name, 'No permission statements') AS permission_name 
FROM sys.database_principals AS pr
LEFT OUTER JOIN sys.database_permissions AS pe
    ON pr.principal_id = pe.grantee_principal_id
WHERE pr.is_fixed_role = 0 
ORDER BY pr.name, type_desc;
```

可将权限表中的每个权限类联接到其他提供有关该安全对象类的信息的系统视图。 例如，以下查询提供受权限影响的数据库对象的名称。    
```sql
SELECT pr.type_desc, pr.name, pe.state_desc, 
 pe.permission_name, s.name + '.' + oj.name AS Object, major_id
 FROM sys.database_principals AS pr
 JOIN sys.database_permissions AS pe
   ON pr.principal_id = pe.grantee_principal_id
 JOIN sys.objects AS oj
   ON oj.object_id = pe.major_id
 JOIN sys.schemas AS s
   ON oj.schema_id = s.schema_id
 WHERE class_desc = 'OBJECT_OR_COLUMN';
```
使用 `HAS_PERMS_BY_NAME` 函数可以确定某个特定用户（在本例中为 `TestUser`）是否拥有某个权限。 例如：   
```sql
EXECUTE AS USER = 'TestUser';
SELECT HAS_PERMS_BY_NAME ('dbo.T1', 'OBJECT', 'SELECT');
REVERT;
```
有关语法的详细信息，请参阅 [HAS_PERMS_BY_NAME](../../../t-sql/functions/has-perms-by-name-transact-sql.md)。

## <a name="see-also"></a>另请参阅：

[数据库引擎权限入门](../../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)    
[教程：数据库引擎入门](../../../relational-databases/tutorial-getting-started-with-the-database-engine.md) 

