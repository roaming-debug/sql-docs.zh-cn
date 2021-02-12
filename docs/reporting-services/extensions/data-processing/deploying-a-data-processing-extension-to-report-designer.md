---
title: 如何向报表设计器部署数据处理扩展插件 | Microsoft Docs
description: 通过学习将哪些条目添加到哪些配置文件中，了解如何向报表设计器部署数据处理扩展插件。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- assemblies [Reporting Services], data processing extension deployments
ms.assetid: 3614e601-004e-4a16-8388-836ffd67e9dd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ee0f4c58af897bacf929f0447141256a30682990
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065393"
---
# <a name="deploying-a-data-processing-extension-to-report-designer"></a>向报表设计器部署数据处理扩展插件
  报表设计器在您设计报表时使用数据处理扩展插件检索和处理数据。 您应将数据处理扩展插件程序集作为专用程序集部署到报表设计器。 还需要在报表设计器配置文件 RSReportDesigner.config 中生成一个条目。  
  
#### <a name="to-deploy-a-data-processing-extension-assembly"></a>部署数据处理扩展插件程序集  
  
1.  将程序集从临时位置复制到报表设计器目录中。 报表服务器目录的默认位置为 C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies。  
  
2.  在复制程序集文件后，打开 RSReportDesigner.config 文件。 RSReportDesigner.config 文件也位于报表设计器目录中。 还需要在配置文件中为数据处理扩展插件程序集文件生成一个条目。 可以使用 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 或简单文本编辑器（如记事本）打开配置文件。  
  
3.  在 RSReportDesigner.config 文件中找到 **Data** 元素。 应当在以下位置为新创建的数据处理扩展插件生成一个条目：  
  
    ```  
    <Extensions>  
       <Data>  
          <Your extension configuration information goes here>  
       </Data>  
    </Extensions>  
    ```  
  
4.  为数据处理扩展插件添加一个条目，该条目包含具有 Name、Type 和 Visible 属性值的 Extension 元素   。 您的条目可能如下所示：  
  
    ```  
    <Extension Name="ExtensionName" Type="CompanyName.ExtensionName.MyConnectionClass, AssemblyName" />  
    ```  
  
     Name 的值是数据处理扩展插件的唯一名称。 Type 的值是以逗号分隔的列表，包括实现 <xref:Microsoft.ReportingServices.Interfaces.IExtension> 和 <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection> 接口的类的完全限定命名空间的条目，后跟程序集的名称（不包括 .dll 文件扩展名）。 默认情况下，数据处理扩展插件是可见的。 若要从用户界面（如报表设计器）中隐藏扩展插件，请将 Visible 属性添加到 Extension 元素，并将其设置为 false。  
  
5.  最后，为自定义程序集添加一个代码组，以便为扩展插件授予 FullTrust 权限。 要完成该操作，将该代码组添加到 rspreviewpolicy.config 文件中即可，该文件在默认情况下位于 C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies 中。 代码组可能如下所示：  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="MyExtensionCodeGroup"  
       Description="Code group for my data processing extension">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url="C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies\MyExtensionAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 URL 成员身份只是您可以为数据处理扩展插件选择的众多成员条件之一。 有关 [!INCLUDE[ssRSversion2005](../../../includes/ssrsversion2005-md.md)] 中的代码访问安全性的详细信息，请参阅[安全开发 (Reporting Services)](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md)  
  
## <a name="generic-query-designer"></a>通用查询设计器  
 报表设计器提供了可用于自定义数据处理扩展插件的通用查询设计器。 该设计器包含两个窗格：查询窗格和结果窗格。 您可以使用该通用设计器编写图形界面不支持的查询。 与图形查询设计器不同的是，通用查询设计器不检查查询语法或调整查询的结构。  
  
#### <a name="to-enable-the-generic-query-designer-for-a-custom-extension"></a>为自定义扩展插件启用通用查询设计器  
  
-   将以下条目添加到 Designer 元素下的 RSReportDesigner.config 文件中，用在之前条目中提供的名称替换 Name 属性。  
  
    ```  
    <Extension Name="ExtensionName" Type="Microsoft.ReportingServices.QueryDesigners.GenericQueryDesigner,Microsoft.ReportingServices.QueryDesigners"/>  
    ```  
  
## <a name="verifying-the-deployment"></a>验证部署  
 必须先关闭本地计算机上的所有 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 实例，然后才能验证部署。 结束所有当前会话之后，可以在 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 中创建一个新报表项目，以验证数据处理扩展插件是否已成功部署到报表设计器。 为报表创建新的数据集时，您的扩展插件应当包括在可用数据源类型列表中。  
  
## <a name="see-also"></a>另请参阅  
 [部署数据处理扩展插件](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Reporting Services 扩展插件](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [实现数据处理扩展插件](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
