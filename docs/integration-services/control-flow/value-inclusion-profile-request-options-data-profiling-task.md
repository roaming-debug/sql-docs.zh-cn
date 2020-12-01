---
description: 值包含配置文件请求选项（数据事件探查任务）
title: 值包含配置文件请求选项（数据事件探查任务）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: ca94da82-a4c9-4e87-9cba-c2d85bd31f01
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 2c30a5e35a3c3e5b8e127a317e6d44880dee2e7f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430929"
---
# <a name="value-inclusion-profile-request-options-data-profiling-task"></a>值包含配置文件请求选项（数据事件探查任务）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“配置文件请求”** 页的 **“请求属性”** 窗格，为请求窗格中选定的 **“值包含配置文件请求”** 设置选项。 值包含配置文件计算两列或列集之间的值的重叠 因此，它可以确定一个列或列集是否适于用作两个选定表之间的外健。 此配置文件还有助于标识数据中的问题，如值无效。 例如，使用值包含配置文件对 Sales 表中的 ProductID 列进行事件探查。 在配置文件中发现，该列所包含的某些值不能在 Products 表的 ProductID 列中找到。  
  
> [!NOTE]  
>  本主题中介绍的选项显示在 **“数据事件探查任务编辑器”** 的 **“配置文件请求”** 页中。 有关此编辑器页的详细信息，请参阅[数据事件探查任务编辑器（“配置文件请求”页）](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)。  
  
 有关如何使用数据事件探查任务的详细信息，请参阅 [设置数据事件探查任务](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)。 有关如何使用数据配置文件查看器分析数据事件探查任务输出的详细信息，请参阅 [数据配置文件查看器](../../integration-services/control-flow/data-profile-viewer.md)。  
  
## <a name="understanding-the-selection-of-columns-for-the-inclusioncolumns-property"></a>了解 InclusionColumns 属性列的选择。  
 **“值包含配置文件请求”** 计算子集中的所有值是否在超集中显示。 超集通常是一个查找表或引用表。 例如，地址表中的州列是子集表。 此列中以两个字符表示的每个州代码也应在超集表（美国邮政服务州代码表）中找到。  
  
 当将通配符 (*) 用作子集列或超集列的值时，数据事件探查任务将该端的每个列与另一端上指定的列进行比较。  
  
> [!NOTE]  
>  如果选择 (*)，则此选项可能会导致大量计算并降低任务性能。  
  
## <a name="understanding-the-threshold-settings"></a>了解阈值设置  
 可以使用两个不同的阈值设置来优化值包含配置文件请求的输出。  
  
 当为 **InclusionThresholdSetting** 指定非 **None** 的值时，该配置文件仅在以下条件之一报告超集中子集的包含强度：  
  
-   当包含强度超出 **InclusionStrengthThreshold** 中指定的阈值时。  
  
-   当包含强度的值为 1.0，而 **InclusionStrengthThreshold** 设置为 **Exact** 时。  
  
 由于为非唯一值，因此可以筛选出超集列不是超集表的适当的键的组合，从而更好地优化输出。 当为 **SupersetColumnsKeyThresholdSetting** 指定非 **None** 的值时，配置文件仅在以下条件之一报告超集中子集的包含强度：  
  
-   当超集列作为超集表中的键的适用性超出了 **SupersetColumnsKeyThreshold** 中指定的阈值时。  
  
-   当包含强度的值为 1.0，而 **SupersetColumnsKeyThreshold** 设置为 **Exact** 时。  
  
## <a name="request-properties-options"></a>请求属性选项  
 对于 **“值包含配置文件请求”**， **“请求属性”** 窗格显示下面的选项组：  
  
-   **Data**，它包含 **SubsetTableOrView**、 **SupersetTableOrView** 和 **InclusionColumns** 选项  
  
-   **常规**  
  
-   **选项**  
  
### <a name="data-options"></a>Data 选项  
 **ConnectionManager**  
 选择使用 .NET Data Provider for [!INCLUDE[vstecado](../../includes/vstecado-md.md)] (SqlClient) 的现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接管理器连接到包含要进行事件探查的表或视图的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
 **SubsetTableOrView**  
 选择要进行事件探查的现有表或视图。  
  
 有关详细信息，请参阅本主题中的“SubsetTableOrView 和 SupersetTableOrView 选项”部分。  
  
 **SupersetTableOrView**  
 选择要进行事件探查的现有表或视图。  
  
 有关详细信息，请参阅本主题中的“SubsetTableOrView 和 SupersetTableOrView 选项”部分。  
  
 **InclusionColumns**  
 从子集和超集表选择列或列集。  
  
 有关详细信息，请参阅本主题中的“了解 InclusionColumns 属性列的选择”和“InclusionColumns 选项”部分。  
  
#### <a name="subsettableorview-and-supersettableorview-options"></a>SubsetTableOrView 和 SupersetTableOrView 选项  
 **架构**  
 指定选定表所属的架构。 此选项是只读的。  
  
 **TableOrView**  
 显示所选表的名称。 此选项是只读的。  
  
#### <a name="inclusioncolumns-options"></a>InclusionColumns 选项  
 将针对在 **InclusionColumns** 中进行探查的每个选定列集显示以下选项：  
  
 有关详细信息，请参阅本主题前面的“了解 InclusionColumns 属性列的选择”部分。  
  
 **IsWildcard**  
 指定是否已选择通配符 **(\*)** 。 如果已选择 **(\*)** 来对所有列进行事件探查，则此选项设置为 **True**。 如果您已选择要对单独列进行事件探查，则为 **False** 。 此选项是只读的。  
  
 **ColumnName**  
 显示所选列的名称。 如果已选择 **(\*)** 来对所有列进行事件探查，则此选项空白。 此选项是只读的。  
  
 **StringCompareOptions**  
 选择用于比较字符串值的选项。 此属性具有下表所列的选项。 此选项的默认值为 **Default**。  
  
> [!NOTE]  
>  如果使用通配符 **(\*)** 作为 **ColumnName**，则 **CompareOptions** 将变为只读并设置为 **Default** 设置。  
  
|值|说明|  
|-----------|-----------------|  
|**Default**|根据源表中列的排序规则对数据进行排序和比较。|  
|**BinarySort**|根据为每个字符所定义的位模式对数据进行排序和比较。 二进制排序顺序既区分大小写，也区分重音。 二进制排序顺序的速度也最快。|  
|**DictionarySort**|根据关联语言或文字字典中定义的排序和比较规则对数据进行排序和比较。|  
  
 如果选择 **DictionarySort**，还可以选择下表中列出的任意选项组合。 默认情况下，不会选择这些附加选项中的任何一个。  
  
|值|说明|  
|-----------|-----------------|  
|**IgnoreCase**|指定比较是否区分大小写字母。 如果设置了此选项，字符串比较会忽略大小写。 例如，"ABC" 和 "abc" 没有区别。|  
|**IgnoreNonSpace**|指定比较是否区分空格字符和标注字符。 如果设置了此选项，则比较会忽略标注字符。 例如，“Ã¥”等于“a”。|  
|**IgnoreKanaType**|指定比较是否区分日语的两种假名字符类型：平假名和片假名。 如果设置了此选项，字符串比较会忽略假名类型。|  
|**IgnoreWidth**|指定比较是否区分字符的单字节形式和该字符的双字节形式。 如果设置了此选项，字符串比较将把同一字符的单字节形式和双字节形式视为相同。|  
  
### <a name="general-options"></a>常规选项  
 **RequestID**  
 键入一个标识此配置文件请求的描述性名称。 通常无需更改自动生成的值。  
  
### <a name="options"></a>选项  
 **None**  
 选择阈值设置来优化配置文件的输出。 此属性的默认值为 **Specified**。 有关详细信息，请参阅本主题前面的“了解阈值设置”部分。  
  
|值|说明|  
|-----------|-----------------|  
|**无**|不指定阈值。 不管键强度值如何，都会报告键强度。|  
|**Specified**|使用 **InclusionStrengthThreshold** 中指定的阈值。 只有在包含强度大于阈值时才报告该包含强度。|  
|**Exact**|不指定阈值。 只有在子集值完全包含在超集值中时才报告包含强度。|  
  
 **InclusionStrengthThreshold**  
 使用 0 到 1 之间的值指定阈值，高于此阈值将报告包含强度。 此属性的默认值为 0.95。 只有在选择 **Specified** 作为 **InclusionThresholdSetting** 的值时，才启用此选项。  
  
 有关详细信息，请参阅本主题前面的“了解阈值设置”部分。  
  
 **None**  
 指定超集阈值。 此属性的默认值为 **Specified**。 有关详细信息，请参阅本主题前面的“了解阈值设置”部分。  
  
|值|说明|  
|-----------|-----------------|  
|**无**|不指定阈值。 报告包含强度时不考虑超集列的键强度。|  
|**Specified**|使用 **SupersetColumnsKeyThreshold** 中指定的阈值。 只有在超集列的键强度大于阈值时才报告包含强度。|  
|**Exact**|不指定阈值。 只有在超集列为超集表中的确切键时才报告包含强度。|  
  
 **SupersetColumnsKeyThreshold**  
 使用 0 到 1 之间的值指定阈值，高于此阈值将报告包含强度。 此属性的默认值为 0.95。 只有在选择 **Specified** 作为 **SupersetColumnsKeyThresholdSetting** 的值时，才启用此选项。  
  
 有关详细信息，请参阅本主题前面的“了解阈值设置”部分。  
  
 **MaxNumberOfViolations**  
 指定要在输出中报告的最大包含冲突数。 此属性的默认值为 100。 只有在选择 **Exact** 作为 **InclusionThresholdSetting** 时，才会禁用该选项。  
  
## <a name="see-also"></a>另请参阅  
 [数据事件探查任务编辑器（“常规”页）](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [单个表快速配置文件窗体（数据事件探查任务）](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
