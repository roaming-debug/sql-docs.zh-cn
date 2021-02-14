---
title: 实现 IRenderingExtension 接口 | Microsoft Docs
description: 了解如何实现 IRenderingExtension 接口，该接口将报表数据转换为查看器、打印机和其他目标设备使用的输出格式。
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- IRenderingExtension interface
- rendering extensions [Reporting Services], IRenderingExtension interface
ms.assetid: 74b2f2b7-6796-42da-ab7d-b05891ad4001
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 984775286d4f32d0c1e844bc6ba0ca62bcf198ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042177"
---
# <a name="implementing-the-irenderingextension-interface"></a>实现 IRenderingExtension 接口
  呈现扩展插件从与实际数据相结合的报表定义中获取结果，并将得到的数据呈现为某种可用的格式。 组合的数据和格式的转换是通过一个实现 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension> 的公共语言运行时 (CLR) 类完成的。 该类将对象模型转换为可由查看器、打印机或其他输出目标使用的输出格式。  
  
 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension> 具有三个必须进行编码的方法：  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A> - 呈现报表。  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.RenderStream%2A> - 呈现报表中特定的流。  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> - 获取报表所需的其他信息，如图标。  
  
 下列各节更为详细地讨论这些方法。  
  
## <a name="render-method"></a>Render 方法  
 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A> 方法包含表示以下对象的参数：  
  
-   要呈现的 report 本身。 此对象包含报表的属性、数据和布局信息。 report 是报表对象模型树的根。  
  
-   ServerParameters，包含字符串字典对象以及用于报表服务器的参数（如果有）。  
  
-   deviceInfo 参数，其中包含设备设置。 有关详细信息，请参阅[将设备信息设置传递给呈现扩展插件](../../../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md)。  
  
-   clientCapabilities 参数，其中包含 <xref:System.Collections.Specialized.NameValueCollection> 字典对象，此字典对象具有与要向其呈现的客户端的信息。  
  
-   RenderProperties，其中包含有关呈现结果的信息。  
  
-   createAndRegisterStream 是一个委托函数，可以调用它来获取要呈现到的流。  
  
### <a name="deviceinfo-parameter"></a>deviceInfo 参数  
 deviceInfo 参数包含呈现参数，而不包含报表参数。 这些呈现参数将传递给呈现扩展插件。 报表服务器会将 deviceInfo 值转换为一个 <xref:System.Collections.Specialized.NameValueCollection> 对象。 deviceInfo 参数中的项被视为区分大小写的值。 如果作为 URL 访问的结果提出呈现请求，则格式为 `rc:key=value` 的 URL 参数将转换为 deviceInfo 字段对象中的键/值对。 浏览器检测代码还在 clientCapabilities 字典中提供了以下各项：EcmaScriptVersion、JavaScript、MajorVersion、MinorVersion、Win32、Type 和 AcceptLanguage。 将忽略 deviceInfo 中呈现扩展插件不理解的任何名称/值对。 以下代码示例显示用于检索图标的示例 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> 方法：  
  
```csharp  
public void GetRenderingResource (CreateStream createStreamCallback, NameValueCollection deviceInfo)  
{  
    string[] iconTagValues = deviceInfo.GetValues("Icon");  
    if ((iconTagValues != null) && (iconTagValues.Length > 0) )  
    {  
        // Create a stream to output to.  
        Stream outputStream = createStreamCallback(m_iconResourceName, "gif", null, "image/gif", false);  
        // Get the GIF image for one of the buttons on the toolbar  
        Image requiredImage = (Image) m_resourcemanager.GetObject(m_iconResourceName  
        // Write the image to the output stream  
        requiredImage.Save(outputStream, requiredImage.RawFormat);  
    }  
    return;  
}  
```  
  
## <a name="renderstream-method"></a>RenderStream 方法  
 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.RenderStream%2A> 方法呈现报表中的特定流。 所有流都是在初始 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A> 调用过程中创建的，但最初不向客户端返回这些流。 此方法用于辅助流（如 HTML 呈现中的图像）或多页呈现扩展插件的其他页（如图像/EMF）。  
  
## <a name="getrenderingresource-method"></a>GetRenderingResource 方法  
 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> 方法检索信息而不执行报表的完整呈现。 有时，报表所需的信息并不要求呈现此报表本身。 例如，如果需要与呈现扩展插件关联的图标，则可以使用包含单个标记 \<Icon> 的 deviceInfo 参数。 在此类情况下，可以使用 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> 方法。  
  
## <a name="see-also"></a>另请参阅  
 [实现呈现扩展插件](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [呈现扩展插件概述](../../../reporting-services/extensions/rendering-extension/rendering-extensions-overview.md)  
  
  
