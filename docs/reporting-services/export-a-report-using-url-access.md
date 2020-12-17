---
title: 使用 URL 访问导出报表 | Microsoft Docs
description: 了解如何通过使用 rs:Format URL 参数指定报表呈现格式，来达到通过 URL 访问来导出报表的目的。
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- formats [Reporting Services], URL rendering
- URL access [Reporting Services], rendering formats
ms.assetid: 6a3b7fc3-3d91-4d12-8371-42ea12e74517
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d61f509fa07528b71cd7cbb23488bc14f33959ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472488"
---
# <a name="export-a-report-using-url-access"></a>使用 URL 访问导出报表
  可以选择使用 *rs:Format* URL 参数指定呈现报表的格式。  HTML4.0 和 HTM5 格式（呈现扩展插件）将呈现在浏览器中，对于其他格式，浏览器将会提示将报告输出保存到本地文件。  
  
 例如，若要直接从本机模式报表服务器获取报表的 PDF 副本：  
  
```  
https://myrshost/ReportServer?/myreport&rs:Format=PDF  
```  

::: moniker range="=sql-server-2016"
  
 此外，若要从 SharePoint 集成模式的报表服务器获取：  
  
```  
https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/myrereport.rdl&rs:Format=PDF  
```  
 
::: moniker-end
 
 例如，浏览器中的以下 URL 命令从报表服务器的命名实例导出 PPTX 报表：  
  
```  
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 对于此参数，有效值为何随要访问的报表服务器上所安装的报表呈现扩展插件而异。 常见的扩展插件有 HTML4.0、MHTML、IMAGE、EXCELOPENXML (xlsx)、WORDOPENXML (docx)、CSV、PDF、XML 和 NULL。 如果指定的呈现扩展插件未安装在相应的报表服务器上，则相应报表将不能呈现，并将生成错误，同时通过浏览器来显示该错误。  
  
 如果未在 URL 中纳入 *Format* 参数，则报表服务器将检测浏览器，并将相应报表呈现为合适的 HTML 格式。  
  
## <a name="see-also"></a>另请参阅  
 [URL 访问 (SSRS)](../reporting-services/url-access-ssrs.md)   
 [URL 访问参数引用](../reporting-services/url-access-parameter-reference.md)  
  
  
