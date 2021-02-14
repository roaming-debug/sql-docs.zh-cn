---
title: 数据虚拟化扩展
description: Azure Data Studio 的数据虚拟化扩展
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: rajmera3
ms.author: raajmera
ms.reviewer: alayu, sstein, maghan
ms.custom: ''
ms.date: 11/04/2019
ms.openlocfilehash: a428322867df8585160a3363d174b2cd92bd1f17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052048"
---
# <a name="data-virtualization-extension-for-azure-data-studio"></a>Azure Data Studio 的数据虚拟化扩展

Azure Data Studio 的数据虚拟化扩展为 [ODBC 数据源的外部表向导](../../relational-databases/polybase/data-virtualization.md)提供支持。

## <a name="install-the-data-virtualization-extension"></a>安装数据虚拟化扩展

若要安装数据虚拟化扩展，请访问[扩展 Azure Data Studio 的功能](./add-extensions.md)。

## <a name="changes-in-release-10"></a>版本 1.0 中的更改

* 扩展重命名为数据虚拟化。
* “创建外部表”向导：
  * 包含了适用于虚拟化 MongoDB 和 Teradata 源的引导式笔记本。
  * 添加了用于填写 MongoDB 和 Teradata 虚拟化笔记本中的变量的对话框。

## <a name="changes-in-release-016"></a>版本 0.16 中的更改

* “创建外部表”向导：
  * 改进了在对象映射页上加载表和视图时的错误处理。

## <a name="changes-in-release-015"></a>版本 0.15 变化

* “创建外部表”向导：
  *  缩短了加载“对象映射”页面上的表和列信息所需的时间。
  * 修复了在“连接详细信息”页上加载现有数据库范围凭据时出现的 bug。
* “从 CSV 文件创建外部表”向导：
  * 增加了用于 PROSE 分析的默认样本大小。

## <a name="changes-in-release-0141"></a>版本 0.14.1 中的更改

* 支持 CTP 3.1 数据源支持

## <a name="changes-in-release-0121"></a>版本 0.12.1 中的更改

* 此版本中已删除 SQL Server 大数据群集连接类型。 之前 SQL Server 大数据群集连接提供的所有功能现在都可以在 SQL Server 连接中获取。
* 可以在“数据服务”文件夹下找到 HDFS 浏览
* 对于笔记本，PySpark 和其他大数据内核在连接到 SQL Server 大数据群集中的 SQL Server 主实例时可以正常工作。
* “创建外部表”向导：
  * 支持使用现有外部数据源创建外部表。
  * 对整个向导进行了性能改进。
  * 改进了对具有特殊字符的对象名称的处理效果。 在某些情况下，这些字符曾导致向导发生故障
  * 增强了“对象映射”页的可靠性。
  * 从数据库下拉列表中删除了系统数据库 - `DWConfiguration`、`DWDiagnostics`、`DWQueue`。
  * 支持在“从 CSV 文件创建外部表”向导中设置外部文件格式对象的名称。
  * 在“从 CSV 文件创建外部表”向导的第一页中添加了一个刷新按钮。

## <a name="release-notes-v0110"></a>发行说明 (v 0.11.0)

* Jupyter Notebook 支持，特别是对 Python3 和 Spark 内核的支持，已转移到 Azure Data Studio 中。 使用 Notebook 时不再需要此扩展。
* 修复了外部数据向导中的多个 Bug：
  * Oracle 类型映射已更新，以匹配 SQL Server 2019 CTP 2.3 中的更改。
  * 修复了在表映射控件中键入的新架构会丢失的问题。
  * 修复了检查表映射中的数据库节点时未检查全部表和视图的问题。

## <a name="release-notes-v0102"></a>发行说明 (v 0.10.2)

### <a name="sql-server-2019-support"></a>SQL Server 2019 支持

对 SQL Server 2019 的支持已更新。 在连接到 SQL Server 大数据群集实例后，资源管理器树中会显示一个新的“数据服务”文件夹。 该文件夹包含一些操作的启动点，这些操作包括针对连接打开新 Notebook、提交 Spark 作业以及使用 HDFS 等。 对于某些操作（例如通过 HDFS 文件/文件夹创建外部数据），必须安装 SQL Server 2019 扩展 。

### <a name="notebook-support"></a>Notebook 支持

我们对笔记本用户界面进行了较大程度的更新。 我们的工作重点是让你能够轻松读取别人与你共享的笔记本。 这意味着删除单元格周围的所有边框（已选中或鼠标悬停的单元格除外）、添加悬停支持以便于无需选择单元格即可轻松实施单元格级操作、通过添加执行计数和一个动态“停止运行”按钮等内容来明确执行状态。 我们还为后列操作添加了键盘快捷方式：新建笔记本 (`Ctrl+Shift+N`)、运行单元 (`F5`)、新代码单元 (`Ctrl+Shift+C`)、新文本单元 (`Ctrl+Shift+T`)   。 我们的目标是让你能够通过快捷方式实现所有关键操作，所以请告诉我们还缺少哪些吧！

其他改进和修复包括：

* SQL Server 2019 扩展现会提示用户为 Python 依赖项选取安装目录。 它也不再在 `.vsix file` 中包含 Python，从而减少了整体扩展大小。 Python 依赖项支持 Spark 和 Python3 内核。
* 添加了对“从命令行启动新笔记本”的支持。 使用参数 `--command=notebook.command.new --server=myservername` 启动时应会打开一个新笔记本并连接到此服务器。
* 对单元格中代码长度较长的笔记本进行了性能修复。 如果代码单元格超过 250 行，则会添加一个滚动条。
* 改进了 `.ipynb` 文件支持。 现在支持版本 3 或更高版本。 

    >[!Note]
    > 保存文件更新到版本 4 或更高版本。

* 因为内置的 Notebook 查看器稳定，所以现在删除了 `notebook.enabled` 用户设置。
* 现在支持高对比度主题，并在此前提下对对象布局进行了一些修复。
* 修复了 #3680 问题：输出有时会错误地显示一些 `,,,` 字符。
* 修复了#3602 问题：编辑器在离开 Azure Data Studio 后消失。
* 添加了支持：为 `application/vnd.dataresource+json` 输出 MIME 类型使用“网格”视图。 这意味着许多使用此选项的笔记本（例如通过在 Python 笔记本中设置 `pd.options.display.html.table_schema`）会具有更美观的表格输出。

#### <a name="known-issues"></a>已知问题

* 打开 Notebook 时，会出现“安装 python”对话框。 取消此安装后，“内核”和“附加到”下拉列表不会显示预期值。 暂时的解决方法是完成 Python 安装。
* 使用不受支持的内核打开笔记本时，“内核”和“附加到”下拉列表会导致 Azure Data Studio 停止响应。 关闭 Azure Data Studio，并确保使用受支持的内核（Python3、Spark | R、Spark | Scala、PySpark、PySpark3）。
* 当对 SQL Server 终结点使用 PySpark3 或其他 Spark 内核时，Spark UI 链接失败。 一个暂时的解决方法是，在仪表板中选择“Spark UI”，或使用 SQL Server 大数据群集连接类型进行连接，因为这会得到正确的 Spark UI 超链接。

### <a name="extensibility-improvements"></a>扩展性改进

此版本中添加了许多可优化扩展程序控件的改进。

* 通过一个新的 `ObjectExplorerNodeProvider` API，扩展能够在 SQL Server 或其他连接节点下贡献文件夹。 这是在 SQL Server 2019 实例下添加 `Data Services` 节点的方式，但可以用于将“监视”文件夹或其他文件夹轻松添加到 UI。
* 有两个新的上下文键值可用于帮助显示/隐藏对仪表板的贡献。
  * `mssql:iscluster` 指示这是否是 SQL Server 2019 大数据群集
  * `mssql:servermajorversion` 指示服务器版本（15 为 SQL Server 2019，14 为 SQL Server 2017，依此类推）。 如果只显示（例如）SQL Server 2017 或更高版本的功能，可以使用这个键。

## <a name="release-notes-v080"></a>发行说明 (v 0.8.0)

*Notebooks*：

* 现在是通过选择“更多操作”单元格按钮来支持在现有单元格之前/之后添加单元格
* 已向“附加到”下拉列表中的连接中添加了“添加新连接”选项
* 添加了“重新安装 Notebook 依赖项”命令，用于协助 Python 程序包更新，同时，可通过关闭应用程序解决安装过程中途停止的问题。 该命令可通过命令面板运行（使用 `Ctrl/Cmd+Shift+P` 并键入 `Reinstall Notebook Dependencies`）
* PROSE python 包已更新到 1.1.0 并修复了许多 Bug。 使用“重新安装 Notebook 依赖项”命令更新此包
* 现在，选择“更多操作”单元格按钮，可使用“清除输出”命令 
* 修复了客户报告的以下问题：
  * 由于 PATH 问题，笔记本会话无法在 Windows 上启动
  * 无法从驱动器的根文件夹启动 Notebook，例如 C:\ 或 D:\
  * [#2820](https://github.com/Microsoft/azuredatastudio/issues/2820) 无法在 VS Code 中编辑从 ADS 创建的笔记本
  * 现在，运行 Spark 内核时，Spark UI 链接可正常运行
  * 将“托管包”重命名为“安装包”

*创建外部数据*：

* 错误消息可以复制，并已拆分为摘要和详细视图，以便于查看
* 改进了 UI 布局，还改进了可靠性和错误处理
* 修复了客户报告的以下问题：
  * 具有无效列映射的表显示为“禁用”，并会显示一个说明错误的警告

## <a name="release-notes-v072"></a>发行说明 (v 0.7.2)

* Azure 资源浏览器现已内置于 Azure Data Studio 中，并已从此扩展中删除。 非常感谢你提供反馈！
* 改进了具有许多 Markdown 单元格的笔记本的性能。
* 在 Notebook 中自动调整代码单元格的大小。 该单元格仍具有基于单元格工具栏的最小大小。
* 安装 Notebook 依赖项时通知用户。 特别是在 Windows 上，这可能需要很长时间，因此通知现在显示在“任务”视图中。
* 支持重新安装 Notebook 依赖项。 如果用户之前在安装途中关闭了 Azure Data Studio，此功能会很有用。
* 支持在 Notebook 中取消单元格执行。
* 提高了使用“创建外部数据”向导时的可靠性，特别是在发生连接错误时。
* 如果未在目标服务器中启用或运行 PolyBase，则阻止使用“创建外部数据”向导。
* 修复了与 SQL Server 2019 和创建外部数据相关的拼写和命名问题。
* 消除了 Azure Data Studio 调试控制台中的大量错误。