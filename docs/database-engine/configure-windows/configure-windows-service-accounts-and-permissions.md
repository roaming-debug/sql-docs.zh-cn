---
title: 配置 Windows 服务帐户和权限 | Microsoft Docs
description: 熟悉用于在 SQL Server 中启动和运行服务的服务帐户。 了解如何配置它们并分配适当的权限。
ms.custom: contperfq4
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: reference
helpviewer_keywords:
- startup service states [SQL Server]
- Setup [SQL Server], user accounts
- Windows permissions [SQL Server]
- modifying user accounts
- default accounts
- domains [SQL Server], user accounts
- startup accounts [SQL Server]
- system accounts [SQL Server]
- services [SQL Server], permissions
- ACL (access control list)
- local system accounts [SQL Server]
- instance-aware services [SQL Server]
- permissions [SQL Server], services
- SQL Server Agent service, user accounts
- Windows NT permissions [SQL Server]
- user accounts [SQL Server]
- identifying instance-unaware services [SQL Server]
- installing SQL Server, user accounts
- disabled startup state [SQL Server]
- user accounts [SQL Server], users
- Local Service account [SQL Server]
- SQL Server Installation Wizard
- instance-unaware services [SQL Server]
- services [SQL Server], configuring at installation
- Windows accounts [SQL Server]
- SQL Server services, user accounts
- user accounts [SQL Server], services
- MSSQLServer
- identifying instance-aware services [SQL Server]
- services [SQL Server], accounts
- access control lists
- optional accounts [SQL Server]
- service accounts [SQL Server]
- accounts [SQL Server], services
- built-in system accounts [SQL Server]
- automatic startup state
- domains [SQL Server]
- manual startup state [SQL Server]
- accounts [SQL Server], user
ms.assetid: 309b9dac-0b3a-4617-85ef-c4519ce9d014
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b52093904be2bd3ae649e4472dc9b08ffdea89b6
ms.sourcegitcommit: 7f76975c29d948a9a3b51abce564b9c73d05dcf0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96901022"
---
# <a name="configure-windows-service-accounts-and-permissions"></a>配置 Windows 服务帐户和权限

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的每个服务表示一个进程或一组进程，用于通过 Windows 管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 操作的身份验证。 本主题介绍此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本中服务的默认配置，以及可以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中以及安装之后设置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的配置选项。 本主题将帮助高级用户了解服务帐户的详细信息。

 大多数服务及其属性可通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器进行配置。 以下是在 C 盘安装 Windows 的情况下最新的四个版本的路径。

|SQL Server 版本|路径|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|C:\Windows\SysWOW64\SQLServerManager12.msc|
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|C:\Windows\SysWOW64\SQLServerManager11.msc|

## <a name="services-installed-by-ssnoversion"></a><a name="Service_Details"></a>[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装的服务

根据您决定安装的组件， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将安装以下服务：

- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Database Services** - 用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 关系 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的服务。 可执行文件为 \<MSSQLPATH>\MSSQL\Binn\sqlservr.exe。
- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理** - 执行作业、监视 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、激发警报以及允许自动执行某些管理任务。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]的实例上存在，但处于禁用状态。 可执行文件为 \<MSSQLPATH>\MSSQL\Binn\sqlagent.exe。
- **[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]** - 为商业智能应用程序提供联机分析处理 (OLAP) 和数据挖掘功能。 可执行文件为 \<MSSQLPATH>\OLAP\Bin\msmdsrv.exe。
- **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]** - 管理、执行、创建、计划和传递报表。 可执行文件为 \<MSSQLPATH>\Reporting Services\ReportServer\Bin\ReportingServicesService.exe。
- **[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]** - 为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的存储和执行提供管理支持。 可执行文件的路径为 \<MSSQLPATH>\130\DTS\Binn\MsDtsSrvr.exe

   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 可能包括其他用于横向扩展部署的服务。 有关详细信息，请参阅[演练：安装 Integration Services (SSIS) Scale Out](../../integration-services/scale-out/walkthrough-set-up-integration-services-scale-out.md)。

- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser** - 向客户端计算机提供 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接信息的名称解析服务。 可执行文件的路径为 c:\Program Files (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe
- **全文搜索** - 对结构化和半结构化数据的内容和属性快速创建全文索引，从而为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]提供文档筛选和断字功能。
- **SQL 编写器** - 允许备份和还原应用程序在卷影复制服务 (VSS) 框架中运行。
- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式重播控制器** - 跨多个分布式重播客户端计算机提供跟踪重播业务流程。
- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 客户端** - 与 Distributed Replay 控制器一起来模拟针对 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]实例的并发工作负荷的一台或多台 Distributed Replay 客户端计算机。
- **[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]** - 用于托管 Microsoft 提供的外部可执行文件的可信服务，例如作为 R Services 或机器学习服务的一部分安装的 R 或 Python 运行时。 附属进程可由启动板进程启动，但将根据单个实例的配置进行资源调控。 启动板服务在其自己的用户帐户下运行，特定注册运行时的各个附属进程将继承启动板的用户帐户。 附属进程将在执行过程中按需创建和销毁。

  如果安装 SQL Server 的计算机同时用作域控制器，则启动板无法创建其自己使用的帐户。 因此无法在作用域控制器上安装 R Services（数据库内）或机器学习服务（数据库内）。

- **SQL Server PolyBase 引擎** - 提供对外部数据源的分布式查询功能。
- **SQL Server Polybase 数据移动服务** - 支持在 SQL Server 和外部数据源之间以及在 PolyBase 横向扩展组中的 SQL 节点之间移动数据。

## <a name="service-properties-and-configuration"></a><a name="Serv_Prop"></a> 服务属性和配置

用于启动和运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的启动帐户可以是 [域用户帐户](#Domain_User)、 [本地用户帐户](#Local_User)、 [托管服务帐户](#MSA)、 [虚拟帐户](#VA_Desc)或 [内置系统帐户](#Local_Service)。 若要启动和运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的每项服务，这些服务都必须有一个在安装过程中配置的启动帐户。

此部分介绍可配置为启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的帐户、[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序使用的默认值、Per-service SID 的概念、启动选项以及配置防火墙。

- [默认服务帐户](#Default_Accts)
- [自动启动](#Auto_Start)
- [配置服务启动类型](#Configure_services)
- [防火墙端口](#Firewall)

### <a name="default-service-accounts"></a><a name="Default_Accts"></a> 默认服务帐户

下表列出了安装程序在安装所有组件时使用的默认服务帐户。 列出的默认帐户是建议使用的帐户，但特殊注明的除外。

**独立服务器或域控制器**

|组件|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)]|Windows 7 和 [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 及更高版本|
|---------------|------------------------------------|----------------------------------------------------------------|
|[!INCLUDE[ssDE](../../includes/ssde-md.md)]|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)*|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)*|
|[!INCLUDE[ssAS](../../includes/ssas-md.md)]|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)\*\*\*|
|[!INCLUDE[ssIS](../../includes/ssis-md.md)]|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)\*|
|[!INCLUDE[ssRS](../../includes/ssrs.md)]|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)\*|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)\*|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 客户端|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)\*|
|FD 启动器（全文搜索）|[LOCAL SERVICE](#Local_Service)|[虚拟帐户](#VA_Desc)|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|[LOCAL SERVICE](#Local_Service)|[LOCAL SERVICE](#Local_Service)|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS 编写器|[LOCAL SYSTEM](#Local_System)|[LOCAL SYSTEM](#Local_System)|
|[!INCLUDE[rsql_extensions](../../includes/rsql-extensions-md.md)]|NTSERVICE\MSSQLLaunchpad|NTSERVICE\MSSQLLaunchpad|
|PolyBase 引擎 |[NETWORK SERVICE](#Network_Service) |[NETWORK SERVICE](#Network_Service)|
|PolyBase 数据移动服务 |[NETWORK SERVICE](#Network_Service) |[NETWORK SERVICE](#Network_Service)|

\*当需要 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 计算机外部的资源时，[!INCLUDE[msCoName](../../includes/msconame-md.md)] 建议使用配置了必需的最小特权的托管服务帐户 (MSA)。
\*\*在域控制器上安装时，不支持虚拟帐户作为服务帐户。

**SQL Server 故障转移群集实例**

|组件|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)]|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2|
|---------------|------------------------------------|---------------------------------------|
|[!INCLUDE[ssDE](../../includes/ssde-md.md)]|无。 提供 [域用户](#Domain_User) 帐户。|提供 [域用户](#Domain_User) 帐户。|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理|无。 提供 [域用户](#Domain_User) 帐户。|提供 [域用户](#Domain_User) 帐户。|
|[!INCLUDE[ssAS](../../includes/ssas-md.md)]|无。 提供 [域用户](#Domain_User) 帐户。|提供 [域用户](#Domain_User) 帐户。|
|[!INCLUDE[ssIS](../../includes/ssis-md.md)]|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)|
|[!INCLUDE[ssRS](../../includes/ssrs.md)]|[NETWORK SERVICE](#Network_Service)|[虚拟帐户](#VA_Desc)|
|FD 启动器（全文搜索）|[LOCAL SERVICE](#Local_Service)|[虚拟帐户](#VA_Desc)|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|[LOCAL SERVICE](#Local_Service)|[LOCAL SERVICE](#Local_Service)|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS 编写器|[LOCAL SYSTEM](#Local_System)|[LOCAL SYSTEM](#Local_System)|

#### <a name="changing-account-properties"></a><a name="Changing_Accounts"></a> 更改帐户属性

> [!IMPORTANT]
>
> - 始终使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具（例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器）来更改 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务使用的帐户，或更改帐户的密码。 除了更改帐户名称以外， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器还可以执行其他配置，例如，更新保护 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的服务主密钥的 Windows 本地安全存储区。 其他工具（例如 Windows 服务控制管理器）可以更改帐户名称，但不更改所有必需的设置。
> - 对于您在 SharePoint 场中部署的 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例，始终使用 SharePoint 管理中心为 [!INCLUDE[ssGeminiMTS](../../includes/ssgeminimts-md.md)] 应用程序和 [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)]更改服务器帐户。 使用管理中心时，关联的设置和权限将更新为使用新的帐户信息。
> - 若要更改 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 选项，请使用 Reporting Services 配置工具。

### <a name="managed-service-accounts-group-managed-service-accounts-and-virtual-accounts"></a><a name="New_Accounts"></a> 托管服务帐户、组托管服务帐户和虚拟帐户

托管服务帐户、组托管服务帐户和虚拟帐户设计用于向关键应用程序（例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ）提供其自己帐户的隔离，同时使管理员无需手动管理这些帐户的服务主体名称 (SPN) 和凭据。 这就使得管理服务帐户用户、密码和 SPN 的过程变得简单得多。

- <a name="MSA"></a> 托管服务帐户

  托管服务帐户 (MSA) 是一种由域控制器创建和管理的域帐户。 它分配给单个成员计算机以用于运行服务。 域控制器将自动管理密码。 您不能使用 MSA 登录到计算机，但计算机可以使用 MSA 来启动 Windows 服务。 MSA 获得读写 servicePrincipalName 权限时，可在 Active Directory 中注册服务主体名称 (SPN)。 MSA 的名称中有一个 **$** 后缀，例如 **DOMAIN\ACCOUNTNAME$** 。 在指定 MSA 时，请将密码留空。 因为将 MSA 分配给单个计算机，它不能用于 Windows 群集的不同节点。

  > [!NOTE]
  > 域管理员必须先在 Active Directory 中创建 MSA，然后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序才能将其用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。

- <a name="GMSA"></a> 组托管服务帐户

  组托管服务帐户 (gMSA) 是针对多个服务器的 MSA。 Windows 为在一组服务器上运行的服务管理服务帐户。 Active Directory 自动更新组托管服务帐户密码，而不重启服务。 你可以配置 SQL Server 服务以使用组托管服务帐户主体。 从 SQL Server 2014 开始，SQL Server 支持适用于独立实例的组管理服务帐户，而 SQL Server 2016 及更高版本还支持适用于故障转移群集实例和可用性组的组管理服务帐户。

  若要使用 SQL Server 2014 或更高版本的 gMSA，操作系统必须是 Windows Server 2012 R2 或更高版本。 装有 Windows Server 2012 R2 的服务器需要应用 [KB 2998082](https://support.microsoft.com/kb/2998082) ，以便服务可以在密码更改后立即登录而不中断。

  有关详细信息，请参阅[组托管服务帐户](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11))

  > [!NOTE]
  > 域管理员必须先在 Active Directory 中创建 gMSA，然后 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序才能将其用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。

- <a name="VA_Desc"></a>**Virtual Accounts**

  虚拟帐户（从 Windows Server 2008 R2 和 Windows 7 开始）是“托管的本地帐户”  ，此类帐户提供以下功能，可简化服务管理。 虚拟帐户是自动管理的，并且虚拟帐户可以访问域环境中的网络。 如果在安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 期间对服务帐户使用默认值，则会使用将实例名称用作服务名称的虚拟帐户，格式为 NT SERVICE\\ _\<SERVICENAME>_ 。 以虚拟帐户身份运行的服务通过使用计算机帐户的凭据（格式为 *<domain_name>* __\\__ *<computer_name>* __$__ ）访问网络资源。 当指定一个虚拟帐户以启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，应将密码留空。 如果虚拟帐户无法注册服务主体名称 (SPN)，则手动注册该 SPN。 有关手动注册 SPN 的详细信息，请参阅 [手动注册 SPN](register-a-service-principal-name-for-kerberos-connections.md)。

  > [!NOTE]
  > 虚拟帐户不能用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例，因为虚拟帐户在群集的每个节点不会有相同 SID。

  下表列出了虚拟帐户名称的示例。

  |服务|虚拟帐户名称|
  |-------------|--------------------------|
  |[!INCLUDE[ssDE](../../includes/ssde-md.md)] 服务的默认实例|**NT SERVICE\MSSQLSERVER**|
  |名为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的 **的** 服务的命名实例|**NT SERVICE\MSSQL$PAYROLL**|
  |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务，位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|**NT SERVICE\SQLSERVERAGENT**|
  |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务，位于名为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PAYROLL **的**|**NT SERVICE\SQLAGENT$PAYROLL**|

有关托管服务帐户和虚拟帐户的详细信息，请参阅 **服务帐户分步指南** 的 [托管服务和虚拟帐户概念](https://technet.microsoft.com/library/dd548356\(WS.10\).aspx) 部分以及 [托管服务帐户常见问题解答 (FAQ)](https://technet.microsoft.com/library/ff641729\(WS.10\).aspx)。

**安全说明：** [!INCLUDE[ssNoteLowRights](../../includes/ssnotelowrights-md.md)] 应尽可能使用 [MSA](#MSA)、[gMSA](#GMSA) 或 [虚拟帐户](#VA_Desc)。 如果无法使用 MSA、gMSA 和虚拟帐户，请在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务中使用特定的低特权用户帐户或域帐户，请勿使用共享帐户。 对不同的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务使用单独的帐户。 不要向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户或服务组授予其他权限。 在支持服务 SID 的情况下，将通过组成员身份或直接将权限授予服务 SID。

### <a name="automatic-startup"></a><a name="Auto_Start"></a> 自动启动

除了具有用户帐户外，每项服务还有用户可控制的三种可能的启动状态：

- **已禁用** 服务已安装但当前未运行。
- **手动** 服务已安装，但仅当另一个服务或应用程序需要该服务的功能时才启动。
- **自动** 服务由操作系统自动启动。

在安装过程中，启动状态处于选中状态。 当安装命名实例时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务应设置为自动启动。

### <a name="configuring-services-during-unattended-installation"></a><a name="Configure_services"></a> 在无人参与的安装过程中配置服务

下表显示了可以在安装过程中配置的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。 对于无人参与的安装，可以在配置文件中或在命令提示符下使用开关。

|SQL Server 服务名称|无人参与安装的开关\*|
|-----------------------------|---------------------------------------------|
|MSSQLSERVER|SQLSVCACCOUNT、SQLSVCPASSWORD、SQLSVCSTARTUPTYPE|
|SQLServerAgent\*\*|AGTSVCACCOUNT、AGTSVCPASSWORD、AGTSVCSTARTUPTYPE|
|MSSQLServerOLAPService|ASSVCACCOUNT、ASSVCPASSWORD、ASSVCSTARTUPTYPE|
|ReportServer|RSSVCACCOUNT、RSSVCPASSWORD、RSSVCSTARTUPTYPE|
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|ISSVCACCOUNT、ISSVCPASSWORD、ISSVCSTARTUPTYPE|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器|DRU_CTLR、CTLRSVCACCOUNT、CTLRSVCPASSWORD、CTLRSTARTUPTYPE、CTLRUSERS|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 客户端|DRU_CLT、CLTSVCACCOUNT、CLTSVCPASSWORD、CLTSTARTUPTYPE、CLTCTLRNAME、CLTWORKINGDIR、CLTRESULTDIR|
|R Services 或机器学习服务|EXTSVCACCOUNT、EXTSVCPASSWORD、ADVANCEDANALYTICS\*\*\*|
|PolyBase 引擎| PBENGSVCACCOUNT、PBENGSVCPASSWORD、PBENGSVCSTARTUPTYPE、PBDMSSVCACCOUNT、PBDMSSVCPASSWORD、PBDMSSVCSTARTUPTYPE、PBSCALEOUT、PBPORTRANGE

\*有关无人参与安装的详细信息和示例语法，请参阅[从命令提示符安装 SQL Server 2016](../install-windows/install-sql-server-from-the-command-prompt.md)。

\*\*[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例和具有高级服务的 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例上处于禁用状态。

\*\*\*当前不支持仅通过交换机设置启动板帐户。 请使用 SQL Server 配置管理器更改帐户或其他服务设置。

### <a name="firewall-port"></a><a name="Firewall"></a> 防火墙端口

在大多数情况下，首次安装时，可以通过与 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 安装在相同计算机上的 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 等此类工具连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序不会在 Windows 防火墙中打开端口。 在将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 配置为侦听 TCP 端口，并且在 Windows 防火墙中打开适当的端口进行连接之前，将无法从其他计算机建立连接。 有关详细信息，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。

## <a name="service-permissions"></a><a name="Serv_Perm"></a> 服务权限

此部分介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的 Per-service SID 配置的权限。

- [服务配置和访问控制](#Serv_SID)
- [Windows 特权和权限](#Windows)
- [授予 SQL Server Per-service SID 或 SQL Server 本地 Windows 组的文件系统权限](#Reviewing_ACLs)
- [授予其他 Windows 用户帐户或组的文件系统权限](#File_System_Other)
- [与非寻常磁盘位置相关的文件系统权限](#Unusual_Locations)
- [查看其他注意事项](#Review_additional_considerations)
- [注册表权限](#Registry)
- [WMI](#WMI)
- [命名管道](#Pipes)

### <a name="service-configuration-and-access-control"></a><a name="Serv_SID"></a> 服务配置和访问控制

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 会为其每项服务启用 Per-service SID，以提供深层服务隔离与防御。 Per-service SID 从服务名称派生得到，对该服务是唯一的。 例如，[!INCLUDE[ssDE](../../includes/ssde-md.md)]服务的命名实例的服务 SID 名称可能是 NT Service\MSSQL$\<InstanceName>。 通过服务隔离，可直接访问特定的对象，而无需运行高特权帐户，也不会削弱为对象提供的安全保护水平。 通过使用包含服务 SID 的访问控制项， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务可限制对其资源的访问。

> [!NOTE]
> 在 Windows 7 和 [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2（及更高版本）上，Per-service SID 可以是服务使用的虚拟帐户。

对于大多数组件， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 直接为 Per-service 帐户配置 ACL，因此，无需重复资源 ACL 过程即可更改此服务帐户。

当安装 [!INCLUDE[ssAS](../../includes/ssas-md.md)]时，将创建 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的 Per-service SID。 创建了本地 Windows 组，其命名格式为 **SQLServerMSASUser$** _computer_name_ **$** _instance_name_。 Per-service SID **NT SERVICE\MSSQLServerOLAPService** 已被授予本地 Windows 组中的成员资格，而本地 Windows 组在 ACL 中被授予了适当的权限。 如果更改了用来启动 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务的帐户， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器必须更改某些 Windows 权限（如作为服务登录的权限），但分配给本地 Windows 组的权限将仍可用且没有任何更新，因为 Per-service SID 没发生变化。 此方法允许在升级过程中重命名 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务。

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序为 [!INCLUDE[ssAS](../../includes/ssas-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务创建一个本地 Windows 组。 对于这些服务， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将为此本地 Windows 组配置 ACL。

在安装或升级期间，系统可能会将服务或服务 SID 的服务帐户添加为服务组的成员，具体取决于服务配置。

### <a name="windows-privileges-and-rights"></a><a name="Windows"></a> Windows 特权和权限

为启动服务分配的帐户需要对于服务的 **启动、停止和暂停权限** 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将自动分配此权限。 首先，安装远程服务器管理工具 (RSAT)。 请参阅 [Windows 10 的远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。

下表说明 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件使用的 Per-service SID 或本地 Windows 组请求的权限。

|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序授予的权限|
|---------------------------------------|------------------------------------------------------------|
|**[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]:**<br /><br /> （所有权限都将授予 Per-service SID。 默认实例：NT SERVICE\MSSQLSERVER。 命名实例：NT Service\MSSQLServer$InstanceName。）|**以服务身份登录** (SeServiceLogonRight)<br /><br /> **替换进程级别标记** (SeAssignPrimaryTokenPrivilege)<br /><br /> **跳过遍历检查** (SeChangeNotifyPrivilege)<br /><br /> **调整进程的内存配额** (SeIncreaseQuotaPrivilege)<br /><br /> 启动 SQL 编写器的权限<br /><br /> 读取事件日志服务的权限<br /><br /> 读取远程过程调用服务的权限|

|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]代理：\*<br /><br /> （所有权限都将授予 Per-service SID。 默认实例：NT Service\SQLSERVERAGENT。 命名实例：**NT Service\SQLAGENT$** _InstanceName_.)|**作为服务登录** (SeServiceLogonRight)<br /><br /> **替换进程级别标记** (SeAssignPrimaryTokenPrivilege)<br /><br /> **跳过遍历检查** (SeChangeNotifyPrivilege)<br /><br /> 调整进程的内存配额 (SeIncreaseQuotaPrivilege)| |[!INCLUDE[ssAS](../../includes/ssas-md.md)] ：<br /><br /> （所有权限都授予本地 Windows 组。 默认实例：SQLServerMSASUser$ComputerName$MSSQLSERVER。 命名实例：SQLServerMSASUser$ComputerName$InstanceName。 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 实例：**SQLServerMSASUser$** _ComputerName_ **$** _PowerPivot_.)|**作为服务登录** (SeServiceLogonRight)<br /><br /> 仅适用于表格：<br /><br /> **增加进程工作集** (SeIncreaseWorkingSetPrivilege)<br /><br /> **调整进程的内存配额** (SeIncreaseQuotaPrivilege)<br /><br /> “锁定内存页”(SeLockMemoryPrivilege) - 仅当完全关闭分页时才需要。<br /><br /> 仅适用于故障转移群集安装：<br /><br /> 提高计划优先级 (SeIncreaseBasePriorityPrivilege)| ||[!INCLUDE[ssRS](../../includes/ssrs.md)] ：<br /><br /> （所有权限都将授予 Per-service SID。 默认实例：NT SERVICE\ReportServer。 命名实例：**NT SERVICE\\ReportServer$** _InstanceName_.)| (SeServiceLogonRight)| |**作为服务登录[!INCLUDE[ssIS](../../includes/ssis-md.md)]:**<br /><br /> （所有权限都将授予 Per-service SID。 默认实例和命名实例：NT SERVICE\MsDtsServer130。 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 没有针对命名实例的单独进程。)|**作为服务登录** (SeServiceLogonRight)<br /><br /> 应用程序事件日志的写入权限。<br /><br /> **跳过遍历检查** (SeChangeNotifyPrivilege)<br /><br /> 身份验证后模拟客户端 (SeImpersonatePrivilege)| |全文搜索 ：<br /><br /> （所有权限都将授予 Per-service SID。 默认实例：NT Service\MSSQLFDLauncher。 命名实例：**NT Service\ MSSQLFDLauncher$** _InstanceName_.)|**作为服务登录** (SeServiceLogonRight)<br /><br /> **调整进程的内存配额** (SeIncreaseQuotaPrivilege)<br /><br /> **跳过遍历检查** (SeChangeNotifyPrivilege)| |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器：<br /><br /> （所有权限都授予本地 Windows 组。 默认实例或命名实例：SQLServer2005SQLBrowserUser$ComputerName。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 浏览器没有针对命名实例的单独进程。)|**作为服务登录** (SeServiceLogonRight)| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS Writer:**<br /><br /> （所有权限都将授予 Per-service SID。 默认实例或命名实例：NT Service\SQLWriter。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS 编写器没有针对已命名实例的单独的进程。）SQLWriter 服务在具有所需的所有权限的 LOCAL SYSTEM 帐户下运行。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 设置不为此服务检查或授予权限。| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式播放控制器:** |**作为服务登录** (SeServiceLogonRight)| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分布式播放客户端:** |**作为服务登录** (SeServiceLogonRight)| |**PolyBase 引擎和 DMS**| **作为服务登录** (SeServiceLogonRight)| |**Launchpad:** |**作为服务登录** (SeServiceLogonRight) <br /><br /> **替换进程级别标记** (SeAssignPrimaryTokenPrivilege)<br /><br />**跳过遍历检查** (SeChangeNotifyPrivilege)<br /><br />调整进程的内存配额 (SeIncreaseQuotaPrivilege)| |R 服务/机器学习服务 ：**SQLRUserGroup** (SQL 2016 and 2017) |默认没有“**允许本地登录**”权限 | |**机器学习服务** '**所有应用程序包' [AppContainer]** (SQL 2019) |对 SQL Server 'Binn'、R_Services 和 PYTHON_Services 目录的 **读取和执行权限** |

 \*[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的实例上处于禁用状态。

### <a name="file-system-permissions-granted-to-sql-server-per-service-sids-or-local-windows-groups"></a><a name="Reviewing_ACLs"></a> 授予 SQL Server Per-service SID 或本地 Windows 组的文件系统权限

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须具有对资源的访问权限。 为 Per-service SID 或本地 Windows 组设置了访问控制列表。

> [!IMPORTANT]
> 对于故障转移群集安装，必须为本地帐户的 ACL 设置共享磁盘上的资源。

下表显示了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序设置的 ACL：

|服务帐户针对|文件和文件夹|访问|
|-------------------------|-----------------------|------------|
|MSSQLServer|Instid\MSSQL\backup|完全控制|
||Instid\MSSQL\binn|读取和执行|
||Instid\MSSQL\data|完全控制|
||Instid\MSSQL\FTData|完全控制|
||Instid\MSSQL\Install|读取和执行|
||Instid\MSSQL\Log|完全控制|
||Instid\MSSQL\Repldata|完全控制|
||130\shared|读取和执行|
||Instid\MSSQL\Template Data（仅限[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] ）|读取|
|SQLServerAgent\*|Instid\MSSQL\binn|完全控制|
||Instid\MSSQL\Log|读取、写入、删除和执行|
||130\com|读取和执行|
||130\shared|读取和执行|
||130\shared\Errordumps|读取和写入|
||ServerName\EventLog|完全控制|
|FTS|Instid\MSSQL\FTData|完全控制|
||Instid\MSSQL\FTRef|读取和执行|
||130\shared|读取和执行|
||130\shared\Errordumps|读取和写入|
||Instid\MSSQL\Install|读取和执行|
||Instid\MSSQL\jobs|读取和写入|
|MSSQLServerOLAPService|130\shared\ASConfig|完全控制|
||Instid\OLAP|读取和执行|
||Instid\Olap\Data|完全控制|
||Instid\Olap\Log|读取和写入|
||Instid\OLAP\Backup|读取和写入|
||Instid\OLAP\Temp|读取和写入|
||130\shared\Errordumps|读取和写入|
|ReportServer|Instid\Reporting Services\Log Files|读取、写入、删除|
||Instid\Reporting Services\ReportServer|读取和执行|
||Instid\Reporting Services\ReportServer\global.asax|完全控制|
||Instid\Reporting Services\ReportServer\rsreportserver.config|读取|
||Instid\Reporting Services\RSTempfiles|读取、写入、执行、删除|
||Instid\Reporting Services\RSWebApp|读取和执行|
||130\shared|读取和执行|
||130\shared\Errordumps|读取和写入|
|MSDTSServer100|130\dts\binn\MsDtsSrvr.ini.xml|读取|
||130\dts\binn|读取和执行|
||130\shared|读取和执行|
||130\shared\Errordumps|读取和写入|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|130\shared\ASConfig|读取|
||130\shared|读取和执行|
||130\shared\Errordumps|读取和写入|
|SQLWriter|不适用（以 Local System 身份运行）||
|用户|Instid\MSSQL\binn|读取和执行|
||Instid\Reporting Services\ReportServer|读取、执行和列出文件夹内容|
||Instid\Reporting Services\ReportServer\global.asax|读取|
||Instid\Reporting Services\RSWebApp|读取、执行和列出文件夹内容|
||130\dts|读取和执行|
||130\tools|读取和执行|
||100\tools|读取和执行|
||90\tools|读取和执行|
||80\tools|读取和执行|
||130\sdk|读取|
||Microsoft SQL Server\130\Setup Bootstrap|读取和执行|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 控制器|\<ToolsDir>\DReplayController\Log\（空目录）|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayController\DReplayController.exe|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayController\resources\|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayController\\{所有 DLL}|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayController\DReplayController.config|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayController\IRTemplate.tdf|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayController\IRDefinition.xml|读取、执行和列出文件夹内容|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay 客户端|\<ToolsDir>\DReplayClient\Log\|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayClient\DReplayClient.exe|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayClient\resources\|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayClient\（所有 DLL）|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayClient\DReplayClient.config|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayClient\IRTemplate.tdf|读取、执行和列出文件夹内容|
||\<ToolsDir>\DReplayClient\IRDefinition.xml|读取、执行和列出文件夹内容|
|启动板|%binn|读取和执行|
||ExtensiblilityData|完全控制|
||Log\ExtensibilityLog|完全控制|

\*[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例和具有高级服务的 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 实例上处于禁用状态。

当数据库文件存储在用户定义的位置时，您必须授予每个服务 SID 访问该位置的权限。 有关将文件系统权限授予 Per-service SID 的详细信息，请参阅 [配置数据库引擎访问的文件系统权限](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md)。

### <a name="file-system-permissions-granted-to-other-windows-user-accounts-or-groups"></a><a name="File_System_Other"></a> 授予其他 Windows 用户帐户或组的文件系统权限

可能还必须向内置帐户或其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户授予某些访问控制权限。 下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序设置的其他 ACL。

|请求组件|帐户|资源|权限|
|--------------------------|-------------|--------------|-----------------|
|MSSQLServer|性能日志用户|Instid\MSSQL\binn|列出文件夹内容|
||性能监视器用户|Instid\MSSQL\binn|列出文件夹内容|
||性能日志用户、性能监视器用户|\WINNT\system32\sqlctr130.dll|读取和执行|
||仅限于管理员|\\\\.\root\Microsoft\SqlServer\ServerEvents\\<sql_instance_name>\*|完全控制|
||管理员和系统|\tools\binn\schemas\sqlserver\2004\07\showplan|完全控制|
||用户|\tools\binn\schemas\sqlserver\2004\07\showplan|读取和执行|
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|报表服务器 Windows 服务帐户|*\<install>* \Reporting Services\LogFiles|DELETE<br /><br /> READ_CONTROL<br /><br /> SYNCHRONIZE<br /><br /> FILE_GENERIC_READ<br /><br /> FILE_GENERIC_WRITE<br /><br /> FILE_READ_DATA<br /><br /> FILE_WRITE_DATA<br /><br /> FILE_APPEND_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_WRITE_EA<br /><br /> FILE_READ_ATTRIBUTES<br /><br /> FILE_WRITE_ATTRIBUTES|
||报表服务器 Windows 服务帐户|*\<install>* \Reporting Services\ReportServer|读取|
||报表服务器 Windows 服务帐户|*\<install>* \Reporting Services\ReportServer\global.asax|完全|
||报表服务器 Windows 服务帐户|*\<install>* \Reporting Services\RSWebApp|读取和执行|
||所有人|*\<install>* \Reporting Services\ReportServer\global.asax|READ_CONTROL<br /><br /> FILE_READ_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_READ_ATTRIBUTES|
||ReportServer Windows 服务帐户|*\<install>* \Reporting Services\ReportServer\rsreportserver.config|DELETE<br /><br /> READ_CONTROL<br /><br /> SYNCHRONIZE<br /><br /> FILE_GENERIC_READ<br /><br /> FILE_GENERIC_WRITE<br /><br /> FILE_READ_DATA<br /><br /> FILE_WRITE_DATA<br /><br /> FILE_APPEND_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_WRITE_EA<br /><br /> FILE_READ_ATTRIBUTES<br /><br /> FILE_WRITE_ATTRIBUTES|
||所有人|报表服务器密钥（Instid 配置单元）|查询值<br /><br /> 枚举子项<br /><br /> 通知<br /><br /> 读取控制|
||终端服务用户|报表服务器密钥（Instid 配置单元）|查询值<br /><br /> 设置值<br /><br /> 创建子项<br /><br /> 枚举子项<br /><br /> 通知<br /><br /> 删除<br /><br /> 读取控制|
||超级用户|报表服务器密钥（Instid 配置单元）|查询值<br /><br /> 设置值<br /><br /> 创建子项<br /><br /> 枚举子项<br /><br /> 通知<br /><br /> 删除<br /><br /> 读取控制|

\*这是 WMI 提供程序命名空间。

### <a name="file-system-permissions-related-to-unusual-disk-locations"></a><a name="Unusual_Locations"></a> 与非寻常磁盘位置相关的文件系统权限

默认的作为安装位置的驱动器为 systemdrive，通常是驱动器 C。本部分介绍当将 tempdb 或用户数据库安装到非常用位置时要考虑的注意事项。

**非默认的驱动器**

当安装到不是默认驱动器的本地驱动器时，Per-service SID 必须对文件位置具有访问权限。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将预配所需的访问权限。

**网络共享**

当数据库安装到网络共享时，服务帐户必须对用户数据库和 tempdb 数据库的文件位置具有访问权限。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序无法设置对于网络共享的访问权限。 用户必须为服务帐户设置对 tempdb 位置的访问权限，然后才能运行安装程序。 用户必须设置对用户数据库位置的访问权限，然后才能创建数据库。

> [!NOTE]
> 虚拟帐户无法通过身份验证，因而无法访问远程位置。 所有虚拟帐户均使用计算机帐户的权限。 以 _<domain_name>_ **\\** _<computer_name>_ **$** 格式设置计算机帐户。

### <a name="reviewing-additional-considerations"></a><a name="Review_additional_considerations"></a>查看其他注意事项

下表显示了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务提供其他功能时所需的权限：

|服务/应用程序|功能|必需的权限|
|--------------------------|-------------------|-------------------------|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)|使用 xp_sendmail 写入邮件槽。|网络写入权限。|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)|运行用户的而不是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员的 xp_cmdshell。|充当操作系统的一部分以及替换进程级别标记。|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理 (MSSQLSERVER)|使用自动重新启动功能。|必须是本地 Administrators 组的成员。|
|[!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问|优化数据库以获得最佳查询性能。|第一次使用时，拥有系统管理员权限的用户必须初始化该应用程序。 初始化后，dbo 用户可使用 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 优化顾问仅优化他们拥有的那些表。 有关详细信息，请参阅 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 联机丛书中的“在第一次使用时初始化 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 优化顾问”。|

> [!IMPORTANT]
> 升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前，请先启用 SQL Server 代理，并验证所需的默认配置，即验证 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务帐户是否为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sysadmin 固定服务器角色的成员。

### <a name="registry-permissions"></a><a name="Registry"></a>注册表权限

将在 **HKLM\Software\Microsoft\Microsoft SQL Server\\** _<Instance_ID>_ 下为识别实例的组件创建注册表配置单元。 例如：

- **HKLM\Software\Microsoft\Microsoft SQL Server\MSSQL13.MyInstance**
- **HKLM\Software\Microsoft\Microsoft SQL Server\MSASSQL13.MyInstance**
- **HKLM\Software\Microsoft\Microsoft SQL Server\MSSQL.130**

注册表还维护实例 ID 到实例名的映射。 实例 ID 到实例名的映射按如下方式维护：

- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\SQL] "InstanceName"="MSSQL13"**
- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\OLAP] "InstanceName"="MSASSQL13"**
- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\RS] "InstanceName"="MSRSSQL13"**

### <a name="wmi"></a><a name="WMI"></a> WMI

Windows Management Instrumentation (WMI) 必须能够连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。 为了支持这一点，需在 **中预配 Windows WMI 提供程序 (** NT SERVICE\winmgmt [!INCLUDE[ssDE](../../includes/ssde-md.md)]) 的 per-service SID。

SQL WMI 提供程序需要以下最低权限：

- msdb 数据库的 **db_ddladmin** 或 **db_owner** 固定服务器角色中的成员资格。
- 服务器中的 **CREATE DDL EVENT NOTIFICATION** 权限。
- **中的** CREATE TRACE EVENT NOTIFICATION [!INCLUDE[ssDE](../../includes/ssde-md.md)]权限。
- **VIEW ANY DATABASE** 服务器级别权限。

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序会创建一个 SQL WMI 命名空间，并向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务 SID 授予读取权限。

### <a name="named-pipes"></a><a name="Pipes"></a> 命名管道

在所有安装中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序都通过共享内存协议（这是一种本地命名管道）提供针对 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的访问权限。

## <a name="provisioning"></a><a name="Provisioning"></a> 预配

此部分介绍如何在各种 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件内设置帐户。

- [数据库引擎预配](#DE_Prov)

  - [Windows 主体](#Win_Principals)
  - [sa 帐户](#sa)
  - [SQL Server Per-service SID 登录名和特权](#Logins)
  - [SQL Server 代理登录名和特权](#Agent)
  - [HADRON 和 SQL 故障转移群集实例和特权](#Hadron)
  - [SQL 编写器和特权](#Writer)
  - [SQL WMI 和特权](#SQLWMI)
- [SSAS 预配](#SSAS)
- [SSRS 预配](#SSRS)

### <a name="database-engine-provisioning"></a><a name="DE_Prov"></a> 数据库引擎预配

以下帐户将作为登录名添加到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]中。

#### <a name="windows-principals"></a><a name="Win_Principals"></a> Windows 主体

在安装过程中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序要求至少将一个用户帐户命名为 **sysadmin** 固定服务器角色的成员。

#### <a name="sa-account"></a><a name="sa"></a> sa 帐户

**sa** 帐户始终作为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 登录名存在，它是 **sysadmin** 固定服务器角色的成员。 当仅使用 Windows 身份验证安装[!INCLUDE[ssDE](../../includes/ssde-md.md)]时（也即，当未启用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证时），sa 登录名仍然存在，但处于禁用状态，并且该帐户的密码非常复杂且是随机的。 有关启用 **sa** 帐户的信息，请参阅 [更改服务器身份验证模式](../../database-engine/configure-windows/change-server-authentication-mode.md)。

#### <a name="sql-server-per-service-sid-login-and-privileges"></a><a name="Logins"></a> SQL Server Per-service SID 登录名和特权

将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务的每个服务 SID（有时也称为服务安全主体 (SID)）预配为[!INCLUDE[ssDE](../../includes/ssde-md.md)]登录名。 Per-service SID 登录名是 **sysadmin** 固定服务器角色的成员。 有关每个服务 SID 的信息，请参阅[使用服务 SID 授予对 SQL Server 中服务的访问权限](../../relational-databases/security/using-service-sids-to-grant-permissions-to-services-in-sql-server.md)。

#### <a name="sql-server-agent-login-and-privileges"></a><a name="Agent"></a> SQL Server 代理登录名和特权

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务的 Per-service SID 设置为一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 登录名。 Per-service SID 登录名是 **sysadmin** 固定服务器角色的成员。

#### <a name="sshadrc-and-sql-failover-cluster-instance-and-privileges"></a><a name="Hadron"></a> [!INCLUDE[ssHADRc](../../includes/sshadrc-md.md)] 和 SQL 故障转移群集实例和特权

当将 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 安装为 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] 或 SQL 故障转移群集实例 (SQL FCI) 时，将在 **中设置** LOCAL SYSTEM [!INCLUDE[ssDE](../../includes/ssde-md.md)]。 **LOCAL SYSTEM** 登录名被授予 **ALTER ANY AVAILABILITY GROUP** 权限（对于 [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]）以及 **VIEW SERVER STATE** 权限（对于 SQL FCI）。

#### <a name="sql-writer-and-privileges"></a><a name="Writer"></a> SQL 编写器和特权

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] VSS 编写器服务的 Per-service SID 设置为一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 登录名。 Per-service SID 登录名是 **sysadmin** 固定服务器角色的成员。

#### <a name="sql-wmi-and-privileges"></a><a name="SQLWMI"></a> SQL WMI 和特权

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将 **NT SERVICE\Winmgmt** 帐户设置为 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 登录名，并将其添加到 **sysadmin** 固定服务器角色中。

#### <a name="ssrs-provisioning"></a>SSRS 预配

在安装过程中指定的帐户将设置为 **RSExecRole** 数据库角色的成员。 有关详细信息，请参阅 [配置报表服务器服务帐户（SSRS 配置管理器）](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)。

### <a name="ssas-provisioning"></a><a name="SSAS"></a>SSAS 预配

[!INCLUDE[ssAS](../../includes/ssas-md.md)] 服务帐户要求各不相同，具体取决于服务器的部署方式。 如果您正在安装 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序会要求您将 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 服务配置在域帐户下运行。 为了支持 SharePoint 中内置的托管帐户功能，需要域帐户。 为此， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序没有为 [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 安装提供默认服务帐户，如虚拟帐户。 有关设置 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] for SharePoint 的详细信息，请参阅 [配置 Power Pivot 服务帐户](/analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts)。

对于所有其他独立 [!INCLUDE[ssAS](../../includes/ssas-md.md)] 安装，您可以将服务设置为在域帐户、内置系统帐户、托管帐户或虚拟帐户下运行。 有关帐户设置的详细信息，请参阅[配置服务帐户 (Analysis Services)](/analysis-services/instances/configure-service-accounts-analysis-services)。

对于群集安装，您必须指定一个域帐户或一个内置系统帐户。 [!INCLUDE[ssAS](../../includes/ssas-md.md)] 故障转移群集既不支持托管帐户，也不支持虚拟帐户。

所有 [!INCLUDE[ssAS](../../includes/ssas-md.md)] 安装均要求您指定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 实例的系统管理员。 管理员特权在 Analysis Services 的 **“服务器”** 角色中设置。

### <a name="ssrs-provisioning"></a><a name="SSRS"></a> SSRS 预配

在安装过程中指定的帐户将在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中设置为 **RSExecRole** 数据库角色的成员。 有关详细信息，请参阅 [配置报表服务器服务帐户（SSRS 配置管理器）](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)。

## <a name="upgrading-from-previous-versions"></a><a name="Upgrade"></a>从早期版本升级

此部分介绍在从先前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]升级的过程中进行的更改。

- [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 要求 [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 SP1、Windows Server 2012、Windows 8.0、Windows Server 2012 R2 或 Windows 8.1。 任何在更低操作系统版本上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 先前版本在升级 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]之前，都必须升级操作系统。
- 在将 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 升级到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]的过程中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将按以下方式配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。

  - [!INCLUDE[ssDE](../../includes/ssde-md.md)] 使用 Per-service SID 的安全上下文运行。 将向 Per-service SID 授予针对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的文件夹（如 DATA）和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 注册表项的访问权限。
  - [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的 Per-service SID 在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 中设置为 **sysadmin** 固定服务器角色的成员。
  - Per-service SID 将添加到本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 组中，除非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 是故障转移群集实例。
  - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 资源保持设置为本地 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Windows 组。
  - 服务的本地 Windows 组已从 **SQLServer2005MSSQLUser$** _<computer_name>_ **$** _<instance_name>_ 重命名为 **SQLServerMSSQLUser$** _<computer_name>_ **$** _<instance_name>_ 。 已迁移的数据库的文件位置将具有本地 Windows 组的访问控制项 (ACE)。 新数据库的文件位置将具有 Per-service SID 的 ACE。

- 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 进行升级的过程中，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将保留 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Per-service SID 的 ACE。
- 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 故障转移群集实例，将保留为服务配置的域帐户的 ACE。

## <a name="appendix"></a><a name="Appendix"></a> 附录

本节包含有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的其他信息。

- [服务帐户的描述](#Serv_Accts)
- [辨别识别实例的服务和不识别实例的服务](#Identify_instance_aware_and_unaware)
- [本地化的服务名称](#Localized_service_names)

### <a name="description-of-service-accounts"></a><a name="Serv_Accts"></a> 服务帐户的描述

服务帐户是用来启动 Windows 服务的帐户，如 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]。 对于正在运行的 SQL Server，除了服务SID（始终存在并且是 sysamin 固定服务器角色的成员）之外，无需将服务帐户作为登录名添加到 SQL Server。

#### <a name="accounts-available-with-any-operating-system"></a><a name="Any_OS"></a> 可用于任何操作系统的帐户

除了前面介绍的新的 [MSA](#MSA)、[gMSA](#GMSA) 和[虚拟帐户](#VA_Desc)之外，还可以使用以下帐户。

<a name="Domain_User"></a> 域用户帐户

如果服务必须与网络服务进行交互，则访问类似于文件共享的域资源；如果服务使用到运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的其他计算机的链接服务器连接，则可以使用具有最低特权的域帐户。 许多服务器到服务器的活动只能使用域用户帐户来执行。 此帐户应由域管理员在您的环境内预先创建。

> [!NOTE]
> 如果将 SQL Server 配置为使用域帐户，则可以隔离服务的特权，但必须手动管理密码或创建自定义解决方案来管理这些密码。 许多服务器应用程序使用此策略以增强安全性，但此策略要求额外的管理和复杂性。 在这些部署中，服务管理员将在维护任务方面花费大量时间，如管理服务密码和服务主体名称 (SPN)，但这些维护服务是 Kerberos 身份验证所必需的。 此外，这些维护任务可能会中断服务。

<a name="Local_User"></a> 本地用户帐户

如果计算机不在域中，则建议您使用不具有 Windows 管理员权限的本地用户帐户。

<a name="Local_Service"></a> 本地服务帐户

Local Service 帐户是一个内置帐户，与 Users 组的成员具有相同级别的资源和对象访问权限。 如果有个别服务或进程的安全性受到威胁，则此有限访问权限有助于保护系统的安全性。 以 Local Service 帐户身份运行的服务将以一个没有凭据的 Null 会话形式访问网络资源。 
> [!NOTE]
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理服务不支持本地服务帐户。 不支持将 Local Service 作为运行这些服务的帐户，因为该服务是一个共享服务，并且任何在 Local Service 下运行的其他服务都对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]具有系统管理员访问权限。
该帐户的实际名称为 **NT AUTHORITY\LOCAL SERVICE**。

<a name="Network_Service"></a> 网络服务帐户

Network Service 帐户是一个内置帐户，比 Users 组的成员拥有更多的对资源和对象的访问权限。 以网络服务帐户身份运行的服务通过使用计算机帐户的凭据（格式为 _<domain_name>_ **\\** _<computer_name>_ **$** ）访问网络资源。 该帐户的实际名称为 **NT AUTHORITY\NETWORK SERVICE**。

<a name="Local_System"></a> 本地系统帐户

Local System 是一个具有高特权的内置帐户。 它对本地系统拥有许多特权并作为网络上的计算机。 该帐户的实际名称为 **NT AUTHORITY\SYSTEM**。

### <a name="identifying-instance-aware-and-instance-unaware-services"></a><a name="Identify_instance_aware_and_unaware"></a> 辨别识别实例的服务和不识别实例的服务

识别实例的服务与特定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例相关联，并具有自己的注册表配置单元。 通过为每个组件或服务运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序，可以安装识别实例的服务的多个副本。 不识别实例的服务由所有已安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例共享。 它们不与特定实例相关联，仅安装一次且不能并行安装。

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中识别实例的服务包括：

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理

   请注意，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例和 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] with Advanced Services 实例上 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 代理服务已禁用。

- [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]\*
- [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
- 全文搜索

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中不识别实例的服务包括：

- [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser
- SQL 编写器

 \*SharePoint 集成模式下的 Analysis Services 作为单个命名实例以“[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]”身份运行。 实例名称是固定不变的。 您不能指定其他名称。 你只能在每个物理服务器上安装一个作为“[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]”运行的 Analysis Services 实例。

### <a name="localized-service-names"></a><a name="Localized_service_names"></a> 本地化的服务名称

下表列出了 Windows 的本地化版本所显示的服务名称。

|语言|Local Service 的名称|Network Service 的名称|Local System 的名称|Admin Group 的名称|
|--------------|----------------------------|------------------------------|---------------------------|--------------------------|
|英语<br /><br /> 简体中文<br /><br /> 繁体中文<br /><br /> 韩语<br /><br /> 日语|NT AUTHORITY\LOCAL SERVICE|NT AUTHORITY\NETWORK SERVICE|NT AUTHORITY\SYSTEM|BUILTIN\Administrators|
|德语|NT-AUTORITÄT\LOKALER DIENST|NT-AUTORITÄT\NETZWERKDIENST|NT-AUTORITÄT\SYSTEM|VORDEFINIERT\Administratoren|
|法语|AUTORITE NT\SERVICE LOCAL|AUTORITE NT\SERVICE RÉAU|AUTORITE NT\SYSTEM|BUILTIN\Administrators|
|意大利语|NT AUTHORITY\SERVIZIO LOCALE|NT AUTHORITY\SERVIZIO DI RETE|NT AUTHORITY\SYSTEM|BUILTIN\Administrators|
|西班牙语|NT AUTHORITY\SERVICIO LOC|NT AUTHORITY\SERVICIO DE RED|NT AUTHORITY\SYSTEM|BUILTIN\Administradores|
|俄语|NT AUTHORITY\LOCAL SERVICE|NT AUTHORITY\NETWORK SERVICE|NT AUTHORITY\СИСТЕМА|BUILTIN\Администраторы|

## <a name="related-content"></a>相关内容

[安装 SQL Server 的安全注意事项](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)

[SQL Server 的默认实例和命名实例的文件位置](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)

[安装 Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)
