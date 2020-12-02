---
description: 候选键配置文件请求选项（数据事件探查任务）
title: 候选键配置文件请求选项（数据事件探查任务）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: 8632dbc4-4394-4dc7-b19c-f9adeb21ba52
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 499f56a0ce11bc68ad046035ff0a43d80ae4bfe7
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123592"
---
# <a name="candidate-key-profile-request-options-data-profiling-task"></a>候选键配置文件请求选项（数据事件探查任务）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“配置文件请求”** 页的 **“请求属性”** 窗格，为请求窗格中选定的 **“候选键配置文件请求”** 设置选项。 候选键配置文件报告某个列或列集对于选定的表是键还是近似键。 此配置文件还有助于标识数据中的问题，如潜在键列中存在重复值。  
  
> [!NOTE]  
>  本主题中介绍的选项显示在 **“数据事件探查任务编辑器”** 的 **“配置文件请求”** 页中。 有关此编辑器页的详细信息，请参阅[数据事件探查任务编辑器（“配置文件请求”页）](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)。  
  
 有关如何使用数据事件探查任务的详细信息，请参阅 [设置数据事件探查任务](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)。 有关如何使用数据配置文件查看器分析数据事件探查任务输出的详细信息，请参阅 [数据配置文件查看器](../../integration-services/control-flow/data-profile-viewer.md)。  
  
## <a name="understanding-the-selection-of-columns-for-the-keycolumns-property"></a>了解如何为 KeyColumns 属性选择列  
 每个 **“候选键配置文件请求”** 都会计算由单个列或多个列组成的单个候选键的键强度：  
  
-   如果仅在 **KeyColumns** 中选择了一个列，则该任务将计算那一个列的键强度。  
  
-   如果在 **KeyColumns** 中选择了多个列，则该任务将计算由所有选定列组成的组合键的键强度。  
  
-   如果在 **KeyColumns** 中选择通配符 **(\*)** ，则该任务将计算表或视图中的每个列的键强度。  
  
 例如，假定有一个包含列 A、B 和 C 的示例表，则您可以在 **KeyColumns** 中进行以下选择：  
  
-   在\*KeyColumns **中选择 (**) 和 列 C。 该任务将计算列 C 的键强度，然后计算组合候选键 (A, C) 和 (B, C) 的键强度。  
  
-   在\*KeyColumns\*中选择 ( **) 和 (**)。 该任务将计算单个列 A、B 和 C 的键强度，然后计算组合候选键 (A, B)、(A, C) 和 (B, C) 的键强度。  
  
> [!NOTE]  
>  如果选择 (*)，则此选项可能会导致大量计算并降低任务性能。 但是，如果任务找到满足键阈值的子集，则它不会再分析其他的组合。 例如，在上述示例表中，如果任务确定列 C 是一个键，则不会再继续分析组合候选键。  
  
## <a name="request-properties-options"></a>请求属性选项  
 对于 **“候选键配置文件请求”**， **“请求属性”** 窗格将显示以下选项组：  
  
-   **Data**，它包含 **TableOrView** 选项和 **KeyColumns** 选项  
  
-   **常规**  
  
-   **选项**  
  
### <a name="data-options"></a>Data 选项  
 **ConnectionManager**  
 选择使用 .NET Data Provider for [!INCLUDE[vstecado](../../includes/vstecado-md.md)] (SqlClient) 的现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接管理器连接到包含要进行事件探查的表或视图的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
 **TableOrView**  
 选择要进行事件探查的现有表或视图。  
  
 有关详细信息，请参阅本主题中的“TableorView 选项”部分。  
  
 **KeyColumns**  
 选择要进行事件探查的一个或多个现有列。 选择 **(\*)** 可对所有列进行事件探查。  
  
 有关详细信息，请参阅本主题中的“了解如何为 KeyColumns 属性选择列”和“KeyColumns 选项”部分。  
  
#### <a name="tableorview-options"></a>TableOrView 选项  
 **架构**  
 指定选定表所属的架构。 此选项是只读的。  
  
 **表**  
 显示所选表的名称。 此选项是只读的。  
  
#### <a name="keycolumns-options"></a>KeyColumns 选项  
 对于在 **KeyColumns** 中选定进行分析的每个列或针对 **(\*)** 选项，将显示以下选项。  
  
 有关详细信息，请参阅本主题前面的“了解如何为 KeyColumns 属性选择列”部分。  
  
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
 **ThresholdSetting**  
 此属性具有下表所列的选项。 此属性的默认值为 **Specified**。  
  
|值|说明|  
|-----------|-----------------|  
|**无**|未指定阈值。 不管键强度值如何，都会报告键强度。|  
|**Specified**|在 **KeyStrengthThreshold** 中指定了阈值。 仅当键强度大于阈值时，才会报告键强度。|  
|**Exact**|未指定阈值。 仅当选定列为完全匹配的键时，才报告键强度。|  
  
 **KeyStrengthThreshold**  
 指定阈值（使用介于 0 和 1 之间的值），仅当键强度大于该阈值时，才会报告键强度。 此属性的默认值为 0.95。 仅当选择了 **Specified** 作为 **KeyStrengthThresholdSetting** 时，才会启用该选项。  
  
 **MaxNumberOfViolations**  
 指定要在输出中报告的候选键冲突的最大数量。 此属性的默认值为 100。 只有在选择 **Exact** 作为 **KeyStrengthThresholdSetting** 时，才会禁用该选项。  
  
## <a name="see-also"></a>另请参阅  
 [数据事件探查任务编辑器（“常规”页）](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [单个表快速配置文件窗体（数据事件探查任务）](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
