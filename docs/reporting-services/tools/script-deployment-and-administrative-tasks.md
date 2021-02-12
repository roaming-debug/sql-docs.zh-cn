---
title: 为部署和管理任务编写脚本 | Microsoft Docs
description: 了解用于自动执行报表服务器的常规安装、部署和管理任务的工具、技术和策略。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: tools
ms.topic: conceptual
helpviewer_keywords:
- scripts [Reporting Services]
- moving reports
- report servers [Reporting Services], duplicating settings
- deploying [Reporting Services], scripts
- copying report server settings
- administrative tasks [Reporting Services]
- duplicating report server environment
- migrating reports [Reporting Services]
- scripts [Reporting Services], deployments
- transferrng reports
- reports [Reporting Services], migrating
ms.assetid: d0416c9e-e3f9-456d-9870-2cfd2c49039b
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 883acc6af22c69b6deda8606c2361e33e55aeb1f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077918"
---
# <a name="script-deployment-and-administrative-tasks"></a>为部署和管理任务编写脚本

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 支持使用脚本来自动执行日常安装、部署任务和管理任务。 部署报表服务器的过程包括多个步骤。 必须使用多个工具和过程来配置部署；没有任何单个程序或方法可用于自动执行所有任务。  
  
 并非每个步骤都应自动执行。 在某些情况下，手动或通过图形工具执行步骤是最简单且最有效的方法。 例如，如果要部署大量报表和模型，最好复制报表服务器数据库，而不是编写用于重新创建报表服务器环境的代码。  
  
 某些步骤需要自定义代码。 例如，可以自动为 Web 服务和报表管理器配置 URL，但是只有在编写用于对报表服务器 Windows Management Instrumentation (WMI) 提供程序进行调用的自定义代码时才能这样做。 如果不想编写代码，则必须使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具执行该步骤。  
  
 若要运行配置报表服务器的脚本，您必须是要配置的计算机上的本地管理员。 有关详细信息，请参阅 [配置报表服务器以进行远程管理](../../reporting-services/report-server/configure-a-report-server-for-remote-administration.md)。  
  
 本主题说明自动执行特定步骤的推荐方法。 介绍了几个程序和编程接口，本主题的稍后部分中提供了针对每一种方法的说明。  
  
## <a name="deployment-tasks-and-how-to-automate-them"></a>部署任务及自动执行这些任务的方式  
 下表总结了部署报表服务器所需的安装和配置任务。 您可以使用该表将特定任务与用于自动执行任务或在无人参与的情况下执行任务的方法相匹配。  
  
|任务|方法|  
|----------|--------------|  
|安装 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]。|可以从命令行运行安装程序以执行无人参与的安装。<br /><br /> 可以使用安装程序安装和配置报表服务器，但是只有在指定默认配置选项且系统满足此安装类型的所有要求时才能这样做。 如果不能安装默认配置，则必须执行“仅文件”安装。|  
|配置服务帐户。|服务帐户最初是在安装过程中配置的。 若要将更改服务帐户作为安装后任务自动执行，必须编写可对报表服务器 WMI 接口进行调用的自定义代码。 没有用于以编程方式配置服务帐户的命令提示实用工具或脚本模板。<br /><br /> 如果编码要求阻止您自动执行此步骤，则可通过运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具轻松地手动配置帐户。 有关详细信息，请参阅[配置服务帐户（报表服务器配置管理器）](../install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)。|  
|配置报表服务器 Web 服务和报表管理器 URL。|必须编写用于对报表服务器 WMI 提供程序进行调用的自定义代码。 没有用来配置 URL 的命令行实用工具或脚本模板。<br /><br /> 如果希望避免编写代码，则可以通过运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具来手动配置 URL。 有关详细信息，请参阅[配置 URL（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-url-ssrs-configuration-manager.md)。|  
|创建报表服务器数据库。|必须编写用于对报表服务器 WMI 提供程序进行调用的自定义代码。 没有用来创建报表服务器数据库和 RSExecRole 的命令提示实用工具或脚本模板。<br /><br /> 如果希望避免编写代码，则可以通过运行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 配置工具来手动创建数据库。 有关详细信息，请参阅[创建本机模式报表服务器数据库（报表服务器配置管理器）](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md)。|  
|配置报表服务器数据库连接。|如果要更改连接字符串、帐户、密码或身份验证类型，请通过运行 **rsconfig** 实用工具来配置连接。 有关详细信息，请参阅[配置报表服务器数据库连接（报表服务器配置管理器）](../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md)和 [rsconfig 实用工具 (SSRS)](../../reporting-services/tools/rsconfig-utility-ssrs.md)。<br /><br /> 不能使用 rsconfig.exe 来创建或升级数据库。 数据库和 RSExecRole 必须已经存在。|  
|配置扩展部署。|从以下用于自动执行扩展部署的方法中进行选择：<br /><br /> -   运行 rskeymgmt.exe 实用工具以将报表服务器实例联接到现有安装。 有关详细信息，请参阅[添加和删除横向扩展部署的加密密钥（报表服务器配置管理器）](../../reporting-services/install-windows/add-and-remove-encryption-keys-for-scale-out-deployment.md)。<br />-   编写针对报表服务器 WMI 提供程序运行的自定义代码。|  
|备份加密密钥。|从以下用于自动备份加密密钥的方法中进行选择：<br /><br /> -   运行 rskeymgmt.exe 实用工具以备份密钥。 有关详细信息，请参阅 [Back Up and Restore Reporting Services Encryption Keys](../../reporting-services/install-windows/ssrs-encryption-keys-back-up-and-restore-encryption-keys.md)。<br />-   编写针对报表服务器 WMI 提供程序运行的自定义代码。|  
|配置报表服务器电子邮件。|编写针对 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] WMI 提供程序运行的自定义代码。 该提供程序支持部分电子邮件配置设置。<br /><br /> 虽然 RSReportServer.config 文件包含所有设置，但是不要以自动执行的方式使用该文件。 具体来说，不要使用批处理文件将该文件复制到另一个报表服务器。 每个配置文件都包含特定于当前实例的值。 这些值对于其他报表服务器实例将是无效的。<br /><br /> 有关设置的详细信息，请参阅[电子邮件设置 - Reporting Services 本机模式（配置管理器）](../install-windows/e-mail-settings-reporting-services-native-mode-configuration-manager.md)。|  
|配置无人参与的执行帐户。|从以下用于自动配置无人参与处理帐户的方法中进行选择：<br /><br /> -   运行 rsconfig.exe 实用工具以配置帐户。 有关详细信息，请参阅[配置无人参与的执行帐户（报表服务器配置管理器）](../../reporting-services/install-windows/configure-the-unattended-execution-account-ssrs-configuration-manager.md)。<br />-   编写用于对报表服务器 WMI 提供程序进行调用的自定义代码。|  
|将现有的内容（包括文件夹层次结构、角色分配、报表、订阅、计划、数据源和资源）部署到另一台报表服务器上。|重新创建现有报表服务器环境的最佳方式是将报表服务器数据库复制到新的报表服务器实例中。<br /><br /> 替代方法是编写自定义代码，以编程方式重新创建现有的报表服务器内容。 但应注意，订阅、报表快照和报表历史记录无法以编程方式重新创建。<br /><br /> 某些部署受益于以上两项技术的结合使用（也就是说，还原报表服务器数据库，然后运行自定义代码以便为特定安装修改报表服务器数据库）。<br /><br /> 有关详细示例，请参阅 [用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)。<br /><br /> 有关重新定位报表服务器数据库的详细信息，请参阅[将报表服务器数据库移至其他计算机（SSRS 本机模式）](../../reporting-services/report-server/moving-the-report-server-databases-to-another-computer-ssrs-native-mode.md)。 有关以编程方式创建报表服务器环境的详细信息，请参阅本主题中的“使用脚本迁移报表服务器内容和文件夹”一节。|  
  
## <a name="tools-and-technologies-for-automating-server-deployment"></a>用于自动执行服务器部署的工具和技术  
 以下列表总结了可用于自动执行部署和维护任务的程序和接口：  
  
-   可以在无人参与模式下运行安装程序，以安装和配置（有时）报表服务器组件。 必须使用“仅文件”安装选项，才能让安装程序配置报表服务器实例。  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] WMI 提供程序和 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 命令行实用工具可用于本地和远程服务器配置。  
  
     [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] WMI 提供程序公开了用于对 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 安装的各个方面（包括指定服务帐户、配置 URL、创建和配置报表服务器数据库或者为电子邮件传递配置报表服务器）进行配置的类、属性和方法。 必须编写自定义代码或脚本，才能使用 WMI 提供程序。 有关详细信息，请参阅 [访问 Reporting Services WMI 提供程序](../../reporting-services/tools/access-the-reporting-services-wmi-provider.md)。  
  
     除了编写代码以外，还可以使用命令行实用工具（rsconfig.exe 和 rskeymgmt.exe）。 可以编写运行实用工具的批处理文件。 可以使用实用工具来自动执行某些但不是所有配置任务。  
  
-   报表服务器脚本主机工具 (rs.exe) 可以运行为了重新创建现有内容或将现有内容从一个报表服务器移至另一个报表服务器而编写的自定义 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 代码。 使用此方法，可以在 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]中编写脚本，将其另存为 .rss 文件，并使用 rs.exe 在目标报表服务器上运行该脚本。 所编写的脚本可以调用 SOAP 接口，以访问报表服务器 Web 服务。 应使用此方法编写部署脚本，因为通过此方法可以重新创建报表服务器文件夹命名空间和内容，以及重新创建基于角色的安全性。  
  
-   SQL Server 2012 版引入了用于 SharePoint 集成模式的 PowerShell cmdlet。 您可以使用 PowerShell 配置和管理 SharePoint 集成。  有关详细信息，请参阅 [用于 Reporting Services SharePoint 模式的 PowerShell cmdlet](../../reporting-services/report-server-sharepoint/powershell-cmdlets-for-reporting-services-sharepoint-mode.md)。  
  
## <a name="use-scripts-to-migrate-report-server-content-and-folders"></a>使用脚本迁移报表服务器内容和文件夹  
 可以编写用于在其他报表服务器实例中复制报表服务器环境的脚本。 通常会使用 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 编写部署脚本，然后使用报表服务器脚本主机实用工具处理这些脚本。  
  
 有关详细示例，请参阅 [用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)。  
  
 使用脚本可以在服务器之间复制文件夹、共享数据源、资源、报表、角色分配和设置。 为报表服务器实例编写脚本，然后在其他服务器上运行该脚本以重新创建报表服务器命名空间。 如果在 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 部署中有多个报表服务器，则可以在每台服务器上分别运行该脚本，以便通过同一种方式配置所有的服务器。  
  
 下面是在服务器之间迁移报表的步骤：  
  
1.  将脚本变量设置为源报表服务器的 URL。  
  
2.  使用 <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A> 和 <xref:ReportService2010.ReportingService2010.GetProperties%2A> 方法检索报表定义和报表的属性。  
  
3.  将 URL 设置为指向目标服务器。  
  
4.  使用 <xref:ReportService2010.ReportingService2010.CreateCatalogItem%2A> 方法，传递从 <xref:ReportService2010.ReportingService2010.GetProperties%2A> 返回的属性和由 <xref:ReportService2010.ReportingService2010.GetItemDefinition%2A>。  
  
 通过结合使用 get 方法和 create 方法，您可以执行类似的步骤以迁移设置、文件夹、共享数据源和资源。 有关可用方法的详细信息，请参阅[技术参考 (SSRS)](../../reporting-services/technical-reference-ssrs.md)。  
  
> [!NOTE]  
>  除非已显示设置凭据，否则脚本将使用运行脚本的用户的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 凭据运行。  
  
 有关如何设置格式并运行脚本文件的详细信息，请参阅 [使用 rs.exe 实用工具和 Web 服务编写脚本](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)。  
  
## <a name="using-scripts-to-set-server-properties"></a>使用脚本设置服务器属性  
 可以编写脚本以针对报表服务器设置系统属性。 下面的 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .NET 脚本显示了一种设置属性的方法。 该示例禁用了 RSClientPrint ActiveX 控件，但可以用任意有效的属性名称和值来替换 **EnableClientPrinting** 和 **False** 。 若要查看服务器属性的完整列表，请参阅 [Report Server System Properties](../../reporting-services/report-server-web-service/net-framework/reporting-services-properties-report-server-system-properties.md)。  
  
 若要使用该脚本，则将其保存至扩展名为 .rss 的文件，然后使用 rs.exe 命令提示实用工具在报表服务器上运行该文件。 由于没有编译该脚本，因此不需要安装 [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]。 该示例假定您对承载报表服务器的本地计算机拥有权限。 如果未使用拥有权限的帐户进行登录，则必须通过其他命令行参数来指定帐户信息。 有关详细信息，请参阅 [RS.exe 实用工具 (SSRS)](../../reporting-services/tools/rs-exe-utility-ssrs.md)。  
  
> [!TIP]  
>  有关详细示例，请参阅 [用于在报表服务器之间复制内容的示例 Reporting Services rs.exe 脚本](../../reporting-services/tools/sample-reporting-services-rs-exe-script-to-copy-content-between-report-servers.md)。  
  
```  
Public Sub Main()  
        Dim props(0) As [Property]  
        Dim setProp As New [Property]  
        setProp.Name = "EnableClientPrinting"  
        setProp.Value = "False"   
        props(0) = setProp  
        Try  
            rs.SetSystemProperties(props)  
        Catch ex As System.Web.Services.Protocols.SoapException  
            Console.Write(ex.Detail.InnerXml)  
        Catch e as Exception  
            Console.Write(e.Message)  
        End Try  
End Sub  
```

## <a name="next-steps"></a>后续步骤

[GenerateDatabaseCreationScript 方法 (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabasecreationscript.md)   
[GenerateDatabaseRightsScript 方法 (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaserightsscript.md)   
[GenerateDatabaseUpgradeScript 方法 (WMI MSReportServer_ConfigurationSetting)](../../reporting-services/wmi-provider-library-reference/configurationsetting-method-generatedatabaseupgradescript.md)   
[从命令提示符安装 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md)   
[安装 Reporting Services 本机模式报表服务器](~/reporting-services/install-windows/install-reporting-services-native-mode-report-server.md)   
[Reporting Services 报表服务器（本机模式）](../../reporting-services/report-server/reporting-services-report-server-native-mode.md)   
[报表服务器命令提示实用工具 (SSRS)](../../reporting-services/tools/report-server-command-prompt-utilities-ssrs.md)   
[Reporting Services 和 Power View 的浏览器支持](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)   
[Reporting Services 工具](../../reporting-services/tools/reporting-services-tools.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
