---
title: Azure Data Studio 发行说明
description: 本文提供了从 2017 年 11 月至今的 Azure Data Studio 版本的发行说明。 对于许多汇总问题，提供了指向其他详细信息的链接。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan
ms.custom: seodec18
ms.date: 11/12/2020
ms.openlocfilehash: b9bfda0507b81211feefb0fdb2505ae31f0c8c3b
ms.sourcegitcommit: 0c0e4ab90655dde3e34ebc08487493e621f25dda
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96442649"
---
# <a name="release-notes-for-azure-data-studio"></a>Azure Data Studio 发行说明

**[下载并安装最新版本！](./download-azure-data-studio.md)**

## <a name="november-2020"></a>2020 年 11 月

2020 年 11 月 12 日&nbsp; / &nbsp;版本：1.24.0

&nbsp;

| 更改 | 详细信息 |
| ------ | ------- |
| Bug 修复 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2020+Release%22+is%3Aclosed)。 |
| 连接对话框 | 为“连接”对话框添加了新的“浏览器”选项卡。 |
| 扩展更新 | 已发布 Postgres 扩展的更新。 |
| 新笔记本功能 | 向 SQL 笔记本支持添加了新功能。 <br/> 向笔记本参数化支持添加了新功能。 <br/>  向 SQL 笔记本的结果流式处理添加了新功能。 |
| Python 安装 | 已从默认 Python 安装中删除 PROSE 包。 |

### <a name="known-issues-1240"></a>已知问题 (1.24.0)

| 新项 | 详细信息 | 解决方法 |
|----------|---------|------------|
| Azure Arc 扩展 | [已知问题：](https://github.com/microsoft/azuredatastudio/issues/13319)Arc MIAA & PG 部署的“将脚本保存到笔记本”按钮在编写笔记本脚本之前不会执行字段验证。 这意味着，如果用户在密码确认输入中输入了错误的密码，则可能最终会得到一个具有错误密码值的笔记本。| “部署”按钮按预期工作，因此用户应改为使用该按钮。 |
| “对象资源管理器” | 由于引擎发生了与 [Azure Synapse Analytics 无服务器 SQL 池](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview)相关的更改，低于 1.24.0 的 ADS 版本的对象资源管理器有中断性变更。 | 若要继续将 Azure Data Studio 中的对象资源管理器与 Azure Synapse Analytics 无服务器 SQL 池结合使用，你需要 Azure Data Studio 1.24.0 或更高版本。 |

可参考 [Azure Data Studio 反馈](https://github.com/microsoft/azuredatastudio)了解其他已知问题，并向产品团队提供反馈。

## <a name="october-2020"></a>2020 年 10 月

2020 年 10 月 14 日&nbsp; / &nbsp;版本：1.23.0

&nbsp;

| 更改 | 详细信息 |
| ------ | ------- |
| Azure SQL Edge | 支持 Azure SQL Edge 对象。 |
| Bug 修复 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is:issue+milestone:%22October+2020+Release%22+is:closed)。 |
| 数据库| 支持相同的数据库引用。 |
| 扩展更新 | [Azure Arc](extensions/azure-arc-extension.md)</br>[azdata](../azdata/install/deploy-install-azdata.md)</br>[机器学习](extensions/machine-learning-extension.md)</br>[Kusto (KQL)](extensions/kusto-extension.md)</br>[架构比较](extensions/schema-compare-extension.md)</br>SQL 评估</br>[SQL 数据库项目](extensions/sql-database-project-extension.md)</br>[SQL Server 导入](extensions/sql-server-import-extension.md) |
| 新增部署功能 | 添加了 Azure SQL DB 和 VM 部署。 |
| PowerShell | 添加了 PowerShell 内核结果流式处理支持。 |

## <a name="september-2020-hotfix"></a>2020 年 9 月（修补程序）

2020 年 9 月 30 日 &nbsp; / &nbsp; 版本：1.22.1

&nbsp;

| 更改 | 详细信息 |
| ------ | ------- |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/releases/tag/1.22.1)。 |

## <a name="september-2020"></a>2020 年 9 月

2020 年 9 月 22 日 &nbsp; / &nbsp; 版本：1.22.0

&nbsp;

| 更改 | 详细信息 |
| ------ | ------- |
| 新笔记本功能 | <br/> &bull; &nbsp; 支持基于富文本格式和无缝转换到 markdown（也称为 WYSIWYG 工具栏，即“所见即所得”）的全新文本单元格编辑体验 <br/> &bull; &nbsp; 支持 Kusto 内核 <br/> &bull; &nbsp; 支持固定笔记本 <br/> &bull; &nbsp; 添加了对 Jupyter Books 新版本的支持 <br/> &bull; &nbsp; 改进了 Jupyter 快捷方式 <br/> &bull; &nbsp; 提升了加载性能 |
| SQL 数据库项目扩展 | SQL 数据库项目扩展为 Azure Data Studio 提供基于项目的数据库开发。 在此预览版本中，可以从 Azure Data Studio 创建和发布 SQL 项目。 |
| Kusto (KQL) 扩展 | 为在 Azure 数据资源管理器中存储的大量实时流式处理数据的数据浏览和数据分析带来了 Kusto 的本机 Azure Data Studio 体验。 此预览版支持连接和浏览 Azure 数据资源管理器群集、编写 KQL 查询，以及通过 Kusto 内核创作笔记本。 |
| Azure Arc 扩展 | 用户可以通过 Azure Data Studio 试用 Azure Arc 公共预览版。 这包括： <br/> &bull; &nbsp; 部署数据控制器 <br/> &bull; &nbsp; 部署 Postgres <br/> &bull; &nbsp; 部署 Azure Arc 的托管实例 <br/> &bull; &nbsp; 连接到数据控制器 <br/> &bull; &nbsp; 访问数据服务仪表板 <br/> &bull; &nbsp; Azure Arc Jupyter 书籍 |
| 部署选项 | <br/> &bull; &nbsp; Azure SQL 数据库边缘 <br/> （边缘需要 Azure SQL Edge 部署扩展） |
| SQL Server 导入扩展正式发布版 | 宣布推出 SQL Server 导入扩展的正式发布版，功能不再处于预览阶段。 此扩展有助于导入 csv/txt 文件。 有关此扩展的详细信息，请参阅[这篇文章](./extensions/sql-server-import-extension.md)。 |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2020+Release%22+is%3Aclosed)。 |

## <a name="august-2020"></a>2020 年 8 月

2020 年 8 月 12 日 &nbsp; / &nbsp; 版本：1.21.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 新笔记本功能 | &bull; &nbsp; 移动单元格位置 <br/> &bull; &nbsp; 将单元格转换为文本单元格或代码单元格
| Jupyter Books 选取器 | 用户现在可以从 Github 版本选择 Jupyter Books，并在 Azure Data Studio 中无缝打开 |
| 已将搜索添加到笔记本 Viewlet | 用户可以通过笔记本和 Jupyter Books 轻松搜索内容 |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22August+2020+Release%22+is%3Aclosed)。 |
| &nbsp; | &nbsp; |

## <a name="july-2020-hotfix"></a>2020 年 7 月（修补程序）

2020 年 7 月 17 日 &nbsp; / &nbsp; 版本：1.20.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修补了 bug #11372“对象资源管理器拖放表不正确地包装了表名” | [#11372](https://github.com/microsoft/azuredatastudio/issues/11372) |
| 修补了 bug #11356“深色主题现为默认主题” | [#11356](https://github.com/microsoft/azuredatastudio/issues/11356) |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>已知问题

- 某些用户已报告此版本中包含的新 Microsoft.Data.SqlClient v2.0.0 的连接错误。 用户已[按照以下说明](https://github.com/microsoft/azuredatastudio/issues/11367#issuecomment-659614111)成功连接

## <a name="july-2020"></a>2020 年 7 月

2020 年 7 月 15 日 &nbsp; / &nbsp; 版本：1.20.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 新增了功能教程 | 用户现可从欢迎页面和命令面板中启动一个功能教程，查看“连接 viewlet”、“笔记本 viewlet”和“扩展市场”等常用功能的演练 |
| 新笔记本功能 | &bull; &nbsp; Markdown 工具栏中的标头支持<br/> &bull; &nbsp; 文本单元格中的并排 Markdown 预览
| 在查询编辑器中拖放列和表 | 用户现可将连接 viewlet 中的列和表直接拖放到查询编辑器中 |
| 在活动栏中添加了 Azure 帐户图标 | 用户现可轻松了解在何处登录到 Azure |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22July+2020+Release%22+is%3Aclosed)。 |
| &nbsp; | &nbsp; |

## <a name="june-2020"></a>2020 年 6 月

2020 年 6 月 15 日&nbsp; / &nbsp;版本：1.19.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 向 Azure 门户集成添加了 Azure Data Studio | 用户现可通过 Azure SQL 数据库连接和 Azure Postgres 等途径直接转到 Azure 门户。 |
| 新笔记本功能 | &bull; &nbsp; 新建笔记本工具栏 <br/> &bull; &nbsp; 新建编辑单元格工具栏 <br/> &bull; &nbsp; Python 依赖项向导 UX 更新 <br/> &bull; &nbsp; 缩减了笔记本之间的间距 |
| 发布 SQL 评估 API 扩展 | 此扩展在 ADS 中添加了 SQL Server 的最佳做法评估。 它公开了 SQL 评估 API（以前仅在 PowerShell SqlServer 模块和 SMO 中可用），让你可评估 SQL Server 实例，并接收 SQL Server 团队对它们的建议。 在[本文](../tools/sql-assessment-api/sql-assessment-api-overview.md)中了解 SQL 评估 API 及其功能的详细信息。 |
| [机器学习扩展改进](./extensions/machine-learning-extension.md) | 现在支持 Azure SQL 托管实例。 |
| 数据虚拟化扩展改进 | 现在支持 MongoDB 和 Teradata |
| Postgres 扩展 bug 修复 | 已修复 Azure MFA |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2020+Release%22+is%3Aclosed)。 |
| &nbsp; | &nbsp; |

## <a name="may-2020-hotfix"></a>2020 年 5 月（修补程序）

2020 年 5 月 27 日&nbsp; / &nbsp;版本：1.18.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修复 bug #10538“运行当前查询”keybind 不再按预期方式运行 | [#10538](https://github.com/microsoft/azuredatastudio/issues/10538)  |
| 修复 bug #10537“无法打开 v1.18 上新的或现有的 sql 文件” | [#10537](https://github.com/microsoft/azuredatastudio/issues/10537)  |
| &nbsp; | &nbsp; |

## <a name="may-2020"></a>2020 年 5 月

2020 年 5 月 20 日&nbsp; / &nbsp;版本：1.18.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 发布 Redgate SQL Prompt 扩展 | 此扩展使你能够直接在 Azure Data Studio 中管理格式设置样式，因此无需离开 IDE 即可创建和编辑样式。 |
| 发布机器学习扩展 | 此扩展使你能够执行以下操作： <br/> &bull; &nbsp; 通过 Azure Data Studio 使用 SQL Server 机器学习服务管理 Python 和 R 包。<br/> &bull; &nbsp; 使用 ONNX 模型在 Azure SQL Edge 中进行预测。<br/> &bull; &nbsp; 查看 Azure SQL Edge 数据库中的 ONNX 模型。 <br/> &bull; &nbsp; 将文件或 Azure 机器学习中的 ONNX 模型导入到 Azure SQL Edge 数据库。 <br/> &bull; &nbsp; 创建笔记本以运行试验。 |
| 新笔记本功能 | &bull; &nbsp; 添加了新的 Python 依赖项向导，可更轻松地安装 Python 依赖项 <br/> &bull; &nbsp; 添加了 Markdown 工具栏的下划线支持 |
| Always Encrypted 参数化 | 允许运行通过加密数据库列进行插入、更新或筛选的查询。|
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22May+2020+Release%22+is%3Aclosed)。 |
| &nbsp; | &nbsp; |

## <a name="april-2020-hotfix"></a>2020 年 4 月（修补程序）

2020 年 4 月 30 日 &nbsp; / &nbsp; 版本：1.17.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修复 bug #10197 无法通过 MFA 进行连接 | [#10197](https://github.com/microsoft/azuredatastudio/issues/10197)  |
| &nbsp; | &nbsp; |

## <a name="april-2020"></a>2020 年 4 月

2020 年 4 月 27 日 &nbsp; / &nbsp; 版本：1.17.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 改进了欢迎页 | 欢迎页上进行了 UI 更新，可更轻松地查看常见操作和突出显示扩展。 |
| 新笔记本功能 | &bull; &nbsp; 添加了编辑文本单元格时的 Markdown 工具栏，可帮助使用 Markdown 进行编写 <br/> &bull; &nbsp; 改进了 Jupyter Books viewlet，使之成为笔记本 viewlet，可在其中同时管理 Jupyter Books 和笔记本 <br/>&bull; &nbsp; 添加了对在保存笔记本时保持图表的支持 <br/> &bull; &nbsp; 在 Python 笔记本中添加了对 KQL magic 的支持|
| 改进了仪表板 | 整个 Azure Data Studio 的仪表板使用最新设计模式（包括操作工具栏）进行了更新。 这也适用于许多扩展。 |
| Azure 视图中的 Azure Cloud Shell 集成。 | |
| 对 Always Encrypted 和具有安全 Enclave 的 Always Encrypted 的支持。 | |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22)。 |
| &nbsp; | &nbsp; |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22April+2020+Release%22)。 |
| &nbsp; | &nbsp; |

## <a name="march-2020"></a>2020 年 3 月

2020 年 3月 18 日&nbsp; / &nbsp;版本：1.16.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 在 SQL 笔记本中添加了图表支持 | 在代码单元中运行 SQL 查询时，用户现在可以创建和保存图表。 |
| 添加了创建 Jupyter 书籍体验 | 用户现在可以使用笔记本创建自己的 Jupyter 书籍。 |
| 添加了对 Postgres 扩展的 Azure AD 支持 | |
| 修复了许多辅助功能 bug | [辅助功能错误列表](https://github.com/microsoft/azuredatastudio/issues?page=1&q=is%3Aissue+is%3Aclosed+milestone%3A%22S360+-+Accessibility%22+label%3AA11y_AzureDataStudio) |
| VS Code 合并到 1.42 | 此版本包含对前 3 个 VS Code 版本中 VS Code 的更新。 [阅读其发行说明](https://code.visualstudio.com/updates/v1_42)以了解详细信息。 |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22March+2020%22+is%3Aclosed)。 |
| &nbsp; | &nbsp; |

## <a name="february-hotfix"></a>2 月（修补程序）

2020 年 2 月 19 日&nbsp; / &nbsp;版本：1.15.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修复 Bug #9149 显示活动连接 | [#9149](https://github.com/microsoft/azuredatastudio/issues/9149)  |
| 修复 Bug #9061 显示或隐藏 SQL 窗格时编辑数据网格不会正确调整大小 | [#9061](https://github.com/microsoft/azuredatastudio/issues/9061)  |
| &nbsp; | &nbsp; |

## <a name="february-2020"></a>2020 年 2 月

2020 年 2 月 13 日&nbsp; / &nbsp;版本：1.15.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 新的 Azure 登录改进 | 增加了对 Azure 登录体验的改进，包括删除设备代码的复制/粘贴，以获得更流畅的连接体验。 |
| Notebook 中的“查找”支持 | 用户现在可以在笔记本内使用 Ctrl + F。 笔记本中的“查找”支持可逐行搜索代码和文本单元。 |
| 从版本 1.38 到 1.42 的 VS Code 合并 | 此版本包含对前 3 个 VS Code 版本中 VS Code 的更新。 [阅读其发行说明](https://code.visualstudio.com/updates/v1_42)以了解详细信息。 |
| 修复了许多用户报告的[“白/空白屏幕”](https://github.com/microsoft/azuredatastudio/issues/8775)问题。 | |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+is%3Aclosed+milestone%3A%22February+2020%22)。 |
| &nbsp; | &nbsp; |

### <a name="known-issue"></a>已知问题

- MacOS Catalina 上的用户需要右键单击 Azure Data Studio，然后单击“打开”。

## <a name="december-2019-hotfix"></a>2019 年 12 月（修补程序）

2019 年 12 月 26 日&nbsp; / &nbsp;版本：1.14.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修补了 bug #8747“OE Expansion 失败” | [#8747](https://github.com/microsoft/azuredatastudio/issues/8747)  |
| &nbsp; | &nbsp; |

## <a name="december-2019"></a>2019 年 12 月

2019 年 12 月 19 日&nbsp; / &nbsp;版本：1.14.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 更改了 Notebooks 中的“附加到连接”下拉列表，以仅列出当前的活动连接 | [#8129](https://github.com/microsoft/azuredatastudio/issues/8129) |
| 添加了 bigdatacluster.ignoreSslVerification 设置，以允许在连接到 BDC 时忽略 TLS/SSL 验证错误 | [#8582](https://github.com/microsoft/azuredatastudio/pull/8582) |
| 允许更改脱机查询编辑器的默认语言风格 | [#8419](https://github.com/microsoft/azuredatastudio/pull/8419) |
| 大数据群集/SQL 2019 功能的 GA 状态 | [#8269](https://github.com/microsoft/azuredatastudio/issues/8269) |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/milestone/44?closed=1)。 |
| &nbsp; | &nbsp; |


## <a name="november-2019-hotfix"></a>2019 年 11 月（修补程序）

2019 年 11 月 15 日&nbsp; / &nbsp;版本：1.13.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修复了 bug #8210“复制/粘贴结果顺序错乱” |  |
| &nbsp; | &nbsp; |

## <a name="november-2019"></a>2019 年 11 月

2019 年 11 月 4 日&nbsp; / &nbsp; 版本：1.13.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 新 SQL Server 2019 支持 | &bull; &nbsp; 使用 BDC 部署向导部署 SQL Server 2019 大数据群集 <br/>&bull; &nbsp; 使用控制器仪表板管理群集运行状况 <br/>&bull; &nbsp; 使用安全 ACL 对话框管理 HDFS 访问控制列表 <br/> &bull; &nbsp; 使用 HDFS 分层对话框添加装载 <br/> &bull; &nbsp; 使用内置 Jupyter Book、SQL Server 2019 指南进行排除故障 <br/> &bull; &nbsp; SQL vNext 扩展重命名为数据虚拟化扩展 <br/> &bull; &nbsp; 在外部表向导中添加了 Teradata 和 Mongo 支持|
| 新笔记本功能 | &bull; &nbsp; 宣布 Powershell 笔记本 <br/> &bull; &nbsp; 宣布可折叠代码单元 <br/>&bull; &nbsp; 笔记本性能改进 <br/> &bull; &nbsp; 请在[此处](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed+label%3A%22Area+-+Notebooks%22)查看完整的改进列表 |
| 宣布推出 Jupyter Book  | Jupyter Book 是按目录整理的笔记本和 Markdown 文件的集合。 |
| 新 SQL Server 部署向导  | 现在支持进行以下部署： <br/> &bull; &nbsp; Windows 上的 SQL Server 2019 <br/> &bull; &nbsp; Windows 上的 SQL Server 2017 <br/> &bull; &nbsp; Docker 上的 SQL Server 2019 <br/> &bull; &nbsp; Docker 上的 SQL Server 2017 |
| 宣布正式发布架构比较扩展| &bull; &nbsp; SQLCMD 模式 <br/> &bull; &nbsp; 本地化支持 <br/> &bull; &nbsp; 辅助功能修补程序 <br/> &bull; &nbsp; 安全 bug  |
| 宣布正式发布 SQL Server Dacpac 扩展| <br/> &bull; &nbsp; 本地化支持 <br/> &bull; &nbsp; 辅助功能修补程序 <br/> &bull; &nbsp; 安全 bug |
| 宣布推出 Visual Studio IntelliCode 扩展 | Visual Studio IntelliCode 现在支持 SQL，可提供更加智能的保留关键字建议。 |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22November+2019+Release%22+is%3Aclosed)。 |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix-2"></a>2019 年 10 月（修补程序 2）

2019 年 10 月11 日&nbsp; / &nbsp;版本：1.12.2

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 禁用在检查模式下自动启动 EH |  |
| &nbsp; | &nbsp; |

## <a name="october-2019-hotfix"></a>2019 年 10 月（修补程序）

2019 年 10 月 8 日&nbsp; / &nbsp;版本：1.12.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修复了笔记本中的引号和反斜杠问题，以便正确转义。 |  |
| &nbsp; | &nbsp; |

## <a name="october-2019"></a>2019 年 10 月

2019 年 10 月 2 日&nbsp; / &nbsp;版本：1.12.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| Query History 扩展的发布 | SQL History 扩展保存过去在 Azure Data Studio 会话中执行的所有查询，并按执行顺序列出它们。 用户可以查看打开查询、执行查询、删除查询、暂停查询历史记录或删除所有查询历史记录项。 |
| 新建复制/粘贴结果 | 我们添加了从结果网格复制/粘贴结果的其他方法。 |
| PowerShell 扩展更新 |  |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/milestone/42?closed=1)。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题

- 笔记本
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) 罕见情况，错误序列化笔记本时出现

## <a name="september-2019"></a>2019 年 9 月

2019 年 9 月 10 日 &nbsp; / &nbsp; 版本：1.11.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 启用 SQLCMD 模式 | 查询编辑器现支持切换 SQLCMD 模式，以将查询作为 SQLCMD 脚本写入和编辑 |
| Community 扩展插件：查询编辑器增强 | 查询编辑器增强是一种开放源代码扩展插件，旨在为经常编写查询的用户增强 Azure Data Studio 查询编辑器。 &bull; &nbsp; 将当前查询保存为代码片段 <br/>&bull; &nbsp; 使用 Ctrl+U 切换数据库 <br/> &bull; &nbsp; 通过模板新建查询 <br/> &bull; &nbsp; 请在[此处](https://github.com/dzsquared/query-editor-boost)查看完整的改进列表 |
| 笔记本改进 | &bull; &nbsp; 为支持大型笔记本文件而进行的性能改进 <br/> &bull; &nbsp; 请在[此处](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22September+2019+Release%22+label%3A%22Area%3A+Notebooks%22+is%3Aclosed)查看完整的改进列表 |
| Visual Studio Code 8 月发布合并 1.38 | 可在[此处](https://code.visualstudio.com/updates/v1_38)查看最新的改进内容。 |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1)。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题

- 笔记本
  - [7080](https://github.com/microsoft/azuredatastudio/issues/7080) 罕见情况，错误序列化笔记本时出现

## <a name="august-2019"></a>2019 年 8 月

2019 年 8 月 15 日&nbsp; / &nbsp;版本：1.10.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 发布 SandDance 1.3.1 扩展 | &bull; &nbsp; 智能图表检测 <br/>&bull; &nbsp; 3D 可视化效果 <br/> &bull; &nbsp; 数据筛选 |
| 笔记本改进 | &bull; &nbsp; 在行中添加代码或文本单元格 <br/>&bull; &nbsp; 新增了以下功能：右键单击 SQL 结果网格可以将结果另存为 CSV、JSON 等 <br/> &bull; &nbsp; 提升了笔记本加载性能，以便更快地加载 JSON <br/> &bull; &nbsp; 请在[此处](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+label%3A%22Area%3A+Notebooks%22+milestone%3A%22August+2019+Release%22+is%3Aclosed)查看完整的改进列表 |
| SQL Server 2019 支持 |  此版本中包括对其他 SQL Server 2019 大数据群集功能的支持，包括： <br/> &bull; &nbsp; 缩短了加载“对象映射”页面上的表和列信息所需的时间。 <br/> &bull; &nbsp; 修复了在“连接详细信息”页面上加载现有数据库范围凭据时出现的 bug。 <br/> &bull; &nbsp; 增加了用于 PROSE 分析的默认样本大小。 | 
| Dacpac 扩展现在支持 Azure AD | 
| Visual Studio Code 7 月发布合并 1.37 | 可在[此处](https://code.visualstudio.com/updates/v1_37)查看最新的改进内容。 |
| 解决了 Bug 和问题 | 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/milestone/39?closed=1)。 |
| &nbsp; | &nbsp; |

## <a name="july-2019"></a>2019 年 7 月

2019 年 7 月 11 日&nbsp; / &nbsp;版本：1.9.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 发布 SentryOne Plan Explorer 扩展 | 我们重要的 Microsoft 合作伙伴 SentryOne 将推出[适用于 Azure Data Studio 的 SentryOne Plan Explorer 扩展](https://www.sentryone.com/products/sentryone-plan-explorer-extension-azure-data-studio)。 <br> 这是一个免费扩展，可为 Azure Data Studio 中运行的查询提供强化的计划图表，具备优化的布局算法和直观的颜色编码，有助于快速识别可影响查询性能的资源消耗量最高的运算符。 若要详细了解该扩展，请查看[此处](https://sqlperformance.com/2019/07/sentryone/plan-explorer-extension-azure-data-studio)的 SentryOne 博客文章。 |
| 即将针对架构比较推出的新功能 | &bull; &nbsp; 架构比较文件支持 (.SCMP) <br/>&bull; &nbsp; 取消架构比较支持 <br/>&bull; &nbsp; 可在[此处](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed+)查看完整的更改内容|
| 笔记本改进 | &bull; &nbsp; Plotly Python 支持 <br/>&bull; &nbsp; 通过浏览器打开 Notebook <br/> &bull; &nbsp; Python 包管理对话框 <br/> &bull; &nbsp; 性能和 Markdown 增强功能 <br/> &bull; &nbsp; 键盘快捷方式更新 <br/>  &bull; &nbsp; 可在[此处](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+milestone%3A%22July+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22+)查看 bug 修复和次要功能 |
| SQL Server 2019 支持 |  此版本中包括对其他 SQL Server 2019 大数据群集功能的支持，包括： <br/> &bull; &nbsp; 管理仪表板中的服务终结点表，其中列出了群集中的所有关键服务。 <br/> &bull; &nbsp; 群集状态 Notebook 显示如何跨所有服务和 pod 查询群集状态以及对其进行故障排除。| 
| 提供更新的语言包| 扩展管理器商城中现提供 10 个语言包。 只需使用扩展商城搜索特定语言并进行安装。 安装所选语言后，Azure Data Studio 会提示重启以切换新的语言。 |
| SQL Server Profiler 更新 | SQL Server 配置文件扩展已更新，增加了很多新功能，包括： <br/> &bull; &nbsp; 按数据库名称进行筛选 <br/> &bull; &nbsp; 复制和粘贴支持 <br/> &bull; &nbsp; 保存/加载筛选器 <br/>可以在[此处](https://github.com/microsoft/azuredatastudio/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aclosed+milestone%3A%22July+2019+Release%22+label%3A%22Area%3A+SQL+Profiler%22+)查看 SQL Server Profiler 扩展的完整改进列表。  |
| Visual Studio Code 5 月发布合并 1.35 | 可在[此处](https://code.visualstudio.com/updates/v1_35)查看最新的改进内容。 |
| 解决了 Bug 和问题 | 在以前版本的 Azure Data Studio 中，若在从“连接”对话框进行连接时选择了某个用户数据库，则生成的对象资源管理器条目将完全限定为该单一数据库。 从此版本开始将更改该行为，对象资源管理器中也会显示服务器级别属性。 <br/> 若要查看完整的修复内容列表，请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/milestone/35?closed=1)。 |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>2019 年 6 月

2019 年 6 月 6 日&nbsp; / &nbsp;版本：1.8.0

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 发布中央管理服务器 (CMS) 扩展 | 中央管理服务器存储组织到一个或多个中央管理服务器组中的 SQL Server 实例列表。 用户可以连接到自己的现有 CMS 服务器并管理服务器，例如添加和删除服务器。 有关详细信息，请参阅[此处](../relational-databases/administer-multiple-servers-using-central-management-servers.md) |
| 发布适用于 Windows 的数据库管理工具扩展 | 此扩展在 Azure Data Studio 中启用了 SQL Server Management Studio 中两个最常用的体验。 用户可以右键单击很多不同的对象（例如数据库、表、列、视图等等）并选择“属性”以查看该对象的 SSMS 属性对话框。 此外，用户可以右键单击数据库并选择“生成脚本”以启动广为人知的 SSMS 生成脚本向导。 
| 架构比较改进 | &bull; &nbsp; 添加了排除/包含选项 <br/>&bull; &nbsp; “生成脚本”在脚本生成后打开脚本 <br/>&bull; &nbsp; 删除了双滚动条  <br/>&bull; &nbsp; 格式设置和布局改进 <br/>&bull; &nbsp; 可在[此处](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+label%3A%22Area%3A+Schema+Compare%22+is%3Aclosed)查看完整的更改内容|
| 将“消息”部分移动到专用选项卡 | 以前用户运行 SQL 查询时，结果和消息都在堆叠面板上。 现在它们会在面板中的单独选项卡上，例如在 SSMS 中。 |
| SQL 笔记本改进 | &bull; &nbsp; 用户现在可以选择在笔记本上使用自己的 Python 3 或 Anaconda 安装 <br/>&bull; &nbsp; 多重稳定性 + 适应性/外观修复 <br/> &bull; &nbsp; 请在[此处](https://github.com/microsoft/azuredatastudio/issues?q=is%3Aissue+milestone%3A%22June+2019+Release%22+is%3Aclosed+label%3A%22Area%3A+Notebooks%22)查看完整的改进列表|
| Visual Studio Code 4 月发布合并 1.34 | 可在[此处](https://code.visualstudio.com/updates/v1_34)查看最新的改进内容 |
| 解决了 Bug 和问题。 | 请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/milestone/32?closed=1)。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题

- 适用于 Windows 的数据库管理工具扩展
    - 无法从断开连接的服务器节点启动属性
    - 无法启动 Azure 服务器的属性
    - 并非所有对象都有属性对话框
    - 启动对话框需要很长时间
    - 在通过部分类型的连接（例如 Azure AD）启动服务器时出错
- 笔记本
    - [5838](https://github.com/microsoft/azuredatastudio/issues/5838) 允许用户对笔记本使用系统 Python
- 架构比较
    - [5804](https://github.com/microsoft/azuredatastudio/issues/5804) 架构比较任务显示不执行任何操作的默认取消上下文菜单

## <a name="may-2019"></a>2019 年 5 月

2019 年 5 月 8 日&nbsp; / &nbsp;版本：1.7.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 发布架构比较扩展 | 架构比较是 SQL Server Data Tools (SSDT) 中广为人知的功能，其主要用例是比较并直观地显示数据库和 .dacpac 文件之间的差异，然后执行操作使它们相同。 |
| 将“任务”视图移动到输出窗口 | 用户现在可以在“输出”窗口的“任务”视图中查看长时间运行的任务（如备份、还原和架构比较）的状态
| 添加了欢迎页 | &bull; &nbsp; 指向常见操作（如新建查询、新建文件、新建笔记本）的链接 <br/>&bull; &nbsp; 指向文档和 GitHub 的链接 |
| SQL 笔记本改进 | &bull; &nbsp; 用于呈现改进内容的 Markdown，包括对注释和表的优化支持 <br/>&bull; &nbsp; 对工具栏的可用性改进 <br/>&bull; &nbsp; 指向可信赖笔记本的 Markdown 链接不再需要 Cmd/Ctrl + 单击，可以直接单击 <br/>&bull; &nbsp; 在关闭笔记本后更好地清理 Jupyter 进程，并在同时启动多个笔记本时减少错误 <br/>&bull; &nbsp; 改进 SQL 笔记本的连接，确保在针对同一数据库运行 2 个笔记本时不会出错 <br/>&bull; &nbsp; 改进在单击工具栏中的“运行单元格”时，笔记本自动滚动到正在执行的单元格的情况 <br/>&bull; &nbsp; 常规稳定性和性能改进 |
| 解决了 Bug 和问题。 | 请参阅 [GitHub 上的 Bug 和问题](https://github.com/microsoft/azuredatastudio/milestone/31?closed=1)。 |
| &nbsp; | &nbsp; |

## <a name="april-2019"></a>2019 年 4 月

2019 年 4 月 18 日&nbsp; / &nbsp;版本：1.6.0 

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 将“服务器”选项卡重命名为“连接” | |
| 将 Azure 资源浏览器调整为“连接”下的 Azure viewlet | 用户现在可以在“连接”视图中通过 Azure viewlet 查看其 Azure SQL 实例，还可展开以查看每个服务器或数据库下的对象。|
| SQL 笔记本改进 | &bull; &nbsp; 在工具栏中添加了用于清除所有单元格输出的按钮 <br/>&bull; &nbsp; 在工具栏中添加了用于运行所有单元格的按钮 <br/>&bull; &nbsp; 固定连接名称，而不是“附加到”下拉列表中的服务器名称（如果已设置） <br/>&bull; &nbsp; 修复了在使用相对图像路径时 markdown 中不呈现图像的问题 <br/>&bull; &nbsp; 改进了笔记本网格中的功能 - 添加了双击自动调整列尺寸功能和改进了鼠标滑轮支持 <br/>&bull; &nbsp; 改进了通过笔记本安装 Python 时的错误处理和 Python 安装复原能力 <br/>&bull; &nbsp; 改进了选择笔记本单元时的“全选”功能 <br/>&bull; &nbsp; 改进了笔记本的连接，防止关闭笔记本和影响对象资源管理器连接 <br/>&bull; &nbsp; 改进了笔记本体验，在笔记本断开连接时向用户显示消息，并且需要连接才能运行单元<br/>&bull; &nbsp; 改进了对未保存笔记本的支持，使其在 ADS 再次启动时在 ADS 中解除冻结 |
| 解决了 Bug 和问题。 | 请参阅 [GitHub 上的 Bug 和问题](https://github.com/Microsoft/azuredatastudio/milestone/26?closed=1)。 |
| &nbsp; | &nbsp; |

## <a name="march-2019-hotfix"></a>2019 年 3 月（修补程序）

2019 年 3 月 22 日&nbsp; / &nbsp;版本：1.5.2 &nbsp; / &nbsp;修补程序版本

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修复了在 1.5.1 中发现的几个问题。 | 请参阅 [GitHub 上的 3 月修补程序版本](https://github.com/Microsoft/azuredatastudio/milestone/28)。<br/> <br/>&bull; &nbsp; 修复了以下问题：用户无法关闭从仪表板中的“打开 Notebook”任务打开的笔记本 <br/>&bull; &nbsp; 修复了以下问题：Notebook JSON 在保存后有多余的 } <br/>&bull; &nbsp; 修复了以下问题：笔记本网格不响应主题更改 <br/>&bull; &nbsp; 修复了以下问题：选项卡标题中显示完整的笔记本路径。 现在只显示文件名。 |
| &nbsp; | &nbsp; |

## <a name="march-2019"></a>2019 年 3 月

2019 年 3 月 18 日&nbsp; / &nbsp;版本：1.5.1

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 添加了 [Azure Data Studio 的 PostgreSQL 扩展](./extensions/postgres-extension.md) | 支持的功能： <br/>&bull; &nbsp; 连接对话框 <br/>&bull; &nbsp; 对象资源管理器 <br/>&bull; &nbsp; 查询编辑器 <br/>&bull; &nbsp; 制表 <br/>&bull; &nbsp; 仪表板 <br/>&bull; &nbsp; 代码片段 <br/>&bull; &nbsp; 编辑数据 <br/>&bull; &nbsp; 笔记本 |
| 添加了 SQL 笔记本 | 向内置笔记本查看器添加了 SQL 内核支持： <br/>&bull; &nbsp; 支持 T-SQL <br/>&bull; &nbsp; 支持 PGSQL |
| 添加了 PowerShell 扩展 | 通过 VS Code 引入了 [PowerShell 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)体验。  |
| 添加了 SQL Server dacpac 扩展  | 从 SQL Server 导入扩展中删除数据层应用程序向导并移至新的扩展中。  |
| 添加了 Community 扩展 QueryPlan.show | 添加了集成支持以可视化显示查询计划  |
| 更新了 SQL Server 2019 预览版扩展 | &bull; &nbsp; Jupyter Notebook 支持（特别是 Python3）和 Spark 内核已移动到核心 Azure Data Studio 工具中。 <br/>&bull; &nbsp; 修复了外部数据向导中的 bug |
| 解决了 Bug 和问题。 | 请参阅 [GitHub 上的 Bug 和问题](https://github.com/Microsoft/azuredatastudio/milestone/25?closed=1)。 |
| &nbsp; | &nbsp; |

### <a name="known-issues"></a>已知问题
- [#4427](https://github.com/Microsoft/azuredatastudio/issues/4427)：在内核准备好启动 Spark 之前单击“在单元上运行”会导致灾难性错误 解决方法：先等待内核加载，再运行单元
- [#4493](https://github.com/Microsoft/azuredatastudio/issues/4493)：从 SSMS 启动的 ADS 使用 SQL 身份验证，这会提示用户输入密码 解决方法：现在使用 Windows 身份验证。 
- [#4494](https://github.com/Microsoft/azuredatastudio/issues/4494)：无法安装 SQL 笔记本功能 <br/>
**解决方法：** 请执行 [此处](https://github.com/Microsoft/azuredatastudio/issues/4494#issuecomment-473043832)的解决步骤。 
- [#4503](https://github.com/Microsoft/azuredatastudio/issues/4503)：无法直接从下载文件夹 (Mac) 打开 Azure Data Studio <br />
**解决方法：** 解压缩应用后重启计算机。 将进行调查。 
- [#4539](https://github.com/Microsoft/azuredatastudio/issues/4539)：笔记本的“另存为”功能会丢失连接上下文 <br />
**解决方法：** 将在下一版本中修复。 
- [#4458](https://github.com/Microsoft/azuredatastudio/issues/4458)：如果使用了无效版本，则 Dacpac 提取会导致 SqlToolsService 出现故障 <br/>
**解决方法：** 重新启动 Azure Data Studio 并确保使用正确的版本。
- “新建笔记本”和“打开笔记本”图标丢失 <br/>
**解决方法：** 弃用旧连接类型。 建议连接到 SQL Server 终结点，然后将按预期获取所有操作（新建笔记本、Spark 作业）。 

## <a name="february-2019"></a>2019 年 2 月

2019 年 2 月 13 日&nbsp; / &nbsp;版本：1.4.5

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 添加了“SQL Server 的管理包”扩展包。 | 这样更便于安装 SQL Server 管理相关的扩展。 这包括：<br/>&bull; &nbsp; [SQL Server 代理](./extensions/sql-server-agent-extension.md)<br/>&bull; &nbsp; [SQL Server Profiler](./extensions/sql-server-profiler-extension.md)<br/>&bull; &nbsp; [SQL Server 导入](./extensions/sql-server-import-extension.md) |
| 在 Profiler 扩展中添加了筛选扩展事件支持。 | &nbsp; |
| 添加了“另存为 XML”功能，可将 T-SQL 结果保存为 XML。 | &nbsp; |
| 添加了数据层应用程序向导方面的改进。 | &bull; &nbsp; 添加了“生成脚本”按钮<br/>&bull; &nbsp; 添加了视图，用于在部署过程中针对潜在数据丢失发出警告。 |
| 更新 SQL Server 2019 预览版扩展。 | 请参阅[数据虚拟化扩展](./extensions/data-virtualization-extension.md)。 |
| 为长时间运行的查询默认启用结果流式处理。 | &nbsp; |
| 解决了 Bug 和问题。 | 请参阅 [GitHub 上的 Bug 和问题](https://github.com/Microsoft/azuredatastudio/milestone/23?closed=1)。 |
| &nbsp; | &nbsp; |

## <a name="january-2019-hotfix"></a>2019 年 1 月（修补程序）

2019 年 1 月 16 日&nbsp; / &nbsp;版本：1.3.9 &nbsp; / &nbsp;修补程序版本

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 修复了在 1.3.8 中发现的几个问题。 | 请参阅 [GitHub 上的 1 月修补程序版本](https://github.com/Microsoft/azuredatastudio/milestone/24?closed=1)。<br/><br/>有关详细信息，请参阅：<br/>&bull; &nbsp; [GitHub 上的更改日志](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md)。<br/>&bull; &nbsp; [GitHub 上的发布内容](https://github.com/Microsoft/azuredatastudio/releases)。 |
| &nbsp; | &nbsp; |

## <a name="january-2019"></a>2019 年 1 月

2019 年 1 月 9 日&nbsp; / &nbsp;版本：1.3.8

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 添加了适用于 Windows 的新用户安装程序。 | 不同于现有系统安装程序，新用户安装程序不需要管理员权限。 这也为非管理员用户提供了更轻松的升级体验。 |
| 添加了 Azure Active Directory 身份验证支持。 | &nbsp; |
| 宣布推出 Idera SQL DM Performance Insights（预览版）。 | &nbsp; |
| SQL Server 导入扩展中的数据层应用程序向导支持。 | &nbsp; |
| 更新到 SQL Server 2019 预览版扩展。 | 请参阅[数据虚拟化扩展](./extensions/data-virtualization-extension.md)。 |
| SQL Server Profiler 改进。 | &nbsp; |
| 大型查询的结果流式处理（预览版）。 | &nbsp; |
| Community 扩展：sp_executesql 到 sql 和新数据库。 | &nbsp; |
| 解决了 Bug 和问题。 | 请参阅 [GitHub 上的 Bug 和问题](https://github.com/Microsoft/azuredatastudio/milestone/19?closed=1)。 |
| &nbsp; | &nbsp; |

## <a name="november-2018"></a>2018 年 11 月

2018 年 11 月 6 日&nbsp; / &nbsp;版本：1.2.4

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 更新到 SQL Server 2019 预览版扩展。 | 请参阅[数据虚拟化扩展](./extensions/data-virtualization-extension.md)。 |
| 引入“粘贴计划”扩展。 | &nbsp; |
| 引入增强色查询扩展，包括 SSMS 编辑器主题。 | &nbsp; |
| 修复 SQL Server 代理、Profiler 和导入扩展中的问题。 | &nbsp; |
| 修复导致 macOS 上丢失非活动连接的 .NET Core 套接字 KeepAlive 问题。 | &nbsp; |
| 将 SQL 工具服务升级至 .NET Core 2.2 Preview 3（获得最终的 Azure AD 支持）。 | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-november-2018"></a>Bug 修复，2018 年 11 月

- 修复[问题 #2933](https://github.com/Microsoft/azuredatastudio/issues/2933)：断开与 Azure SQL 数据库的连接
- 修复[问题 #2914](https://github.com/Microsoft/azuredatastudio/issues/2914)：扩展 OE 数据库节点时的“无效参数”异常
- 修复[问题 #2935](https://github.com/Microsoft/azuredatastudio/pull/2935)：在查询结果中正确地显示多行消息
- 修复[问题 #2906](https://github.com/Microsoft/azuredatastudio/pull/2906)：修复表名称包含特殊字符时的“编辑数据”文档名称
- 修复[问题 #2929](https://github.com/Microsoft/azuredatastudio/issues/2929)：内置扩展更改日志，用于查看 VSCode 发行说明中的更改
- 修复[问题 #2719](https://github.com/Microsoft/azuredatastudio/issues/2719)：高对比度主题的图标增加至双倍/三倍
- 修复[问题 #3047](https://github.com/Microsoft/azuredatastudio/pull/3047)：添加用于连接到 SQL Server 的命令行接口
- 修复[问题 #3031](https://github.com/Microsoft/azuredatastudio/pull/3031)：添加查询计划主题支持

## <a name="october-2018"></a>2018 年 10 月

2018 年 10 月 29 日&nbsp; / &nbsp;版本：1.1.4

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 引入 Azure 资源浏览器以浏览 Azure SQL 数据库。 | &nbsp; |
| 改进对象资源管理器和查询编辑器的连接稳定性。 | &nbsp; |
| 改进 SQL 代理扩展。 | &nbsp; |
| 更新到 SQL Server 2019 预览版扩展。 | 请参阅[数据虚拟化扩展](./extensions/data-virtualization-extension.md)。 |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-october-2018"></a>Bug 修复，2018 年 10 月

- 修复[问题 #2717](https://github.com/Microsoft/azuredatastudio/issues/2717)：XML 列结果单击格式问题
- 修复[问题 #2993](https://github.com/Microsoft/azuredatastudio/issues/2993)：宽度的结果窗口不完整
- 修复[问题 #2999](https://github.com/Microsoft/azuredatastudio/issues/2999)：连接到 DB 时，无法在 Mac 上加载文件 System.Diagnostics.Tracing
- 修复[问题 #2851](https://github.com/Microsoft/azuredatastudio/issues/2851)：TimeSeries 图表未正确呈现
- 修复[问题 #2996](https://github.com/Microsoft/azuredatastudio/issues/2996)：由于会话突然更改而丢失临时表

有关详细信息，请参阅[更改日志](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md)和[发布内容](https://github.com/Microsoft/azuredatastudio/releases)。

## <a name="september-2018-ga-release"></a>2018 年 9 月（正式发布版）

2018 年 9 月 24 日&nbsp; / &nbsp;版本：1.0 &nbsp; / &nbsp;正式发布版

Azure Data Studio（以前为 SQL Operations Studio）的正式发布版。

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 对查询结果网格性能和 UX 进行改进，实现更高的结果集数量。 | &nbsp; |
| Visual Studio Code 源代码从 1.23 刷新至 1.26.1，带有网格布局和改进后的设置编辑器（预览版）。 | &nbsp; |
| 屏幕阅读器、键盘导航和高对比度的辅助功能改进。 | &nbsp; |
| 添加了 `Connection name` 选项，在“服务器”viewlet 中提供备选的显示名称。 | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="announcing-the-sql-server-2019-preview-extension"></a>宣布推出 SQL Server 2019 预览版扩展

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 对 SQL Server 2019 预览功能的支持，包括[大数据群集](../big-data-cluster/big-data-cluster-overview.md)支持。 | 连接到 SQL Server 2019 预览版随附的 HDFS/Spark 网关。<br/><br/>浏览 HDFS、上传文件、保存文件和启动有用的操作，例如适用于 CSV 文件的“在笔记本中分析”操作。<br/><br/>从仪表板提交 Spark 作业，或在对象资源管理器中右键单击 HDFS/Spark 连接。 |
| Azure Data Studio 笔记本。 | 使用集成的笔记本查看器创建或打开笔记本。 在此版本中，笔记本查看器仅支持连接到本地内核和 SQL Server 2019 大数据群集。<br/><br/>使用笔记本中的 PROSE 代码加速器库来了解文件格式和数据类型，以便快速完成数据准备。 |
| Azure 资源浏览器。 | 利用 Azure 资源浏览器视图，可以浏览 Azure 帐户的数据相关终结点，并在对象资源管理器中创建到它们的连接。 此版本支持 Azure SQL 数据库。 |
| SQL Server PolyBase 创建外部表向导。 | 使用易于使用的向导创建外部表及其支持元数据结构。 此版本支持远程 SQL Server 和 Oracle 服务器。 |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-september-2018"></a>Bug 修复，2018 年 9 月

- 修复[问题 #2647](https://github.com/Microsoft/azuredatastudio/issues/143)：图表后退一大步。
- 修复[问题 #2648](https://github.com/Microsoft/azuredatastudio/issues/143)：SELECT 对 JSON 超链接返回整列内容。

有关详细信息，请参阅[更改日志](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md)和[发布内容](https://github.com/Microsoft/azuredatastudio/releases)。

## <a name="august-2018"></a>2018 年 8 月

2018 年 8 月 30 日&nbsp; / &nbsp;版本：0.32.8&nbsp; / &nbsp;公共预览版

8 月发布的公共预览版侧重于 Bug 修复、产品稳定性以及填补现有方案中的空白。

0\.32.8 包含对 0.32.7 中发现的几个回归问题（[#1971](https://github.com/Microsoft/azuredatastudio/issues/1971) 和 [#2372](https://github.com/Microsoft/azuredatastudio/issues/2372)）的修复

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 宣布推出 SQL Server 导入扩展。 | &nbsp; |
| SQL Server Profiler 会话管理。 | &nbsp; |
| SQL Server Profiler 会话模板支持。 | &nbsp; |
| SQL Server 代理改进。 | &nbsp; |
| 新的 Community 扩展：第一个响应程序工具包。 | &nbsp; |
| 生存周期质量改进：连接字符串 | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-august-2018"></a>Bug 修复，2018 年 8 月

- 使用 `Parse Syntax` 命令分析查询编辑器窗口中的 SQL。
- 修复[问题 #143](https://github.com/Microsoft/azuredatastudio/issues/143)：双击时未选中变量名称中的“@”。
- 修复[问题 #387](https://github.com/Microsoft/azuredatastudio/issues/387)：SQL 选项卡 DB 图标为红色。
- 修复[问题 #825](https://github.com/Microsoft/azuredatastudio/issues/825)：请求：在编写某些对象的脚本后自动连接到当前服务器... 
- 修复[问题 #1278](https://github.com/Microsoft/azuredatastudio/issues/1278)：sqlops.desktop [桌面条目] - 名称和注释的冗余值。
- 修复[问题 #1285](https://github.com/Microsoft/azuredatastudio/issues/1285)：更新会导致 Windows 中的应用程序图标被删除/替换。
- 修复[问题 #1317](https://github.com/Microsoft/azuredatastudio/issues/1317)：修复小数点分隔符。
- 修复[问题 #1474](https://github.com/Microsoft/azuredatastudio/issues/1474)：取消更改连接会断开当前连接。
- 修复[问题 #1497](https://github.com/Microsoft/azuredatastudio/issues/1497)：“以图表形式查看”选项在底部被截断。
- 修复[问题 #1524](https://github.com/Microsoft/azuredatastudio/issues/1524)：Shell/仪表板：主 viewlet 图表可以被拖动，并且可能使应用出现故障。
- 修复[问题 #1578](https://github.com/Microsoft/azuredatastudio/issues/1578)：点击名称无法展开/折叠远程文件浏览器文件夹。
- 修复[问题 #1620](https://github.com/Microsoft/azuredatastudio/issues/1620)：功能建议：获取现有连接的连接字符串。
- 修复[问题 #1624](https://github.com/Microsoft/azuredatastudio/issues/1624)：SelectBox 在禁用时不会更改颜色。
- 修复[问题 #1728](https://github.com/Microsoft/azuredatastudio/issues/1728)：另存为 JSON/EXCEL/CSV 操作不起作用。
- 修复[问题 #1744](https://github.com/Microsoft/azuredatastudio/issues/1744)：在选项卡之间切换时，结果窗格中的滚动位置会丢失。
- 修复[问题 #1748](https://github.com/Microsoft/azuredatastudio/issues/1748)：第二次及后续保存 Excel 文件时出现错误消息。
- 修复[问题 #1782](https://github.com/Microsoft/azuredatastudio/issues/1782)：编辑数据：按 ESC 键时，单元格不会恢复原始值。
- 修复[问题 #1836](https://github.com/Microsoft/azuredatastudio/issues/1836)：.sql 文件未与 SQL Operations Studio 关联。
- 修复[问题 #1850](https://github.com/Microsoft/azuredatastudio/issues/1850)：键入“N''”时，系统会自动补充为“N'''”。
- 修复[问题 #1985](https://github.com/Microsoft/azuredatastudio/issues/1985)：从查询结果网格进行复制时，会有 1 列偏差。
- 修复[问题 #1998](https://github.com/Microsoft/azuredatastudio/pull/1998)：将 VS Code 版本添加到“关于”对话框。
- 修复[问题 #2042](https://github.com/Microsoft/azuredatastudio/pull/2042)：代理：启用用于从 sql 文件导入查询的按钮。
- 修复[问题 #2091](https://github.com/Microsoft/azuredatastudio/issues/2091)：无法使用 Ctrl + C 快捷方式从结果窗格中复制内容。
- 修复[问题 #2099](https://github.com/Microsoft/azuredatastudio/pull/2099)：添加了更多 saveAsCsv 选项。
- 修复[问题 #2107](https://github.com/Microsoft/azuredatastudio/issues/2107)：更新仪表板和 Profiler 文档的文档图标。
- 修复[问题 #2129](https://github.com/Microsoft/azuredatastudio/pull/2129)：切换选项卡时保存编辑数据滚动位置。
- 修复[问题 #2152](https://github.com/Microsoft/azuredatastudio/issues/2152)：从零开始的结果网格行指示器。

### <a name="known-issues-august-2018"></a>已知问题，2018 年 8 月

- [问题 #2371](https://github.com/Microsoft/azuredatastudio/issues/2371)：“另存为 Excel”仅保存第一行数据
- [问题 #2150](https://github.com/Microsoft/azuredatastudio/issues/2150)：无法在 Ubuntu 16.04 上连接容器中的 SQL

## <a name="july-2018"></a>2018 年 7 月

2018 年 7 月 19 日&nbsp; / &nbsp;版本：0.31.4&nbsp; / &nbsp;公共预览版

7 月公共预览版侧重于以下内容：

- SQL Server 代理配置方案的初始版本。
- SQL Server Profiler 会话和视图模板增强。
- 针对客户报告的 GitHub 问题的持续 bug 修复。

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| [适用于 SQL Operations Studio 的 SQL Server 代理扩展](./extensions/sql-server-agent-extension.md)改进。 | 在左侧窗格中添加了警报、运算符和代理的视图以及图标。<br/><br/>添加了“新建作业”、“新建作业步骤”、“新建警报”和“新建运算符”对话框。<br/><br/>添加了“删除作业”、“删除警报”和“删除运算符”（右键单击）。<br/><br/>添加了以前的运行可视化效果。<br/><br/>为每个列名称添加了筛选器。 |
| [适用于 SQL Operations Studio 的 SQL Server Profiler 扩展](./extensions/sql-server-profiler-extension.md)改进。 | 添加了 5 个默认模板，用于查看扩展事件。<br/><br/>添加了服务器/数据库连接名称。<br/><br/>添加了对 Azure SQL 数据库实例的支持。<br/><br/>添加了在关闭选项卡而 Profiler 依然在运行时的退出 Profiler 的建议。 |
| 发布组合脚本扩展。 | &nbsp; |
| 为扩展作者添加了向导和对话框扩展点。 | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="bug-fixes-july-2018"></a>Bug 修复，2018 年 7 月

- 修复[问题 728](https://github.com/Microsoft/azuredatastudio/issues/728)：macOS 上对“添加连接”无响应
- 修复[问题 1612](https://github.com/Microsoft/azuredatastudio/issues/1612)：结果网格文本显示因国际字符而出现混乱
- 修复[问题 1693](https://github.com/Microsoft/azuredatastudio/issues/1693)：备份对话框：文件浏览器的 UI 损坏
- 修复[问题 1713](https://github.com/Microsoft/azuredatastudio/issues/1713)：行数受到影响
- 修复[问题 1718](https://github.com/Microsoft/azuredatastudio/issues/1718)：无法连接到任何数据源
- 修复[问题 1719](https://github.com/Microsoft/azuredatastudio/issues/1719)：在连接服务器时出现 TypeError
- 修复[问题 1724](https://github.com/Microsoft/azuredatastudio/issues/1724)：扩展对话框停止工作
- 修复[问题 1749](https://github.com/Microsoft/azuredatastudio/issues/1749)：Bug：列中的 HTML 数据被解读
- 修复[问题 1789](https://github.com/Microsoft/azuredatastudio/issues/1789)：扩展性：若添加连接提供程序，卸载操作将永远不会从列表中删除它
- 修复[问题 1791](https://github.com/Microsoft/azuredatastudio/issues/1791)：Sqlops 扩展：queryeditor.connect() 已连接到目标数据库，但 UI 未显示已连接编辑器
- 修复[问题 1799](https://github.com/Microsoft/azuredatastudio/issues/1799)：“前 10 个数据库大小”图表不适用于区分大小写的实例
- 修复[问题 1814](https://github.com/Microsoft/azuredatastudio/issues/1814)：sqlops.d.ts 拼写错误导致隐式“any”类型定义
- 修复[问题 1817](https://github.com/Microsoft/azuredatastudio/issues/1817)：拼写错误
- 修复[问题 1830](https://github.com/Microsoft/azuredatastudio/issues/1830)：调用 component() 后，在 ButtonComponent 中设置 iconPath 没有更改图标
- 修复[问题 1843](https://github.com/Microsoft/azuredatastudio/issues/1843)：优化表组织

## <a name="june-2018"></a>2018 年 6 月

2018 年 6 月 20 日&nbsp; / &nbsp;版本：0.30.6&nbsp; / &nbsp;公共预览版

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 适用于 SQL Operations Studio 的 SQL Server Profiler 预览版扩展初始版本。 | &nbsp; |
| 新的 SQL 数据仓库扩展包含丰富的可自定义仪表板小组件，可为数据仓库提供见解。 | 这可以解锁关于数据仓库管理和优化的关键方案，以确保为获得一致的性能对其进行优化。 |
| 编辑数据“筛选和排序”支持。 | &nbsp; |
| 针对作业和作业历史记录视图，适用于 SQL Operations Studio 的 SQL Server 代理预览版扩展改进。 | &nbsp; |
| 改进了“向导和对话框 UI 生成器框架”扩展性 API。 | &nbsp; |
| 更新 VS Code 平台源代码。 | 集成了以下发布：<br/>&bull; &nbsp; [2018 年 3 月 (1.22)](https://code.visualstudio.com/updates/v1_22)<br/>&bull; &nbsp; [2018 年 4 月 (1.23)](https://code.visualstudio.com/updates/v1_23) |
| &nbsp; | &nbsp; |

### <a name="github-issues-fixes-june-2018"></a>GitHub 问题修复，2018 年 6月

- 功能请求（[问题 1204](https://github.com/Microsoft/azuredatastudio/issues/1204)）：将结果网格列宽设置为自动适应数据，并记住在重新运行相同的查询时手动更改内容。
- 修复[问题 1398](https://github.com/Microsoft/azuredatastudio/issues/1398)：链接的帐户为空时，应显示“添加消息”和“添加帐户”按钮。
- 修复[问题 1399](https://github.com/Microsoft/azuredatastudio/issues/1399)：当视图处于折叠状态时，链接的帐户选项卡会损坏。
- 修复[问题 1374](https://github.com/Microsoft/azuredatastudio/issues/1374)：从磁盘打开 .sql 文件时，SQL 工具服务会出现故障。
- 修复[问题 1372](https://github.com/Microsoft/azuredatastudio/issues/1372)：缺少 SQL 关键字“BETWEEN”。
- 修复[问题 1395](https://github.com/Microsoft/azuredatastudio/issues/1395)：“MATCH”关键字导致 SQL 工具服务出现故障。
- 修复[问题 1496](https://github.com/Microsoft/azuredatastudio/issues/1496)：对象资源管理器中的“新建 Profiler”上下文菜单选项不执行任何操作。
- 修复[问题 1495](https://github.com/Microsoft/azuredatastudio/issues/1495)：查询编辑器“说明”查询计划损坏。

## <a name="may-2018"></a>2018 年 5 月

2018 年 5 月 7 日&nbsp; / &nbsp;版本：0.29.3&nbsp; / &nbsp;公共预览版

5 月公共预览版侧重于稳定性和 Bug 修复。

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 宣布在扩展管理器中提供 Redgate SQL 搜索扩展。 | &nbsp; |
| 提供 10 种语言的 Community 本地化。 | 德语、西班牙语、法语、意大利语、日语、韩语、葡萄牙语、俄语、简体中文和繁体中文。 |
| 更改遥测集合。 | &bull; &nbsp; 减少了遥测集合。<br/>&bull; &nbsp; 改进了选择退出体验。<br/>&bull; &nbsp; 添加了指向隐私声明的产品内链接。 |
| 扩展管理器改善了商城体验。 | 可以更轻松地发现 Community 扩展。 |
| SQL 代理扩展。 | &bull; &nbsp; 作业。<br/>&bull; &nbsp; 作业历史记录视图改进。 |
| 更新 whoisactive 和服务器报表扩展。 | &nbsp; |
| 改进了管理仪表板属性的滚动。 | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues"></a>修复 GitHub 问题

- 修复[问题 703](https://github.com/Microsoft/azuredatastudio/issues/703)：在编辑数据中输入类似 HTML 的文本会导致在刷新之前错误地显示值
- 修复[问题 821](https://github.com/Microsoft/azuredatastudio/issues/821)：azuredatastudio.deb 包依赖项
- 修复[问题 1260](https://github.com/Microsoft/azuredatastudio/issues/1260)：关键字“distinct”未突出显示
- 修复[问题 1332](https://github.com/Microsoft/azuredatastudio/issues/1332)：编辑数据还原行不起作用
- 修复[问题 1215](https://github.com/Microsoft/azuredatastudio/issues/1215)：SQL 代理扩展和状态栏
- 修复[问题 1316](https://github.com/Microsoft/azuredatastudio/issues/1316)：更改窗口大小后，SQL 代理未调整大小

## <a name="april-2018"></a>2018 年 4 月

2018 年 4 月 25 日&nbsp; / &nbsp;版本：0.28.6&nbsp; / &nbsp;公共预览版

4 月公共预览版包含 Bug 修复和改进。

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 改进 SQL 代理预览版扩展： | &nbsp; |
| &nbsp; &nbsp; &nbsp; 改进了对文件的支持。 | &bull; &nbsp; 大型文件。<br/>&bull; &nbsp; 受保护的文件，可以保存受管理员保护的文件。<br/>&bull; &nbsp; 在 SQL Operations Studio 中存储 \>256 M 的文件。 |
| &nbsp; &nbsp; &nbsp; 集成终端拆分。 | 同时处理多个打开的终端。 |
| &nbsp; &nbsp; &nbsp; 加快安装和启动速度。 | 减少了磁盘上文件数页脚打印的安装。 |
| &nbsp; | &nbsp; |

### <a name="fix-github-issues-april-2018"></a>修复 GitHub 问题，2018 年 4 月

- 修复[问题 37](https://github.com/Microsoft/azuredatastudio/issues/37)：当图表查看器引发错误时，会发生意外行为。
- 修复[问题 462](https://github.com/Microsoft/azuredatastudio/issues/462)：功能请求：默认情况下要展开的服务器组的选项。
- 修复[问题 606](https://github.com/Microsoft/azuredatastudio/issues/606)：intellisense -“update”命令的不良建议。
- 修复[问题 967](https://github.com/Microsoft/azuredatastudio/issues/967)：在结果网格中选择 XML 显示计划时应出现查询计划。
- 修复[问题 1023](https://github.com/Microsoft/azuredatastudio/issues/1023)：为来自 flyfishingdba 的 ms_foreachdb 调用添加方括号。
- 修复[问题 1048](https://github.com/Microsoft/azuredatastudio/issues/1048)：预登陆 SSL/TLS 握手错误。
- 修复[问题 1050](https://github.com/Microsoft/azuredatastudio/issues/1050)：在显示错误之前清除见解视图。
- 修复[问题 1057](https://github.com/Microsoft/azuredatastudio/issues/1057)：资源管理器小组件中的还原和新建查询操作损坏。
- 修复[问题 1068](https://github.com/Microsoft/azuredatastudio/issues/1068)：仪表板输出窗口弹出 Azure SQL 数据库的错误消息。
- 修复[问题 1069](https://github.com/Microsoft/azuredatastudio/issues/1069)：“连接”对话框在首次显示时显示“服务器是必需项”错误。
- 修复[问题 1070](https://github.com/Microsoft/azuredatastudio/issues/1070)：现在需要双击才能展开服务器组。
- 修复[问题 1072](https://github.com/Microsoft/azuredatastudio/issues/1072)：选择控件背景为半透明。
- 修复[问题 1115](https://github.com/Microsoft/azuredatastudio/issues/1115)：修复 SQL Operations Studio 中的所有高对比度辅助功能问题。
- 修复[问题 1101](https://github.com/Microsoft/azuredatastudio/issues/1101)：扩展无法对转到错误地址的“手动下载”链接进行升级。
- 修复[问题 1103](https://github.com/Microsoft/azuredatastudio/issues/1103)：垂直滚动在“主页”选项卡上无效。
- 修复[问题 1104](https://github.com/Microsoft/azuredatastudio/issues/1104)：SQL 扩展选项卡停止工作。

### <a name="visual-studio-code-121-platform"></a>Visual Studio Code 1.21 平台

4 月公开预览版的一大重点就是刷新 Visual Studio Code 1.21 平台的源代码。 这在以前的 1.19 同步点基础上为核心编辑器和工作台带来多项更新。 下面是部分示例：

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| [新的通知 UI](https://code.visualstudio.com/updates/v1_21#_new-notifications-ui)。 | 轻松地管理和查看 SQL Operations Studio 通知。 |
| [集成终端拆分](https://code.visualstudio.com/updates/v1_21#_split-terminals)。 | 一次处理多个打开的终端。 |
| [保存大型文件和受保护的文件](https://code.visualstudio.com/updates/v1_20#_save-files-that-need-admin-privileges)。 | 在 SQL Operations Studio 中保存受管理员保护的文件以及 \>256 M 的文件。 |
| [改进的大型文件支持](https://code.visualstudio.com/updates/v1_21#_text-buffer-improvements)。 | 针对大型文件的文本缓冲区优化。 |
| [改进了设置搜索](https://code.visualstudio.com/updates/v1_20#_settings-search)。 | 通过自然语言搜索轻松找到正确的设置。 |
| [全局代码片段](https://code.visualstudio.com/updates/v1_20#_global-snippets)。 | 创建可跨所有文件类型使用的代码片段。 |
| [资源管理器中的多选](https://code.visualstudio.com/updates/v1_20#_multi-select-in-the-explorer)。 | 一次对多个文件执行操作。 |
| [资源管理器中的错误和警告](https://code.visualstudio.com/updates/v1_20#_error-indicators-in-the-explorer)。 | 快速导航到代码库中的错误。 |
| [跨窗口进行拖放、复制和粘贴](https://code.visualstudio.com/updates/v1_21#_better-drag-and-drop-support)。 | 在打开的 SQL Operations Studio 窗口间移动文件。 |
| [Git 子模块支持](https://code.visualstudio.com/updates/v1_20#_git-submodules)。 | 对嵌套的 Git 存储库执行 Git 操作。 |
| [终端屏幕阅读器支持](https://code.visualstudio.com/updates/v1_20#_screen-reader-support)。 | 集成终端现在具备“屏幕阅读器优化”模式。 |
| [居中式编辑器布局](https://code.visualstudio.com/updates/v1_21#_centered-editor-layout)。 | 将代码查看屏幕的空间最大化。 |
| [水平搜索结果（预览版）](https://code.visualstudio.com/updates/v1_21#_horizontal-search)。 | 现在可以在水平面板中查看搜索结果。 |
| &nbsp; | &nbsp; |

有关更多详细信息，请查看 [Visual Studio Code 2 月发行说明](https://code.visualstudio.com/updates/v1_21)以及 [Visual Studio Code 1 月发行说明](https://code.visualstudio.com/updates/v1_20)。

有关详细信息，请参阅[更改日志](https://github.com/Microsoft/azuredatastudio/blob/main/CHANGELOG.md)。

## <a name="march-2018"></a>2018 年 3月

2018 年 3月 28 日&nbsp; / &nbsp;版本：0.27.3&nbsp; / &nbsp;公共预览版

3 月公开预览版继续处理最重要的 GitHub 问题，并侧重于改进扩展性。 专门启用扩展管理器、改进仪表板管理并提供 SQL 代理和见解扩展。 此版本包括以下增强功能：

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 增强仪表板扩展性模型以支持选项卡式见解和配置窗格。 | 扩展管理器可以实现简单的扩展获取。<br/><br/>[whoisactive.com](http://www.whoisactive.com) 中的 sp\_whoisactive 的仪表板扩展。<br/><br/>有关详细信息，请参阅[扩展 SQL Operations Studio 的功能](./extensions/add-extensions.md)。 |
| 添加其他[适用于连接和对象资源管理器的扩展性 API](https://github.com/Microsoft/azuredatastudio/wiki/Extensibility-API) 管理。 | &nbsp; |
| 继续修复影响客户的重要 [GitHub 问题](https://github.com/Microsoft/azuredatastudio/issues)。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="february-2018"></a>2018 年 2 月

2018 年 2 月 15 日&nbsp; / &nbsp;版本：0.26.7&nbsp; / &nbsp;公共预览版

2 月公共预览版包括一些功能建议和高优先级的 Bug 修复。 此版本包括以下增强功能：

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 引入自动更新安装，在新版本可供下载时提供通知。 | &nbsp; |
| 连接对话框的“数据库”字段现在为动态填充的下拉列表，将包括从指定服务器填充的数据库列表。 | &nbsp; |
| 引入连接扩展性 API。 | &nbsp; |
| VS Code 编辑器 1.19 集成。 | &nbsp; |
| 更新 JustinPealing/html-query-plan 组件，以获取多项查询计划查看器改进。 | &nbsp; |
| &nbsp; | &nbsp; |

### <a name="fixed-issues-february-2018"></a>修复问题，2018 年 2月

- 修复[问题 6](https://github.com/Microsoft/azuredatastudio/issues/6)：打开新查询选项卡时保持连接和选定的数据库。
- 修复[问题 22](https://github.com/Microsoft/azuredatastudio/issues/22)：是否能将“服务器名称”和“数据库名称”改为下拉列表，而不是文本框？
- 修复[问题 549](https://github.com/Microsoft/azuredatastudio/issues/549)：无提示安装会在完成后打开应用程序。
- 修复[问题 481](https://github.com/Microsoft/azuredatastudio/issues/481)：添加“检查更新”选项。
- SQL 编辑器着色和自动完成修复：
  - 修复[问题 584](https://github.com/Microsoft/azuredatastudio/issues/584)：IntelliSense 未突出显示关键字“FULL”。
  - 修复[问题 345](https://github.com/Microsoft/azuredatastudio/issues/345)：在编辑器中为 SQL 函数着色。
  - 修复[问题 300](https://github.com/Microsoft/azuredatastudio/issues/300)：[#tempData] latest "]" 将显示绿色。
  - 修复[问题 225](https://github.com/Microsoft/azuredatastudio/issues/225)：关键字颜色不匹配。
  - 修复[问题 60](https://github.com/Microsoft/azuredatastudio/issues/60)：在 from 子句中使用临时表时，sql 语法颜色的突出显示设置无效。

## <a name="january-2018"></a>2018 年 1 月

2018 年 1 月 17 日&nbsp; / &nbsp;版本：0.25.4&nbsp; / &nbsp;公共预览版

1 月公共预览版包括一些功能建议和高优先级的 Bug 修复。 此版本包括以下增强功能：

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| 可以在“连接”对话框中使用已保存的服务器连接。 | &nbsp; |
| 启用热退出。 默认情况下，热退出处于关闭状态。 若要启用，请参阅[热退出设置](settings.md#hot-exit)。 | &nbsp; |
| 基于服务器组进行选项卡着色。 默认情况下，选项卡着色处于关闭状态。 若要启用，请参阅[选项卡颜色设置](settings.md#tab-color)。 | &nbsp; |
| 将“连接”对话框中的“服务器名称”更改为“服务器” 。 | &nbsp; |
| 修复损坏的“运行当前查询”命令。 | &nbsp; |
| 修复拖放时会破坏脚本的 bug。 | &nbsp; |
| 修复未正确固定的“开始”菜单图标。 | &nbsp; |
| 修复缺少的 Azure 帐户品牌图标。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="december-2017"></a>2017 年 12 月

2017 年 12 月 19 日&nbsp; / &nbsp;版本：0.24.1&nbsp; / &nbsp;公共预览版

12 月公共预览版包括涵盖所有功能区域的多项 Bug 修复，以及以下增强功能：

&nbsp;

| 更改 | 详细信息 |
| :----- | :------ |
| “创建防火墙规则”对话框现在可用于协助连接到 Azure SQL 数据库和 Azure Synapse Analytics。 | &nbsp; |
| 添加了 Windows 安装程序以及 Linux DEB 和 RPM 安装包。 | &nbsp; |
| 管理仪表板视觉布局编辑器。 | &nbsp; |
| “编写更改脚本”和“编写执行脚本”命令 。 | &nbsp; |
| “使用实际计划运行当前查询”命令。 | &nbsp; |
| 集成 VS Code 1.18.1 编辑器平台。 | &nbsp; |
| 启用 VSIX 扩展文件的旁加载。 | &nbsp; |
| 支持“GO N”批处理迭代语法。 | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="november-2017"></a>2017 年 11 月

2017 年 11 月 15 日&nbsp; / &nbsp;版本：0.23.6

- Azure Data Studio 的初始版本。

## <a name="next-steps"></a>后续步骤

请参阅以下快速入门以开始使用：

- [连接并查询 SQL Server](quickstart-sql-server.md)
- [连接并查询 Azure SQL 数据库](quickstart-sql-database.md)
- [连接和查询 Azure Synapse Analytics](quickstart-sql-dw.md)

参与 Azure Data Studio：

- [https://github.com/Microsoft/azuredatastudio](https://github.com/Microsoft/azuredatastudio)
