---
description: 模糊分组转换
title: 模糊分组转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.fuzzygroupingtrans.f1
- sql13.dts.designer.fuzzygroupingtransformation.connection.f1
- sql13.dts.designer.fuzzygroupingtransformation.columns.f1
- sql13.dts.designer.fuzzygroupingtransformation.advanced.f1
helpviewer_keywords:
- cleaning data
- comparing data
- token delimiters [Integration Services]
- temporary indexes [Integration Services]
- Fuzzy Grouping transformation
- temporary tables [Integration Services]
- grouping data
- standardizing data [Integration Services]
- columns [Integration Services], standardizing
- similarity thresholds [Integration Services]
- data cleaning [Integration Services]
- duplicate data [Integration Services]
ms.assetid: e43f17bd-9d13-4a8f-9f29-cce44cac1025
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6ce27ef50da9b1c72bc3b620dda61e6c1c925d14
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195948"
---
# <a name="fuzzy-grouping-transformation"></a>模糊分组转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  模糊分组转换执行数据清理任务，它首先查找可能重复的数据行，然后选择要在对数据进行标准化的过程中使用的规范数据行。  
  
> [!NOTE]  
>   有关模糊分组转换（包括性能和内存限制）的详细信息，请参阅白皮书 [Fuzzy Lookup and Fuzzy Grouping in SQL Server Integration Services 2005](/previous-versions/sql/sql-server-2005/administrator/ms345128(v=sql.90))（SQL Server Integration Services 2005 中的模糊查找和模糊分组）。  
  
 模糊分组转换要求与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的实例建立连接，以创建该转换算法完成工作所需的临时 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表。 该连接必须解析为有权在数据库中创建表的用户。  
  
 若要配置该转换，必须选择要在确定重复项时使用的输入列，而且必须为每列选择匹配类型（模糊匹配或完全匹配）。 完全匹配保证只对该列中具有相同值的行进行分组。 完全匹配可以应用到除 DT_TEXT、DT_NTEXT 和 DT_IMAGE 之外的任何 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 数据类型的列。 模糊匹配对具有相似值的行进行分组。 近似匹配数据的方法基于用户指定的相似性得分。 在模糊匹配中，只能使用具有 DT_WSTR 和 DT_STR 数据类型的列。 有关详细信息，请参阅 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)。  
  
 该转换输出包括所有输入列、一个或多个具有标准化数据的列以及一个包含相似性得分的列。 该得分是一个介于 0 和 1 之间的小数值。 规范行的得分为 1。 模糊组中其他行具有的得分指明该行与规范行的匹配程度。 得分越接近 1，行与规范行的匹配程度越高。 如果模糊组包括与规范行完全匹配的行，则这些行的得分也为 1。 该转换不删除这些重复的行；它通过创建一个将规范行与相似行关联的键对重复行进行分组。  
  
 该转换为每个输入行产生一个输出行，同时还包括下面的附加列：  
  
-   **_key_in**，唯一标识每行的列。  
  
-   **_key_out**，标识一组重复行的列。 在规范数据行中， **_key_out** 列的值就是 **_key_in** 列。 **_key_out** 中的值相同的行属于同一个组。 组的 **_key_out** 值对应于规范数据行中 **_key_in** 值。  
  
-   **_score**，一个介于 0 和 1 之间的值，指示输入行与规范行之间的相似性。  
  
 这些都是默认列名，您可以将模糊分组转换配置为使用其他名称。 输出还提供了参与模糊分组的每列的相似性得分。  
  
 模糊分组转换包括两个用于自定义所执行的分组的功能：标记分隔符和相似性阈值。 该转换提供了一组用于对数据进行词汇切分的默认分隔符，但您可以添加新的分隔符以改善您数据的词汇切分。  
  
 相似性阈值指示该转换标识重复项的严格程度。 相似性阈值可以在组件级和列级设置。 列级相似性阈值只适用于执行模糊匹配的列。 相似性范围是 0 到 1。 阈值越接近 1，则行和列必须越相似，才能被认定为重复。 通过在组件级和列级别设置 MinSimilarity 属性，可以指定行和列之间的相似性阈值。 为了满足在组件级指定的相似性，所有列的所有行都必须具有大于或等于在组件级所指定的相似性阈值的相似性。  
  
 模糊分组转换计算相似性的内部度量值，如果行的相似性低于 MinSimilarity 中指定的值，则不对这些行进行分组。  
  
 若要确定适合您数据的相似性阈值，您可能需要使用不同的最低相似性阈值多次应用模糊分组转换。 在运行时，转换输出中的得分列包含组中每行的相似性得分。 您可以使用这些值来确定适合您数据的相似性阈值。 如果要增加相似性，应该将 MinSimilarity 设置为一个大于得分列中所示值的值。  
  
 您可以通过设置模糊分组转换输入中的列属性来自定义该转换所执行的分组。 例如，FuzzyComparisonFlags 属性指定该转换如何比较列中的字符串数据，ExactFuzzy 属性指定该转换是执行模糊匹配还是执行完全匹配。  
  
 可以通过设置 MaxMemoryUsage 自定义属性来配置模糊分组转换所使用的内存数量。 可以指定 MB 数，或使用值 0 以允许转换基于其需要和可用物理内存而使用动态内存数量。 加载包时，可以通过属性表达式来更新 MaxMemoryUsage 自定义属性。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)、[在包中使用属性表达式](../../../integration-services/expressions/use-property-expressions-in-packages.md)和[转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)。  
  
 此转换有一个输入和一个输出。 它不支持错误输出。  
  
## <a name="row-comparison"></a>行比较  
 在配置模糊分组转换时，可以指定该转换比较转换输入中的行所用的算法。 如果将 Exhaustive 属性设置为 **true**，则该转换会将输入中的每一行与输入中所有其他行一一进行比较。 这种比较算法可以生成更准确的结果，但是除非输入中的行数很少，否则很有可能使转换的执行速度变得很慢。 为了避免性能问题，最好在包的开发过程中将 Exhaustive 属性设置为 **true** 。  
  
## <a name="temporary-tables-and-indexes"></a>临时表和索引  
 在运行时，模糊分组转换会在该转换所连接到的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中创建临时对象，例如表和索引，这些表和索引可能会非常大。 表和索引的大小与转换输入中的行数和模糊分组转换所创建的标记数成比例。  
  
 该转换也会查询这些临时表。 因此，应该考虑将模糊分组转换连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]的非生产实例中，在生产服务器只有有限的可用磁盘空间时，尤其应该如此。  
  
 如果此转换所使用的表和索引位于本地计算机，则此转换的性能可能会提高。  
  
## <a name="configuration-of-the-fuzzy-grouping-transformation"></a>配置模糊分组转换  
 可以通过 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何设置此任务的属性的详细信息，请单击下列主题之一：  
  
-   [使用模糊分组转换标识相似数据行](../../../integration-services/data-flow/transformations/identify-similar-data-rows-by-using-the-fuzzy-grouping-transformation.md)  
  
-   [设置数据流组件的属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="fuzzy-grouping-transformation-editor-connection-manager-tab"></a>模糊分组转换编辑器（“连接管理器”选项卡）
  使用 **“模糊分组转换编辑器”** 对话框的 **“连接管理器”** 选项卡可以选择现有连接或创建新的连接。  
  
> [!NOTE]  
>  连接指定的服务器必须正在运行 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 模糊分组转换会在 tempdb 中创建临时数据对象，这些对象的大小可能与为此转换输入的所有内容完全一致。 在执行转换时，该转换将对这些临时对象发出服务器查询。 这会影响服务器的总体性能。  
  
### <a name="options"></a>选项  
 **“无缓存”**  
 使用列表框选择现有的 OLE DB 连接管理器，或使用“新建”按钮创建新的连接。  
  
 **新建**  
 通过使用“配置 OLE DB 连接管理器”  对话框创建新的连接。  
  
## <a name="fuzzy-grouping-transformation-editor-columns-tab"></a>模糊分组转换编辑器（“列”选项卡）
  可以使用 **“模糊分组转换编辑器”** 对话框的 **“列”** 选项卡，指定用于对带有重复值的行进行分组的列。  
  
### <a name="options"></a>选项  
 **可用输入列**  
 从此列表中选择用于对带有重复值的行进行分组的输入列。  
  
 **名称**  
 查看可用输入列的名称。  
  
 **传递**  
 选择是否在转换的输出中包含输入列。 用于分组的所有列将自动复制到输出中。 通过选中此列可以包含其他列。  
  
 **输入列**  
 选择先前在“可用输入列”列表中选中的一个输入列。  
  
 **输出别名**  
 为相应的输出列输入一个描述性名称。 默认情况下，输出列名称与输入列名称相同。  
  
 **组输出别名**  
 为包含分组重复项的规范值的列输入一个描述性名称。 此输出列的默认名称是在输入列名称后面追加 _clean。  
  
 **匹配类型**  
 选择模糊匹配或完全匹配。 在指定了模糊匹配类型的所有列中，如果某些行足够相似，则会将这些行视为重复。 如果还对某些列指定了完全匹配，则只会将在完全匹配列中包含相同值的行视为可能重复。 因此，如果知道特定列中没有错误或不存在不一致的情况，则可以对该列指定完全匹配以提高其他列模糊匹配的准确性。  
  
 **最低相似性**  
 使用滑块在联接级别设置相似性阈值。 该值越接近 1，查找值与源值的相似性必须越接近，才能视为匹配。 由于需要考虑的候选记录更少，因此增加阈值可以提高匹配的速度。  
  
 **相似性输出别名**  
 为包含所选联接相似性得分的新输出列指定名称。 如果将该值保留为空，将不会创建输出列。  
  
 **数字**  
 指定比较列数据时前导数字和尾随数字的重要性。 例如，如果前导数字重要，则“123 Main Street”将不会与“456 Main Street”分组在一起。  
  
|值|描述|  
|-----------|-----------------|  
|**两者均未选中**|前导数字和尾随数字都不重要。|  
|**Leading**|只有前导数字重要。|  
|**Trailing**|只有尾随数字重要。|  
|**LeadingAndTrailing**|前导数字和尾随数字都重要。|  
  
 **比较标志**  
 有关字符串比较选项的信息，请参阅 [比较字符串数据](../../../integration-services/data-flow/comparing-string-data.md)。  
  
## <a name="fuzzy-grouping-transformation-editor-advanced-tab"></a>模糊分组转换编辑器（“高级”选项卡）
  可以使用 **“模糊分组转换编辑器”** 对话框的 **“高级”** 选项卡，指定输入和输出列，设置相似性阈值以及定义分隔符。  
  
> [!NOTE]  
>  模糊分组转换的 **Exhaustive** 和 **MaxMemoryUsage** 属性未在 **“模糊分组转换编辑器”** 中提供，但可以使用 **“高级编辑器”** 进行设置。 有关这些属性的详细信息，请参阅 [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)的“模糊分组转换”部分。  
  
### <a name="options"></a>选项  
 **输入键列名**  
 指定包含每个输入行的唯一表示符的输出列名称。 **_key_in** 列包含的值可唯一标识每个行。  
  
 **输出键列名**  
 对于一组重复的行，指定包含其规范行的唯一标识符的输出列名称。 **_key_out** 列对应于规范数据行的 **_key_in** 值。  
  
 **相似性计分列名**  
 指定包含相似性得分的列的名称。 相似性得分是介于 0 和 1 之间的值，用于指示输入行与规范行的相似性。 得分越接近 1，行与规范行的匹配程度越高。  
  
 **相似性阈值**  
 使用滑块设置相似性阈值。 阈值越接近于 1，则行必须越近似，才能被认定为重复。 增大阈值可以提高匹配的速度，因为需要考虑的候选记录更少。  
  
 **标记分隔符**  
 转换提供了一组默认的分隔符用于对数据进行词汇切分，但是您可以根据需要通过编辑列表来添加或删除分隔符。  
  
## <a name="see-also"></a>另请参阅  
 [模糊查找转换](../../../integration-services/data-flow/transformations/fuzzy-lookup-transformation.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
