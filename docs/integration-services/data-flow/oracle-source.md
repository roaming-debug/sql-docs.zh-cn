---
description: Oracle 源
title: Oracle 源
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fe89a97c1fb13d9446b0fe07f04c7399b42a439e
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489767"
---
# <a name="oracle-source"></a>Oracle 源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Oracle 源从 Oracle Database 中提取数据，模式如下：

- 表或视图。

- SQL 语句的运行结果。

数据源使用 Oracle Connection Manager 连接到 Oracle 源。 有关详细信息，请参阅 [Oracle Connection Manager](oracle-connection-manager.md)。

## <a name="error-output"></a>错误输出

错误输出包括以下列：  

- **错误代码**：表示当前错误的错误类型的数字。 错误代码可能来自：
    - Oracle 服务器。 请参阅 Oracle Database 文档中的详细错误说明。
    - SSIS 运行时。 有关 SSIS 错误代码的列表，请参阅 SSIS 错误代码和消息参考。
- **错误列**：导致转换错误的源列号。

- **错误数据列**：导致错误的数据。

Oracle 源在错误输出中返回在加载和提取过程中出现的错误。 有关详细信息，请参阅 [Oracle 源编辑器（“错误输出”页）](#oracle-source-editor-error-output-page)。

## <a name="troubleshooting-the-oracle-source"></a>Oracle 源疑难解答

可以记录 Oracle 源对 Oracle 数据源进行的 ODBC 调用，以便对数据导出进行故障排除。 若要记录 Oracle 源对 Oracle 数据源进行的 ODBC 调用，请启用 ODBC 驱动程序管理器跟踪。 有关详细信息，请参阅 Microsoft 文档 *数据源管理员如何使用 ODBC 生成 ODBC 跟踪*。

## <a name="oracle-source-custom-properties"></a>Oracle 源的自定义属性

Oracle 源的自定义属性如下所示。 所有属性均可读/写。

|属性名称|数据类型|说明|
|:-|:-|:-|
|AccessMode|Integer（枚举）|用来访问数据库的模式。 可取值为“表名”和“SQL 命令”。 默认值为“表名”。|
|BatchSize|Integer|用于大容量加载的批处理的大小。 这是作为数组提取的记录数。 <br>此属性仅由高级编辑器设置|
|DefaultCodePage|Integer|当数据源没有代码页信息时，要使用的代码页。 <br>此属性仅由高级编辑器设置。|
|PreFetchCount|Integer|预提取的行数。 <br>此属性仅由高级编辑器设置。|
|SqlCommand|字符串|在 AccessMode 设置为“SQL 命令”时要执行的 SQL 命令。|
|TableName|字符串|当 AccessMode 设置为“表名”时，包含要使用的数据的表的名称。|

## <a name="configuring-the-oracle-source"></a>配置 Oracle 源

可以编程方式或通过 SSIS 设计器来配置 Oracle 源。

下图展示了 Oracle 源编辑器。 其中包含“连接管理器”、“列”和“错误输出”页。

有关详细信息，请参阅以下部分之一：

- [Oracle 源编辑器（“连接管理器”页）](#oracle-source-editor-connection-manager-page)
- [Oracle 源编辑器（“列”页）](#oracle-source-editor-columns-page)
- [Oracle 源编辑器（“错误输出”页）](#oracle-source-editor-error-output-page)

![Oracle 源](media/oracle-source.png)

“高级编辑器”对话框包含可以编程方式设置的属性。

打开 **“高级编辑器”** 对话框：

- 在 Integration Services 项目的“数据流”屏幕上，右键单击 Oracle 源，并选择“显示高级编辑器”。

若要详细了解可以在“高级编辑器”对话框中设置的属性，请参阅 [Oracle 源的自定义属性](#oracle-source-custom-properties)。

## <a name="oracle-source-editor-connection-manager-page"></a>Oracle 源编辑器（“连接管理器”页）

“Oracle 源编辑器”对话框的“连接管理器”页用于从数据库中选择 Oracle Database 作为源、表或视图。

**打开 Oracle 源编辑器的“连接管理器”页的具体步骤**

- 在 SQL Server Data Tools 中，打开包含 Oracle 源的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Oracle 源。
### <a name="options"></a>选项

**“ODBC 目标编辑器”**

从列表中选择现有连接管理器，或单击“新建”来新建 Oracle Connection Manager。

**新建**

单击 **“新建”** 。 此时，“Oracle Connection Manager 编辑器”对话框打开，可以在其中新建连接管理器。

**数据访问模式**

选择从源选择数据的方法。 选项显示在下表中：

|选项|说明|
|:-|:-|
|表或视图|从 Oracle 数据源中的表或视图检索数据。 选择此选项后，从“表名或视图名称”列表中选择可用的表或视图。|
|SQL 命令|使用 SQL 查询从 Oracle 数据源中检索数据。 选择此选项后，请使用下列方式之一输入查询： <br>在 **“SQL 命令文本”** 字段中，输入 SQL 查询的文本。 <br>单击 **“浏览”** 从文本文件加载 SQL 查询。 <br>单击 **“分析查询”** 验证查询文本的语法。|

**预览**

单击 **“预览”** ，查看从选定的表或视图中提取的最多前 200 行数据。

## <a name="oracle-source-editor-columns-page"></a>Oracle 源编辑器（“列”页）

“Oracle 源编辑器”对话框的“列”页用于将输出列映射到每个外部（源）列。

**打开 Oracle 源编辑器的“列”页的具体步骤**

- 在 SQL Server Data Tools 中，打开包含 Oracle 源的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Oracle 源。

- 单击“Oracle 源编辑器”中的“列”。

### <a name="options"></a>选项

**可用外部列**

可以选择的可用外部列的列表，以按选择的顺序添加到“外部列”列表中。 此表不能用于添加或删除列。

选中“全选”复选框可以选择所有列。

**外部列**

你选择的外部（源）列按顺序列出。
若要更改顺序，请先清除“可用外部列”**列表，再选择顺序不同的一个或多个列。

**输出列**

选定外部（源）列的名称是默认输出名称。 不过，你可以输入任何唯一名称。
> [!NOTE]
>
>如果有数据类型不受支持的列，系统会显示指明数据类型不受支持的警告，并且会从映射列中删除相关列。

## <a name="oracle-source-editor-error-output-page"></a>Oracle 源编辑器（“错误输出”页）

使用“Oracle 源编辑器”对话框的“错误输出”页，可以选择错误处理选项。

**打开 Oracle 源编辑器的“错误输出”页的具体步骤**

- 在 SQL Server Data Tools 中，打开包含 Oracle 源的 SQL Server Integration Services (SSIS) 包。

- 在“数据流”选项卡上，双击 Oracle 源。

- 单击“Oracle 源编辑器”中的“错误输出”。

### <a name="options"></a>选项

**错误行为**

选择 Oracle 源应如何处理流中的错误：忽略失败、重定向行或让组件失败。
**相关部分**：[数据中的错误处理](./error-handling-in-data.md)

**截断**

选择 Oracle 源应如何处理流中的截断：忽略失败、重定向行或让组件失败。

## <a name="next-steps"></a>后续步骤

- 配置 [Oracle 目标](oracle-destination.md)。
- 若有任何疑问，请访问 [TechCommunity](https://aka.ms/AA5u35j)。
