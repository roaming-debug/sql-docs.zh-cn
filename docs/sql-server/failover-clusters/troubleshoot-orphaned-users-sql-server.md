---
title: 孤立用户疑难解答
description: 当数据库用户登录名不再存在于 master 数据库中时，就会出现孤立用户。 本主题讨论如何识别和解析孤立用户。
ms.custom: seo-lt-2019
ms.date: 07/14/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: high-availability
ms.topic: troubleshooting
helpviewer_keywords:
- orphaned users [SQL Server]
- logins [SQL Server], orphaned users
- troubleshooting [SQL Server], user accounts
- user accounts [SQL Server], orphaned users
- failover [SQL Server], managing metadata
- database mirroring [SQL Server], metadata
- users [SQL Server], orphaned
ms.assetid: 11eefa97-a31f-4359-ba5b-e92328224133
author: cawrites
ms.author: chadam
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016'
ms.openlocfilehash: 105fc8245fa216396117ed32c55aa0b2821f6c0c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477138"
---
# <a name="troubleshoot-orphaned-users-sql-server"></a>孤立用户疑难解答 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  当数据库用户是基于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 主 **数据库中的登录名，但该登录名在** 主 **数据库中已不存在时，** 中会出现孤立用户。 当删除登录名，或者将数据库移动到另一台不存在该登录名的服务器时会出现这种情况。 本主题介绍如何找到孤立用户，并将它们重新映射到登录名。  
  
> [!NOTE]  
>  为可能被移动的数据库使用包含的数据库用户可以减少形成孤立用户的可能性。 有关详细信息，请参阅 [包含的数据库用户 - 使你的数据库可移植](../../relational-databases/security/contained-database-users-making-your-database-portable.md)。  
  
## <a name="background"></a>背景  
 若要使用基于登录名的安全主体（数据库用户标识）连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上的数据库，则该主体必须在 **master** 数据库中拥有有效的登录名。 在身份验证过程中会使用此登录名，以验证主体身份并确定是否允许将主体连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例。 可在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sys.server_principals **目录视图和** sys.sql_logins **兼容性视图中查看服务器实例上的** 登录名。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名作为映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的“数据库用户”访问各个数据库。 此规则有三种例外情况：  
  
-   包含的数据库用户  
  
     包含的数据库用户在用户数据库级别进行身份验证，与登录名无关联。 建议采用这种做法，因为数据库更易于移植，包含的数据库用户不会孤立。 但是必须为每个数据库重新创建用户。 在包含多个数据库的环境中，这可能不切实际。  
  
-   **guest** 帐户。  
  
     在数据库中启用后，此帐户允许未映射到数据库用户的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名作为 **guest** 用户进入数据库。 默认情况下， **guest** 帐户是禁用的。  
  
-   Microsoft Windows 组成员身份。  
  
     如果某 Windows 用户是 Windows 组的成员，并且此组也是数据库中的用户，则基于该 Windows 用户创建的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名可以进入数据库。  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名与数据库用户的映射关系的信息存储在数据库中。 其中包括数据库用户的名称以及对应 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的 SID。 该数据库用户的权限用于在数据库中进行授权。  
  
 在服务器实例上未定义或错误定义了其相应 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的数据库用户（基于登录名）无法登录到该实例。 这样的用户被称为此服务器实例上的数据库的“孤立用户”  。 如果数据库用户映射到 `master` 实例中不存在的登录名 SID，则该用户可能变为孤立用户。 在数据库还原或附加到从未创建过登录名的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 其他实例之后，数据库用户也可能变为孤立用户。 如果删除了对应的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名，则数据库用户可能会变为孤立用户。 即使重新创建该登录名，它也会具有不同的 SID，因此该数据库用户仍为孤立用户。  
  
## <a name="detect-orphaned-users"></a>检测孤立用户  

**对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 PDW**

要在基于缺少 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证登录名的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中检测孤立用户，请在用户数据库中执行以下语句：  
  
```  
SELECT dp.type_desc, dp.SID, dp.name AS user_name  
FROM sys.database_principals AS dp  
LEFT JOIN sys.server_principals AS sp  
    ON dp.SID = sp.SID  
WHERE sp.SID IS NULL  
    AND authentication_type_desc = 'INSTANCE';  
```  
  
 输出中列出了当前数据库中未链接到任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证用户以及对应的安全标识符 (SID)。  

**对于 SQL 数据库和 Azure Synapse Analytics**

`sys.server_principals` 表不适用于 SQL Database 或 Azure Synapse Analytics。 在这些环境中按以下步骤识别孤立用户：

1. 连接到 `master` 数据库，按下列查询为登录名选择 SID：
    ```
    SELECT sid 
    FROM sys.sql_logins 
    WHERE type = 'S'; 
    ```

2. 连接到用户数据库，使用以下查询以查看 `sys.database_principals` 表中用户的 SID：

    ```
    SELECT name, sid, principal_id
    FROM sys.database_principals 
    WHERE type = 'S' 
      AND name NOT IN ('guest', 'INFORMATION_SCHEMA', 'sys')
      AND authentication_type_desc = 'INSTANCE';
    ```

3. 比较两个列表，以确定用户数据库 `sys.database_principals` 表中是否存在与 master 数据库 `sql_logins` 表中不匹配的登录名 SID。 
  
## <a name="resolve-an-orphaned-user"></a>解析孤立用户  
在 master 数据库中，使用带有 SID 选项的 [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) 语句以重新创建缺失的登录名，提供上一部分中获得的数据库用户的 `SID`  
  
```  
CREATE LOGIN <login_name>   
WITH PASSWORD = '<use_a_strong_password_here>',  
SID = <SID>;  
```  
  
 若要将孤立的用户映射到 **master** 数据库中已存在的登录名，请在用户数据库中执行 [ALTER USER](../../t-sql/statements/alter-user-transact-sql.md) 语句，并指定登录名。  
  
```  
ALTER USER <user_name> WITH Login = <login_name>;  
```  
  
 在重新创建缺失的登录名时，用户可以使用提供的密码访问数据库。 然后，用户可以使用 ALTER LOGIN 语句来更改登录帐户的密码。  
  
```  
ALTER LOGIN <login_name> WITH PASSWORD = '<enterStrongPasswordHere>';  
```  
  
> [!IMPORTANT]  
>  任何登录名都可更改其自己的密码。 只有具有 `ALTER ANY LOGIN` 权限的登录名才能更改其他用户的登录密码。 但是，只有 **sysadmin** 角色的成员才能修改 **sysadmin** 角色成员的密码。  
  
## <a name="see-also"></a>另请参阅  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER USER (Transact-SQL)](../../t-sql/statements/alter-user-transact-sql.md)   
 [CREATE USER (Transact-SQL)](../../t-sql/statements/create-user-transact-sql.md)   
 [sys.database_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)   
 [sys.server_principals (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sp_change_users_login (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-change-users-login-transact-sql.md)   
 [sp_addlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md)   
 [sp_grantlogin (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [sp_password (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-password-transact-sql.md)   
 [sys.sysusers (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-sysusers-transact-sql.md)   
 [sys.sql_logins](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md) [sys.syslogins (Transact-SQL)](../../relational-databases/system-compatibility-views/sys-syslogins-transact-sql.md)  
  
  
