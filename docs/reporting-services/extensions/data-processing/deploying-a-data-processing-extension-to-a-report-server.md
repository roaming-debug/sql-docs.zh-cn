---
title: 如何：向报表服务器部署数据处理扩展插件 | Microsoft Docs
description: 通过学习将哪些条目添加到哪些配置文件中，了解如何向报表服务器部署数据处理扩展插件。
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- assemblies [Reporting Services], data processing extension deployments
ms.assetid: e00dface-70f8-434b-9763-8ebee18737d2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1aadbda9a8842777b395612c59d753d3afb875ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060952"
---
# <a name="deploying-a-data-processing-extension-to-a-report-server"></a>向报表服务器部署数据处理扩展插件
  报表服务器使用数据处理扩展插件来检索和处理所呈现报表中的数据。 您应将数据处理扩展插件程序集作为私有程序集部署到报表服务器。 还需要在报表服务器配置文件 RSReportServer.config 中生成一个条目。  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-deploy-a-data-processing-extension-assembly"></a>部署数据处理扩展插件程序集  
  
1.  将程序集从临时位置复制到您要在其上使用数据处理扩展插件的报表服务器的 bin 目录中。 报表服务器 bin 目录的默认位置为 %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<*Instance Name*>\Reporting Services\ReportServer\bin。  
  
    > [!NOTE]  
    >  此步骤会妨碍升级到较新的 SQL Server 实例。 有关详细信息，请参阅 [Upgrade and Migrate Reporting Services](../../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)。  
  
2.  在复制程序集文件后，打开 RSReportServer.config 文件。 RSReportServer.config 文件位于 ReportServer 目录中。 还需要在配置文件中为数据处理扩展插件程序集文件生成一个条目。 可以使用 Visual Studio 或诸如记事本之类的简单文本编辑器打开此配置文件。  
  
3.  在 RSReportServer.config 文件中找到 **Data** 元素。 应当在以下位置为新创建的数据处理扩展插件生成一个条目：  
  
    ```  
    <Extensions>  
       <Data>  
          <Your extension configuration information goes here>  
       </Data>  
    </Extensions>  
    ```  
  
4.  为数据处理扩展插件添加一个条目。 条目应包含一个具有 Name 和 Type 值的 Extension 元素，可能如下所示  ：  
  
    ```  
    <Extension Name="ExtensionName" Type="CompanyName.ExtensionName.MyConnectionClass, MyExtensionAssembly" />  
    ```  
  
     Name 的值是数据处理扩展插件的唯一名称。 Type 的值是以逗号分隔的列表，包括实现 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 和 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> 接口的类的完全限定命名空间的条目，后跟程序集的名称（不包括 .dll 文件扩展名）。 默认情况下，数据处理扩展插件是可见的。 若要从用户界面（如报表管理器）中隐藏扩展插件，请将 **Visible** 属性添加到 **Extension** 元素，并将其设置为 **false**。  
  
5.  最后，为自定义程序集添加一个代码组，以便为你的扩展插件授予 FullTrust 权限。 可以通过将代码组添加到 rssrvpolicy.config 文件来完成此操作。该文件的默认位置为 %ProgramFiles%\Microsoft SQL Server\\<MSRS10_50.\<*Instance Name*>\Reporting Services\ReportServer。 代码组可能如下所示：  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="MyExtensionCodeGroup"  
       Description="Code group for my data processing extension">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.<Instance Name>\Reporting Services\ReportServer\bin\MyExtensionAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 URL 成员身份只是您可以为数据处理扩展插件选择的众多成员条件之一。 有关 [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 中的代码访问安全性的详细信息，请参阅[安全开发 (Reporting Services)](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)。  
  
## <a name="verifying-the-deployment"></a>验证部署  
 您可以使用 Web 服务 <xref:ReportService2010.ReportingService2010.ListExtensions%2A> 方法来验证是否已将数据处理扩展插件成功部署到报表服务器中。 您也可以打开报表管理器，验证扩展插件是否包括在可用数据源列表中。 有关报表管理器和数据源的详细信息，请参阅[创建、修改和删除共享数据源 (SSRS)](../../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)。  
  
## <a name="see-also"></a>另请参阅  
 [部署数据处理扩展插件](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Reporting Services 扩展插件](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [实现数据处理扩展插件](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
