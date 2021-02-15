---
title: 在自定义应用程序中使用 RSClientPrint 控件 | Microsoft Docs
description: RSPrintClient 为在 HTML 查看器中查看的报表提供客户端打印功能。 报表服务器在图像 (EMF) 呈现扩展中呈现报表。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- RSPrintClient control
- print controls [Reporting Services]
- custom printing [Reporting Services]
- client-side printing
ms.assetid: 8c0bdd18-8905-4e22-9774-a240fc81a8a7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c977799cb20d3735b4cc77d2608c0412ce51769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022215"
---
# <a name="using-the-rsclientprint-control-in-custom-applications"></a>在自定义应用程序中使用 RSClientPrint 控件
  对于在 HTML 查看器中查看的报表，[!INCLUDE[msCoName](../../../includes/msconame-md.md)] ActiveX 控件 RSPrintClient 为其提供了客户端打印功能  。 通过该控件提供的“打印”对话框，用户可以启动打印作业、预览报表、指定要打印的页面以及更改边距  。 在客户端打印操作过程中，报表服务器通过图像 (EMF) 呈现扩展插件呈现报表，使用操作系统的打印功能创建打印作业并将作业发送到打印机。  
  
 通过客户端打印功能，可以避开用户计算机上的浏览器打印设置，改用报表的页面尺寸、边距、表头文本和表尾文本来创建打印输出，从而控制和提高 HTML 报表的打印输出质量。 该打印控件通过读取报表的属性值来设置页大小和边距。  
  
 开发人员如果想要在第三方工具栏或查看器中启用客户端打印功能，可以通过 RSClientPrint COM 对象访问该 ActiveX 控件  。 该控件可以自由分发。 下表列出了使用该控件的建议：  
  
-   使用该控件提高基于 Web 的报表的打印质量。 可以使用与 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 兼容的任何编程语言或脚本指定对象。 该控件不用于 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 窗体应用程序。  
  
-   从 [!INCLUDE[ssRSNoVersion](../../../includes/ssrsnoversion-md.md)] 程序文件复制 .cab 文件，并将其添加到自定义应用程序基本代码中。  
 
-   使用 \<OBJECT> 标记指定该控件。  
  
-   在 OBJECT CODEBASE 属性中为 .cab 文件指定相对 URL 或完全限定的 URL。  
  
-   为 .cab 文件指定您自己的应用程序版本信息，以跟踪在您的应用程序中使用的版本。  
    
  
## <a name="rsprintclient-overview"></a>RSPrintClient 概述  
 该控件显示一个自定义打印对话框，它支持其他打印对话框常见的功能，包括打印预览、指定特定页和范围的页面选择、页边距和打印方向等功能。 该控件打包为 CAB 文件。 “打印”对话框中的文本已本地化为 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中支持的所有语言。 RSPrintClient ActiveX 控件使用图像呈现扩展插件 (EMF) 打印报表  。 使用的 EMF 设备信息包括：StartPage、EndPage、MarginBottom、MarginLeft、MarginTop、MarginRight、PageHeight 和 PageWidth。 不支持图像呈现的其他设备信息设置。  
  
### <a name="language-support"></a>语言支持  
 该打印控件可以提供不同语言的用户界面文本，接受符合不同度量系统标准的输入值。 所用的语言和度量系统由 Culture 和 UICulture 属性确定   。 这两个属性都接受 LCID 值。 如果为所支持语言的变体语言指定 LCID，则会应用最接近的匹配语言。 如果不支持指定的 LCID，并且没有最接近的匹配 LCID，则会应用英语（美国）。  
  
## <a name="using-rsclientprint-in-code"></a>在代码中使用 RSClientPrint  
 RSClientPrint 对象用于以编程方式获取对 ActiveX 控件及其方法和属性的访问权  。 该控件为打印预览提供了一个模式对话框。  
  
### <a name="specifying-default-values"></a>指定默认值  
 可使用报表的边距和页值初始化“打印”对话框  。 默认情况下，使用报表定义中的值初始化“打印”对话框  。 您可以使用这些默认值，也可以通过设置对象属性来指定不同的值。  
  
 所有尺寸都是以毫米为单位进行设置的。 如果将 Culture 和 UICulture 设置为不使用公制度量单位的区域，则在运行时会进行度量换算   。  
  
 若要了解哪些值用于页面尺寸和边距，可以使用 GetProperties 方法检索默认值  ：  
  
-   PageHeight 和 PageWidth 用于指定默认的页高和页宽   。 在启动该打印控件时，将使用这些属性值选择可用于当前所选打印机的最接近的纸张大小。 如果 PageWidth 大于 PageHeight，方向将设置为“横向”   。 否则，方向设置为“纵向”。  
  
-   默认情况下，LeftMargin、RightMargin、TopMargin 和 BottomMargin 均设置为 12.2 毫米     。  
  
 这些属性存储在报表服务器上的 Item 属性集合中  。 每次更新报表定义时，都将覆盖这些值。  
  
### <a name="rsclientprint-properties"></a>RSClientPrint 属性  
  
|properties|类型|RW|默认|说明|  
|--------------|----------|--------|-------------|-----------------|  
|MarginLeft|Double|RW|报表设置|获取或设置左边距。 如果开发人员没有设置或报表中未指定，则默认值为 12.2 毫米。|  
|MarginRight|Double|RW|报表设置|获取或设置右边距。 如果开发人员没有设置或报表中未指定，则默认值为 12.2 毫米。|  
|MarginTop|Double|RW|报表设置|获取或设置上边距。 如果开发人员没有设置或报表中未指定，则默认值为 12.2 毫米。|  
|MarginBottom|Double|RW|报表设置|获取或设置下边距。 如果开发人员没有设置或报表中未指定，则默认值为 12.2 毫米。|  
|PageWidth|Double|RW|报表设置|获取或设置页宽。 如果开发人员或报表定义中未进行设置，则默认值为 215.9 毫米。|  
|PageHeight|Double|RW|报表设置|获取或设置页高。 如果开发人员或报表定义中未进行设置，则默认值为 279.4 毫米。|  
|环境|Int32|RW|浏览器区域设置|指定区域设置标识符 (LCID)。 此值将确定用户输入的度量单位。 例如，如果用户键入 3，则在语言为法语时，值将按毫米度量；在语言为英语（美国）时，值将按英寸度量  。 有效值包括：1028、1031、1033、1036、1040、1041、1042、2052、3082。|  
|UICulture|String|RW|客户端区域性|指定对话框字符串的本地化语言。 “打印”对话框中的文本已本地化为以下语言：简体中文、繁体中文、英语、法语、德语、意大利语、日语、朝鲜语和西班牙语。 有效值包括：1028、1031、1033、1036、1040、1041、1042、2052、3082。|  
|Authenticate|Boolean|RW|False|指定控件是否向报表服务器发出 GET 命令，以启动无会话打印连接。|  
  
### <a name="when-to-set-the-authenticate-property"></a>何时设置 Authenticate 属性  
 从浏览器会话中进行打印时，无需设置 Authenticate 属性  。 在活动会话的上下文中，打印控件对报表服务器的所有请求都是通过浏览器来处理的。 浏览器会设置与报表服务器通信所需的会话变量。  
  
 如果进行无会话打印（例如，将报表直接发送至打印机，而不是首先打开该报表），打印控件必须发出 HTTP GET 请求，以便与报表服务器建立会话  。 若要发布 GET 请求，请将 Authenticate 设置为 True    。  
  
 仅当使用 Windows 集成安全性或基本身份验证时，才需要发布 GET 请求  。 如果使用的是窗体身份验证，将忽略 Authenticate 属性  。 此时，应用程序代码必须使用您提供的自定义安全扩展插件来设置会话及验证用户身份。 如果使用窗体身份验证，请确保针对身份验证 cookie 设置的过期值可使会话保留一段合理的时间。 如果该值过低，每当 cookie 过期时，系统都将提示用户提供登录凭据。  
  
### <a name="clsids"></a>CLSID  
 当在内部运行报表时，使用以下 CLSID 值。  
  
-   41861299-EAB2-4DCC-986C-802AE12AC499  
  
-   5554DCB0-700B-498D-9B58-4E40E5814405  
  
-   60677965-AB8B-464f-9B04-4BA871A2F17F  
  
 当在 Azure SQL 报告中运行报表时，使用以下 CLSID 值。  
  
-   3DD32426-554D-48C0-A200-65D3BF880E38  
  
-   05662494-ACF9-446A-BE4C-7D3F7EA7F62F  
  
### <a name="rsprintclient-support-for-the-print-method"></a>RSPrintClient 对 Print 方法的支持  
 RSClientPrint 对象支持用于启动“打印”对话框的 Print 方法   。 Print 方法具有以下参数  。  
  
|参数|I/O|类型|说明|  
|--------------|----------|----------|-----------------|  
|ServerPath|In|String|指定报表服务器虚拟目录（例如，`https://adventure-works/reportserver`）。|  
|ReportPathParameters|In|String|指定报表在报表服务器文件夹命名空间中的全名，包括参数。 对报表的检索是通过 URL 访问进行的。 例如：“/AdventureWorks Sample Reports/Employee Sales Summary&EmpID=1234”|  
|ReportName|In|String|报表的简称（在上面的示例中，简称为 Employee Sales Summary）。 它显示在“打印”对话框和打印队列中。|  
  
### <a name="example"></a>示例  
 下面的 HTML 示例显示如何在 JavaScript 中指定 .cab 文件、Print 方法和属性  ：  
  
 `<BODY onload="Print()">`  
  
 `<OBJECT ID="RSClientPrint" CLASSID="CLSID: 5554DCB0-700B-498D-9B58-4E40E5814405D3" CODEBASE="<URL to the .CAB file>#Version=<your application version information>" VIEWASTEXT></OBJECT>`  
  
 `<script language="javascript">`  
  
 `function Print()`  
  
 `{`  
  
 `RSClientPrint.MarginLeft = 12.7;`  
  
 `RSClientPrint.MarginTop = 12.7;`  
  
 `RSClientPrint.MarginRight = 12.7;`  
  
 `RSClientPrint.MarginBottom = 12.7;`  
  
 `RSClientPrint.Culture = 1033;`  
  
 `RSClientPrint.UICulture = 9;`  
  
 `RSClientPrint.Print('https://localhost/rtm', '%2fEmployee_Sales_Summary&ReportMonth=6&ReportYear=2004&EmpID=20', 'Employee_Sales_Summary')`  
  
 `}`  
  
 `</script>`  
  
 `</BODY>`  
  
## <a name="see-also"></a>另请参阅  
 [使用打印控件从浏览器中打印报表（报表生成器和 SSRS）](../../../reporting-services/report-builder/print-reports-from-a-browser-with-the-print-control-report-builder-and-ssrs.md)   
 [打印报表（报表生成器和 SSRS）](../../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
 [图像设备信息设置](../../../reporting-services/image-device-information-settings.md)  
  
  
