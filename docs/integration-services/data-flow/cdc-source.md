---
description: CDC 源
title: CDC 源 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdcsource.f1
- sql13.ssis.designer.cdcsource.connection.f1
- sql13.ssis.designer.cdcsource.columns.f1
- sql13.ssis.designer.cdcsource.errorhandling.f1
ms.assetid: 99775608-e177-44ed-bb44-aaccb0f4f327
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c10e3f1aa1460f63f3fcd3e95bb95bef8e63065
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049204"
---
# <a name="cdc-source"></a>CDC 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  CDC 源从 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 更改表中读取某一范围的更改数据并且将更改向下传递给其他 SSIS 组件。  
  
 由 CDC 源读取的更改数据的范围称作 CDC 处理范围并且由在当前数据流开始前执行的 CDC 控制任务确定。 该 CDC 处理范围是从维护一组表的 CDC 处理状态的包变量的值派生的。  
  
 CDC 源通过使用数据库表、视图或 SQL 语句，从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中提取数据。  
  
 CDC 源使用以下配置：  
  
-   用于访问 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC 数据库的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ADO.NET 连接管理器。 有关配置 CDC 源连接的详细信息，请参阅 [CDC 源编辑器（“连接管理器”页）]()。  
  
-   为 CDC 启用的表。  
  
-   所选表（如果存在多个）的捕获实例的名称。  
  
-   更改处理模式。  
  
-   基于所确定的 CDC 处理范围的 CDC 状态包变量的名称。 CDC 源不修改该变量。  
  
 CDC 源返回的数据与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CDC 函数 cdc.fn_cdc_get_all_changes_\<capture-instance-name> 或 cdc.fn_cdc_get_net_changes_\<capture-instance-name>（在可用时）返回的数据相同 。 唯一可选的添加是列 **__$initial_processing** ，它指示当前处理范围是否可与表的初始加载重叠。 有关初始处理的详细信息，请参阅 [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md)。  
  
 CDC 源有一个常规输出和一个错误输出。  
  
## <a name="error-handling"></a>错误处理  
 CDC 源有一个错误输出。 组件的错误输出包括以下输出列：  
  
-   **错误代码**：值始终为 -1。  
  
-   **错误列**：导致错误（针对转换错误）的源列。  
  
-   **错误行列**：导致了错误的记录数据。  
  
 根据错误行为设置，CDC 源支持在错误输出中返回在提取过程中发生的错误（数据转换、截断）。  
  
## <a name="data-type-support"></a>数据类型支持  
 Microsoft 的 CDC 源组件支持所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型，这些数据类型映射到正确的 SSIS 数据类型。  
  
## <a name="troubleshooting-the-cdc-source"></a>CDC 源故障排除  
 下面包含与排除 CDC 源问题有关的信息。  
  
### <a name="use-this-script-to-isolate-problems-and-reproduce-them-in-sql-server-management-studio"></a>使用此脚本可以确定问题并且在 SQL Server Management Studio 中复现这些问题  
 CDC 源操作受到在调用 CDC 源之前执行的 CDC 控制任务操作的约束。 CDC 控制任务准备 CDC 状态包变量的值以便包含开始和结束 LSN。 它执行与下面的脚本等效的函数：  
  
```sql
use <cdc-enabled-database-name>  
               declare @start_lsn binary(10), @end_lsn binary(10)  
               set @start_lsn = sys.fn_cdc_increment_lsn(  
                       convert(binary(10),'0x' + '<value-from-state-cs>', 1))  
               set @end_lsn =   
                       convert(binary(10),'0x' + '<value-from-state-ce>', 1)  
               select * from cdc.fn_cdc_get_net_changes_dbo_Table1(@start_lsn,  
@end_lsn, '<mode>')  
```  
  
 其中：  
  
-   \<cdc-enabled-database-name> 是包含更改表的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的名称。  
  
-   \<value-from-state-cs> 是在 CDC 状态变量中以 CS/\<value-from-state-cs>/（CS 表示 Current-processing-range-Start）形式出现的值。  
  
-   \<value-from-state-ce> 是在 CDC 状态变量中以 CE/\<value-from-state-cs>/（CE 表示 Current-processing-range-End）形式出现的值。  
  
-   \<mode> 是 CDC 处理模式。 处理模式具有以下值之一： **“全部”** 、 **“全部且具有旧值”** 、 **“净值”** 、 **“具有更新掩码的净值”** 和 **“净值且具有合并”** 。  
  
 此脚本可通过在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中复现问题（在其中可以轻松地复现和标识错误），有助于标识问题。  
  
#### <a name="sql-server-error-message"></a>SQL Server 错误消息  
 下面是可以由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]返回的消息：  
  
 为过程或函数 cdc.fn_cdc_get_net_changes_\<..> 提供的参数数目不足。  
  
 此错误并不表示缺少参数。 这意味着 CDC 状态变量中的开始或结束 LSN 值无效。  
  
## <a name="configuring-the-cdc-source"></a>配置 CDC 源  
 您可以通过编程方式或者通过 SSIS 设计器配置 CDC 源。  
  
 有关详细信息，请参阅下列主题之一：  
  
-   [CDC 源编辑器（“连接管理器”页）]()  
  
-   [CDC 源编辑器（“列”页）]()  
  
-   [CDC 源编辑器（“错误输出”页）]()  
  
 **“高级编辑器”** 对话框包含可通过编程方式设置的属性。  
  
 打开 **“高级编辑器”** 对话框：  
  
-   在您的 **项目的** “数据流” [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 屏幕上，右键单击 CDC 源，然后选择 **“显示高级编辑器”** 。  
  
 有关可在 **“高级编辑器”** 对话框中设置的属性的详细信息，请参阅 [CDC Source Custom Properties](../../integration-services/data-flow/cdc-source-custom-properties.md)。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [CDC 源自定义属性](../../integration-services/data-flow/cdc-source-custom-properties.md)  
  
-   [使用 CDC 源提取更改数据](../../integration-services/data-flow/extract-change-data-using-the-cdc-source.md)  
  
## <a name="cdc-source-editor-connection-manager-page"></a>CDC 源编辑器（“连接管理器”页）
  可以使用“CDC 源编辑器”对话框的“连接管理器”页，为 CDC 源从其中读取更改行的 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 数据库（CDC 数据库）选择 ADO.NET 连接管理器。 一旦选择了 CDC 数据库，则需要选择该数据库中的一个捕获表。  
  
 有关 CDC 源的详细信息，请参阅 [CDC Source](../../integration-services/data-flow/cdc-source.md)。  
  
### <a name="task-list"></a>任务列表  
 **打开“CDC 源编辑器”的“连接管理器”页**  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开具有 CDC 源的 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 CDC 源。  
  
3.  在 **“CDC 源编辑器”** 中，单击 **“连接管理器”** 。  
  
### <a name="options"></a>选项  
 **ADO.NET 连接管理器**  
 从列表中选择现有连接管理器，或单击“新建”  创建新的连接。 该连接必须是指向为 CDC 启用的并且所选更改表位于其中的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的连接。  
  
 **新建**  
 单击 **“新建”** 。 **“配置 ADO.NET 连接管理器编辑器”** 对话框打开，可在其中创建新的连接管理器。  
  
 **CDC 表**  
 选择您要读取并馈送到下游 SSIS 组件以便处理的捕获更改所在的 CDC 源表。  
  
 **捕获实例**  
 选择或键入具有要读取的 CDC 表的“CDC 捕获实例”的名称。  
  
 一个捕获源表可具有一个或两个捕获实例，以便通过架构更改处理表定义的无缝转换。 如果为要捕获的源表定义了一个捕获实例，则选择要在此处使用的捕获实例。 表 [schema].[table] 的默认捕获实例名称为 \<schema>_\<table>，但使用的实际捕获实例名称可能会不同。 读取的实际表是 CDC 表 cdc .\<capture-instance>_CT。  
  
 **CDC 处理模式**  
 选择可以最好地满足您的处理需要的处理模式。 可能的选项包括：  
  
-   **所有**：返回当前 CDC 范围中的更改，但没有 **“更新前”** 值。  
  
-   **全部且具有旧值**：返回当前 CDC 处理范围中的更改，包括旧值（“更新前”  ）。 对于每个更新操作将会有两行，一个针对更新前值，一个针对更新后值。  
  
-   **净值**：对于当前 CDC 处理范围中修改的每个源行，仅返回一个更改行。 如果某一源行更新了多次，将生成合并的更改（例如，插入+更新作为单个更新生成，更新+删除作为单个删除生成）。 在净更改处理模式下工作时，可以拆分对删除、插入和更新输出的更改并且并行处理它们，因为单个源行出现多次。  
  
-   **具有更新掩码的净值**：此模式类似于一般的净值模式，但它还添加了命名模式为 __$\<column-name>\_ 的布尔值列（指示当前更改行中已更改的列）。  
  
-   **净值且具有合并**：此模式类似于一般的净值模式，但具有合并到单个合并操作中的插入和更新操作 (UPSERT)。  
  
> [!NOTE]  
>  对于所有净更改选项，源表必须具有主键或唯一索引。 对于不具有主键或唯一索引的表，您必须选择 **“全部”** 选项。  
  
 **包含 CDC 状态的变量**  
 选择为当前 CDC 上下文维护 CDC 状态的 SSIS 字符串包变量。 有关 CDC 状态变量的详细信息，请参阅 [定义状态变量](../../integration-services/data-flow/define-a-state-variable.md)。  
  
 **包括重新处理指示器列**  
 选中此复选框可以创建称作 **__$reprocessing** 的特殊输出列。  
  
 在 CDC 处理范围与初始处理范围（与初始加载期间相对应的 LSN 的范围）重叠时，或者在之前的运行存在错误后重新处理某一 CDC 处理范围时，该列具有值 **true** 。 通过此指示器列，SSIS 开发人员可以在重新处理更改时以不同方式处理错误（例如，可忽略删除不存在的行和插入在重复键上失败之类的操作）。  
  
 有关详细信息，请参阅 [CDC Source Custom Properties](../../integration-services/data-flow/cdc-source-custom-properties.md)。  
  
## <a name="cdc-source-editor-columns-page"></a>CDC 源编辑器（“列”页）
  可以使用“CDC 源编辑器”对话框的“列”页，将输出列映射到每个外部（源）列   。  
  
### <a name="task-list"></a>任务列表  
 **打开“CDC 源编辑器”的“列”页**  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开具有 CDC 源的 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 CDC 源。  
  
3.  在 **“CDC 源编辑器”** 中，单击 **“列”** 。  
  
### <a name="options"></a>选项  
 **可用外部列**  
 数据源中的可用外部列的列表。 无法使用此表添加或删除列。 在源中选择要使用的列。 所选列将按照选择它们时的顺序添加到 **“外部列”** 列表中。  
  
 **“外部列”**  
 外部（源）列的视图，该视图按照您在配置使用 CDC 源中数据的组件时所看到的列顺序显示。 若要更改此顺序，请首先清除 **“可用外部列”** 列表中所选的列，然后以不同的顺序从列表中选择外部列。 所选列将按照选择它们时的顺序添加到 **“外部列”** 列表中。  
  
 **输出列**  
 输入每个输出列的唯一名称。 默认值为所选外部（源）列的名称，不过，也可以任选一个唯一的描述性名称。 输入的名称显示在 SSIS 设计器中。  
  
## <a name="cdc-source-editor-error-output-page"></a>CDC 源编辑器（“错误输出”页）
  可以使用 **“CDC 源编辑器”** 对话框的 **“错误输出”** 页选择错误处理选项。  
  
### <a name="task-list"></a>任务列表  
 **打开“CDC 源编辑器”的“错误输出”页**  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开具有 CDC 源的 [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] 包。  
  
2.  在“数据流”  选项卡上，双击 CDC 源。  
  
3.  在 **“CDC 源编辑器”** 中，单击 **“错误输出”** 。  
  
### <a name="options"></a>选项  
 **输入/输出**  
 查看数据源的名称。  
  
 **列**  
 查看在“CDC 源编辑器”对话框中“连接管理器”页上选择的外部（源）列。  
  
 **错误**  
 选择 CDC 源应该如何处理流中的错误：忽略失败、重定向行或使组件失败。  
  
 **截断**  
 选择 CDC 源应该如何处理流中的截断：忽略失败、重定向行或使组件失败。  
  
 **说明**  
 未使用。  
  
 **将此值设置到选定的单元格**  
 选择发生错误或截断时 CDC 源应如何处理所有选定的单元格：忽略失败、重定向行或使组件失败。  
  
 **应用**  
 将错误处理选项应用到选定的单元格。  
  
### <a name="error-handling-options"></a>错误处理选项  
 使用下列选项来配置 CDC 源处理错误和截断的方式。  
  
 **组件失败**  
 发生错误或截断时数据流任务失败。 此选项为默认行为。  
  
 **忽略失败**  
 忽略错误或截断，并且将数据行定向到 CDC 源输出。  
  
 **重定向流**  
 错误或截断数据行定向到 CDC 源的错误输出。 在此情况下使用 CDC 源错误处理。 有关详细信息，请参阅 [CDC Source](../../integration-services/data-flow/cdc-source.md)。  
  
## <a name="related-content"></a>相关内容  
  
-   mattmasson.com 上的博客文章 [CDC 源的处理模式](https://www.mattmasson.com/2012/01/processing-modes-for-the-cdc-source/)。  
  
