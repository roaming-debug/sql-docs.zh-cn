---
title: 报表定义语言 |Microsoft Docs
description: 了解关于报表定义语言 (RDL) 的详细信息。 你将了解到 RDL 是 SQL Server Reporting Services 报表定义的 XML 表示形式。
ms.date: 01/24/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Reporting Services, RDL
- Reporting Services, RDL
- RDL [Reporting Services], about Report Definition Language
- SSRS, RDL
- Report Definition Language, about Report Definition Language
- Report Definition Language
- RDL [Reporting Services]
- reports [Reporting Services], definitions
ms.assetid: b18b025e-f4bd-4744-8f86-0ac9fb967548
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3fd91a74fe64d055860c2382efeb16523685c2a3
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596697"
---
# <a name="report-definition-language-ssrs"></a>报表定义语言 (SSRS)
  报表定义语言 (RDL) 是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表定义的 XML 表示形式。 报表定义包含报表的数据检索和布局信息。 RDL 由 XML 元素组成，这些元素符合为 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]创建的 XML 语法。 通过访问报表定义文件中的代码程序集，可以添加您自己的自定义函数，以便控制报表项值、样式和格式。  
  
 RDL 通过定义支持报表定义互换的公共架构，提升了商业报表产品的互操作性。 使用 XML 的任何协议或编程接口都可以使用 RDL。 RDL 是：  
  
-   报表定义的 XML 架构。  
  
-   企业和第三方的交换格式。  
  
-   支持其他命名空间和自定义元素的可扩展开放式架构。  
  
##  <a name="rdl-specifications"></a><a name="bkmk_RDL_Specifications"></a> RDL 规范  
 若要下载特定架构版本的规范，请参阅 [报表定义语言规范](/openspecs/sql_server_protocols/ms-rdl/53287204-7cd0-4bc9-a5cd-d42a5925dca1)。  
  
##  <a name="rdl-xml-schema-definition"></a><a name="bkmk_RDL_XML_Schema_Definition"></a> RDL XML 架构定义  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 报表定义语言 (RDL) 文件使用 XML 架构定义 (XSD) 文件进行验证。 架构定义 RDL 元素可在 .rdl 文件中什么位置出现的规则。 元素包括其数据类型和基数，即允许的出现次数。 元素可以是简单的，也可以是复杂的。 简单元素没有子元素或属性。 复杂元素具有子元素以及可选具有属性。  
  
 例如，此架构包含 RDL 元素 **ReportParameters**，它为复杂类型 **ReportParametersType**。 根据约定，元素的复杂类型是元素名称后跟单词 **Type**。 **ReportParameters** 元素可包含在 **报表** 元素（复杂类型）中，并可包含 **ReportParameter** 元素。 ReportParameterType 是一种简单类型，只能为下列某个值  ：Boolean、DateTime、Integer、Float 或 String      。 有关 XML 架构数据类型的详细信息，请参阅 [XML Schema Part 2:Datatypes Second Edition](https://go.microsoft.com/fwlink/?linkid=4871)（XML 架构第 2 部分：数据类型第二版）。  
  
 可在 ReportDefinition.xsd 文件中找到 RDL XSD，该文件位于产品 CD-ROM 的 Extras 文件夹中。 此外，还可通过以下 URL 在报表服务器上获取：`https://servername/reportserver/reportdefinition.xsd`。  
  
##  <a name="creating-rdl"></a><a name="bkmk_Creating_RDL"></a> 创建 RDL  
 由于 RDL 的开放式和可扩展特性，可以创建基于其 XML 架构生成 RDL 的各种工具和应用程序。  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 提供用于生成 RDL 文件的多种工具。 有关详细信息，请参阅 [Reporting Services 工具](../../reporting-services/tools/reporting-services-tools.md)。  
  
 从应用程序生成 RDL 的一种最简便方式是使用 <xref:System.Xml> 命名空间和 <xref:System.Linq> 命名空间的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 类。 尤其是可以使用 **XmlTextWriter** 类编写 RDL。 使用 **XmlTextWriter**，可以在任何 [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 应用程序中从头到尾生成完整的报表定义。 开发人员还可以通过添加具有自定义属性的自定义报表项来扩展 RDL。 有关 XmlTextWriter  类和 <xref:System.Xml> 命名空间的详细信息，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 开发人员指南。 有关语言集成查询 (LINQ) 的详细信息，请在 MSDN 上搜索 "LINQ to XML"。  
  
 报表定义文件的标准文件扩展名为 .rdl。 还可以开发具有扩展名 .rdlc 的客户端报表定义文件。 两种扩展名的 MIME 类型都为 text/xml。 有关报表的详细信息，请参阅 [Reporting Services 报表 &#40;SSRS&#41;](../../reporting-services/reports/reporting-services-reports-ssrs.md)。  
  
##  <a name="rdl-types"></a><a name="bkmk_RDL_Types"></a> RDL 类型  
 下表列出了在 RDL 元素和属性中使用的类型。  
  
|类型|说明|  
|----------|-----------------|  
|**二进制**|具有 Base-64 编码二进制值的属性。|  
|**布尔值**|以 **true** 或 **false** 作为对象值的属性。 除非另行指定，否则未指定的可选布尔对象的值为 **False**。|  
|**Date**|具有以 ISO8601 日期格式 YYYY-MM-DD[THH:MM[:SS[.S]]] 指定的完全指定日期或日期时间值的属性。|  
|**Enum**|具有字符串文本值的属性，该文本值必须是指定值列表中的某个值。|  
|**Float**|具有浮点值的属性。 使用句号 (.) 作为可选的小数分隔符。|  
|**整数**|具有整数 (int32) 值的属性。|  
|**语言**|具有包含语言和区域性代码（例如 "en-us" 表示“美国英语”）的文本值的属性。 该值必须是特定语言，或者是在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] 中为其定义了默认语言的非特定语言。|  
|**名称**|具有字符串文本值的属性。 名称在该项的命名空间中必须唯一。 如果未指定，则项的命名空间为具有名称的最内层包含对象。|  
|**NormalizedString**|具有已规范化的字符串文本值的属性。|  
|**大小**|大小元素必须包含数字（以句点字符作为可选的小数分隔符）。 数字后面必须是 CSS 长度单位（例如 cm、mm、in、pt 或 pc）的指示符。 数字和指示符之间的空格是可选的。 有关大小指示符的详细信息，请参阅 [CSS 值和单位参考](/previous-versions//ms537660(v=vs.85))。<br /><br /> 在 RDL 中， **Size** 的最大值为 160 in。 最小大小为 0 in。|  
|**字符串**|具有字符串文本值的属性。|  
|**UnsignedInt**|具有无符号整数 (uint32) 值的属性。|  
|**Variant**|具有任何简单 XML 类型的属性。|  
  
##  <a name="rdl-data-types"></a><a name="bkmk_RDL_Data_Types"></a> RDL 数据类型  
 DataType 枚举定义 RDL 中的属性、表达式或参数的数据类型。 下表显示公共语言运行时 (CLR) 数据类型是如何与 RDL 数据类型相对应的。  
  
|**CLR 类型**|**相应的数据类型**|  
|-----------------------|---------------------------------|  
|Boolean|Boolean|  
|DateTime、DateTimeOffset|DateTime|  
|Int16、Int32、UInt16、Byte、SByte|Integer|  
|Single、Double|Float|  
|String、Char、GUID、Timespan|String|  
  
## <a name="see-also"></a>另请参阅  
 [查找报表定义架构版本 (SSRS)](../../reporting-services/reports/find-the-report-definition-schema-version-ssrs.md)   
 [将自定义程序集用于报表](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [自定义报表项](../../reporting-services/custom-report-items/custom-report-items.md)  
  
