---
title: SQL Server 2012 发行说明 | Microsoft Docs
description: 本发行说明文档介绍了在安装 Microsoft SQL Server 2012 或者解决其相关问题之前，应该了解的一些已知问题。
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Release Notes, SQL Server
ms.assetid: 9ccb390a-67a9-4593-85ea-2b4c41c4620f
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 06294cb2dbf588ed5f7588d05ee5aa1c5af971bc
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076069"
---
# <a name="sql-server-2012-release-notes"></a>SQL Server 2012 发行说明
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
本发行说明文档介绍了在安装 [Microsoft SQL Server 2012](https://go.microsoft.com/fwlink/?LinkId=238647) 或解决其相关问题之前，应该了解的一些已知问题）。 本发行说明文档只能在线下载，而不提供有关的安装介质，并且本文档将定期更新。  
  
有关如何开始安装 SQL Server 2012 的信息，请参阅 SQL Server 2012 自述文件。 该自述文档在安装介质上提供，也可从 [自述文件](https://download.microsoft.com/download/3/B/D/3BD9DD65-D3E3-43C3-BB50-0ED850A82AD5/ENU/Readme.htm) (#自述文件) 下载页获得。 您还可以在 [SQL Server 联机丛书](/previous-versions/sql/sql-server-2012/ms130214(v=sql.110)) 中以及 [SQL Server 论坛](https://go.microsoft.com/fwlink/?LinkId=213599)上找到更多的信息。  
  
## <a name="10-before-you-install"></a><a name="Install"></a>1.0 安装之前  
在安装 [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]之前，请考虑以下信息。  
  
### <a name="11-rules-documentation-for-sql-server-2012-setup"></a>1.1 针对 SQL Server 2012 安装的规则文档  
**问题：** SQL Server 安装程序会在安装操作完成前验证你的计算机配置。 使用系统配置检查器 (SCC) 报告捕获在 SQL Server 安装操作过程中运行的不同规则。 与这些安装规则有关的文档在 MSDN 库中将不再提供。  
  
**解决方法：** 你可以参考系统配置检查报告，了解有关这些安装规则的详细信息。 系统配置检查将会生成一个报告，该报告包含对每个执行规则的简短说明以及执行状态。 该系统配置检查报告位于 %programfiles%\Microsoft SQL Server\110\Setup Bootstrap\Log\\<YYYYMMDD_HHMM>\\。  
  
### <a name="12-adding-a-local-user-account-for-the-distributed-replay-controller-service-might-terminate-setup-unexpectedly"></a>1.2 为分布式重播控制器服务添加本地用户帐户可能会意外终止安装程序  
**问题：** 在 SQL Server 安装程序的“Distributed Replay 控制器”页中，在尝试为 Distributed Replay 控制器服务添加本地用户帐户时，安装程序将意外终止并且显示“SQL Server 安装失败”的错误消息  。  
  
**解决方法：** 在 SQL 安装过程中，请勿通过“添加当前用户”或“添加...”来添加本地用户帐户。 在安装过程后，通过执行下面的步骤来手动添加本地用户帐户：  
  
1.  停止 SQL Server 分布式重播控制器服务。  
  
2.  在安装有控制器服务的控制器计算机上，在命令提示符下键入 dcomcnfg。  
  
3.  在“组件服务”窗口中，导航到“**控制台根节点**” -> “**组件服务**” -> “**计算机**” -> “**我的电脑**” -> “**Dconfig**” ->“**DReplayController**”。  
  
4.  右键单击“DReplayController”  ，然后单击“属性”  。  
  
5.  在 **“DReplayController 属性”** 窗口中的 **“安全性”** 选项卡上，单击 **“启动和激活权限”** 部分的 **“编辑”** 。  
  
6.  向该本地用户帐户授予“ **本地和远程激活** ”权限，然后单击“ **确定**”。  
  
7.  单击“访问权限”部分的“ **编辑** ”，并为本地用户帐户授予“ **本地和远程访问** ”权限，然后单击“ **确定**”。  
  
8.  单击 **“确定”** 以关闭 **“DReplayController 属性”** 窗口。  
  
9. 在控制器计算机上，将本地用户帐户添加到“ **分布式 COM 用户** ”组中。  
  
10. 启动 SQL Server 分布式重播控制器服务。  
  
### <a name="13-sql-server-setup-might-fail-while-trying-to-start-the-sql-server-browser-service"></a>1.3 SQL Server 安装程序在试图启动 SQL Server Browser 服务时可能失败  
**问题：** SQL Server 安装程序在试图启动 SQL Server Browser 服务时可能失败，出现如下错误：  
  
```
The following error has occurred:  
Service 'SQLBrowser' start request failed. Click 'Retry' to retry the failed action, or click 'Cancel' to cancel this action and continue setup.
```
  
或  
  
```
The following error has occurred:  
SQL Server Browser configuration for feature 'SQL_Browser_Redist_SqlBrowser_Cpu32' was cancelled by user after a previous installation failure. The last attempted step: Starting the SQL Server Browser service 'SQLBrowser', and waiting for up to '900' seconds for the process to complete.
```
  
**解决方法：** 在未能安装 SQL Server 引擎或 Analysis Services 时就会出现此情况。 若要解决该问题，请参考 SQL Server 安装程序日志来解决 SQL Server 引擎和 Analysis Services 失败的问题。 有关详细信息，请参阅查看和阅读 SQL Server 安装程序日志文件。 有关详细信息，请参阅 [查看和读取 SQL Server 安装程序日志文件](../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
### <a name="14-sql-server-2008-2008-r2-analysis-services-failover-cluster-upgrade-to-sql-server-2012-might-fail-after-renaming-the-network-name"></a>1.4 在重命名网络名称后，SQL Server 2008、2008 R2 Analysis Services 故障转移群集升级到 SQL Server 2012 可能失败  
**问题：** 在使用 Windows 群集管理员工具更改 Microsoft SQL Server 2008 或 2008 R2 Analysis Services 故障转移群集实例的网络名称后，升级操作可能会失败。  
  
**解决方法：** 若要解决此问题，请按照 [此知识库文章](https://support.microsoft.com/kb/955784)的解决方法部分中的说明更新 ClusterName 注册表项。  
  
### <a name="15-installing-sql-server-2012-on-windows-server-2008-r2-server-core-service-pack-1"></a>1.5 在 Windows Server 2008 R2 Server Core Service Pack 1 上安装 SQL Server 2012  
您可以在 Windows Server 2008 R2 Server Core SP1 上安装 SQL Server，但具有以下限制：  
  
-   Microsoft SQL Server 2012 不支持在 Server Core 操作系统上使用安装向导进行安装。 在服务器核心上进行安装时，SQL Server 安装程序支持完全静默模式（通过使用 /Q 参数）或简单静默模式（通过使用 /QS 参数）。  
  
-   在运行 Windows Server 2008 R2 Server Core SP1 的计算机上，不支持将早期版本的 SQL Server 升级到 Microsoft SQL Server 2012。  
  
-   在运行 Windows Server 2008 R2 Server Core SP1 的计算机上不支持安装 Microsoft SQL Server 2012 的 32 位版本。  
  
-   不能在运行 Windows Server 2008 R2 Server Core SP1 的计算机上并行安装 Microsoft SQL Server 2012 和早期版本的 SQL Server。  
  
-   不是 SQL Server 2012 的所有功能在 Server Core 操作系统上都支持。 有关支持的功能以及在 Server Core 上安装 SQL Server 2012 的详细信息，请参阅 [在 Server Core 上安装 SQL Server 2012](/previous-versions/sql/sql-server-2012/hh231669(v=sql.110))(#在-server-core-上安装-sql-server-2012)。  
  
### <a name="16-semantic-search-requires-you-to-install-an-additional-dependency"></a>1.6 语义搜索要求您安装其他依赖项  
**问题：** 统计语义搜索要求其他的必备组件，即语义语言统计数据库，而 SQL Server 安装程序并不安装此组件。  
  
**解决方法：** 若要将语义语言统计数据库设置为语义索引的必备组件，请执行以下任务：  
  
1.  在 SQL Server 安装介质上找到并运行名为 SemanticLanguageDatabase.msi 的 Windows Installer 包，以便解压缩数据库。 对于 SQL Server 2012 Express，从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=52681) (https://www.microsoft.com/download/details.aspx?id=52681) 下载语义语言统计数据库，然后运行 Windows Installer 包。  
  
2.  将数据库移到相应的数据文件夹。 如果您要使数据库保持在其默认位置，必须首先更改权限，然后才能成功附加该数据库。  
  
3.  附加已解压缩的数据库。  
  
4.  通过调用 **sp_fulltext_semantic_register_language_statistics_db** 存储过程并且提供在附加数据库时向该数据库提供的名称来注册该数据库。  
  
如果这些任务未完成，则在您尝试创建语义索引时，将看到以下错误消息：  
  
```
Msg 41209, Level 16, State 3, Line 1  
A semantic language statistics database is not registered. Full-text indexes using 'STATISTICAL_SEMANTICS' cannot be created or populated.
```
  
### <a name="17-installation-prerequisite-handling-during-sql-server-2012-setup"></a>1.7 SQL Server 2012 安装过程中的安装必备组件处理  
以下各项介绍了 SQL Server 2012 安装过程中的必备组件安装行为：  
  
-   仅在 Windows 7 SP1 或 Windows Server 2008 R2 SP1 上支持安装 SQL Server 2012。 不过，安装程序不会阻止在 Windows 7 或 Windows Server 2008 R2 上安装 SQL Server 2012。  
  
-   在您选择“数据库引擎”、“复制”、“Master Data Services”、“Reporting Services”、“Data Quality Services (DQS)”或“SQL Server Management Studio”时，.NET Framework 3.5 SP1 是 SQL Server 2012 所必需的，并且不再通过 SQL Server 安装程序进行安装。  
  
    -   如果您在使用 Windows Vista SP2 或 Windows Server 2008 SP2 操作系统的计算机上运行安装程序且未安装 .NET Framework 3.5 SP1，则 SQL Server 安装程序将要求您先下载并安装 .NET Framework 3.5 SP1，然后才能继续 SQL Server 安装。 可以从 Windows 更新或直接从 [此处](https://www.microsoft.com/download/details.aspx?id=25150)(#此处) 下载 .NET Framework 3.5 SP1。 若要避免在 SQL Server 安装期间中断，可在运行 SQL Server 安装程序之前，先下载并安装 .NET Framework 3.5 SP1。  
  
    -   如果您在使用 Windows 7 SP1 或 Windows Server 2008 R2 SP1 操作系统的计算机上运行安装程序，则必须先启用 .NET Framework 3.5 SP1，然后才能安装 SQL Server 2012。  
  
        **请使用以下方法之一在 Windows Server 2008 R2 SP1 上启用 .NET Framework 3.5 SP1：**  
  
        方法 1：使用服务器管理器  
  
        1.  在服务器管理器中，单击“ **添加功能** ”以显示可能功能的列表。  
  
        2.  在“ **选择功能** ”界面中，展开“ **.NET Framework 3.5.1 功能** ”条目。  
  
        3.  展开“ **.NET Framework 3.5.1 功能**”后，会看到两个复选框。 一个复选框用于 .NET Framework 3.5.1，另一个复选框用于 WCF 激活。 选择“ **.NET Framework 3.5.1**”，然后单击“ **下一步**”。 如果未安装必需的角色服务和功能，则无法安装 .NET Framework 3.5.1 功能。  
  
        4.  在“ **确认安装选择**”中，检查所选项，然后单击“安装”。  
  
        5.  等到安装过程完毕后，再单击“ **关闭**”。  
  
        方法 2：使用 Windows PowerShell  
  
        1.  单击“**启动**” | “**所有程序**” | “**附件**”。  
  
        2.  展开“ **Windows PowerShell**”，右键单击“ **Windows PowerShell**”，然后单击“ **以管理员身份运行**”。 在“ **用户帐户控制** ”框中单击“ **是** ”。  
  
        3.  在 PowerShell 命令提示符处，键入以下命令，然后在每条命令之后按 Enter：  
  
            ```  
            Import-Module ServerManager  
            Add-WindowsFeature as-net-framework  
            ```  
  
        **请使用以下方法在 Windows 7 SP1 上启用 .NET Framework 3.5 SP1：**  
  
        1.  单击“**开始**” | “**控制面板**” | “**程序**”，然后单击“**打开或关闭 Windows 功能**”。 如果系统提示您输入管理员密码或进行确认，请键入密码或提供确认。  
  
        2.  若要启用“ **Microsoft .NET Framework 3.5.1**”，请选中该功能旁边的复选框。 若要关闭 Windows 功能，请取消选中该复选框。  
  
        3.  单击“确定”。   
  
        **使用部署映像服务和管理 (DISM.exe) 启用 .NET Framework 3.5 SP1：**  
  
        您还可以使用部署映像服务和管理 (DISM.exe) 启用 .NET Framework 3.5 SP1。 有关联机启用 Windows 功能的详细信息，请参阅 [联机启用或禁用 Windows 功能](/previous-versions/windows/it-pro/windows-7/dd744582(v=ws.10))(#联机启用或禁用-windows-功能)。 下面是启用 .NET Framework 3.5 SP1 的说明：  
  
        1.  在命令提示符下，键入以下命令以便列出在操作系统中提供的所有功能：  
  
            ```  
            sm /online /Get-Features  
            ```  
  
        2.  可选：在命令提示符下，键入以下命令以便列出与你感兴趣的特定功能有关的信息。  
  
            ```  
            Dism /online /Get-FeatureInfo /FeatureName:NetFx3  
            ```  
  
        3.  键入以下命令启用 Microsoft .NET Framework 3.5.1。  
  
            ```  
            Dism /online /Enable-Feature /FeatureName:NetFx3  
            ```  
  
-   .NET Framework 4 是 SQL Server 2012 所必需的。 SQL Server 安装程序会在执行功能安装步骤的过程中安装 .NET Framework 4。  
  
    在 Windows Server 2008 R2 SP1 Server Core 操作系统上进行安装时，SQL Server 2012 Express 不安装 .NET Framework 4。 在安装 SQL Server 2012 Express（仅限数据库）时，如果存在 .NET Framework 3.5 SP1，则不需要 .NET Framework 4。 在 .NET Framework 3.5 SP1 不存在或者在安装 SQL Server 2012 Management Studio Express、SQL Server 2012 Express with Tools 或 SQL Server 2012 Express with Advanced Services 时，您必须首先安装 .NET Framework 4，然后在 Windows Server 2008 R2 SP1 Server Core 操作系统上安装 SQL Server2012 Express。  
  
-   为了确保 Visual Studio 组件可以正确安装，SQL Server 要求您安装更新。 SQL Server 安装程序会检查此更新是否存在，然后会要求您在继续安装 SQL Server 之前先下载并安装此更新。 若要避免在 SQL Server 安装期间中断，可在运行 SQL Server 安装程序之前先按下面所述下载并安装此更新（也可以安装 Windows 更新上提供的 .NET Framework 3.5 SP1 的所有更新）。  
  
    -   如果在使用 Windows Vista SP2 或 Windows Server 2008 SP2 操作系统的计算机上安装 SQL Server 2012，则可以从 [此处](https://support.microsoft.com/?kbid=956250)(#此处) 获得所需更新。  
  
    -   如果您在使用 Windows 7 SP1 或 Windows Server 2008 R2 SP1 操作系统的计算机上安装 SQL Server 2012，则此更新已安装在该计算机上。  
  
-   Windows PowerShell 2.0 是用于安装 SQL Server 2012 数据库引擎组件和 SQL Server Management Studio 的必备组件，但 Windows PowerShell 不再由 SQL Server 安装程序安装。 如果你的计算机上没有 PowerShell 2.0，则可以按照 [Windows Management Framework](https://mskb.pkisolutions.com/kb/968929) (#windows-management-framework) 页上的说明启用它。 您获取 Windows PowerShell 2.0 的方式取决于您正在运行的操作系统：  
  
    -   Windows Server 2008 – Windows PowerShell 1.0 是一个功能并且可以添加。 下载并安装 Windows PowerShell 2.0 版本（作为操作系统修补程序生效）。  
  
    -   Windows 7/Windows Server 2008 R2 – 默认安装 Windows PowerShell 2.0。  
  
-   如果您计划在 SharePoint 环境中使用 SQL Server 2012 功能，则需要 SharePoint Server 2010 Service Pack 1 (SP1) 和 SharePoint 八月累积更新。 必须首先安装 SP1（SharePoint [八月累积更新](https://blogs.technet.com/b/stefan_gossner/archive/2010/09/02/august-2010-cumulative-update-for-sharepoint-has-been-released.aspx)(#八月累计更新)）并全面修补服务器场，才能将 SQL Server 2012 功能添加到场中。 此要求适用于以下 SQL Server 2012 功能：使用数据库引擎实例作为场的数据库服务器，配置 PowerPivot for SharePoint，或者在 SharePoint 模式下部署 Reporting Services。  
  
### <a name="18-supported-operating-systems-for-sql-server-2012"></a>1.8 SQL Server 2012 支持的操作系统  
在 Windows Vista SP2、Windows Server 2008 SP2、Windows 2008 R2 SP1 和 Windows 7 SP1 操作系统上支持 SQL Server 2012。  
  
### <a name="19-sync-framework-is-not-included-in-the-installation-package"></a>1.9 Sync Framework 未包含在安装包中  
**问题：** Sync Framework 未包含在 SQL Server 2012 安装包中。  
  
**解决方法：** 可以从 [此 Microsoft 下载中心页](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=23217)下载适当版本的 Sync Framework。  
  
### <a name="110-if-visual-studio-2010-service-pack-1-is-uninstalled-the-sql-server-2012-instance-must-be-repaired-to-restore-certain-components"></a>1.10 如果卸载了 Visual Studio 2010 Service Pack 1，则必须修复 SQL Server 2012 实例以还原特定组件  
**问题：** [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 安装依赖于 Visual Studio 2010 Service Pack 1 的某些组件。 如果卸载 Service Pack 1，某些共享组件将降级为其初始版本，并且另有少数组件将从计算机中完全删除。  
  
**解决方法：** 从最初的源介质或网络安装位置修复 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] 实例。  
  
1.  从 SQL Server 安装介质中启动 SQL Server 安装程序 (setup.exe)。  
  
2.  安装必备组件并进行系统验证之后，安装程序会显示“ **SQL Server 安装中心** ”页。  
  
3.  单击左侧导航区域中的“维护”  ，然后单击“修复”  启动修复操作。 如果使用“ **开始** ”菜单启动了安装中心，则需要在此时提供安装介质的位置。  
  
4.  将运行安装程序支持规则和文件例程，以确保您的系统上安装了必备组件，并且计算机能够通过安装程序验证规则。 单击 **“确定”** 或 **“安装”** 以继续操作。  
  
5.  在“ **选择实例** ”页上选择要修复的实例，然后单击“ **下一步** ”继续操作。  
  
6.  将运行修复规则以验证修复操作。 若要继续，请单击 **“下一步”** 。  
  
7.  “ **准备修复** ”页指示修复操作已准备就绪，可以继续。 若要继续，请单击 **“修复”** 。  
  
8.  “ **修复进度** ”页显示修复操作的状态。 “ **完成** ”页指示修复操作已完成。  
  
有关如何修复 SQL Server 实例的详细信息，请参阅 [修复失败的 SQL Server 2012 安装](../database-engine/install-windows/repair-a-failed-sql-server-installation.md)(#修复失败的-sql-server-2012-安装)。  
  
### <a name="111-an-instance-of-sql-server-2012-might-fail-after-an-os-upgrade"></a>1.11 SQL Server 2012 实例在操作系统升级后可能失败  
**问题：** 如果将操作系统从 Windows Vista 升级到 Windows 7 SP1，SQL Server 2012 实例可能失败，且具有以下错误。  
  
`Setup has detected that the .NET Framework version 4 needs to be repaired. Do not restart your computer until Setup is complete.`  
  
**解决方法**：在升级操作系统后修复安装的 .NET Framework 4。 有关详细信息，请参阅 [如何修复现有的 .NET Framework 安装](https://support.microsoft.com/kb/306160)(#如何修复现有的-.net-framework-安装)。  
  
### <a name="112-sql-server-edition-upgrade-requires-a-restart"></a>1.12 Server SQL 版本升级要求重新启动  
**问题**：对 SQL Server 2012 实例进行版本升级时，某些与新版本相关的功能可能不能立即激活。  
  
**解决方法**：在对 SQL Server 2012 实例进行版本升级后重新启动计算机。 有关 SQL Server 2012 中支持的升级的详细信息，请参阅 [支持的版本和版本升级](../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md)(#支持的版本和版本升级)。  
  
### <a name="113-database-with-read-only-filegroup-or-files-cannot-be-upgraded"></a>1.13 不能升级具有只读文件组或文件的数据库  
**问题**：如果数据库或其文件/文件组被设置为只读，则无法通过附加数据库或从备份还原数据库来升级数据库。  返回错误 3415。  执行 SQL Server 实例的就地升级时也存在该问题。 即，您尝试通过安装 SQL Server 2012 来替换现有 SQL Server 实例且一个或多个现有数据库被设置为只读。  
  
**解决方法：** 在升级前，确保将数据库及其文件/文件组设置为读写。  
  
### <a name="114-reinstalling-an-instance-of-sql-server-failover-custer-fails-if-you-use-the-same-ip-address"></a>1.14 如果使用同一 IP 地址，则重新安装 SQL Server 故障转移群集实例失败  
**问题：** 如果在安装 SQL Server 故障转移群集实例期间指定不正确的 IP 地址，安装将失败。 卸载失败的实例后，如果尝试使用同一实例名称和正确的 IP 地址重新安装 SQL Server 故障转移群集实例，安装将失败。 安装失败是由于上次安装留下的重复资源组造成的。  
  
**解决方法：** 要解决此问题，请在重新安装时使用不同的实例名称，或在重新安装前手动删除该资源组。 有关详细信息，请参阅 [在 SQL Server 故障转移群集中添加或删除节点](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="20-analysis-services"></a><a name="AS"></a>2.0 Analysis Services  
  
### <a name="21-sql-editor-and-as-editor-cannot-connect-to-their-respective-server-instances-in-the-same-ssms-instance"></a>2.1 SQL 编辑器和 AS 编辑器无法在同一个 SSMS 实例中连接到其各自的服务器实例  
**问题：** 在已连接 SQL 编辑器时无法使用 MDX/DMX 编辑器连接到 Analysis Services 服务器。  
  
在使用 SQL Server Management Studio 2012 (SSMS) 时，如果在编辑器中打开某一 .sql 文件并且连接到某一 SQL Server 实例，则在同一个 SSMS 实例中打开的 MDX 或 DMX 文件无法连接到 AS 服务器的实例。 同样，如果某一 MDX 或 DMX 文件已在 SSMS 的编辑器中打开并且连接到某一 AS 服务器实例，则在同一个 SSMS 实例中打开的 .sql 文件无法连接到 SQL Server 的实例。  
  
**解决方法**：使用以下选项之一可解决此问题。  
  
-   启动另一个 SSMS 实例以便打开该 MDX / DMX 文件。  
  
-   断开与 SQL 编辑器的连接，然后将 MDX / DMX 编辑器连接到 AS 服务器。  
  
### <a name="22-cannot-create-or-open-tabular-projects-when-builtinadministrators-group-name-cannot-be-resolved"></a>2.2 在 BUILTIN\Administrators 组名称无法解析时，无法创建或打开表格项目  
**问题：** 你必须首先是工作区数据库服务器的管理员，然后才能创建或打开表格项目。 可以通过添加用户名或组名，将某一用户添加到服务器管理员组。 如果您是 BUILTIN\Administrator 组的成员，则无法创建或编辑 BIM 文件，除非该工作区数据库服务器联接到最初对其进行设置的域。 如果您打开或创建该 BIM 文件，则操作将失败并且显示以下错误消息：  
  
`"The BIM file cannot be opened. The server connected to is not valid. Reason: You are not an administrator of server [server name]."`  
  
**解决方法：**  
  
-   将工作区数据库服务器和 BSQL Server Data Tools (SSDT) 计算机重新联接到域。  
  
-   如果工作区数据库服务器和/或 SSDT 计算机在任何情况下都将不会联接到域，则添加单独的用户名作为工作区数据库服务器上的管理员，而非添加 BUILTIN\Administrators 组。  
  
### <a name="23-ssis-components-for-as-tabular-models-do-not-work-as-expected"></a>2.3 用于 AS 表格模型的 SSIS 组件不像预期一样工作  
对于表格模型，用于 Analysis Services (AS) 的 SQL Server Integration Services (SSIS) 组件不像预期一样工作。 下面是在您尝试为使用表格模型编写 SSIS 包时可能会发生的一些已知问题。  
  
**问题：** AS 连接管理器无法在与数据源相同的解决方案中使用表格模型。  
  
**解决方法：** 你必须首先显式连接到 AS 服务器，然后配置 AS 处理任务或 AS 执行 DDL 任务。  
  
在您使用表格模型时，存在与 AS 处理任务有关的问题：  
  
**问题：** 你将看到多维数据集、度量值组和维度，而非数据库、表和分区。 这是对该任务的一个限制。  
  
**解决方法：** 你仍可以使用多维数据集/度量值组/维度结构处理你的表格模型。  
  
**问题：** 在表格模式下运行的 AS 支持的某些处理选项在 AS 处理任务（例如处理碎片整理）中未公开。  
  
**解决方法：** 请改用 Analysis Services 执行 DDL 任务执行包含 ProcessDefrag 命令的 XMLA 脚本。  
  
**问题：** 该工具中的某些配置选项不适用。 例如，在处理分区时不应使用“处理相关对象”，并且“并行处理”配置选项包含指示在标准 SKU 上不支持并行处理的无效错误消息。  
  
**解决方法：** 无  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="30-books-online"></a><a name="BOL"></a>3.0 联机丛书  
  
### <a name="31-help-viewer-for-sql-server-crashes-in-environments-configured-to-run-only-ipv6"></a>3.1 用于 SQL Server 的帮助查看器在配置为“仅运行 IPv6”的环境中崩溃  
**问题**：如果你的环境配置为仅运行 IPv6，则用于 SQL Server 2012 的帮助查看器将崩溃，并且将显示以下错误消息：  
  
`HelpLibAgent.exe has stopped working.`  
  
> [!IMPORTANT]  
> 这适用于仅在启用了 IPv6 的情况下运行的所有环境。 启用了 IPv4（以及 IPv4 与 IPv6）的环境不受影响。  
  
**解决方法**：若要避免此问题，请启用 IPv4，或者使用以下步骤添加一个注册表项并且创建一个 ACL 以便启用用于 IPv6 的帮助查看器：  
  
1.  在 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Help\v1.0 下创建一个名为“IPv6”且值为“1 (DWORD(32 bit))”的注册表项。  
  
2.  为用于 IPv6 的端口设置安全性 ACL，并且从管理 CMD 窗口执行以下命令：  
  
    ```  
    netsh http add urlacl url=https://[::1]:47873/help/ sddl=D:(A;;GX;;;WD)  
    ```  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="40-data-quality-services"></a><a name="DQS"></a>4.0 Data Quality Services  
  
### <a name="41-dqs-not-supported-in-a-cluster"></a>4.1 在群集中不支持 DQS  
**问题：** 在 SQL Server 群集安装中不支持 DQS。 如果你在安装 SQL Server 的某一群集实例，则不得在“ **功能选择** ”页上选中“ **Data Quality Services** ”和“ **数据质量客户端** ”复选框。 如果在群集实例安装过程中选中了这些复选框（并且通过运行 DQSInstaller.exe 文件完成了数据质量服务器安装），则 DQS 将安装在此节点上，但在将更多节点添加到群集时不可用于附加节点，因此在附加节点上将不起作用。  
  
**解决方法：** 安装 SQL Server 2012 累积更新 1 可解决此问题。 有关说明，请参阅[https://support.microsoft.com/kb/2674817](https://support.microsoft.com/kb/2674817)。  
  
### <a name="42-to-reinstall-data-quality-server-delete-the-dqs-objects-after-uninstalling-data-quality-server"></a>4.2 若要重新安装数据质量服务器，请在卸载数据质量服务器后删除 DQS 对象  
**问题：** 如果卸载数据质量服务器，DQS 对象（DQS 数据库、DQS 登录名和 DQS 存储过程）不会从 SQL Server 实例中删除。  
  
**解决方法：** 要在同一台计算机上和相同 SQL Server 实例中重新安装数据质量服务器，必须从 SQL Server 实例中手动删除 DQS 对象。 此外，您还必须首先从计算机上的 C:\Program Files\Microsoft SQL Server\MSSQL11.<SQL_Server_Instance>\MSSQL\DATA 文件夹中删除 DQS 数据库（DQS_MAIN、DQS_PROJECTS 和 DQS_STAGING_DATA）文件，然后才能重新安装数据质量服务器。 否则，数据质量服务器安装将失败。 如果您想要保留数据，例如知识库或数据质量项目，则应移动数据库文件，而非删除它们。 有关在卸载进程完成后删除 DQS 对象的详细信息，请参阅 [删除数据质量服务器对象](./install/remove-data-quality-server-objects.md)(#删除数据质量服务器对象)。  
  
### <a name="43-indication-of-a-terminated-knowledge-discovery-or-interactive-cleansing-activity-is-delayed"></a>4.3 知识发现或交互式清理活动已终止的指示被延迟  
**问题：** 如果管理员在“活动监视”屏幕中终止某一活动，则在正在运行知识发现、域管理或交互式清理活动的交互用户执行下一操作前，将不会收到指出其活动已终止的任何指示。  
  
**解决方法：** 无  
  
### <a name="44-a-cancel-operation-discards-work-from-multiple-activities"></a>4.4 取消操作放弃来自多个活动的工作  
**问题：** 如果为正在运行的知识发现或域管理活动单击“取消”，并且当此活动正在运行时其他活动之前已完成了且没有任何发布操作正在执行，则自上次发布以来执行的所有活动中的工作都将被放弃，而不仅是放弃当前工作  。  
  
**解决方法：** 为避免此问题，请在开始新活动之前，发布需要保留在知识库中的工作。  
  
### <a name="45-controls-do-not-scale-properly-on-large-font-sizes"></a>4.5 对于大字号，控件不能正确缩放  
**问题：** 如果将文本大小更改为“更大 - 150%”（在 Windows Server 2008 或 Windows 7 中），或者将自定义 DPI 设置更改为 200%（在 Windows 7 中），“新建知识库”页上的“取消”和“创建”按钮将无法访问    。  
  
**解决方法：** 若要解决此问题，请设置较小的字号。  
  
### <a name="46-screen-resolution-of-800x600-is-not-supported"></a>4.6 不支持 800x600 的屏幕分辨率  
**问题：** 如果屏幕分辨率设置为 800x600，则数据质量客户端应用程序不会正确显示。  
  
**解决方法：** 若要解决此问题，请将屏幕分辨率设为更高值。  
  
### <a name="47-map-bigint-column-in-the-source-data-to-a-decimal-domain-to-prevent-data-loss"></a>4.7 将源数据中的 Bigint 列映射为小数域以免数据丢失  
**问题：** 如果源数据中的某一列为 bigint 数据类型，则必须在 DQS 中将此列映射为 decimal 数据类型的域，而非 integer 数据类型    。 其原因在于， **decimal** 数据类型与 **int** 数据类型相比可表示更大的值范围，因此可以存放更大的值。  
  
### <a name="48-nvarcharmax-and-varcharmax-data-types-are-not-supported-in-the-dqs-cleansing-component-in-integration-services"></a>4.8 在 Integration Services 的 DQS 清理组件中不支持 NVARCHAR(MAX) 和 VARCHAR(MAX) 数据类型  
**问题：** Integration Services 的 DQS 清理组件中不支持 nvarchar(max) 和 varchar(max) 数据类型的数据列   。 同样地，这些数据列在 DQS 清理转换编辑器的“映射”选项卡中无法使用，因此无法清理。  
  
**解决方法：** 在使用 DQS 清理组件处理这些数据列前，必须使用数据转换将其转换为 DT_STR 或 DT_WSTR 数据类型   。  
  
### <a name="49-the-item-to-run-dqsinstallerexe-on-the-start-menu-is-overwritten-on-new-sql-server-instance-installation"></a>4.9 在新的 SQL Server 实例安装上，“开始”菜单上运行 DQSInstaller.exe 的项被改写  
**问题：** 如果选择在 SQL Server 实例上安装 Data Quality Services，完成 SQL Server 安装后，则会在名为“Data Quality 服务器安装程序”的“Data Quality Services”程序组下的“开始”菜单上创建一个项    。 但是，如果在同一计算机上安装多个 SQL Server 实例，在“ **开始** ”菜单上仍有单个“ **数据质量服务器安装程序** ”项。 单击此项将在最近安装的 SQL Server 实例中运行 DQSInstaller.exe 文件。  
  
### <a name="410-activity-monitoring-displays-incorrect-status-for-failed-integration-services-cleansing-activities"></a>4.10 对于失败的 Integration Services 清理活动，“活动监视”显示不正确的状态  
“活动监视”屏幕甚至对于“ **当前状态** ”列中失败的 Integration Services 清理活动也错误地显示“ **成功** ”。  
  
### <a name="411-schema-name-is-not-displayed-as-part-of-tableview-name"></a>4.11 架构名称不作为表/视图名称的一部分显示  
在数据质量客户端中的映射阶段，在任意 DQS 活动中选择 SQL Server 数据源时，显示不包含架构名称的表和视图的列表。 因此，如果有几个具有不同架构的同名表/视图，只能通过查看数据预览或通过选择它们然后查看要映射的可用字段来区分它们。  
  
### <a name="412-issue-with-cleansing-output-and-export-if-data-source-is-mapped-to-a-composite-domain-containing-a-child-domain-of-date-type"></a>4.12 将数据源映射到包含 date 类型的子域的复合域时清理输出和导出的问题  
在清理数据质量项目中，如果映射了某些源数据（带有包含 date 数据类型的子域的复合域）中的字段，则清理结果中的子域输出的日期格式不正确并且导出到数据库的操作失败。  
  
### <a name="413-error-when-mapping-to-an-excel-sheet-that-contains-a--semicolon-in-its-name"></a>4.13 映射到名称中包含 ;（分号）的 Excel 工作表时的错误  
**问题：** 在 Data Quality Client 中的任何 DQS 活动的“映射”页上，如果映射到名称中包含 ;（分号）的源 excel 工作表，当你在“映射”页上单击“下一步”时，系统将显示未经处理的异常消息    。  
  
**解决方法：** 从包含要映射的源数据的 Excel 文件的工作表名称中删除 ;（分号），然后重试。  
  
### <a name="414-issue-with-date-or-datetime-values-in-unmapped-source-fields-in-excel-during-cleansing-and-matching"></a>4.14 在清理和匹配期间 Excel 中未映射的源字段中 Date 或 DateTime 值的问题  
**问题**：如果源数据为 Excel 且没有映射包含 Date 或 DateTime 数据类型的值的源字段，在清理和匹配活动期间将发生以下事件   ：  
  
-   以 yyyy-mm-dd 格式显示和导出未映射的“Date”值。  
  
-   对于未映射的“DateTime”值，时间值将丢失，并以 yyyy-mm-dd 的格式显示和导出。  
  
**解决方法：** 你可以在清理活动中的“管理和查看结果”页以及匹配活动中的“匹配”页右下窗格中查看未映射的字段值   。  
  
### <a name="415-cannot-import-domain-values-from-an-excel-file-xls-containing-more-than-255-columns-of-data"></a>4.15 无法从包含 255 列以上的数据的 Excel 文件 (.xls) 导入域值  
**问题：** 如果将值导入某个域（该域来自包含 255 列以上数据的 Excel 97-2003 文件 (.xls)），将显示异常消息并且导入失败。  
  
**解决方法：** 若要解决此问题，可以执行以下操作之一：  
  
-   将 .xls 文件另存为 .xlsx 文件，然后将 .xlsx 文件中的值导入域。  
  
-   在 .xls 文件中删除第 255 列之后所有列的数据，保存该文件，然后将 .xls 文件中的值导入域。  
  
### <a name="416-activity-monitoring-feature-is-unavailable-for-roles-other-than-dqs_administrator"></a>4.16 活动监视功能不可用于 dqs_administrator 以外的其他规则  
活动监视功能仅供具有 dqs_administrator 角色的用户使用。 如果您的用户帐户具有 dqs_kb_editor 或 dqs_kb_operator 角色，则活动监视功能将不可用于数据质量客户端应用程序。  
  
### <a name="417-error-on-opening-a-knowledge-base-in-the-recent-knowledge-base-list-for-domain-management"></a>4.17 在为域管理打开“最近的知识库”列表中的数据库时出错  
问题：如果你在 Data Quality Client 主屏幕中为域管理活动打开“最近的知识库”列表中的某一知识库，则可能会出现以下错误  ：  
  
`"A configuration with name 'RecentList:KB:<domain>\<username>' already exists in the database."`  
  
此错误是由于 DQS 在 SQL Server 数据库和 C# 中采用不同的方法对字符串进行比较导致的。 SQL Server 数据库中的字符串比较不区分大小写，而在 C# 中则区分大小写。  
  
我们将用一个示例加以说明。 以用户 Domain\user1 为例。 该用户使用“user1”帐户登录到数据质量客户端计算机，并且对某个知识库进行处理。 DQS 将每个用户的最近知识库作为 DQS_MAIN 数据库的 A_CONFIGURATION 表的一条记录存储。 在此例中，将用以下名称存储该记录：RecentList:KB:Domain\user1. 之后，该用户以“User1”的身份登录到数据质量客户端计算机（请注意，U 为大写），并且尝试为域管理活动在“最近的知识库”列表中打开该知识库  。 DQS 中的基础代码将比较这两个字符串 RecentList:KB:DOMAIN\user1 和 DOMAIN\User1，并且在 C# 中考虑区分大小写的字符串比较，这两个字符串将不匹配，因此，DQS 将尝试为用户 (User1) 在 DQS_MAIN 数据库的 A_CONFIGURATION 表中插入一个新记录。 但是，由于在 SQL 数据库中采用不区分大小写的字符串比较，所以，该字符串在 DQS_MAIN 数据库的 A_CONFIGURATION 表中已存在，并且插入操作将失败。  
  
**解决方法：** 若要解决此问题，可以执行以下操作之一：  
  
-   通过运行下面的语句来验证是否存在重复的项：  
  
    ```  
    SELECT * FROM DQS_MAIN.dbo.A_CONFIGURATION WHERE NAME like 'RecentList%'  
    ```  
  
    接下来，可以运行以下语句（通过更改 WHERE 子句中的值以便匹配受影响的域和用户名）仅删除受影响用户的记录。  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%<domain>\<username>'  
    ```  
  
    或者，可以为 DQS 中的所有用户删除所有最近的项：  
  
    ```  
    DELETE DQS_MAIN.dbo.A_Configuration WHERE NAME LIKE 'RecentList%'  
    ```  
  
-   在登录到数据质量客户端计算机时使用相同的大小写作为最近的时间来指定您的用户帐户。  
  
> [!NOTE]  
> 若要解决此问题，请在登录到数据质量客户端计算机时使用一致的大小写规则来指定您的用户帐户。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="50-database-engine"></a><a name="DE"></a>5.0 数据库引擎  
  
### <a name="51-use-of-distributed-replay-controller-and-distributed-replay-client-features"></a>5.1 使用分布式重播控制器和分布式重播客户端功能  
**问题：** Windows Server 2008、Windows Server 2008 R2 和 Windows Server 7 的 Server Core SKU 中提供分布式重播控制器和分布式重播客户端功能，尽管 Server Core SKU 中不支持这两个功能。  
  
**解决方法：** 请勿在 Windows Server 2008、Windows Server 2008 R2 和 Windows Server 7 的 Server Core SKU 中安装或使用这两个功能。  
  
### <a name="52-sql-server-management-studio-depends-on-visual-studio-2010-sp1"></a>5.2 SQL Server Management Studio 依赖于 Visual Studio 2010 SP1  
**问题**：SQL Server 2012 Management Studio 必须依赖 Visual Studio 2010 SP1 才能正常工作。 卸载 Visual Studio 2010 SP1 可能会导致 SQL Server Management Studio 中的功能丧失并且使 Management Studio 处于不支持的状态。 在此情况下可能会出现以下问题：  
  
-   ssms.exe 的命令行参数将不会正常工作。  
  
-   在使用 /? 开关尝试运行 ssms.exe 时显示的帮助信息将不正确。  
  
-   对于通过在 Windows 资源管理器中双击打开的每个文件，将启动一个新的 SSMS 实例以便打开该文件。  
  
-   不能在正常的用户模式下调试查询。  
  
**解决方法**：再次安装 Visual Studio 2010 SP1 并重启 Management Studio。  
  
### <a name="53-x64-operating-systems-require-64-bit-powershell-20"></a>5.3 x64 操作系统要求 64 位 PowerShell 2.0  
**问题：** 64 位操作系统上的 SQL Server 2012 实例不支持安装 32 位的 Windows PowerShell Extensions for SQL Server。  
  
**解决方法：**  
  
-   将 64 位 SQL Server 2012 与 64 位管理工具和 64 位 Windows PowerShell Extensions for SQL Server 一起安装。  
  
-   或者，从 32 位 Windows PowerShell 2.0 提示符导入 SQLPS 模块。  
  
### <a name="54-an-error-might-occur-when-navigating-in-the-generate-script-wizard"></a>5.4 在生成脚本向导中浏览时可能会出错  
**问题：** 通过单击“保存或发布脚本”在生成脚本向导中生成脚本后，如果单击“选择选项”或“设置脚本编写选项”进行浏览，再次单击“保存或发布脚本”则可能会导致以下错误     ：  
  
```
An exception occurred while executing a Transact-SQL statement or batch. (Microsoft.SqlServer.ConnectionInfo)  
------------------------------  
ADDITIONAL INFORMATION:  
Invalid object name 'sys.federations'. (Microsoft SQL Server, Error: 208)
```
  
**解决方法：** 关闭后重新打开该生成脚本向导。  
  
### <a name="55-new-maintenance-plan-layout-not-compatible-with-earlier-sql-server-tools"></a>5.5 新的维护计划布局与早期的 SQL Server 工具不兼容  
**问题：** 在使用 SQL Server 2012 管理工具修改在以前版本的 SQL Server 管理工具（SQL Server 2008 R2、SQL Server 2008 或 SQL Server 2005）中创建的现有维护计划时，该维护计划以新格式保存。 早期版本的 SQL Server 管理工具不支持这个新格式。  
  
**解决方法**：无  
  
### <a name="56-intellisense-has-limitations-when-logged-in-to-a-contained-database"></a>5.6 在登录到包含的数据库时 Intellisense 具有限制  
问题：包含的用户登录到包含的数据库时，SQL Server Management Studio (SSMS) 和 SQL Server Data Tools (SSDT) 中的 Intellisense 无法按预期方式正常工作。 在此类情况下会出现以下行为：  
  
1.  针对无效对象的下划线未出现。  
  
2.  记忆式键入功能列表未出现。  
  
3.  针对内置功能的工具提示帮助不工作。  
  
**解决方法**：无  
  
### <a name="57-alwayson-availability-groups"></a>5.7 AlwaysOn 可用性组  
在尝试创建可用性组前，请参阅联机丛书中的 [AlwaysOn 可用性组的先决条件、限制和建议 (SQL Server)](https://go.microsoft.com/?linkid=9753168) (#alwayson-可用性组的先决条件、限制和建议-(sql-server))。 有关 AlwaysOn 可用性组的简介，请参阅联机丛书中的 [AlwaysOn 可用性组 (SQL Server)](https://go.microsoft.com/?linkid=9753166)(#alwayson-可用性组-(sql-server))。  
  
#### <a name="571-client-connectivity-for-alwayson-availability-groups"></a>5.7.1 AlwaysOn 可用性组的客户端连接性  
**更新时间：** 2012 年 8 月 13 日  
  
本节介绍针对 AlwaysOn 可用性组的驱动程序支持以及不支持的驱动程序的解决方法。  
  
**驱动程序支持**  
  
下表汇总了针对 AlwaysOn 可用性组的驱动程序支持：  
  
|驱动程序|多子网故障转移|应用程序意向|只读路由|多子网故障转移：更快的单子网终结点故障转移|多子网故障转移：SQL 群集实例的命名实例解析|  
|----------|--------------------------|----------------------|----------------------|------------------------------------------------------------------|---------------------------------------------------------------------------------|  
|SQL Native Client 11.0 ODBC|是|是|是|是|是|  
|SQL Native Client 11.0 OLEDB|否|是|是|否|否|  
|ADO.NET（结合使用 .NET Framework 4.0 和连接性修补程序 **\&#42;** ）|是|是|是|是|是|  
|ADO.NET（结合使用 .NET Framework 3.5 SP1 和连接性修补程序 **\&#42;\&#42;** ）|是|是|是|是|是|  
|Microsoft JDBC driver 4.0 for SQL Server|是|是|是|是|是|  
  
\&#42; 下载 ADO .NET（结合使用 .NET Framework 4.0）的连接性修补程序：[https://support.microsoft.com/kb/2600211](https://support.microsoft.com/kb/2600211)  。  
  
\&#42;\&#42; 下载 ADO .NET（结合使用 .NET Framework 3.5 SP1）的连接性修补程序：[https://support.microsoft.com/kb/2654347](https://support.microsoft.com/kb/2654347)  。  
  
**MultiSubnetFailover 关键字和相关功能**  
  
MultiSubnetFailover 是 SQL Server 2012 中用于允许使用 AlwaysOn 可用性组和 AlwaysOn 故障转移群集实例进行更快故障转移的新连接字符串关键字。 在连接字符串中设置 MultiSubnetFailover=True 时，启用以下三个子功能：  
  
-   更快进行多子网故障转移到 AlwaysOn 可用性组或故障转移群集实例的多子网侦听器。  
  
    -   多子网 AlwaysOn 故障转移群集实例的命名实例解析。  
  
-   更快进行单子网故障转移到 AlwaysOn 可用性组或故障转移群集实例的单子网侦听器。  
  
    -   当连接到具有单个子网中的单个 IP 的侦听器时使用此功能。 这将进行更频繁的 TCP 连接重试以加快单子网故障转移。  
  
-   多子网 AlwaysOn 故障转移群集实例的命名实例解析。  
  
    -   这将添加对具有多子网端点的 AlwaysOn 故障转移群集实例的命名实例解析支持。  
  
**NET Framework 3.5 或 OLEDB 不支持 MultiSubnetFailover=True**  
  
**问题：** 如果你的可用性组或故障转移群集实例具有取决于不同子网的多个 IP 地址的侦听器名称（在 WSFC 群集管理器中称作网络名称或客户端访问点），并且你将 ADO.NET 用于 .NET Framework 3.5SP1 或 SQL Native Client 11.0 OLEDB，则可能你对可用性组侦听器的 50% 的客户端连接请求都将遇到连接超时。  
  
**解决方法：** 我们建议你执行以下任务之一。  
  
-   如果您无权操作群集资源，则将连接超时更改为 30 秒（该值导致 20 秒的 TCP 超时期加上 10 秒的缓冲）。  
  
    优点  ：如果发生跨子网故障转移，则客户端恢复时间将比较短。  
  
    缺点  ：半数的客户端连接将需要 20 多秒  
  
-   如果你有权操作群集资源，则更强烈推荐的方法是将你的可用性组侦听器的网络名称设置为 **RegisterAllProvidersIP**=0。 有关详细信息，请参阅本部分后面的“用于禁用 RegisterAllProvidersIP 和减少 TTL 的示例 PowerShell 脚本”。  
  
    优点  ：无需增加客户端连接超时值。  
  
    缺点  ：如果发生跨子网故障转移，则客户端恢复时间可能为 15 分钟或更长，具体时间取决于 HostRecordTTL 设置以及跨站点 DNS/AD 复制计划的设置。  
  
**用于禁用 RegisterAllProvidersIP 和减少 TTL 的示例 PowerShell 脚本**  
  
下面的示例 PowerShell 脚本阐释如何禁用 `RegisterAllProvidersIP` 和减少 TTL。 使用要更改的侦听器名称替换 `yourListenerName` 。  
  
```  
Import-Module FailoverClusters  
Get-ClusterResource yourListenerName|Set-ClusterParameter RegisterAllProvidersIP 0  
Get-ClusterResource yourListenerName|Set-ClusterParameter HostRecordTTL 300  
```  
  
#### <a name="572-upgrading-from-ctp3-with-availability-group-configured-is-not-supported"></a>5.7.2 不支持从配置了可用性组的 CTP3 升级  
在升级前删除该可用性组然后重新创建它。 这是由于 CTP3 内部版本的限制造成的。 将来的内部版本将不再有这样的限制。  
  
#### <a name="573-side-by-side-installation-of-ctp3-with-later-versions-is-not-supported-if-you-have-an-availability-group-configured-in-your-instance"></a>5.7.3 如果在实例中配置了可用性组，则不支持并行安装 CTP3 和更高版本  
这是由于 CTP3 内部版本的限制造成的。 将来的内部版本将不再有这样的限制。  
  
#### <a name="574-side-by-side-installation-of-ctp3-with-later-versions-of-failover-cluster-instances-is-not-supported"></a>5.7.4 不支持并行安装 CTP3 与故障转移群集实例的更高版本。  
这是由于 CTP3 内部版本的限制造成的。 将来的内部版本将不再有这样的限制。 若要从 CTP3 升级故障转移群集实例，请确保同时在节点上升级所有实例。  
  
#### <a name="575--timeouts-may-occur-when-using-multi-ips-in-the-same-subnet-with-alwayson"></a>5.7.5 在将多个 IP 与 AlwaysOn 一起用于同一个子网中时可能会发生超时  
**问题：** 在将多个 IP 与 AlwaysOn 一起用于同一个子网中时，客户有时候可能会看到超时。 如果列表中最顶部的 IP 是错误的，则可能会发生此情况。  
  
**解决方法：** 在连接字符串中使用“multisubnetfailover = true”。  
  
#### <a name="576-failure-to-create-new-availability-group-listeners-because-of-active-directory-quotas"></a>5.7.6 因 Active Directory 配额未能新建可用性组侦听器  
**问题：** 新的可用性组侦听器可能在创建时失败，因为您已经达到参与群集节点计算机帐户的 Active Directory 配额。 有关详细信息，请参阅 [如何在群集服务帐户修改计算机对象时排除其故障](https://support.microsoft.com/kb/307532) (#如何在群集服务帐户修改计算机对象时排除其故障)和 [Active Directory 配额](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc904295(v=ws.10))(#active.directory-配额)。  
  
#### <a name="577-netbios-conflicts-because-availability-group-listener-names-use-an-identical-15-character-prefix"></a>5.7.7 NetBIOS 发生冲突，因为可用性组侦听器名称使用相同的 15 字符前缀  
如果您的两个 WSFC 群集均由同一 Active Directory 控制，而您试图使用超过 15 个字符的名称（具有相同的 15 字符前缀）在这两个群集中创建可用性组侦听器，此时您将收到错误，报告无法使虚拟网络名称资源联机。 有关 DNS 名称的前缀命名规则的信息，请参阅 [分配域名](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731265(v=ws.10))(#分配域名)。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="60-integration-services"></a><a name="IS"></a>6.0 Integration Services  
  
### <a name="61-the-change-data-capture-service-for-oracle-and-the-change-data-capture-designer-console-for-oracle"></a>6.1 Change Data Capture Service for Oracle 和 Change Data Capture Designer Console for Oracle  
Oracle CDC 服务是一种 Windows 服务，该服务将扫描 Oracle 事务日志并将对有关 Oracle 表的更改捕获到 SQL Server 更改表中。 CDC 设计器控制台用于开发和维护 Oracle CDC 实例。 CDC 设计器控制台是一种 Microsoft 管理控制台 (MMC) 管理单元。  
  
#### <a name="611-install-the-cdc-service-for-oracle-and-the-cdc-designer-for-oracle"></a>6.1.1 安装 CDC Service for Oracle 和 CDC Designer for Oracle  
**问题：** 该 CDC 服务和 CDC 设计器不是由 SQL Server 安装程序安装的。 您必须在满足更新的帮助文件中所述的要求和先决条件的计算机上安装 CDC 服务或 CDD 设计器。  
  
**解决方法：** 若要安装 Oracle CDC 服务，请从 SQL Server 安装介质手动运行 AttunityOracleCdcService.msi。 若要安装 CDC 设计器控制台，请从 SQL Server 安装介质手动运行 AttunityOracleCdcDesigner.msi。  用于 x86 和 x64 的安装包位于 SQL Server 安装介质上的 .\Tools\AttunityCDCOracle\ 中。  
  
#### <a name="612-f1-help-functionality-points-to-incorrect-documentation-files"></a>6.1.2 F1 帮助功能指向错误的文档文件  
**问题：** 你不能通过使用 F1 帮助下拉列表或者在 Attunity 控制台中单击“?”来访问正确的帮助文档。 这些方法指向错误的 chm 文件。  
  
**解决方法：** 在安装适用于 Oracle 的 CDC Service 和适用于 Oracle 的 CDC Designer 时将安装正确的 chm 文件。 若要查看正确的帮助内容，请从以下位置直接启动 chm 文件： `%Program Files%\Change Data Capture for Oracle by Attunity\*.chm`。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="70-master-data-services"></a><a name="MDS"></a>7.0 Master Data Services  
  
### <a name="71-fixing-an-mds-installation-in-a-cluster"></a>7.1 在群集中修复 MDS 安装  
**问题：** 如果在安装 SQL Server 2012 的 RTM 版本的群集实例时选中了“Master Data Services”复选框，MDS 则将安装在单个节点上，但它将不可用并且在你添加到群集的附加节点上将不起作用  。  
  
**解决方法**：要解决此问题，必须执行以下步骤安装 SQL Server 2012 Cumulative Release 1 (CU1)：  
  
1.  确保不存在 SQL/MDS 安装。  
  
2.  将 SQL Server 2012 CU1 下载到本地目录。  
  
3.  在主群集节点上安装包含 MDS 功能的 SQL Server 2012，然后在任何附加群集节点上安装包含 MDS 功能的 SQL Server 2012。  
  
有关这些问题的详细信息以及有关如何执行上述步骤的信息，请参阅 [https://support.microsoft.com/kb/2683467](https://support.microsoft.com/kb/2683467)。  
  
### <a name="72-microsoft-silverlight-5-required"></a>7.2 需要 Microsoft Silverlight 5  
在使用主数据管理器 Web 应用程序时，Silverlight 5.0 必须安装在客户端计算机上。 如果您不具有所需版本的 Silverlight，则在您导航到需要 Silverlight 的 Web 应用程序区域时，系统将提示您安装 Silverlight。 可以从 [https://go.microsoft.com/fwlink/?LinkId=243096](https://go.microsoft.com/fwlink/?LinkId=243096) 安装 Silverlight 5。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="80-reporting-services"></a><a name="RS"></a>8.0 Reporting Services  
  
### <a name="81-reporting-services-connectivity-to-sql-server-pdw-requires-updated-drivers"></a>8.1 Reporting Services 与 SQL Server PDW 连接要求更新的驱动程序  
从 SQL Server 2012 Reporting Services 连接到 Microsoft SQL Server PDW 应用程序更新 2 和更高版本要求更新 PDW 连接驱动程序。 有关详细信息，SQL Server PDW 客户应该与 Microsoft 支持部门联系。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="90-streaminsight"></a><a name="SI"></a>9.0 StreamInsight  
SQL Server 2012 包含 StreamInsight 2.1。 StreamInsight 2.1 要求 Microsoft SQL Server 2012 许可证和 .NET Framework 4.0。 它还包含了许多性能改进和几个错误修补程序。 有关详细信息，请参阅 [Microsoft StreamInsight 2.1 发行说明](https://social.technet.microsoft.com/wiki/contents/articles/6539.aspx)。 若要单独下载 StreamInsight 2.1，请访问 Microsoft 下载中心上的 [Microsoft StreamInsight 2.1 下载页](https://www.microsoft.com/download/details.aspx?id=30149)。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")  
  
## <a name="100-upgrade-advisor"></a><a name="UA"></a>10.0 升级顾问  
  
### <a name="101-link-to-install-upgrade-advisor-is-not-enabled-on-chinese-hk-operating-systems"></a>10.1 指向安装升级顾问的链接在中文 (HK) 操作系统上未启用  
问题：尝试在中文（香港特别行政区）操作系统 (OS) 支持的任何 Windows 版本上安装升级顾问时，可能会发现用于安装升级顾问的链接未启用。  
  
**解决方法**：找到 SQLUA.msi 文件（在你的 SQL Server 2012 介质的 `\1028_CHT_LP\x64\redist\Upgrade Advisor` 或 `\1028_CHT_LP\x86\redist\Upgrade Advisor` 位置，具体取决于你的操作系统体系结构）  。  
  
![horizontal_bar](media/horizontal-bar.png "horizontal_bar")
