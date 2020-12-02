---
description: 平面文件连接管理器
title: 平面文件连接管理器 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.ffileconnection.general.f1
- sql13.dts.designer.ffileconnection.columns.f1
- sql13.dts.designer.ffileconnection.columnproperties.f1
- sql13.dts.designer.ffileconnection.preview.f1
helpviewer_keywords:
- connection managers [Integration Services], Flat File
- connections [Integration Services], flat files
- files [Integration Services], connections
- Flat File connection manager
- flat files
- flat file connections [Integration Services]
ms.assetid: 7830f80d-af32-4e8f-a6fc-f03af6bc1946
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 93890a2bf8fc0b9209eb9582c8129ee6856865ee
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130105"
---
# <a name="flat-file-connection-manager"></a>平面文件连接管理器

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  平面文件连接管理器使包可以访问平面文件中的数据。 例如，平面文件源和目标可以使用平面文件连接管理器提取和加载数据。  
  
 平面文件连接管理器只能访问一个文件。 若要引用多个文件，请使用多平面文件连接管理器，而不用平面文件连接管理器。 有关详细信息，请参阅 [Multiple Flat Files Connection Manager](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md)。  
  
## <a name="column-length"></a>列长度  
 默认情况下，平面文件连接管理器会将字符串列的长度设置为 50 个字符。 在 **“平面文件连接管理器编辑器”** 对话框中，可以计算示例数据，并自动调整这些列的长度，以防止发生数据截断或超过列宽的情况。 而且，除非随后在平面文件源或转换中调整列长度，否则字符串列的列长度将在整个数据流中保持不变。 如果这些字符串列映射到更窄的目标列，用户界面中将显示警告。 此外，在运行时，可能由于数据截断而发生错误。 若要避免错误或截断，可以在平面文件连接管理器、平面文件源或转换中调整列的大小，以便与目标列兼容。 若要修改输出列的长度，请在 **“高级编辑器”** 对话框中的 **“输入属性和输出属性”** 选项卡上设置输出列的 **Length** 属性。  
  
 在已添加并配置使用连接管理器的平面文件源之后，如果在平面文件连接管理器中更新列长度，则不必在平面文件源中手动调整输出列的大小。 打开 **“平面文件源”** 对话框时，平面文件源将提供同步列元数据的选项。  
  
## <a name="configuration-of-the-flat-file-connection-manager"></a>平面文件连接管理器的配置  
 将平面文件连接管理器添加到包时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 会创建将在运行时解析为平面文件连接的连接管理器，同时将设置该平面文件连接的属性，并将该平面文件连接管理器添加到包的 Connections 集合。  
  
 该连接管理器的 **ConnectionManagerType** 属性设置为 **FLATFILE**。  
  
 默认情况下，平面文件连接管理器始终检查未被引号引起的数据中的行分隔符，在找到行分隔符时开始一个新行。 这使连接管理器可以正确地分析具有缺少列字段的行的文件。  
  
 在某些情况下，禁用此功能可以提高包性能。 您可以通过将平面文件连接管理器属性 **AlwaysCheckForRowDelimiters** 设置为 **False** 来禁用此功能。  
  
 可以按下列方式配置平面文件连接管理器：  
  
-   指定要使用的文件、区域设置和代码页。 区域设置用于解释受区域设置影响的数据（如日期），代码页用于将字符串数据转换为 Unicode。  
  
-   指定文件格式。 可以使用带分隔符、具有固定宽度或右边未对齐的文件格式。  
  
-   指定标题行、数据流和列分隔符。 列分隔符可以在文件级别设置，而在列级别被覆盖。  
  
-   指示文件中的第一行是否包含列名称。  
  
-   指定文本限定符。 可以将每一列配置为识别文本限定符。  
  
     平面文件连接管理器支持将限定符嵌入限定的字符串。 文本限定符的双实例将被解释为文字，即该字符串的单个实例。 例如，如果文本限定符是单引号并且输入数据为 'abc'、'def'、'g'hi'，则输出数据为 abc、def、g'hi。 但是，在限定的字符串中嵌入限定符的实例将导致平面文件源失败，错误为 DTS_E_PRIMEOUTPUTFAILED。
  
-   对各列设置诸如名称、数据类型和最大宽度等属性。  
  
 通过在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]的“属性”窗口中指定表达式，可以设置平面文件连接管理器的 ConnectionString 属性。 为避免验证错误，请执行以下操作。  
  
-   使用表达式指定文件时，在 **“平面文件连接管理器编辑器”** 的 **“文件名”** 框中添加文件路径。  
  
-   将平面文件连接管理器的 **“DelayValidation”** 属性设置为 **“True”**。  
  
 使用平面文件连接管理器和平面文件目标，您可以在运行时使用表达式创建文件名。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关以编程方式配置连接管理器的信息，请参阅 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> 和 [以编程方式添加连接](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md)项目。  
  
## <a name="flat-file-connection-manager-editor-general-page"></a>平面文件连接管理器编辑器（“常规”页）
  可以使用 **“平面文件连接管理器编辑器”** 对话框的 **“常规”** 页选择文件和数据格式。 使用平面文件连接可以将包连接到文本文件。  
  
 若要了解有关平面文件连接管理器的详细信息，请参阅 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **连接管理器名称**  
 为工作流中的平面文件连接提供唯一的名称。 所提供的名称将在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中显示。  
  
 **说明**  
 描述此连接。 最好按照连接的用途对其进行说明，使包的说明一目了然，且更便于维护。  
  
 **文件名**  
 键入要在平面文件连接中使用的路径和文件名。  
  
 **“浏览”**  
 定位要在平面文件连接中使用的文件名。  
  
 **区域设置**  
 指定区域设置，以便为排序以及日期和时间格式提供语言特定的信息。  
  
 **Unicode**  
 指示是否使用 Unicode。 如果使用 Unicode，则不能指定代码页。  
  
 **代码页**  
 指定非 Unicode 文本的代码页。  
  
 **格式**  
 指示文件是否使用带分隔符、固定宽度或右边未对齐的格式。  
  
|值|说明|  
|-----------|-----------------|  
|带分隔符|各列之间由在 **“列”** 页上指定的分隔符隔开。|  
|固定宽度|列的宽度固定。|  
|右边未对齐|在右边未对齐的文件中，除最后一列之外的每一列的宽度都固定。 它由行分隔符分隔。|  
  
 **文本限定符**  
 指定要使用的文本限定符。 例如，可以指定文本字段必须用引号括起来。  
  
> [!NOTE]  
>  选择文本限定符之后，不能重新选择“无”选项。 键入 **None** 以取消选择文本限定符。  
  
 **标题行分隔符**  
 从标题行的分隔符列表中选择，或输入分隔符文本。  
  
|值|说明|  
|-----------|-----------------|  
|**{CR}{LF}**|标题行由回车符和换行符的组合分隔。|  
|**{CR}**|标题行由回车符分隔。|  
|**{LF}**|标题行由换行符分隔。|  
|**分号 {;}**|标题行由分号分隔。|  
|**冒号 {:}**|标题行由冒号分隔。|  
|**逗号 {,}**|标题行由逗号分隔。|  
|**制表符 {t}**|标题行由制表符分隔。|  
|**竖线 {&#124;}** 。|标题行由竖线分隔。|  
  
 **要跳过的标题行数**  
 指定要跳过的标题行数或初始数据行数（如果有的话）。  
  
 **第一个数据行中的列名称**  
 指示在第一个数据行中是否要求列名或提供列名。  
## <a name="flat-file-connection-manager-editor-columns-page"></a>平面文件连接管理器编辑器（“列”页）
  可以使用 **“平面文件连接管理器编辑器”** 对话框的 **“列”** 页，指定行和列的信息以及预览相应的文件。  
  
 若要了解有关平面文件连接管理器的详细信息，请参阅 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
### <a name="static-options"></a>静态选项  
 **连接管理器名称**  
 为工作流中的平面文件连接提供唯一的名称。 所提供的名称将在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中显示。  
  
 **说明**  
 描述此连接。 最好按照连接的用途对其进行说明，使包的说明一目了然，且更便于维护。  
  
### <a name="flat-file-format-dynamic-options"></a>平面文件格式动态选项  
  
#### <a name="format--delimited"></a>格式 = 带分隔符  
 **行分隔符**  
 从可用行分隔符的列表中选择，或输入分隔符文本。  
  
|值|说明|  
|-----------|-----------------|  
|**{CR}{LF}**|行由回车符和换行符的组合分隔。|  
|**{CR}**|行由回车符分隔。|  
|**{LF}**|行由换行符分隔。|  
|**分号 {;}**|行由分号分隔。|  
|**冒号 {:}**|行由冒号分隔。|  
|**逗号 {,}**|行由逗号分隔。|  
|**制表符 {t}**|行由制表符分隔。|  
|**竖线 {&#124;}** 。|行由竖线分隔。|  
  
 **列分隔符**  
 从可用列分隔符的列表中选择，或输入分隔符文本。  
  
|值|说明|  
|-----------|-----------------|  
|**{CR}{LF}**|列由回车符和换行符的组合分隔。|  
|**{CR}**|列由回车符分隔。|  
|**{LF}**|列由换行符分隔。|  
|**分号 {;}**|列由分号分隔。|  
|**冒号 {:}**|列由冒号分隔。|  
|**逗号 {,}**|列由逗号分隔。|  
|**制表符 {t}**|列由制表符分隔。|  
|**竖线 {&#124;}** 。|列由竖线分隔。|  
  
 **“刷新”**  
 通过单击“刷新”查看更改要跳过的分隔符后的效果。 只有在更改其他连接选项之后，此按钮才可见。  
  
 **预览行**  
 查看平面文件中的示例数据，这些数据已按所选的选项划分为列和行。  
  
 **重置列**  
 通过单击“重置列”可以删除除原始列之外的所有列。  
  
#### <a name="format--fixed-width"></a>格式 = 固定宽度  
 **字体**  
 选择用于显示预览数据的字体。  
  
 **源数据列**  
 通过滑动垂直的红色行标记可以调整行宽，通过单击预览窗口顶部的标尺可以调整列宽。  
  
 **行宽**  
 为各列添加分隔符之前，先指定行的长度。 或者，拖动预览窗口中的垂直红线，以标记行尾。 行宽值将自动更新。  
  
 **重置列**  
 通过单击“重置列”可以删除除原始列之外的所有列。  
  
#### <a name="format--ragged-right"></a>格式 = 右边未对齐  
  
> [!NOTE]  
>  在右边未对齐的文件中，除最后一列之外的每一列的宽度都固定。 它由行分隔符分隔。  
  
 **字体**  
 选择用于显示预览数据的字体。  
  
 **源数据列**  
 通过滑动垂直的红色行标记可以调整行宽，通过单击预览窗口顶部的标尺可以调整列宽。  
  
 **行分隔符**  
 从可用行分隔符的列表中选择，或输入分隔符文本。  
  
|值|说明|  
|-----------|-----------------|  
|**{CR}{LF}**|行由回车符和换行符的组合分隔。|  
|**{CR}**|行由回车符分隔。|  
|**{LF}**|行由换行符分隔。|  
|**分号 {;}**|行由分号分隔。|  
|**冒号 {:}**|行由冒号分隔。|  
|**逗号 {,}**|行由逗号分隔。|  
|**制表符 {t}**|行由制表符分隔。|  
|**竖线 {&#124;}** 。|行由竖线分隔。|  
  
 **重置列**  
 通过单击“重置列”可以删除除原始列之外的所有列。  
## <a name="flat-file-connection-manager-editor-advanced-page"></a>平面文件连接管理器编辑器（“高级”页）
  可以使用 **“平面文件连接管理器编辑器”** 对话框的 **“高级”** 页，设置指定 Integration Services 如何读写平面文件中的数据的属性。 可以更改平面文件中各个列的名称，并设置包括文件中每个列的数据类型和分隔符在内的属性。  
  
 默认情况下，字符串列的长度为 50 个字符。 可以调整这些列的长度，以免数据截断或超出列宽。 还可以更新其他元数据以便与目标列兼容。 例如，可以将只包含整型数据的列的数据类型更改为数值数据类型，例如 DT_I2。 可以手动进行这些修改，也可以单击 **“选择类型”** 按钮，以使用 **“提供列类型建议”** 对话框来评估示例数据并自动进行其中一些更改。  
  
 若要了解有关平面文件连接管理器的详细信息，请参阅 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **连接管理器名称**  
 为工作流中的平面文件连接管理器提供唯一的名称。 所提供的名称将在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中显示。  
  
 **说明**  
 描述连接管理器。 最好按照连接管理器的用途对其进行说明，使包的说明一目了然，且更便于维护。  
  
 **配置各列的属性**  
 选择左窗格中的列可在右窗格中查看列的属性。 请参阅下表以了解数据类型属性的说明。 列出的部分属性仅对某些平面文件格式是可配置的。  
  
|属性|说明|  
|--------------|-----------------|  
|**ColumnType**|表示列是由分隔符分隔、还是固定宽度，或是右边未对齐。 此属性为只读。 在右边未对齐的文件中，除最后一列之外的每一列的宽度都固定。 它由行分隔符分隔。|  
|**OutputColumnWidth**|指定值存储为字节数；对于 Unicode 文件，此值对应于字符数。 在数据流任务中，此值用于设置平面文件源的输出列宽。 在对象模型中，此属性的名称为 MaximumWidth。|  
|**DataType**|从可用数据类型的列表中进行选择。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。|  
|**TextQualified**|指示文本数据周围是否有文本限定符（例如引号字符）。<br /><br /> True：平面文件中的文本数据是受限定的。 False：平面文件中的文本数据是不受限定的。|  
|**名称**|提供说明性列名。 如果不输入名称，则 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 将自动创建名称，格式为“列 0”、“列 1”，依此类推。|  
|**DataScale**|指定数字数据的小数位数。 小数位数是指小数点后的位数。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。|  
|**ColumnDelimiter**|从可用列分隔符的列表中进行选择。 选择不可能出现在文本中的分隔符。 对于固定宽度的列，将忽略此值。<br /><br /> **{CR}{LF}** 。 列由回车符和换行符的组合分隔。<br /><br /> **{CR}** 。 列由回车符分隔。<br /><br /> **{LF}** 。 列由换行符分隔。<br /><br /> **分号 {;}** 。 列由分号分隔。<br /><br /> **冒号 {:}** 。 列由冒号分隔。<br /><br /> **逗号 {,}** 。 列由逗号分隔。<br /><br /> **制表符 {t}** 。 列由制表符分隔。<br /><br /> **竖线 {&#124;}** 。 列由竖线分隔。|  
|**DataPrecision**|指定数字数据的精度。 精度是指数字的位数。 有关详细信息，请参阅 [Integration Services 数据类型](../../integration-services/data-flow/integration-services-data-types.md)。|  
|**InputColumnWidth**|指定值以字节数进行存储；对于 Unicode 文件，该值将显示为字符数。 对于分隔列，将忽略此值。<br /><br /> **注意** ：在对象模型中，此属性的名称为 ColumnWidth。|  
  
 **新建**  
 单击“新建”添加一个新列。 默认情况下，单击 **“新建”** 按钮将会在列表末尾添加新列。 该按钮还包括以下选项，可以在下拉列表中选择。  
  
|值|说明|  
|-----------|-----------------|  
|**添加列**|在列表末尾添加新列。|  
|**在其前插入**|在所选列前面插入新列。|  
|**在其后插入**|在所选列后面插入新列。|  
  
 **删除**  
 选择一列，然后单击“删除”来删除该列。  
  
 **建议类型**  
 使用“提供列类型建议”对话框可以计算文件中的示例数据，并获取关于每列的数据类型和长度的建议。 有关详细信息，请参阅 [“提供列类型建议”对话框 UI 参考](../../integration-services/connection-manager/suggest-column-types-dialog-box-ui-reference.md)。  
## <a name="flat-file-connection-manager-editor-preview-page"></a>平面文件连接管理器编辑器（“预览”页）
  可以使用 **“平面文件连接管理器编辑器”** 对话框的 **“预览”** 节点，按表格格式查看源文件的内容。  
  
 若要了解有关平面文件连接管理器的详细信息，请参阅 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
### <a name="options"></a>选项  
 **连接管理器名称**  
 为工作流中的平面文件连接提供唯一的名称。 所提供的名称将在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中显示。  
  
 **说明**  
 描述此连接。 最好按照连接的用途对其进行说明，使包的说明一目了然，且更便于维护。  
  
 **要跳过的数据行数**  
 指定在平面文件的开头要跳过多少行。  
  
 **“刷新”**  
 通过单击“刷新”可以查看更改要跳过的行数后的效果。 只有在更改其他连接选项之后，此按钮才可见。  
  
 **预览行**  
 查看平面文件中的示例数据，这些数据已按所选的选项划分为列和行。  
 
