---
title: 配置 Windows 防火墙
description: 了解如何将 Windows 防火墙配置为允许通过防火墙访问 SQL Server 的实例。
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- Windows Firewall ports
- WMI firewall ports
- Windows Firewall [Database Engine]
- firewall systems, configuring
- advfirewall
- firewall systems
- rules firewall
- firewall systems, overview and port list
- 1433 TCP port
- portopening using netsh
- ports [SQL Server], TCP
- netsh to open firewall ports
ms.assetid: f55c6a0e-b6bd-4803-b51a-f3a419803024
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7a00c8196a3a20d17f07c215b8d0d0f1c3ffd84e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352455"
---
# <a name="configure-the-windows-firewall-to-allow-sql-server-access"></a>Configure the Windows Firewall to Allow SQL Server Access
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

防火墙系统有助于阻止对计算机资源进行未经授权的访问。 如果防火墙已打开但却未正确配置，则可能会阻止连接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
若要通过防火墙访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例，必须在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 防火墙是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 的一个组件。 也可以安装其他公司的防火墙。 本文讨论如何配置 Windows 防火墙，不过所述基本原理也适用于其他防火墙程序。  
  
> [!NOTE]  
>  本文概述了防火墙配置并汇总了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理员所需的信息。 有关防火墙的详细信息以及权威防火墙信息，请参阅 [Windows 防火墙安全部署指南](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide)等防火墙文档。  
  
 熟悉如何管理 Windows 防火墙的用户，以及对想要配置的防火墙设置有所了解的用户，可直接转而参阅更高级的文章  ：  
  
-   [为数据库引擎访问配置 Windows 防火墙](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)    
-   [配置 Windows 防火墙以允许 Analysis Services 访问](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)    
-   [将防火墙配置为允许报表服务器访问](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
  
##  <a name="basic-firewall-information"></a><a name="BKMK_basic"></a> 基本防火墙信息  
 防火墙的工作原理是检查传入的数据包并将其与一组规则进行比较。 如果数据包符合规则所规定的标准，则防火墙会将该数据包传递给 TCP/IP 协议进行其他处理。 如果数据包不符合规则指定的标准，则防火墙将丢弃该数据包，此外，如果已启用日志记录，防火墙还将在防火墙日志文件中创建一个条目。  
  
 允许的通信的列表采用以下某种方式进行填充：  

- **自动**：启用了防火墙的计算机开始通信时，防火墙会在列表中创建一个条目以便允许响应。 此响应被视为为已请求流量，无需进行任何配置。 
  
-   **手动**：管理员可配置防火墙例外。 这样就可访问计算机上的指定程序或端口。 在此情况下，当计算机充当服务器、侦听器或对等方时，将会接受未经请求的传入通信。 必须完成此类配置才能连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
 选择防火墙策略远较只是确定应打开还是关闭给定端口复杂。 当为企业设计防火墙策略时，请确保考虑所有可供使用的规则和配置选项。 本文并未完整讨论所有可能的防火墙选项。 建议您查看以下文档：  
  
 [Windows 防火墙部署指南](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide)    
 [Windows 防火墙设计指南](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-design-guide)    
 [Introduction to Server and Domain Isolation（服务器和域隔离简介）](/windows/security/threat-protection/windows-firewall/domain-isolation-policy-design)  
  
##  <a name="default-firewall-settings"></a><a name="BKMK_default"></a> 默认防火墙设置  
 规划防火墙配置的第一步是确定操作系统的防火墙的当前状态。 如果操作系统是从早期版本升级而来，则可能已保留以前的防火墙设置。 此外，防火墙设置可能已由其他管理员或域中的组策略更改。  
  
> [!NOTE]  
>  打开防火墙将影响访问此计算机的其他程序，例如文件和打印共享以及远程桌面连接。 在调整防火墙设置之前，管理员应对计算机上运行的所有应用程序加以考虑。  
  
##  <a name="programs-to-configure-the-firewall"></a><a name="BKMK_programs"></a> 用于配置防火墙的程序  
通过 **Microsoft 管理控制台** 或 **netsh** 配置 Windows 防火墙设置。  

-  **Microsoft 管理控制台 (MMC)**  
  
     使用高级安全 Windows 防火墙 MMC 管理单元可以配置更高级的防火墙设置。 此管理单元以一种易于使用的方式呈现大多数防火墙选项，并且会显示所有防火墙配置文件。 有关详细信息，请参阅本文后面的[使用高级安全 Windows 防火墙管理单元](#BKMK_WF_msc)。  
  
-   **netsh**  
  
     管理员可以在命令提示符下使用 **netsh.exe** 工具配置和监视基于 Windows 的计算机，也可以使用批处理文件执行此操作 **。** 通过使用 **netsh** 工具，可以将输入的上下文命令定向到相应帮助器，然后由帮助器执行此命令。 帮助器是一个动态链接库 (.dll) 文件，它通过对一种或多种服务、实用工具或协议提供配置、监视和支持来扩展 **netsh** 工具的功能。 所有支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的操作系统都具有防火墙帮助器。 [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 也具有称作 **advfirewall** 的高级防火墙帮助器。 本文不讨论有关使用 **netsh** 的详细信息。 不过，所述配置选项中的许多选项都可以通过使用 **netsh** 加以配置。 例如，在命令提示符下运行以下脚本，以打开 TCP 端口 1433：  
  
    ```  
    netsh firewall set portopening protocol = TCP port = 1433 name = SQLPort mode = ENABLE scope = SUBNET profile = CURRENT  
    ```  
  
     使用高级安全 Windows 防火墙帮助器的一个类似示例：  
  
    ```  
    netsh advfirewall firewall add rule name = SQLPort dir = in protocol = tcp action = allow localport = 1433 remoteip = localsubnet profile = DOMAIN  
    ```  
  
     有关 **netsh** 的详细信息，请参阅以下链接：  
  
    -   [Netsh 命令语法、上下文和格式](/windows-server/networking/technologies/netsh/netsh-contexts)    
    -   [如何使用“netsh advfirewall firewall”上下文而非“netsh firewall”上下文控制 Windows Server 2008 和 Windows Vista 中的 Windows 防火墙行为](https://support.microsoft.com/kb/947709)    

    
- **对于 Linux**：在 Linux 上，还需要打开与需要访问的服务关联的端口。 不同的 Linux 分发和不同的防火墙具有各自的过程。 如需了解两个示例，请参阅 [Red Hat 上的 SQL Server](../../linux/quickstart-install-connect-red-hat.md) 和 [SUSE 上的 SQL Server](../../linux/quickstart-install-connect-suse.md)。 
  
## <a name="ports-used-by-ssnoversion"></a>使用的端口 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 下面几个表可有助于您确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]所使用的端口。  
  
###  <a name="ports-used-by-the-database-engine"></a><a name="BKMK_ssde"></a> Ports Used By the Database Engine  
 

默认情况下，SQL Server 使用的典型端口和关联的数据库引擎服务是：TCP **1433**、**4022**、**135**、**1434**、UDP **1434**。 下表更为详细地介绍了这些端口。 命名实例使用[动态端口](#BKMK_dynamic_ports)。
 
 下表列出了 [!INCLUDE[ssDE](../../includes/ssde-md.md)]经常使用的端口。  
  
|场景|端口|注释|  
|--------------|----------|--------------|  
|通过 TCP 运行的默认实例|TCP 端口 1433|这是允许通过防火墙的最常用端口。 它适用于与默认 [!INCLUDE[ssDE](../../includes/ssde-md.md)]安装或作为计算机上唯一运行实例的命名实例之间的例行连接。 （命名实例具有特殊的注意事项。 请参阅本文后面的 [动态端口](#BKMK_dynamic_ports)。）|  
|具有默认端口的命名实例|此 TCP 端口是在启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 时确定的动态端口。|请参阅下面 [动态端口](#BKMK_dynamic_ports)部分中的描述。 当使用命名实例时， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务可能需要 UDP 端口 1434。|  
| 具有固定端口的命名实例 |由管理员配置的端口号。|请参阅下面 [动态端口](#BKMK_dynamic_ports)部分中的描述。|  
|专用管理连接|对于默认实例，为 TCP 端口 1434。 其他端口用于命名实例。 有关端口号，请查看错误日志。|默认情况下，不会启用与专用管理员连接 (DAC) 的远程连接。 若要启用远程 DAC，请使用外围应用配置器方面。 有关详细信息，请参阅 [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md)。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务|UDP 端口 1434|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务用于侦听指向命名实例的传入连接，并为客户端提供与此命名实例对应的 TCP 端口号。 通常，只要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的命名实例，就会启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Browser 服务。 如果客户端配置为连接到命名实例的特定端口，则不必启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务。|  
|具有 HTTP 终结点的实例。|可以在创建 HTTP 端点时指定。 对于 CLEAR_PORT 通信，默认端口为 TCP 端口 80，对于 SSL_PORT 通信，默认端口为 443。|用于通过 URL 实现的 HTTP 连接。|  
|具有 HTTPS 终结点的默认实例 |TCP 端口 443|用于通过 URL 实现的 HTTPS 连接。 HTTPS 是使用传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）的 HTTP 连接。|  
|[!INCLUDE[ssSB](../../includes/sssb-md.md)]|TCP 端口 4022。 若要验证使用的端口，请执行下面的查询：<br /><br /> `SELECT name, protocol_desc, port, state_desc`<br /><br /> `FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'SERVICE_BROKER'`|对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssSB](../../includes/sssb-md.md)]，没有默认端口，不过这是联机丛书示例中使用的常规配置。|  
|数据库镜像|管理员选择的端口。 若要确定此端口，请执行以下查询：<br /><br /> `SELECT name, protocol_desc, port, state_desc FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'DATABASE_MIRRORING'`|对于数据库镜像，没有默认端口，不过联机丛书示例使用 TCP 端口 5022 或 7022。 请务必不要中断正在使用的镜像终结点，尤其是在处于带有自动故障转移功能的高安全模式下时。 防火墙配置必须避免破坏仲裁。 有关详细信息，请参阅 [指定服务器网络地址（数据库镜像）](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)。|  
|复制|与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的复制连接使用典型的常规 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 端口（供默认实例使用的 TCP 端口 1433 等）<br /><br /> 复制快照的 Web 同步和 FTP/UNC 访问要求在防火墙上打开其他端口。 为了将初始数据和架构从一个位置传输到另一个位置，复制可以使用 FTP（TCP 端口 21）或者通过 HTTP（TCP 端口 80）或文件共享进行的同步。 文件共享使用 UDP 端口 137 和 138，如果使用 NetBIOS，则还有 TCP 端口 139。 文件共享使用 TCP 端口 445。|对于通过 HTTP 进行的同步，复制使用 IIS 端点（其端口可配置，但默认情况下为端口 80），不过 IIS 进程通过标准端口（对于默认实例为 1433）连接到后端 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br /> 在使用 FTP 进行 Web 同步期间，FTP 传输是在 IIS 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 发布服务器之间进行，而非在订阅服务器和 IIS 之间进行。|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] 调试器|TCP 端口 135<br /><br /> 请参阅 [端口 135 的特殊注意事项](#BKMK_port_135)<br /><br /> 可能还需要 [IPsec](#BKMK_IPsec) 例外。|如果使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]，则在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 主机计算机上，还必须将 **Devenv.exe** 添加到“例外”列表中并打开 TCP 端口 135。<br /><br /> 如果使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]，则在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 主机计算机上，还必须将 **ssms.exe** 添加到“例外”列表中并打开 TCP 端口 135。 有关详细信息，请参阅 [运行 TSQL 调试器之前配置防火墙规则](../../ssms/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md)。|  
  
 有关为 [!INCLUDE[ssDE](../../includes/ssde-md.md)]配置 Windows 防火墙的分步说明，请参阅 [为数据库引擎访问配置 Windows 防火墙](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)。  
  
####  <a name="dynamic-ports"></a><a name="BKMK_dynamic_ports"></a> 动态端口  
 默认情况下，命名实例（包括 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]）使用动态端口。 也就是说，每次启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 时，它都将确定一个可用端口并使用此端口号。 如果命名实例是安装的唯一 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则它可能使用 TCP 端口 1433。 如果还安装了其他 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则它可能会使用其他 TCP 端口。 由于所选端口可能会在每次启动 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 时更改，因而很难配置防火墙以启用对正确端口号的访问。 因此，如果使用防火墙，则建议重新配置 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 以每次都使用同一端口号。 这称为固定端口或静态端口。 有关详细信息，请参阅[将服务器配置为侦听特定 TCP 端口（SQL Sever 配置管理器）](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)。  
  
 另一种配置命名实例以侦听固定端口的方法是在防火墙中为诸如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sqlservr.exe **之类的** 程序创建例外（针对 [!INCLUDE[ssDE](../../includes/ssde-md.md)]）。 这会非常方便，但当使用高级安全 Windows 防火墙 MMC 管理单元时，端口号将不会显示在“入站规则”  页的“本地端口”  列中。 这会使审核哪些端口处于打开状态变得更为困难。 另一个注意事项是，Service Pack 或累积更新可能会更改 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可执行文件的路径，这将使防火墙规则作废。  
  
##### <a name="to-add-a-program-exception-to-the-firewall-using-windows-defender-firewall-with-advanced-security"></a>使用高级安全 Windows Defender 防火墙向防火墙添加程序例外
  
1. 从“开始”菜单键入 wf.msc  。 按 Enter 或选择搜索结果 wf.msc 打开“高级安全 Windows Defender 防火墙”。
1. 在左窗格中，选择“入站规则”  。
1. 在右窗格的“操作”下，选择“新建规则...”。“新建入站规则向导”随即打开。
1. 在“规则类型”中，选择“程序”   。 选择“**下一页**”。
1. 在“程序”中，选择“此程序路径”   。 选择“浏览”，找到 SQL Server 实例  。 该程序名为 sqlservr.exe。 通常位于：

   `C:\Program Files\Microsoft SQL Server\MSSQL15.<InstanceName>\MSSQL\Binn\sqlservr.exe`

   选择“**下一页**”。

1. 在“操作”上，选择“允许连接”。 选择“**下一页**”。
1. 在“配置文件”上，包括所有三个配置文件。 选择“**下一页**”。
1. 在“名称”  中键入规则的名称。 选择“完成”  。

有关终结点的详细信息，请参阅[将数据库引擎配置为侦听多个 TCP 端口](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md)和[终结点目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)。 
  
###  <a name="ports-used-by-analysis-services"></a><a name="BKMK_ssas"></a> Analysis Services 使用的端口  
 
默认情况下，SQL Server Analysis Services 使用的典型端口和关联的服务是：TCP **2382**、**2383**、**80**、**443**。 下表更为详细地介绍了这些端口。  
 
 下表列出了 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]经常使用的端口。  
  
|Feature|端口|注释|  
|-------------|----------|--------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|对于默认实例，为 TCP 端口 2383。|默认 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例的标准端口。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务|仅 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 命名实例需要的 TCP 端口 2382|客户端向 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 命名实例发出不指定端口号的连接请求时，该连接请求将被转到端口 2382，即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 侦听的端口。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 将此请求重定向到该命名实例所使用的端口。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 配置为通过 IIS/HTTP 使用<br /><br /> （PivotTable® Service 使用 HTTP 或 HTTPS）|TCP 端口 80|用于通过 URL 实现的 HTTP 连接。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 配置为通过 IIS/HTTPS 使用<br /><br /> （PivotTable® Service 使用 HTTP 或 HTTPS）|TCP 端口 443|用于通过 URL 实现的 HTTPS 连接。 HTTPS 是使用 TLS 的 HTTP 连接。|  
  
 如果用户通过 IIS 和 Internet 访问 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ，则必须打开 IIS 侦听的端口，并在客户端连接字符串中指定该端口。 在这种情况下，不需要打开任何端口就能直接访问 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]。 默认端口 2389 和端口 2382 应当与所有其他并非必需的端口一起受到限制。  
  
 有关为 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]配置 Windows 防火墙的分步说明，请参阅 [将 Windows 防火墙配置为允许 Analysis Services 访问](/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)。  
  
###  <a name="ports-used-by-reporting-services"></a><a name="BKMK_ssrs"></a> Reporting Services 使用的端口  

默认情况下，SQL Server Reporting Services 使用的典型端口和关联的服务是：TCP **80**、**443**。 下表更为详细地介绍了这些端口。 


下表列出了 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]经常使用的端口。  
  
|Feature|端口|注释|  
|-------------|----------|--------------|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 服务|TCP 端口 80|用于通过 URL 实现的与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 之间的 HTTP 连接。 建议不要使用预配置规则 **万维网服务(HTTP)** 。 有关详细信息，请参阅下面的 [与其他防火墙规则的交互](#BKMK_other_rules) 部分。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置为通过 HTTPS 使用|TCP 端口 443|用于通过 URL 实现的 HTTPS 连接。 HTTPS 是使用 TLS 的 HTTP 连接。 建议不要使用预配置规则“安全万维网服务(HTTPS)”  。 有关详细信息，请参阅下面的 [与其他防火墙规则的交互](#BKMK_other_rules) 部分。|  
  
当 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]实例时，还必须为这些服务打开相应的端口。 有关为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]配置 Windows 防火墙的分步说明，请参阅 [将防火墙配置为允许报表服务器访问](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)。  
  
###  <a name="ports-used-by-integration-services"></a><a name="BKMK_ssis"></a> Integration Services 使用的端口  
 下表列出了 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务经常使用的端口。  
  
|Feature|端口|注释|  
|-------------|----------|--------------|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] 远程过程调用 (MS RPC)<br /><br /> 由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 运行时使用。|TCP 端口 135<br /><br /> 请参阅 [端口 135 的特殊注意事项](#BKMK_port_135)|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务在端口 135 上使用 DCOM。 服务控制管理器使用端口 135 执行诸如启动和停止 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务以及将控制请求传送到正在运行的服务等任务。 此端口号无法更改。<br /><br /> 仅当从 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 或自定义应用程序连接到远程 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 服务实例时，才需要打开此端口。|  
  
有关为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 配置 Windows 防火墙的分步说明，请参阅 [Integration Services 服务 (SSIS 服务)](/previous-versions/sql/sql-server-2012/ms137861(v=sql.110))。  
  
###  <a name="additional-ports-and-services"></a><a name="BKMK_additional_ports"></a> 其他端口和服务  
下表列出了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能依赖的一些端口和服务。  
  
|场景|端口|注释|  
|--------------|----------|--------------|  
|Windows Management Instrumentation<br /><br /> 有关 WMI 的详细信息，请参阅 [WMI Provider for Configuration Management Concepts](../../relational-databases/wmi-provider-configuration/wmi-provider-for-configuration-management.md)。|WMI 作为共享服务主机的一部分使用通过 DCOM 分配的端口运行。 WMI 可能使用 TCP 端口 135。<br /><br /> 请参阅 [端口 135 的特殊注意事项](#BKMK_port_135)|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器使用 WMI 列出和管理各个服务。 建议使用预配置规则组 **Windows 管理规范 (WMI)** 。 有关详细信息，请参阅下面的 [与其他防火墙规则的交互](#BKMK_other_rules) 部分。|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC)|TCP 端口 135<br /><br /> 请参阅 [端口 135 的特殊注意事项](#BKMK_port_135)|如果应用程序使用分布式事务处理，可能必须要将防火墙配置为允许 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分布式事务处理协调器 (MS DTC) 在不同的 MS DTC 实例之间以及在 MS DTC 和资源管理器（如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]）之间进行通信。 建议使用预配置的 **“分布式事务处理协调器”** 规则组。<br /><br /> 当在单独的资源组中为整个群集配置单个共享 MS DTC 时，应当将 sqlservr.exe 作为异常添加到防火墙。|  
|[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的浏览按钮使用 UDP 连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务。 有关详细信息，请参阅 [SQL Server Browser 服务（数据库引擎和 SSAS）](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)。|UDP 端口 1434|UDP 是一种无连接协议。<br /><br /> 防火墙具有一个名为 [INetFwProfile 接口的 UnicastResponsesToMulticastBroadcastDisabled 属性](/windows/win32/api/netfw/nf-netfw-inetfwprofile-get_unicastresponsestomulticastbroadcastdisabled) 的设置，用于控制防火墙在对广播（或多播）UDP 请求的单播响应方面的行为。  它有以下两种行为：<br /><br /> 如果此设置为 TRUE，则根本不允许对广播进行任何单播响应。 枚举服务将失败。<br /><br /> 如果此设置为 FALSE（默认值），则允许单播响应 3 秒钟。 此时间长度不可配置。 在堵塞或长时间滞后的网络中，或者对于负载很重的服务器，尝试枚举 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例可能会返回部分列表，这可能会误导用户。|  
|<a name="BKMK_IPsec"></a> IPsec 通信|UDP 端口 500 和 UDP 端口 4500|如果域策略要求通过 IPSec 进行网络通信，还必须将 UDP 端口 4500 和 UDP 端口 500 添加到例外列表。 使用 Windows 防火墙管理单元中的“新建入站规则向导”  可以选择 IPsec。 有关详细信息，请参阅下面的 [使用高级安全 Windows 防火墙管理单元](#BKMK_WF_msc) 。|  
|将 Windows 身份验证用于可信域|必须将防火墙配置为允许身份验证请求。|有关详细信息，请参阅 [如何为域和信任关系配置防火墙](https://support.microsoft.com/kb/179442/)。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows 群集|群集需要与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]不直接相关的其他端口。|有关详细信息，请参阅 [Enable a network for cluster use](/previous-versions/windows/it-pro/windows-server-2003/cc728293(v=ws.10))（启用网络以供群集使用）。|  
|HTTP 服务器 API (HTTP.SYS) 中保留的 URL 命名空间|很可能为 TCP 端口 80，但可以配置为其他端口。 有关常规信息，请参阅 [配置 HTTP 和 HTTPS](/dotnet/framework/wcf/feature-details/configuring-http-and-https)。|有关使用 HttpCfg.exe 预留 HTTP.SYS 端点的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特定信息，请参阅[关于 URL 预留和注册（SSRS 配置管理器）](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md)。|  
  
##  <a name="special-considerations-for-port-135"></a><a name="BKMK_port_135"></a> 端口 135 的特殊注意事项  
 将 RPC 与 TCP/IP 或 UDP/IP 一起用作传输方式时，通常会根据需要为系统服务动态分配入站端口。将使用端口号大于 1024 的 TCP/IP 和 UDP/IP 端口。 这些端口通常被不正式地称为“随机 RPC 端口”。 在这些情况下，RPC 客户端依赖 RPC 端点映射程序来通知它们为服务器分配了哪些动态端口。 对于一些基于 RPC 的服务，可以配置特定端口，而非让 RPC 动态分配一个端口。 此外，还可以将 RPC 动态分配的端口范围限制为一个较小的范围，不管何种服务均可如此。 由于许多服务都使用端口 135，它经常受到恶意用户的攻击。 当打开端口 135 时，请考虑限制防火墙规则的作用范围。  
  
 有关端口 135 的详细信息，请参阅以下参考内容：  
  
-   [Windows Server 系统的服务概述和网络端口要求](https://support.microsoft.com/kb/832017)   
-   [如何排除 RPC 端点映射程序错误](https://mskb.pkisolutions.com/kb/839880)  
-   [Remote procedure call (RPC)（远程过程调用 (RPC)）](/previous-versions/ms950395(v=msdn.10))    
-   [如何配置与防火墙一起使用的 RPC 动态端口分配](https://support.microsoft.com/kb/154596/)  
  
##  <a name="interaction-with-other-firewall-rules"></a><a name="BKMK_other_rules"></a> 与其他防火墙规则的交互  
 Windows 防火墙使用规则和规则组建立其配置。 每个规则或规则组通常与特定程序或服务相关，并且该程序和服务可以在您不知道的情况下修改或删除相应规则。 例如，规则组“万维网服务 (HTTP)”  和“万维网服务 (HTTPS)”  与 IIS 相关。 启用这些规则将打开端口 80 和 443，并且如果启用这些规则，则依赖端口 80 和 443 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能将能正常工作。 不过，配置 IIS 的管理员可能会修改或禁用这些规则。 因此，如果您为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用端口 80 或端口 443，则应创建您自己的规则或规则组，这样可以独立于其他 IIS 规则之外维护您的所需端口配置。  
  
 高级安全 Windows 防火墙 MMC 管理单元允许符合任何适用允许规则的所有通信。 因此，如果有两个均应用于端口 80 的规则（具有不同的参数），则符合任一规则的通信都将得到允许。 因此，如果一个规则允许来自本地子网的通过端口 80 的通信而另一个规则允许来自任意地址的通信，则实际结果是不管通信来源是什么，所有通向端口 80 的通信都将得到允许。 若要有效地管理对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的访问，管理员应定期查看服务器上启用的所有防火墙规则。  
  
##  <a name="overview-of-firewall-profiles"></a><a name="BKMK_profiles"></a> 防火墙配置文件概述  
 操作系统使用防火墙配置文件按照连接性、连接数和类别来识别并记住与它们连接的每个网络。  
  
 在高级安全 Windows 防火墙中有三种网络位置类型：  
  
-   **域**：Windows 可以验证对计算机所联接域的域控制器的访问。
-   **公共**：除域网络之外，其他所有网络最初都归为公共网络一类。 直接连到 Internet 的网络或者位于公共场所（如机场和咖啡店）的网络应保留为公共网络。
-   **专用**：由用户或应用程序标识为专用的网络。 只应将可信网络标识为专用网络。 用户很可能希望将家庭网络或小型企业网络标识为专用网络。  
  
 管理员可以为每种网络位置类型创建一个配置文件，每个配置文件均包含不同的防火墙策略。 在任何时候只能应用一个配置文件。 应用配置文件的顺序如下：  
  
1.  如果要向计算机所属域的域控制器验证所有接口，则应用域配置文件。    
2.  如果要向域控制器验证所有接口或者所有接口均连接到归为专用网络位置一类的网络，则应用专用配置文件。    
3.  否则，应用公共配置文件。  
  
 使用高级安全 Windows 防火墙 MMC 管理单元查看和配置所有防火墙配置文件。 “控制面板”中的 **“Windows 防火墙”** 项仅可配置当前配置文件。  
  
##  <a name="additional-firewall-settings-using-the-windows-firewall-item-in-control-panel"></a><a name="BKMK_additional_settings"></a> 使用“控制面板”中的“Windows 防火墙”项进行其他防火墙设置  
 添加到防火墙的例外可以限制端口仅对来自特定计算机或本地子网的传入连接打开。 这种对端口打开范围的限制可以大大减少计算机对恶意用户的暴露程度，因此建议采用此类限制。  
  
> [!NOTE]  
>  使用控制面板中的“Windows 防火墙”  项仅可配置当前防火墙配置文件。  
  
### <a name="to-change-the-scope-of-a-firewall-exception-using-the-windows-firewall-item-in-control-panel"></a>使用“控制面板”中的“Windows 防火墙”项更改防火墙例外的范围  
  
1.  在“控制面板”中的 **“Windows 防火墙”** 项的 **“例外”** 选项卡上，选择一个程序或端口，然后单击 **“属性”** 或 **“编辑”** 。  
  
2.  在 **“编辑程序”** 或 **“编辑端口”** 对话框中，单击 **“更改范围”** 。  
  
3.  选择以下选项之一：  
  
    -   **任何计算机(包括 Internet 上的计算机)** ：建议不要使用。 这将使任何可寻址到您计算机的计算机都可以连接到指定程序或端口。 如果要允许向 Internet 上的匿名用户呈现信息，则此设置可能是必需的，不过这会增加您对于恶意用户的暴露程度。 如果启用此设置，同时还允许网络地址转换 (NAT) 遍历（例如“允许边缘遍历”选项），则会进一步增加您的暴露程度。  
  
    -   **仅我的网络(子网)** ：这是比 **“任何计算机”** 更安全的设置。 只有网络的本地子网中的计算机可以连接到相应程序或端口。  
  
    -   **自定义列表**：只有具有列表中的 IP 地址的计算机可以连接。 这是比“仅我的网络(子网)”  更安全的设置，不过，使用 DHCP 的客户端计算机有时候会更改它们的 IP 地址。 这样的话，目标计算机将无法连接。 另一台您未打算授权的计算机可能接受这一列出的 IP 地址，从而能够连接。 如果希望列出其他配置为使用固定 IP 地址的服务器，则可能最好选择“自定义列表”选项；不过，入侵者可能会假冒 IP 地址  。 限制防火墙规则的作用大小取决于网络基础结构的优劣。  
  
##  <a name="using-the-windows-firewall-with-advanced-security-snap-in"></a><a name="BKMK_WF_msc"></a> 使用高级安全 Windows 防火墙管理单元  
 可以通过使用高级安全 Windows 防火墙 MMC 管理单元来配置其他高级防火墙设置。 此管理单元包括规则向导，并提供“控制面板”中的“Windows 防火墙”  项中未提供的附加设置。 这些设置包括：  
  
-   加密设置  
-   服务限制   
-   按名称限制计算机的连接    
-   限制连接到特定用户或配置文件  
-   边缘遍历允许通信绕过网络地址转换 (NAT) 路由器    
-   配置出站规则    
-   配置安全规则    
-   传入连接需要 IPsec  
  
### <a name="to-create-a-new-firewall-rule-using-the-new-rule-wizard"></a>使用新建规则向导创建新防火墙规则  
  
1.  在“开始”菜单上，选择“运行”，键入 WF.msc，然后选择“确定”    。    
2.  在“高级安全 Windows 防火墙”的左窗格中，右键单击“入站规则”，然后选择“新建规则”    。   
3.  使用所需设置完成 **“新建入站规则向导”** 。  
  
##  <a name="troubleshooting-firewall-settings"></a><a name="BKMK_troubleshooting"></a> 解决防火墙设置问题  
 以下工具和方法对于解决防火墙问题会非常有用：  
  
-   有效端口状态是与端口相关的所有规则的总体作用结果。 试图禁止通过某一端口访问时，查看引用该端口号的所有规则将会非常有用。 为此，请使用高级安全 Windows 防火墙 MMC 管理单元，并按端口号对入站和出站规则进行排序。  
  
-   查看在运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的计算机上处于活动状态的端口。 此检查过程包括确认正在侦听的 TCP/IP 端口，同时确认这些端口的状态。  
  
     若要验证哪些端口正在侦听，请使用 **netstat** 命令行实用工具。 除了显示活动 TCP 连接以外， **netstat** 实用工具还将显示多种 IP 统计信息和其他信息。  
  
    #### <a name="to-list-which-tcpip-ports-are-listening"></a>列出正在侦听的 TCP/IP 端口  
  
    1.  打开命令提示符窗口。  
  
    2.  在命令提示符下，键入 **netstat -n -a**。  
  
         **-n** 开关指示 **netstat** 以数字方式显示活动 TCP 连接的地址和端口号。 **-a** 开关指示 **netstat** 显示计算机正在侦听的 TCP 和 UDP 端口。  
  
-   **PortQry** 实用工具可用于报告 TCP/IP 端口的状态（正在侦听、未在侦听或已筛选）。 （对于已筛选状态，端口可能正在侦听，也可能未在侦听；此状态指示实用工具没有收到端口的响应。）**PortQry** 实用工具可以从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=17148)下载。  
  
## <a name="see-also"></a>另请参阅  
 [Windows Server 系统的服务概述和网络端口要求](https://support.microsoft.com/kb/832017)   
 [如何：配置防火墙设置（Azure SQL 数据库）](/azure/azure-sql/database/firewall-configure)  
  
