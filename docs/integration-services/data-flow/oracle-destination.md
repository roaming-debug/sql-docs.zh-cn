---
description: Oracle 目标
title: Oracle 目标 | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3b28d420d66e55565c882c9fa2ae41f86e1c0764
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343237"
---
# <a name="oracle-destination"></a>Oracle 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Oracle 目标将数据批量加载到 Oracle Database 中。

目标使用 Oracle Connection Manager 连接到数据源。 有关详细信息，请参阅 [Oracle Connection Manager](oracle-connection-manager.md)。

Oracle 目标包括，输入列和目标数据源中列之间的映射。 您不必将输入列映射到所有目标列，但有时如果没有将输入列映射到目标列可能会出错，具体取决于目标列的属性。 例如，如果目标列不允许出现 Null 值，则必须将输入列映射到该列。 此外，如果输入数据与目标列类型不兼容，则会在运行时发生错误。 处理方式包括，忽略错误、导致失败或将行重定向到错误输出，具体视错误行为设置而定。

Oracle 目标有一个常规输入和一个错误输出。

映射前，数据类型不受支持的列会从列中删除，并显示警告。
有关详细信息，请参阅[数据类型支持](oracle-data-type-support.md)。

## <a name="load-options"></a>加载选项

支持两种访问加载模式。 可以在 [Oracle 目标编辑器（“连接管理器”页）](#oracle-destination-editor-connection-manager-page)中设置模式。 这两种模式是：

- 批量加载：此模式是将数据批量加载到 Oracle 表中，并在同一事务下插入整个批。
若要详细了解如何配置此模式，请参阅 [Oracle 目标编辑器（“连接管理器”页）](#oracle-destination-editor-connection-manager-page)和 [Oracle 目标的自定义属性](#oracle-destination-custom-properties)。

- 使用直接路径快速加载：此模式是使用驱动程序的直接路径模式来加载 Oracle 表。 使用此模式时需要遵循一些限制；有关详细信息，请参阅 Oracle 文档。  
若要详细了解如何配置此模式，请参阅 [Oracle 目标编辑器（“连接管理器”页）](#oracle-destination-editor-connection-manager-page)和 [Oracle 目标的自定义属性](#oracle-destination-custom-properties)。

## <a name="error-handling"></a>错误处理。

Oracle 目标有错误输出。 组件的错误输出包括以下输出列：

- **错误代码**：表示当前错误的错误类型的数字。 错误代码可能来自：
    - Oracle 服务器。 请参阅 Oracle Database 文档中的详细错误说明。
    - SSIS 运行时。 有关 SSIS 错误代码的列表，请参阅 SSIS 错误代码和消息参考。
- **错误列**：导致转换错误的源列号。

- **错误数据列**：导致错误的数据。

支持在加载过程中出现的输出错误类型包括：数据转换、截断或约束冲突等。 请参阅 [Oracle 目标编辑器（“错误输出”页）](#oracle-destination-editor-error-output-page)。

“最大错误数(MaxErrors)”属性设置可以出现的最大错误数。 达到最大错误数后，执行停止并返回错误。 只有在达到最大错误数之前的执行记录，才会包含在目标表中。 有关详细配置，请参阅 [Oracle 目标编辑器（“连接管理器”页）](#oracle-destination-editor-connection-manager-page)。

## <a name="parallelism"></a>并行度

在批量加载模式下，并行运行配置没有限制，但性能可能会受标准记录锁定机制影响。 性能损失量取决于数据和表组织。

在直接路径协议（快速加载）中，只能将一个 Oracle 目标配置为同时针对同一个表运行，但可以使用并行模式。

使用并行直接路径，可以进行多个直接路径加载，从而能够将多个 Oracle 目标配置为同时针对同一个表并行运行。 Oracle 不会锁定目标表以仅在快速加载会话中使用，这样就能运行其他快速加载目标组件来并行加载同一个目标表。
并行直接路径的限制更多，应提前计划好任何并行加载。

没有理由使用单个并行会话。

有关使用并行直接路径加载时的限制，请参阅 Oracle 文档。

有关详细信息，请参阅 [Oracle 目标的自定义属性](#oracle-destination-custom-properties)。

## <a name="troubleshooting-the-oracle-destination"></a>Oracle 目标疑难解答

可以记录 Oracle 源对 Oracle 数据源进行的 ODBC 调用，以便对数据导出进行故障排除。 若要记录 Oracle 源对 Oracle 数据源进行的 ODBC 调用，请启用 ODBC 驱动程序管理器跟踪。 有关详细信息，请参阅 Microsoft 文档 *数据源管理员如何使用 ODBC 生成 ODBC 跟踪*。

## <a name="oracle-destination-custom-properties"></a>Oracle 目标的自定义属性

下表列出了 Oracle 目标的自定义属性。 所有属性均可读/写。

|属性名称|数据类型|说明|加载模式|
|:-|:-|:-|:-|
|BatchSize|Integer|用于大容量加载的批处理的大小。 这是作为批处理加载的行数。|仅用于批量模式。|
|DefaultCodePage|Integer|当数据源没有代码页信息时，要使用的代码页。 <br>**注意**：此属性仅由高级编辑器设置。|用于两种模式。|
|FastLoad|布尔|是否使用快速加载。 默认值是 **false** 秒。 也可以在 [Oracle 目标编辑器（“连接管理器”页）](#oracle-destination-editor-connection-manager-page)中设置此模式。 |用于两种模式。|
|MaxErrors|Integer|在数据流停止前可能发生的错误数。 默认值为 0，表示没有错误数限制。<br> 如果选择的是“错误处理”页中的“重定向流”。 在达到错误数限制前，所有错误都在错误输出中返回。 有关详细信息，请参阅[错误处理](#error-handling)。|仅用于快速加载模式。|
|NoLogging|布尔|是否禁用数据库日志记录。 默认值为 False，表示启用日志记录。|用于两种模式。|
|并行程序|布尔|是否允许并行加载。 True 表示允许其他加载会话针对同一个目标表运行。<br> 有关详细信息，请参阅[并行](#parallelism)。|仅用于快速加载模式。|
|TableName|String|包含正在使用的数据的表的名称。|用于两种模式。|
|TableSubName|String|子名称或子分区。 此值是可选的。<br> **注意**：只能在高级编辑器中设置此属性。|仅用于快速加载模式。|
|TransactionSize|Integer|可以在一个事务中进行的插入数。 默认值为 BatchSize。|仅用于批量模式。|
|TransferBufferSize|Integer|传输缓冲区大小。 默认值为 64 KB。|仅用于快速加载模式。|

## <a name="configuring-the-oracle-destination"></a>配置 Oracle 目标

可以编程方式或通过 SSIS 设计器来配置 Oracle 目标。

下图展示了 Oracle 目标编辑器。 其中包含“连接管理器”、“映射”和“错误输出”页。

有关详细信息，请参阅以下部分之一：

- [Oracle 目标编辑器（“连接管理器”页）](#oracle-destination-editor-connection-manager-page)
- [Oracle 目标编辑器（“映射”页）](#oracle-destination-editor-mappings-page)
- [Oracle 目标编辑器（“错误输出”页）](#oracle-destination-editor-error-output-page)

![Oracle 目标](media/oracle-destination.png)

**“高级编辑器”** 对话框包含可通过编程方式设置的属性。
打开 **“高级编辑器”** 对话框：

- 在 Integration Services 项目的“数据流”屏幕上，右键单击 Oracle 目标，并选择“显示高级编辑器”。

若要详细了解可以在“高级编辑器”对话框中设置的属性，请参阅 [Oracle 目标的自定义属性](#oracle-destination-custom-properties)。

## <a name="oracle-destination-editor-connection-manager-page"></a>Oracle 目标编辑器（“连接管理器”页）

使用“Oracle 目标编辑器”对话框的“连接管理器”页，可以为目标选择 Oracle Connection Manager。 使用此页还可以选择数据库中的表或视图。

**打开 Oracle 目标编辑器的“连接管理器”页的具体步骤**

- 在 SQL Server Data Tools 中，打开包含 Oracle 目标的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Oracle 目标。

- 单击“Oracle 目标编辑器”中的“连接管理器”。

### <a name="options"></a>选项

**“ODBC 目标编辑器”**

从列表中选择现有连接管理器，或单击“新建”来新建 Oracle Connection Manager。

**新建**

单击 **“新建”** 。 此时，“Oracle Connection Manager 编辑器”对话框打开，可以在其中新建连接管理器。

**数据访问模式**

选择从源选择数据的方法。 选项显示在下表中：

|选项|说明|
|:-|:-|
|表名称|将 Oracle 目标配置为，在批量模式下运行。 选项：<br><br> **表或视图的名称**：从列表中选择数据库中的可用表或视图。<br><br> **事务大小**：输入可以在一个事务中进行的插入数。 默认值为 BatchSize。<br><br> **批大小**：键入用于批量加载的批大小（已加载的行数）。
|表名 - 快速加载|将 Oracle 目标配置为，在快速（直接路径）加载模式下运行。 <br><br>可用选项如下：<br><br> **表或视图的名称**：从列表中选择数据库中的可用表或视图。<br><br> **并行加载**：是否启用并行加载。 有关详细信息，请参阅[并行](#parallelism)。<br><br> **无日志记录**：此复选框可禁用数据库日志记录。 此日志记录是用于恢复用途的 Oracle Database，与跟踪无关。<br><br> **最大错误数**：在数据流停止前可能发生的最大错误数。 默认值为 0，表示没有错误数限制。<br><br> 所有可能发生的错误都在错误输出中返回。<br><br> **传输缓冲区大小(KB)** ：输入传输缓冲区大小。 默认大小为 64KB。|

**查看现有数据**

单击“查看现有数据”最多可查看选定表的 200 行数据。

## <a name="oracle-destination-editor-mappings-page"></a>Oracle 目标编辑器（“映射”页）

使用“Oracle 目标编辑器”对话框的“映射”页，可以将输入列映射到目标列。

**打开 Oracle 目标编辑器的“映射”页的具体步骤**

- 在 SQL Server Data Tools 中，打开包含 Oracle 目标的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Oracle 目标。

- 单击“Oracle 目标编辑器”中的“映射”。

### <a name="options"></a>选项

**可用输入列**

可用输入列的列表。 将输入列拖放到某一可用目标列以映射这些列。

**可用目标列**

可用目标列的列表。 将目标列拖放到某一可用输入列以映射这些列。

**输入列**

查看选定的输入列。 可以删除映射，具体方法为选择“< 忽略 >”，以从输出中排除列。

**目标列**

查看所有可用目标列（包括映射和未映射的列）。

> [!NOTE]
>
>数据类型不受支持的列会从映射中删除，并显示警告。

## <a name="oracle-destination-editor-error-output-page"></a>Oracle 目标编辑器（“错误输出”页）

使用“Oracle 目标编辑器”对话框的“错误输出”页，可以选择错误处理选项。

**打开 Oracle 目标编辑器的“错误输出”页的具体步骤**

- 在 SQL Server Data Tools 中，打开包含 Oracle 目标的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Oracle 目标。

- 单击“Oracle 目标编辑器”中的“错误输出”。

### <a name="options"></a>选项

**错误行为**

选择 Oracle 源应如何处理流中的错误：忽略失败、重定向行或让组件失败。
**相关部分**：[数据中的错误处理](./error-handling-in-data.md)

**截断**

选择 Oracle 源应如何处理流中的截断：忽略失败、重定向行或让组件失败。

## <a name="next-steps"></a>后续步骤

- 配置 [Oracle Connection Manager](oracle-connection-manager.md)。
- 配置 [Oracle 源](oracle-source.md)。
- 配置 [Oracle 目标](oracle-destination.md)。
- 若有任何疑问，请访问 [TechCommunity](https://aka.ms/AA5u35j)。
