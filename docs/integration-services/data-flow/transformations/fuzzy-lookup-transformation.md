---
description: 模糊查找转换
title: 模糊查找转换 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.fuzzylookuptrans.f1
- sql13.dts.designer.fuzzylookuptransformation.referencetable.f1
- sql13.dts.designer.fuzzylookuptransformation.columns.f1
- sql13.dts.designer.fuzzylookuptransformation.advanced.f1
helpviewer_keywords:
- cleaning data
- comparing data
- token delimiters [Integration Services]
- temporary indexes [Integration Services]
- temporary tables [Integration Services]
- Fuzzy Lookup transformation
- reference tables [Integration Services]
- match similar data [Integration Services]
- replacing missing values
- correcting data [Integration Services]
- cache [Integration Services]
- standardizing data [Integration Services]
- lookups [Integration Services]
- confidence scores [Integration Services]
- fuzzy matches
- missing values replaced [Integration Services]
- similarity thresholds [Integration Services]
ms.assetid: 019db426-3de2-4ca9-8667-79fd9a47a068
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ab0e3826b20df90102bfc97d5d3730b4e83806dd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92195941"
---
# <a name="fuzzy-lookup-transformation"></a>模糊查找转换

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  模糊查找转换执行数据清理任务，例如标准化数据、更正数据以及提供丢失的值。  
  
> [!NOTE]  
>   有关模糊查找转换的详细信息（包括性能和内存限制），请参阅白皮书： [Fuzzy Lookup and Fuzzy Grouping in SQL Server Integration Services 2005](/previous-versions/sql/sql-server-2005/administrator/ms345128(v=sql.90))（SQL Server Integration Services 2005 中的模糊查找和模糊分组）。  
  
 模糊查找转换与查找转换之间的不同之处在于：它使用了模糊匹配。 查找转换使用同等联接在引用表中查找匹配的记录。 它返回带有至少一个匹配记录的记录，并且返回没有匹配记录的记录。 与此相比较，模糊查找转换使用模糊匹配返回引用表中一个或多个接近的匹配项。  
  
 在包数据流中，模糊查找转换通常在查找转换之后。 首先，查找转换尝试找到一个完全匹配的项。 如果未找到，模糊查找转换提供引用表中接近的匹配项。  
  
 这种转换需要访问包含用于清理和扩展输入数据的值的引用数据源。 引用数据源必须是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中的表。 输入列中的值与引用表中的值之间的匹配可以是完全匹配，也可以是模糊匹配。 但是，这种转换要求至少为模糊匹配配置一个列匹配。 如果希望仅使用完全匹配，请改用查找转换。  
  
 此转换有一个输入和一个输出。  
  
 在模糊匹配中，只能使用具有 **DT_WSTR** 和 **DT_STR** 数据类型的输入列。 完全匹配可以使用除 **DT_TEXT**、 **DT_NTEXT** 和 **DT_IMAGE** 之外的所有 DTS 数据类型。 有关详细信息，请参阅 [Integration Services 数据类型](../../../integration-services/data-flow/integration-services-data-types.md)。 参与输入和引用表之间联接的列必须具有兼容的数据类型。 例如，可以将具有 DTS DT_WSTR 数据类型的列联接到具有  nvarchar 数据类型的列，但是不能将具有 DT_WSTR 数据类型的列联接到具有 int 数据类型的列[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
 通过指定最大内存量、行比较算法以及对转换所用的索引和引用表进行缓存，可以自定义这种转换。  
  
 可以通过设置 MaxMemoryUsage 自定义属性来配置模糊查找转换所使用的内存数量。 可以指定内存量 (MB)；或使用值 0，让转换根据其需要和可用物理内存来使用动态内存量。 加载包时，可以通过属性表达式来更新 MaxMemoryUsage 自定义属性。 有关详细信息，请参阅 [Integration Services (SSIS) 表达式](../../../integration-services/expressions/integration-services-ssis-expressions.md)、[在包中使用属性表达式](../../../integration-services/expressions/use-property-expressions-in-packages.md)和[转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)。  
  
## <a name="controlling-fuzzy-matching-behavior"></a>控制模糊匹配的行为  
 模糊查找转换包含以下三项可自定义所执行的查找的功能：每个输入行可返回的最大匹配项数、标记分隔符和相似性阈值。  
  
 这种转换返回零个或多个匹配项，匹配项的最大数量为所指定的匹配项数。 指定最大匹配项数并不保证转换会返回最大数量的匹配项；它只保证转换最多返回该数量的匹配项。 如果将最大匹配项数设置为大于 1 的值，则对于每次查找，转换的输出可能包括多行，而且其中一些行可能是重复的。  
  
 该转换提供了一组用于对数据进行词汇切分的默认分隔符，但您可以添加适合自己数据需要的标记分隔符。 Delimiters 属性包含默认分隔符。 因为词汇切分在要进行比较的数据内定义相关单元，所以词汇切分是非常重要的操作。  
  
 相似性阈值可以在组件级和联接级设置。 仅当转换在输入中的列与引用表的列之间执行模糊匹配时，联接级相似性阈值才可用。 相似性范围是 0 到 1。 阈值越接近 1，则行和列必须越相似，才能被认定为重复。 通过在组件级和联接级设置 MinSimilarity 属性，可以指定相似性阈值。 为了满足在组件级指定的相似性，所有匹配项的所有行都必须具有大于或等于在组件级所指定的相似性阈值的相似性。 即，您不能在组件级指定非常接近的匹配项，除非行级或联接级的匹配项同样接近。  
  
 每个匹配项都包括一个相似性得分和一个置信度得分。 相似性得分是一个数学度量值，表示输入记录与模糊查找转换从引用表中返回的记录之间在结构上的相似程度。 置信度得分是一个可能性的度量值，表示特定值在从引用表中所发现的匹配项中成为最佳匹配项的可能程度。 分配给记录的置信度得分取决于所返回的其他匹配记录。 例如，匹配 *St.* 和 *Saint* 会返回一个较低的相似性得分，而无论其他匹配项是什么。 如果 *Saint* 是返回的唯一匹配项，则置信度得分会很高。 如果 *Saint* 和 *St.* 同时出现在引用表中，则 *St.* 的置信度较高，而 *Saint* 的置信度较低。 但是，高相似性可能并不意味着高置信度。 例如，如果正在查找值 *Chapter 4*，返回的结果 *Chapter 1*、 *Chapter 2* 和 *Chapter 3* 都具有很高的相似性得分，然而置信度得分却都较低，这是因为无法肯定哪个结果是最佳匹配项。  
  
 相似性得分由介于 0 和 1 之间的一个小数值表示，其中，相似性得分 1 表示输入列中的值与引用表中的值完全匹配。 置信度得分也是介于 0 和 1 之间的一个小数值，表示对匹配项的置信度。 如果没有发现有用的匹配项，则为该行分配相似性得分 0 和置信度得分 0，并且从引用表中复制的输出列会包含 Null 值。  
  
 有时，模糊查找可能在引用表中找不到相应的匹配项。 如果查找中使用的输入值是一个短单词，则可能会发生这种情况。 例如，如果该列或行中的任何其他列中都没有任何其他标记，则 *helo* 与引用表中的 *hello* 不匹配。  
  
 该转换的输出列包括标记为传递列的输入列、查找表中的选定列和以下其他列：  
  
-   **_Similarity**，此列描述输入列中的值和引用列中的值之间的相似性。  
  
-   **_Confidence**，此列描述匹配程度。  
  
 该转换使用与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库的连接来创建模糊匹配算法所使用的临时表。  
  
## <a name="running-the-fuzzy-lookup-transformation"></a>运行模糊查找转换  
 当包首次运行转换时，该转换将复制引用表，然后将具有整数数据类型的键添加到新表中，接着生成该键列的索引。 随后，该转换生成引用表的副本的索引，该索引称为匹配索引。 匹配索引存储转换输入列中的值的词汇切分结果，接着该转换在查找操作中使用这些标记。 匹配索引是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中的一个表。  
  
 当包再次运行时，该转换可以使用现有的匹配索引，也可以创建新的索引。 如果引用表是静态的，对于数据清理的重复会话，包可以避免开销可能很大的重新生成索引的过程。 您可以选择使用现有索引，该索引是在包首次运行时创建的。 如果多个模糊查找转换使用同一引用表，则它们可以使用同一索引。 若要重用该索引，查找操作必须是相同的，而且查找必须使用相同的列。 您可以命名该索引，然后选择到保存该索引的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库的连接。  
  
 如果该转换保存了匹配索引，则匹配索引将得到自动维护。 这意味着，每次更新引用表中的记录时，也会更新匹配索引。 维护匹配索引可以节省处理时间，因为无需在包运行时重新生成索引。 您可以指定该转换如何管理匹配索引。  
  
 下表介绍了匹配索引选项：  
  
|选项|说明|  
|------------|-----------------|  
|**GenerateAndMaintainNewIndex**|创建一个新的索引，保存它，然后对其进行维护。 该转换在引用表上安装触发器，使引用表和索引表同步。|  
|**GenerateAndPersistNewIndex**|创建一个新的索引，保存它，但不对其进行维护。|  
|**GenerateNewIndex**|创建一个新的索引，但不保存它。|  
|**ReuseExistingIndex**|重用现有索引。|  
  
### <a name="maintenance-of-the-match-index-table"></a>维护匹配索引表  
 **GenerateAndMaintainNewIndex** 选项在引用表上安装触发器，以保持匹配索引表和引用表同步。 如果必须删除已安装的触发器，则必须运行 **sp_FuzzyLookupTableMaintenanceUnInstall** 存储过程，然后将 MatchIndexName 属性中指定的名称提供为输入参数值。  
  
 在运行 **sp_FuzzyLookupTableMaintenanceUnInstall** 存储过程之前，不应该删除维护的匹配索引表。 如果删除了匹配索引表，引用表上的触发器将无法正确执行。 在手动删除引用表上的触发器之前，对引用表进行的所有后续更新都将失败。  
  
 SQL TRUNCATE TABLE 命令不调用 DELETE 触发器。 如果对引用表使用 TRUNCATE TABLE 命令，则引用表和匹配索引表将无法再同步，模糊查找转换将失败。 尽管维护匹配索引表的触发器安装在引用表上，您也应该使用 SQL DELETE 命令，而不是使用 TRUNCATE TABLE 命令。  
  
> [!NOTE]  
>  如果在 **“模糊查找转换编辑器”** 的 **“引用表”** 选项卡中选择 **“维护存储的索引”**，则转换将使用托管存储过程维护索引。 这些托管存储过程使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的公共语言运行时 (CLR) 集成功能。 默认情况下，不启用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的 CLR 集成。 若要使用 **“维护存储的索引”** 功能，必须启用 CLR 集成。 有关详细信息，请参阅 [Enabling CLR Integration](../../../relational-databases/clr-integration/clr-integration-enabling.md)。  
>   
>  由于 **“维护存储的索引”** 选项需要 CLR 集成，所以只有在选择已启用 CLR 集成的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上的引用表时，此功能才能发挥作用。  
  
## <a name="row-comparison"></a>行比较  
 配置模糊查找转换时，可以指定该转换在定位引用表中的匹配记录时所用的比较算法。 如果将 Exhaustive 属性设置为 **True**，则转换会将输入中的每一行与引用表中的每一行相比较。 这种比较算法可以生成更准确的结果，但是，除非引用表中的行数较少，否则很有可能使转换的执行速度变得很慢。 如果 Exhaustive 属性设置为 **True**，则整个引用表都会加载到内存中。 为了避免性能问题，最好只在包的开发过程中将 Exhaustive 属性设置为 **True** 。  
  
 如果将 Exhaustive 属性设置为 **False**，则模糊查找转换只返回与输入记录一样至少有一个索引令牌或子字符串（该子字符串称为 q-gram）的匹配项。 若要最大程度提高查找效率，请以模糊查找转换查找匹配项时所用的倒排索引结构仅对表内每行中的一个令牌子集建立索引。 当输入数据集很小时，可以将 Exhaustive 设置为 **True** ，以避免遗漏索引表中不存在其公共令牌的匹配项。  
  
## <a name="caching-of-indexes-and-reference-tables"></a>缓存索引和引用表  
 在配置模糊查找转换时，可以指定转换在开始执行其工作之前，是否将部分索引和引用表缓存到内存中。 如果 WarmCaches 属性设置为 **True**，则索引和引用表将加载到内存中。 当输入具有很多行时，将 WarmCaches 属性设置为 **True** 可以提高转换的性能。 当输入行数很小时，将 WarmCaches 属性设置为 **False** 可以使重用大型索引的速度加快。  
  
## <a name="temporary-tables-and-indexes"></a>临时表和索引  
 在运行时，模糊查找转换会在该转换所连接到的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中创建临时对象，例如表和索引。 这些临时表和索引的大小与引用表中的行数和标记数以及模糊查找转换所创建的标记数成比例；因此，它们有可能会占用相当大的磁盘空间。 该转换也会查询这些临时表。 因此，应该考虑将模糊查找转换连接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库的非生产实例中，在生产服务器只有有限的可用磁盘空间时，尤其应该如此。  
  
 如果此转换所使用的表和索引位于本地计算机，则此转换的性能可能会提高。 如果模糊查找转换使用的引用表位于生产服务器上，您应该考虑将该表复制到非生产服务器，并将模糊查找转换配置为访问该副本。 这样做可以防止查找查询占用生产服务器上的资源。 此外，如果模糊查找转换维护匹配索引（即如果 MatchIndexOptionsis 设置为“GenerateAndMaintainNewIndex”），则转换可以在执行数据清理操作的过程中锁定引用表，以防止其他用户和应用程序访问该表。  
  
## <a name="configuring-the-fuzzy-lookup-transformation"></a>配置模糊查找转换  
 可以通过 [!INCLUDE[ssIS](../../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可以在 **“高级编辑器”** 对话框中或以编程方式设置的属性的详细信息，请单击下列主题之一：  
  
-   [Common Properties](../set-the-properties-of-a-data-flow-component.md)  
  
-   [转换自定义属性](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 有关如何设置数据流组件属性的详细信息，请参阅 [设置数据流组件属性](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)。  
  
## <a name="fuzzy-lookup-transformation-editor-reference-table-tab"></a>模糊查找转换编辑器（“引用表”选项卡）
  使用 **“模糊查找转换编辑器”** 对话框的 **“引用表”** 选项卡可以指定用于查找的源表和索引。 引用数据源必须是 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据库中的表。  
  
> [!NOTE]  
>  模糊查找转换将创建引用表的工作副本。 下面描述的索引是通过使用特殊的表对此工作表创建的，它们不是普通的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 索引。 除非选择了 **“维护存储的索引”**，否则转换不会修改现有的源表。 在这种情况下，转换将对引用表创建一个触发器，此触发器将基于对引用表所做的更改来更新工作表和查找索引表。  
  
> [!NOTE]  
>  模糊查找转换的 **Exhaustive** 和 **MaxMemoryUsage** 属性未在 **“模糊查找转换编辑器”** 中提供，但可以使用 **“高级编辑器”** 进行设置。 此外，大于 100 的 **MaxOutputMatchesPerInput** 值只能在 **“高级编辑器”** 中指定。 有关这些属性的详细信息，请参阅 [Transformation Custom Properties](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)的“模糊查找转换”部分。  
  
### <a name="options"></a>选项  
 **“无缓存”**  
 从列表中选择现有的 OLE DB 连接管理器，或通过单击“新建”创建一个新连接。  
  
 **新建**  
 通过使用“配置 OLE DB 连接管理器”  对话框创建新的连接。  
  
 **生成新索引**  
 指定转换应创建新的索引以用于查找。  
  
 **引用表的名称**  
 选择要用作引用（查找）表的现有表。  
  
 **存储新索引**  
 如果希望保存新的查找索引，请选择此选项。  
  
 **新索引名称**  
 如果已选择保存新的查找索引，请为其键入描述性名称。  
  
 **“维护存储的索引”**  
 如果已选择保存新的查找索引，请指定是否还希望 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 维护该索引。  
  
> [!NOTE]  
>  如果在 **“模糊查找转换编辑器”** 的 **“引用表”** 选项卡中选择 **“维护存储的索引”**，则转换将使用托管存储过程维护索引。 这些托管存储过程使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中的公共语言运行时 (CLR) 集成功能。 默认情况下，不启用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的 CLR 集成。 若要使用 **“维护存储的索引”** 功能，必须启用 CLR 集成。 有关详细信息，请参阅 [Enabling CLR Integration](../../../relational-databases/clr-integration/clr-integration-enabling.md)。  
>   
>  由于 **“维护存储的索引”** 选项需要 CLR 集成，所以只有在选择已启用 CLR 集成的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例上的引用表时，此功能才能发挥作用。  
  
 **使用现有索引**  
 指定转换应使用现有索引来执行查找。  
  
 **现有索引的名称**  
 从列表中选择以前创建的查找索引。  
  
## <a name="fuzzy-lookup-transformation-editor-columns-tab"></a>模糊查找转换编辑器（“列”选项卡）
  可以使用 **“模糊查找转换编辑器”** 对话框的 **“列”** 选项卡，为输入和输出列设置属性。  
  
### <a name="options"></a>选项  
 **可用输入列**  
 拖动输入列以将其连接到可用查找列。 这些列必须具有所支持的相互匹配的数据类型。 选择一个映射行，再右键单击可在 [创建关系](../../../integration-services/data-flow/transformations/create-relationships.md) 对话框中编辑该映射。  
  
 **名称**  
 查看可用输入列的名称。  
  
 **传递**  
 指定是否在转换的输出中包含输入列。  
  
 **可用查找列**  
 使用这些复选框可以选择要对其执行模糊查找操作的列。  
  
 **查找列**  
 从引用表的可用列的列表中选择查找列。 通过选中 **“可用查找列”** 表中的相应复选框即可选择查找列。 选择 **“可用查找列”** 表中的某个列将创建一个输出列，其中包含返回的对应于每个匹配行的引用表列值。  
  
 **输出别名**  
 为每个查找列的输出键入一个别名。 默认值为查找列的名称，并追加一个数字索引值；不过，您也可以任选一个唯一的描述性名称。  
  
## <a name="fuzzy-lookup-transformation-editor-advanced-tab"></a>模糊查找转换编辑器（“高级”选项卡）
  可以使用 **“模糊查找转换编辑器”** 对话框的 **“高级”** 选项卡设置模糊查找的参数。  
  
### <a name="options"></a>选项  
 **每次查找输出的最大匹配数**  
 指定为每个输入行返回的最大匹配转换数。 默认值为 **1**。  
  
 **相似性阈值**  
 使用滑块在组件级别设置相似性阈值。 该值越接近 1，查找值与源值的相似性必须越接近，才能视为匹配。 由于需要考虑的候选记录更少，因此增加阈值可以提高匹配的速度。  
  
 **标记分隔符**  
 指定转换用来对列值进行词汇切分的分隔符。  
  
## <a name="see-also"></a>另请参阅  
 [查找转换](../../../integration-services/data-flow/transformations/lookup-transformation.md)   
 [Fuzzy Grouping Transformation](../../../integration-services/data-flow/transformations/fuzzy-grouping-transformation.md)   
 [Integration Services 转换](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
