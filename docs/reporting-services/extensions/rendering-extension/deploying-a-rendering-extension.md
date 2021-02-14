---
title: 部署呈现扩展插件 | Microsoft Docs
description: 了解如何部署报表呈现扩展插件。 查看要添加的配置文件条目，以便报表服务器和报表设计器找到扩展插件。
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- deploying [Reporting Services], extensions
- rendering extensions [Reporting Services], deploying
ms.assetid: 9fb8c887-5cb2-476e-895a-7b0e2dd11398
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 56c22120fb08b538b845647f1a2300c22e17d017
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042208"
---
# <a name="deploying-a-rendering-extension"></a>部署呈现扩展插件
  在编写 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 报表呈现扩展插件并将其编译为 [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] 库之后，需要使其变得可供报表服务器和报表设计器发现。 为此，请将此扩展插件复制到适当的目录并向适当的 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 配置文件添加条目。  
  
## <a name="configuration-file-rendering-extension-element"></a>配置文件呈现扩展插件元素  
 将呈现扩展插件编译为 .DLL 后，您可以在 rsreportserver.config 文件中添加一个条目。 默认位置为 %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer。 父元素为 \<Render>。 在 Render 元素之下是用于每个呈现扩展插件的 Extension 元素。 **Extension** 元素包含两个属性，即 Name 和 Type。  
  
 下表描述了呈现扩展插件的 **Extension** 元素的属性：  
  
|特性|描述|  
|---------------|-----------------|  
|**Name**|扩展插件的唯一名称。 **Name** 属性的最大长度是 255 个字符。 该名称在配置文件的 **Extensions** 元素内的所有条目中必须唯一。 如果存在重复的名称，则报表服务器返回错误。|  
|类型|以逗号分隔的列表，其中包含完全限定的命名空间以及程序集的名称。|  
|**Visible**|值为 **false** 指示在用户界面中将不显示该呈现扩展插件。 如果未包含此属性，则默认值为 **true**。|  
|**LogAllExecutionRequests**|值 **false** 表示在一个会话中只对第一次报表执行记录一个条目。 如果未包含此属性，则默认值为 **true**。<br /><br /> 例如，此设置决定着是只对报表中呈现的第一页记录一个条目（设为 **false** 时），还是对报表中呈现的每一页都记录一个条目（设为 **true** 时）。|  
  
 有关详细信息，请参阅 [RsReportServer.config 配置文件](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md)。  
  
## <a name="deploying-the-extension-to-the-report-server"></a>将扩展插件部署到报表服务器  
 报表服务器使用呈现扩展插件将报表导出为其他格式。 您应将呈现扩展插件程序集作为专用程序集部署到报表服务器。 还需要在报表服务器配置文件 rsreportserver.config 中生成一个条目。  
  
### <a name="to-deploy-the-assembly"></a>部署程序集  
  
1.  将程序集从临时位置复制到您要在其上使用此呈现扩展插件的报表服务器的 bin 目录中。 报表服务器 Bin 目录的默认位置为 %ProgramFiles%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer\Bin。  
  
2.  在复制程序集文件后，打开 rsreportserver.config 文件。 rsreportserver.config 文件也位于报表服务器 bin 目录中。 还需要在配置文件中为扩展插件程序集文件生成一个条目。 可使用 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 或简单的文本编辑器打开该文件。  
  
     有关详细信息，请参阅 [RsReportServer.config 配置文件](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md)。  
  
3.  在 Rsreportserver.config 文件中找到 **Render** 元素。 应当在以下位置为新创建的扩展插件生成一个条目：  
  
    ```  
    <Extensions>  
       <Render>  
          <extension configuration>  
       </Render>  
    </Extensions>  
    ```  
  
4.  为呈现扩展插件添加一个条目。 此条目应包含一个具有 **Name** 和 **Type** 值的元素，可能如下所示：  
  
    ```  
    <Extension Name="My Rendering Extension Name" Type="CompanyName.ExtensionName.MyRenderingProvider, AssemblyName" />  
    ```  
  
     **Name** 的值必须是呈现扩展插件的唯一名称。 Type 的值是一个以逗号分隔的列表，它包含 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension> 实现的完全限定命名空间的一个条目，后跟程序集的名称（不包含 .dll 文件扩展名）。 默认情况下，呈现扩展插件是可见的。 若要从用户界面（如报表管理器）中隐藏扩展插件，请将 **Visible** 属性添加到 **Extension** 元素，并将其设置为 **false**。  
  
## <a name="verifying-the-deployment"></a>验证部署  
 还可以打开报表管理器，并验证您的扩展插件是否包括在报表的可用导出类型列表中。  
  
## <a name="see-also"></a>另请参阅  
 [实现呈现扩展插件](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [呈现扩展插件概述](../../../reporting-services/extensions/rendering-extension/rendering-extensions-overview.md)   
 [实现 IRenderingExtension 接口](../../../reporting-services/extensions/rendering-extension/implementing-the-irenderingextension-interface.md)   
 [扩展插件的安全注意事项](../../../reporting-services/extensions/security-considerations-for-extensions.md)  
  
  
