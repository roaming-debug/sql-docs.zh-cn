---
description: 实现 SQL Server 代理安全性
title: 实现 SQL Server 代理安全性
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, security
- security [SQL Server Agent], about security
- security [SQL Server Agent]
- security [SQL Server], SQL Server Agent
ms.assetid: d770d35c-c8de-4e00-9a85-7d03f45a0f0d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7bd95527dd51981c3ba3b5d44e4beb2be6a1db80
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342672"
---
# <a name="implement-sql-server-agent-security"></a>实现 SQL Server 代理安全性
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理使数据库管理员能够在一个安全上下文中运行每个作业步骤，这个安全上下文只具有执行该作业步骤所需的权限，这是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理决定的。 若要为某个特定的作业步骤设置权限，可以创建一个具有所需权限的代理，然后将该代理分配给该作业步骤。 一个代理可以指定给多个作业步骤。 对于需要相同权限的作业步骤，可以使用同一个代理。  
  
下面的内容将解释必须为用户授予什么样的数据库角色，他们才能使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理创建或执行作业。  
  
## <a name="granting-access-to-sql-server-agent"></a>授予访问 SQL Server 代理的权限  
若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，用户必须是下列一个或多个固定数据库角色的成员：  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
这些角色存储在 **msdb** 数据库中。 默认情况下，任何用户都不是这些数据库角色的成员。 必须显式授予这些角色中的成员身份。 作为 **sysadmin** 固定服务器角色成员的用户可以完全访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理，不需要成为这些固定数据库角色的成员便可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理。 如果某个用户既不是这些数据库角色的成员，也不是 **sysadmin** 角色的成员，那么当他们使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，不能访问 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]代理节点。  
  
这些数据库角色的成员可以查看和执行它们所拥有的作业，还可以创建作为现有代理帐户运行的作业步骤。 有关与每个这些角色关联的特定权限的详细信息，请参阅 [SQL Server 代理固定数据库角色](../../ssms/agent/sql-server-agent-fixed-database-roles.md)。  
  
**sysadmin** 固定服务器角色的成员具有创建、修改和删除代理帐户的权限。 **sysadmin** 角色的成员可以创建作业步骤，无需指定代理，但需作为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户运行，该帐户是用于启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的帐户。  
  
## <a name="guidelines"></a>指南  
遵循下列指导原则可以提高 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理实现的安全性：  
  
-   专门为代理创建专用的用户帐户，并且只使用这些代理用户帐户来运行作业步骤。  
  
-   只为代理用户帐户授予必需的权限。 只授予运行分配给给定代理帐户的作业步骤实际所需的那些权限。  
  
-   不要使用作为 Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Administrators **组成员的 Microsoft Windows 帐户运行** 代理服务。  
  
-   代理仅具有与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 凭据存储区相同的安全性。  
  
-   如果用户写入操作可对 NT 事件日志进行写入，则用户可通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理引发警报。  
  
-   请不要将 NT 管理帐户指定为服务帐户或代理帐户。  
  
-   请注意，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理有权互相访问资产。 这两项服务共享一个进程空间，并且 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的 sysadmin。  
  
-   当 TSX 使用 MSX 进行登记时，MSX sysadmins 将获得对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的 TSX 实例的完全控制权。  
  
-   ACE 是一个扩展插件，不能调用自身。 ACE 由 Chainer ScenarioEngine.exe（也称为 Microsoft.SqlServer.Chainer.Setup.exe）调用，也可由其他主机进程调用。  
  
-   ACE 取决于 SSDP 拥有的以下配置 DLL，因为这些 DLL 的 API 是由 ACE 调用的：  
  
    -   **SCO** - Microsoft.SqlServer.Configuration.Sco.dll，包括针对虚拟帐户的新 SCO 验证  
  
    -   **Cluster** - Microsoft.SqlServer.Configuration.Cluster.dll  
  
    -   **SFC** - Microsoft.SqlServer.Configuration.SqlConfigBase.dll  
  
    -   **Extension** - Microsoft.SqlServer.Configuration.ConfigExtension.dll  
  
## <a name="see-also"></a>另请参阅  
[使用预定义角色](../../reporting-services/security/role-definitions-predefined-roles.md)  
[sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
[sp_droprolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)  
[安全性和保护（数据库引擎）](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
