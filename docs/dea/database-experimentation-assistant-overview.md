---
title: 数据库实验助手概述
description: 了解数据库实验助手 (DEA) 的详细信息，例如如何评估特定工作负荷的目标 SQL Server 目标版本。
ms.date: 12/12/2019
ms.prod: sql
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.openlocfilehash: 09d334d7cfcc313cd36d3b59a6b47c64d441f3ee
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017067"
---
# <a name="overview-of-database-experimentation-assistant"></a>数据库实验助手概述

数据库实验助手 (DEA) 是 SQL Server 升级的试验解决方案。 DEA 可帮助你评估特定工作负载的 SQL Server 目标版本。 从 2005) 开始，从到最新版本 SQL Server 的 SQL Server 升级 (的客户可以使用该工具提供的分析指标。

DEA 分析指标包括：

- 具有兼容性错误的查询。
- 降级的查询和查询计划。
- 其他工作负荷比较数据。

比较数据可能会导致更高的置信度，并有助于确保升级成功。

## <a name="get-dea"></a>获取 DEA

若要安装 DEA，请 [下载](https://www.microsoft.com/download/details.aspx?id=54090) 最新版本的工具。 然后，运行 **DatabaseExperimentationAssistant.exe** 文件。

## <a name="solution-architecture-for-comparing-workloads"></a>用于比较工作负载的解决方案体系结构

下图显示了用于工作负载比较的解决方案体系结构。 在从 SQL Server 2008 升级到 SQL Server 2016 期间，工作负载比较使用 DEA 和 Distributed Replay。

![工作负载比较解决方案体系结构](./media/database-experimentation-assistant-overview/dea-overview-compare-solution-architecture.png)

## <a name="dea-prerequisites"></a>DEA 先决条件

下面是运行 DEA 的一些先决条件：

- 最低硬件要求：具有 3.5 GB RAM 的单核计算机。
- 理想硬件要求：具有 3.5 GB RAM (8 核 CPU) 。 具有8个以上内核的处理器不会提高 DEA 运行时间。
- 存储、B 和报表分析数据库需要额外33% 的性能跟踪大小。

## <a name="configure-dea"></a>配置 DEA

在必备的环境体系结构中，我们建议在 *Distributed Replay 控制器所在的同一台计算机上* 安装 DEA。 这种做法可以避免跨计算机调用并简化配置。

### <a name="required-configuration-for-workload-comparison-using-dea"></a>使用 DEA 进行工作负荷比较所需的配置

DEA 使用 Windows 身份验证连接到数据库服务器。 确保运行 DEA 的用户可以使用 Windows 身份验证连接到数据库服务器 (源、目标和分析) 。

**捕获配置要求**

捕获跟踪要求运行 DEA 的用户：

- 可以使用 Windows 身份验证连接到源数据库服务器。
- 对源数据库服务器具有 sysadmin 权限。

此外，运行源数据库服务器的服务帐户需要对跟踪文件夹路径的写访问权限。

有关详细信息，请参阅 [有关跟踪捕获](database-experimentation-assistant-capture-trace.md#frequently-asked-questions-about-trace-capture)的常见问题。

**重播配置要求**

重播跟踪要求运行 DEA 的用户：

- 可以使用 Windows 身份验证连接到目标数据库服务器。
- 具有对目标数据库服务器的 sysadmin 权限。

此外，重播跟踪需要：

- 运行目标数据库服务器的服务帐户具有对跟踪文件夹路径的写访问权限。
- Distributed Replay 客户端运行的服务帐户可以使用 Windows 身份验证连接到目标数据库服务器。
- 为 Distributed Replay 控制器上的传入请求打开 TCP 端口。 DEA 使用 COM 接口与 Distributed Replay 控制器通信。

有关详细信息，请参阅 [有关跟踪重播](database-experimentation-assistant-replay-trace.md#frequently-asked-questions-about-trace-replay)的常见问题。

**分析配置要求**

执行此分析要求运行 DEA 的用户：

- 可以使用 Windows 身份验证连接到分析数据库服务器。
- 对源数据库服务器具有 sysadmin 权限。

有关详细信息，请参阅 [有关分析报表](database-experimentation-assistant-create-report.md#frequently-asked-questions-about-analysis-reports)的常见问题。

## <a name="set-up-telemetry"></a>设置遥测

DEA 具有支持 internet 的功能，该功能可将遥测信息发送给 Microsoft，以用于增强产品体验。 收集的信息也保存在您的计算机上以进行本地审核，因此您始终可以查看收集的信息。 所有 DEA 日志文件都保存在% temp% \\ DEA 文件夹中。

可以收集四种事件类型的遥测数据：

- **TraceEvent**：应用程序 (的使用事件例如，"触发停止捕获" ) 。
- **异常**：应用程序使用期间引发了异常。
- **DiagnosticEvent**：一种事件日志，用于在出现问题时帮助诊断 (*未* 发送到 Microsoft) 。
- **FeedbackEvent**：通过应用程序提交的用户反馈。

收集和发送遥测数据是可选的。 若要指定收集的事件以及收集的事件是否发送给 Microsoft，请执行以下步骤：

1. 请参阅 DEA 的安装位置 (例如，C： \\ Program Files (x86) \\ Microsoft Corporation \\ 数据库实验助手) 。
2. 打开并修改应用程序) **DEA.exe.config** (的 .config 文件，并根据需要为 CLI (**DEACmd.exe.config**) 来处理方案：
    - 若要停止收集事件类型，请设置 *事件* (的值，例如，将 **TraceEvent**) 为 **false**。 若要再次开始收集事件，请将值设置为 **true**。
    - 若要停止保存事件的本地副本，请将 **TraceLoggerEnabled** 的值设置为 **false**。 若要再次开始保存本地副本，请将值设置为 **true**。
    - 若要停止向 Microsoft 发送事件，请将 **AppInsightsLoggerEnabled** 的值设置为 **false**。 若要再次开始向 Microsoft 发送事件，请将值设置为 **true**。

DEA 由 [Microsoft 隐私声明](https://aka.ms/dea-privacy)控制。

## <a name="see-also"></a>另请参阅

- [工作负载比较过程的概述](database-experimentation-assistant-get-started.md)，其中介绍了在两个环境中比较工作负荷时所涉及的过程。
