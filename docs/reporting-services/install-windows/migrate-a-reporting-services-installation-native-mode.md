---
description: 迁移 Reporting Services 安装（本机模式）
title: 迁移 Reporting Services 安装（本机模式）| Microsoft Docs
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.date: 05/01/2020
ms.openlocfilehash: ae3932611181f99b71d72bc2365967a19c968b9c
ms.sourcegitcommit: f2bdebed3efa55a2b7e64de9d6d9d9b1c85f479e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "96125016"
---
# <a name="migrate-a-reporting-services-installation-native-mode"></a>迁移 Reporting Services 安装（本机模式）

本主题提供有关将以下支持的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式部署版本之一迁移到新的 SQL Server Reporting Services 实例的分步说明：  
  
::: moniker range=">=sql-server-2017||=sqlallproducts-allversions"
* [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]

* [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
* [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
* [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
* [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
::: moniker-end

::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
* [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
* [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
* [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
* [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]

有关迁移 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式部署的信息，请参阅 [迁移 Reporting Services 安装（SharePoint 模式）](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)。  

::: moniker-end
  
 迁移是指将应用程序数据文件移到新的 SQL Server 实例。 下面是必须迁移您的安装的常见原因：  
  
* 您具有大规模部署或正常运行时间要求。  
  
* 您在更改安装的硬件或拓扑。  
  
* 您遇到了阻止升级的问题。

## <a name="native-mode-migration-overview"></a><a name="bkmk_nativemode_migration_overview"></a> 本机模式迁移概述

 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的迁移过程包括手动步骤和自动步骤。 报表服务器迁移包括以下任务：  
  
* 备份数据库、应用程序和配置文件。  
  
* 备份加密密钥。  
  
* 安装 SQL Server 的新实例。 如果使用相同硬件，且现有安装是受支持的版本之一，则可以并行安装 SQL Server。  
  
    > [!TIP]  
    >  并行安装可能需要将 SQL Server 作为命名实例安装。
  
* 将报表服务器数据库和其他应用程序文件从现有安装移到新的 SQL Server 安装中。  
  
* 将任何自定义应用程序文件移到新安装中。  
  
* 配置报表服务器。  
  
* 编辑 **RSReportServer.config** ，使其包括先前安装中的任何自定义设置。  
  
* 或者，为新的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Windows 服务组配置自定义访问控制列表 (ACL)。  
  
* 在确认新实例完全正常之后，删除未使用的应用程序和工具。  
  
 对于承载报表服务器数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本存在限制。 如果您在重复使用在以前安装中已创建的报表服务器数据库，请查看以下文章。  
  
* [创建报表服务器数据库](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
  
## <a name="fixed-database-name"></a><a name="bkmk_fixed_database_name"></a> 固定数据库名称

 不能重命名报表服务器数据库。 创建数据库时，数据库的标识将记录在报表服务器存储过程中。 重命名报表服务器主数据库或临时数据库会在过程运行时导致错误，从而使报表服务器安装无效。  
  
 如果现有安装的数据库名称不适合新安装，应考虑创建一个具有名称的新数据库，然后使用下面列出的技术加载现有应用程序数据：  
  
* 编写调用报表服务器 Web 服务 SOAP 方法的 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 脚本，以在各数据库之间复制数据。 可以使用 RS.exe 实用工具来运行该脚本。 有关这种方法的详细信息，请参阅 [脚本编写和带 Reporting Services 的 PowerShell](../../reporting-services/tools/scripting-and-powershell-with-reporting-services.md)。  
  
* 编写调用 WMI 提供程序的代码，以在各数据库之间复制数据。 有关这种方法的详细信息，请参阅 [访问 Reporting Services WMI 提供程序](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md)。  
  
* 如果仅有少量项，则可以将报表和共享数据源从报表设计器、模型设计器和报表生成器重新发布到新的报表服务器。 重新创建角色分配、订阅、共享计划、报表快照计划、对报表或其他项设置的自定义属性、模型项安全性以及对报表服务器设置的属性。 如果执行这些操作，请做好丢失报表历史记录和报告执行日志数据的准备。
  
## <a name="before-you-start"></a><a name="bkmk_before_you_start"></a> 开始之前

 即使要迁移（而不是升级）安装，也要考虑针对现有的安装运行升级顾问以帮助确定可能会影响迁移的任何问题。 如果要迁移尚未安装或配置的报表服务器，则该步骤尤其有用。 通过运行升级顾问，可以查明新 SQL Server 安装可能不支持的自定义设置。  
  
 此外，应当注意 SQL Server Reporting Services 中进行了多项影响安装迁移方式的重要改动：

* 新的 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 替代了报表管理器。
  
* 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]开始，IIS 不再是必备组件。 如果要将报表服务器安装迁移到新计算机上，则无需添加 Web 服务器角色。 另外，URL 和身份验证的配置步骤不同于以前的版本，用来诊断和解决问题的方法和工具也是如此。  
  
* 报表服务器 Web 服务、 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)]和报表服务器 Windows 服务使用同一帐户运行。 所有这三个应用程序都从 RSReportServer.config 文件中读取配置设置。
  
* [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 和 SQL Server Management Studio 设计用于删除重叠功能。 每个工具支持不同的一组任务。
  
* 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 及更高版本中不支持 ISAPI 筛选器。 如果使用 ISAPI 筛选器，则必须在迁移之前重新设计报表解决方案。  
  
* 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 及更高版本中不支持 IP 地址限制。 如果使用 IP 地址限制，则必须在迁移之前重新设计报表解决方案，或使用诸如防火墙、路由器或网络地址转换 (NAT) 等技术来配置被禁止访问报表服务器的地址。  
  
* [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 及更高版本不支持客户端传输层安全性 (TLS)（旧称为“安全套接字层 (SSL)”）证书。 如果使用客户端 TLS 证书，必须在迁移前重新设计报表解决方案。  
  
* 如果使用 Windows 集成身份验证之外的身份验证类型，则必须将 RSReportServer.config 文件中的 `<AuthenticationTypes>` 元素更新为支持的身份验证类型  。 支持的身份验证类型包括 NTLM、Kerberos、Negotiate 和 Basic。 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 及更高版本中不支持匿名、.NET Passport 和摘要式身份验证。  
  
* 如果在报表环境中使用自定义级联样式表，则不能迁移这些样式表。 迁移后对它们进行手动移动。
  
有关 SQL Server Reporting Services 中的更改的详细信息，请参阅升级顾问文档和 [Reporting Services 中的新增功能](../../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md)。  

## <a name="backup-files-and-data"></a><a name="bkmk_backup"></a> 备份文件和数据

 在安装新 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]实例之前，请确保对当前安装中的所有文件进行备份。  
  
1. 备份报表服务器数据库的加密密钥。 此步骤对于成功迁移至关重要。 而且，在迁移过程中，必须还原加密密钥才能使报表服务器重新获得对加密数据的访问权限。 若要备份密钥，请使用报表服务器配置管理器。  
  
2. 使用任一支持的备份 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的方法来备份报表服务器数据库。 有关详细信息，请参阅[将报表服务器数据库移至其他计算机（SSRS 本机模式）](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md)中有关如何备份报表服务器数据库的说明。  
  
3. 备份报表服务器配置文件。 要备份的文件包括：  
  
    1. RSReportServer.config  
  
    2. Rswebapplication.config  
  
    3. Rssrvpolicy.config  
  
    4. Rsmgrpolicy.config  
  
    5. Reportingservicesservice.exe.config  
  
    6. 针对报表服务器 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 应用程序的 Web.config。  
  
    7. [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 的 Machine.config（如果您为报表服务器操作修改过它）。  

## <a name="install-sql-server-reporting-services"></a><a name="bkmk_install_ssrs"></a> 安装 SQL Server Reporting Services

 在仅文件模式下安装新的报表服务器实例，以便可以将该实例配置为使用非默认值。 对于命令行安装，请使用 FilesOnly 参数  。 在安装向导中，选中 **“安装但不配置”** 选项。  
  
 单击下面的链接之一以查看有关如何安装新 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]实例的说明：  
  
* [使用安装向导安装 SQL Server Reporting Services 2016 及更低版本（安装程序）](install-reporting-services-native-mode-report-server.md) 
  
* [从命令提示符安装 SQL Server Reporting Services 2016 及更低版本](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)  

* [安装 SQL Server Reporting Services 2017 及更高版本](install-reporting-services.md)

## <a name="move-the-report-server-database"></a><a name="bkmk_move_database"></a> 移动报表服务器数据库

 报表服务器数据库包含已发布的报表、模型、共享数据源、计划、资源、订阅和文件夹， 还包含系统属性、项属性以及对报表服务器内容的访问权限。  
  
 如果您的迁移涉及到使用另一个 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则必须将报表服务器数据库移到新的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例中。 如果您在使用相同的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例，则跳到 [移动自定义程序集或扩展插件](#bkmk_move_custom)。  
  
 若要移动报表服务器数据库，请按下列步骤进行操作：
  
1. 选择要使用的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 实例。 SQL Server Reporting Services 要求你使用以下版本之一承载报表服务器数据库：  
  
    ::: moniker range="=sql-server-2017||=sqlallproducts-allversions"
    * [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]

    * [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
  
    * [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
  
    * [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
  
    * [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
    ::: moniker-end

    ::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
    * [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  

    * [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  

    * [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  

    * [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]
    ::: moniker-end
  
2. 启动 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 并连接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
3. 如果 **从未承载过报表服务器数据库，请在系统数据库中创建** RSExecRole [!INCLUDE[ssDE](../../includes/ssde-md.md)] 。 有关详细信息，请参阅 [创建 RSExecRole](../../reporting-services/security/create-the-rsexecrole.md)。  
  
4. 遵循[将报表服务器数据库移至其他计算机（SSRS 本机模式）](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md)中的说明。  
  
 请记住，报表服务器数据库和临时数据库相互依赖而且必须一起移动。 请不要复制数据库；复制不会将所有安全设置转移至新安装。 请不要移动用于计划报表服务器操作的 SQL Server 代理作业。 报表服务器自动重新创建这些作业。  

## <a name="move-custom-assemblies-or-extensions"></a><a name="bkmk_move_custom"></a> 移动自定义程序集或扩展插件

 如果安装中包括自定义的报表项、程序集或扩展插件，则必须重新部署这些自定义组件。 如果没有使用自定义组件，请跳至 [配置报表服务器](#bkmk_configure_reportserver)部分。  
  
 若要重新部署自定义组件，请执行以下操作：  
  
1. 确定是支持程序集还是需要重新编译程序集：

    * 必须使用 [IAuthenticationExtension2](/dotnet/api/microsoft.reportingservices.interfaces.iauthenticationextension2) 接口重新编写自定义安全扩展插件。
  
    * 必须使用呈现对象模型 (ROM) 重新编写 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的自定义呈现扩展插件。  
  
    * 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 及更高版本中不支持 HTML 3.2 和 HTML OWC 呈现器。  
  
    * 其他自定义程序集应当不需要重新编译。  
  
2. 将这些程序集移到新的报表服务器 \bin 文件夹中。 在 SQL Server 中，报表服务器二进制文件位于默认报表服务器实例的以下位置：  
  
     `\Program files\Microsoft SQL Server\MSRS13.MSSQLSERVER\Reporting Services\ReportServer\bin`  
  
3. 修改配置文件，以便为自定义组件添加条目。 所用程序集的种类不同，这些条目也有所不同。 有关在何处放置文件和添加配置条目的说明，请参阅以下内容：  
  
    1. [部署自定义程序集](../../reporting-services/custom-assemblies/deploying-a-custom-assembly.md)  
  
    2. [如何：部署自定义报表项](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
    3. [部署数据处理扩展插件](../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)  
  
    4. [部署传递扩展插件](../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)  
  
    5. [部署呈现扩展插件](../../reporting-services/extensions/rendering-extension/deploying-a-rendering-extension.md)  
  
    6. [实现安全扩展插件](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  

## <a name="configure-the-report-server"></a><a name="bkmk_configure_reportserver"></a> 配置报表服务器

 为报表服务器 Web 服务和 Web 门户配置 URL，并配置与报表服务器数据库的连接。  
  
 如果要迁移扩展部署，则应使所有报表服务器节点脱机并按照一次迁移一个服务器的方式迁移各个服务器。 一旦迁移了第一个报表服务器并且其成功连接到报表服务器数据库，则该报表服务器数据库版本将自动升级到 SQL Server 数据库版本。  
  
> [!IMPORTANT]
>  如果横向扩展部署中的所有报表服务器均联机并且尚未被迁移，则它们可能会遇到 rsInvalidReportServerDatabase 异常，因为它们在连接到升级的报表服务器数据库之后使用的仍是旧版架构  。  

如果迁移的报表服务器配置为用于横向扩展部署的共享数据库，则需要在配置报表服务器服务前从 ReportServer 数据库的 Keys 表中删除所有旧加密密钥   。 如果未删除这些密钥，迁移的报表服务器将尝试在扩展部署模式下进行初始化。 有关详细信息，请参阅[添加和删除横向扩展部署的加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)和[配置和管理加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md)。  

无法使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器删除扩展密钥。 必须使用 SQL Server Management Studio 从 **Keys** 数据库的 **ReportServer** 表中删除旧密钥。 删除 Keys 表中的所有行。 此操作清除该表并准备将其用于只还原对称密钥，如下面的步骤所述。  

在删除密钥之前，建议您首先备份对称加密密钥。 可以使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置管理器来备份密钥。 打开配置管理器，单击“加密密钥”选项卡，然后单击“备份”按钮  。 还可以撰写 WMI 命令的脚本以便备份加密密钥。 有关 WMI 的详细信息，请参阅 [BackupEncryptionKey 方法 (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-backupencryptionkey.md)。  
  
1. 启动报表服务器配置管理器，然后连接到安装的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 实例。 有关详细信息，请参阅[报表服务器配置管理器（本机模式）](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)。  
  
2. 为报表服务器和 Web 门户配置 URL。 有关详细信息，请参阅[配置 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)。  
  
3. 配置报表服务器数据库，并从以前的安装中选择现有的报表服务器数据库。 成功配置之后，报表服务器服务将重新启动，并且一旦与报表服务器数据库建立连接，该数据库就会自动升级到 SQL Server Reporting Services。 有关如何运行“更改服务器向导”（该向导可用来创建或选择报表服务器数据库）的详细信息，请参阅[创建本机模式报表服务器数据库](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)。  
  
4. 还原加密密钥。 在针对报表服务器数据库中预先存在的连接字符串和凭据启用可逆加密时，此步骤是必不可少的。 有关详细信息，请参阅 [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)。  
  
5. 如果报表服务器安装在新计算机上，并且您使用的是 Windows 防火墙，请确保该报表服务器侦听的 TCP 端口处于打开状态。 默认情况下，此端口为 80。 有关详细信息，请参阅 [将防火墙配置为允许报表服务器访问](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)。  
  
6. 若要在本地管理本机模式报表服务器，需要配置操作系统以允许使用 Web 门户进行本地管理。 有关详细信息，请参阅[为本地管理配置本机模式报表服务器](../../reporting-services/report-server/configure-a-native-mode-report-server-for-local-administration-ssrs.md)。  

## <a name="copy-custom-configuration-settings-to-rsreportserverconfig-file"></a><a name="bkmk_copy_custom_config"></a> 将自定义配置设置复制到 RSReportServer.config 文件

如果对先前安装中的 RSReportServer.config 文件或 RSWebApplication.config 文件进行过修改，则应当在新的 RSReportServer.config 文件中进行同样的修改。 下面的列表概述了修改先前配置文件的某些原因，并提供了一些指向其他信息的链接，这些信息介绍如何在 SQL Server 2016 中配置同样的设置。  
  
|自定义|信息|  
|-------------------|-----------------|  
|具有自定义设置的报表服务器电子邮件传递|[电子邮件设置 * Reporting Services 本机模式](../../reporting-services/install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md)。|  
|设备信息设置|[在 RSReportServer.Config 中自定义呈现扩展插件参数](../../reporting-services/customize-rendering-extension-parameters-in-rsreportserver-config.md)|

## <a name="windows-service-group-and-security-acls"></a><a name="bkmk_windowsservice_group"></a> Windows 服务组与安全 ACL

 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 中存在一个服务组，即 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Windows 服务组。可使用该组为与 SQL Server Reporting Services 一起安装的所有注册表项、文件和文件夹创建安全 ACL。 此 Windows 组的名称以 SQLServerReportServerUser$\<*computer_name*>$\<*instance_name*> 格式显示。  

## <a name="verify-your-deployment"></a><a name="bkmk_verify"></a> 验证部署

1. 打开浏览器，并在 URL 地址中键入报表服务器虚拟目录和 [!INCLUDE[ssRSWebPortal](../../includes/ssrswebportal.md)] 虚拟目录，对这些目录进行测试。 有关详细信息，请参阅 [验证 Reporting Services 安装](../../reporting-services/install-windows/verify-a-reporting-services-installation.md)  
  
2. 测试报表，并验证它们是否包含所需的数据。 检查数据源信息，查看是否仍指定了数据源连接信息。 报表服务器在处理和呈现报表时使用报表对象模型，但是该服务器不将 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]、[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 或 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 构造替换为新的报表定义语言元素。 若要了解有关如何在新版本的报表服务器上运行现有报表的详细信息，请参阅[升级报表](../../reporting-services/install-windows/upgrade-reports.md)。  

## <a name="remove-unused-programs-and-files"></a><a name="bkmk_remove_unused"></a> 删除未使用的程序和文件

一旦成功将报表服务器迁移到新实例，则可能需要执行以下步骤以删除不再需要的程序和文件。  
  
1. 如果不再需要早期版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，则将其卸载。 此步骤不会删除下列项，但是，如果不再需要这些项，则可以手动将其删除：  
  
    * 旧的报表服务器数据库  
  
    * RsExec 角色  
  
    * 报表服务器服务帐户  
  
    * 报表服务器 Web 服务的应用程序池  
  
    * 报表管理器和报表服务器的虚拟目录  
  
    * 报表服务器日志文件  
  
2. 如果此计算机上不再需要 IIS，则将其删除。

## <a name="next-steps"></a>后续步骤

* [迁移 Reporting Services 安装](../../reporting-services/install-windows/migrate-a-reporting-services-installation-sharepoint-mode.md)  
* [报表服务器数据库](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md)   
* [升级和迁移 Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)   
* [Reporting Services 后向兼容性](../../reporting-services/reporting-services-backward-compatibility.md)   
* [报表服务器配置管理器](../../reporting-services/install-windows/reporting-services-configuration-manager-native-mode.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)