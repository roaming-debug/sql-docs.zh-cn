---
title: 服务器级别角色 | Microsoft Docs
description: SQL Server 提供服务器级别角色。 这些安全主体将其他主体分组，以管理服务器范围内的权限。
ms.custom: ''
ms.date: 05/24/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.Security.NT_AUTHORITY.SYSTEM
- sql13.Security.BUILTIN.administrators
helpviewer_keywords:
- roles [SQL Server], server-level
- principals [SQL Server], server-level
- CONTROL SERVER permission
- fixed server roles [SQL Server]
- credentials [SQL Server], roles
- sysadmin fixed server role
- server-level roles [SQL Server]
- authentication [SQL Server], roles
ms.assetid: 7adf2ad7-015d-4cbe-9e29-abaefd779008
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 60a5fb875bf2ae69a4f87c543476b91844738276
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475578"
---
# <a name="server-level-roles"></a>服务器级别角色
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 提供服务器级角色以帮助你管理服务器上的权限。 这些角色是可组合其他主体的安全主体。 服务器级角色的权限作用域为服务器范围。 （“角色”类似于 Windows 操作系统中的“组”。）  
  
 提供固定服务器角色是为了方便使用和向后兼容。 应尽可能分配更具体的权限。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 提供了九种固定服务器角色。 无法更改授予固定服务器角色（public 角色除外）的权限。 从 [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]开始，您可以创建用户定义的服务器角色，并将服务器级权限添加到用户定义的服务器角色。  
  
 可以将服务器级别主体（[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名、Windows 帐户和 Windows 组）添加到服务器级别角色。 固定服务器角色的每个成员都可以将其他登录名添加到该同一角色。 用户定义的服务器角色的成员则无法将其他服务器主体添加到角色。  
> [!NOTE]
>  服务器级权限不适用于 SQL 数据库或 Azure Synapse Analytics。 有关 SQL 数据库的详细信息，请参阅[控制和授予数据库访问权限](/azure/sql-database/sql-database-manage-logins)。
  
## <a name="fixed-server-level-roles"></a>服务器级的固定角色  
 下表显示了服务器级的固定角色及其权限。  
  
|服务器级的固定角色|说明|  
|------------------------------|-----------------|  
|**sysadmin**|sysadmin 固定服务器角色的成员可以在服务器上执行任何活动。|  
|**serveradmin**|**serveradmin** 固定服务器角色的成员可以更改服务器范围的配置选项和关闭服务器。|  
|**securityadmin**|**securityadmin** 固定服务器角色的成员可以管理登录名及其属性。 他们可以 `GRANT`、`DENY` 和 `REVOKE` 服务器级权限。 他们还可以 `GRANT`、`DENY` 和 `REVOKE` 数据库级权限（如果他们具有数据库的访问权限）。 此外，他们还可以重置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名的密码。<br /><br /> **重要提示：** 如果能够授予对 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 的访问权限和配置用户权限，安全管理员可以分配大多数服务器权限。 **securityadmin** 角色应视为与 **sysadmin** 角色等效。|  
|**processadmin**|processadmin 固定服务器角色的成员可以终止在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例中运行的进程。|  
|**setupadmin**|setupadmin 固定服务器角色的成员可以使用 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 语句添加和删除链接服务器。 （使用 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 时需要 sysadmin 成员资格。）|  
|**bulkadmin**|bulkadmin 固定服务器角色的成员可以运行 `BULK INSERT` 语句。|  
|**diskadmin**|diskadmin 固定服务器角色用于管理磁盘文件。|  
|**dbcreator**|**dbcreator** 固定服务器角色的成员可以创建、更改、删除和还原任何数据库。|  
|**public**|每个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名都属于 public 服务器角色。 如果未向某个服务器主体授予或拒绝对某个安全对象的特定权限，该用户将继承授予该对象的 public 角色的权限。 只有在希望所有用户都能使用对象时，才在对象上分配 Public 权限。 你无法更改具有 Public 角色的成员身份。<br /><br /> 注意：public 与其他角色的实现方式不同，可通过 public 固定服务器角色授予、拒绝或撤销权限 。|  
  
> [!IMPORTANT] 
> 以下服务器角色提供的大多数权限都不适用于 Synapse SQL：processadmin、serveradmin、setupadmin和 diskadmin   。
  
## <a name="permissions-of-fixed-server-roles"></a>固定服务器角色的权限  
 每个固定服务器角色都被分配了特定的权限。 下图显示了分配给服务器角色的权限。   
![fixed_server_role_permissions](../../../relational-databases/security/authentication-access/media/permissions-of-server-roles.png)   
  
> [!IMPORTANT]  
>  **CONTROL SERVER** 权限与 **sysadmin** 固定服务器角色类似，但并不完全相同。 权限并不表示角色成员身份，并且角色成员身份不会授予权限。 （例如， **CONTROL SERVER** 不表示 **sysadmin** 固定服务器角色的成员身份。）但是，有时可在角色和相等的权限之间模拟。 大多数 **DBCC** 命令和许多系统过程要求 **sysadmin** 固定服务器角色的成员身份。 对于需要 **sysadmin** 成员资格的 171 个系统存储过程的列表，请参阅 Andreas Wolter 的以下博客帖子： [CONTROL SERVER vs. sysadmin/sa: permissions, system procedures, DBCC, automatic schema creation and privilege escalation - caveats](http://andreas-wolter.com/en/control-server-vs-sysadmin-sa/)（CONTROL SERVER 与 sysadmin/sa：权限、系统过程、DBCC、自动创建架构和特权升级 - 注意事项）。  
  
## <a name="server-level-permissions"></a>服务器级权限  
 只能向用户定义的服务器角色中添加服务器级权限。 若要列出服务器级权限，请执行下面的语句。 服务器级权限如下：  
  
```  
SELECT * FROM sys.fn_builtin_permissions('SERVER') ORDER BY permission_name;  
```  
  
 有关权限的详细信息，请参阅[权限（数据库引擎）](../../../relational-databases/security/permissions-database-engine.md) 和 [sys.fn_builtin_permissions (Tansact SQL)](../../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)。  
  
## <a name="working-with-server-level-roles"></a>使用服务器级角色  
 下表介绍了可以用于服务器级角色的命令、视图和功能。  
  
|Feature|类型|说明|  
|-------------|----------|-----------------|  
|[sp_helpsrvrole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helpsrvrole-transact-sql.md)|元数据|返回服务器级角色的列表。|  
|[sp_helpsrvrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)|元数据|返回有关服务器级角色成员的信息。|  
|[sp_srvrolepermission (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)|元数据|显示服务器级角色的权限。|  
|[IS_SRVROLEMEMBER (Transact-SQL)](../../../t-sql/functions/is-srvrolemember-transact-sql.md)|元数据|指示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名是否为指定服务器级角色的成员。|  
|[sys.server_role_members (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)|元数据|为每个服务器级角色的每个成员返回一行。|  
|[sp_addsrvrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)|Command|将登录名添加为某个服务器级角色的成员。 已弃用。 应改用 [ALTER SERVER ROLE](../../../t-sql/statements/alter-server-role-transact-sql.md) 。|  
|[sp_dropsrvrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dropsrvrolemember-transact-sql.md)|Command|从服务器级角色中删除 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名或 Windows 用户或组。 已弃用。 应改用 [ALTER SERVER ROLE](../../../t-sql/statements/alter-server-role-transact-sql.md) 。|  
|[CREATE SERVER ROLE (Transact-SQL)](../../../t-sql/statements/create-server-role-transact-sql.md)|Command|创建用户定义的服务器角色。|  
|[ALTER SERVER ROLE (Transact-SQL)](../../../t-sql/statements/alter-server-role-transact-sql.md)|Command|更改服务器角色的成员关系或更改用户定义的服务器角色的名称。|  
|[DROP SERVER ROLE (Transact-SQL)](../../../t-sql/statements/drop-server-role-transact-sql.md)|Command|删除用户定义的服务器角色。|  
|[IS_SRVROLEMEMBER (Transact-SQL)](../../../t-sql/functions/is-srvrolemember-transact-sql.md)|函数|确定服务器角色的成员关系。|  
  
## <a name="see-also"></a>另请参阅  
 [数据库级别的角色](../../../relational-databases/security/authentication-access/database-level-roles.md)   
 [安全性目录视图 (Transact-SQL)](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)   
 [安全函数 (Transact-SQL)](../../../t-sql/functions/security-functions-transact-sql.md)   
 [保护 SQL Server](../../../relational-databases/security/securing-sql-server.md)   
 [授予服务器主体权限 (Transact-SQL)](../../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [撤消服务器主体权限 (Transact-SQL)](../../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)   
 [拒绝服务器主体权限 (Transact-SQL)](../../../t-sql/statements/deny-server-principal-permissions-transact-sql.md)   
 [创建服务器角色](../../../relational-databases/security/authentication-access/create-a-server-role.md)  
  
