---
description: 比较共享数据源和嵌入数据源 - 报表生成器和 Reporting Services (SSRS)
title: 比较共享数据源和嵌入数据源 - 报表生成器和 Reporting Services | Microsoft Docs
ms.date: 11/18/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 5e2c72ad95392be7571c05e1dddcee1ed35c3d6a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042214"
---
# <a name="compare-shared-and-embedded-data-sources---report-builder--reporting-services-ssrs"></a>比较共享数据源和嵌入数据源 - 报表生成器和 Reporting Services (SSRS)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]
 
可以使用共享或嵌入数据源连接到数据。 共享数据源的定义独立于任何报表。 可以在报表服务器或 SharePoint 站点上的多个报表中使用它。 在报表中定义嵌入数据源。 只能在报表中使用它。 

 如果您的数据源使用频率较高，就可以采用共享数据源。 建议尽量创建和使用共享数据源。 使用共享数据源可便于对报表和报表访问进行管理，并有助于提高报表和报表所访问数据源的访问安全性。 如果需要共享数据源，需要请求系统管理员为你创建一个。  
  
 嵌入数据源（也称为报表特定数据源）是保存在报表定义中的数据连接。 只有嵌入数据源连接信息所嵌入的报表才能使用这些信息。 若要定义并管理嵌入数据源，请使用 **“数据源属性”** 对话框。  
  
 嵌入数据源和共享数据源的区别在于创建、存储和管理它们的方式不同。  
  
-   在报表设计器中，将嵌入数据源或共享数据源作为 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] 项目的一部分创建。 您可以控制是在本地使用它们以便进行预览，还是将其作为项目的一部分部署到报表服务器或 SharePoint 站点。 您可以使用已安装在您的计算机上和安装在报表服务器或 SharePoint 站点（在其中部署您的报表）上的自定义数据扩展插件。  
  
     系统管理员可以安装和配置其他数据处理扩展插件和 .NET Framework 数据访问接口。 有关详细信息，请参阅[数据处理扩展插件和 .NET Framework 数据提供程序 (SSRS)](../../reporting-services/report-data/data-processing-extensions-and-net-framework-data-providers-ssrs.md)。  
  
     开发人员可以使用 <xref:Microsoft.ReportingServices.DataProcessing> API 创建数据处理扩展插件以支持其他类型的数据源。  
  
-   在 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]中，浏览到某一报表服务器或 SharePoint 站点并选择共享数据源，或者在报表中创建嵌入数据源。 不能在 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 中创建共享数据源。 不能在 [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] 中使用自定义数据扩展插件。  

## <a name="summary-of-differences"></a>差异摘要
  
 下表总结了嵌入数据源和共享数据源之间的差异。  
  
|说明|嵌入<br /><br /> 数据源|共享<br /><br /> 数据源|  
|-----------------|------------------------------|----------------------------|  
|数据连接嵌入在报表定义中。|![可用](../../reporting-services/report-data/media/greencheck.gif "可用")||  
|指向报表服务器上的数据连接的指针嵌入在报表定义中。||![可用](../../reporting-services/report-data/media/greencheck.gif "可用")|  
|在报表服务器上管理|![可用](../../reporting-services/report-data/media/greencheck.gif "可用")|![可用](../../reporting-services/report-data/media/greencheck.gif "可用")|  
|对于共享数据集，要求这么做||![可用](../../reporting-services/report-data/media/greencheck.gif "可用")|  
|对于组件，要求这么做||![可用](../../reporting-services/report-data/media/greencheck.gif "可用")|  

## <a name="next-steps"></a>后续步骤

[创建和管理共享数据源](../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md)   
[创建和修改嵌入的数据源](../../reporting-services/report-data/create-and-modify-embedded-data-sources.md)   
[设置部署属性](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
[为报表数据源指定凭据和连接信息](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
