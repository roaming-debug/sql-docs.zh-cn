---
title: 呈现扩展插件概述 | Microsoft Docs
description: 查看 Reporting Services 中包含的数据呈现扩展插件。 了解如何添加自定义呈现扩展插件，以生成其他格式的报表。
ms.date: 12/7/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- formats [Reporting Services], rendering extensions
- rendering extensions [Reporting Services], about extensions
ms.assetid: 909356a0-4709-43e5-b597-33bd9bb22882
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b3d3dc5fac6915faafb48b045b70c5b57f6e6ad8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071114"
---
# <a name="rendering-extensions-overview"></a>呈现扩展插件概述
  呈现扩展插件是将报表数据和布局信息转换为设备特定格式的报表服务器的组件或模块。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 包括七种呈现扩展插件：HTML、Excel、Word、CSV 或 Text、XML、Image 和 PDF。 您可以创建其他呈现扩展插件以便以其他格式生成报表。  
  
> [!NOTE]  
>  若要确定哪些呈现扩展插件可用，您可以在 RSReportServer.config 文件中查看已安装的扩展插件列表。  
  
 下表介绍随 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 提供的呈现扩展插件。  
  
|扩展名称|说明|  
|--------------------|-----------------|  
|**XML**|以 XML 格式呈现报表。 报表在浏览器中打开。 应用于此 XML 输出的其他转换可能是一种经济有效的方法，使您免于开发自己的呈现扩展插件。|  
|**CSV**|以逗号分隔的格式呈现报表。 报表在与 CSV 文件格式关联的查看工具中打开。|  
|**IMAGE**|以面向页面的格式呈现报表。 此格式在报表工具栏的“导出”下拉列表中显示为 **TIFF**。|  
|**PDF**|在 Adobe Acrobat Reader 中呈现报表。 此格式在报表工具栏的“导出”下拉列表中显示为“Acrobat (PDF) 文件”。|  
|**EXCEL**|在 [!INCLUDE[ofprexcel](../../../includes/ofprexcel-md.md)] 中呈现报表。|  
|**WORD**|在 [!INCLUDE[ofprword](../../../includes/ofprword-md.md)] 中呈现报表。|  
|HTML 4.0（HTML 呈现扩展插件的一部分）|HTML 是用于最初呈现报表的格式。 如果浏览器支持 HTML 4.0，则这是所使用的格式。 否则，使用 HTML 3.2。|  
|MHTML（HTML 呈现扩展插件的一部分）|以 MHTML 格式呈现报表。 报表在 Internet Explorer 中打开。 此格式在报表工具栏的“导出”下拉列表中显示为“Web 存档”。|  
|**NULL**|不将报表呈现为特定的格式。 这一呈现扩展插件用于将报表放到缓存中。 空呈现应与计划的执行或传递一起使用。|  
  
 有关建议的格式及其用法的详细信息，请参阅[导出报表（报表生成器和 SSRS）](../../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)。  
  
 由 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] 实现或随 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 提供的每个呈现扩展插件都使用一组通用的接口。 这可确保每个扩展插件实现类似的功能并降低报表服务器核心中的呈现代码的复杂性。  
  
## <a name="rendering-object-model"></a>呈现对象模型  
 当处理报表时，结果是公开的对象模型，称为呈现对象模型 (ROM)。 呈现对象模型是定义已处理报表的内容、布局和数据的各个类的集合。 ROM 可供希望为 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 设计、开发和部署自定义呈现扩展插件的开发人员使用。 当报表服务器处理报表的 XML 定义以及用户定义的报表数据时，将生成 ROM。 当完成处理后，呈现扩展插件使用公共对象模型以定义报表的输出。 ROM 的可用公共类在 Microsoft.ReportingServices.OnDemandReportRendering 命名空间中定义。  
  
## <a name="writing-custom-rendering-extensions"></a>编写自定义呈现扩展插件  
 在决定创建自定义呈现扩展插件之前，应评估更为简单的替代方法。 可以：  
  
-   通过为现有扩展插件指定设备信息设置，自定义呈现的输出。  
  
-   通过将 XSL 转换 (XSLT) 与 XML 呈现格式的输出结合起来，添加自定义格式和演示功能。  
  
 编写自定义呈现扩展插件的过程困难重重。 呈现扩展插件通常必须支持报表元素的所有可能组合，并要求您实现许许多多的类、接口、方法和属性。 如果必须以未随 [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 提供的格式呈现报表，并且决定为呈现扩展插件编写自己的托管代码实现，则呈现扩展插件代码必须实现报表服务器要求的 Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension 接口。  
  
## <a name="see-also"></a>另请参阅  
 [实现呈现扩展插件](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Reporting Services 扩展插件库](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
