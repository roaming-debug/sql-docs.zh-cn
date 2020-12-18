---
title: 数据库级别的角色 | Microsoft Docs
description: SQL Server 提供若干角色，它们是一些安全主体，用于对其他主体进行分组以管理数据库中的权限。
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database, azure-synapse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.database.f1
- sql13.swb.roleproperties.object.f1
- SQL13.SWB.DBROLEPROPERTIES.GENERAL.F1
- sql13.swb.roleproperties.general.f1
helpviewer_keywords:
- db_denydatareader role
- users [SQL Server], database roles
- database-level roles [SQL Server]
- db_denydatawriter role
- roles [SQL Server], database
- principals [SQL Server], database-level
- db_backupoperator role
- credentials [SQL Server], roles
- db_accessadmin role
- schemas [SQL Server], roles
- permissions [SQL Server], roles
- database roles [SQL Server], listed
- db_datareader role
- db_ddladmin role
- db_datawriter role
- db_securityadmin role
- db_owner role
- database roles [SQL Server]
- fixed database roles [SQL Server]
- authentication [SQL Server], roles
- groups [SQL Server], roles
ms.assetid: 7f3fa5f6-6b50-43bb-9047-1544ade55e39
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 268aafa5b95bed4c9e2687fef430aa4a972ea2c7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463158"
---
# <a name="database-level-roles"></a>数据库级别的角色

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  为便于管理数据库中的权限， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 提供了若干“角色”  ，这些角色是用于对其他主体进行分组的安全主体。 它们类似于 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows 操作系统中的组。 数据库级角色的权限作用域为数据库范围。  

若要向数据库角色添加和删除成员，请使用 `ADD MEMBER` ALTER ROLE `DROP MEMBER` 语句的 [和](../../../t-sql/statements/alter-role-transact-sql.md) 选项。 [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] 和 Azure Synapse 不支持这样使用 `ALTER ROLE`。 改为使用较早版本的 [sp_addrolemember](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) 和 [sp_droprolemember](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md) 过程。
  
 存在两种类型的数据库级角色：数据库中预定义的“固定数据库角色”和可以创建的“用户定义的数据库角色”。  
  
 固定数据库角色是在数据库级别定义的，并且存在于每个数据库中。 **db_owner** 数据库角色的成员可以管理固定数据库角色成员身份。 msdb 数据库中还有一些特殊用途的数据库角色。  
  
 可以向数据库级角色中添加任何数据库帐户和其他 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 角色。
  
> [!TIP]  
>  请不要将用户定义的数据库角色添加为固定角色的成员。 这会导致意外的权限升级。  

可以使用 GRANT、DENY 和 REVOKE 语句自定义用户定义数据库角色的权限。 有关详细信息，请参阅 [权限（数据库引擎）](../../../relational-databases/security/permissions-database-engine.md)。

有关所有权限的列表，请参阅 [数据库引擎权限](https://aka.ms/sql-permissions-poster) 招贴。 不能向数据库角色授予服务器级别权限。 不能向数据库角色添加登录名和其他服务器级别主体（如服务器角色）。 对于 [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)]中的服务器级别安全性，请改为使用 [服务器角色](../../../relational-databases/security/authentication-access/server-level-roles.md) 。 不能通过 [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] 和 Azure Synapse 中的角色授予服务器级别权限。

## <a name="fixed-database-roles"></a>固定数据库角色
  
 下表显示了固定数据库角色及其能够执行的操作。 所有数据库中都有这些角色。 无法更改分配给固定数据库角色的权限，“公共”数据库角色除外。   
  
|固定数据库角色名|说明|  
|-------------------------------|-----------------|  
|**db_owner**|**db_owner** 固定数据库角色的成员可以执行数据库的所有配置和维护活动，还可以删除 [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)]中的数据库。 （在 [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] 和 Synapse Analytics 中，某些维护活动需要服务器级别权限，并且不能由 db_owners 执行。）|  
|**db_securityadmin**|db_securityadmin 固定数据库角色的成员可以仅修改自定义角色的角色成员资格和管理权限。 此角色的成员可能会提升其权限，应监视其操作。|  
|**db_accessadmin**|**db_accessadmin** 固定数据库角色的成员可以为 Windows 登录名、Windows 组和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 登录名添加或删除数据库访问权限。|  
|**db_backupoperator**|**db_backupoperator** 固定数据库角色的成员可以备份数据库。|  
|**db_ddladmin**|**db_ddladmin** 固定数据库角色的成员可以在数据库中运行任何数据定义语言 (DDL) 命令。|  
|**db_datawriter**|**db_datawriter** 固定数据库角色的成员可以在所有用户表中添加、删除或更改数据。|  
|**db_datareader**|**db_datareader** 固定数据库角色的成员可以从所有用户表中读取所有数据。|  
|**db_denydatawriter**|**db_denydatawriter** 固定数据库角色的成员不能添加、修改或删除数据库内用户表中的任何数据。|  
|**db_denydatareader**|**db_denydatareader** 固定数据库角色的成员不能读取数据库内用户表中的任何数据。|  

无法更改分配给固定数据库角色的权限。 下图显示了分配给固定数据库角色的权限：

![fixed_database_role_permissions](../../../relational-databases/security/authentication-access/media/permissions-of-database-roles.png)

## <a name="special-roles-for-sssds_md-and-azure-synapse"></a>[!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] 和 Azure Synapse 的特殊角色

这些数据库角色仅存在于虚拟 master 数据库中。 他们的权限仅限于在 master 中执行的操作。 只能向这些角色添加 master 中的数据库用户。 无法向这些角色添加登录名，但可以基于登录名创建用户，然后向角色添加用户。 也可以向这些角色添加 master 中包含的数据库用户。 不过，如果向 master 中的 dbmanager 角色添加包含的数据库用户，这些用户无法用于新建数据库。

|角色名称|说明|  
|--------------------|-----------------|
|**dbmanager** | 可以创建和删除数据库。 创建数据库的 dbmanager 角色的成员成为相应数据库的所有者，这样可便于用户以 dbo 用户身份连接到相应数据库。 Dbo 用户具有数据库中的所有数据库权限。 Dbmanager 角色的成员不一定具有访问非他们所有的数据库的权限。|
|**loginmanager** | 可以创建和删除虚拟 master 数据库中的登录名。|

> [!NOTE]
> 服务器级别主体和 Azure Active Directory 管理员（如果已配置）具有 [!INCLUDE[ssSDS_md](../../../includes/sssds-md.md)] 和 Azure Synapse 中的所有权限，且无需成为任何角色的成员。 有关详细信息，请参阅 [SQL 数据库身份验证和授权：授予访问权限](/azure/azure-sql/database/logins-create-manage)。 

部分数据库角色不适用于 Azure SQL 或 Synapse SQL：
- db_backupoperator 不适用于 Azure SQL 数据库（非托管实例）和 Synapse SQL 无服务器池，因为 T-SQL 备份和还原命令不可用。
- db_datawriter 和 db_denydatawriter 不适用于 Synapse SQL 无服务器，因为它只读取外部数据 。
  
## <a name="msdb-roles"></a>msdb 角色  
 msdb 数据库中包含下表显示的特殊用途的角色。  
  
|msdb 角色名称|说明|  
|--------------------|-----------------|  
|**db_ssisadmin**<br /><br /> **db_ssisoperator**<br /><br /> **db_ssisltduser**|这些数据库角色的成员可以管理和使用 [!INCLUDE[ssIS](../../../includes/ssis-md.md)]。 从早期版本升级的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例可能包含使用 Data Transformation Services (DTS)（而不是 [!INCLUDE[ssIS](../../../includes/ssis-md.md)]）命名的旧版本角色。 有关详细信息，请参阅 [Integration Services Roles（SSIS 服务）](../../../integration-services/security/integration-services-roles-ssis-service.md)。|  
|**dc_admin**<br /><br /> **dc_operator**<br /><br /> **dc_proxy**|这些数据库角色的成员可以管理和使用数据收集器。 有关详细信息，请参阅 [Data Collection](../../../relational-databases/data-collection/data-collection.md)。|  
|**PolicyAdministratorRole**|**db_ PolicyAdministratorRole** 数据库角色的成员可以对基于策略的管理策略和条件执行所有配置和维护活动。 有关详细信息，请参阅 [使用基于策略的管理来管理服务器](../../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)。|  
|**ServerGroupAdministratorRole**<br /><br /> **ServerGroupReaderRole**|这些数据库角色的成员可以管理和使用注册的服务器组。|  
|**dbm_monitor**|在数据库镜像监视器中注册第一个数据库时在 **msdb** 数据库中创建。 在系统管理员为 **dbm_monitor** 角色分配用户之前，该角色没有任何成员。|  
  
> [!IMPORTANT]  
>  **db_ssisadmin** 角色和 **dc_admin** 角色的成员可以将其特权提升为 sysadmin。 因为这些角色可以修改 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包，而 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理的 sysadmin 安全上下文可以执行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 包，所以可以实现特权提升。 若要防止在运行维护计划、数据收集组和其它 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 包时提升特权，请将运行包的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 代理作业配置为具有有限特权的代理帐户，或仅将 **sysadmin** 成员添加到 **db_ssisadmin** 和 **dc_admin** 角色。  

## <a name="working-with-database-level-roles"></a>使用数据库级角色  
 下表说明了用于数据库级角色的命令、视图和函数。  
  
|Feature|类型|说明|  
|-------------|----------|-----------------|  
|[sp_helpdbfixedrole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)|元数据|返回固定数据库角色的列表。|  
|[sp_dbfixedrolepermission (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)|元数据|显示固定数据库角色的权限。|  
|[sp_helprole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)|元数据|返回当前数据库中有关角色的信息。|  
|[sp_helprolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)|元数据|返回有关当前数据库中某个角色的成员的信息。|  
|[sys.database_role_members (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)|元数据|为每个数据库角色的每个成员返回一行。|  
|[IS_MEMBER (Transact-SQL)](../../../t-sql/functions/is-member-transact-sql.md)|元数据|指示当前用户是否为指定 Microsoft Windows 组或 Microsoft SQL Server 数据库角色的成员。|  
|[CREATE ROLE (Transact-SQL)](../../../t-sql/statements/create-role-transact-sql.md)|Command|在当前数据库中创建新的数据库角色。|  
|[ALTER ROLE (Transact-SQL)](../../../t-sql/statements/alter-role-transact-sql.md)|Command|更改数据库角色的名称或成员身份。|  
|[DROP ROLE (Transact-SQL)](../../../t-sql/statements/drop-role-transact-sql.md)|Command|从数据库中删除角色。|  
|[sp_addrole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)|Command|在当前数据库中创建新的数据库角色。|  
|[sp_droprole (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-droprole-transact-sql.md)|Command|从当前数据库中删除数据库角色。|  
|[sp_addrolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)|Command|为当前数据库中的数据库角色添加数据库用户、数据库角色、Windows 登录名或 Windows 组。 除 [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] 和 Azure Synapse 外，所有平台都应改为使用 `ALTER ROLE` 。|  
|[sp_droprolemember (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)|Command|从当前数据库的 SQL Server 角色中删除安全帐户。 除 [!INCLUDE[ssPDW_md](../../../includes/sspdw-md.md)] 和 Azure Synapse 外，所有平台都应改为使用 `ALTER ROLE` 。|
|[GRANT](../../../t-sql/statements/grant-transact-sql.md)| 权限 | 向角色添加权限。
|[DENY](../../../t-sql/statements/deny-transact-sql.md)| 权限 | 拒绝向角色授予权限。
|[REVOKE](../../../t-sql/statements/revoke-transact-sql.md)| 权限 | 撤消以前授予或拒绝的权限。
  
  
## <a name="public-database-role"></a>public 数据库角色  
 每个数据库用户都属于 **public** 数据库角色。 如果未向某个用户授予或拒绝对安全对象的特定权限时，该用户将继承授予该对象的 **public** 角色的权限。 无法将数据库用户从 **public** 角色删除。 
  
## <a name="related-content"></a>相关内容  
 [安全性目录视图 (Transact-SQL)](../../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
 [安全存储过程 (Transact-SQL)](../../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
 [安全函数 (Transact-SQL)](../../../t-sql/functions/security-functions-transact-sql.md)  
  
 [保护 SQL Server](../../../relational-databases/security/securing-sql-server.md)  
  
 [sp_helprotect (Transact-SQL)](../../../relational-databases/system-stored-procedures/sp-helprotect-transact-sql.md)  
  
