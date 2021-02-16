---
description: 连接到 Teradata 源
title: 连接到 Teradata 源 | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d0170a81d73edef1e893a16df93e1bc004415739
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337810"
---
# <a name="connect-to-the-teradata-source"></a>连接到 Teradata 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Teradata 源使用以下内容从 Teradata 数据库提取数据：
- 表或视图。
- SQL 语句的运行结果。

源使用 Teradata 连接管理器连接到 Teradata 源。 有关详细信息，请参阅[使用 Teradata 连接管理器](teradata-connection-manager.md)。

## <a name="troubleshoot-the-teradata-source"></a>Teradata 源疑难解答

可以记录 Teradata 源对 Teradata Parallel Transporter (TPT) API 所做的调用。 要执行此操作，可以在包级别启用包日志记录并选择“诊断”事件。

可以通过启用 ODBC 驱动程序管理器跟踪，记录 Teradata 源对 Teradata ODBC 驱动程序所做的开放式数据库连接 (ODBC) 调用。 有关详细信息，请参阅 [ODBC 数据源管理员如何生成 ODBC 跟踪](../../odbc/admin/setting-tracing-options.md)。

## <a name="parallelism"></a>并行度

Teradata 源支持并行，其中导出作业可以同时访问同一个表或不同的表。 称为 `MaxLoadTasks` 的数据库变量设置可同时运行的导出作业的数目限制。 可以使用 `MaxLoadTasks` 变量定义此最大值。

## <a name="teradata-source-custom-properties"></a>Teradata 源的自定义属性

下表列出了 Teradata 源的自定义属性。 所有属性均可读/写。

|属性名称|数据类型|说明|
|:-|:-|:-|
|AccessMode|Integer（枚举）|用来访问数据库的模式。 可取值为“表名”和“SQL 命令”。 默认值为“Table Name”。|
|BlockSize|Integer|将数据返回到客户端时使用的块大小（以字节为单位）。 默认值为 1048576 (1 MB)。 最小值为 256 个字节。 最大值为 16775168 个字节。<br> 此属性位于“高级编辑器”窗格中。|
|BufferMaxSize|Integer|GetBuffer 函数返回的数据缓冲区的最大总大小。 此大小必须足够大才能容纳至少一行数据，包括行标题、实际数据行和缓冲区尾部。 数据缓冲区的默认最大总大小为 16775552 个字节。 <br>有关详细信息，请参阅[使用 GetBuffer 从 Teradata 数据库导出数据](https://docs.teradata.com/reader/TvVKKmxaBAoyETJZD8zz_g/oaxiwNJmnCa6UctY4k498w)。|
|BufferMode|布尔|默认值为 *True*。 如果使用 PutBuffer 功能，值必须为 True。 此属性位于“高级编辑器”窗格中。|
|DataEncryption|布尔|默认值为 *False*。 如果值为 True，则使用完全安全加密。|
|DefaultCodePage|Integer|当数据源没有代码页信息时，要使用的代码页。 此属性位于“高级编辑器”窗格中。|
|DetailedTracingLevel|Integer（枚举）|对于高级跟踪，请选择以下选项之一： <br> *OFF*：无高级日志记录。 <br> *常规*：记录特定于驱动程序的活动常规跟踪。 <br> *CLI*：记录 CLIv2 相关的活动跟踪。 <br> *通知方法*：记录通知功能相关的活动跟踪。 <br> *公共库*：记录 opcommon 库活动跟踪。 <br> *全部*：记录前面的所有活动跟踪。 <br> 高级跟踪日志文件在 `DetailedTracingFile` 属性中定义。 <br> 如果选项未关闭，则必须设置 `DetailedTracingFile` 属性。 此属性位于“高级编辑器”窗格中。|
|DetailedTracingFile|String|当 DetailedTracingLevel 未关闭时，自动生成的日志文件的路径。 此属性位于“高级编辑器”窗格中。|
|DiscardLargeRow|布尔|默认值为 *False*。 如果值为 True，则弃用大行（大于 64 KB）。|
|ExtendedStringColumnsAllocation|布尔|如果值为 True，则使用最大传输字符分配系数。 <br> 如果将 Teradata 数据库 `Export Width Table ID` 属性设置为“最大默认值”，应将此值设置为 True。 <br> 默认值为 *False*。|
|JobMaxRowSize|Integer|可支持最大行大小。 如果 `DiscardLargeRow` 值为 True，则需要此值。<br>有效值： <br>*64*（默认值）：可支持 2 字节的行长度。 <br>*1024*：可支持 4 字节的行长度。|
|MaxSessions|Integer|登录会话的最大数目。 此值必须大于 1。 对于每个可用的访问模块处理器 (AMP)，默认值为一个会话。|
|MinSessions|Integer|登录会话的最小数目。 此值必须大于 1。 对于每个可用 AMP，默认值为一个会话。|
|QueryBandSessInfo|Varchar|连接字符串格式的用户定义的、基于会话的查询带区表达式。 此属性用于退款监视和管理。 此属性位于“高级编辑器”窗格中。|
|SpoolMode|Varchar|有效值是： <br>*Spool*：首先使用默认值“Spool”。 <br> *NoSpool*：请不要使用 Spool。 仅当数据库服务器（DBS）支持“NoSpool”时，此值才有效。 <br>  *NoSpoolOnly*：在任何情况下都不要使用“Spool”。 如果 DBS 不支持“NoSpool”，则该作业将终止，并出现错误。|
|SqlCommand|String|在 `AccessMode` 设置为“SQL 命令”时要执行的 SQL 命令。|
|TableName|String|当 `AccessMode` 设置为“表名”时，包含要使用的数据的表的名称。|
|TenacityHours|Integer|当加载/导出操作的最大数目已运行时，TPT 驱动程序尝试登录的小时数。 默认值为 4 小时。 此属性位于“高级编辑器”窗格中。|
|TenacitySleep|Integer|达到限制时，TPT 驱动程序在尝试登录之前暂停的分钟数。 该限制由 `MaxSessions` 和 `TenacityHours` 属性定义。 默认值为 6 分钟。 此属性位于“高级编辑器”窗格中。|
|UnicodePassThrough|布尔|*Off*（默认值）：禁用 Unicode 直通。 <br>*更新时间*：弃用 Unicode 直通。|

## <a name="configure-the-teradata-source"></a>配置 Teradata 源

可以通过编程方式或使用 SQL Server Integration Services (SSIS) 设计器来配置 Teradata 源。

下图显示了“Teradata 源编辑器”窗格。 有关详细信息，请参阅以下每个 Teradata 源编辑器部分：

- [“连接管理器”窗格](#the-connection-manager-pane)
- [“列”窗格](#the-columns-pane)
- [“错误输出”窗格](#the-error-output-pane)

![Teradata 源编辑器](media/teradata-source.png)

“高级编辑器”窗格包含可以编程方式设置的属性。 打开窗格：
- 在 Integration Services 项目的“数据流”页上，右键单击 Oracle 源，然后选择“显示高级编辑器”。

有关可在“高级编辑器”窗格中设置的属性的详细信息，请参阅 [Teradata 源自定义属性](#teradata-source-custom-properties)。

## <a name="the-connection-manager-pane"></a>“连接管理器”窗格

使用“连接管理器”窗格可为源选择 Teradata 连接管理器实例。 在该窗格中，你还可以从数据库中选择表或视图。 打开窗格：

1. 在 SQL Server Data Tools 中，打开包含 Teradata 源的 SSIS 包。

1. 在“数据流”选项卡上，双击 Teradata 源。

1. 在“Teradata 源编辑器”中，选择“连接管理器”选项卡。

### <a name="options"></a>选项

**“ODBC 目标编辑器”**

* 从列表中选择现有连接管理器，或选择“新建”来创建新的 Teradata 连接管理器实例。

**新建**

* 选择“新建”。 随即打开“Teradata 连接管理器编辑器”窗格。 在此窗格中，你可以创建新的连接管理器。

**数据访问模式**

* 选择从源选择数据的方法。 选项显示在下表中：

    |选项|说明|
    |:-|:-|
    |表名称 - TPT 导出|从 Teradata 数据源中的表或视图检索数据。 选择此选项后，从“表名或视图名称”列表中选择可用的表或视图。|
    |SQL 命令 - TPT 导出|使用 SQL 查询从 Teradata 数据源中检索数据。 选择此选项后，请使用下列方式之一输入查询： <ul><li>在 **“SQL 命令文本”** 字段中，输入 SQL 查询的文本。</li><li>选择“浏览”从文本文件加载 SQL 查询。</li><li>选择“分析查询”验证查询文本的语法。</li></ul>|

**预览**

* 选择“预览”，查看从选定的表或视图中提取的最多前 200 行数据。

## <a name="the-columns-pane"></a>“列”窗格

使用“列”窗格将输出列映射到每个外部（源）列。 打开窗格：

1. 在 SQL Server Data Tools 中，打开包含 Teradata 源的 SSIS 包。

1. 在“数据流”选项卡上，双击 Teradata 源。

1. 在“Teradata 源编辑器”中，选择“列”选项卡。

### <a name="options"></a>选项

**可用外部列**

此表列出了可以选择添加到“外部列”列表的可用外部列。 可以按你选择的顺序列出这些列。 无法使用此表添加或删除列。

* 选中“全选”复选框可以选择所有列。

**外部列**

选择的外部（源）列按顺序列出。 若要更改顺序，请先清除“可用外部列”列表，再选择顺序不同的列。

**输出列**

虽然选定外部（源）列的名称是默认输出名称，但你可以输入任何惟一的名称。

>[!NOTE]
>>如果存在包含不受支持的数据类型的列，你将收到一条警告，其中显示不支持的数据类型，并且将从映射列中删除相关列。

## <a name="the-error-output-pane"></a>“错误输出”窗格

使用“错误输出”窗格选择错误处理选项。 打开窗格：

1. 在 SQL Server Data Tools 中，打开包含 Teradata 源的 SSIS 包。

1. 在“数据流”选项卡上，双击 Teradata 源。

1. 在“Teradata 源编辑器”中，选择“错误输出”选项卡。

### <a name="options"></a>选项

**错误行为**

* 选择 Teradata 源应如何处理流中的错误： 
  * 忽略失败
  * 重定向行
  * 使组件失败

**相关主题**：请参阅 [数据中的错误处理](error-handling-in-data.md)。

**截断**

* 选择 Teradata 源应如何处理流中的截断： 
  * 忽略失败
  * 重定向行
  * 使组件失败

## <a name="next-steps"></a>后续步骤

- 配置 [Teradata 目标](teradata-destination.md)。
- 如有任何疑问，请访问[技术社区](https://aka.ms/AA6iwdw)。