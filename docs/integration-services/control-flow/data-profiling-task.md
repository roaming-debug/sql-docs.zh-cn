---
description: 数据事件探查任务
title: 数据事件探查任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataprofilingtask.f1
helpviewer_keywords:
- Data Profiling task [Integration Services], about Data Profiling task
- data profiling
- profiling data
ms.assetid: 248ce233-4342-42c5-bf26-f4387ea152cf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 60e136951deb8701893ea3d611597e8388909816
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123533"
---
# <a name="data-profiling-task"></a>数据事件探查任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  数据事件探查任务计算的各种配置文件可帮助您熟悉数据源并找出数据中要修复的问题。  
  
 可以在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中使用数据事件探查任务对 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中存储的数据进行事件探查并标识潜在的数据质量问题。  
  
> [!NOTE]  
>  本主题只介绍数据事件探查任务的功能和要求。 有关如何使用数据事件探查任务的演练，请参阅 [数据事件探查任务和查看器](../../integration-services/control-flow/data-profiling-task-and-viewer.md)。  
  
## <a name="requirements-and-limitations"></a>要求和限制  
 数据事件探查任务仅适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中存储的数据。 此任务不适用于第三方或基于文件的数据源。  
  
 而且，若要运行包含数据事件探查任务的包，必须使用对 tempdb 数据库具有读/写权限（包括 CREATE TABLE 权限）的帐户。  
  
## <a name="data-profiler-viewer"></a>数据配置文件查看器  
 在使用该任务计算了数据配置文件并将它们保存在文件中之后，可以使用独立的数据配置文件查看器来查看配置文件输出。 数据配置文件查看器还支持明细功能，以帮助您了解在配置文件输出中标识的数据质量问题。 有关详细信息，请参阅 [数据配置文件查看器](../../integration-services/control-flow/data-profile-viewer.md)。  
  
> [!IMPORTANT]  
>  输出文件可能包含有关数据库的敏感数据和数据库所包含的数据。 有关如何使此文件更加安全的建议，请参阅 [对包使用的文件的访问](../../integration-services/security/security-overview-integration-services.md#files)。  
>   
>  数据配置文件查看器中提供的明细功能会将实时查询发送到原始数据源。  
  
## <a name="available-profiles"></a>可用的配置文件  
 数据事件探查任务可以计算 8 个不同的数据配置文件。 其中的 5 个配置文件分析单列，其余的 3 个则分析多列或列和表之间的关系。  
  
 以下五个配置文件分析单个列。  
  
|分析单个列的配置文件|说明|  
|----------------------------------------------|-----------------|  
|列长度分布配置文件|报告所选列中各个字符串值的不同长度，以及每个长度表示的行在表中的百分比。<br /><br /> 此配置文件可以帮助您识别数据中存在的问题，如无效的值。 例如，美国州代码列应以两个字符来表示，但在对其进行事件探查时却发现有超过两个字符的值。|  
|列 Null 比率配置文件|报告所选列中 Null 值的百分比。<br /><br /> 此配置文件可以帮助您识别数据中存在的问题，如列中 null 值的比率意外偏高。 例如，您对邮政编码列进行事件探查，却发现缺失的邮政编码所占的比例超出允许的范围。|  
|列模式配置文件|报告一组正则表达式，其中涵盖字符串列中指定百分比的值。<br /><br /> 此配置文件可以帮助您识别数据中存在的问题，如无效的字符串。 它还可以建议可用于以后验证新值的正则表达式。 例如，美国邮政编码列的模式配置文件可能会生成正则表达式：\d{5}-\d{4}、\d{5} 和 \d{9}。 如果看到其他正则表达式，则数据可能包含无效或格式不正确的值。|  
|列统计信息配置文件|报告各种统计信息，例如数值列的最小值、最大值、平均值和标准偏差，以及 **datetime** 列的最小值和最大值。<br /><br /> 此配置文件可以帮助您识别数据中存在的问题，如无效的日期。 例如，您对历史日期列进行事件探查，却发现最近的日期是一个将来的日期。|  
|列值分布配置文件|报告选定列中所有的非重复值以及每个值所表示的行在表中的百分比。 还可以报告一些表示该表中的行超过指定百分比的值。<br /><br /> 此配置文件可以帮助您识别数据中存在的问题，例如列中非重复值的数目不正确。 例如，您对应该包含美国的各州的列进行事件探查，却发现 50 多个非重复值。|  
  
 以下三个配置文件分析多个列或列与表之间的关系。  
  
|分析多个列的配置文件|说明|  
|--------------------------------------------|-----------------|  
|候选键配置文件|报告某个列或列集是选定表的键还是近似键。<br /><br /> 此配置文件还可以帮助您识别数据中存在的问题，如可能的键列中存在重复值。|  
|函数依赖关系配置文件|报告某列（依赖列）中的值对另一列或一组列（决定列）中的值的依赖程度。<br /><br /> 此配置文件也可以帮助您识别数据中存在的问题，如无效的值。 例如，您对包含美国邮政编码的列和包含美国各州的列之间的依赖关系进行事件探查。 同一邮政编码应始终对应同一州，但配置文件却发现有违反此依赖关系的情况。|  
|值包含配置文件|计算两列或两个列集之间的重叠值。 此配置文件还可以确定列或列集是否适合用作选定表间的外键。<br /><br /> 此配置文件也可以帮助您识别数据中存在的问题，如无效的值。 例如，您对某个 Sales 表的 ProductID 列进行事件探查，却发现该列包含在产品表的 ProductID 列中找不到的值。|  
  
## <a name="prerequisites-for-a-valid-profile"></a>有效配置文件的必备条件  
 除非选择的表和列不为空并且这些列包含对该配置文件有效的数据类型，否则配置文件无效。  
  
### <a name="valid-data-types"></a>有效的数据类型  
 某些可用的配置文件仅对于特定的数据类型有效。 例如，对包含 numeric 或 **datetime** 值的列计算列模式配置文件是没有意义的。 因此，这样的配置文件无效。  
  
|配置文件|有效的数据类型*|  
|-------------|------------------------|  
|列统计信息配置文件|numeric 类型或 **datetime** 类型的列（ **datetime** 列无 **mean** 和 **stddev** ）|  
|列 Null 比率配置文件|所有列**|  
|列值分布配置文件|**integer** 类型、 **char** 类型和 **datetime** 类型的列|  
|列长度分布配置文件|**char** 类型的列|  
|列模式配置文件|**char** 类型的列|  
|候选键配置文件|**integer** 类型、 **char** 类型和 **datetime** 类型的列|  
|函数依赖关系配置文件|**integer** 类型、 **char** 类型和 **datetime** 类型的列|  
|包含配置文件|**integer** 类型、 **char** 类型和 **datetime** 类型的列|  
  
 \* 在上述有效的数据类型表中， **integer**、 **char**、 **datetime** 和 **numeric** 类型分别包括以下特定的数据类型：  
  
 Integer 类型包括 **bit**、 **tinyint**、 **smallint**、 **int** 和 **bigint**。  
  
 Character 类型包括 **char**、 **nchar**、 **varchar** 和 **nvarchar** ，但不包括 **varchar(max)** 和 **nvarchar(max)**。  
  
 Date 和 time 类型包括 **datetime**、 **smalldatetime** 和 **timestamp**。  
  
 Numeric 类型包括 **integer** 类型（ **bit** 除外）、 **money**、 **smallmoney**、 **decimal**、 **float**、 **real** 和 **numeric**。  
  
 \* 除列 Null 比率配置文件以外，其他任何配置文件都不支持 image、text、XML、udt 和 variant 类型\* 。  
  
### <a name="valid-tables-and-columns"></a>有效的表和列  
 如果表或列为空，则数据事件探查将执行以下操作：  
  
-   当所选表或视图为空时，数据事件探查任务不会计算任何配置文件。  
  
-   当所选列中的所有值为 null 值时，数据事件探查任务只计算列 Null 比率配置文件。 此任务不计算类长度分布配置文件、列模式配置文件、列统计信息配置文件或列值分布配置文件。  
  
## <a name="features-of-the-data-profiling-task"></a>数据事件探查任务的功能  
 数据事件探查任务具有以下便利的配置选项：  
  
-   **通配符列** 在配置配置文件请求时，该任务接受用 **(\*)** 通配符代替列名称。 这简化了配置并更易于发现不熟悉的数据的特征。 当任务运行时，该任务对具有适当的数据类型的各列进行事件探查。  
  
-   **“快速配置文件”** You can select “快速配置文件” to configure the task quickly. 快速配置文件使用所有默认配置文件和默认设置对表或视图进行事件探查。  
  
## <a name="custom-logging-messages-available-on-the-data-profililng-task"></a>数据事件探查任务可用的自定义日志记录消息  
 下表列出了数据事件探查任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**DataProfilingTaskTrace**|提供有关任务状态的说明性信息。 日志记录消息包含以下信息：<br /><br /> 开始处理请求<br /><br /> 查询开始<br /><br /> 查询结束<br /><br /> 完成计算请求|  
  
## <a name="output-and-its-schema"></a>输出及其架构  
 数据事件探查任务将所选的配置文件输出到根据 DataProfile.xsd 架构进行组织的 XML。 可以指定是将此 XML 输出保存到文件还是保存到包变量。 可以在 [https://schemas.microsoft.com/sqlserver/2008/DataDebugger/](https://schemas.microsoft.com/sqlserver/2008/DataDebugger/) 中在线查看此架构。 可以从网页上保存该架构的副本。 然后，可以在 Microsoft [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 或其他架构编辑器、在 XML 编辑器或在文本编辑器（如记事本）中查看该架构的本地副本。  
  
 此数据质量信息架构可能对以下操作有用：  
  
-   在组织内和跨组织交换数据质量信息。  
  
-   生成处理数据质量信息的自定义工具。  
  
 目标命名空间在架构中被标识为 [https://schemas.microsoft.com/sqlserver/2008/DataDebugger/](https://schemas.microsoft.com/sqlserver/2008/DataDebugger/)。  
  
## <a name="output-in-the-conditional-workflow-of-a-package"></a>在包的条件工作流中的输出  
 数据事件探查组件不包含内置功能，无法根据数据事件探查任务的输出在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包的工作流中实现条件逻辑。 但是，您只要在脚本任务中进行少量的编程工作即可轻松地添加此逻辑。 此代码将对 XML 输出执行 XPath 查询，然后将结果保存在包变量中。 用于将脚本任务连接到后续任务的优先约束可以使用表达式来确定工作流。 例如，脚本任务检测某一列中 null 值超过阈值的百分比。 如果此条件为 true，则可能需要中断包并解决此问题，然后再继续执行。  
  
## <a name="configuration-of-the-data-profiling-task"></a>数据事件探查任务的配置  
 使用 **“数据事件探查任务编辑器”** 配置数据事件探查任务。 此编辑器有以下两页：  
  
 [“常规”页](../../integration-services/control-flow/data-profiling-task-editor-general-page.md)  
 在“常规”页上，可指定输出文件或变量。 还可以选择 **“快速配置文件”** 对任务进行快速配置，以便使用默认的设置来计算配置文件。 有关详细信息，请参阅 [单个表快速配置文件窗体（数据事件探查任务）](../../integration-services/control-flow/single-table-quick-profile-form-data-profiling-task.md)。  
  
 [“配置文件请求”页](../../integration-services/control-flow/data-profiling-task-editor-profile-requests-page.md)  
 在“配置文件请求”页上，可指定数据源，选择要计算的数据配置文件并对其进行配置。 有关可以配置的各种配置文件的详细信息，请参阅以下主题：  
  
-   [候选键配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/candidate-key-profile-request-options-data-profiling-task.md)  
  
-   [列长度分布配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-length-distribution-profile-request-options-data-profiling-task.md)  
  
-   [列 Null 比率配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-null-ratio-profile-request-options-data-profiling-task.md)  
  
-   [列模式配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-pattern-profile-request-options-data-profiling-task.md)  
  
-   [列统计信息配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-statistics-profile-request-options-data-profiling-task.md)  
  
-   [列值分布配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/column-value-distribution-profile-request-options-data-profiling-task.md)  
  
-   [函数依赖关系配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/functional-dependency-profile-request-options-data-profiling-task.md)  
  
-   [值包含配置文件请求选项（数据事件探查任务）](../../integration-services/control-flow/value-inclusion-profile-request-options-data-profiling-task.md)  
  
  
