---
title: 什么是 Azure Data Studio
description: Azure Data Studio 是一种免费的轻型工具，可在 Windows、macOS 和 Linux 上运行，用于管理 SQL Server、Azure SQL 数据库和 Azure Synapse Analytics。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: overview
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, sstein
ms.custom: seodec18, sqlfreshmay19
ms.date: 10/20/2020
ms.openlocfilehash: daba14f8b407d2ce5aabd81fc38915c29e114a5e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048058"
---
# <a name="what-is-azure-data-studio"></a>什么是 Azure Data Studio？

Azure Data Studio 是一种跨平台的数据库工具，适合在 Windows、macOS 和 Linux 上使用本地和云数据平台的数据专业人员。

Azure Data Studio 利用 IntelliSense、代码片段、源代码管理集成和集成终端提供新式编辑器体验。 它在设计时考虑了数据平台用户，带有内置查询结果集图表和可自定义的仪表板。

可通过一个提供软件修改和使用权限的源代码 EULA 来获取 GitHub 上 Azure Data Studio 的源代码及其数据提供程序，但不能在云服务中重新分发或托管该源代码。 有关详细信息，请参阅 [Azure Data Studio 常见问题解答](faq.md)。

[下载并安装 Azure Data Studio](./download-azure-data-studio.md)

## <a name="sql-code-editor-with-intellisense"></a>带有 IntelliSense 的 SQL 代码编辑器

Azure Data Studio 利用内置功能（例如多个选项卡窗口、丰富的 SQL 编辑器、IntelliSense、关键字完成、代码片段、代码导航和源代码管理集成 (Git)）提供一种基于键盘的新式 SQL 编码体验，使日常任务变得更轻松。 运行按需 SQL 查询，查看结果并将其保存为文本、JSON 或 Excel 格式。 编辑数据，组织你最喜欢的数据库连接，并以熟悉的对象浏览体验浏览数据库对象。 若要了解如何使用 SQL 编辑器，请参阅[使用 SQL 编辑器创建数据库对象](tutorial-sql-editor.md)。

## <a name="smart-sql-code-snippets"></a>智能 SQL 代码片段

SQL 代码片段可生成正确的 SQL 语法来创建数据库、表、视图、存储过程、用户、登录名、角色，并更新现有的数据库对象。 使用智能代码片段快速创建数据库副本，以便进行开发或测试，并生成和执行 CREATE 和 INSERT 脚本。

Azure Data Studio 还提供用于创建自定义 SQL 代码片段的功能。 若要了解详细信息，请参阅[创建和使用代码片段](code-snippets.md)。

## <a name="customizable-server-and-database-dashboards"></a>可自定义的服务器和数据库仪表板

创建丰富的可自定义仪表板，以监视并快速排查数据库中的性能瓶颈问题。 若要了解见解小组件以及数据库（和服务器）仪表板，请参阅[使用见解小组件管理服务器和数据库](insight-widgets.md)。

## <a name="connection-management-server-groups"></a>连接管理（服务器组）

借助服务器组，可以组织所使用的服务器和数据库的连接信息。 有关详细信息，请参阅[服务器组](server-groups.md)。

## <a name="integrated-terminal"></a>集成终端

在 Azure Data Studio 用户界面中的“集成终端”窗口中使用常用的命令行工具（例如，Bash、PowerShell、sqlcmd、bcp 和 ssh）。 若要了解集成终端，请参阅[集成终端](integrated-terminal.md)。

## <a name="extensibility-and-extension-authoring"></a>扩展性和扩展创作

通过扩展基本安装的功能来增强 Azure Data Studio 体验。 Azure Data Studio 为数据管理活动提供扩展点，并支持扩展创作。

若要了解 Azure Data Studio 中的扩展性，请参阅[扩展性](extensibility.md)。
若要了解如何创作扩展，请参阅[扩展创作](extensions/extension-authoring.md)。

## <a name="feature-comparison-with-sql-server-management-studio-ssms"></a>与 SQL Server Management Studio (SSMS) 的功能比较

**如为以下情况，请使用 Azure Data Studio：**

- 主要是编辑或执行查询。
- 需要能够快速绘图和直观显示结果集。
- 可通过集成终端使用 sqlcmd 或 PowerShell 执行大多数管理任务。
- 对向导的使用需求较少。
- 不需要实施深层次的管理或平台配置。
- 需要在 macOS 或 Linux 上运行。

**如为以下情况，请使用 SQL Server Management Studio：**

- 需要实施复杂的管理或平台配置。
- 要实施安全管理，包括用户管理、漏洞评估和安全功能配置。
- 需要使用性能优化顾问和仪表板。
- 使用数据库关系图和表设计器。
- 需要访问经过注册的服务器。
- 需要利用实时查询统计信息或客户端统计信息。

### <a name="shell-features"></a>Shell 功能

|Feature|Azure Data Studio|SSMS|
|:---|:---|:---|
|Azure 登录|是|是|
|仪表板|是| |
|扩展|是| |
|集成终端|是||
|“对象资源管理器”|是|是|
|对象脚本|是|是|
|项目系统|是||
|从表中选择|是|是|
|源代码管理|是||
|任务窗格|是||
|主题，包括深色模式|是||
|Azure 资源浏览器|预览||
|生成脚本向导||是|
|对象属性||是|
|表设计器||是|

### <a name="query-editor"></a>查询编辑器

|Feature|Azure Data Studio|SSMS|
|:---|:---|:---|
|图表查看器|是||
|将结果导出为 CSV、JSON、XLSX|是||
|将结果保存到文件||是|
|以文本格式显示结果||是|
|IntelliSense|是|是|
|代码片段|是|是|
|显示计划|预览|是|
|客户端统计信息||是|
|实时查询统计信息||是|
|查询选项||是|
|空间查看器||是|
|SQLCMD|是|是|

### <a name="operating-system-support"></a>操作系统支持

|Feature|Azure Data Studio|SSMS|
|:---|:---|:---|
|Windows|是|是|
|macOS|是||
|Linux|是||

### <a name="data-engineering"></a>数据工程

|Feature|Azure Data Studio|SSMS|
|:---|:---|:---|
|外部数据向导|预览||
|HDFS 集成|预览||
|笔记本|预览||

### <a name="database-administration"></a>数据库管理

|Feature|Azure Data Studio|SSMS|
|:---|:---|:---|
|备份/还原|是|是|
|平面文件导入|是|是|
|SQL 代理|预览|是|
|SQL Profiler|预览|是|
|AlwaysOn||是|
|Always Encrypted||是|
|复制数据向导||是|
|数据优化顾问||是|
|数据库关系图||是|
|错误日志查看器||是|
|维护计划||是|
|多服务器查询||是|
|基于策略的管理||是|
|PolyBase||是|
|查询存储||是|
|已注册的服务器||是|
|复制||是|
|安全管理||是|
|Service Broker||是|
|SQL 评估|预览|是|
|SQL Mail||是|
|模板资源管理器||是|
|漏洞评估||是|
|XEvent 管理||是|

### <a name="database-development"></a>数据库开发
|Feature|Azure Data Studio|SSMS|
|:---|:---|:---|
|导入\导出 DACPAC|是|是|
|SQL 项目|预览||
|架构比较|是||

## <a name="next-steps"></a>后续步骤

- [下载并安装 Azure Data Studio](./download-azure-data-studio.md)
- [Azure Data Studio 常见问题解答](faq.md)
- [连接并查询 SQL Server](quickstart-sql-server.md)
- [连接并查询 Azure SQL 数据库](quickstart-sql-database.md)

[!INCLUDE[get-help-sql-tools](../includes/paragraph-content/get-help-sql-tools.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]
