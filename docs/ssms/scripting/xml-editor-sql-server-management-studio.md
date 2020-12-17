---
title: XML 编辑器 (SQL Server Management Studio)
description: 了解如何使用 SQL Server Management Studio XML 编辑器的可视化工具来处理 XML 架构 (XSD)、ADO.NET 数据集和 XML 文档。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.editor.xml.f1
- sql13.swb.editorxml.f1
- vs.xmleditor
- sql13.swb.xmleditor.f1
helpviewer_keywords:
- XML Designer [SQL Server Management Studio]
ms.assetid: 0824a5ce-e67b-4b53-98d9-d371faf2d23c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7215b2bc235b846e67484e19a77ba75cea2f9881
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480558"
---
# <a name="xml-editor-sql-server-management-studio"></a>XML 编辑器 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

提供用于处理 XML 架构、ADO.NET 数据集和 XML 文档的一组可视工具。 XML 设计器支持由万维网联合会 (W3C) 定义的 XML 架构定义 (XSD) 语言。 该设计器不支持 DTD（文档类型定义）或其他 XML 架构语言，例如 XDR（XML 数据简化）。  

若要显示该设计器，请向您的项目中添加数据集、XML 架构或 XML 文件，或打开下表中列出的任何类型的文件。  

> [!CAUTION]
> 在“架构”视图中没有 **“撤消”** 命令。 请仔细规划您的任务并经常保存文件。  
  
 该设计器提供了以下三种视图（或模式）来处理 XML 文件、XML 架构和数据集：  
  
|视图|描述|支持的文件类型|  
|----------|-----------------|--------------------------|  
|**架构**|用于直观地创建和修改 XML 架构以及 ADO.NET 数据集。|.xsd|  
|**数据**|用于在结构化数据网格中直观地修改 XML 数据文件。|.xml|  
|**XML**|用于编辑 XML；源编辑器提供有颜色编码和 IntelliSense 功能，包括“完成单词”和“列出成员”。|.xml .xsd .xslt .wsdl.web.resx.tdl.wsf.hta.disco.vsdisco.config|  
|**显示计划**|显示使用 SET SHOWPLAN_XML ON 选项创建的 xml 查询计划。|.showplan|  
  
## <a name="schema-view"></a>“架构”视图  
 “架构”视图可以直观地显示构成 XML 架构和 ADO.NET 数据集的元素、属性、类型等。  
  
 在“架构”视图中，您可以通过从“工具箱”的“XML 架构”选项卡中，或从服务器资源管理器中，将元素拖动到设计图面，来构造架构和数据集。 另外，您可以通过右键单击设计图面，再从快捷菜单中选择“添加”，向设计器添加元素。  
  
 在“架构”视图中，您可以：  
  
-   构造和修改现有的 XML 架构及 ADO.NET 数据集  
  
-   创建和编辑表之间的关系  
  
-   创建和编辑键  
  
-   基于 XML 架构生成 ADO.NET 数据集  
  
> [!NOTE]  
>  “架构”视图中元素的布局存储在 .xsx 文件中，通过在解决方案资源管理器的工具栏中单击 **“显示所有文件”** ，再展开 .xsd 文件，可以看到该文件。 如果没有 .xsx 文件，则表示在 XML 设计器中从未打开过 .xsd 文件。  
  
### <a name="customizing-schema-view"></a>自定义“架构”视图  
 通过以下功能可以修改“架构”视图中元素的可视布局：  
  
-   缩放  
  
-   展开或折叠嵌套的元素  
  
-   自动排列元素的布局  
  
-   重置折叠元素的默认状态  
  
##### <a name="to-expand-hidden-nested-elements"></a>展开隐藏的嵌套元素  
  
-   单击元素底部的加号图标。  
  
##### <a name="to-collapse-nested-elements"></a>折叠嵌套的元素  
  
-   单击希望显示在设计器中的最底部元素的减号图标。  
  
## <a name="data-view"></a>数据视图  
 “数据”视图提供一个可用于修改 .xml 文件的数据网格。 在“数据”视图中只能编辑 XML 文件中的内容（不包括标记和结构）。  
  
 “数据”视图中有两个单独的区域：“数据表”和“数据”。 “数据表”区域用于按照嵌套顺序（从最外层到最内层）列出 XML 文件中所定义的关系。 **“数据”** 区域是一个数据网格，会根据“数据表”区域中的所选内容显示数据。  
  
> [!NOTE]  
>  新创建的 XML 文件不包含数据，因此不能在“数据”视图中显示。 另外，有一些 XML 文档实例根本不能调用“数据”视图。 即使系统认为 XML 文件的格式正确，但如果其中的数据不是结构化数据，则在尝试切换到“数据”视图时将生成以下消息：“尽管此文档的格式良好，但它包含了数据视图无法显示的结构”。  
  
 在“数据”视图中，您可以：  
  
-   手动填充数据表  
  
-   编辑现有的数据表  
  
-   基于 XML 文档生成 XML 架构  
  
## <a name="xml-view"></a>XML 视图  
 XML 视图提供了一个用于编辑原始 XML 数据的编辑器，并提供有 IntelliSense 和颜色编码功能。 在处理具有关联架构的 .xsd 文件和 .xml 文件时，可以使用语句结束功能。 键入 < 即可开始一个标记，并将显示对于所在位置有效的元素列表。 在键入元素名称并按空格键后，将显示相应元素所支持属性的列表。  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] IntelliSense 选项。 若要在 XML 编辑器中访问这些选项，请在 **“编辑”** 菜单上单击 **IntelliSense**。  
  
## <a name="showplan-view"></a>“显示计划”视图  
 在使用 SET SHOWPLAN_XML ON 选项创建查询计划时，可以将其保存为 XML 格式。 双击带有 .showplan 扩展名的文件即可打开该查询计划。  
  
## <a name="see-also"></a>另请参阅  
 [以 XML 格式保存执行计划](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
