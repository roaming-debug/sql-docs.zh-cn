---
title: 在 Windows 应用程序中使用 URL 访问
description: 尽管可以针对 Web 环境优化对报表服务器的 URL 访问，但也可以使用 URL 访问将 Reporting Services 报表嵌入到 Windows 应用程序中。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- Windows applications [Reporting Services]
- Web Browser controls [Reporting Services]
- Windows Forms [Reporting Services]
- browser controls [Reporting Services]
- URL access [Reporting Services], Windows applications
ms.assetid: a4b222e5-0cbd-409c-92c4-046a674db8ac
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2a3f301fa5e9395c86a3a771a959584acd181938
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016967"
---
# <a name="integrating-reporting-services-using-url-access---windows-application"></a>使用 URL 访问集成 Reporting Services - Windows 应用程序
  尽管可以针对 Web 环境优化对报表服务器的 URL 访问，但也可以使用 URL 访问将 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表嵌入到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 应用程序中。 不过，涉及 Windows 窗体的 URL 访问仍然要求您使用 Web 浏览器技术。 您可以将以下集成方案用于 URL 访问和 Windows 窗体：  
  
-   通过以编程方式启动 Web 浏览器，从 Windows 窗体应用程序显示报表。  
  
-   使用 Windows 窗体上的 <xref:System.Windows.Forms.WebBrowser> 控件显示报表。  
  
## <a name="starting-internet-explorer-from-a-windows-form"></a>从 Windows 窗体启动 Internet Explorer  
 可以使用 <xref:System.Diagnostics.Process> 类访问正在计算机上运行的进程。 对于启动、停止、控制和监视应用程序等任务，<xref:System.Diagnostics.Process> 类是一个很有用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 构造。 若要查看报表服务器数据库中的特定报表，可以启动 IExplore 进程，同时将 URL 传递给报表  。 以下代码示例可用于在用户单击 Windows 窗体上的某个按钮时，启动 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer 并传递特定的报表 URL。  
  
```vb  
Private Sub viewReportButton_Click(ByVal sender As Object, ByVal e As System.EventArgs) Handles viewReportButton.Click  
   ' Build the URL access string based on values supplied by a user  
   Dim url As String = serverUrlTextBox.Text + "?" & reportPathTextBox.Text & _  
   "&rs:Command=Render" & "&rs:Format=HTML4.0"  
  
   ' If the user does not select the toolbar check box,  
   ' turn the toolbar off in the HTML Viewer  
   If toolbarCheckBox.Checked = False Then  
      url += "&rc:Toolbar=False"  
   End If  
   ' load report in the Web browser  
   Try  
      System.Diagnostics.Process.Start("IExplore", url)  
   Catch  
      MessageBox.Show("The system could not start the specified report using Internet Explorer.", _  
      "An error has occurred", MessageBoxButtons.OK, MessageBoxIcon.Error)  
   End Try  
End Sub 'viewReportButton_Click  
```  
  
```csharp  
// Sample click event for a Button control on a Windows Form  
private void viewReportButton_Click(object sender, System.EventArgs e)  
{  
   // Build the URL access string based on values supplied by a user  
   string url = serverUrlTextBox.Text + "?" + reportPathTextBox.Text +  
      "&rs:Command=Render" + "&rs:Format=HTML4.0";  
  
   // If the user does not check the toolbar check box,  
   // turn the toolbar off in the HTML Viewer  
   if (toolbarCheckBox.Checked == false)  
      url += "&rc:Toolbar=False";  
  
   // load report in the Web browser  
   try  
   {  
      System.Diagnostics.Process.Start("IExplore", url);  
   }  
  
   catch (Exception)  
   {  
      MessageBox.Show(  
         "The system could not open the specified report using Internet Explorer.",   
         "An error has occurred", MessageBoxButtons.OK, MessageBoxIcon.Error);  
   }  
}  
```  
  
## <a name="embedding-a-browser-control-on-a-windows-form"></a>在 Windows 窗体中嵌入浏览器控件  
 如果您不想在外部 Web 浏览器中查看报表，可以使用 <xref:System.Windows.Forms.WebBrowser> 控件将 Web 浏览器作为 Windows 窗体的一部分无缝嵌入。  
  
###### <a name="to-add-the-webbrowser-control-to-your-windows-form"></a>将 WebBrowser 控件添加到 Windows 窗体  
  
1.  在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] 或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] 中创建一个新的 Windows 应用程序。  
  
2.  找到“工具箱”对话框中的 <xref:System.Windows.Forms.WebBrowser> 控件  。  
  
     如果不显示“工具箱”，可以通过单击“视图”菜单项并选择“工具箱”来访问它    。  
  
3.  将 <xref:System.Windows.Forms.WebBrowser> 控件拖到 Windows 窗体的设计图面上。  
  
     名为 webBrowser1 的 <xref:System.Windows.Forms.WebBrowser> 控件被添加到窗体中  
  
 可以通过调用其 Navigate 方法将 <xref:System.Windows.Forms.WebBrowser> 控件定向到 URL  。 可以在运行时将特定的 URL 访问字符串分配给 <xref:System.Windows.Forms.WebBrowser> 控件，如下面的示例所示。  
  
```vb  
Dim url As String = "https://localhost/reportserver?/" & _  
                    "AdventureWorks2012 Sample Reports/" & _  
                    "Company Sales&rs:Command=Render"  
WebBrowser1.Navigate(url)  
```  
  
```csharp  
string url = "https://localhost/reportserver?/" +  
             "AdventureWorks2012 Sample Reports/" +  
             "Company Sales&rs:Command=Render";  
webBrowser1.Navigate(url);  
```  
  
## <a name="see-also"></a>另请参阅  
 [将 Reporting Services 集成到应用程序中](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [使用 URL 访问集成 Reporting Services](../../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)   
 [使用 SOAP 集成 Reporting Services](../../reporting-services/application-integration/integrating-reporting-services-using-soap.md)   
 [使用 ReportViewer 控件集成 Reporting Services](../../reporting-services/application-integration/integrating-reporting-services-using-reportviewer-controls.md)   
 [URL 访问 (SSRS)](../../reporting-services/url-access-ssrs.md)  
  
  
