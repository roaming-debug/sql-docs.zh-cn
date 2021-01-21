---
title: 安装故障转移群集前的准备工作 | Microsoft Docs
description: 本文介绍准备安装 SQL Server 故障转移群集（包括硬件、操作系统和配置）的规划注意事项。
ms.custom: ''
ms.date: 08/24/2016
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], preinstallation checklist
- installing failover clusters
- failover clustering [SQL Server], preinstallation checklist
ms.assetid: a655225d-8c54-4b30-95fd-31f588167899
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2ed61eb89deb29b22f82f39de4091f65be43cea
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595320"
---
# <a name="before-installing-failover-clustering"></a>安装故障转移群集前的准备工作
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  安装 SQL Server 故障转移群集之前，必须选择运行 SQL Server 的硬件和操作系统。 还必须配置 Windows Server 故障转移群集 (WSFC)，检查网络和安全性，并了解将在故障转移群集上运行的其他软件的注意事项。  
  
 如果 Windows 群集具有本地磁盘驱动器，且同一盘符还在一个或多个群集节点上作为共享驱动器使用，则不能在该驱动器上安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 此限制适用于属于 Windows 故障转移群集实例的服务器上的 SQL Server 故障转移群集实例和单独的实例。
  
 您还可能要查看以下主题以便了解与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集概念、功能和任务有关的更多信息。  
  
|主题说明|主题|  
|-----------------------|-----------|  
|描述 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集概念并提供指向关联内容和任务的链接。|[AlwaysOn 故障转移群集实例 (SQL Server)](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)|  
|描述 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移策略概念，并提供指向配置故障转移策略以满足您的组织要求的链接。|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|描述如何维护您的现有 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集。|[故障转移群集实例管理和维护](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|介绍如何在 Windows Server 故障转移群集 (WSFC) 上安装 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 。|[如何安装群集 SQL Server Analysis Services](/previous-versions/sql/sql-server-2012/dn736073(v=msdn.10))|  
  
 
  
##  <a name="best-practices"></a><a name="BestPractices"></a> 最佳实践  
  
-   查看 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] [发行说明](/previous-versions/sql/sql-server-15/dn169381(v=sql.15))  
  
-   安装必备软件。 在运行安装程序以安装或升级到 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]之前，请首先安装下列必备软件以缩短安装时间。 可以在每个故障转移群集节点上安装必备软件，然后在运行安装程序之前将这些节点重新启动一次。  
  
    -   Windows PowerShell 不再由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序安装。 Windows PowerShell 是安装 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)][!INCLUDE[ssDE](../../../includes/ssde-md.md)] 组件和 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]。 如果你的计算机上没有 Windows PowerShell，你可以按照 [Windows Management Framework](https://go.microsoft.com/fwlink/?LinkId=186214) （Windows 管理框架）页上的说明启用它。  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序不再安装 .NET Framework 3.5 SP1，但是在较旧版本的 Windows 操作系统上安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 时可能需要该软件。 有关详细信息，请参阅 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)][发行说明](/previous-versions/sql/sql-server-15/dn169381(v=sql.15))。  
  
    -   **[!INCLUDE[msCoName](../../../includes/msconame-md.md)] 更新包：** 为了避免在安装过程中由于安装 .NET Framework 4 而重启计算机，[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 安装程序要求在计算机上安装 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 更新。  如果您正在 Windows 7 SP1 或 [!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] SP2 上安装 [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] ，则包括此更新。 如果您在早期的 Windows 操作系统上安装，则从 [Windows Vista 和 Windows Server 2008 上的 Microsoft Update for .NET Framework 4.0](https://go.microsoft.com/fwlink/?LinkId=198093)上下载它。  
  
    -   .NET Framework 4：安装程序在群集化的操作系统上安装 .NET Framework 4。 为了缩短安装时间，您可以考虑在您运行安装程序之前安装 .NET Framework 4。  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序支持文件。 您可以通过运行位于您的 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 安装介质上的 SqlSupport.msi 来安装这些文件。  
  
-   确认 WSFC 群集上未安装防病毒软件。 有关详细信息，请参阅 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 知识库文章 [Antivirus software may cause problems with cluster services（防病毒软件可能会导致群集服务出现问题）](/troubleshoot/windows-server/high-availability/not-cluster-aware-antivirus-software-cause-issue)。  
  
-   对故障转移群集安装的群集组命名时，群集组名称中不能包含以下任何字符：  
  
    -   小于运算符 (\<)  
  
    -   大于运算符 (>)  
  
    -   双引号 (")  
  
    -   单引号 (')  
  
    -   与符号 (&)  
  
     此外还要确认现有的群集组名称不包含不受支持的字符。  
  
-   确保所有群集节点配置相同，包括 COM+、磁盘驱动器号和 Administrators 组中的用户。  
  
-   确认已清除所有节点中的系统日志，并再次查看了系统日志。 确保在继续操作之前，日志中没有任何错误消息。  
  
-   在安装或更新 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集之前，应禁用在安装过程中可能会使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组件的所有应用程序和服务，但应使磁盘资源保持联机状态。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序自动设置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集组与将处于故障转移群集中的磁盘之间的依赖关系。 因此不要在运行安装程序之前设置磁盘的依赖关系。  
  
    -   在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集安装期间，将为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 网络资源名称创建计算机对象（Active Directory 计算机帐户）。 在 [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 群集中，群集名称帐户（群集自身的计算机帐户）需要有权创建计算机对象。 有关详细信息，请参阅 [在 Active Directory 中配置帐户](https://technet.microsoft.com/library/cc731002\(WS.10\).aspx)。  
  
    -   如果您要使用 SMB 文件共享作为存储选项，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装帐户必须拥有对文件服务器的 SeSecurityPrivilege 权限。 为此，请使用文件服务器上的“本地安全策略”控制台将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装帐户添加到 **“管理审核和安全日志”** 权限中。  
  
##  <a name="verify-your-hardware-solution"></a><a name="Hardware"></a> 确认您的硬件解决方案  
  
-   如果群集解决方案中包含地理位置分散的群集节点，则还必须验证网络延迟和共享磁盘支持之类的附加项。  
  
    -   有关 [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 和 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]的详细信息，请参阅 [为故障转移群集验证硬件](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732035(v=ws.10)) 和 [针对 Windows 故障转移群集的支持策略](https://go.microsoft.com/fwlink/?LinkId=196818)。  
  
-   确认未对要安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的磁盘进行压缩或加密。 如果尝试将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装到压缩驱动器或加密驱动器上， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序将失败。  
  
-   [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 和 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Advanced Server 和 Datacenter Server 版还支持 SAN 配置。 Windows 目录和硬件兼容性列表中的“群集/多群集设备”类别中列出了一组支持 SAN 的存储设备，这些设备已经过测试，可作为 SAN 存储单元并且附加多个 WSFC 群集。 在找到经过验证的组件后请运行群集验证。  
  
-   还支持使用 SMB 文件共享来安装数据文件。 有关详细信息，请参阅 [Storage Types for Data Files](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes)。  
  
    > [!WARNING]  
    >  如果您要将 Windows 文件服务器用作 SMB 文件共享存储，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装帐户必须拥有对文件服务器的 SeSecurityPrivilege 权限。 为此，请使用文件服务器上的“本地安全策略”控制台将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装帐户添加到 **“管理审核和安全日志”** 权限中。  
    >   
    >  如果要使用 Windows 文件服务器之外的 SMB 文件共享存储，请就文件服务器端的等效设置咨询存储供应商。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持装入点。  
  
     通过已装入卷或装入点可以使用单个驱动器号来引用多个磁盘或卷。 如果您有一个驱动器号 D: 引用常规磁盘或卷，则您可以在附加磁盘或卷不需要拥有各自的驱动器号的情况下，在驱动器号 D: 下连接或“装入”附加磁盘或卷作为目录。  
  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集的其他装入点注意事项：  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序要求已装入驱动器的基准驱动器具有关联驱动器号。 对于故障转移群集安装，此基准驱动器必须是群集驱动器。 在此版本中，不支持卷 GUID。  
  
    -   不能在故障转移群集实例之间共享具有驱动器号的基准驱动器。 这是对故障转移群集的正常限制，而不是对独立的多实例服务器的限制。  
  
    -   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的群集安装数取决于可用驱动器号的数量。 如果只对操作系统使用一个驱动器号，而所有其他驱动器号均可用作正常群集驱动器或群集驱动器宿主装入点，则每个故障转移群集最多只能有 25 个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。  
  
        > [!TIP]  
        >  通过使用 SMB 文件共享选项可以超过 25 个实例的限制。 如果您使用 SMB 文件共享作为存储选项，则可以安装最多 50 个 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例。  
  
    -   不支持在安装其他驱动器后对驱动器进行格式化。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集安装只支持使用本地磁盘安装 tempdb 文件。 确保为 tempdb 数据和日志文件指定的路径在所有群集节点上均有效。 在故障转移期间，如果 tempdb 目录对故障转移目标节点不可用，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源将无法联机。 有关详细信息，请参阅 [数据文件的存储类型](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md#StorageTypes) 和 [数据库引擎配置 - 数据目录](../../../database-engine/install-windows/install-sql-server.md)。  
  
-   如果您在 iSCSI 技术组件上部署 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集，建议您多加注意。 有关详细信息，请参阅 [针对 iSCSI 技术组件的 SQL Server 支持](/troubleshoot/sql/admin/support-iscsi-technology-components)。  
  
-   有关详细信息，请参阅 [针对 Microsoft 群集的 SQL Server 支持策略](/troubleshoot/sql/failover-clusters/support-policy-clustered-configurations)。  
  
-   有关正确的仲裁驱动器配置的详细信息，请参阅 [仲裁驱动器配置信息](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770620(v=ws.10))。  
  
-   如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 源安装文件和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集位于不同的域中，若要安装该群集，需要将安装文件复制到可用于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集的当前域中。  
  
##  <a name="review-security-considerations"></a><a name="Security"></a> 查看安全注意事项  
  
-   若要使用加密，请在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集中的所有节点上安装带有完全限定的 WSFC 群集 DNS 名称的服务器证书。 例如，如果您有一个包含两个节点（节点的名称分别为“Test1.DomainName.com”和“Test2.DomainName.com”）的群集和一个名为“Virtsql”的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集实例，则必须获取“Virtsql.DomainName.com”的证书，并在 test1 和 test2 节点上安装该证书。 然后，可以选中 **配置管理器中的** “强制协议加密” [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 复选框，以将故障转移群集配置为使用加密。  
  
    > [!IMPORTANT]  
    >  在将证书安装到故障转移群集实例中的所有参与节点上之前，请勿选中 **“强制协议加密”** 复选框。  
  
-   如果采用与早期版本并行的配置来安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务必须只使用全局域组内的帐户。 此外， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务所使用的帐户不得出现在本地 Administrators 组中。 不遵守此原则会导致意外的安全行为。  
  
-   若要创建故障转移群集，您必须是本地管理员，有权作为服务登录并有权在故障转移群集实例的所有节点上作为操作系统的一部分工作。  
  
-   在 [!INCLUDE[nextref_longhorn](../../../includes/nextref-longhorn-md.md)]上，会自动生成用于 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 服务的服务 SID。 对于从以前版本的 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 升级得到的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]故障转移群集实例，将保留现有的域组和 ACL 配置。  
  
-   域组必须与计算机帐户位于同一域中。 例如，如果将安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的计算机在 SQLSVR 域（MYDOMAIN 的子域）中，则必须在 SQLSVR 域中指定一个组。 SQLSVR 域可能包含来自 MYDOMAIN 的用户帐户。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集。  
  
-   请查阅 [Security Considerations for a SQL Server Installation](../../../sql-server/install/security-considerations-for-a-sql-server-installation.md)中的内容。  
  
-   若要对 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]启用 Kerberos 身份验证，请参阅 [知识库中的](https://support.microsoft.com/kb/319723) How to use Kerberos authentication in SQL Server [!INCLUDE[msCoName](../../../includes/msconame-md.md)] （如何在 SQL Server 中使用 Kerberos 身份验证）。  

-   SQL Server 故障转移群集实例 (FCI) 要求群集节点加入域。 不支持使用以下配置： 
    *   工作组群集上的 SQL FCI。 
    *   多域群集上的 SQL FCI。   
    *   域和工作组群集上的 SQL FCI。 

  
##  <a name="review-network-port-and-firewall-considerations"></a><a name="Network"></a> 查看网络、端口和防火墙注意事项  
  
-   确认在开始安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前，已对所有专用网卡禁用 NetBIOS。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的网络名称和 IP 地址不能用于任何其他目的，如共享文件。 如果要创建文件共享资源，请为该资源指定不同的唯一网络名称和 IP 地址。  
  
    > [!IMPORTANT]  
    >  我们建议您不要在数据驱动器上使用文件共享，因为它们可能影响 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的行为和性能。  
  
-   虽然 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 在群集中支持 TCP/IP 上的命名管道和 TCP/IP 套接字，但我们建议您在群集配置中使用 TCP/IP 套接字。  
  
-   请注意，ISA Server 在 Windows 群集上不受支持，因此它在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集上也不受支持。  
  
-   必须启动并正在运行远程注册表服务。  
  
-   必须启用远程管理。  
  
- 对于使用非默认端口的 SQL Server 实例，请使用 SQL Server 配置管理器的网络配置来确定要取消阻止的 SQL Server 实例所使用的端口。 如果要使用 [SQL Server Browser 服务](../../../tools/configuration-manager/sql-server-browser-service.md)（使用的 IP 地址不同于群集实例的 IP 地址）和 UDP 端口 1434 连接到 SQL Server 实例，请在防火墙中启用 IPALL 的 TCP 端口。 
  
-   故障转移群集安装程序操作包括一个用于检查网络绑定顺序的规则。 尽管绑定顺序看起来可能是正确的，但您可能已对系统禁用或“幻像”NIC 配置。 “幻像”NIC 配置可影响绑定顺序并导致绑定顺序规则发出警告。 若要避免此问题，请使用下列步骤来标识并删除禁用的网络适配器：  
  
    1.  在命令提示符下，键入：set devmgr_Show_Nonpersistent_Devices=1。  
  
    2.  键入并运行：start Devmgmt.msc。  
  
    3.  展开网络适配器的列表。 此列表中应仅包含物理适配器。 如果您具有禁用的网络适配器，安装程序将根据网络绑定顺序规则报告故障。 “控制面板/网络连接”还将显示适配器已禁用。 确认“控制面板”中“网络设置”显示的已启用物理适配器的列表与 devmgmt.msc 显示的列表相同。  
  
    4.  删除禁用的网络适配器，然后再运行 SQL Server 安装程序。  
  
    5.  安装完成后，请返回到“控制面板”中的“网络连接”，禁用当前未使用的任何网络适配器。  
  
##  <a name="verify-your-operating-system"></a><a name="OS_Support"></a> 确认您的操作系统  
 确保您的操作系统已正确安装并且支持故障转移群集。 下表列出了 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本和支持这些版本的操作系统。  
  
|[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本|[!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] Enterprise|[!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] Datacenter Server|[!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Enterprise|[!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] Datacenter Server|  
|---------------------------------------|------------------------------------------------|-------------------------------------------------------|----------------------------------------------|-----------------------------------------------------|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise（64 位）x64*|是|是|是**|是**|  
|[!INCLUDE[ssSQL14](../../../includes/sssql14-md.md)] Enterprise（32 位）|是|是|||  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Developer（64 位）|是|是|是**|是**|  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Developer（32 位）|是|是|||  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Standard（64 位）|是|是|是|是|  
|[!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] Standard（32 位）|是|是|||  
  
 *[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 群集。 这包括从最初安装在 WOW 中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集的早期版本进行升级。 对于这种情况，只能选择通过并行安装新版本并迁移进行升级。  
  
 **支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集。  
  
##  <a name="additional-considerations-for-multi-subnet-configurations"></a><a name="MultiSubnet"></a> 针对多子网配置的其他注意事项  
 下面的部分描述了在安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集时要记住的要求。 多子网配置涉及跨多个子网的群集，因此，涉及使用多个 IP 地址以及对 IP 地址资源依赖关系的更改。  
  
### <a name="ssnoversion-edition-and-operating-system-considerations"></a>[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Edition 和操作系统注意事项  
  
-   有关支持 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 版本的信息，请参阅 [SQL Server 2016 各个版本支持的功能](~/sql-server/editions-and-supported-features-for-sql-server-2016.md)。  
  
-   若要创建 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集，您必须首先在多个子网上创建 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] 多站点故障转移群集。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集依赖于 Windows Server 故障转移群集，以便确保在存在失败时 IP 依赖关系条件依然有效。  
  
-   [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] 要求所有群集服务器都必须处于同一 Active Directory 域中。 因此， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 多子网故障转移群集要求所有群集节点都位于同一 Active Directory 域中，即使它们处于不同的子网中。  
  
#### <a name="ip-address-and-ip-address-resource-dependencies"></a>IP 地址和 IP 地址资源依赖关系  
  
1.  在多子网配置中，IP 地址资源依赖关系设置为 OR。 有关详细信息，请参阅[创建新的 SQL Server 故障转移群集（安装程序）](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)  
  
2.  不支持混合的 AND-OR IP 地址依赖关系。 例如，不支持 \<IP1> AND \<IP2> OR \<IP3>。  
  
3.  不支持每个子网多个 IP 地址。  
  
     如果您决定使用为同一子网配置的多个 IP 地址，在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 启动过程中可能会遇到客户端连接失败。  
  
#### <a name="related-content"></a>相关内容  
 有关 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)] 多站点故障转移的详细信息，请参阅 [Windows Server 2008 R2 故障转移群集站点](https://technet.microsoft.com/library/ff182338\(v=WS.10\).aspx) 和 [在多站点故障转移群集中设计群集服务或应用程序](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197430(v=ws.10))。  
  
##  <a name="configure-windows-server-failover-cluster"></a><a name="WSFC"></a> 配置 Windows Server 故障转移群集  
  
-   [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 必须至少在服务器群集的一个节点上配置群集服务 (WSFC)。 您还必须将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise、 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence 或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard 与 WSFC 一起运行。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Enterprise 支持最多 16 节点的故障转移群集。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Business Intelligence 和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Standard 支持两节点的故障转移群集。  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 服务的资源 DLL 导出两个函数，WSFC 群集管理器使用它们来检查 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 资源的可用性。 有关详细信息，请参阅 [故障转移群集实例的故障转移策略](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)。  
  
-   WSFC 必须能够使用 IsAlive 检查来验证故障转移群集实例是否正在运行。 这需要使用可信连接来连接到服务器。 默认情况下，在群集的节点上未将运行群集服务的帐户配置为管理员，并且 BUILTIN\Administrators 组没有登录到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的权限。 只有更改对群集节点的权限时，这些设置才会改变。  
  
-   配置域名服务 (DNS) 或 Windows Internet 名称服务 (WINS)。 必须在要安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 故障转移群集的环境中运行 DNS 服务器或 WINS 服务器。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序要求 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] IP 接口虚拟引用注册动态域名服务。 DNS 服务器配置应允许群集节点动态注册映射到网络名称的联机 IP 地址。 如果无法完成动态注册，安装程序将失败，安装将回滚。 有关详细信息，请参阅 [这篇知识库文章](https://support.microsoft.com/kb/947048)。  
  
##  <a name="install-msconame-distributed-transaction-coordinator"></a><a name="MSDTC"></a> 安装 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 分布式事务处理协调器  
 在故障转移群集上安装 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之前，请确定是否必须创建 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 分布式事务处理协调器 (MSDTC) 群集资源。 如果只安装 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]，则 MSDTC 群集资源不是必需的。 如果要安装 [!INCLUDE[ssDE](../../../includes/ssde-md.md)] 和 SSIS、工作站组件或者要使用分布式事务处理，则必须安装 MSDTC。 请注意，MSDTC 对于仅 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)]实例不是必需的。  
  
 在 [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] 和 [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]中，您可以在单个故障转移群集上安装 MSDTC 的多个实例。 安装的第一个 MSDTC 实例将是 MSDTC 的群集默认实例。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将通过自动使用安装到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 本地群集资源组的 MSDTC 实例，利用该 MSDTC 实例。 但是，单个应用程序可以映射到群集上的任何 MSDTC 实例。  
  
 下面的规则适用于由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]选择的 MSDTC 的实例：  
  
-   使用安装到本地组的 MSDTC，否则  
  
-   使用 MSDTC 的映射的实例，否则  
  
-   使用群集的 MSDTC 的默认实例，否则  
  
-   使用本地计算机上安装的 MSDTC 实例  
  
> [!IMPORTANT]  
>  如果安装到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的本地群集组的 MSDTC 实例失败， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将不自动尝试使用 MSDTC 的默认群集实例或本地计算机实例。 为了使用 MSDTC 的其他实例，您将需要从 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组中完全删除 MSDTC 的失败的实例。 同样，如果您为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 创建一个映射并且该映射的 MSDTC 实例失败，则您的分布式事务也将失败。 如果您希望 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用其他 MSDTC 实例，则必须将 MSDTC 的某个实例添加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的本地群集组，或删除该映射。  
  
### <a name="configure-msconame-distributed-transaction-coordinator"></a>配置 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 分布式事务处理协调器  
 安装操作系统并配置群集后，您还必须使用群集管理器配置 MSDTC 以便在群集中使用。 群集 MSDTC 失败不会导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 安装程序停止运行，但如果未能正确配置 MSDTC，则 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 应用程序功能可能会受影响。  
  
## <a name="see-also"></a>另请参阅  
 [安装 SQL Server 2016 的硬件和软件要求](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)   
 [系统配置检查器的检查参数](../../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)   
 [故障转移群集实例管理和维护](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)  
  
