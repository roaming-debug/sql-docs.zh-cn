---
title: 创建自定义报表项运行时组件 | Microsoft Docs
description: 了解如何创建自定义报表项运行时组件，并在设计环境中定义该组件的属性。
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, creating
ms.assetid: b3e15a4a-98f8-4dbb-b847-bbcb20327051
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c6d70bba220624cd43ca1ced80cbe89ba936460
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061942"
---
# <a name="creating-a-custom-report-item-run-time-component"></a>创建自定义报表项运行时组件
  自定义报表项运行时组件作为使用任何符合 CLS 的语言的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 组件实现，该组件由报表处理器在运行时调用。 可在设计环境下定义此类运行时组件的属性，方法为修改相应自定义报表项的对应设计时组件。  
  
 有关完全实现的自定义报表项的示例，请参阅 [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889)（SQL Server Reporting Services 产品示例）。  
  
## <a name="definition-and-instance-objects"></a>定义和实例对象  
 实现自定义报表项之前，必须了解“定义对象”和“实例对象”之间的差别   。 定义对象用于提供自定义报表项的 RDL 表示形式，而实例对象是定义对象的已计算版本。 对于报表上的每一项，都只有一个定义对象。 访问包含表达式的定义对象上的属性时，将获取未经计算的表达式字符串。 实例对象包含定义对象的已计算版本，与项的定义对象可以是一对多的关系。 例如，如果报表有一个 <xref:Microsoft.ReportingServices.OnDemandReportRendering.Tablix> 数据区域，其详细信息行包含 <xref:Microsoft.ReportingServices.OnDemandReportRendering.CustomReportItem>，则在此数据区域中，将只有一个定义对象，但是每一行中都有一个实例对象。  
  
## <a name="implementing-the-icustomreportitem-interface"></a>实现 ICustomReportItem 接口  
 若要创建 CustomReportItem 运行时组件，则需要实现 Microsoft.ReportingServices.ProcessingCore.dll 中定义的 <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem> 接口  ：  
  
```csharp  
namespace Microsoft.ReportingServices.OnDemandReportRendering  
{  
    public interface ICustomReportItem  
    {  
        void GenerateReportItemDefinition(CustomReportItem customReportItem);  
void EvaluateReportItemInstance(CustomReportItem customReportItem);  
    }  
}  
```  
  
 实现 <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem> 接口后，将为您生成两个方法存根：<xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.GenerateReportItemDefinition%2A> 和 <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.EvaluateReportItemInstance%2A>。 先调用的是 <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.GenerateReportItemDefinition%2A> 方法，该方法用于设置定义属性和创建 <xref:Microsoft.ReportingServices.OnDemandReportRendering.Image> 对象，后者将包含用于呈现相应项的定义属性和实例属性。 <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.EvaluateReportItemInstance%2A> 方法在定义对象被计算后调用，该方法可提供将用于呈现相应项的实例对象。  
  
 以下示例是用于将相应控件名称呈现为图像的自定义报表项的实现示例。  
  
```csharp  
namespace Microsoft.Samples.ReportingServices  
{  
    using System;  
    using System.Collections.Generic;  
    using System.Collections.Specialized;  
    using System.Drawing.Imaging;  
    using System.IO;  
    using System.Text;  
    using Microsoft.ReportingServices.OnDemandReportRendering;  
  
    public class PolygonsCustomReportItem : ICustomReportItem  
    {  
        #region ICustomReportItem Members  
  
        public void GenerateReportItemDefinition(CustomReportItem cri)  
        {  
            // Create the Image object that will be   
            // used to render the custom report item  
            cri.CreateCriImageDefinition();  
            Image polygonImage = (Image)cri.GeneratedReportItem;  
        }  
  
        public void EvaluateReportItemInstance(CustomReportItem cri)  
        {  
            // Get the Image definition  
            Image polygonImage = (Image)cri.GeneratedReportItem;  
  
            // Create the image for the custom report item  
            polygonImage.ImageInstance.ImageData = DrawImage(cri);  
        }  
  
        #endregion  
  
        /// <summary>  
        /// Creates an image of the CustomReportItem's name  
        /// </summary>  
        private byte[] DrawImage(CustomReportItem customReportItem)  
        {  
            int width = 1;          // pixels  
            int height = 1;         // pixels  
            int resolution = 75;    // dpi  
  
            System.Drawing.Bitmap bitmap = new System.Drawing.Bitmap(width, height);  
            bitmap.SetResolution(resolution, resolution);  
  
            System.Drawing.Graphics graphics = System.Drawing.Graphics.FromImage(bitmap);  
            graphics.PageUnit = System.Drawing.GraphicsUnit.Pixel;  
  
            // Get the Font for the Text  
            System.Drawing.Font font = new System.Drawing.Font(System.Drawing.FontFamily.GenericMonospace,  
                12, System.Drawing.FontStyle.Regular);  
  
            // Get the Brush for drawing the Text  
            System.Drawing.Brush brush = new System.Drawing.SolidBrush(System.Drawing.Color.LightGreen);  
  
            // Get the measurements for the image  
            System.Drawing.SizeF maxStringSize = graphics.MeasureString(customReportItem.Name, font);  
            width = (int)(maxStringSize.Width + 2 * font.GetHeight(resolution));  
            height = (int)(maxStringSize.Height + 2 * font.GetHeight(resolution));  
  
            bitmap.Dispose();  
            bitmap = new System.Drawing.Bitmap(width, height);  
            bitmap.SetResolution(resolution, resolution);  
  
            graphics.Dispose();  
            graphics = System.Drawing.Graphics.FromImage(bitmap);  
            graphics.PageUnit = System.Drawing.GraphicsUnit.Pixel;  
  
            // Draw the text  
            graphics.DrawString(customReportItem.Name, font, brush, font.GetHeight(resolution),   
                font.GetHeight(resolution));  
  
            // Create the byte array of the image data  
            MemoryStream memoryStream = new MemoryStream();  
            bitmap.Save(memoryStream, ImageFormat.Bmp);  
            memoryStream.Position = 0;  
            byte[] imageData = new byte[memoryStream.Length];  
            memoryStream.Read(imageData, 0, imageData.Length);  
  
            return imageData;  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [自定义报表项体系结构](../../reporting-services/custom-report-items/custom-report-item-architecture.md)   
 [创建自定义报表项设计时组件](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)   
 [自定义报表项类库](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)   
 [如何：部署自定义报表项](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
  
