---
title: SAP NetWeaver BI 连接类型 | Microsoft Docs
description: 利用本文中有关 SAP NetWeaver BI 连接类型的信息，了解如何生成数据源。
ms.date: 03/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
ms.assetid: f985856b-31d5-4e56-844b-8a8ee38da67e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4b376fddd69a44f9280d4c0aaec4f58880d9ff76
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596489"
---
# <a name="sap-netweaver-bi-connection-type-ssrs"></a>SAP NetWeaver BI 连接类型 (SSRS)
  若要在报表中包含来自 SAP NetWeaver® Business Intelligence 外部数据源的数据，您必须拥有一个基于 [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)]类型的报表数据源的数据集。 此内置数据源类型基于 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 的 [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)].NET Framework Data Provider 1.0 的数据扩展插件。  
  
 利用此数据扩展插件，可以从 [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] 外部数据源中定义的 InfoCubes、MultiProviders（虚拟 InfoCubes）和基于 Web 的查询中检索多维数据。  
  
 使用本主题中的信息来生成一个数据源。 有关分步说明，请参阅 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)。  
  
##  <a name="connection-string"></a><a name="Connection"></a> 连接字符串  
 请联系数据库管理员，获取连接信息以及用于连接到数据源的凭据。 下面的连接字符串示例指定使用端口 8000 的服务器上的 [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] 数据源，以及使用 SOAP 的 Internet 上的 XML for Analysis Services (XMLA)：  
  
```  
DataSource=https://mySAPNetWeaverBIServer:8000/sap/bw/xml/soap/xmla  
```  
  
 有关更多连接字符串示例，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)。  
  
  
##  <a name="credentials"></a><a name="Credentials"></a> 凭据  
 执行以下操作时需要提供凭据：运行查询、本地预览报表以及从报表服务器预览报表。  
  
 报表发布后，您可能需要更改数据源的凭据，以使报表在报表服务器上运行时，用于检索数据的权限有效。  
  
 有关详细信息，请参阅[为报表数据源指定凭据和连接信息](specify-credential-and-connection-information-for-report-data-sources.md)  
  
  
##  <a name="queries"></a><a name="Query"></a> 查询  
 可以在设计模式或查询模式下使用图形查询设计器，通过浏览数据源中的基础数据结构来生成多维表达式 (MDX) 查询。 在设计时，您可以采用交互方式从查询设计器中运行查询，以查看结果。 生成的查询定义了数据集中的字段。 在运行时，将从数据源返回实际数据。 使用图形查询设计器执行以下操作：  
  
-   在设计模式下，将维度、成员、成员属性和关键数字从数据源拖至“数据”窗格，以生成多维表达式 (MDX) 查询。 将计算成员从“计算成员”窗格拖至“数据”窗格，以定义附加数据集字段。  
  
-   在查询模式下，将维度、成员、成员属性和关键数字拖至“查询”窗格或在“查询”窗格中直接键入 MDX 文本。 将计算成员从“计算成员”窗格拖至“数据”窗格，以定义附加数据集字段。  
  
 生成查询时，查询设计器会自动向 MDX 查询添加默认属性。 若要包含默认属性之外的属性，必须手动修改 MDX 查询。  
  
 有关使用此查询设计器的详细信息，请参阅 [SAP NetWeaver BI 查询设计器用户界面（报表生成器）](/previous-versions/sql/)。  
  
  
##  <a name="extended-field-properties"></a><a name="Extended"></a> 扩展字段属性  
 [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] 数据源支持扩展字段属性。 扩展字段属性是除了通过数据处理扩展插件为数据集字段定义的 **Value** 和 **IsMissing** 之外的其他属性。 扩展属性包括预定义属性和自定义属性。 预定义属性是对多个数据源通用的属性。 自定义属性对于每个数据源都是唯一的。  
  
### <a name="working-with-field-properties"></a>使用字段属性  
 扩展字段属性不作为可拖至报表布局的项出现在“报表数据”窗格中。 不过，您可以将该属性的父字段拖至报表中，然后将默认属性由 **Value** 更改为要使用的属性。 例如，如果在 MDX 查询设计器中通过将一个级别从“元数据”窗格放至“查询”窗格创建了字段名“日历年/月级别 01”，则需要在表达式中使用以下语法来引用自定义扩展属性“长名称”：  
  
 `=Fields!Calendar_Year_Month_Level_01("Long Name")`  
  
 当您将光标悬停在“元数据”窗格中的某个字段上时，扩展字段属性的名称便会显示在工具提示中。 有关可用于浏览基础数据的查询设计器的详细信息，请参阅 [SAP NetWeaver BI Query Designer User Interface](../../reporting-services/report-data/sap-netweaver-bi-query-designer-user-interface.md)。  
  
> [!NOTE]  
>  仅当数据源在报表运行和为其数据集检索数据的情况下提供扩展字段属性的值时，这些值才存在。 然后，您可以使用下面所述的语法引用任意表达式中的这些 **Field** 属性值。 但是，由于这些字段特定于此数据访问接口，而不是报表定义语言的一部分，因此，对这些值所做的更改不会随报表定义一同保存。  
  
 使用以下两种语法中的一种可以在表达式中引用预定义扩展属性：  
  
-   *Fields!FieldName.PropertyName*  
  
-   *Fields!FieldName("PropertyName")*  
  
 可以使用以下语法在表达式中引用自定义扩展属性：  
  
-   *Fields!FieldName("PropertyName")*  
  
  
### <a name="predefined-field-properties"></a>预定义的字段属性  
 下表提供了可以用于 [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] 数据源的预定义字段属性的列表。  
  
|**Property**|**Type**|**说明或所需的值**|  
|------------------|--------------|---------------------------------------|  
|**值**|**Object**|指定字段的数据值。|  
|**IsMissing**|**布尔值**|指示是否在结果数据集中找到了该字段。|  
|**FormattedValue**|**字符串**|返回关键数字的格式值。|  
|**BackgroundColor**|**字符串**|返回数据库中为该字段定义的背景颜色。|  
|**彩色**|**字符串**|返回数据库中为该项定义的前景色。|  
|**Key**|**Object**|返回级别的键。|  
|**LevelNumber**|**整数**|针对父子层次结构返回级别号或维度编号。|  
|**ParentUniqueName**|**字符串**|针对父子层次结构返回父级的完全限定名称。|  
|**UniqueName**|**字符串**|返回级别的完全限定名称。 例如，某员工的 **UniqueName** 值可能为 *[0D_Company].[10D_Department].[11]* 。|  
  
 有关在表达式中使用字段和字段属性的详细信息，请参阅[表达式中的内置集合（报表生成器和 SSRS）](../../reporting-services/report-design/built-in-collections-in-expressions-report-builder.md)。  
  
  
##  <a name="remarks"></a><a name="Remarks"></a> 注释  
 不是所有的报表传递模式都受到此数据访问接口的支持。 此数据处理扩展插件不支持通过数据驱动订阅传递报表。 有关详细信息，请参阅[使用外部数据源提供订阅方数据（数据驱动订阅）](../../reporting-services/subscriptions/use-an-external-data-source-for-subscriber-data-data-driven-subscription.md)。  
  
 有关详细信息，请参阅 [Using SQL Server 2008 Reporting Services with SAP NetWeaver Business Intelligence](/previous-versions/sql/sql-server-2008/cc974473(v=sql.100))（使用具有 SAP NetWeaver Business Intelligence 的 SQL Server 2008 Reporting Services）。  
  
  
##  <a name="how-to-topics"></a><a name="HowTo"></a> 操作指南主题  
 本节包含使用数据连接、数据源和数据集的分步说明。  
  
 [添加和验证数据连接（报表生成器和 SSRS）](../../reporting-services/report-data/add-and-verify-a-data-connection-report-builder-and-ssrs.md)  
  
 [创建共享数据集或嵌入数据集（报表生成器和 SSRS）](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)  
  
 [向数据集添加筛选器（报表生成器和 SSRS）](../../reporting-services/report-data/add-a-filter-to-a-dataset-report-builder-and-ssrs.md)  
  
  
##  <a name="related-sections"></a><a name="Related"></a> 相关章节  
 文档中的这些章节提供有关报表数据的深入概念性信息，以及有关如何定义、自定义和使用与数据相关的报表部件的步骤信息。  
  
 [报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)  
 提供访问报表数据的概述。  
  
 [创建数据连接字符串 - 报表生成器和 SSRS](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
 提供有关数据连接和数据源的信息。  
  
 [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
 提供有关嵌入数据集和共享数据集的信息。  
  
 [数据集字段集合（报表生成器和 SSRS）](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)  
 提供有关查询生成的数据集字段集合的信息。  
  
 [Reporting Services 支持的数据源 (SSRS)](../../reporting-services/report-data/data-sources-supported-by-reporting-services-ssrs.md)  
 提供有关每个数据扩展插件的平台和版本支持的详细信息。  
  
  
## <a name="see-also"></a>另请参阅  
 [报表参数（报表生成器和报表设计器）](../../reporting-services/report-design/report-parameters-report-builder-and-report-designer.md)   
 [对数据进行筛选、分组和排序（报表生成器和 SSRS）](../../reporting-services/report-design/filter-group-and-sort-data-report-builder-and-ssrs.md)   
 [表达式（报表生成器和 SSRS）](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)  
  
