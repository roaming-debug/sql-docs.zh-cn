---
description: Teradata 目标
title: Teradata 目标 | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 191c89d1fdced6ad1581dadff1e4ed92f397f640
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194709"
---
# <a name="teradata-destination"></a>Teradata 目标

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Teradata 目标在 Teradata 数据库中批量加载数据。

目标使用 Teradata 连接管理器连接到数据源。 有关详细信息，请参阅 [Teradata 连接管理器](teradata-connection-manager.md)。

## <a name="load-options"></a>加载选项

Teradata 目标支持两种数据加载模式：

- **TPT 流**：此模式使用 TPT API 流运算符（Teradata TPump 协议）。

- **TPT 加载**（快速批量加载）：此模式使用 TPT API 加载运算符（Teradata FastLoad 协议）进行快速批量加载。

快速加载模式具有以下限制：

- Teradata 数据库的会话限制取决于以下首先遇到的因素：
    - 使用 SESSIONS 命令设置会话限制
    - 每个 AMP 对应一个会话的 Teradata 数据库限制
    - 每个应用程序的最大会话数的平台限制：由通信处理器 (COP) 接口软件文件 CLISPB.DAT 中的 MaxSess 变量定义。 可以使用 TDP SET MAXSESSIONS 命令指定平台限制。 默认限制等于服务器 MAXSESS。

- 不支持联接索引。
- 不支持对目标表的外键引用。
- 不支持使用辅助索引定义的目标表。

有关 Teradata 快速加载限制的详细信息，请参阅 Teradata 的快速加载引用。

可以在 [Teradata 目标编辑器（“连接管理器”页）](#teradata-destination-editor-connection-manager-page)中设置模式。

## <a name="error-handling"></a>错误处理。

加载过程中返回的错误被写入到加载过程中锁定的临时错误表。
“高级编辑器”中的“最大错误数(MaxErrors)”属性设置可以写入这些表的最大错误数。

如果最大错误数大于零，则生成具有唯一名称的错误表，并将信息性消息输出到包日志。 可以通过标准 SSIS 组件错误输出来检索这些错误。

加载过程完成后，临时表将被删除。 如果 Teradata 目标无法读取临时表，则不会删除这些临时表，除非选中“始终删除错误表”属性。  如果加载过程在完成之前停止，则需要手动删除这些表（如果需要）。 这些表与目标表位于同一数据库中。

当达到最大错误数时，目标表状态取决于所使用的模式。

- 在快速加载模式下，目标表不可用。 若要再次执行，必须截断或删除并重新创建目标表。 不支持回滚。
- 在 TPT 流运算符模式下，Teradata 目标通过缓冲行机制执行。 如果作业失败，则在出现故障时完成的所有更改（发送的缓冲区）在目标表中将是永久性的。 不存在回滚概念。 将删除错误表。

Teradata 目标有错误输出。 有关详细信息，请参阅 [Teradata 目标编辑器（“错误输出”页）](#teradata-destination-editor-error-output-page)。

## <a name="parallelism"></a>并行度

并行度在快速加载模式下受到限制，多个独立的快速加载作业不能同时访问同一个表。 同时，并发快速加载作业的数量受数据库变量 MaxLoadTasks 的限制。

TPT Stream 模式中没有并行性限制。 可以同时对同一个表运行多个 Teradata 目标，而这可能会降低每个 Teradata 的性能。 有关详细信息，请参阅 Teradata 文档。

## <a name="troubleshooting-the-teradata-destination"></a>Teradata 目标疑难解答

可以记录 Teradata 源对 Teradata Parallel Transporter API (TPT API) 所做的调用。 可以在包级别启用包日志记录并选择“诊断”事件以记录调用。

可以通过启用 ODBC 驱动程序管理器跟踪，记录 Teradada 源对 Teradata ODBC 驱动程序所做的 ODBC 调用。 有关详细信息，请参阅 Microsoft 文档 *数据源管理员如何使用 ODBC 生成 ODBC 跟踪*。

## <a name="teradata-destination-custom-properties"></a>Teradata 目标的自定义属性

下表列出了 Teradata 目标的自定义属性。 所有属性均可读/写。

|属性名称|数据类型|说明|
|:-|:-|:-|
|AlwaysDropErrorTable|布尔|默认值为 False。 如果为 True，则删除所有错误表（即使 Teradata 目标无法读取）。|
|ArraySupport|布尔|默认值为 True。 如果为 True，DML 组将使用 ArraySupport。 仅适用于 TPT 流。 此属性位于“高级编辑器”中。|
|缓冲区|Integer|要增加的请求缓冲区数，值可设置为 2 到 64。 仅适用于 TPT 流。 此属性位于“高级编辑器”中。|
|BufferMode|布尔|默认值为 True。 如果使用 PutBuffer 功能，则必须为 True。 此属性位于“高级编辑器”中。|
|BufferSize|Integer|用于发送加载包的输出缓冲区大小 (KB)。 默认值为 1024。 仅适用于 TPT 加载。 此属性位于“高级编辑器”中。|
|DataEncryption|布尔|默认值为 False。 如果为 True，则使用完全安全加密。|
|DefaultCodePage|Integer|当数据源没有代码页信息时，要使用的代码页。 <br>**注意**：此属性位于“高级编辑器”中。|
|DetailedTracingLevel|Integer（枚举）|对于高级跟踪，请选择以下选项之一： <br> **OFF**：无高级日志记录。 <br> **常规**：记录特定于驱动程序的活动常规跟踪。 <br> **CLI**：记录 CLIv2 相关的活动跟踪。 <br> **通知方法**：记录通知功能相关的活动跟踪。 <br> **公共库**：记录 opcommon 库活动跟踪。 <br> **全部**：记录上述所有活动跟踪。 <br> 高级跟踪日志文件在 DetailedTracingFile 属性中定义。 <br> 如果选项未关闭，则必须设置 DetailedTracingFile 属性。 <br> 此属性位于“高级编辑器”中。|
|DetailedTracingFile|String|当 DetailedTracingLevel 未关闭时，自动生成的日志文件的路径。 此属性位于“高级编辑器”中。|
|DiscardLargeRow|布尔|默认值为 False。 如果为 True，则弃用大行（大于 64K）|
|ErrorTableName|String|错误表名称。 默认值为目标表名称|
|ExtendedStringColumnsAllocation|布尔|如果为 True，则使用最大传输字符分配系数。 <br> 如果将 Teradata 数据库“导出宽度表 ID”属性设置为“最大默认值”，应将此值设置为 True。 <br> 默认值为 False。|
|FastLoad|布尔|如果为 True，则使用快速加载。 默认值是 **false** 秒。 也可以在 [Teradata 目标编辑器（“连接管理器”页）](#teradata-destination-editor-connection-manager-page)中设置此模式。|
|MaxErrors|Integer|在数据流停止前可能发生的错误数。 默认值为 0，表示没有错误数限制。<br> 如果选择的是“错误处理”页中的“重定向流”。 在达到错误数限制前，所有错误都在错误输出中返回。 有关详细信息，请参阅 [Teradata 目标编辑器（“错误输出”页）](#teradata-destination-editor-error-output-page)。|
|MaxSessions|Integer|登录会话的最大数目。 此值必须大于 1。 对于每个可用 AMP，默认值为一个会话。|
|MinSessions|Integer|登录会话的最小数目。 此值必须大于 1。 对于每个可用 AMP，默认值为一个会话。|
|Pack|Integer|要打包到多语句请求中的语句数。 默认值为 20，允许的最大值为 2400。 仅适用于 TPT 流。 此属性位于“高级编辑器”中。|
|PackMaximum|布尔|如果为 True，请动态确定当前 Stream 作业的最大打包因子。 仅适用于 TPT 流。 此属性位于“高级编辑器”中。|
|QueryBandSessInfo|Varchar|用户定义的、基于会话的查询带表达式，用于启用退款监视和管理。 此属性必须为连接字符串格式。 此属性位于“高级编辑器”中。|
|ReplicationOveride|Integer（枚举）|选项： <br> **默认**：不会将任何 SET SESSION OVERRIDE REPLICATION 语句发送到数据库。 使用数据库默认设置。 <br> **更新时间**：重写标准复制服务控件。 <br> **OFF**：使用标准复制服务控件。 <br> 此属性仅适用于 TPT 流。 <br> 此属性位于“高级编辑器”中。|
|Robust|布尔|如果为 True，则使用可靠的重新启动逻辑进行恢复和重新启动操作。 此属性仅适用于 TPT 流。 此属性位于“高级编辑器”中。|
|TableName|String|包含正在使用的数据的表的名称。|
|TenacityHours|Integer|当加载/导出操作的最大数目已运行时，TPT 驱动程序尝试登录的小时数。 默认值为 4 小时。 此属性位于“高级编辑器”中|
|TenacitySleep|Integer|达到限制时，TPT 驱动程序在尝试登录之前暂停的分钟数。 限制由 MaxSessions 和 TenacityHours 属性定义。 默认值为 6 分钟。 此属性位于“高级编辑器”中|
|UnicodePassThrough|布尔|Off（默认值）：禁用 Unicode 直通。 <br>On：启用 Unicode 直通。|

## <a name="configuring-the-teradata-destination"></a>配置 Teradata 目标

可以编程方式或通过 SSIS 设计器来配置 Teradata 目标。

下图展示了 Teradata 目标编辑器。 其中包含“连接管理器”、“映射”和“错误输出”页。

有关详细信息，请参阅下列主题之一：

- [Teradata 目标编辑器（“连接管理器”页）](#teradata-destination-editor-connection-manager-page)
- [Teradata 目标编辑器（“映射”页）](#teradata-destination-editor-mappings-page)
- [Teradata 目标编辑器（“错误输出”页）](#teradata-destination-editor-error-output-page)

![目标编辑器](media/teradata-destination.png)

**“高级编辑器”** 对话框包含可通过编程方式设置的属性。
打开 **“高级编辑器”** 对话框：

- 在 Integration Services 项目的“数据流”屏幕上，右键单击 Teradata 目标，并选择“显示高级编辑器”。

若要详细了解可以在“高级编辑器”对话框中设置的属性，请参阅 [Teradata 目标的自定义属性](#teradata-destination-custom-properties)。

## <a name="teradata-destination-editor-connection-manager-page"></a>Teradata 目标编辑器（“连接管理器”页）

使用“Teradata 目标编辑器”对话框的“连接管理器”页，可以为目标选择 Teradata 连接管理器。 使用此页还可以选择数据库中的表或视图。

打开 Teradata 目标编辑器“连接管理器”页

- 在 SQL Server Data Tools 中，打开包含 Teradata 目标的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Teradata 目标。

- 单击“Teradata 目标编辑器”中的“连接管理器”。

### <a name="options"></a>选项

**“ODBC 目标编辑器”**

从列表中选择现有连接管理器，或单击“新建”来创建新的 Teradata 连接管理器。

**新建**

单击 **“新建”** 。 此时，“Teradata 连接管理器编辑器”对话框打开，可以在其中新建连接管理器。

**数据访问模式**

选择从源选择数据的方法。 选项显示在下表中：

|选项|说明|
|:-|:-|
|表名称 - TPT 流|使用 TPT 流运算符的增量模式。 <br>**表或视图的名称**：从该列表中选择现有表或视图。 此列表仅显示前 1000 个表。 可以键入表名前缀，或将名称的任何部分与 (*) 通配符一起使用来列出要使用的一个或多个表。|
|表名称 - TPL 加载|使用 TPT API 加载运算符（Teradata FastLoad 协议）的快速（直接路径）加载模式，要求目标表为空。 <br>**表或视图的名称**：从该列表中选择现有表或视图。 此列表仅显示前 1000 个表。 可以键入表名前缀，或将名称的任何部分与 (*) 通配符一起使用来列出要使用的一个或多个表。|

“数据加密”复选框：用于启用数据加密。 默认情况下，不选择此复选框。

“始终删除错误表”复选框：可删除所有实例中的错误表。

“错误表”：写入错误的表的名称。

“会话数下限”：登录会话的最小数目。 对于每个可用 AMP，默认值为一个会话。 该值必须大于 1。

“会话数上限”：登录会话的最大数目。 对于每个可用 AMP，默认值为一个会话。 该值必须大于 1。

“最大错误数”：在停止或重定向数据流之前可返回的最大错误数。 

## <a name="teradata-destination-editor-mappings-page"></a>Teradata 目标编辑器（“映射”页）

使用“Teradata 目标编辑器”对话框的“映射”页，可以将输入列映射到目标列。

打开 Teradata 目标编辑器“映射”页

- 在 SQL Server Data Tools 中，打开包含 Teradata 目标的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Teradata 目标。

- 单击“Teradata 目标编辑器”中的“映射”。

### <a name="options"></a>选项

**可用输入列**

可用输入列的列表。 将输入列拖放到某一可用目标列以映射这些列。

**可用目标列**

可用目标列的列表。 将目标列拖放到某一可用输入列以映射这些列。

**输入列**

查看选定的输入列。 可以删除映射，具体方法为选择“< 忽略 >”，以从输出中排除列。

**目标列**

查看所有可用目标列（包括映射和未映射的列）。

>[!NOTE]
>
>数据类型不受支持的列会从映射中删除，并显示警告。

## <a name="teradata-destination-editor-error-output-page"></a>Teradata 目标编辑器（“错误输出”页）
使用“Teradata 目标编辑器”对话框的“错误输出”页，可以选择错误处理选项。

**打开 Teradata 目标编辑器“错误输出”页**

- 在 SQL Server Data Tools 中，打开包含 Teradata 目标的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Teradata 目标。

- 单击“Teradata 目标编辑器”中的“错误输出”。

### <a name="options"></a>选项

**错误行为**

选择 Teradata 目标应如何处理流中的错误：忽略失败、重定向行或使组件失败。

**相关主题**：[数据中的错误处理](./error-handling-in-data.md?view=sql-server-2017)

**截断**

选择 Teradata 目标应如何处理流中的截断：忽略失败、重定向行或使组件失败。

## <a name="next-steps"></a>后续步骤

- 配置 [Teradata 连接管理器](teradata-connection-manager.md)
- 配置 [Teradata 源](teradata-source.md)
- 配置 [Teradata 目标](teradata-destination.md)
- 如有任何疑问，请访问[技术社区](https://aka.ms/AA6iwdw)。