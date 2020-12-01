---
title: Azure Data Studio 故障排除
description: 了解如何获取日志并对 Azure Data Studio 进行故障排除，以帮助报告 Bug 报告。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: hanqin, maghan
ms.custom: seodec18
ms.date: 11/24/2020
ms.openlocfilehash: 1d2483532589cd840f1120cfb0ac3273b41e0bb5
ms.sourcegitcommit: 2e7154475ba1f31d1aeebc8f48ac05846f793736
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "95920205"
---
# <a name="azure-data-studio-troubleshooting"></a>Azure Data Studio 故障排除
Azure Data Studio 跟踪 `azuredatastudio` 存储库的 [GitHub 存储库问题跟踪器](https://github.com/Microsoft/azuredatastudio/issues)上使用的问题和功能请求。 

## <a name="if-youve-experienced-any-issue"></a>如遇到任何问题

向 [GitHub 问题跟踪器](https://github.com/Microsoft/azuredatastudio/issues)报告问题，并提供有助于再现错误的任何详细信息。 通过日志文件添加任何[日志信息](#how-to-set-the-logging-level)。

## <a name="writing-good-bug-reports-and-feature-requests"></a>完善 Bug 报告和功能请求

每个问题和功能请求对应一个问题。

* 不要在同一问题中枚举多个 bug 或功能请求。
* 不要将你的问题作为评论添加到现有问题中，除非它针对的是相同的输入信息。 许多问题虽然看起来相似，但产生的原因有所不同。

你能提供的信息越多，我们就越有可能成功再现该问题并找到解决方法。 

每个问题应包括以下信息：

* Azure Data Studio 的版本
* 可重现步骤（1...2...3...) 以及你期望的效果和实际看到的效果。 
* 图像、动画或视频链接。 图像和动画可演示重现步骤，但不取代它们。
* 用于演示问题的代码段或指向代码存储库的链接，方便我们下拉到计算机以重新创建问题。 

> [!NOTE]
>  由于我们需要复制并粘贴代码片段，因此仅添加代码片段作为媒体文件（如 .gif）还不够。 

* 开发人员工具控制台中的错误（帮助 | 切换开发人员工具）

请记得执行以下操作：

* 搜索问题存储库，查看是否存在重复问题。 
* 简化围绕问题的代码，让我们能更好地划分出该问题。 

如果我们不能再现这个问题并需要你提供更多信息，请提供支持！

## <a name="how-to-set-the-logging-level"></a>如何设置日志记录级别

### <a name="azure-data-studio"></a>Azure Data Studio
运行 `Developer: Set Log Level...` 命令，选择当前会话的日志级别。 此值不会在多个会话中持久保存，因此如果重新启动 Azure Data Studio，它将恢复到默认的 `Info` 级别。 

如果要在启动时启用调试日志记录，请将日志级别设置为 `Debug` 并运行 `Developer: Reload Window` 命令

### <a name="mssql-built-in-extension"></a>MSSQL（内置扩展）

如果 `Mssql: Log Debug Info` 用户设置设为“true”，则会将调试日志信息发送到 `MSSQL` 输出通道。

`Mssql: Tracing Level` 用户设置用于控制日志记录的详细程度。

## <a name="debug-log-location"></a>调试日志位置
在 Azure Data Studio 中运行 `Developer: Open Logs Folder` 命令，打开日志的路径。 有许多不同类型的日志文件写入其中，一些常用的日志文件包括：

1. `renderer#.log`（例如，renderer1.log）- 该文件是主进程的日志文件。
1. `telemetry.log` - 当日志级别设置为 `Trace` 时，该文件将包含 Azure Data Studio 发送的遥测事件
1. `exthost#/exthost.log` - 扩展主机进程的日志文件（这只是进程本身，而不是在其中运行的扩展）
1. `exthost#/Microsoft.mssql` - mssql 扩展的日志，其中包含 MSSQL 相关功能的很多核心逻辑
   * sqltools.log 是 SQL 工具服务的日志
1. `exthost#/output_logging_#######` - 这些文件夹包含 Azure Data Studio 的 `Output` 面板中显示的消息。 每个文件都命名为 `#-<Channel Name>`，例如，`Notebooks` 输出通道输出的文件命可能为 `3-Notebooks.log`。

如果系统要求你提供日志，请压缩整个文件夹，确保包含正确的日志。 

## <a name="next-steps"></a>后续步骤
- [报告问题](https://github.com/Microsoft/azuredatastudio/issues)
- [什么是 Azure Data Studio](what-is-azure-data-studio.md)