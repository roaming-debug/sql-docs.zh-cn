---
description: 函数依赖关系配置文件请求选项（数据事件探查任务）
title: 函数依赖关系配置文件请求选项（数据事件探查任务）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: 6eb853aa-8016-490c-be4f-06ab8d7f5021
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1fa83e2b75860730f4e3d9b419a2ca8ca374ba31
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88393423"
---
# <a name="functional-dependency-profile-request-options-data-profiling-task"></a>函数依赖关系配置文件请求选项（数据事件探查任务）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“配置文件请求”** 页的 **“请求属性”** 窗格，为请求窗格中选定的 **“函数依赖关系配置文件请求”** 设置选项。 函数依赖关系配置文件报告一个列（依赖列）中的值对另一个列或另一个列集（决定列）中的值的依赖程度。 此配置文件还有助于标识数据中的问题，如值无效。 例如，您对邮政编码列与美国的州列之间的依赖关系进行事件探查， 在此配置文件中，同一邮政编码应始终对应相同的州，但配置文件发现依赖关系冲突。  
  
> [!NOTE]  
>  本主题中介绍的选项显示在 **“数据事件探查任务编辑器”** 的 **“配置文件请求”** 页中。 有关此编辑器页的详细信息，请参阅[数据事件探查任务编辑器（“配置文件请求”页）](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)。  
  
 有关如何使用数据事件探查任务的详细信息，请参阅 [设置数据事件探查任务](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)。 有关如何使用数据配置文件查看器分析数据事件探查任务输出的详细信息，请参阅 [数据配置文件查看器](../../integration-services/control-flow/data-profile-viewer.md)。  
  
## <a name="understanding-the-selection-of-determinant-and-dependent-columns"></a>了解如何选择决定列和依赖列  
 “函数依赖关系配置文件请求”计算决定端列或列集（在 **DeterminantColumns** 属性中指定）对依赖端列（在 **DependentColumn** 属性中指定）的值的决定程度。 例如，美国的州列在函数关系上应依赖于美国邮政编码列。 也就是说，如果邮政编码（决定列）为 98052，则州（依赖列）应始终为华盛顿。  
  
 可以在 **DeterminantColumns** 属性中指定一个列或列集作为决定端。 例如，假定有一个包含列 A、B 和 C 的示例表，则您可以在 **DeterminantColumns** 属性中进行以下选择：  
  
-   如果选择通配符 **(\*)** ，则数据事件探查任务会将每个列都当作依赖关系的决定端进行测试。  
  
-   如果选择通配符 **(\*)** 和其他一个或多个列，则数据事件探查任务会将每个列组合作为依赖关系的决定端进行测试。 例如，假定有一个包含列 A、B 和 C 的示例表。如果指定 **(\*)** 和列 C 作为 **DeterminantColumns** 属性的值，则数据事件探查任务会将组合 (A, C) 和 (B, C) 作为依赖关系的决定端进行测试。  
  
 可以在 **(\*)** 属性中指定一个列或 **DependentColumn** 属性中指定一个列或列集作为决定端。 如果选择 **(\*)** ，则数据事件探查任务将针对每个列测试决定端列或列集。  
  
> [!NOTE]  
>  如果选择 **(\*)** ，则此选项可能会导致大量计算并降低任务性能。 但是，如果任务找到满足函数依赖关系阈值的子集，则它不会再分析其他的组合。 例如，在上述示例表中，如果任务确定列 C 为决定列，则不会再继续分析组合候选列。  
  
## <a name="request-properties-options"></a>请求属性选项  
 对于 **“函数依赖关系配置文件请求”**， **“请求属性”** 窗格将显示以下选项组：  
  
-   **Data**，它包含 **DeterminantColumns** 选项和 **DependentColumn** 选项  
  
-   **常规**  
  
-   **选项**  
  
### <a name="data-options"></a>Data 选项  
 **ConnectionManager**  
 选择使用 .NET Data Provider for [!INCLUDE[vstecado](../../includes/vstecado-md.md)] (SqlClient) 的现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接管理器连接到包含要进行事件探查的表或视图的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
 **TableOrView**  
 选择要进行事件探查的现有表或视图。  
  
 **DeterminantColumns**  
 选择决定列或列集， 即：选择其值用于确定依赖列的值的列或列集。  
  
 有关详细信息，请参阅本主题中的“了解如何选择决定列和依赖列”以及“DeterminantColumns 和 DependentColumn 选项”部分。  
  
 **DependentColumn**  
 选择依赖列， 即：选择其值由决定端列或列集的值确定的列。  
  
 有关详细信息，请参阅本主题中的“了解如何选择决定列和依赖列”以及“DeterminantColumns 和 DependentColumn 选项”部分。  
  
#### <a name="determinantcolumns-and-dependentcolumn-options"></a>DeterminantColumns 和 DependentColumn 选项  
 对于在 **DeterminantColumns** 和 **DependentColumn** 中选定进行事件探查的每个列，将显示以下选项。  
  
 有关详细信息，请参阅本主题前面的“了解如何选择决定列和依赖列”部分。  
  
 **IsWildCard**  
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
  
|值|描述|  
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
 指定阈值设置。 此属性的默认值为 **Specified**。  
  
|值|说明|  
|-----------|-----------------|  
|**无**|不指定阈值。 不管函数依赖关系强度值如何，都会报告函数依赖关系强度。|  
|**Specified**|使用 **FDStrengthThreshold** 中指定的阈值。 仅当函数依赖关系强度大于阈值时，才会报告函数依赖关系强度。|  
|**Exact**|不指定阈值。 仅当选定列之间具有完全匹配的函数依赖关系时，才会报告函数依赖关系强度。|  
  
 **FDStrengthThreshold**  
 指定阈值（使用介于 0 和 1 之间的值），仅当函数依赖关系强度大于该阈值时，才会报告函数依赖关系强度。 此属性的默认值为 0.95。 仅当选择了 **Specified** 作为 **ThresholdSetting** 时，才会启用该选项。  
  
 **MaxNumberOfViolations**  
 指定要在输出中报告的函数依赖关系冲突的最大数量。 此属性的默认值为 100。 只有在选择 **Exact** 作为 **ThresholdSetting** 时，才会禁用该选项。  
  
## <a name="see-also"></a>另请参阅  
 [数据事件探查任务编辑器（“常规”页）](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [单个表快速配置文件窗体（数据事件探查任务）](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
