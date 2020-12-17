---
description: 排除 Reporting Services 安装故障
title: 排除 Reporting Services 安装故障 | Microsoft Docs
ms.date: 01/17/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: e2536f7f-d90c-4571-9ffd-6bbfe69018d6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1cc420302bb8d1610adcc1848fda226c4c55b492
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472468"
---
# <a name="troubleshoot-a-reporting-services-installation"></a>排除 Reporting Services 安装故障

  如果由于安装期间出现错误而导致无法安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]，请使用本文中的说明来解决最有可能导致安装错误的情况。  
  
 有关与 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 相关的其他错误和问题，请参阅[解决 SSRS 问题和错误](https://social.technet.microsoft.com/wiki/contents/articles/ssrs-troubleshooting-issues-and-errors.aspx)。  
  
 查看[联机发行说明](https://go.microsoft.com/fwlink/?linkid=236893) ，也许在发行说明中会论及你遇到的问题。  
  
##  <a name="check-setup-logs"></a><a name="bkmk_setuplogs"></a> 检查安装程序日志  
 安装程序错误记录在 **[!INCLUDE[ssInstallPath](../../includes/ssinstallpath-md.md)]Setup Bootstrap\Log** 文件夹内的日志文件中。 每次运行安装程序时都会创建一个子文件夹。 该子文件夹名称为您运行安装程序的时间和日期。 有关如何查看安装程序日志文件的说明，请参阅 [查看和阅读 SQL Server 安装程序日志文件](../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)。  
  
-   该日志文件包括文件集合。  
  
-   打开 *_summary.txt 文件可查看产品、组件和实例信息。  
  
-   打开 *_errorlog.txt 文件可查看安装期间生成的错误信息。  
  
-   打开 *_RS\_\*_ComponentUpdateSetup.log 以查看 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装程序信息。  
  
##  <a name="check-prerequisites"></a><a name="bkmk_prereq"></a> 检查系统必备条件  
 安装程序会自动检查系统必备条件。 不过，如果要排除安装中的问题，则了解安装程序将检查哪些条件非常有用。  
  
-   运行安装程序的帐户要求包括本地 Administrators 组中的成员身份。 安装程序必须具有添加文件的权限、注册表设置权限、创建本地安全组权限和设置权限。 如果要在安装时采用默认配置，安装程序必须具有在所安装的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例上创建报表服务器数据库的权限。  
  
-   操作系统必须支持 HTTP.SYS 1.1。  
  
-   HTTP 服务必须已启用并正在运行。  
  
-   如果还要安装 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服务，则必须运行分布式事务处理协调器 (DTC)。  
  
-   System32 文件夹中必须存在 Authz.dll。  
  
 安装程序不再检查 Internet Information Services (IIS) 或 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 需要 MDAC 2.0 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 2.0 版；如果尚未安装这些组件，安装程序将会安装它们。  

::: moniker range="=sql-server-2016"
  
##  <a name="troubleshoot-problems-with-sharepoint-mode-installations"></a><a name="bkmk_tshoot_sharepoint"></a>排除 SharePoint 模式安装问题  
  
-   [报表服务器配置管理器未启动](#bkmk_configmanager_notstart)  
  
-   [在 SharePoint 模式下安装 SQL Server 2016 SSRS 后，在 SharePoint 管理中心看不到 SQL Server Reporting Services 服务](#bkmk_no_ssrs_service)  
  
-   [Reporting Services PowerShell cmdlet 不可用，并且命令无法识别](#bkmk_cmdlets_not_recognized)  
  
-   [您将会看到一则错误消息，指示 URL 未配置](#bkmk_URL_not_configured)  
  
-   [在安装有但未配置 SharePoint 的计算机上进行安装时安装程序将失败](#bkmk_sharepoint_not_confiugred)  
  
-   [SharePoint 管理中心页空白](#bkmk_central_admin_blank)  
  
-   [当您尝试创建新的报表生成器报表时，您会看到一条错误消息](#bkmk_reportbuilder_newreport_error)  
  
-   [您看到一条指示 PREPAREIMAGE 不支持 RS_SHP 的错误消息](#bkmk_RS_SHP_notsupported)  

### <a name="report-server-configuration-manager-does-not-start"></a><a name="bkmk_configmanager_notstart"></a>报表服务器配置管理器未启动

 **说明：** 此问题是 SQL Server 2012 和更高版本中的设计问题。 Reporting Services 是为 SharePoint 服务体系结构而设计的。 在 SharePoint 模式下配置和管理 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不再需要配置管理器。  
  
 **解决方法：** 使用 SharePoint 管理中心在 SharePoint 模式中配置报表服务器。 有关详细信息，请参阅 [管理 Reporting Services SharePoint 服务应用程序](../../reporting-services/report-server-sharepoint/manage-a-reporting-services-sharepoint-service-application.md)  
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-do-not-see-the-sql-server-reporting-services-service-in-sharepoint-central-administration-after-installing-sql-server-2016-ssrs-in-sharepoint-mode"></a><a name="bkmk_no_ssrs_service"></a> 在 SharePoint 模式下安装 SQL Server 2016 SSRS 后，在 SharePoint 管理中心看不到 SQL Server Reporting Services 服务  
 **说明：** 如果在 SharePoint 模式中成功安装 SQL Server 2016 Reporting Services 和适用于 SharePoint 2013/2016 的 SQL Server 2016 Reporting Services 加载项后，在以下两个菜单中看不到“SQL Server Reporting Services”，则 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务尚未注册：  
  
-   SharePoint 2013/2016 管理中心 ->“应用程序管理” ->“管理服务器上的服务”页  
  
-   SharePoint 2013/2016 管理中心 ->“应用程序管理”->“管理服务应用程序”->“新建”菜单  
  
 **解决方法：** 注册并启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint Services，完成以下步骤：  
  
1.  在运行 SharePoint 2013/2016 管理中心的计算机上  
  
    1.  使用管理员权限打开 SharePoint 2013/2016 命令行管理程序。 右键单击图标，然后单击“以管理员身份运行”。 从 shell 运行以下三个 cmdlet：  
  
    2.  ```  
        Install-SPRSService  
        ```  
  
    3.  ```  
        Install-SPRSServiceProxy  
        ```  
  
    4.  ```  
        Get-SPServiceInstance -all |where {$_.TypeName -like "SQL Server Reporting*"} | Start-SPServiceInstance  
        ```  
  
2.  请验证 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务状态在以下页面上是否显示为“已启动”：SharePoint 2013/2016 管理中心 ->“应用程序管理”->“管理服务器上的服务”   
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  
  
###  <a name="reporting-services-powershell-cmdlets-are-not-available-and-commands-are-not-recognized"></a><a name="bkmk_cmdlets_not_recognized"></a> Reporting Services PowerShell cmdlet 不可用，并且命令无法识别  
 **说明：** 尝试运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] PowerShell cmdlet 时，会看到类似如下的错误消息：  
  
-   术语“Install-SPRSServiceInstall-SPRSService” **无法识别** 作为 cmdlet、函数、脚本文件或可操作程序的名称。 检查名称的拼写，如果包含路径，请验证该路径是否正确，并重试。 At line:1 char:39+ Install-SPRSServiceInstall-SPRSService <<<<    + CategoryInfo          : ObjectNotFound: (Install-SPRSServiceInstall-SPRSService:String) [], CommandNotFoundExcep  
  
 **解决方法：** 完成以下操作之一：  
  
-   运行用于 SharePoint 产品的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 外接程序。 **rssharepoint.msi**。  
  
-   从 SQL Server 安装介质安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 模式。  
  
 如果在完成上述解决方法之一时，“SharePoint 2013/2016 命令行管理程序”处于打开状态，请关闭命令行管理程序，然后将其重新打开。  
  
 有关详细信息，请参阅下列文章：  
  
-   [在何处查找用于 SharePoint 产品的 Reporting Services 外接程序](../../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  
  
-   [在 SharePoint 模式下安装第一个报表服务器](install-the-first-report-server-in-sharepoint-mode.md)  
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-see-an-error-message-indicating-the-url-is-not-configured"></a><a name="bkmk_URL_not_configured"></a> 您将会看到一则错误消息，指示 URL 未配置  
 **说明**：会看到类似如下错误消息：  
  
 不支持此 SQL Server Reporting Services (SSRS) 功能。 使用管理中心验证和修复以下一个或多个问题：
 
 - 报表服务器 URL 未配置。 使用 SSRS 集成页来对其进行设置。
 
 - SSRS 服务应用程序代理未配置。 使用 SSRS 服务应用程序页来配置代理。
 
 - SSRS 服务应用程序未映射到此 web 应用程序。 使用 SSRS 服务应用程序页可以将该 SSRS 服务应用程序代理与此 Web 应用程序的应用程序代理组相关联。 
  
 **解决方法：** 错误消息包含三个建议的步骤来纠正此问题。 “报表服务器 URL 未配置”消息中的第一个建议。 在与 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]之前的报表服务器版本集成时相关。 使用 **SQL Server Reporting Services (2008 和 2008 R2)** 在 **常规应用程序设置** 页上完成针对以前报表服务器版本的 SharePoint 配置。  
  
 **更多信息：** 在您尝试使用要求连接到 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务的任何 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 功能时将看到此错误消息。 这包括：  
  
-   从 SharePoint 文档库中打开 SQL Server 报表生成器。  
  
-   管理订阅。  
  
-   管理服务应用程序。  
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  
  
###  <a name="setup-fails-on-a-computer-with-sharepoint-installed-but-it-is-not-configured"></a><a name="bkmk_sharepoint_not_confiugred"></a> 在安装有但未配置 SharePoint 的计算机上进行安装时安装程序将失败  
 **说明：** 如果选择在安装有但未配置 SharePoint 的计算机上安装 Reporting Services SharePoint 模式，则将看到一条类似于以下内容的消息，而且安装程序将停止运行：  
  
 SQL Server 安装程序已停止工作  
  
 **解决方法：** 配置 SharePoint，然后运行 SQL Server 安装。  
  
 **更多信息：** 将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装到现有 SharePoint 安装中时，安装程序会尝试安装并启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] SharePoint 服务。 如果未配置 SharePoint，服务安装将失败，从而导致安装程序失败。  
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  
  
###  <a name="sharepoint-central-administration-page-is-blank"></a><a name="bkmk_central_admin_blank"></a> SharePoint 管理中心页空白  
 **说明：** 你能够成功地安装 SharePoint 2013/2016，而不出现任何安装错误。 但是，当您浏览到管理中心时，您仅看到空白页：  
  
 **解决方法：** 此问题不是特定于 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ，但与整体 SharePoint 安装中的权限配置有关。 下面是一些建议：  
  
-   查看有关开发环境的 SharePoint 文章。 [为 SharePoint 设置常规开发环境](/sharepoint/dev/general-development/set-up-a-general-development-environment-for-sharepoint)  
  
-   查阅论坛帖子： [在 Windows 7 上进行安装后管理中心返回空白页](https://social.technet.microsoft.com/Forums/en/sharepoint2010setup/thread/a422a3c8-39f6-4b9e-988a-4c4d1e745694)  
  
-   用于 SharePoint 2013/2016 管理中心服务等 SharePoint 服务的服务帐户在本地操作系统中应具有管理权限。  
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-see-an-error-message-when-you-try-to-create-a-new-report-builder-report"></a><a name="bkmk_reportbuilder_newreport_error"></a> 当您尝试创建新的报表生成器报表时，您会看到一条错误消息  
 **说明：** 当您尝试在文档库内创建报表生成器报表时您会看到一条类似于以下内容的错误消息：  
  
 不支持此功能，因为 SQL Server Reporting Services 服务应用程序不存在或者尚未在管理中心配置报表服务器 URL。  
  
 **解决方法：** 确认您有 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序且已正确对其进行配置。 有关详细信息，请参阅[在 SharePoint 模式下安装第一个报表服务器](install-the-first-report-server-in-sharepoint-mode.md)。
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  
  
###  <a name="you-see-an-error-message-that-rs_shp-is-not-supported-with-prepareimage"></a><a name="bkmk_RS_SHP_notsupported"></a> 您看到一条指示 PREPAREIMAGE 不支持 RS_SHP 的错误消息  
 **说明：** 尝试为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 运行 PREPAREIMAGE 时，会看到类似如下的错误消息：  
  
 “当执行 PREPAREIMAGE 操作时，指定的功能“RS_SHP”不受支持，因为它不支持 SysPrep。 删除与 SysPrep 不兼容的功能，然后重新运行安装程序。”  
  
 **解决方法：** 目前没有解决方法。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 不支持 SYSPREP (PREPAREIMAGE)。 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 本机模式支持 SYSPREP。  
  
 ![用于返回首页链接的箭头图标](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "用于返回页首链接的箭头图标") [排除 SharePoint 模式安装问题](#bkmk_tshoot_sharepoint)  

::: moniker-end
  
##  <a name="troubleshoot-problems-with-the-native-mode-installations"></a><a name="bkmk_tshoot_native"></a> 排除本机模式安装问题  
  
###  <a name="performance-counters-are-not-visible-after-upgrading-to-windows-vista-or-windows-server-2008"></a><a name="PerfCounters"></a> 升级到 Windows Vista 或 Windows Server 2008 后，性能计数器不可见  
 如果将运行 [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] 的计算机的操作系统升级到 [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 或 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]，则升级后不会设置 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 性能计数器。  
  
#### <a name="to-reinstate-reporting-services-performance-counters"></a>恢复 Reporting Services 性能计数器  
  
1.  删除以下注册表项：  
  
    -   **HKLM\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service**  
  
    -   **HKLM\SYSTEM\CurrentControlSet\Services\MSRS 2016 Windows Service**  
  
2.  打开命令窗口，然后在提示符下键入以下命令：  
  
    -   **run \<** *.NET 4.0 Framework directory* **>\InstallUtil.exe \<** *Report Server Bin directory* **>\ReportingServicesLibrary.dll**  
  
        > [!NOTE]  
        >  将 \<*.NET 4.0 Framework directory*> 替换为 .NET Framework 4.0 文件的物理路径，将 \<*Report Server Bin directory*> 替换为报表服务器 bin 文件的物理路径。  
  
3.  重新启动 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务。  
  
 若要验证这些步骤是否有效，请打开 Web 浏览器，导航到 Web 门户 URL 或报表服务器 URL。 然后打开性能监视器以验证计数器是否正常工作。  
  
#### <a name="to-add-the-performance-registry-keys-again-by-using-registry-editor"></a>使用注册表编辑器重新添加性能注册表项  
  
1.  打开注册表编辑器：  
  
    1.  单击 **“开始”**，再单击 **“运行”**。  
  
    2.  在“运行”  对话框的“打开”  框中，键入 **regedit**。  
  
2.  在注册表编辑器中，选择以下注册表项： `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance`  
  
3.  右键单击“性能”节点，指向“新建”，然后单击“多字符串值”。  
  
4.  键入 **Counter Names** ，然后按 Enter。  
  
5.  在该节点中重复添加 **Counter Types** 注册表项。  
  
6.  导航到以下注册表项： `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance`  
  
7.  右键单击“性能”节点，指向“新建”，然后单击“多字符串值”。  
  
8.  键入 **Counter Names** ，然后按 Enter。  
  
9. 在该节点中重复添加 **Counter Types** 注册表项。  
  
 修复 64 位实例或手动重新添加注册表项之后，可以使用性能监视器来配置要监视的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 性能对象。  
  
###  <a name="reportserverexternalurl-and-passthroughcookies-configuration-properties-are-not-configured-after-an-upgrade-from-sql-server-2005"></a><a name="ConfigPropsMissing"></a> 从 SQL Server 2005 升级后，未配置 ReportServerExternalURL 和 PassThroughCookies 配置属性  
 当你从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 升级到 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]时，升级过程不会配置 **ReportServerExternalURL** 和 **PassThroughCookies** 配置属性。 **ReportServerExternalURL** 是一个可选属性，并且仅在以下情况下才应对其进行设置：如果你使用的是 SharePoint 2.0 Web 部件，并且希望用户能够检索报表并在新的浏览器窗口中打开。 有关 ReportServerExternalURL 的详细信息，请参阅[配置文件中的 URL（报表服务器配置管理器）](../../reporting-services/install-windows/urls-in-configuration-files-ssrs-configuration-manager.md)。 仅当使用自定义身份验证方法时，才需要 **PassThroughCookies** 。 有关 PassThroughCookies的详细信息，请参阅 [配置 Web 门户以便传递自定义身份验证 Cookie](../../reporting-services/security/configure-the-web-portal-to-pass-custom-authentication-cookies.md)。  
  
> [!NOTE]  
>  使用自定义身份验证时，建议您迁移安装而不是执行升级。 有关迁移 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的详细信息，请参阅[迁移 Reporting Services 安装（本机模式）](../../reporting-services/install-windows/migrate-a-reporting-services-installation-native-mode.md)。  
  
 默认情况下， [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 配置中不存在这些属性。 如果在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中配置了这些属性且继续需要这些属性提供的功能，则在升级过程后，必须手动将它们添加到 **RSReportServer.config** 文件中。 有关详细信息，请参阅[修改 Reporting Services 配置文件 (RSreportserver.config)](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md)。  

### <a name="401-unauthorized-error-when-using-windows-authentication-after-an-upgrade-from-sql-server-2005-to-sql-server-2016"></a><a name="WindowsAuthBreaksAfterUpgrade"></a> 从 SQL Server 2005 升级到 SQL Server 2016 后，使用 Windows 身份验证时发生 401-未经授权的错误

 如果从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 升级到 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)]，并对 Report Server 服务帐户使用 NTLM 身份验证和内置帐户，则在升级后访问报表服务器或 Web 门户时，可能会遇到 401-未经授权的错误。  
  
 出现此错误消息是因为 Windows 身份验证的默认 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 配置发生了更改。 当报表服务器服务帐户是 Network Service 或 Local System 时，会配置 Negotiate。 当报表服务器服务帐户不是这些内置帐户之一时，会配置 NTLM。 若要在升级后解决此问题，可以编辑 RSReportServer.config 文件，将 **AuthenticationType** 配置为 **RSWindowsNTLM**。 有关详细信息，请参阅[在报表服务器上配置 Windows 身份验证](../../reporting-services/security/configure-windows-authentication-on-the-report-server.md)。  

### <a name="uninstalling-32-bit-instance-of-sql-server-2016-reporting-services-in-side-by-side-deployment-with-a-64-bit-instance-breaks-the-64-bit-instance"></a><a name="Uninstall32BitBreaks64Bit"></a> 在与 64 位实例的并行部署中，卸载 SQL Server 2016 Reporting Services 的 32 位实例破坏了 64 位实例

 如果在计算机上并行安装 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 的 32 位实例和 64 位实例，卸载 32 位实例时，将删除 4 个 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 注册表项。 删除密钥会破坏 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的 64 位实例。 卸载 32 位实例时删除的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 注册表项为：  
  
 `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance:Counter Names` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Windows Service\Performance:Counter Names` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Web Service\Performance:Counter Types` `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MSRS 2016 Windows Service\Performance:Counter Types`  
  
 若要解决此问题，可以修复 64 位实例。 尽管建议使用修复，但可以使用注册表编辑器手动重新添加注册表项。  
  
> [!CAUTION]  
>  错误编辑注册表会严重损坏您的系统。 在更改注册表之前，应备份计算机上任何有价值的数据。  
  
##  <a name="additional-resources"></a><a name="bkmk_additional"></a> 其他资源  
 可以查阅以下资源，帮助解决问题：  
  
-   TechNet Wiki：[排除 SharePoint 2010 集成模式下的 SQL Server Reporting Services (SSRS) 问题](https://social.technet.microsoft.com/wiki/contents/articles/troubleshoot-sql-server-reporting-services-ssrs-in-sharepoint-integrated-mode.aspx)  
  
-   [Microsoft 问答：SQL Server Reporting Services](/answers/topics/sql-server-reporting-services.html)  
  
-   获得反馈或提出更多问题？ 请访问 [Microsoft SQL Server UserVoice](https://feedback.azure.com/forums/908035-sql-server)。  
  
