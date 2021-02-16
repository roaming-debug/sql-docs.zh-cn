---
title: 配置 Server Core 安装
description: 本文介绍有关在服务器核心安装上配置 SQL Server 的详细信息（包括故障排除工具）。
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- IsHadrEnabled server property
- Server Core Installation [SQL Server]
ms.assetid: ed6e5e94-4b8d-422a-a17e-61b05a4df903
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 300e06b7eaab03af63685e495d8aa0de2ad83ed6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336384"
---
# <a name="configure-sql-server-on-a-server-core-installation"></a>在 Server Core 安装上配置 SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

本文详细介绍如何在服务器核心安装上配置 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  

##  <a name="configure-and-manage-server-core-on-windows-server"></a><a name="BKMK_ConfigureWindows"></a> 在 Windows Server 上配置和管理 Server Core  
本节提供帮助配置和管理 Server Core 安装的文章参考资料。  
  
在 Server Core 模式下，部分 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 功能不受支持。  其中的一些功能可以安装在客户端计算机或未运行 Server Core 的另一服务器上，然后连接到在 Server Core 上安装的数据库引擎服务。  
  
有关远程配置和管理 Server Core 安装的详细信息，请参阅以下文章：  
  
- [安装服务器核心](/windows-server/get-started/getting-started-with-server-core)  
  
- [通过 Sconfig.cmd 配置 Windows Server 2016 的服务器核心安装](/windows-server/get-started/sconfig-on-ws2016)  
  
- [在服务器核心服务器 Windows Server 2012 R2 上安装服务器角色和功能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574158(v=ws.11))
  
- [管理 Server Core 安装：概述](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441255(v=ws.10))  
  
- [管理 Server Core 安装](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee441258(v=ws.10))
  
##  <a name="install-ssnoversion-updates"></a><a name="BKMK_InstallSQLUpdates"></a> 安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新  
本节提供有关在 Windows Server Core 计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 更新的信息。 我们建议客户及时评估和安装最新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新，以便确保系统是最新的并且具有最近的安全更新。 有关在 Windows 服务器核心计算机上安装 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 的详细信息，请参阅[在服务器核心上安装 SQL Server](../../database-engine/install-windows/install-sql-server-on-server-core.md)。  
  
以下是安装产品更新的两个方案：  
  
- [在全新安装期间为 SQL Server 安装更新](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_NewInstall)  
  
- [在已安装 SQL Server 后为其安装更新](../../database-engine/install-windows/configure-sql-server-on-a-server-core-installation.md#bkmk_alreadyInstall)  
  
###  <a name="installing-updates-for-ssnoversion-during-a-new-installation"></a><a name="bkmk_NewInstall"></a> 在全新安装期间为 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 安装更新  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序在 Server Core 操作系统上仅支持命令提示符安装。 有关详细信息，请参阅 [从命令提示符安装 SQL Server](./install-sql-server-from-the-command-prompt.md)。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序将最新的产品更新集成到主产品安装中，以便可以同时安装主产品及其适用的更新。  
  
在找到最新版本的适用更新后，安装程序将下载这些更新并将其与当前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装过程集成在一起。 产品更新可请求累积更新、Service Pack 或者 Service Pack 连同累积更新。  
  
指定 UpdateEnabled 和 UpdateSource 参数可以在主产品安装中包含最新的产品更新。 参考以下示例以便在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装期间允许产品更新：  
  
```  
Setup.exe /qs /ACTION=Install /FEATURES=SQLEngine /INSTANCENAME=MSSQLSERVER /SQLSVCACCOUNT="<DomainName\UserName>" /SQLSVCPASSWORD="<StrongPassword>" /SQLSYSADMINACCOUNTS="<DomainName\UserName>" /AGTSVCACCOUNT="NT AUTHORITY\Network Service" /UpdateEnabled=True /UpdateSource="<SourcePath>" /IACCEPTSQLSERVERLICENSETERMS  
```  
  
###  <a name="installing-updates-for-ssnoversion-after-it-has-been-installed"></a><a name="bkmk_alreadyInstall"></a> 在已安装 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 后为其安装更新  
在 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]的已安装实例上，我们建议您应用最新的安全更新和关键更新，包括常规分发发布 (GDR) 和 Service Pack (SP)。 单独的累积更新和安全更新应该根据需要逐案例采用。 评估更新；如果需要，则应用该更新。  
  
在命令提示符下应用更新，从而将 <package_name> 替换为更新包的名称：  
  
- 更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的单个实例和所有共享组件。 您可以通过使用 InstanceName 参数或 InstanceID 参数指定实例。  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /InstanceName=MyInstance  
    ```  
  
- 仅更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 共享组件：  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch  
    ```  
  
- 更新计算机上的所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例和所有共享组件：  
  
    ```  
    <package_name>.exe /qs /IAcceptSQLServerLicenseTerms /Action=Patch /AllInstances  
    ```  
  
## <a name="startstop-ssnoversion-service"></a><a name="BKMK_StartStopServices"></a> 启动/停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务  
[sqlservr Application](../../tools/sqlservr-application.md) 应用程序可以在命令提示符下启动、停止、暂停和继续 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的实例。  
  
您还可以使用 Net 服务启动和停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。  
  
## <a name="enable-alwayson-availability-groups"></a><a name="BKMK_EnableAlwaysON"></a> 启用 AlwaysOn 可用性组  
启用 AlwaysOn 可用性组是服务器实例将可用性组用作高可用性和灾难恢复解决方案的一个先决条件。 有关管理 AlwaysOn 可用性组的详细信息，请参阅 [启用和禁用 AlwaysOn 可用性组 (SQL Server)](../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)。  
  
### <a name="using-ssnoversion-configuration-manager-remotely"></a>远程使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器  
这些步骤用于在运行 Windows 的客户端版的电脑上执行，或在安装了服务器图形 Shell 的 Windows Server 上运行。  
  
1. 打开“计算机管理”。 要打开“计算机管理”，请单击“启动”，键入 `compmgmt.msc`，然后单击“确定”  。    
  
2. 在控制台树中，右键单击“计算机管理”，再单击“连接到另一台计算机...” 。  
  
3. 在“选择计算机”对话框中，键入想要管理的服务器核心计算机名称或单击“浏览”进行查找，然后单击“确定”  。  
  
4. 在控制台树中，单击服务器核心计算机的“计算机管理”下的“服务和应用程序” 。  
  
5. 双击“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器”。  
  
6. 在“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器”中，单击“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务”，右键单击“[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (\<instance name>)”，其中 \<instance name> 是要启用 Always On 可用性组的本地服务器实例的名称，然后单击“属性”  。  
  
7. 选择 **“AlwaysOn 高可用性”** 选项卡。  
  
8. 验证“Windows 故障转移群集名称”字段包含本地故障转移群集节点的名称。 如果此字段为空，则此服务器实例当前不支持 AlwaysOn 可用性组。 原因可能是本地计算机不是群集节点、WSFC 群集已关闭或此版本的 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 不支持 AlwaysOn 可用性组。  
  
9. 选中“启用 AlwaysOn 可用性组”复选框，然后单击“确定”。  
  
10. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 配置管理器保存您的更改。 然后，必须手动重新启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务。 这使您可以选择最适合您的业务要求的重新启动时间。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务重新启动后，AlwaysOn 将启用，而且 IsHadrEnabled 服务器属性将设置为 1。  
  
> [!NOTE]
>  -   您在目标计算机上必须具有相应的用户权限或被委托了相应的授权才能连接到该计算机。  
> -   您正在管理的计算机名称显示在控制台树中的“计算机管理”旁边的括号中。  
  
### <a name="using-powershell-cmdlets-to-enable-alwayson-availability-groups"></a>使用 PowerShell Cmdlet 启用 AlwaysOn 可用性组  
PowerShell Cmdlet Enable-SqlAlwaysOn 用于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上启用 AlwaysOn 可用性组。 如果当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务正在运行时启用 AlwaysOn 可用性组，则必须重新启动数据库引擎服务才能完成更改。 除非您指定 -Force 参数，否则，cmdlet 将询问您是否要重新启动服务；如果取消，将不会发生任何操作。  
  
您必须拥有管理员权限才能执行此 cmdlet。  
  
可以使用以下语法之一来为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例启用 AlwaysOn 可用性组：  
  
```  
Enable-SqlAlwaysOn [-Path <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn -InputObject <Server> [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
```  
Enable-SqlAlwaysOn [-ServerInstance <string>] [-Credential <PSCredential>] [-Force] [-NoServiceRestart] [-Confirm] [-WhatIf] [<Commom Parameters>]  
```  
  
以下 PowerShell 命令在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例 (Machine\Instance) 上启用 AlwaysOn 可用性组：  
  
```  
Enable-SqlAlwaysOn -Path SQLSERVER:\SQL\Machine\Instance  
```  
  
##  <a name="configuring-remote-access-of-ssnoversion-running-on-server-core"></a><a name="BKMK_ConfigureRemoteAccess"></a> 配置运行在 Server Core 上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的远程访问  
 执行下述操作以配置运行在 Windows 服务器核心上的 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 实例的远程访问。  
  
### <a name="enable-remote-connections-on-the-instance-of-ssnoversion"></a>启用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例上的远程连接  
 若要启用远程连接，请在本地使用 SQLCMD.exe 并对 Server Core 实例执行以下语句：  
  
-   `EXEC sys.sp_configure N'remote access', N'1'`  
  
     `GO`  
  
-   `RECONFIGURE WITH OVERRIDE`  
  
     `GO`  
  
### <a name="enable-and-start-the-ssnoversion-browser-service"></a>启用并启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服务  
 默认情况下，Browser 服务是禁用的。  如果在 Server Core 上运行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例禁用了该服务，请从命令提示符运行以下命令来启用它：  
  
 `sc config SQLBROWSER start= auto`  
  
 在启用该服务后，请从命令提示符运行以下命令来启动该服务：  
  
 `net start SQLBROWSER`  
  
### <a name="create-exceptions-in-windows-firewall"></a>在 Windows 防火墙中创建例外  
 若要在 Windows 防火墙中创建 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 访问的例外，请执行 [配置 Windows 防火墙以允许 SQL Server 访问](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)中指定的步骤。  
  
### <a name="enable-tcpip-on-the-instance-of-ssnoversion"></a>在其实例上启用 TCP/IP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 可以在 Server Core 上通过 Windows PowerShell 为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启用 TCP/IP 协议。 执行以下步骤:  
  
1.  在运行 Windows 服务器核心的计算机上，启动“任务管理器”。  
  
2.  在 **“应用程序”** 选项卡上，单击 **“新建任务”** 。  
  
3.  在 **“创建新任务”** 对话框上的 **“打开”** 字段中键入 **sqlps.exe** ，然后单击 **“确定”** 。 这将打开 **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell** 窗口。  
  
4.  在 **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Powershell** 窗口中，运行以下脚本以启用 TCP/IP 协议：  
  
```powershell
$smo = 'Microsoft.SqlServer.Management.Smo.'  
$wmi = new-object ($smo + 'Wmi.ManagedComputer')  
# Enable the TCP protocol on the default instance.  If the instance is named, replace MSSQLSERVER with the instance name in the following line.  
$uri = "ManagedComputer[@Name='" + (get-item env:\computername).Value + "']/ServerInstance[@Name='MSSQLSERVER']/ServerProtocol[@Name='Tcp']"  
$Tcp = $wmi.GetSmoObject($uri)  
$Tcp.IsEnabled = $true  
$Tcp.Alter()  
$Tcp  
```  
  
##  <a name="ssnoversion-profiler"></a><a name="BKMK_Profiler"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 探查器  
 在远程计算机上，启动 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 并从“文件”菜单中选择“新建跟踪”，应用程序将显示“连接到服务器”对话框，在此对话框您可以指定要连接的、位于 Server Core 计算机上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 有关详细信息，请参阅 [Start SQL Server Profiler](../../tools/sql-server-profiler/start-sql-server-profiler.md)。  
  
 有关运行 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]需要哪些权限的信息，请参阅 [运行 SQL Server Profiler 所需的权限](../../tools/sql-server-profiler/permissions-required-to-run-sql-server-profiler.md)。  
  
 有关 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]的其他详细信息，请参阅 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)。  
  
##  <a name="ssnoversion-auditing"></a><a name="BKMK_Auditing"></a> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 审核  
 可以远程使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] 定义审核。 在创建并启用审核后，目标将接收各项。 有关创建和管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 审核的详细信息，请参阅 [SQL Server 审核（数据库引擎）](../../relational-databases/security/auditing/sql-server-audit-database-engine.md)。  
  
##  <a name="command-prompt-utilities"></a><a name="BKMK_CMD"></a> 命令提示实用工具  
 可以使用以下命令提示实用工具，它们允许您在 Server Core 计算机上为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 操作编写脚本。 下表包含了随 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的用于 Server Core 的命令提示实用工具列表：  
  
|**实用工具**|**说明**|**安装位置**|  
|-----------------|---------------------|----------------------|  
|[bcp 实用工具](../../tools/bcp-utility.md)|用于在 [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例和用户指定格式的数据文件之间复制数据。|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[dtexec 实用工具](../../integration-services/packages/dtexec-utility.md)|用于配置和执行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[dtutil 实用工具](../../integration-services/dtutil-utility.md)|用于管理 SSIS 包。|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]DTS\Binn|  
|[osql 实用工具](../../tools/osql-utility.md)|您可以在命令提示符下输入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、系统过程和脚本文件。|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlagent90 应用程序](../../tools/sqlagent90-application.md)|用于在命令提示符下启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理。|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<instance_name>\MSSQL\Binn|  
|[sqlcmd 实用工具](../../tools/sqlcmd-utility.md)|您可以在命令提示符下输入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句、系统过程和脚本文件。|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[SQLdiag 实用工具](../../tools/sqldiag-utility.md)|用于为 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 客户服务和支持部门收集诊断信息。|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlmaint 实用工具](../../tools/sqlmaint-utility.md)|用于执行在早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中创建的数据库维护计划。|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
|[sqlps 实用工具](../../tools/sqlps-utility.md)|用于运行 PowerShell 命令和脚本。 加载和注册 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 提供程序和 cmdlet。|[!INCLUDE[ssInstallPathVar](../../includes/ssinstallpathvar-md.md)]Tools\Binn|  
|[sqlservr Application](../../tools/sqlservr-application.md)|用于在命令提示符下启动和停止 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例以进行故障排除。|\<drive>:\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL14.MSSQLSERVER\MSSQL\Binn|  
  
##  <a name="use-troubleshooting-tools"></a><a name="BKMK_troubleshoot"></a> 使用故障排除工具  
 可以使用 [SQLdiag 实用工具](../../tools/sqldiag-utility.md) 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和其他类型的服务器中收集日志和数据文件，同时还可将其用于一直监视服务器或对服务器的特定问题进行故障排除。 SQLdiag 用于加快和简化为 Microsoft 客户支持服务部门收集诊断信息的过程。  
  
 可以在 Server Core 上使用以下文章中指定的语法在管理员命令提示符下启动该实用工具：[SQLdiag 实用工具](../../tools/sqldiag-utility.md)。  
  
## <a name="see-also"></a>另请参阅  
 [在服务器核心上安装 SQL Server](../../database-engine/install-windows/install-sql-server-on-server-core.md)   
 [安装操作指南文章](/previous-versions/sql/)  
  
