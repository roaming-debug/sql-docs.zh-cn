---
description: XML 源
title: XML 源 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.xmlsource.f1
- sql13.dts.designer.xmlsourceadapter.connectionmanager.f1
- sql13.dts.designer.xmlsourceadapter.columns.f1
- sql13.dts.designer.xmlsourceadapter.erroroutput.f1
helpviewer_keywords:
- sources [Integration Services], XML
- XML source [Integration Services]
- XML Source Editor
ms.assetid: 68c27ea5-e93d-4e26-bfb2-d967ca0a5282
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c066add73dbc8049f389c828363f1157bb39edae
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194543"
---
# <a name="xml-source"></a>XML 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  XML 源读取 XML 数据文件，并用数据填充源输出中的列。  
  
 XML 文件中的数据常常包含层次结构关系。 例如，XML 数据文件可以表示目录和目录中的项。 必须先确定 XML 数据文件中元素的关系，并且为文件中的每个元素都生成了一个输出，数据才能进入数据流。  
  
## <a name="schemas"></a>架构  
 XML 源使用某种架构来解释 XML 数据。 XML 源支持使用 XML 架构定义 (XSD) 文件或内联架构将 XML 数据解释为表格格式。 如果使用 **“XML 源编辑器”** 对话框配置 XML 源，用户界面可以根据指定的 XML 数据文件生成 XSD。  
  
> [!NOTE]  
>  不支持 DTD。  
  
 这些架构仅可以支持一个命名空间；不支持架构集合。  
  
> [!NOTE]  
>  XML 源并不根据 XSD 来验证 XML 文件中的数据。  
  
## <a name="xml-source-editor"></a>XML 源编辑器  
 XML 文件中的数据常常包含层次结构关系。 **“XML 源编辑器”** 对话框使用指定的架构生成 XML 源输出。 您可以指定 XSD 文件，使用内联架构或根据指定的 XML 数据文件生成 XSD。 该架构必须在设计时可用。  
  
 XML 源通过为 XML 文件中每个包含其他元素的元素创建一个输出，以便根据 XML 数据生成表格结构。 例如，如果 XML 数据表示目录和目录中的项，则 XML 源将为目录创建一个输出，而且为目录包含的每种类型的项都创建一个输出。 每项的输出将包含该项的属性的输出列。  
  
 为了在输出中提供关于数据层次结构关系的信息，XML 源在输出中添加了一个为各个子元素标识父元素的列。 通过使用带有不同类型项的目录示例，每个项将具有一个用于标识该项所属目录的列值。  
  
 XML 源为每个元素创建一个输出，但您不需要使用所有输出。 您可以删除不想使用的输出，或只是不将其连接到下游组件。  
  
 XMl 源还生成输出名称，以确保输出名称明确。 这些名称可能比较长，而且可能没有以对您有用的方式标识输出。 可以重命名输出，但是要保持其名称的唯一性。 还可以修改输出列的数据类型和长度。  
  
 对于每个输出，XML 源都会添加一个错误输出。 默认情况下，错误输出中的列的数据类型为 Unicode 字符串数据类型 (DT_WSTR)，列的长度为 255 个字符，但您可以通过修改列的数据类型和长度来配置错误输出中的列。  
  
 如果 XML 数据文件包含 XSD 中没有的元素，则这些元素将被忽略，并且不会为其生成输出。 另一方面，如果 XML 数据文件缺少 XSD 中存在的元素，则输出将包含空值列。  
  
 从 XML 数据文件提取数据后，数据将转换为 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型。 不过，XML 源不能将 XML 数据转换为 DT_TIME2 或 DT_DBTIMESTAMP2 数据类型，这是因为源不支持这些数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
 XSD 或内联架构可能为元素指定了数据类型，但如果未指定，则“XML 源编辑器”  对话框将为输出中包含该元素的列指定 Unicode 字符串数据类型 (DT_WSTR)，并将列长度设置为 255 个字符。  
  
 如果该架构指定了元素的最大长度，则输出列的长度将设置为此值。 如果最大长度大于将元素转换为的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 数据类型所支持的长度，则数据将被截断为该数据类型的最大长度。 例如，如果一个字符串的长度为 5000，则该字符串将因为 DT_WSTR 数据类型的最大长度而截断为 4000 字符；类似地，字节数据将截断为 DT_BYTES 数据类型的最大长度 8000 字符。 如果架构未指定最大长度，则具有任何一种数据类型的列的默认长度都设置为 255。 对 XML 源中的数据截断的处理方式与其他数据流组件中截断的处理方式相同。 有关详细信息，请参阅 [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)。  
  
 您可以修改数据类型和列长度。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。  
  
## <a name="configuration-of-the-xml-source"></a>XML 源的配置  
 XML 源支持三种不同的数据访问模式。 您可以指定 XML 数据文件的文件位置、包含文件位置的变量或包含 XML 数据的变量。  
  
 XML 源包括可以在加载包时通过属性表达式进行更新的 **XMLData** 和 **XMLSchemaDefinition** 自定义属性。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../integration-services/expressions/integration-services-ssis-expressions.md)、[在包中使用属性表达式](../../integration-services/expressions/use-property-expressions-in-packages.md)和 [XML 源自定义属性](../../integration-services/data-flow/xml-source-custom-properties.md)。  
  
 XML 源支持多个常规输出和多个错误输出。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含用于配置 XML 源的“XML 源编辑器”对话框。 此对话框在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中可用。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 **“高级编辑器”** 对话框反映了可以通过编程方式进行设置的属性。 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](./set-the-properties-of-a-data-flow-component.md)  
  
-   [XML 源自定义属性](../../integration-services/data-flow/xml-source-custom-properties.md)  
  
 有关如何设置属性的详细信息，请单击下列主题之一：  
  
-   [设置数据流组件的属性](../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="xml-source-editor-connection-manager-page"></a>XML 源编辑器（“连接管理器”页）
  可以使用 **“XML 源编辑器”** 的 **“连接管理器”** 页指定 XML 文件和转换 XML 数据的 XSD。  
  
### <a name="static-options"></a>静态选项  
 **数据访问模式**  
 指定从源选择数据的方法。  
  
|值|描述|  
|-----------|-----------------|  
|XML 文件位置|从 XML 文件检索数据。|  
|来自变量的 XML 文件|在变量中指定 XML 文件名。<br /><br /> **相关信息**：[在包中使用变量](../integration-services-ssis-variables.md)|  
|来自变量的 XML 数据|从变量检索 XML 数据。|  
  
 **使用内联架构**  
 指定 XML 源数据本身是否包含 XSD 架构（用于定义和验证 XML 源数据的结构和数据）。  
  
 **XSD 位置**  
 键入 XSD 架构文件的路径和文件名，或者可以单击“浏览”定位该文件。  
  
 **“浏览”**  
 使用“打开”对话框定位到 XSD 架构文件。  
  
 **生成 XSD**  
 使用“另存为”对话框可以为自动生成的 XSD 架构文件选择位置。 编辑器将根据 XML 数据的结构来推断架构。  
  
### <a name="data-access-mode-dynamic-options"></a>数据访问模式动态选项  
  
#### <a name="data-access-mode--xml-file-location"></a>数据访问模式 = XML 文件位置  
 **XML 位置**  
 键入 XML 数据文件的路径和文件名，或者通过单击“浏览”查找文件。  
  
 **“浏览”**  
 使用“打开”对话框定位到 XML 数据文件。  
  
#### <a name="data-access-mode--xml-file-from-variable"></a>数据访问模式 = 来自变量的 XML 文件  
 **变量名称**  
 选择包含 XML 文件的路径和文件名的变量。  
  
#### <a name="data-access-mode--xml-data-from-variable"></a>数据访问模式 = 来自变量的 XML 数据  
 **变量名称**  
 选择包含 XML 数据的变量。  
  
## <a name="xml-source-editor-columns-page"></a>XML 源编辑器（“列”页）
  可以使用“XML 源编辑器”对话框的“列”节点，将输出列映射到外部（源）列。  
  
### <a name="options"></a>选项  
 **可用外部列**  
 查看数据源中可用外部列的列表。 无法使用此表添加或删除列。  
  
 **“外部列”**  
 按任务读取外部（源）列的顺序查看这些列。 首先在编辑器中显示的表中清除所选择的列，然后以不同的顺序从列表中选择外部列，即可更改顺序。  
  
 **输出列**  
 为每个输出列提供唯一的名称。 默认值为所选外部（源）列的名称；不过，您也可以任选一个唯一的描述性名称。 所提供的名称将在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中显示。  
  
## <a name="xml-source-editor-error-output-page"></a>XML 源编辑器（“错误输出”页）
  可以使用 **“XML 源编辑器”** 对话框的 **“错误输出”** 页选择错误处理选项，以及设置错误输出列的属性。  
  
### <a name="options"></a>选项  
 **输入/输出**  
 查看数据源的名称。  
  
 **列**  
 查看在“XML 源编辑器”对话框中“连接管理器”页上选择的外部（源）列。  
  
 **错误**  
 指定发生错误时应执行的操作：忽略失败、重定向行或使组件失败。  
  
 **相关主题：** [数据中的错误处理](../../integration-services/data-flow/error-handling-in-data.md)  
  
 **截断**  
 指定发生截断时应执行的操作：忽略失败、重定向行或使组件失败。  
  
 **说明**  
 查看对错误的说明。  
  
 **将此值设置到选定的单元格**  
 指定发生错误或截断时应对所有选定单元格执行的操作：忽略失败、重定向行或使组件失败。  
  
 **应用**  
 将错误处理选项应用到选定的单元格。  
  
## <a name="related-tasks"></a>Related Tasks  
 [使用 XML 源提取数据](../../integration-services/data-flow/extract-data-by-using-the-xml-source.md)