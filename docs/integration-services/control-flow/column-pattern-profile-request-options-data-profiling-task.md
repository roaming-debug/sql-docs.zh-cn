---
description: 列模式信息配置文件请求选项（数据事件探查任务）
title: 列模式配置文件请求选项（数据事件探查任务）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Data Profiling Task Editor
ms.assetid: 9ccb8fc5-f65e-41a2-9511-7fa55586eb8b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 85beabd085481a48d6681bbaf2d2d381a1d0df93
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123562"
---
# <a name="column-pattern-profile-request-options-data-profiling-task"></a>列模式信息配置文件请求选项（数据事件探查任务）

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  可以使用 **“配置文件请求”** 页的 **“请求属性”** 窗格，为请求窗格中选定的 **“列模式信息配置文件请求”** 设置选项。 列模式配置文件报告一组涵盖指定字符串列中值的百分比的正则表达式。 此配置文件可以帮助您识别数据中的问题（如无效字符串），还可以建议可用于以后验证新值的正则表达式。 例如，美国邮政编码列的模式配置文件可能会生成正则表达式 \d{5}-\d{4}、\d{5} 和 \d{9}。 如果看到其他的正则表达式，则数据有可能包含无效或格式不正确的值。  
  
> [!NOTE]  
>  本主题中介绍的选项显示在 **“数据事件探查任务编辑器”** 的 **“配置文件请求”** 页中。 有关此编辑器页的详细信息，请参阅[数据事件探查任务编辑器（“配置文件请求”页）](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)。  
  
 有关如何使用数据事件探查任务的详细信息，请参阅 [设置数据事件探查任务](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)。 有关如何使用数据配置文件查看器分析数据事件探查任务输出的详细信息，请参阅 [数据配置文件查看器](../../integration-services/control-flow/data-profile-viewer.md)。  
  
## <a name="understanding-the-use-of-delimiters-and-symbols"></a>了解分隔符和符号的使用  
 在计算 **“列模式配置文件请求”** 的模式之前，数据事件探查任务先标记数据。 也就是说，该任务将字符串值分隔成称为标记的较小单元。 任务基于为 **“分隔符”** 和 **“符号”** 属性指定的分隔符和符号将字符串分隔成标记：  
  
-   **分隔符** 默认情况下，分隔符列表包含下列字符：空格、水平制表符 (\t)、换行符 (\n) 和回车符 (\r)。 可以指定其他分隔符，但不能删除默认分隔符。  
  
-   **符号**：默认情况下，“符号”列表包含字符 `,.;:-"'~=&/@!?()<>[]{}|#*^%` 以及刻度线。 例如，如果符号为“`()-`”，则值“(425) 123-4567”将标记为 ["(", "425", ")", "123", "-", "4567", ")"]。  
  
 一个字符不能同时作为分隔符和符号。  
  
 作为标记化过程的一部分，所有分隔符都将规范为一个空格，但字符被保留。  
  
## <a name="understanding-the-use-of-the-tag-table"></a>了解标记表的使用  
 根据需要，通过对在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中创建的特殊表中的标记和关联项进行排序，可以使用一个标记 (tag) 对关联标记 (token) 进行分组。 标记表必须有两个字符串列，一个命名为“标记”，另一个命名为“术语”。 这些列可以为 **char**、 **nchar**、 **varchar** 或 **nvarchar** 类型，但不可以为 **text** 或 **ntext**。 可以将多个标记和相应的术语组合到一个表中。 一个列模式配置文件请求仅可以使用一个标记表。 可以使用单独的 [!INCLUDE[vstecado](../../includes/vstecado-md.md)] 连接管理器来连接到标记表。 因此，标记表可以与源数据位于不同的数据库或不同的服务器。  
  
 例如，可以使用一个标记“方向”将可能出现在街道地址中的“东部”、“西部”、“北部”和“南部”值分为一组。 下表就是这样一个标记表的示例。  
  
|标记|术语|  
|---------|----------|  
|方向|东部|  
|方向|West|  
|方向|北部|  
|方向|南部|  
  
 可以使用另一个标记将表示街道地址中“街道”概念的不同的词分为一组。  
  
|标记|术语|  
|---------|----------|  
|街道|街道|  
|街道|大街|  
|街道|位置|  
|街道|道路|  
  
 基于这种标记组合，街道地址的结果模式可能类似下列模式：  
  
 `\d+\ LookupTag=Direction \d+\p{L}+\ LookupTag=Street`  
  
> [!NOTE]  
>  使用标记表会降低数据事件探查任务的性能。 请勿使用 10 个以上的标记，而且每个标记不应超过 100 个术语。  
  
 同一术语可属于多个标记。  
  
## <a name="request-properties-options"></a>请求属性选项  
 对于 **“列模式配置文件请求”**， **“请求属性”** 窗格显示下列选项组：  
  
-   **Data**，它包含 **TableOrView** 选项和 **Column** 选项  
  
-   **常规**  
  
-   **选项**  
  
### <a name="data-options"></a>Data 选项  
 **ConnectionManager**  
 选择使用 .NET Data Provider for [!INCLUDE[vstecado](../../includes/vstecado-md.md)] (SqlClient) 的现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接管理器连接到包含要进行事件探查的表或视图的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
 **TableOrView**  
 选择包含要进行事件探查的列的现有表或视图。  
  
 有关详细信息，请参阅本主题中的“TableorView 选项”部分。  
  
 **列**  
 选择要进行事件探查的现有列。 选择 **(\*)** 可对所有列进行事件探查。  
  
 有关详细信息，请参阅本主题中的“Column 选项”部分。  
  
#### <a name="tableorview-options"></a>TableOrView 选项  
 **架构**  
 指定选定表所属的架构。 此选项是只读的。  
  
 **表**  
 显示所选表的名称。 此选项是只读的。  
  
#### <a name="column-options"></a>Column 选项  
 **IsWildCard**  
 指定是否已选择通配符 **(\*)** 。 如果已选择 **(\*)** 来对所有列进行事件探查，则此选项设置为 **True**。 如果您已选择要对单独列进行事件探查，则为 **False** 。 此选项是只读的。  
  
 **ColumnName**  
 显示所选列的名称。 如果已选择 **(\*)** 来对所有列进行事件探查，则此选项空白。 此选项是只读的。  
  
 **StringCompareOptions**  
 此选项不适用于列模式配置文件。  
  
### <a name="general-options"></a>常规选项  
 **RequestID**  
 键入一个标识此配置文件请求的描述性名称。 通常无需更改自动生成的值。  
  
### <a name="options"></a>选项  
 **MaxNumberOfPatterns**  
 指定需要配置文件计算的模式的最大值。 此选项的默认值为 10。 最大值为 100。  
  
 **PercentageDataCoverageDesired**  
 指定需要计算模式覆盖的数据的百分比。 此选项的默认值为 95（百分比）。  
  
 **CaseSensitive**  
 指示模式是否应区分大小写。 此选项的默认值为 **False**。  
  
 **分隔符**  
 列出标记化文本时应视为与词之间的空格等效的字符。 在默认情况下，“分隔符”列表包含下列字符：空格、水平制表符 (\t)、换行符 (\n) 和回车符 (\r)。 可以指定其他分隔符，但不能删除默认分隔符。  
  
 有关详细信息，请参阅本主题前面的“了解分隔符和符号的使用”。  
  
 **符号**  
 列出应保留为模式一部分的符号。 示例可能包含：日期中的“/”、时间中的“:”以及电子邮件地址中的“\@”。 默认情况下，“符号”列表包含以下字符：`,.;:-"'~=&/@!?()<>[]{}|#*^%`。  
  
 有关详细信息，请参阅本主题前面的“了解分隔符和符号的使用”。  
  
 **TagTableConnectionManager**  
 选择使用 .NET Data Provider for [!INCLUDE[vstecado](../../includes/vstecado-md.md)] (SqlClient) 的现有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 连接管理器连接到包含标记表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库。  
  
 有关详细信息，请参阅本主题前面的“了解分隔符和符号的使用”。  
  
 **TagTableName**  
 选择现有的标记表，该表必须有两个分别命名为“标记”和“术语”的字符串列。  
  
 有关详细信息，请参阅本主题前面的“了解分隔符和符号的使用”。  
  
## <a name="see-also"></a>另请参阅  
 [数据事件探查任务编辑器（“常规”页）](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)   
 [单个表快速配置文件窗体（数据事件探查任务）](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)  
  
  
