---
title: 启用和禁用 Reporting Services 的客户端打印 | Microsoft Docs
description: 了解如何启用或禁用在浏览器中查看的 Reporting Services 报表的客户端打印。 默认情况下，客户端打印功能使用 PDF 格式且是启用的。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- pdf
- viewer
- reportviewer
- toolbar
ms.assetid: 0e709c96-7517-4547-8ef6-5632f8118524
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8cf4544579f4a3258d4bc785c98ada5935653dff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075772"
---
# <a name="enable-and-disable-client-side-printing-for-reporting-services"></a>启用和禁用 Reporting Services 的客户端打印

  报表查看器工具栏上的打印按钮使用可移植文档格式 (PDF) 以实现客户端打印在浏览器中查看的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表。 新的远程打印体验使用 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]附带的 PDF 呈现扩展插件，以便以 PDF 格式呈现报表。 你可以下载 PDF 格式的报表，或者如果安装了用于查看 .PDF 文件的应用程序，则打印按钮会针对页面一般配置项目显示打印对话框，例如 .PDF 文件的页面大小、方向和预览。 虽然默认情况下将启用客户端打印功能，但是您也可以将其禁用，以禁止使用该功能。  
  
 以前版本的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 使用 ActiveX 控件，该控件需要从报表服务器下载到客户端计算机上。 如果将报表服务器升级到 SQL Server 2016 或更高版本，则打印控件不会从报表服务器或客户端计算机上删除。  

##  <a name="the-print-experience"></a><a name="bkmk_clientside_printexpereince"></a> 打印体验  
 单击报表查看器工具栏上的打印 ![htmlviewer_print](../../reporting-services/report-server/media/htmlviewer-print.png "htmlviewer_print") 按钮时，根据客户端计算机上安装的不同 .PDF 查看应用程序以及用户使用的不同浏览器，体验会有所不同。   你可以下载 PDF 文件或从对话框中配置打印选项，或者采用两种方法，具体取决于客户端计算机。  
  
 ![报表工具栏](../../reporting-services/media/ssrs-htmlviewer-toolbar.png "报表工具栏")  
  
|交互|用户界面|  
|-|-|  
|对于所有浏览器而言，第一个对话框都一样，你可以更改基本布局属性，例如方向。 单击“打印” 时，体验会略有不同，具体取决于所使用的浏览器。|![ssrs_pdfprint_chrome1](../../reporting-services/report-server/media/ssrs-pdfprint-chrome1.png "ssrs_pdfprint_chrome1")|  
|在 Chrome 中，将打开详细的浏览器打印对话框。   你可以更改打印配置、打印和打开操作系统打印对话框。|![ssrs_pdfprint_chrome2](../../reporting-services/report-server/media/ssrs-pdfprint-chrome2.png "ssrs_pdfprint_chrome2") ![ssrs_pdfprint_chrome3.png](../../reporting-services/report-server/media/ssrs-pdfprint-chrome3-png.png "ssrs_pdfprint_chrome3.png")|  
|如果安装了 PDF 阅读器应用程序，打印按钮将打开 PDF 文件的预览窗口，以便执行保存或打印功能。||  
|如果没有安装 PDF 阅读器应用程序，有两种用户体验︰<br /><br /> 报表将自动呈现，并使用浏览器下载进程来下载 PDF 文件。   **注意︰** 报表越复杂，则从单击 **打印** 到看到浏览器下载通知的延时越长。 你也可以通过单击“单击此处查看报表的 PDF” 再次强制下载。<br /><br /> 单击“单击此处查看报表的 PDF” 强制下载 PDF。|![ssrs_pdfprint_firefox2](../../reporting-services/report-server/media/ssrs-pdfprint-firefox2.png "ssrs_pdfprint_firefox2")|  
  
##  <a name="troubleshoot-client-side-printing"></a><a name="bkmk_troubleshoot_clientsideprinting"></a> 客户端打印故障排除  
 如果报表查看器工具栏上的打印按钮被禁用，请检查以下事项：  
  
-   在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中禁用了报表服务器的客户端打印功能。 请参阅本主题中的  [启用与禁用客户端打印](#bkmk_enable) 部分。  
  
-   [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] PDF 呈现扩展插件被禁用。 查看 `<Extension Name="PDF"` rsreportserver.config **文件的** 部分。  
  
-   你正在可比性模式下查看，该模式使用旧的 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] HTML4 呈现引擎。 PDF 打印体验需要 HTML 5 呈现引擎。  单击工具栏上的“尝试预览”  按钮。  
  
     ![ssrs_html5_switch2html5](../../reporting-services/report-server/media/ssrs-html5-switch2html5.png "ssrs_html5_switch2html5")  
  
##  <a name="enable-and-disable-client-side-printing"></a><a name="bkmk_enable"></a> 启用与禁用客户端打印  
 报表服务器管理员可以通过将报表服务器系统属性 **EnableClientPrinting** 设置为 **false**，以禁用远程打印功能。 这将对该服务器管理的所有报表禁用客户端打印功能。 默认情况下， **EnableClientPrinting** 设置为 **true**。 您可以通过下列方式禁用客户端打印功能：  
  
-   对于本机模式下的报表服务器 ：  
  
    1.  使用管理权限启动 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 。  
  
    2.  连接到 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中的报表服务器实例。  
  
    3.  右键单击报表服务器节点，然后单击“属性”。 如果 **“属性”** 选项被禁用，请确认您是在使用管理权限启动 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 。  
  
    4.  单击“高级”。  
  
    5.  选择 **EnableClientPrinting** 中禁用了报表服务器的客户端打印功能。  
  
    6.  设置为“True”或“False”，然后单击“确定” 。  
  
         ![ssrs_ssmsproperties_clientprinting](../../reporting-services/report-server/media/ssrs-ssmsproperties-clientprinting.png "ssrs_ssmsproperties_clientprinting")  
  
-   对于 SharePoint 模式报表服务器 ：  
  
    1.  在 SharePoint 管理中心中，单击 **“应用程序管理”** 。  
  
    2.  单击 **“管理服务应用程序”** 。  
  
    3.  单击您的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 服务应用程序的名称，然后单击 SharePoint 功能区中的 **“管理”** 。  
  
    4.  单击 **“系统设置”** 。  
  
    5.  选择 **“启用客户端打印”** 。 **“启用客户端打印”** 选项位于页面的底部附近。  
  
    6.  单击“确定”。  
  
-   编写脚本或代码，将报表服务器系统属性 **EnableClientPrinting** 设置为 **false.** 。  
  
 下面的示例脚本说明了一种禁用客户端打印功能的方法。 编译并运行以下 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 代码，将 EnableClientPrinting 属性设置为 False。 在运行代码后，请重新启动 IIS。  
  
### <a name="sample-script"></a>示例脚本  
  
```  
Imports System  
Imports System.Web.Services.Protocols  
Class Sample  
   Public Shared Sub Main()  
Dim rs As New ReportingService()  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
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
    End Sub 'Main  
End Class 'Sample  
```

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
