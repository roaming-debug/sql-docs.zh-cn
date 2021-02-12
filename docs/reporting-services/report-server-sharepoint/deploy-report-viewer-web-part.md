---
title: 在 SharePoint 网站上部署 SQL Server Reporting Services 报表查看器 Web 部件 | Microsoft Docs
description: 对于 SQL Server Reporting Services，可以手动将报表查看器自定义 Web 部件添加到 SharePoint 产品中。
ms.date: 11/15/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4b8638e0c9a958e8f5dd6b6039540e8492555b03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074818"
---
# <a name="deploy-the-sql-server-reporting-services-report-viewer-web-part-on-a-sharepoint-site"></a>在 SharePoint 网站上部署 SQL Server Reporting Services 报表查看器 Web 部件

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

报表查看器 Web 部件是一个自定义 Web 部件，可用于查看 SharePoint 网站中的 SQL Server Reporting Services（本机模式）报表。 可以使用此 Web 部件在报表服务器上查看、导航、打印和导出报表。 报表查看器 Web 部件与由 SQL Server Reporting Services 报表服务器或 Power BI 报表服务器处理的报表定义 (.rdl) 文件相关联。 此报表查看器 Web 部件不能用于 Power BI 报表服务器中托管的 Power BI 报表。

使用以下说明以手动部署解决方案包，该解决方案包会将报表查看器 Web 部件添加到 SharePoint Server 2013、SharePoint Server 2016 或 SharePoint Server 2019 环境中。 部署解决方案是配置 Web 部件的必需步骤。

**报表查看器 Web 部件是独立的解决方案包，不与 SQL Server Reporting Services 的 SharePoint 集成模式相关联。**

## <a name="requirements"></a>要求

> [!IMPORTANT]
> 从版本“15.X.X.X”开始，可以并行安装 ReportViewerWebPart 和现有的 Reporting Services SharePoint 集成模式共享服务应用程序。
> 此次 .wsp 解决方案更新，我们引入了新的文件，必须分别使用 Uninstall-SPSolution 和 Install-SPSolution cmdlet 撤销之前的解决方案和重复部署新的 .wsp。
>

**支持的 SharePoint Server 版本：**
* SharePoint Server 2019
* SharePoint Server 2016
* SharePoint Server 2013

**支持的 Reporting Services 版本：**  
* SQL Server 2008 Reporting Services（本机模式）和更高版本。
* Power BI 报表服务器

## <a name="download-the-report-viewer-web-part-solution-package"></a>下载报表查看器 Web 部件解决方案包

可从 Microsoft 下载中心下载报表查看器 Web 部件。

[下载报表查看器 Web 部件解决方案包](https://www.microsoft.com/download/details.aspx?id=55949)

## <a name="deploy-the-farm-solution"></a>部署场解决方案

本部分演示如何将解决方案包部署到 SharePoint 场。 此任务仅需执行一次。

1. 在 SharePoint Server 上，使用“以管理员身份运行”选项打开 SharePoint 命令行管理程序。

2. 运行 [Add-SPSolution](/powershell/module/sharepoint-server/Add-SPSolution) 以添加场解决方案。

    ```
    Add-SPSolution -LiteralPath "{path to file}\ReportViewerWebPart.wsp"
    ```

    该 cmdlet 返回解决方案的名称、其解决方案 ID 和 Deployed=False。 在下一步骤中，您将部署解决方案。

3. 运行 [Install-SPSolution](/powershell/module/sharepoint-server/Install-SPSolution) cmdlet 以部署场解决方案。

    **SharePoint 2013**

    ```
    Install-SPSolution -Identity ReportViewerWebPart.wsp -CompatibilityLevel "14,15" -GACDeployment -WebApplication {URL to web application}
    ```

    **SharePoint Server 2016 和 2019**

    ```
    Install-SPSolution -Identity ReportViewerWebPart.wsp -GACDeployment -WebApplication {URL to web application}
    ```

## <a name="activate-feature"></a>激活功能

1. 在 SharePoint 网站中，选择左上角的“齿轮”图标，然后选择“网站设置” 。

    ![齿轮图标中的“网站设置”。](media/sharepoint-site-settings.png)

    默认情况下，通过端口 80 访问 SharePoint Web 应用程序。 这意味着通常可通过输入 https://<computer name> 打开根网站集以访问 SharePoint 网站。

3. 在“网站集管理”中，选择“网站集功能” 。

4. 向下滚动页面，直至找到“报表查看器 Web 部件”功能为止。

5. 选择“激活”。

    ![激活报表查看器 Web 部件功能](media/web-part-activiate-feature.png)

6. 通过打开各网站并单击“网站操作”来对其他网站集重复上述操作。

或者，还可使用 PowerShell 通过 [ Enable-SPFeature](/powershell/module/sharepoint-server/Enable-SPFeature) cmdlet 在所有网站上启用此功能。

```
Get-SPWebApplication "<web application url>" | Get-SPSite -Limit ALL | 
        ForEach-Object {
            Write-Host "Enabling feature for $($_.URL)"
            Enable-SPFeature -identity "ReportViewerWebPart" -URL $_.URL -ErrorAction Continue
        }
```

## <a name="remove-the-solution"></a>删除解决方案

尽管 SharePoint 管理中心提供了解决方案收回，但除非是在系统地解决安装问题或修补部署问题，否则便无需收回 ReportViewerWebPart.wsp 文件。

1. 在 SharePoint 管理中心的“系统设置”中，选择“管理场解决方案” 。

2. 选择“ReportViewerWebPart.wsp”。

3. 选择“收回解决方案”。

### <a name="remove-the-web-part-from-site-settings"></a>从网站设置中删除 Web 部件

收回解决方案不会从 SharePoint 网站内的 Web 部件列表中删除报表查看器 Web 部件。 若要删除报表查看器 Web 部件，请执行以下操作。

1. 在 SharePoint 网站中，选择左上角的“齿轮”图标，然后选择“网站设置” 。

    ![齿轮图标中的“网站设置”。](media/sharepoint-site-settings.png)

    默认情况下，通过端口 80 访问 SharePoint Web 应用程序。 这意味着通常可通过输入 https://<computer name> 打开根网站集以访问 SharePoint 网站。

2. 在“Web 设计器库”下，选择“Web 部件” 。

3. 选择“ReportViewerNativeMode.dwp”旁边的“编辑图标” 。 它可能没有列在结果的第一页上。

4. 选择“删除项”。

    ![编辑并删除报表查看器本机模式 Web 部件](media/report-viewer-native-mode-edit-delete.png)

可以使用 PowerShell 尝试删除 Web 部件，但没有直接的命令用于此操作。 有关脚本示例，请参阅 [How to delete web parts from the web part Gallery](https://gallery.technet.microsoft.com/office/How-to-delete-Web-Parts-1132701f)（如何从 Web 部件库中删除 Web 部件）。

## <a name="supported-languages"></a>支持的语言

Web 部件支持以下语言：

* 英语 (en)
* 德语 (de)
* 西班牙语 (sp)
* 法语 (fr)
* 意大利语 (it)
* 日语 (ja)
* 韩语 (ko)
* 葡萄牙语 (pt)
* 俄语 (ru)
* 中文（简体 - zh-HANS 和 zh-CHS）
* 中文（繁体 - zh-HANT 和 zh-CHT）

## <a name="troubleshoot"></a>疑难解答

* 如果已配置 SharePoint 集成模式，则卸载 SSRS 时出错：

    Install-SPRSService：[A] Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService 无法强制转换为 [B]Microsoft.ReportingServices.SharePoint.SharedService.Service.ReportingWebService。 A 类型来自“C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll”位置处上下文“Default”中的“Microsoft.ReportingServices.SharePoint.SharedService,Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91”。 B 类型来自“C:\Windows\assembly\GAC_MSIL\Microsoft.Reporting Services.SharePoint.SharedService.dll”位置处上下文“Default”中的“Microsoft.ReportingServices.SharePoint.SharedService,Version=12.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91”。
    
    解决方案：
    1. 删除报表查看器 Web 部件
    2. 卸载 SSRS
    3. 重新安装报表查看器 Web 部件

* 如果已配置 SharePoint 集成模式，则尝试升级 SharePoint 时出错：

    无法加载文件或程序集“Microsoft.ReportingServices.Alerting.ServiceContract, Version=14.0.0.0, Culture=neutral, PublicKeyToken=89845dcd8080cc91”或其依赖项之一。 系统找不到指定的文件。 00000000-0000-0000-0000-000000000000
    
    解决方案：
    1. 删除报表查看器 Web 部件
    2. 卸载 SSRS
    3. 重新安装报表查看器 Web 部件

## <a name="next-steps"></a>后续步骤

在部署并激活报表查看器 Web 部件后，可将 Web 部件添加到 SharePoint 页面。 有关详细信息，请参阅[将报表查看器 Web 部件添加到 SharePoint 页面](add-report-viewer-web-part-to-page.md)。

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)