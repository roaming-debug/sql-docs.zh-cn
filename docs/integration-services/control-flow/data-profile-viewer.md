---
description: 数据配置文件查看器 (Data Profile Viewer)
title: 数据配置文件查看器 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dataprofileviewer.f1
helpviewer_keywords:
- Data Profile Viewer [Integration Services]
- Data Profiling task [Integration Services], output viewer
ms.assetid: b9043428-ce26-45bb-910c-588d07579565
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aaa5d2276908feccd86aac75012703e093b861d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352131"
---
# <a name="data-profile-viewer"></a>数据配置文件查看器 (Data Profile Viewer)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  数据事件探查过程的下一步是查看和分析数据配置文件。 可以在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包内运行数据事件探查任务并计算数据配置文件之后，查看这些配置文件。 有关如何设置和运行数据事件探查任务的详细信息，请参阅 [设置数据事件探查任务](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)。  
  
> [!IMPORTANT]  
>  输出文件可能包含有关数据库的敏感数据和数据库所包含的数据。 有关如何使此文件更加安全的建议，请参阅 [访问包使用的文件](../../integration-services/security/security-overview-integration-services.md#files)。  
  
## <a name="data-profiles"></a>数据配置文件  
 若要查看数据配置文件，请将数据事件探查任务配置为将其输出发送到文件，然后使用独立的数据配置文件查看器。 若要打开数据配置文件查看器，请执行以下操作之一。  
  
-   在“[!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器”中右键单击“数据事件探查”任务，然后单击“编辑”。 在 **“数据事件探查任务编辑器”** 的 **“常规”** 页上，单击 **“打开配置文件查看器”** 。  
  
-   在文件夹 \<drive>:\Program Files (x86) | Program Files\Microsoft SQL Server\110\DTS\Binn 中运行 DataProfileViewer.exe。  
  
 该查看器使用多个窗格来显示请求的配置文件和计算所得的结果，包含可选详细信息和明细功能：  
  
 **“配置文件”** 窗格  
 “配置文件”  窗格显示数据配置文件任务中所请求的配置文件。 若要查看配置文件的计算结果，请在 **“配置文件”** 窗格中选择配置文件，结果将显示在查看器的其他窗格中。  
  
 **“结果”** 窗格  
 “结果”  窗格使用单行来汇总配置文件的计算结果。 例如，如果请求 **“列长度分布配置文件”** ，此行将包括最小和最大长度以及行数。 对于大多数配置文件，可以在 **“结果”** 窗格中选择此行，以便在可选的 **“详细信息”** 窗格中查看其他详细信息。  
  
 **“详细信息”** 窗格  
 对于大多数配置文件类型，“详细信息”  窗格显示有关在“结果”  窗格中选择的配置文件结果的其他信息。 例如，如果请求 **“列长度分布配置文件”** ， **“详细信息”** 窗格将显示找到的每列长度。 该窗格还显示列值为该列长度的行数和行数百分比。  
  
 对于对多列进行计算的三种配置文件类型（候选键、函数依赖关系和值包含），“详细信息”  窗格显示违反期望关系的信息。 例如，如果请求候选键配置文件，“详细信息”窗格将显示违反候选键唯一性约束的重复值。  
  
 如果有用于计算配置文件的数据源，则可以双击“详细信息”  窗格中的行，在“明细”  窗格中查看匹配的数据行。  
  
 “明细”  窗格  
 在满足以下条件的情况下，可以双击“详细信息”  窗格中的行，在“明细”  窗格中查看匹配的数据行：  
  
-   有用于计算配置文件的数据源。  
  
-   您有权查看数据。  
  
 为了连接到明细请求的源数据库，数据配置文件查看器使用 Windows 身份验证和当前用户的凭据。 数据配置文件查看器不会使用运行数据事件探查任务的包中存储的连接信息。  
  
> [!IMPORTANT]  
>  数据配置文件查看器中提供的明细功能会将实时查询发送到原始数据源。 这些查询可能会对服务器的性能产生负面影响。  
>   
>  如果从并非最近创建的输出文件中深化，则明细查询所返回的行集可能会与计算原始输出时所使用的行集不同。  
  
 有关数据配置文件查看器的用户界面的详细信息，请参阅 [Data Profile Viewer F1 Help]()。  
  
## <a name="data-profile-viewer-f1-help"></a>数据配置文件查看器 F1 帮助
  可以使用数据配置文件查看器查看数据事件探查任务的输出。  
  
 有关如何使用数据配置文件查看器的详细信息，请参阅 [数据配置文件查看器 (Data Profile Viewer)](../../integration-services/control-flow/data-profile-viewer.md)。 有关如何使用数据事件探查任务（该任务可创建在数据配置文件查看器中所分析的配置文件输出）的详细信息，请参阅 [设置数据事件探查任务](../../integration-services/control-flow/setup-of-the-data-profiling-task.md)。  
  
### <a name="static-options"></a>静态选项  
 **打开**  
 单击可查找包含数据事件探查任务输出的已保存文件。  
  
 **“配置文件”** 窗格  
 展开“配置文件”窗格中的树可以查看输出中包含的配置文件。 选择一个配置文件可以查看该配置文件的结果。  
  
 “消息”窗格  
 显示状态消息。  
  
 “明细”  窗格  
 如果数据事件探查任务使用的数据源可用，则可显示与输出中的值匹配的数据行。  
  
 例如，如果在查看针对美国各州的列的列值分布配置文件的输出，则 **“详细值分布”** 窗格可能包含值为 "WA" 的行。 双击“详细值分布”窗格中的行可以在明细窗格中查看该州列中值为 "WA" 的数据行。  
  
### <a name="dynamic-options"></a>动态选项  
  
#### <a name="profile-type--column-length-distribution-profile"></a>配置文件类型 = 列长度分布配置文件  
  
##### <a name="column-length-distribution-profile---column-pane"></a>列长度分布配置文件 - \<column> 窗格  
 **最小长度**  
 显示此列中值的最小长度。  
  
 **最大长度**  
 显示此列中值的最大长度。  
  
 **忽略前导空格**  
 显示计算此配置文件时 **IgnoreLeadingSpaces** 值是设置为 True 还是 False。 此属性已在数据事件探查任务编辑器的 **“配置文件请求”** 页中设置。  
  
 **忽略尾随空格**  
 显示计算此配置文件时 **IgnoreTrailingSpaces** 值是设置为 True 还是 False。 此属性已在数据事件探查任务编辑器的 **“配置文件请求”** 页中设置。  
  
 **行计数**  
 显示表或视图中的行数。  
  
##### <a name="detailed-length-distribution-pane"></a>“详细长度分布”窗格  
 **长度**  
 显示在进行事件探查的列中找到的列长度。  
  
 **Count**  
 显示进行事件探查的列的值为“长度”列中显示的长度的行数。  
  
 **百分比**  
 显示进行事件探查的列的值为“长度”列中显示的长度的行数百分比。  
  
#### <a name="profile-type--column-null-ratio-profile"></a>配置文件类型 = 列 Null 比率配置文件  
  
##### <a name="column-null-ratio-profile---column-pane"></a>列 Null 比率配置文件 - \<column> 窗格  
 **Null 计数**  
 显示进行事件探查的列为 Null 值的行数。  
  
 **Null 百分比**  
 显示进行事件探查的列为 Null 值的行数百分比。  
  
 **行计数**  
 显示表或视图中的行数。  
  
#### <a name="profile-type--column-pattern-profile"></a>配置文件类型 = 列模式配置文件  
  
##### <a name="column-pattern-profile---column-pane"></a>列模式配置文件 - \<column> 窗格  
 **行计数**  
 显示表或视图中的行数。  
  
##### <a name="pattern-distribution-pane"></a>“模式分布”窗格  
 **模式**  
 显示为进行事件探查的列计算的模式。  
  
 **百分比**  
 显示行值与“模式”列中显示的模式匹配的行数百分比。  
  
#### <a name="profile-type--column-statistics-profile"></a>配置文件类型 = 列统计信息配置文件  
  
##### <a name="column-statistics-profile---column-pane"></a>列统计信息配置文件 - \<column> 窗格  
 **最低**  
 显示在进行事件探查的列中发现的最小值。  
  
 最大值  
 显示在进行事件探查的列中发现的最大值。  
  
 **中间线**  
 显示在进行事件探查的列中发现的值的平均值。  
  
 **标准偏差**  
 显示在进行事件探查的列中发现的值的标准偏差。  
  
#### <a name="profile-type--column-value-distribution-profile"></a>配置文件类型 = 列值分布配置文件  
  
##### <a name="column-value-distribution-profile---column-pane"></a>列值分布配置文件 - \<column> 窗格  
 **非重复值数目**  
 显示在进行事件探查的列中发现的非重复值的计数。  
  
 **行计数**  
 显示表或视图中的行数。  
  
##### <a name="detailed-value-distribution-pane"></a>“详细值分布”窗格  
 **值**  
 显示在进行事件探查的列中发现的非重复值。  
  
 **Count**  
 显示进行事件探查的列具有“值”列中显示的值的行数。  
  
 **百分比**  
 显示进行事件探查的列具有“值”列中显示的值的行数的百分比。  
  
#### <a name="profile-type--candidate-key-profile"></a>配置文件类型 = 候选键配置文件  
  
##### <a name="candidate-key-profile---table-pane"></a>候选键配置文件 - \<table> 窗格  
 **键列**  
 显示为作为候选键进行事件探查所选择的列。  
  
 **键强度**  
 显示候选键列或候选键列组合的强度（按百分比）。 键强度小于 100% 指示存在重复值。  
  
##### <a name="key-violations-pane"></a>“键冲突”窗格  
 \<column1>、\<column2> 等  
 显示进行事件探查的列中找到的重复值。  
  
 **Count**  
 显示指定的列具有第一列中显示的值的行数。  
  
#### <a name="profile-type--functional-dependency-profile"></a>配置文件类型 = 函数依赖关系配置文件  
  
##### <a name="functional-dependency-profile-pane"></a>“函数依赖关系配置文件”窗格  
 **“决定列”**  
 显示选择作为决定列的单个列或多个列。 在相同的美国邮政编码应总对应相同的州的示例中，邮政编码列就是决定列。  
  
 **依赖列**  
 显示选择作为依赖列的单个列或多个列。 在相同的美国邮政编码应总对应相同的州的示例中，州列就是依赖列。  
  
 **函数依赖关系强度**  
 显示列之间的函数依赖关系强度（按百分比）。 键强度小于 100% 指示存在决定值不能决定依赖值的情况。 在相同的美国邮政编码应总对应相同的州的示例中，这可能表示某些州值无效。  
  
##### <a name="functional-dependency-violations-pane"></a>“函数依赖关系冲突”窗格  
  
> [!NOTE]  
>  数据中的错误值的高百分比可能导致函数依赖关系配置文件产生意外结果。 例如，在 90% 的行中，与邮政编码值“98052”对应的州值为“WI”。 配置文件将包含正确州值“WA”的行报告为冲突。  
  
 **\<determinant column name>**  
 显示在此函数依赖关系冲突实例中决定列或决定列组合的值。  
  
 **\<dependent column name>**  
 显示在此函数依赖关系冲突实例中依赖列的值。  
  
 **支持计数**  
 显示决定列值决定依赖列的行数。  
  
 **冲突计数**  
 显示决定列值不能决定依赖列的行数。 （在这些行中，依赖值是 \<dependent column name> 列中显示的值。）  
  
 **支持百分比**  
 显示决定列决定依赖列的行数的百分比。  
  
#### <a name="profile-type--value-inclusion-profile"></a>配置文件类型 = 值包含配置文件  
  
##### <a name="value-inclusion-profile-pane"></a>“值包含配置文件”窗格  
 **“子集端列”**  
 显示进行事件探查的列或列组合，以确定它们是否包含在超集列中。  
  
 **“超集端列”**  
 显示进行事件探查的列或列组合，以确定它们是否包含子集列中的值。  
  
 **包含强度**  
 显示列之间的重叠强度（按百分比）。 键强度小于 100% 指示存在在超集值中找不到子集值的情况。  
  
##### <a name="inclusion-violations-pane"></a>“包含冲突”窗格  
 \<column1>、\<column2> 等  
 显示在单个或多个超集列中找不到的单个或多个子集列中的值。  
  
 **Count**  
 显示指定的列具有第一列中显示的值的行数。  
