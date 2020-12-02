---
description: Integration Services 包记录的事件
title: Integration Services 包记录的事件 | Microsoft Docs
ms.custom: supportability
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- package [Integration Services], events
- events [Integration Services], package
ms.assetid: 55a0951a-46f3-4f0f-9972-74cec9cc26b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 44f3efa30bc33bd7a8b9bc5057d2877be62f8f8a
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130033"
---
# <a name="events-logged-by-an-integration-services-package"></a>Integration Services 包记录的事件

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包将各种事件消息记入 Windows 应用程序事件日志。 包会在包启动时、包停止时和特定问题出现时记录这些消息。  
  
 本主题提供有关包记入应用程序事件日志的常见事件消息的信息。 默认情况下，即使未对包启用日志记录，包也会记录其中某些消息。 但对于其他消息，只有对包启用了日志记录，包才会记录这些消息。 无论包是在默认情况下还是在启用日志记录后记录这些消息，消息的事件源都是 SQLISPackage。  
  
 有关如何运行 SSIS 包的常规信息，请参阅 [项目和包的执行](../packages/run-integration-services-ssis-packages.md)。  
  
 有关如何对运行的包进行故障排除的信息，请参阅 [对包执行进行故障排除的工具](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)。  
  
## <a name="messages-about-the-status-of-the-package"></a>有关包状态的消息  
 运行 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包时，包会通常会记录有关包的进度和状态的各种消息。 下表列出了这些消息。  
  
> [!NOTE]  
>  即使未对包启用日志记录，包也会记录下表中的消息。  
  
|事件 ID|符号名称|文本|备注|  
|--------------|-------------------|----------|-----------|  
|12288|DTS_MSG_PACKAGESTART|包“”已开始运行。|包已开始运行。|  
|12289|DTS_MSG_PACKAGESUCCESS|包“”已成功完成。|包已运行成功且不再运行。|  
|12290|DTS_MSG_PACKAGECANCEL|包“%1!s!” 已取消。|包已取消，因此不再运行。|  
|12291|DTS_MSG_PACKAGEFAILURE|包“”已失败。|包无法成功运行，已停止运行。|  
  
 默认情况下，在全新安装中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 配置为不将有关包运行的某些事件记录到应用程序事件日志中。 使用当前版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]的数据收集器功能时，此设置可防止生成过多的事件日志项。 未记录的事件是，EventID 12288“包已启动”和 EventID 12289“包已成功完成”。 若要将这些事件记录到应用程序事件日志中，请打开注册表以进行编辑。 然后在注册表中，找到 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\130\SSIS 节点，并将 LogPackageExecutionToEventLog 设置的 DWORD 值从 1 更改为 0。 不过，在升级安装中， [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 配置为记录这两个事件。 若要禁用记录功能，请将 LogPackageExecutionToEventLog 设置的值从 1 更改为 0。  
  
## <a name="messages-associated-with-package-logging"></a>与包日志记录关联的消息  
 如果已对包启用日志记录，应用程序事件日志将是 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包中可选的日志记录功能所支持的目标之一。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
 如果已对包启用日志记录而且日志位置为应用程序事件日志，则包将记录与以下信息相关的项：  
  
-   有关包运行时所处的阶段的消息。  
  
-   有关包运行时发生的特定事件的消息。  
  
### <a name="messages-about-the-stages-of-package-execution"></a>有关包执行阶段的消息  
  
|事件 ID|符号名称|文本|备注|  
|--------------|-------------------|----------|-----------|  
|12544|DTS_MSG_EVENTLOGENTRY|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|将包日志记录配置为记入应用程序事件日志时，各种消息都会使用这种通用格式。|  
|12556|DTS_MSG_EVENTLOGENTRY_PACKAGESTART|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|包已启动。|  
|12547|DTS_MSG_EVENTLOGENTRY_PREVALIDATE|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|对象验证即将开始。|  
|12548|DTS_MSG_EVENTLOGENTRY_POSTVALIDATE|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|对象验证已完成。|  
|12552|DTS_MSG_EVENTLOGENTRY_PROGRESS|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|此通用消息用于报告包的进度。|  
|12546|DTS_MSG_EVENTLOGENTRY_POSTEXECUTE|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|对象已完成其工作。|  
|12557|DTS_MSG_EVENTLOGENTRY_PACKAGEEND|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|包已经完成运行。|  
  
### <a name="messages-about-events-that-occur"></a>有关发生的事件的消息  
 下表仅列出属于事件结果的部分消息。 有关 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 使用的错误、警告和信息性消息的更全面的列表，请参阅 [Integration Services 错误和消息引用](../../integration-services/integration-services-error-and-message-reference.md)。  
  
|事件 ID|符号名称|文本|备注|  
|--------------|-------------------|----------|-----------|  
|12251|DTS_MSG_EVENTLOGENTRY_TASKFAILED|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|任务失败。|  
|12250|DTS_MSG_EVENTLOGENTRY_ERROR|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|此消息用于报告产生了一个错误。|  
|12249|DTS_MSG_EVENTLOGENTRY_WARNING|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|此消息用于报告产生了一个警告。|  
|12258|DTS_MSG_EVENTLOGENTRY_INFORMATION|事件名称: %1%r 消息: %9%r 操作员: %2%r 源名称: %3%r 源 ID: %4%r 执行 ID: %5%r 开始时间: %6%r 结束时间: %7%r 数据代码: %8|此消息用于报告与错误或警告无关的信息。|  

## <a name="view-log-entries-in-the-log-events-window"></a>在“日志事件”窗口中查看日志项
  此过程描述如何运行包并查看它写入的日志项。 您可以实时查看日志项。 此外，还可以将写入 **“日志事件”** 窗口的日志项复制并保存，以便进行进一步分析。  
  
 不需要将日志项写入日志即可将这些项写入 **“日志事件”** 窗口。  
  
### <a name="to-view-log-entries"></a>查看日志项  
  
1.  在 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]中，打开包含所需包的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目。  
  
2.  在 **SSIS** 菜单上单击 **“日志事件”** 。 通过将 View.LogEvents 命令映射为在 **“选项”** 对话框的 **“键盘”** 页中所选的组合键，您可以选择显示 **“日志事件”** 窗口。  
  
3.  在 **“调试”** 菜单中，单击 **“启动调试”** 。  
  
     当运行时遇到为日志记录启用的事件和自定义消息时，每个事件或消息的日志项将写入 **“日志事件”** 窗口。  
  
4.  在 **“调试”** 菜单中，单击 **“停止调试”** 。  
  
     日志项在 **“日志事件”** 窗口中保留可用状态，直到重新运行包、运行其他包或关闭 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]。  
  
5.  在 **“日志事件”** 窗口中查看日志项。  
  
6.  （可选）单击要复制的日志项，右键单击，然后单击“复制”。  
  
7.  （可选）双击日志项，然后在“日志条目”对话框中，查看单个日志项的详细信息。  
  
8.  在 **“日志条目”** 对话框中，单击向上和向下键头，以显示上一个或下一个日志项，并单击复制图标以复制该日志项。  
  
9. 打开文本编辑器，粘贴，然后将日志项保存到文本文件中。
