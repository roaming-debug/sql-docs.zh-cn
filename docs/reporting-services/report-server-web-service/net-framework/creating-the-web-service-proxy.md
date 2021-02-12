---
title: 创建 Web 服务代理 | Microsoft Docs
description: 客户端和 Web 服务可以使用 SOAP 消息进行通信。 将代理类添加到项目中，以将参数映射到 XML 元素并发送 SOAP 消息。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- Report Server Web service, proxies
- proxies [Reporting Services]
- XML Web service [Reporting Services], proxies
- Web service [Reporting Services], proxies
- Web references [Reporting Services]
ms.assetid: b1217843-8d3d-49f3-a0d2-d35b0db5b2df
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8f2f9f3986697f83e5b9bb5616f30f4f1086373f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063752"
---
# <a name="creating-the-web-service-proxy"></a>创建 Web 服务代理
  客户端和 Web 服务可以通过 SOAP 消息进行通信，这些消息将输入参数和输出参数封装为 XML。 代理类将参数映射到 XML 元素，然后通过网络发送 SOAP 消息。 通过这种方法，代理类使您不必在 SOAP 级别与 Web 服务通信，并允许您在支持 SOAP 和 Web 服务代理的任何开发环境中调用 Web 服务方法。  
  
 可以通过两种方法，使用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 将代理类添加到开发项目中：使用 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 中的 WSDL 工具和在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 中添加 Web 引用。 下列各节更为详细地讨论这一主题。  
  
## <a name="adding-the-proxy-using-the-wsdl-tool"></a>使用 WSDL 工具添加代理  
 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] SDK 包括 Web 服务描述语言工具 (Wsdl.exe)，它使得您能够生成 Web 服务代理以用于 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 开发环境中。 在支持 Web 服务的语言（当前为 C# 和 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]）中创建客户端代理的最常用方法是使用 WSDL 工具。  
  
 **使用 Wsdl.exe 将代理类添加到项目中**  
  
1.  从命令提示符下，使用 Wsdl.exe 创建代理类，同时指定（至少）指向报表服务器 Web 服务的 URL。  
  
     例如，下面的命令提示符语句为报表服务器 Web 服务的管理端点指定 URL。  
  
    ```  
    wsdl /language:CS /n:"Microsoft.SqlServer.ReportingServices2010" https://<Server Name>/reportserver/reportservice2010.asmx?wsdl  
    ```  
  
     WSDL 工具接受多种用于生成代理的命令提示符参数。 前一示例指定语言 C# 和建议在代理中使用的一个命名空间（以防止在使用多个 Web 服务端点时出现名称冲突），并生成一个名为 ReportingService2010.cs 的 C# 文件。 如果本示例已指定 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)]，则该示例应已生成名为 ReportingService2010.vb 的代理文件。 将在您运行此命令的目录中创建此文件。  
  
2.  将代理类编译为程序集文件（具有扩展名 .dll）并在项目中引用它，或者将该类添加为一个项目项。  
  
    > [!NOTE]  
    >  当您手动将代理类添加到项目时，您需要添加对于 System.Web.Services.dll 的引用。 如果您在 Visual Studio .NET 中使用 Web 引用添加代理，则将自动为您创建引用。 有关详细信息，请参阅本主题后面的“在 Visual Studio 中使用 Web 引用添加代理”。  
  
     在将代理类作为项添加到项目后，关联的文件将出现在解决方案资源管理器中。  
  
3.  若要以编程方式调用此服务，应创建代理类的实例。  
  
     以下代码示例显示在项目中创建 <xref:ReportService2010.ReportingService2010> 代理类的实例的语法：  
  
```vb  
Dim service As New ReportingService2010()  
```  
  
```csharp  
ReportingService2010 service = new ReportingService2010();  
  
```  
  
 有关 Wsdl.exe 工具的详细信息（包括其完整语法），请参阅 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] SDK 文档中的“Web 服务描述语言工具”。 有关 Web 服务代理的完整解释，请参阅 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] SDK 文档中的“创建 XML Web 服务代理”。  
  
## <a name="adding-the-proxy-using-a-web-reference-in-visual-studio"></a>在 Visual Studio 中使用 Web 引用添加代理  
 Web 引用使一个项目可以使用一个或多个 Web 服务。 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 使用户可以通过以下几个简单步骤向项目添加 Web 服务引用。  
  
 **将 Web 引用添加到项目**  
  
1.  在“解决方案资源管理器”  中，选择要使用 Web 服务的项目。  
  
2.  在“项目”  菜单中，单击“添加 Web 引用”  。  
  
     “添加 Web 引用”  对话框打开。  
  
3.  在“URL”  字段中，输入指向报表服务器 Web 服务的完整路径。  
  
     报表服务器 Web 服务的报表执行端点的简化 URL 可能如下所示：  
  
    ```  
    https://<Server Name>/reportserver/reportexecution2005.asmx  
    ```  
  
     此 URL 包含在其中部署报表服务器 Web 服务的域、包含该服务的文件夹的名称以及该服务的发现文件的名称。 有关不同 URL 元素的完整说明，请参阅[访问 SOAP API](../../../reporting-services/report-server-web-service/accessing-the-soap-api.md)。  
  
     由 Web 服务提供的方法和属性的说明将出现在“浏览器”窗格的左侧。  
  
    > [!NOTE]  
    >  有关与报表服务器 Web 服务关联的项的详细信息，请参阅[报表服务器 Web 服务方法](../../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)。  
  
4.  验证项目是否可以使用报表服务器 Web 服务，以及您是否具有适当的权限访问报表服务器。  
  
5.  在“Web 引用名”  字段中输入一个名称，将在代码中使用该名称以编程方式访问报表服务器 Web 服务。  
  
6.  选择“添加引用”  按钮，以在应用程序中创建对 Web 服务的引用。  
  
     新引用将出现在“解决方案资源管理器”  中处于活动状态的项目的“Web 引用”节点下，其名称在“Web 引用名”  字段中指定。  
  
7.  在“解决方案资源管理器”  中，展开“Web 引用”文件夹，以记下与可用于项目中的项的 Web 引用类对应的命名空间。  
  
     在将 Web 引用添加到项目后，关联的文件将显示在“解决方案资源管理器”  的“Web 引用”文件夹内的某个文件夹中。  
  
 在添加 Web 引用之后，使用以下语法创建代理类的实例：  
  
```vb  
Dim rs As New myNamespace.myReferenceName.ReportExecutionService()  
rs.Url = "https://<Server Name>/reportserver/reportexecution2005.asmx?wsdl"  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
```  
  
```csharp  
myNamespace.myReferenceName.ReportExecutionService rs = new myNamespace.myReferenceName.ReportExecutionService();  
rs.Url = "https://<Server Name>/reportserver/reportexecution2005.asmx?wsdl";  
rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
```  
  
 还可以将“using”（在 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] 中为“Import”）指令添加到报表服务器 Web 服务引用中。 如果您使用该指令，则不必完全限定命名空间中的类型。 为此，请在文件中添加以下代码：  
  
```vb  
Import myNamespace.myReferenceName  
```  
  
```csharp  
using myNamespace.myReferenceName;  
```  
  
## <a name="see-also"></a>另请参阅  
 [报表服务器 Web 服务](../../../reporting-services/report-server-web-service/report-server-web-service.md)   
 [使用 Web 服务和 .NET Framework 生成应用程序](../../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [技术参考 (SSRS)](../../../reporting-services/technical-reference-ssrs.md)  
  
  
