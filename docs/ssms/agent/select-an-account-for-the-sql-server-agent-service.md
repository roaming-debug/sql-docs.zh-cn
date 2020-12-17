---
description: 为 SQL Server 代理服务选择帐户
title: 为 SQL Server 代理服务选择帐户
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- roles [SQL Server], SQL Server Agent
- SQL Server Agent, accounts
- startup accounts [SQL Server]
- SQL Server Agent service, accounts
- accounts [SQL Server], SQL Server Agent
- Windows groups [SQL Server Agent]
- SQL Server Agent, permissions
- members [SQL Server], SQL Server Agent service
- Windows domain accounts [SQL Server]
- security [SQL Server], SQL Server Agent
ms.assetid: fe658e32-9e6b-4147-a189-7adc3bd28fe7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 05/04/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 121799da3fe6d259c92deb900fe21833cbfceef2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464368"
---
# <a name="select-an-account-for-the-sql-server-agent-service"></a>为 SQL Server 代理服务选择帐户

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

服务启动帐户可以定义运行 [!INCLUDE[msCoName](../../includes/msconame_md.md)] 代理的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 帐户及其网络权限。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理在指定的用户帐户下运行。 可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务选择一个帐户，可选帐户如下：  
  
-   **内置帐户**。 可以从下列内置 Windows 服务帐户的列表中选择：  
  
    -   **Local System** 帐户。 此帐户的名称是 NT AUTHORITY\System。 它是一个功能强大的帐户，可以不受限制地访问所有本地系统资源。 它是本地计算机上 Windows“管理员”组的成员，因此也是  sysadmin[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  固定服务器角色的成员。  
  
        > [!IMPORTANT]  
        > 提供“Local System 帐户”选项只是为了向后兼容。 本地系统帐户具有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理不需要的权限。 避免使用本地系统帐户运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理。 为了提高安全性，请使用具有下面部分“Windows 域帐户权限”中所列出权限的 Windows 域帐户。  
  
-   **本帐户**。 使您可以指定运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务的 Windows 域帐户。 建议选择非 Windows **管理员** 组成员的 Windows 用户帐户。 但是，当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户不是本地 **管理员** 组的成员时，在使用多服务器管理时存在限制。 有关详细信息，请参阅本主题后面的“支持的服务帐户类型”。  
  
## <a name="windows-domain-account-permissions"></a>Windows 域帐户权限  
为了提高安全性，可以选择“本帐户”来指定 Windows 域帐户。 指定的 Windows 域帐户必须具有下列权限：  
  
-   在所有 Windows 版本中，作为服务登录的权限 (SeServiceLogonRight)  
  
> [!NOTE]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户必须是域控制器上 Pre-Windows 2000 Compatible Access 组的一部分，否则，非 Windows Administrators 组成员的域用户拥有的作业将失败。  
  
-   在 Windows 服务器中，运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务的帐户需要具有下列权限才能支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的代理帐户。  
  
    -   跳过遍历检查的权限 (SeChangeNotifyPrivilege)  
  
    -   替换进程级别标记的权限 (SeAssignPrimaryTokenPrivilege)  
  
    -   调整进程的内存配额的权限 (SeIncreaseQuotaPrivilege)  
  
    -   通过网络访问此计算机的权限 (SeNetworkLogonRight)  
  
> [!NOTE]  
> 如果帐户不具有支持代理帐户所需的权限，则只有 **sysadmin** 固定服务器角色的成员才可创建作业。  
  
> [!NOTE]  
> 必须为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理的服务帐户授予包含 WMI 事件的命名空间的权限以及 ALTER ANY EVENT NOTIFICATION 权限，才能接收 WMI 警报通知。  
  
## <a name="sql-server-role-membership"></a>SQL Server 角色成员身份  
运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务时使用的帐户必须是下列 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 角色的成员：  
  
-   该帐户必须是 **sysadmin** 固定服务器角色的成员。  
  
-   若要使用多服务器作业处理，帐户必须是主服务器上 **msdb** 数据库角色 **TargetServersRole** 的成员。  
  
## <a name="supported-service-account-types"></a>支持的服务帐户类型  
下表列出了可用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务的 Windows 帐户类型。  
  
|服务帐户类型|非聚集服务器|群集服务器|域控制器（非聚集）|  
|------------------------|-------------------------|--------------------|--------------------------------------|  
|[!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows 域帐户（Windows 管理员组的成员）|支持|支持|支持|  
|Windows 域帐户（非管理）|支持<br /><br />请参阅下面的限制 1。|支持<br /><br />请参阅下面的限制 1。|支持<br /><br />请参阅下面的限制 1。|  
|网络服务帐户 (NT AUTHORITY\NetworkService)|支持<br /><br />请参阅下方的限制 1、3 和 4。|不支持|不支持|  
|本地用户帐户（非管理）|支持<br /><br />请参阅下面的限制 1。|不支持|不适用|  
|本地系统帐户 (NT AUTHORITY\System)|支持<br /><br />请参阅下面的限制 2。|不支持|支持<br /><br />请参阅下面的限制 2。|  
|本地服务帐户 (NT AUTHORITY\LocalService)|不支持|不支持|不支持|  
  
### <a name="limitation-1-using-non-administrative-accounts-for-multiserver-administration"></a>限制 1：针对多服务器管理使用非管理帐户  
目标服务器可能无法登记到主服务器，并出现以下错误信息：“登记操作失败”。  
  
若要解决该错误，请重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务。 有关详细信息，请参阅 [启动、停止、暂停、继续、重启 SQL Server 服务](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)。  
  
### <a name="limitation-2-using-the-local-system-account-for-multiserver-administration"></a>限制 2：针对多服务器管理使用本地系统帐户  
仅当主服务器和目标服务器位于同一台计算机中，并在本地系统帐户下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务时，才支持多服务器管理。 如果使用此配置，则在将目标服务器登记到主服务器时返回以下消息：  
  
“请确保 <target_server_computer_name> 的代理启动帐户拥有以 targetServer 身份登录的权限”。  
  
您可以忽略此信息性消息。 登记操作将成功完成。 有关详细信息，请参阅 [创建多服务器环境](../../ssms/agent/create-a-multiserver-environment.md)。  
  
### <a name="limitation-3-using-the-network-service-account-when-it-is-a-sql-server-user"></a>限制 3：在网络服务帐户为 SQL Server 用户时使用该帐户  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果在网络服务帐户下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务，并显式授予网络服务帐户以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用户身份登录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的访问权限，则可能无法启动代理。  
  
为了解决此问题，请重新启动运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机。 此操作仅需执行一次。  
  
### <a name="limitation-4-using-the-network-service-account-when-sql-server-reporting-services-is-running-on-the-same-computer"></a>限制 4：当同一台计算机中还运行有 SQL Server Reporting Services 时使用网络服务帐户  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果在网络服务帐户下运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务并且在同一台计算机中还运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，则可能无法启动代理。  
  
为了解决此问题，请重新引导运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机，然后重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务。 此操作仅需执行一次。  
  
## <a name="common-tasks"></a>常见任务  
**指定 SQL Server 代理服务的启动帐户**  
  
-   [为 SQL Server 代理设置服务启动帐户&#40;SQL Server 配置管理器&#41;](../../ssms/agent/set-service-startup-account-sql-server-agent-sql-server-configuration-manager.md)  
  
**指定 SQL Server 代理的邮件配置文件**  
  
-   [如何：将 SQL Server 代理邮件配置为使用数据库邮件](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
> [!NOTE]  
> 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器可以指定启动操作系统时必须启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理。  
  
## <a name="see-also"></a>另请参阅  
[设置 Windows 服务帐户](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
[使用 SQL 计算机管理器管理服务](../../database-engine/configure-windows/scm-services-connect-to-another-computer.md)  
[实现 SQL Server 代理安全性](../../ssms/agent/implement-sql-server-agent-security.md)  
