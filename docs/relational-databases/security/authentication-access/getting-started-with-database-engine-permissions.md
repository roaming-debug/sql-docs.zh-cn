---
title: 数据库引擎权限入门 | Microsoft Docs
description: 查看 SQL Server 中的一些基本的安全概念，了解典型的数据库引擎权限实现。
ms.custom: ''
ms.date: 01/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: quickstart
helpviewer_keywords:
- permissions [SQL Server], getting started
ms.assetid: 051af34e-bb5b-403e-bd33-007dc02eef7b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 81bb8dd3acae7fda65af0ada009b065c909506b3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460062"
---
# <a name="getting-started-with-database-engine-permissions"></a>数据库引擎权限入门
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 中的权限通过登录名和服务器角色在服务器级别进行管理，以及通过数据库用户和数据库角色在数据库级别进行管理。 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 的模型在每个数据库中公开同一系统，但服务器级别权限不可用。 本主题复习一些基本的安全概念，然后介绍典型的权限实现。  
  
## <a name="security-principals"></a>安全主体  
 安全主体是使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 并可以为其分配执行操作的权限的标识的正式名称。 它们通常是人员或人员组，但可以是伪装成人员的其他实体。 安全主体可以使用列出的 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 或通过使用 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]来进行创建和管理。  
  
##### <a name="logins"></a>登录名  
 登录名是用于登录到 [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)]的单个用户帐户。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 支持基于 Windows 身份验证的登录名和基于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证的登录名。 有关这两种类型的登录名的信息，请参阅 [Choose an Authentication Mode](../../../relational-databases/security/choose-an-authentication-mode.md)。  
  
##### <a name="fixed-server-roles"></a>固定服务器角色  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，固定服务器角色是一组预配置的角色，便于对服务器级别权限进行分组。 可以使用 `ALTER SERVER ROLE ... ADD MEMBER` 语句将登录名添加到角色。 有关详细信息，请参阅 [ALTER SERVER ROLE (Transact-SQL)](../../../t-sql/statements/alter-server-role-transact-sql.md)。 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 不支持固定服务器角色，但在 master 数据库中有两个角色（`dbmanager` 和 `loginmanager`）充当服务器角色。  
  
##### <a name="user-defined-server-roles"></a>用户定义的服务器角色  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，可以创建你自己的服务器角色并向它们分配服务器级权限。 可以使用 `ALTER SERVER ROLE ... ADD MEMBER` 语句将登录名添加到服务器角色。 有关详细信息，请参阅 [ALTER SERVER ROLE (Transact-SQL)](../../../t-sql/statements/alter-server-role-transact-sql.md)。 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 不支持用户定义的服务器角色。  
  
##### <a name="database-users"></a>数据库用户  
 通过在数据库中创建数据库用户并将该数据库用户映射到登录名来授予登录名对数据库的访问权限。 通常，数据库用户名与登录名相同，尽管它不必要相同。 每个数据库用户均映射到单个登录名。 一个登录名只能映射到数据库中的一个用户，但可以映射为多个不同数据库中的数据库用户。  
  
 也可以创建不具有相应登录名的数据库用户。 这些数据库用户称为“包含的数据库用户” 。 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 鼓励使用包含的数据库用户，因为这样可以更轻松地将你的数据库移到另一个服务器。 与登录名类似，包含的数据库用户可以使用 Windows 身份验证或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证。 有关详细信息，请参阅 [包含的数据库用户 - 使你的数据库可移植](../../../relational-databases/security/contained-database-users-making-your-database-portable.md)。  
  
 有 12 种类型的用户，它们在如何进行身份验证以及所表示的人员方面略有差异。 若要查看用户列表，请参阅 [CREATE USER (Transact-SQL)](../../../t-sql/statements/create-user-transact-sql.md)。  
  
##### <a name="fixed-database-roles"></a>固定数据库角色  
 固定数据库角色是一组预配置的提供方便的数据库级权限组的角色。 可以使用 `ALTER ROLE ... ADD MEMBER` 语句将数据库用户和用户定义的数据库角色添加到固定数据库角色。 有关详细信息，请参阅 [ALTER ROLE (Transact-SQL)](../../../t-sql/statements/alter-role-transact-sql.md)。  
  
##### <a name="user-defined-database-roles"></a>用户定义的数据库角色  
 具有 `CREATE ROLE` 权限的用户可以创建新的用户定义的数据库角色来表示具有常用权限的用户组。 通常对整个角色授予或拒绝权限，从而简化了权限管理和监视。 可以使用 `ALTER ROLE ... ADD MEMBER` 语句向数据库角色添加数据库用户。 有关详细信息，请参阅 [ALTER ROLE (Transact-SQL)](../../../t-sql/statements/alter-role-transact-sql.md)。  
  
##### <a name="other-principals"></a>其他主体  
 这里不讨论的其他安全主体包括基于证书或非对称密钥的应用程序角色、登录名和用户。  
  
 有关显示 Windows 用户、Windows 组、登录名和数据库用户之间关系的图形，请参阅 [Create a Database User](../../../relational-databases/security/authentication-access/create-a-database-user.md)。  
  
## <a name="typical-scenario"></a>典型方案  
 下面的示例表示配置权限的常见和建议的方法。  
  
#### <a name="in-active-directory-or-azure-active-directory"></a>在 Active Directory 或 Azure Active Directory 中：  
  
1.  为每个人员创建一个 Windows 用户。  
  
2.  创建表示工作单位和工作职能的 Windows 组。  
  
3.  将 Windows 用户添加到 Windows 组。  

#### <a name="if-the-person-connecting-will-be-connecting-to-many-databases"></a>如果连接的人员将连接到多个数据库  
  
1.  为 Windows 组创建登录名。 （如果使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，请跳过 Active Directory 步骤，并在此处创建 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证登录名。）  
  
2.  在用户数据库中，为表示 Windows 组的登录名创建一个数据库用户。  
  
3.  在用户数据库中，创建一个或多个用户定义的数据库角色，每个角色表示相似的职能。 例如，财务分析人员和销售分析人员。  
  
4.  将数据库用户添加到一个或多个用户定义的数据库角色。  
  
5.  向用户定义的数据库角色授予权限。  
  
#### <a name="if-the-person-connecting-will-be-connecting-to-only-one-database"></a>如果连接的人员将只连接到一个数据库  
  
1.  在用户数据库中，为 Windows 组创建一个包含的数据库用户。 （如果使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证，请跳过 Active Directory 步骤，并在此处创建包含的数据库用户 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 身份验证。）  
  
1.  在用户数据库中，创建一个或多个用户定义的数据库角色，每个角色表示相似的职能。 例如，财务分析人员和销售分析人员。  
  
1.  将数据库用户添加到一个或多个用户定义的数据库角色。  
  
1.  向用户定义的数据库角色授予权限。  
  
 此时的典型结果是，Windows 用户是 Windows 组的成员。 Windows 组在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]中具有登录名。 该登录名将映射到用户数据库中的用户标识。 用户是数据库角色的成员。 现在，你需要将权限添加到角色。  
  
## <a name="assigning-permissions"></a>分配权限  
 大多数权限语句具有以下格式：  
  
```  
AUTHORIZATION  PERMISSION  ON  SECURABLE::NAME  TO  PRINCIPAL;  
```  
  
-   `AUTHORIZATION` 必须为 `GRANT`、 `REVOKE` 或 `DENY`。  
  
-   `PERMISSION` 确立允许或禁止哪个操作。 [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] 可以指定 230 种权限。 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)] 具有较少的权限，因为某些操作在 Azure 中不相关。 这些权限在[权限（数据库引擎）](../../../relational-databases/security/permissions-database-engine.md)主题和下面引用的图表中列出。  
  
-   `ON SECURABLE::NAME` 是安全对象（服务器、服务器对象、数据库或数据库对象）的类型及其名称。 某些权限不需要 `ON SECURABLE::NAME` ，因为它是明确的或在上下文中不适当。 例如，`CREATE TABLE` 权限不需要 `ON SECURABLE::NAME` 子句。 （例如， `GRANT CREATE TABLE TO Mary;` 允许 Mary 创建表。）  
  
-   `PRINCIPAL` 是获得或失去权限的安全主体（登录名、用户或角色）。 尽可能向角色授予权限。  
  
 下面的示例 grant 语句将对 `UPDATE` 架构中包含的 `Parts` 表或视图的 `Production` 权限授予名为 `PartsTeam`的角色：  
  
```  
GRANT UPDATE ON OBJECT::Production.Parts TO PartsTeam;  
```  
  
 使用 `GRANT` 语句向安全主体（登录名、用户和角色）授予权限。 使用  `DENY` 命令显式拒绝权限。 使用 `REVOKE` 语句删除以前授予或拒绝的权限。 权限是累积的，用户将获得授予该用户、登录名和任何组成员身份的所有权限；但是任何权限拒绝将覆盖所有授予。  
  
> [!TIP]  
>  一个常见错误是尝试使用 `GRANT` 而不是 `DENY` 来删除 `REVOKE`。 当用户从多个源获得权限时，这会导致问题；此错误相当常见。 以下示例演示了主体。  
  
 Sales 组通过语句 `SELECT` 获得对 OrderStatus 表的 `GRANT SELECT ON OBJECT::OrderStatus TO Sales;`权限。 用户 Ted 是 Sales 角色的成员。 还通过语句 `SELECT` 在 Ted 自己的用户名下授予 Ted 对 OrderStatus 表的  `GRANT SELECT ON OBJECT::OrderStatus TO Ted;`权限。 假设管理员希望删除对 Sales 角色的 `GRANT` 。  
  
-   如果管理员正确执行 `REVOKE SELECT ON OBJECT::OrderStatus TO Sales;`，则 Ted 将通过其个人 `SELECT` 语句保留对 OrderStatus 表的 `GRANT` 访问权限。  
  
-   如果管理员未正确执行 `DENY SELECT ON OBJECT::OrderStatus TO Sales;` ，则 Ted 作为 Sales 角色的成员将被拒绝 `SELECT` 权限，因为对 Sales 的 `DENY` 将覆盖其个人  `GRANT`。  
  
> [!NOTE]  
>  可以使用 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]配置权限。 在对象资源管理器中查找安全对象，右键单击该安全对象，然后单击“属性”。 选择“权限”页  。 有关使用权限页的帮助，请参阅 [Permissions or Securables Page](../../../relational-databases/security/permissions-or-securables-page.md)。  
  
## <a name="permission-hierarchy"></a>权限层次结构  
 权限具有父/子层次结构。 也就是说，如果你授予对数据库的 `SELECT` 权限，则该权限包括对数据库中所有（子）架构的 `SELECT` 权限。 如果你授予对架构的 `SELECT` 权限，则该权限包括对架构中所有（子）表和视图的 `SELECT` 权限。 权限是可传递的；也就是说，如果你授予对数据库的 `SELECT` 权限，则该权限包括对所有（子级）架构和所有（孙级）表和视图的 `SELECT` 权限。  
  
 权限还可以涵盖权限。 对某个对象的 `CONTROL` 权限通常为你提供对该对象的所有其他权限。  
  
 由于父/子层次结构和包含的层次结构可以作用于相同的权限，因此权限系统可以变得很复杂。 例如，让我们以数据库 (SalesDB) 的架构 (Customers) 中的表 (Region) 为例。  
  
-   `CONTROL` 权限包括对表 Region 的所有其他权限，包括 `ALTER`、 `SELECT`、 `INSERT`、  `UPDATE`、 `DELETE`、 and some other permissions.  
  
-   `SELECT` 包括对 Region 表的 `SELECT` 权限。  
  
 因此，对 Region 表的 `SELECT` 权限可以通过以下六个语句中的任一个实现：  
  
```  
GRANT SELECT ON OBJECT::Region TO Ted;   
  
GRANT CONTROL ON OBJECT::Region TO Ted;   
  
GRANT SELECT ON SCHEMA::Customers TO Ted;   
  
GRANT CONTROL ON SCHEMA::Customers TO Ted;   
  
GRANT SELECT ON DATABASE::SalesDB TO Ted;   
  
GRANT CONTROL ON DATABASE::SalesDB TO Ted;  
```  
  
## <a name="grant-the-least-permission"></a>授予最少权限  
 上面列出的第一个权限 (`GRANT SELECT ON OBJECT::Region TO Ted;`) 的粒度最细，这就是说，该语句是授予 `SELECT`的可能的最少权限。 没有向它附带的从属对象授予权限。 好的主体始终会授予可能的最少权限，但（与此相反）以较高级别授权以简化授权系统。 因此，如果 Ted 需要对整个架构的权限，则只需在架构级别授予 `SELECT` 一次，而不是在表或视图级别授予 `SELECT` 多次。 数据库的设计对此策略可以有多成功的影响很大。 当数据库设计为需要相同权限的对象都包含在单个架构中时，此策略最有效。  
  
## <a name="list-of-permissions"></a>权限的列表  
 [!INCLUDE[ssSQL15](../../../includes/sssql15-md.md)] 具有 230 个权限。 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] 拥有 219 个权限。 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] 拥有 214 个权限。 [!INCLUDE[ssKilimanjaro](../../../includes/sskilimanjaro-md.md)] 拥有 195 个权限。 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]、 [!INCLUDE[ssDW](../../../includes/ssdw-md.md)]和 [!INCLUDE[ssAPS](../../../includes/ssaps-md.md)] 拥有较少的权限，因为他们仅公开数据库引擎的一部分，且一些权限并不适用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 
 
 [!INCLUDE[database-engine-permissions](../../../includes/paragraph-content/database-engine-permissions.md)]
 
 有关显示 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 主体与服务器和数据库对象之间关系的图形，请参阅[权限层次结构（数据库引擎）](../../../relational-databases/security/permissions-hierarchy-database-engine.md)。  
  
## <a name="permissions-vs-fixed-server-and-fixed-database-roles"></a>权限与固定服务器和固定数据库角色  
 固定服务器角色和固定数据库角色的权限很相似，但与细粒度的权限并不完全相同。 例如， `sysadmin` 固定服务器角色的成员在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的实例上具有所有权限，如同使用 `CONTROL SERVER` 权限登录一样。 但是授予 `CONTROL SERVER` 权限不会使登录名成为 sysadmin 固定服务器角色的成员，将登录名添加到  `sysadmin` 固定服务器角色不会显式授予登录名  `CONTROL SERVER` 权限。 有时存储过程将通过检查固定角色而不检查细粒度的权限来检查权限。 例如，分离数据库需要具有 `db_owner` 固定数据库角色中的成员身份。 等效的 `CONTROL DATABASE` 权限并不够。 这两个系统并行运行，但彼此很少进行交互。 Microsoft 建议尽可能使用更新的细粒度权限系统，而不是使用固定角色。
  
## <a name="monitoring-permissions"></a>监视权限  
 以下视图返回安全信息。  
  
-   可以使用 `sys.server_principals` 视图查看服务器上的登录名和用户定义的服务器角色。 此视图在 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]中不可用。  
  
-   可以使用 `sys.database_principals` 视图查看数据库中的用户和用户定义的角色。  
  
-   可以使用 `sys.server_permissions` 视图查看授予登录名和用户定义的固定服务器角色的权限。 此视图在 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]中不可用。  
  
-   可以使用 `sys.database_permissions` 视图查看授予用户和用户定义的固定数据库角色的权限。  
  
-   可以使用 `sys. sys.database_role_members` 视图查看数据库角色成员身份。  
  
-   可以使用 `sys.server_role_members` 视图查看服务器角色成员身份。 此视图在 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]中不可用。  
  
-   有关更多与安全性相关的视图，请参阅 [安全性目录视图 (Transact-SQL)](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md) 来进行创建和管理。  
  
### <a name="useful-transact-sql-statements"></a>有用的 Transact-SQL 语句  
 以下语句返回有关权限的有用信息。  
  
 若要返回在数据库（[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]）中授予或拒绝的显式权限，请在数据库中执行以下语句。  
  
```sql  
SELECT   
    perms.state_desc AS State,   
    permission_name AS [Permission],   
    obj.name AS [on Object],   
    dPrinc.name AS [to User Name]  
FROM sys.database_permissions AS perms  
JOIN sys.database_principals AS dPrinc  
    ON perms.grantee_principal_id = dPrinc.principal_id  
JOIN sys.objects AS obj  
    ON perms.major_id = obj.object_id;  
```  
  
 若要返回服务器角色的成员（仅限 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]），请执行以下语句。  
  
```sql  
SELECT sRole.name AS [Server Role Name] , sPrinc.name AS [Members]  
FROM sys.server_role_members AS sRo  
JOIN sys.server_principals AS sPrinc  
    ON sRo.member_principal_id = sPrinc.principal_id  
JOIN sys.server_principals AS sRole  
    ON sRo.role_principal_id = sRole.principal_id;  
```  
  
 
 若要返回数据库角色的成员（[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDS](../../../includes/sssds-md.md)]），请在数据库中执行以下语句。  
  
```sql  
SELECT dRole.name AS [Database Role Name], dPrinc.name AS [Members]  
FROM sys.database_role_members AS dRo  
JOIN sys.database_principals AS dPrinc  
    ON dRo.member_principal_id = dPrinc.principal_id  
JOIN sys.database_principals AS dRole  
    ON dRo.role_principal_id = dRole.principal_id;  
```  
  
## <a name="next-steps"></a>后续步骤  
 有关可帮助你入门的更多主题，请参阅：  
  
-   [教程：数据库引擎入门](../../../relational-databases/tutorial-getting-started-with-the-database-engine.md) 

-   [创建数据库（教程）](../../../t-sql/lesson-1-creating-database-objects.md)  
  
-   [教程：SQL Server Management Studio](../../../ssms/quickstarts/connect-query-sql-server.md)  
  
-   [教程：编写 Transact-SQL 语句](../../../t-sql/tutorial-writing-transact-sql-statements.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库引擎和 Azure SQL 数据库的安全中心](../../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)   
 [安全函数 (Transact-SQL)](../../../t-sql/functions/security-functions-transact-sql.md)   
 [与安全性相关的动态管理视图和函数 (Transact-SQL)](../../../relational-databases/system-dynamic-management-views/security-related-dynamic-management-views-and-functions-transact-sql.md)   
 [安全性目录视图 (Transact-SQL)](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [sys.fn_builtin_permissions (Transact-SQL)](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [确定有效的数据库引擎权限](../../../relational-databases/security/authentication-access/determining-effective-database-engine-permissions.md)
  
