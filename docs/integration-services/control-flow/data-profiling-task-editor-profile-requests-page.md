---
description: 数据事件探查任务编辑器（“配置文件请求”页）
title: 数据事件探查任务编辑器（“配置文件请求”页）| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataprofilingtask.profilerequests.f1
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: c72acb3d-380e-436e-8041-ed364eddfabd
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3365f6540a95639f2f6d4c607bfdee094c550c1c
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123529"
---
# <a name="data-profiling-task-editor-profile-requests-page"></a>数据事件探查任务编辑器（“配置文件请求”页）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“数据事件探查任务编辑器”** 的 **“配置文件请求”** 页，选择和配置需要计算的配置文件。 在单个数据事件探查任务中，可以为多列或多个表或视图中的列组合计算多个配置文件。  
  
 有关如何使用数据事件探查任务的详细信息，请参阅 [设置数据事件探查任务](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)。 有关如何使用数据配置文件查看器分析数据事件探查任务输出的详细信息，请参阅 [数据配置文件查看器](../../integration-services/control-flow/data-profile-viewer.md)。  
  
 **打开数据事件探查任务编辑器的“配置文件请求”页面**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，打开具有该数据事件探查任务的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包。  
  
2.  在“控制流”  选项卡上，双击数据事件探查任务。  
  
3.  在 **“数据事件探查任务编辑器”** 中，单击 **“配置文件请求”**。  
  
## <a name="using-the-requests-pane"></a>使用请求窗格  
 请求窗格是出现在页顶部的窗格。 此窗格将列出所有为当前数据事件探查任务配置的配置文件。 如果尚未配置任何配置文件，则请求窗格为空。 若要添加新的配置文件，请在 **“配置文件类型”** 列下的空白区域单击，并从列表中选择配置文件类型。 若要配置配置文件，请在请求窗格中选择配置文件，然后在 **“请求属性”** 窗格中设置配置文件的属性。  
  
### <a name="requests-pane-options"></a>请求窗格选项  
 请求窗格具有下列选项：  
  
 **视图**  
 选择查看为该任务配置的所有配置文件，还是仅查看其中的一个配置文件。  
  
 请求窗格中的列会根据选择的 **“视图”** 而发生更改。 有关这些列中各列的详细信息，请参阅下一节“请求窗格列”。  
  
### <a name="requests-pane-columns"></a>请求窗格列  
 请求窗格显示的列取决于选定的 **“视图”** ：  
  
-   如果选择查看 **“所有请求”**，则请求窗格会显示两列： **“配置文件类型”** 和 **“请求 ID”**。  
  
-   如果选择查看五个列配置文件中的一个，则请求窗格会显示四列： **“配置文件类型”**、 **“表或视图”**、 **“列”** 和 **“请求 ID”**。  
  
-   如果选择查看候选键配置文件，则请求窗格会显示四列：“配置文件类型”、“表或视图”、“键列”和“请求 ID”。  
  
-   如果选择查看函数依赖关系配置文件，则请求窗格会显示五列： **“配置文件类型”**、 **“表或视图”**、 **“决定列”**、 **“依赖列”** 和 **“请求 ID”**。  
  
-   如果选择查看值包含配置文件，则请求窗格会显示六列： **“配置文件类型”**、 **“子集端表或视图”**、 **“超集端表或视图”**、 **“子集端列”**、 **“超集端列”** 和 **“请求 ID”**。  
  
 以下各节逐一介绍这些列。  
  
#### <a name="columns-common-to-all-views"></a>所有视图的公共列  
 **“配置文件类型”**  
 从下面的选项选择一个数据配置文件：  
  
|值|说明|  
|-----------|-----------------|  
|**候选键配置文件请求**|计算候选键项配置文件。<br /><br /> 此配置文件报告某个列或列集是选定表的键还是近似键。 此配置文件还可以帮助您识别数据中的问题，如可能的键列中的重复值。|  
|**列长度分布配置文件请求**|计算列长度分布配置文件。<br /><br /> 列长度分布配置文件报告选定列中字符串值的所有不同长度以及每个长度所表示的表中的行的百分比。 此配置文件可以帮助您识别数据中的问题，例如值无效。 例如，在对以两个字符表示的美国州代码列进行事件探查，发现存在超过两个字符的值。|  
|**列 Null 比率配置文件请求**|列 Null 比率配置文件。<br /><br /> 列 Null 比率配置文件报告选定列中 null 值的百分比。 此配置文件可以帮助您识别数据中的问题，例如，列中 null 值的比率以外偏高。 例如，您对邮政编码列进行事件探查，却发现缺失的邮政编码所占的比例超出允许的范围。|  
|**列模式配置文件请求**|计算列模式配置文件。<br /><br /> 列模式配置文件报告涵盖字符串列中值的指定百分比的一组正则表达式。 此配置文件可以帮助您识别数据中的问题，如无效字符串。 它还可以建议可用于以后验证新值的正则表达式。 例如，邮政编码列的模式配置文件可能会产生正则表达式 \d{5}-\d{4}、\d{5} 和 \d{9}。 如果看到其他正则表达式，则数据可能包含无效或格式不正确的值。|  
|**列统计信息配置文件请求**|选择此选项可使用选定表或视图中所有适用列的默认设置来计算列统计信息配置文件。<br /><br /> 列统计信息配置文件报告的统计信息包括：例如，数值列的最小值、最大值、平均值和标准偏差以及 **datetime** 列的最小值和最大值。 此配置文件可以帮助您识别数据中的问题，如无效值。 例如，您对历史日期列进行事件探查，却发现最近的日期是一个将来的日期。|  
|**列值分布配置文件请求**|计算列值分布配置文件。<br /><br /> 列值分布配置文件报告选定列中所有的非重复值以及每个值所表示的表中的行的百分比。 此配置文件还可以报告其表示内容超过表中指定的行百分比的值。 此配置文件可帮助您识别数据中的问题，例如，列中非重复值的数目不正确。 例如，在对包含美国各州的列进行事件探查时发现，其中存在 50 多个非重复值。|  
|**函数依赖关系配置文件请求**|计算函数依赖关系配置文件。<br /><br /> 函数依赖关系配置文件报告某列（依赖列）中的值依赖另一列或列集（决定列）中的值的程度。 此配置文件还可以帮助您识别数据中的问题，如无效值。 例如，您探查美国邮政编码列和美国的州列之间的依赖关系。 同一邮政编码应始终对应同一州，但配置文件却发现有违反此依赖关系的情况。|  
|**值包含配置文件请求**|计算值包含配置文件。<br /><br /> 值包含配置文件计算两列或列集之间值的重叠。 此配置文件还可以确定一个列或列集是否适于用作两个选定表之间的外键。 此配置文件还可以帮助您识别数据中的问题，如无效值。 例如，您对某个 Sales 表的 ProductID 列进行事件探查，却发现该列包含在产品表的 ProductID 列中找不到的值。|  
  
 **RequestID**  
 显示请求的标识符。 通常无需更改自动生成的值。  
  
#### <a name="columns-common-to-all-individual-profiles"></a>对所有单个配置文件都通用的列  
 **连接管理器**  
 显示连接到源数据库的 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器。  
  
 **请求 ID**  
 显示请求的标识符。 通常无需更改自动生成的值。  
  
#### <a name="columns-common-to-the-five-individual-column-profiles"></a>对五个单个列配置文件都通用的列  
 **“表或视图”**  
 显示包含所选列的表或视图。  
  
 **列**  
 显示要进行分析的列。  
  
#### <a name="columns-specific-to-the-candidate-key-profile"></a>特定于候选键配置文件的列  
 **“表或视图”**  
 显示包含所选列的表或视图。  
  
 **键列**  
 显示要进行事件探查的列。  
  
#### <a name="columns-specific-to-the-functional-dependency-profile"></a>特定于函数依赖关系配置文件的列  
 **“表或视图”**  
 显示包含所选列的表或视图。  
  
 **“决定列”**  
 显示选定要作为决定列进行事件探查的列。 在美国邮政编码决定美国的州的示例中，决定列是邮政编码列  
  
 **Dependent column**  
 显示选定要作为依赖列进行事件探查的列。 在美国邮政编码决定美国的州的示例中，依赖列是州列。  
  
#### <a name="columns-specific-to-the-value-inclusion-profile"></a>特定于值包含配置文件的列  
 **“子集端表或视图”**  
 显示包含选定作为子集端列的列的表或视图。  
  
 **“超集端表或视图”**  
 显示包含选定作为超集端的列的表或视图。  
  
 **“子集端列”**  
 显示选定要作为子集端列进行事件探查的列。 在需要验证美国州列中的值是否可以在以两个字符表示的美国州代码的引用表中找到的示例中，子集列是源表中的州列。  
  
 **“超集端列”**  
 显示选定要作为超集端列进行事件探查的列。 在需要验证美国州列中的值是否可以在以两个字符表示的美国州代码的引用表中找到的示例中，超集列是引用表中的州代码列。  
  
## <a name="using-the-request-properties-pane"></a>使用请求属性窗格  
 **“请求属性”** 窗格显示在请求窗格下。 此窗格显示在请求窗格中选定的用于配置文件的选项。  
  
> [!NOTE]  
>   选择 **“配置文件类型”** 后，必须选择 **“请求 ID”** 字段查看 **“请求属性”** 中请求的配置文件的属性。  
  
 这些选项根据选定的配置文件而有所差异。 有关单个配置文件类型选项的详细信息，请参阅下面主题：  
  
-   [候选键配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/candidate-key-profile-request-options-data-profiling-task.md)  
  
-   [列 Null 比率配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-null-ratio-profile-request-options-data-profiling-task.md)  
  
-   [列统计信息配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-statistics-profile-request-options-data-profiling-task.md)  
  
-   [列值分布配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-value-distribution-profile-request-options-data-profiling-task.md)  
  
-   [列长度分布配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-length-distribution-profile-request-options-data-profiling-task.md)  
  
-   [列模式配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-pattern-profile-request-options-data-profiling-task.md)  
  
-   [函数依赖关系配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/functional-dependency-profile-request-options-data-profiling-task.md)  
  
-   [值包含配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/value-inclusion-profile-request-options-data-profiling-task.md)  
  
## <a name="see-also"></a>另请参阅  
 [数据事件探查任务编辑器（“常规”页）](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [单个表快速配置文件窗体（数据事件探查任务）](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
