---
title: SQL Server 配置管理器 | Microsoft Docs
description: 利用 SQL Server 配置管理器客户端
ms.custom: ''
ms.date: 12/31/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: vanto
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server], managing
- network protocols [SQL Server], managing
- Client Network Utility
- accounts [SQL Server]
- SQL Server Configuration Manager
- Server Network Utility
- accounts [SQL Server], services
- services [SQL Server], managing
- tools [SQL Server], SQL Server Configuration Manager
- configuration manager [SQL Server]
ms.assetid: e6beaea4-164c-4078-95ae-b9e28b0aefe8
author: rothja
ms.author: jroth
ms.openlocfilehash: fc5fb052e2a02630a78f899e5dcdad615bb00266
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172739"
---
# <a name="sql-server-configuration-manager"></a>SQL Server 配置管理器
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]


  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器是一种工具，用于管理与 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]相关联的服务、配置 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]使用的网络协议以及从 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 客户端计算机管理网络连接配置。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器随 SQL Server 一起安装。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器是一种可以通过“开始”菜单访问的 [!INCLUDE[msCoName](../includes/msconame-md.md)] 管理控制台管理单元，也可以将其添加到任何其他 [!INCLUDE[msCoName](../includes/msconame-md.md)] 管理控制台的显示界面中。 [!INCLUDE[msCoName](../includes/msconame-md.md)]管理控制台 (mmc.exe) 使用 SQLServerManager.msc\<version>.msc 文件（例如 [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] 的 SQLServerManager13.msc）打开配置管理器  。 需要相应的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器版本来管理此特定版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。 以下是在 C 盘上安装 Windows 时的最近五个版本的路径。  
  
|版本|路径|  
|-|-|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2019|C:\Windows\SysWOW64\SQLServerManager15.msc| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|  
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]|C:\Windows\SysWOW64\SQLServerManager12.msc|  
|[!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|C:\Windows\SysWOW64\SQLServerManager11.msc|
  
> [!NOTE]
>  因为 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器是 [!INCLUDE[msCoName](../includes/msconame-md.md)] 管理控制台程序的一个管理单元而不是单独的程序，所以 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器在新版本的 Windows 中不显示为一个应用程序。  
> 
>  -   **Windows 10**：  
>          要打开 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器，请在“起始页” 中键入 SQLServerManager13.msc（适用于 [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]）。 对于其他版本的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]，请将 13 替换为相应的数字。 单击“SQLServerManager13.msc”可打开配置管理器。 要将配置管理器固定到“起始页”或“任务栏”，请右键单击“SQLServerManager13.msc”，然后单击“打开文件位置” 。 在“Windows 文件资源管理器”中，右键单击“SQLServerManager13.msc”，然后单击“固定到‘开始’屏幕”  或“固定到任务栏” 。  
> -   **Windows 8**：  
>          若要打开 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器，请在“搜索”超级按钮中的“应用”下，键入 SQLServerManager\<version>.msc（例如 SQLServerManager13.msc），然后按 Enter    。  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器和 SQL Server Management Studio 使用 Window Management Instrumentation (WMI) 来查看和更改某些服务器设置。 WMI 提供了一种统一的方式，用于与管理 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工具所请求注册表操作的 API 调用进行连接，并可对 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器管理单元组件选定的 SQL 服务提供增强的控制和操作。 有关配置与 WMI 相关的权限的信息，请参阅 [在 SQL Server 工具中将 WMI 配置为显示服务器状态](../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)。  
  
 若要使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器在另一台计算机上启动、停止、暂停、恢复或配置服务，请参阅[连接到另一台计算机（SQL Server 配置管理器）](../database-engine/configure-windows/scm-services-connect-to-another-computer.md)。  
  
## <a name="managing-services"></a>管理服务  
 使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器可以启动、暂停、恢复或停止服务，还可以查看或更改服务属性。  
  
 使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器通过启动参数启动 [!INCLUDE[ssDE](../includes/ssde-md.md)] 。  有关详细信息，请参阅[配置服务器启动选项（SQL Server 配置管理器）](../database-engine/configure-windows/scm-services-configure-server-startup-options.md)。  
  
## <a name="changing-the-accounts-used-by-the-services"></a>更改服务使用的帐户  
 使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器可以管理 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 服务。  
  
> [!IMPORTANT]  
>  始终使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 工具（例如 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器）来更改 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 代理服务使用的帐户，或更改帐户的密码。 除了更改帐户名以外， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器还可以执行其他配置，例如在 Windows 注册表中设置权限，以使新的帐户可以读取 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 设置。 其他工具（例如 Windows 服务控制管理器）可以更改帐户名，但不能更改关联的设置。 如果服务不能访问注册表的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 部分，该服务可能无法正确启动。  
  
 另一个优点是：使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器、SMO 或 WMI 更改的密码无需重新启动服务便可立即生效。  
  
## <a name="manage-server--client-network-protocols"></a>管理服务器和客户端网络协议  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器可以配置服务器和客户端网络协议以及连接选项。 启用正确协议后，通常不需要更改服务器网络连接。 但是，如果您需要重新配置服务器连接，以使 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 侦听特定的网络协议、端口或管道，则可以使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器。 有关启用协议的详细信息，请参阅 [启用或禁用服务器网络协议](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md)。 有关通过防火墙允许访问协议的信息，请参阅 [配置 Windows 防火墙以允许 SQL Server 访问](../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)。  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 使用配置管理器可以管理服务器和客户端网络协议，其中包括强制协议加密、查看别名属性或启用/禁用协议等功能。  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器可以创建或删除别名、更改协议的使用顺序或查看服务器别名的属性，其中包括：  
  
-   服务器别名 — 客户端所连接到的计算机的服务器别名。  
  
-   协议 - 用于配置条目的网络协议。  
  
-   连接参数 - 与用于网络协议配置的连接地址关联的参数。  
  
 虽然某些操作（例如启动和停止服务）应使用群集管理器，但使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器也可以查看有关故障转移群集实例的信息。  
  
### <a name="available-network-protocols"></a>可用的网络协议  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 支持共享内存、TCP/IP 和命名管道协议。 有关选择网络协议的信息，请参阅 [Configure Client Protocols](../database-engine/configure-windows/configure-client-protocols.md)。 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 不支持 VIA、Banyan VINES 顺序包协议 (SPP)、多协议、AppleTalk 或 NWLink IPX/SPX 网络协议。 以前使用这些协议连接的客户端必须选择其他协议才能连接到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]。 不能使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 配置管理器来配置 WinSock 代理。 若要配置 WinSock 代理，请参阅 ISA Server 文档。  
  
## <a name="related-tasks"></a>Related Tasks  
 [管理服务操作指南主题（SQL Server 配置管理器）](../database-engine/configure-windows/scm-services-connect-to-another-computer.md)  
  
 [启动、停止、暂停、继续、重新启动数据库引擎、SQL Server 代理或 SQL Server Browser 服务](../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
 [启动、停止或暂停 SQL Server 代理服务](../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
 [将 SQL Server 实例设置为自动启动（SQL Server 配置管理器）](../database-engine/configure-windows/scm-services-set-an-instance-to-start-automatically.md)  
  
 [防止 SQL Server 实例自动启动（SQL Server 配置管理器）](../database-engine/configure-windows/scm-services-prevent-automatic-startup-of-an-instance.md)  
  
