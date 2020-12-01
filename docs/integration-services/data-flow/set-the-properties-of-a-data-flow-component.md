---
description: 设置数据流组件的属性
title: 设置数据流组件的属性 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- components [Integration Services], properties
ms.assetid: 73000ef6-52a2-4dec-8320-0e79acf0c2c5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c7799b5d2f5f541b6713821dccbec820697371ed
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88348943"
---
# <a name="set-the-properties-of-a-data-flow-component"></a>设置数据流组件的属性

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  若要设置数据流组件（包括源、目标和转换）的属性，请使用下列功能之一：  
  
-   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 提供的组件编辑器。 这些编辑器仅包含每个数据流组件的自定义属性。  
  
-   “属性”窗口列出每个元素的组件级自定义属性以及所有数据流元素通用的属性。  
  
-   通过 **“高级编辑器”** 对话框可以访问每个组件的自定义属性。 通过“高级编辑器”对话框，还可以访问所有数据流组件通用的属性，包括输入属性、输出属性、错误输出属性、列属性和外部列属性。  
  
## <a name="set-the-properties-of-a-data-flow-component-with-a-component-editor"></a>使用组件编辑器设置数据流组件的属性  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击“控制流”选项卡，然后双击数据流任务，此任务包含带有要查看和修改属性的组件的数据流。  
  
4.  双击数据流组件。  
  
5.  在组件编辑器中查看或修改属性值，然后关闭编辑器。  
  
6.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  
  
## <a name="set-the-properties-of-a-data-flow-component-in-the-properties-window"></a>在“属性”窗口中设置数据流组件的属性  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击“控制流”选项卡，然后双击包含要查看和修改其属性的组件的数据流任务。  
  
4.  双击数据流组件，然后单击“属性”。  
  
5.  查看或修改属性值，然后关闭 **“属性”** 窗口。  
  
    > [!NOTE]  
    >  许多属性为只读，因此不能修改。  
  
6.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  
  
## <a name="set-the-properties-of-a-data-flow-component-with-the-advanced-editor"></a>使用高级编辑器设置数据流组件的属性  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  单击“控制流”选项卡，然后双击包含要查看或修改的组件的数据流任务。  
  
4.  在数据流设计器中，右键单击数据流组件，然后单击“显示高级编辑器”。  
  
    > [!NOTE]  
    >   在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中，支持多个输入的数据流组件不能使用 **“高级编辑器”**。  
  
5.  在 **“高级编辑器”** 对话框中，执行下列任意步骤：  
  
    -   若要查看和指定组件使用的连接，请单击 **“连接管理器”** 选项卡。  
  
        > [!NOTE]  
        >  **“连接管理器”** 选项卡仅对使用连接管理器连接到数据源（如文件和数据库）的数据流组件可用。  
  
    -   若要查看和修改组件级属性，请单击“组件属性”选项卡。  
  
    -   若要查看和修改外部列和可用输出之间的映射，请单击 **“列映射”** 选项卡。  
  
        > [!NOTE]  
        >  **“列映射”** 选项卡仅在查看或编辑源或者目标时可用。  
  
    -   若要查看可用输入列的列表并更新输出列的名称，请单击 **“输入列”** 选项卡。  
  
        > [!NOTE]  
        >  输入列选项卡只在对转换或目标进行操作时可用。 有关详细信息，请参阅 [Integration Services Transformations](../../integration-services/data-flow/transformations/integration-services-transformations.md)。  
  
    -   若要查看和修改输入、输出和错误输出的属性以及它们包含的列的属性，请单击 **“输入属性和输出属性”** 选项卡。  
  
        > [!NOTE]  
        >  源没有输入。 除了一个可选错误输出之外，目标没有输出。  
  
6.  查看或修改属性值。  
  
7.  单击“确定”。  
  
8.  若要保存已更新的包，请在 **“文件”** 菜单中单击 **“保存选定项”** 。  

## <a name="common-properties-of-data-flow-components"></a>数据流组件的通用属性
[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中的数据流对象在组件级、输入和输出级以及输入列和输出列级具有通用属性和自定义属性。 其中许多属性的值是只读的，由数据流引擎在运行时分配。  
  
 本主题列出并描述了数据流对象的通用属性。  
  
-   [组件](#components)  
  
-   [输入](#inputs)  
  
-   [Input columns](#inputcolumns)  
  
-   [输出](#outputs)  
  
-   [输出列](#outputcolumns)  
  
 
###  <a name="component-properties"></a><a name="components"></a>组件属性  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中，数据流中的组件实现 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> 接口。  
  
 下表介绍了数据流中的组件的属性。 其中某些属性的值是只读的，由数据流引擎在运行时分配。  
  
|属性|数据类型|说明|  
|--------------|---------------|-----------------|  
|ComponentClassID|String|组件的 CLSID。|  
|ContactInfo|String|组件开发人员的联系信息。|  
|说明|String|对数据流组件的说明。 此属性的默认值是数据流组件的名称。|  
|ID|Integer|唯一标识此组件实例的值。|  
|IdentificationString|String|标识组件。|  
|IsDefaultLocale|布尔|指示组件是否使用其所属的数据流任务的区域设置。|  
|LocaleID|Integer|包运行时数据流组件使用的区域设置。 数据流组件可以使用所有 Windows 区域设置。|  
|名称|String|数据流组件的名称。|  
|PipelineVersion|Integer|将某组件设计为要在其中执行的数据流任务的版本。|  
|UsesDispositions|布尔|指示组件是否有错误输出。|  
|ValidateExternalMetadata|Boolean|指示外部列的元数据是否经过验证。 此属性的默认值为 **True**。|  
|版本|Integer|组件的版本。|  
  
###  <a name="input-properties"></a><a name="inputs"></a>输入属性  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中，转换和目标都具有输入。 数据流中的组件的输入实现了 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> 接口。  
  
 下表描述了数据流中的组件的输入属性。 其中某些属性的值是只读的，由数据流引擎在运行时分配。  
  
|属性|数据类型|说明|  
|--------------|---------------|-----------------|  
|说明|String|输入的说明。|  
|ErrorOrTruncationOperation|String|一个可选字符串，它指定处理行时可以发生的错误或截断的类型。|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于指定错误的处理方式的值。 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。|  
|HasSideEffects|布尔|指示当组件没有附加到下游组件并且 **RunInOptimizedMode** 为 **true** 时，是否可以从数据流的执行计划中删除该组件。|  
|ID|Integer|用于唯一标识输入的值。|  
|IdentificationString|String|用于标识输入的字符串。|  
|IsSorted|布尔|指示输入中的数据是否已排序。|  
|名称|String|输入的名称。|  
|SourceLocale|Integer|输入数据的区域设置 ID (LCID)。|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于确定组件如何处理在处理行时发生的截断的值。 . 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。|  
  
 目标以及某些转换不支持错误输出，这些组件的 ErrorRowDisposition 和 TruncationRowDisposition 属性是只读的。  
  
###  <a name="input-column-properties"></a><a name="inputcolumns"></a>输入列属性  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中，输入包含输入列集合。 数据流中的组件的输入列实现了 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInputColumn100> 接口。  
  
 下表描述了数据流中的组件的输入列属性。 其中某些属性的值是只读的，由数据流引擎在运行时分配。  
  
|属性|数据类型|说明|  
|--------------|---------------|-----------------|  
|ComparisonFlags|Integer|一组标志，用于指定数据类型为 character 的列的比较方式。 有关详细信息，请参阅 [Comparing String Data](../../integration-services/data-flow/comparing-string-data.md)。|  
|说明|String|对输入列的说明。|  
|ErrorOrTruncationOperation|String|一个可选字符串，它指定处理行时可以发生的错误或截断的类型。|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于指定错误的处理方式的值。 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。|  
|ExternalMetadataColumnID|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumn100>|分配给输入列的外部元数据列的 ID。|  
|ID|Integer|用于唯一标识输入列的值。|  
|IdentificationString|String|用于标识输入列的字符串。|  
|LineageID|Integer|上游列的 ID。|  
|LineageIdentificationString|String|标识字符串，包含上游列的名称。|  
|名称|String|输入列的名称。|  
|SortKeyPosition|Integer|用于指示单个列是否已排序、其排序顺序以及多个列的排序顺序的值。 如何值为 **0** ，则表示未对该列进行排序。  有关详细信息，请参阅 [为合并转换和合并联接转换排序数据](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)。|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于确定组件如何处理在处理行时发生的截断的值。 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。|  
|UpstreamComponentName|String|上游组件的名称。|  
|UsageType|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSUsageType>|用于确定组件如何使用输入列的值。|  
  
 输入列还具有“数据类型属性”下描述的数据类型属性。  
  
###  <a name="output-properties"></a><a name="outputs"></a>输出属性  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中，源和转换具有输出。 数据流中的组件的输出实现了 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> 接口。  
  
 下表描述了数据流中的组件的输出属性。 其中某些属性的值是只读的，由数据流引擎在运行时分配。  
  
|属性|数据类型|说明|  
|--------------|---------------|-----------------|  
|DeleteOutputOnPathDetached|布尔|用于确定当输出与路径分离时数据流引擎是否将其删除的值。|  
|说明|String|对输出的说明。|  
|ErrorOrTruncationOperation|String|一个可选字符串，它指定处理行时可以发生的错误或截断的类型。|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于指定错误的处理方式的值。 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。|  
|ExclusionGroup|Integer|用于标识一组互斥输出的值。|  
|HasSideEffects|布尔|用于指示当组件没有附加到上游组件并且 **RunInOptimizedMode** 为 **true** 时是否可以从数据流的执行计划中删除该组件的值。|  
|ID|Integer|用于唯一标识输出的值。|  
|IdentificationString|String|用于标识输出的字符串。|  
|IsErrorOut|布尔|指示输出是否为错误输出。|  
|IsSorted|布尔|指示输出是否已排序。 默认值为 **False**。<br /><br /> **\*\* 重要提示\*\*** 将 IsSorted 属性的值设置为 True 时，不会对数据进行排序 。 此属性仅向下游组件提示数据之前已经过排序。 有关详细信息，请参阅 [为合并转换和合并联接转换排序数据](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)。|  
|名称|String|输出的名称。|  
|SynchronousInputID|Integer|与输出同步的输入的 ID。|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于确定组件如何处理在处理行时发生的截断的值。 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。|  
  
###  <a name="output-column-properties"></a><a name="outputcolumns"></a>输出列属性  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中，输出包含输出列集合。 数据流中的组件的输出列实现了 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100> 接口。  
  
 下表描述了数据流中的组件的输出列属性。 其中某些属性的值是只读的，由数据流引擎在运行时分配。  
  
|属性|数据类型|说明|  
|--------------|---------------|-----------------|  
|ComparisonFlags|Integer|一组标志，用于指定数据类型为 character 的列的比较方式。 有关详细信息，请参阅 [Comparing String Data](../../integration-services/data-flow/comparing-string-data.md)。|  
|说明|String|对输出列的说明。|  
|ErrorOrTruncationOperation|String|一个可选字符串，它指定处理行时可以发生的错误或截断的类型。|  
|ErrorRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于指定错误的处理方式的值。 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。 默认值为 **Fail component**。|  
|ExternalMetadataColumnID|Integer|分配给输入列的外部元数据列的 ID。|  
|ID|Integer|用于唯一标识输出列的值。|  
|IdentificationString|String|用于标识输出列的字符串。|  
|LineageID|Integer|输出列的 ID。 下游组件使用此值引用列。|  
|LineageIdentificationString|String|标识字符串，包含列的名称。|  
|名称|String|输出列的名称。|  
|SortKeyPosition|Integer|用于指示单个列是否已排序、其排序顺序以及多个列的排序顺序的值。 如何值为 **0** ，则表示未对该列进行排序。 有关详细信息，请参阅 [为合并转换和合并联接转换排序数据](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)。|  
|SpecialFlags|Integer|包含输出列的特殊标志的值。|  
|TruncationRowDisposition|<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSRowDisposition>|用于确定组件如何处理在处理行时发生的截断的值。 具体的值为 **Fail component**、 **Ignore failure** 和 **Redirect row**。 默认值为 **Fail component**。|  
  
 输出列还包括一组数据类型属性。  
  
### <a name="external-metadata-column-properties"></a>外部元数据列属性  
 在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 对象模型中，输入和输出可以包含一组外部元数据列。 数据流中的组件的外部元数据列实现了 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumn100> 接口。  
  
 下表描述了数据流中的组件的外部元数据列属性。 其中某些属性的值是只读的，由数据流引擎在运行时分配。  
  
|属性|数据类型|说明|  
|--------------|---------------|-----------------|  
|说明|String|对外部列的说明。|  
|ID|Integer|用于唯一标识列的值。|  
|IdentificationString|String|用于标识列的字符串。|  
|名称|String|外部列的名称。|  
  
 外部元数据列还包括一组数据类型属性。  
  
### <a name="data-type-properties"></a>数据类型属性  
 输出列和外部元数据列还包括一组数据类型属性。 这些属性可能为读/写属性或只读属性，具体取决于列的数据类型。  
  
 下表描述了输出列和外部元数据列的数据类型属性。  
  
|属性|数据类型|说明|  
|--------------|---------------|-----------------|  
|CodePage|Integer|指定非 Unicode 字符串数据的代码页。|  
|数据类型|Integer（枚举）|列的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。|  
|长度|Integer|以字符计的列的长度。|  
|精度|Integer|数字列的精度。|  
|缩放|Integer|数字列的小数位数。|  

## <a name="custom-properties-of-data-flow-components"></a>数据流组件的自定义属性
有关自定义属性的信息，请参阅以下主题  
  
-   [ADO NET 自定义属性](../../integration-services/data-flow/ado-net-custom-properties.md)  
  
-   [CDC 控制任务自定义属性](../../integration-services/control-flow/cdc-control-task-custom-properties.md)  
  
-   [CDC 源自定义属性](../../integration-services/data-flow/cdc-source-custom-properties.md)  
  
-   [数据挖掘模型定型目标自定义属性](../../integration-services/data-flow/data-mining-model-training-destination-custom-properties.md)  
  
-   [DataReader 目标自定义属性](../../integration-services/data-flow/datareader-destination-custom-properties.md)  
  
-   [维度处理目标自定义属性](../../integration-services/data-flow/dimension-processing-destination-custom-properies.md)  
  
-   [Excel 自定义属性](../../integration-services/data-flow/excel-custom-properties.md)  
  
-   [平面文件自定义属性](../../integration-services/data-flow/flat-file-custom-properties.md)  
  
-   [ODBC 目标自定义属性](../../integration-services/data-flow/odbc-destination-custom-properties.md)  
  
-   [ODBC Source Custom Properties](../../integration-services/data-flow/odbc-source-custom-properties.md)  
  
-   [OLE DB Custom Properties](../../integration-services/data-flow/ole-db-custom-properties.md)OLE DB 自定义属性  
  
-   [分区处理目标自定义属性](../../integration-services/data-flow/partition-processing-destination-custom-properties.md)  
  
-   [原始文件自定义属性](../../integration-services/data-flow/raw-file-custom-properties.md)  
  
-   [记录集目标自定义属性](../../integration-services/data-flow/recordset-destination-custom-properties.md)  
  
-   [SQL Server Compact Edition 目标自定义属性](../../integration-services/data-flow/sql-server-compact-edition-destination-custom-properties.md)  
  
-   [SQL Server 目标自定义属性](../../integration-services/data-flow/sql-server-destination-custom-properties.md)  
  
-   [转换自定义属性](../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
-   [XML 源自定义属性](../../integration-services/data-flow/xml-source-custom-properties.md)  
  
## <a name="use-an-expression-in-a-data-flow-component"></a>在数据流组件中使用表达式
本过程介绍如何将表达式添加到有条件拆分转换或派生列转换中。 有条件拆分转换使用表达式定义将数据行定向到转换输出的条件，而派生列转换使用表达式定义分配给列的值。  
  
 若要在转换中实现表达式，包必须至少已经包含一项数据流任务和一个源。 
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在解决方案资源管理器中，双击该包将其打开。  
  
3.  在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中，单击 **“控制流”** 选项卡，然后单击包含要在其中实现表达式的数据流的数据流任务。  
  
4.  单击 **“数据流”** 选项卡，然后将有条件拆分转换或派生列转换从 **“工具箱”** 拖到设计图面。  
  
5.  将绿色的连接器从源或转换拖到有条件拆分转换或派生列转换。  
  
6.  双击该转换打开其对话框。  
  
7.  在左窗格中，展开“变量”显示系统变量和用户定义的变量，然后展开“列”显示转换输入列。  
  
8.  在右窗格中，展开“数学函数”、“字符串函数”、“日期/时间函数”、“NULL 函数”、“类型转换”和“运算符”，访问表达式语法提供的函数、转换和运算符。  
  
9. 根据转换的类型，可以执行下列某项操作来生成表达式：  
  
    -   在 **“有条件拆分转换编辑器”** 对话框中，将变量、列、函数、运算符和转换拖到 **“条件”** 列中。 另外，您还可以直接在 **“条件”** 列中键入表达式。  
  
    -   在 **“派生列转换编辑器”** 对话框中，将变量、列、函数、运算符和转换拖到 **“表达式”** 列中。 另外，您还可以直接在 **“表达式”** 列中键入表达式。  
  
        > [!NOTE]  
        >   当焦点离开 **“条件”** 列或 **“表达式”** 列时，表达式文本可能会突出显示，指示表达式语法不正确。  
  
10. 单击 **“确定”**，退出对话框。  
  
    > [!NOTE]  
    >  如果该表达式无效，则会出现一个警告，描述表达式中的语法错误。  

## <a name="data-flow-properties-that-you-can-set-with-an-expression"></a>可以使用表达式设置的数据流属性
可以使用数据流任务容器上的可用属性表达式来指定数据流对象的某些属性的值。  
  
 有关使用属性表达式的信息，请参阅 [在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)。  
  
 可以使用属性表达式为包的每个已部署的实例自定义配置。 也可以使用属性表达式来为包指定运行时约束，方法是将 **/set** 选项与 **dtexec** 命令提示实用工具一起使用。 例如，可以约束排序转换使用的 **MaximumThreads** ，或约束模糊分组和模糊查找转换的 **MaxMemoryUsage** 。 如果无约束，则这些转换可能会在内存中高速缓存大量数据。  
  
 若要为本主题中列出的数据流对象的其中一个属性指定属性表达式，请在设计器的 **“控制流”** 图面上选择该数据流任务，或选择设计器的 **“数据流”** 选项卡但不选择任何单个组件或路径，以此方式显示数据流任务的 **“属性”** 窗口。 选择“表达式”属性，然后单击省略号 (...) 以显示“属性表达式编辑器”对话框。 下拉“属性”列表以选择某个属性，然后在“表达式”文本框中键入一个表达式，或者单击省略号 (...) 以显示“表达式生成器”对话框。  
  
 **“属性”** 列表仅显示那些已位于设计器的 **“数据流”** 图面上的数据流对象的可用属性。 因此，不能使用 **“属性”** 列表来查看那些支持属性表达式的数据流对象的所有可能的属性。 例如，如果已将 ADO NET 源放置在设计器图面上，则“属性”列表包含 **[ADO NET Source].[SqlCommand]** 属性的条目。 该列表还显示了数据流任务自身的许多属性。  
 
 下面的列表中的属性值可以使用属性表达式来指定。  
  
### <a name="data-flow-sources"></a>数据流源  
  
|数据流对象|属性|  
|----------------------|--------------|  
|ADO NET 源|TableOrViewName 属性<br /><br /> SqlCommand 属性|  
|XML 源|XMLData 属性<br /><br /> XMLSchemaDefinition 属性|  
  
### <a name="data-flow-transformations"></a>数据流转换  
 有关这些自定义属性的详细信息，请参阅 [Transformation Custom Properties](../../integration-services/data-flow/transformations/transformation-custom-properties.md)。  
  
|数据流对象|属性|  
|----------------------|--------------|  
|有条件拆分转换|FriendlyExpression 属性|  
|派生列转换|FriendlyExpression 属性|  
|模糊分组转换|MaxMemoryUsage 属性|  
|模糊查找转换|MaxMemoryUsage 属性|  
|查找转换|SqlCommand 属性<br /><br /> SqlCommandParam 属性|  
|OLE DB 命令转换|SqlCommand 属性|  
|百分比抽样转换|SamplingValue 属性|  
|透视转换|PivotKeyValue 属性|  
|行抽样转换|SamplingValue 属性|  
|排序转换|MaximumThreads 属性|  
|逆透视转换|PivotKeyValue 属性|  
  
### <a name="data-flow-destinations"></a>数据流目标  
  
|数据流对象|属性|  
|----------------------|--------------|  
|ADO NET 目标|TableOrViewName 属性<br /><br /> BatchSize 属性<br /><br /> CommandTimeout 属性|  
|平面文件目标|Header 属性|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Compact 目标|TableName 属性|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]目标|BulkInsertTableName 属性<br /><br /> BulkInsertFirstRow 属性<br /><br /> BulkInsertLastRow 属性<br /><br /> BulkInsertOrder 属性<br /><br /> Timeout 属性|  

