---
description: ODBC 源
title: ODBC 源 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.odbcsource.f1
- sql13.ssis.designer.odbcsource.connection.f1
- sql13.ssis.designer.odbcsource.columns.f1
- sql13.ssis.designer.odbcsource.errorhandling.f1
ms.assetid: abcf34eb-9140-4100-82e6-b85bccd22abe
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 12d70dc722250d33de52bf8d1f13d1ab06083108
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194756"
---
# <a name="odbc-source"></a>ODBC 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  ODBC 源通过使用数据库表、视图或 SQL 语句，从支持 ODBC 的数据库中提取数据。  
  
 ODBC 源具有以下数据访问模式用于提取数据：  
  
-   表或视图。  
  
-   SQL 语句的运行结果。  
  
 源使用 ODBC 连接管理器，它指定要使用的访问接口。  
  
 ODBC 源包含源数据输出列。 当 ODBC 目标中的输出列映射到目标列中时，如果没有任何输出列映射到目标列，可能会出现错误。 可以映射不同类型的列，但是，如果输出数据与目标不兼容，则在运行时会出现错误。 根据错误行为，将忽略对错误的设置，并且导致失败，或者行将发送到错误输出。  
  
 ODBC 源有一个常规输出和一个错误输出。  
  
## <a name="error-handling"></a>错误处理  
 ODBC 源有一个错误输出。 组件的错误输出包括以下输出列：  
  
-   **错误代码**：与当前错误相对应的编号。 有关错误的列表，请参阅您正在使用的支持 ODBC 的数据库文档。 有关 SSIS 错误代码的列表，请参阅 SSIS 错误代码和消息参考。  
  
-   **错误列**：导致错误（针对转换错误）的源列。  
  
-   标准的输出数据列。  
  
 根据错误行为设置，CDC 源支持在错误输出中返回在提取过程中发生的错误（数据转换、截断）。 有关详细信息，请参阅 [ODBC 目标编辑器（“连接管理器”页）](./odbc-destination.md)。  
  
## <a name="data-type-support"></a>数据类型支持  
 有关 ODBC 源支持的数据类型的信息，请参阅开放式数据库连接 (ODBC) 连接器。  
  
## <a name="extract-options"></a>提取选项  
 ODBC 源在“批处理”或“逐行”模式下操作。 使用的模式由 **FetchMethod** 属性确定。 下表对这些模式进行了说明。  
  
-   **批处理**：组件将基于发现的 ODBC 访问接口功能尝试使用最高效的提取方法。 对于大多数现今的 ODBC 提供程序，这是具有数组绑定的 SQLFetchScroll（其中，数组大小由 **BatchSize** 属性确定）。 如果选择“批处理”  并且提供程序不支持此方法，则 ODBC 目标将自动切换到“逐行”  模式。  
  
-   **逐行**：组件使用 SQLFetch 来一次一行的检索行。  
  
 有关 **FetchMethod** 属性的详细信息，请参阅 [ODBC Source Custom Properties](../../integration-services/data-flow/odbc-source-custom-properties.md)。  
  
## <a name="parallelism"></a>并行度  
 对于可对同一台计算机或不同计算机（并非一般的全局会话限制）上的相同表或不同表并行运行的 ODBC 源组件的数目没有限制。  
  
 但是，要使用的 ODBC 访问接口的限制可能会限制通过该访问接口的同时连接的数目。 这些限制将会限制 ODBC 源可能支持的并行实例的数目。 SSIS 开发人员必须知道要使用的任何 ODBC 访问接口的限制并且在生成 SSIS 包时将这些限制元素考虑进去。  
  
## <a name="troubleshooting-the-odbc-source"></a>ODBC 源故障排除  
 可以记录 ODBC 源对外部数据访问接口所做的调用。 利用此日志记录功能，可以对 ODBC 源执行的从外部数据源加载数据的操作进行故障排除。 若要记录 ODBC 源对外部数据访问接口进行的调用，请启用 ODBC 驱动程序管理器跟踪。 有关详细信息，请参阅 Microsoft 文档 *数据源管理员如何使用 ODBC 生成 ODBC 跟踪*。  
  
## <a name="configuring-the-odbc-source"></a>配置 ODBC 源  
 您可以通过编程方式或者通过 SSIS 设计器配置 ODBC 源。  
  
 **“高级编辑器”** 对话框包含可通过编程方式设置的属性。  
  
 打开 **“高级编辑器”** 对话框：  
  
-   在您的 **项目的** “数据流” [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 屏幕上，右键单击 ODBC 源，然后选择 **“显示高级编辑器”**。  
  
 有关可在“高级编辑器”对话框中设置的属性的详细信息，请参阅 [ODBC Source Custom Properties](../../integration-services/data-flow/odbc-source-custom-properties.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [使用 ODBC 源提取数据](../../integration-services/data-flow/extract-data-by-using-the-odbc-source.md)  
  
-   [ODBC Source Custom Properties](../../integration-services/data-flow/odbc-source-custom-properties.md)  
  
## <a name="odbc-source-editor-connection-manager-page"></a>ODBC 源编辑器（“连接管理器”页）
  可以使用 **“ODBC 源编辑器”** 对话框的 **“连接管理器”** 页，为源选择 ODBC 连接管理器。 使用此页还可以选择数据库中的表或视图。  
  
### <a name="task-list"></a>任务列表  
 **打开“ODBC 源编辑器”的“连接管理器”页**  
  
-   在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开具有 ODBC 源的 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 包。  
  
-   在“数据流”选项卡上，双击 ODBC 源。  
  
### <a name="options"></a>选项  
  
#### <a name="connection-manager"></a>“ODBC 源编辑器”  
 从列表中选择现有 ODBC 连接管理器，或单击 **“新建”** 创建新的连接。 该连接可以指向支持 ODBC 的任何数据库。  
  
#### <a name="new"></a>新建  
 单击 **“新建”** 。 **“配置 ODBC 连接管理器编辑器”** 对话框随即打开，供您在其中创建新的 ODBC 连接管理器。  
  
#### <a name="data-access-mode"></a>数据访问模式  
 选择从源选择数据的方法。 选项显示在下表中：  
  
|选项|说明|  
|------------|-----------------|  
|表名称|从 ODBC 数据源中的表或视图检索数据。 选择此选项后，请从列表中为以下选项选择一个值：|  
||**表或视图的名称**：从列表中选择一个可用表或视图，或键入正则表达式以标识该表。|  
||该列表仅包含前 1000 个表。 如果您的数据库包含超过 1000 个表，则可以键入表名的开头，或者使用 (*) 通配符输入名称的任何部分以便显示要使用的表。|  
|SQL 命令|使用 SQL 查询从 ODBC 数据源中检索数据。 您应该采用正在使用的源数据库的语法编写查询。 选择此选项后，请采用以下方法之一输入查询：|  
||在 **“SQL 命令文本”** 字段中，输入 SQL 查询的文本。|  
||单击 **“浏览”** 从文本文件加载 SQL 查询。|  
||单击 **“分析查询”** 验证查询文本的语法。|  
  
#### <a name="preview"></a>预览  
 单击 **“预览”** ，查看从选定的表或视图中提取的最多前 200 行数据。  
  
## <a name="odbc-source-editor-columns-page"></a>ODBC 源编辑器（“列”页）
  可以使用“ODBC 源编辑器”对话框的“列”页，将输出列映射到每个外部（源）列。  
  
### <a name="task-list"></a>任务列表  
 **打开“ODBC 源编辑器”的“列”页**  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开具有 ODBC 源的 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 包。  
  
2.  在“数据流”选项卡上，双击 ODBC 源。  
  
3.  在 **“ODBC 源编辑器”** 中，单击 **“列”**。  
  
### <a name="options"></a>选项  
  
#### <a name="available-external-columns"></a>可用外部列  
 数据源中的可用外部列的列表。 无法使用此表添加或删除列。 从源中选择要使用的列。 所选列将按照选择它们时的顺序添加到 **“外部列”** 列表中。  
  
 选中 **“全选”** 复选框可以选择所有列。  
  
#### <a name="external-column"></a>外部列  
 外部（源）列的视图，该视图按照您在配置使用 ODBC 源中数据的组件时所看到的列顺序显示。  
  
#### <a name="output-column"></a>输出列  
 输入每个输出列的唯一名称。 默认值为所选外部（源）列的名称；不过，您也可以任选一个唯一的描述性名称。 输入的名称显示在 SSIS 设计器中。  
  
## <a name="odbc-source-editor-error-output-page"></a>ODBC 源编辑器（“错误输出”页）
  可以使用 **“ODBC 源编辑器”** 对话框的 **“错误输出”** 页选择错误处理选项。  
  
### <a name="task-list"></a>任务列表  
 **打开“ODBC 源编辑器”的“错误输出”页**  
  
-   在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开具有 ODBC 源的 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 包。  
  
-   在“数据流”选项卡上，双击 ODBC 源。  
  
-   在 **“ODBC 源编辑器”** 中，单击 **“错误输出”**。  
  
### <a name="options"></a>选项  
  
#### <a name="inputoutput"></a>输入/输出  
 查看数据源的名称。  
  
#### <a name="column"></a>列  
 未使用。  
  
#### <a name="error"></a>错误  
 选择 ODBC 源应该如何处理流中的错误：忽略失败、重定向行或使组件失败。  
  
#### <a name="truncation"></a>截断  
 选择 ODBC 源应该如何处理流中的截断：忽略失败、重定向行或使组件失败。  
  
#### <a name="description"></a>描述  
 未使用。  
  
#### <a name="set-this-value-to-selected-cells"></a>将此值设置到选定的单元格  
 选择发生错误或截断时 ODBC 源应如何处理所有选定的单元格：忽略失败、重定向行或使组件失败。  
  
#### <a name="apply"></a>应用  
 将错误处理选项应用到选定的单元格。  
  
### <a name="error-handling-options"></a>错误处理选项  
 使用下列选项来配置 ODBC 源处理错误和截断的方式。  
  
#### <a name="fail-component"></a>组件失败  
 发生错误或截断时数据流任务失败。 此选项为默认行为。  
  
#### <a name="ignore-failure"></a>忽略失败  
 忽略错误或截断。  
  
#### <a name="redirect-flow"></a>重定向流  
 将引起错误或截断的行定向到 ODBC 源的错误输出。  
  
