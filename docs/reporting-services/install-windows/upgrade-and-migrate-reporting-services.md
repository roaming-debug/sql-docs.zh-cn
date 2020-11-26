---
description: Upgrade and Migrate Reporting Services
title: 升级和迁移 Reporting Services | Microsoft Docs
ms.prod: reporting-services
ms.prod_service: reporting-services-native
helpviewer_keywords:
- SSRS, upgrading
- Reporting Services, upgrades
- SQL Server Reporting Services, upgrading
- upgrading Reporting Services
author: maggiesMSFT
ms.author: maggies
ms.topic: conceptual
ms.date: 05/01/2020
ms.openlocfilehash: f473590243956cd2fcba1961d3580fa052d6f4c1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91934627"
---
# <a name="upgrade-and-migrate-reporting-services"></a>Upgrade and Migrate Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

  本主题概述 SQL Server Reporting Services 的升级和迁移选项。 有几种用于升级 SQL Server Reporting Services 部署的常规方法：  
 
- 从 Reporting Services 2016 及更低版本升级到 Reporting Services 2016 及更低版本   ：升级服务器和实例上当前安装的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 组件。 这通常称为“就地”升级。 对于从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务器的一种模式升级到另一模式，不支持就地升级。 例如，不能将本机模式报表服务器升级到 SharePoint 模式报表服务器。 您可以将报表项从一个模式迁移到另一个模式。 有关详细信息，请参阅本文档后面的 [SharePoint 模式升级和迁移方案](#bkmk_sharePoint_scenarios)部分。  

- 从 Reporting Services 2016 及更低版本升级到 Reporting Services 2017 及更高版本是与以前版本中不同的升级方案   。 升级到 Reporting Services 2016 及更低版本时  ，可以使用 SQL Server 安装介质执行就地升级过程。 从 Reporting Services 2016 及更低版本升级到 Reporting Services 2017 及更高版时   ，无法执行相同的步骤，因为新的 Reporting Services 安装是独立产品。 它不再是 SQL Server 安装介质的一部分。 

    若要从 Reporting Services 2016 及更低版本升级到 Reporting Services 2017 及更高版本，请遵循[迁移 Reporting Services 安装（本机模式）](migrate-a-reporting-services-installation-native-mode.md)一文（将 Reporting Services 2017 或更高版本作为目标实例）。 

- 从 Reporting Services 2017 升级到未来版本同样是就地升级方案，因为产品安装 GUID 相同  。 运行 SQLServerReportingServices.exe 安装文件以在当前安装 Reporting Services 的服务器上开始就地升级。
  
- **迁移**：安装并配置新 SharePoint 环境，并将报表项和资源复制到此新环境中，然后将此环境配置为使用现有内容。 迁移的较低级别形式是复制 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 数据库、配置文件，如果使用的是 SharePoint 模式，则还包括复制 SharePoint 内容数据库。  


> [!NOTE]
> 自 SQL Server 2016 之后，不提供 Reporting Services 与 SharePoint 的集成这一功能。
   
##  <a name="known-upgrade-issues-and-best-practices"></a><a name="bkmk_known_issues"></a> 已知的升级问题和最佳做法  
 有关可以升级的受支持版本的详细列表，请参阅 [Supported Version and Edition Upgrades](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)。  
  
> [!TIP]  
>  有关 SQL Server 的问题相关的最新信息，请参阅 [SQL Server 2016 发行说明](https://go.microsoft.com/fwlink/?LinkID=398124)。  
  
  
##  <a name="side-by-side-installations"></a><a name="bkmk_side_by_side"></a> 并行安装  
 SQL Server Reporting Services 本机模式可与 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 本机模式部署并行安装。  
  
 不支持并行部署 SharePoint 模式下的 SQL Server Reporting Services 和任何先前版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式组件。  
  
  
##  <a name="in-place-upgrade"></a><a name="bkmk_inplace_upgrade"></a> 就地升级  
 升级由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序完成。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序可用于升级任意或所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 组件，包括 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]。 安装程序将检测现有实例并提示您进行升级。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序提供了升级选项，您可以将其作为命令行参数指定或在安装向导中指定。  
  
 运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装程序时，可以选择从以下版本之一升级，也可以安装运行现有并行安装的 SQL Server Reporting Services 的新实例：  
  
-   [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
-   [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
-   [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
-   [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]  
  
 有关 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的详细信息，请参阅以下内容：  

* [升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server.md)
* [使用安装向导（安装程序）升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)
* [从命令提示符安装 SQL Server 2016](../../database-engine/install-windows/install-sql-server-2016-from-the-command-prompt.md)
  
  
##  <a name="pre-upgrade-checklist"></a><a name="bkmk_upgrade_checklist"></a> 升级准备一览表  
 升级到 SQL Server Reporting Services 之前，查看以下内容：  
  
-   检查相关要求，以确定硬件和软件是否可以支持 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]。 有关详细信息，请参阅 [安装 SQL Server 2016 的硬件和软件要求](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)。  
  
-   使用系统配置检查器 (SCC) 扫描报表服务器计算机中是否有可能妨碍 SQL Server Reporting Services 成功安装的任何情况。 有关详细信息，请参阅 [Check Parameters for the System Configuration Checker](../../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)。  
  
-   查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的安全最佳实践和指南。 有关详细信息，请参阅 [Security Considerations for a SQL Server Installation](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)。  
  
-   备份对称密钥。 有关详细信息，请参阅 [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)。  
  
-   备份报表服务器数据库和配置文件。 有关详细信息，请参阅 [Backup and Restore Operations for Reporting Services](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md)。  
  
-   备份对 IIS 中现有 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 虚拟目录的任何自定义。  
  
-   删除无效的 TLS/SSL 证书。  这包括在升级 Reporting Services 前未计划更新的到期证书。  无效证书会导致升级失败，且如下所示的错误消息会写入 Reporting Services 日志文件：**Microsoft.ReportingServices.WmiProvider.WMIProviderException:未在网站上配置安全套接字层(SSL)证书。**  
  
 在升级生产环境之前，务必在与生产环境具有相同配置的生产前环境中运行测试升级。  
  
  
## <a name="overview-of-migration-scenarios"></a>迁移方案概述  
 如果从 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的受支持版本升级到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，则通常可以运行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装向导来升级报表服务器程序文件、数据库以及所有应用程序数据。  
  
 然而，如果遇到以下任何情况，都需要手动 **迁移** 报表服务器安装：  
  
-   您想要更改部署中使用的报表服务器的类型。 例如，不能将本机模式报表服务器升级或转换到 SharePoint 模式。 有关详细信息，请参阅[从本机迁移到 SharePoint (SSRS)](../../reporting-services/install-windows/native-to-sharepoint-migration-ssrs.md)。  
  
-   您需要在升级过程中最大限度地减少报表服务器的脱机时间。 在您将内容数据复制到新报表服务器实例，并在不改变现有报表服务器安装状态的情况下测试该安装时，当前安装将保持联机状态。  
  
-   要将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的 SharePoint 2010 部署迁移到 SharePoint 2013/2016。 SharePoint 2013/2016 不支持从 SharePoint 2010 就地升级。 有关详细信息，请参阅[迁移 Reporting Services 安装（SharePoint 模式）](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)。  
  
  
##  <a name="native-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_native_scenarios"></a> 本机模式升级和迁移方案  
 **升级：** 本机模式的就地升级与本主题前面列出的每个受支持版本的升级过程相同。 运行 SQL Server 安装向导或命令行安装。 在安装后，报表服务器数据库将自动升级到新的报表服务器数据库架构。 有关详细信息，参阅本主题中的 [就地升级](#bkmk_inplace_upgrade) 部分。  
  
 当选择了一个要升级的现有报表服务器实例时，升级过程即开始。  
  
1.  如果报表服务器数据库位于远程计算机上，而您没有更新该数据库的权限，则安装程序将提示您提供更新远程报表服务器数据库的凭据。 请确保提供具有 **sysadmin** 或数据库更新权限的凭据。  
  
2.  安装程序检查阻止升级的条件或设置并读取配置设置。 示例包括在报表服务器上部署的自定义扩展插件。 如果升级受阻，则必须修改安装以便不再阻止升级，或者迁移到新的 SQL Server Reporting Services 实例。 有关详细信息，请参阅升级顾问文档。  
  
3.  如果升级可以继续，则安装程序将提示您继续升级过程。  
  
4.  安装程序为 SQL Server Reporting Services 程序文件创建新的文件夹。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装的程序文件夹包括 MSRS13.\<*instance name*>。  
  
5.  安装程序将添加 SQL Server Reporting Services 报表服务器程序文件、配置工具和作为报表服务器功能一部分的命令行实用工具。  
  
    1.  删除先前版本中的程序文件。  
  
    2.  升级到新版本的报表服务器配置工具和实用工具包括本机模式 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具、命令行实用工具（如 RS.exe）和报表生成器。  
  
    3.  其他客户端工具（如 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ）是单独下载，需要单独升级。 有关详细信息，请参阅 [下载 SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)。
  
    4.  [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 为单独下载。 有关详细信息，请参阅 [Visual Studio 2015 中的 SQL Server Data Tools](https://msdn.microsoft.com/mt186501)。  
  
6.  安装程序将重用服务控制管理器中 SQL Server Reporting Services 报表服务器服务的服务条目。 此服务条目包括报表服务器 Windows 服务帐户。  
  
7.  安装程序基于 IIS 中的现有虚拟目录设置保留新的 URL。 安装程序可能不会删除 IIS 中的虚拟目录，所以请确保在完成升级之后手动删除它们。  
  
8.  安装程序合并配置文件中的设置。 添加新条目时，使用当前安装的配置文件作为基础。 不会删除过时的条目，但是在升级完成后，报表服务器不会再读取它们。 升级不会删除旧日志文件、过时的 RSWebApplication.config 文件或 IIS 中的虚拟目录设置。 升级不会删除 Report Designe、Management Studio 或其他客户端工具的旧版本。 如果不再需要它们，请确保在升级完成后删除这些文件和工具。  
  
 **迁移：** 将旧版本机模式安装迁移到 SQL Server Reporting Services 与本主题前面列出的每个受支持版本的迁移步骤相同。 有关详细信息，请参阅 [迁移 Reporting Services 安装（本机模式）](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)  
  
  
##  <a name="upgrade-a-reporting-services-native-mode-scale-out-deployment"></a><a name="bkmk_native_scaleout"></a> 升级 Reporting Services 本机模式扩展部署  
 下面概述了如何升级扩展为多个报表服务器的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式部署。 此过程需要 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 部署停机：  
  
1.  备份报表服务器数据库和加密密钥。 有关详细信息，请参阅 [Reporting Services 的备份和还原操作](../../reporting-services/install-windows/backup-and-restore-operations-for-reporting-services.md)和[添加和删除横向扩展部署的加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)。  
  
2.  使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器，从扩展部署中删除所有报表服务器。 有关详细信息，请参阅[配置本机模式报表服务器横向扩展部署（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)。  
  
3.  将其中一台报表服务器升级为 SQL Server Reporting Services。  
  
4.  使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器，将报表服务器重新添加到扩展部署。 有关详细信息，请参阅[配置本机模式报表服务器横向扩展部署（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md)。  
  
     对于每个服务器，重复升级和扩展步骤。  
  
##  <a name="sharepoint-mode-upgrade-and-migration-scenarios"></a><a name="bkmk_sharePoint_scenarios"></a> SharePoint 模式升级和迁移方案  
 以下各节介绍从指定版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式升级或迁移到 SQL Server Reporting Services [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式时可能会遇到的问题和所需的基本步骤。  
  
 有两种升级 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式部署的安装组件。  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 共享服务。  
  
    > [!TIP]  
    >  使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint cmdlet `Get-SPRSServiceApplicationServers` 确定 SharePoint 场中当前运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 共享服务并因此需要升级的服务器。  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 用于 SharePoint 产品的外接程序。 有关详细信息，请参阅 [安装或卸载适用于 SharePoint 的 Reporting Services 外接程序](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)。  
  
 有关迁移 SharePoint 模式安装的详细步骤，请参阅[迁移 Reporting Services 安装（SharePoint 模式）](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)。  
  
> [!IMPORTANT]  
>  由于需要升级不同技术，下面的某些方案将需要将 SharePoint 环境停止运行。 如果您的情况不允许停机，则需要完成迁移，而非就地升级。  
  
### <a name="sssql14-to-sql-server-reporting-services"></a>[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 到 SQL Server Reporting Services  
 **起始环境：** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP1、SharePoint 2010 或 SharePoint 2013。  
  
 **结束环境：** SQL Server Reporting Services、SharePoint 2013 或 SharePoint 2016。   
  
-   **SharePoint 2013/2016：** SharePoint 2013/2016 不支持从 SharePoint 2010 就地升级。 但是支持 **数据库附加升级**  过程。
  
     如果您具有与 SharePoint 2010 相集成的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装，则不能就地升级 SharePoint 服务器。 不过，可以将内容数据库和服务应用程序数据库从 SharePoint 2010 场迁移到 SharePoint 2013/2016 场。  
  
### <a name="sssql11-to-sql-server-reporting-services"></a>[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 到 SQL Server Reporting Services  
 **起始环境：** [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或 [!INCLUDE[ssSQL11SP1](../../includes/sssql11sp1-md.md)]、SharePoint 2010。  
  
 **结束环境：** SQL Server Reporting Services、SharePoint 2013 或 SharePoint 2016。   
  
-   **SharePoint 2013/2016：** SharePoint 2013/2016 不支持从 SharePoint 2010 就地升级。 但是支持 **数据库附加升级**  过程。
  
     如果您具有与 SharePoint 2010 相集成的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装，则不能就地升级 SharePoint 服务器。 不过，可以将内容数据库和服务应用程序数据库从 SharePoint 2010 场迁移到 SharePoint 2013/2016 场。  
  
### <a name="sskilimanjaro-to-sql-server-reporting-services"></a>[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 到 SQL Server Reporting Services  
 **起始环境：** [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、SharePoint 2010。  
  
 **结束环境：** SQL Server Reporting Services、SharePoint 2013 或 SharePoint 2016。  
 
-   **SharePoint 2013/2016：** SharePoint 2013/2016 不支持从 SharePoint 2010 就地升级。 但是支持 **数据库附加升级**  过程。

    必须先迁移 SharePoint，然后才能升级 Reporting Services。
  
-   在场中的每个 Web 前端上安装用于 SharePoint 的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序的 SQL Server Reporting Services 版本。 可以通过使用 SQL Server Reporting Services 安装向导或通过下载外接程序安装该外接程序。  
  
-   运行 SQL Server Reporting Services 安装以升级每个“报表服务器”的 SharePoint 模式。 SQL Server 安装向导会安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务并创建新的服务应用程序。 
  
  
##  <a name="considerations-for-a-migration"></a><a name="bkmk_migration_considerations"></a> 迁移注意事项  
 移动应用程序数据时，应注意下列事项和约束：  
  
-   加密密钥的保护包括一个合并计算机标识的哈希。  
  
-   报表服务器数据库名称将固定，并且无法在新计算机上重命名。  
  
### <a name="encryption-key-considerations"></a>加密密钥注意事项  
 将报表服务器数据库移到新计算机之前始终备份加密密钥。  
  
 将报表服务器安装移到另一台计算机会使保护加密密钥（这些加密密钥用于为报表服务器数据库中存储的敏感数据提供安全保障）的哈希无效。 使用该数据库的每个报表服务器实例都有其加密密钥副本，在当前计算机对其定义时将使用服务帐户的标识对其进行加密。 如果更改计算机，则即使在新计算机上使用同一帐户名称，服务也无法访问其密钥。  
  
 若要在新的报表服务器计算机上重新建立可逆加密，必须还原先前备份的密钥。 报表服务器数据库中存储的完整密钥集由对称密钥值和服务标识信息组成，后者用于限制密钥的访问，使密钥仅可供存储它的报表服务器实例使用。 在密钥还原过程中，报表服务器将用密钥的新版本替换密钥的现有副本。 新版本包括当前计算机上定义的计算机标识值和服务标识值。 有关详情，请参阅以下主题：  
  
-   SharePoint 模式：请参阅[管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)的“密钥管理”部分  
  
-   本机模式：请参阅[备份和还原 Reporting Services 加密密钥](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)  
  
  
### <a name="fixed-database-name"></a>固定数据库名称  
 不能重命名报表服务器数据库。 创建数据库时，数据库的标识将记录在报表服务器存储过程中。 重命名报表服务器主数据库或临时数据库会在过程运行时导致出现错误，从而使报表服务器安装无效。  
  
 如果现有安装的数据库名称不适合新安装，应考虑创建一个具有您所选名称的新数据库，然后使用下面列出的技术加载现有应用程序数据：  
  
-   编写调用报表服务器 Web 服务 SOAP 方法的 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 脚本，以在各数据库之间复制数据。 可以使用 RS.exe 实用工具来运行该脚本。 有关这种方法的详细信息，请参阅 [脚本编写和带 Reporting Services 的 PowerShell](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md)。  
  
-   编写调用 WMI 提供程序的代码，以在各数据库之间复制数据。 有关这种方法的详细信息，请参阅 [访问 Reporting Services WMI 提供程序](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md)。  
  
-   如果仅有少量项，则可以将报表和共享数据源从报表设计器、模型设计器和报表生成器重新发布到新的报表服务器。 必须重新创建角色分配、订阅、共享计划、报表快照计划、对报表或其他项设置的自定义属性、模型项安全性以及对报表服务器设置的属性。 您将丢失报表历史记录和报表执行日志数据。  
  
  
##  <a name="additional-resources"></a><a name="bkmk_additional_resources"></a> 其他资源  
  
> [!NOTE]  
>  有关 SharePoint 数据库附加升级的详细信息，请参阅下列文章：  
  
-   [升级到 SharePoint 2016 的过程概述](https://technet.microsoft.com/library/cc262483\(v=office.16\))。

-   [升级到 SharePoint 2013 的过程概述](https://go.microsoft.com/fwlink/p/?LinkId=256688)。
  
-   [升级到 SharePoint 2013 之前的清除准备](https://go.microsoft.com/fwlink/p/?LinkId=256689)。  
  
-   [将数据库从 SharePoint 2013 升级到 SharePoint 2016](https://technet.microsoft.com/library/cc303436\(v=office.16\))。

-   [将数据库从 SharePoint 2010 升级到 SharePoint 2013](https://go.microsoft.com/fwlink/p/?LinkId=256690)。  

## <a name="next-steps"></a>后续步骤

[升级报表](../../reporting-services/install-windows/upgrade-reports.md)   
[使用安装向导（安装程序）升级到 SQL Server 2016](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
