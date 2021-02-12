---
title: SQL Server Profiler 扩展
description: 连接如何安装和使用 SQL Server Profiler 扩展。 它是与 SSMS Profiler 类似的易用 SQL Server 跟踪解决方案。
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: yualan
ms.author: alayu
ms.reviewer: maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: f2a6672f3b73a5be8077229019473758b2bf1e95
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040247"
---
# <a name="sql-server-profiler-extension-preview"></a>SQL Server Profiler 扩展（预览版）

SQL Server Profiler 扩展（预览版）提供类似于 SQL Server Management Studio (SSMS) Profiler 的简单 SQL Server 跟踪解决方案，但前者是使用扩展事件构建的。 SQL Server Profiler 非常易于使用，为最常见的跟踪配置设置了很合适的默认值。 UX 针对事件浏览和关联 Transact-SQL (T-SQL) 文本的查看性能进行了优化。 Azure Data Studio 的 SQL Server Profiler 还为实现后列操作设置了很合适的默认值：通过易于使用的 UX 收集 T-SQL 执行活动。 此扩展当前处于预览状态。

**常见的 SQL Profiler 用例：**

- 逐步分析有问题的查询以找到问题的原因。
- 查找并诊断运行慢的查询。
- 捕获导致问题的一系列 Transact-SQL 语句。
- 监视 SQL Server 的性能以优化工作负荷。
- 使性能计数器与诊断问题关联。

## <a name="install-the-sql-server-profiler-extension"></a>安装 SQL Server Profiler 扩展

1. 若要打开扩展管理器并访问可用扩展，请选择扩展图标，或在“视图”菜单中选择“扩展”   。
2. 选择某个可用扩展以查看其详细信息。

    ![Profiler 扩展管理器](media/sql-server-profiler-extension/profiler-extension.png)

3. 选择所需的扩展并“安装”它。
4. 选择“重新加载”以启用扩展（仅在第一次安装扩展时是必需的）  。

## <a name="start-profiler"></a>启动 Profiler

1. 要启动 Profiler，需先在“服务器”选项卡中建立与服务器的连接。
2. 建立连接后，键入“Alt + P”启动 Profiler  。
3. 要启动 Profiler，请键入“Alt + S”  现在可以开始查看扩展事件。

    ![查看探查器](media/sql-server-profiler-extension/view-profiler.png)

4. 要停止 Profiler，请键入“Alt + S”  。此热键是一个开关。

## <a name="next-steps"></a>后续步骤

要了解有关 Profiler 和扩展事件的详细信息，请参阅[扩展事件](../../relational-databases/extended-events/extended-events.md)。