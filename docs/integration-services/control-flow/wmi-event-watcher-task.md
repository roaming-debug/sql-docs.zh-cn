---
description: WMI 事件观察器任务
title: WMI 事件观察器任务 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.wmieventwatchertask.f1
- sql13.dts.designer.wmieventwatcher.general.f1
- sql13.dts.designer.wmieventwatcher.wmiquery.f1
helpviewer_keywords:
- WQL [Integration Services]
- WMI Event Watcher task [Integration Services]
ms.assetid: b5bb52e9-a77e-41e1-93f9-d4c3bc6b2c9a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8b4e04871bf0264c9e9c723365f2a29d95265ffa
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594931"
---
# <a name="wmi-event-watcher-task"></a>WMI 事件观察器任务

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  WMI 事件观察器任务以使用 Management Instrumentation 查询语言 (WQL) 事件查询指定所关注事件的方式来监视 Windows Management Instrumentation (WMI) 事件。 可以将 WMI 事件观察器任务用于下列目的：  
  
-   等待已将文件添加到文件夹的通知，然后开始处理文件。  
  
-   当服务器上的可用内存低于指定百分比时，运行删除文件的包。  
  
-   监视应用程序的安装，然后运行使用该应用程序的包。  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 包含读取 WMI 信息的任务。  
  
 有关此任务的详细信息，请单击下列主题：  
  
-   [WMI 数据读取器任务](../../integration-services/control-flow/wmi-data-reader-task.md)  
  
## <a name="wql-queries"></a>WQL 查询  
 WQL 是 SQL 的方言，其扩展插件支持 WMI 事件通知和其他 WMI 特定功能。 有关 WQL 的详细信息，请参阅 [MSDN Library](/documentation/)中的 Windows Management Instrumentation 文档。  
  
> [!NOTE]  
>  WMI 类因 Windows 版本的不同而异。  
  
 以下查询监视 CPU 使用率超过 40% 的通知。  
  
```  
SELECT * from __InstanceModificationEvent WITHIN 2 WHERE TargetInstance ISA 'Win32_Processor' and TargetInstance.LoadPercentage > 40  
```  
  
 以下查询监视已将文件添加到文件夹的通知。  
  
```  
SELECT * FROM __InstanceCreationEvent WITHIN 10 WHERE TargetInstance ISA "CIM_DirectoryContainsFile" and TargetInstance.GroupComponent= "Win32_Directory.Name=\"c:\\\\WMIFileWatcher\""   
```  
  
## <a name="custom-logging-messages-available-on-the-wmi-event-watcher-task"></a>WMI 事件观察器任务可用的自定义日志记录消息  
 下表列出了 WMI 事件观察器任务的自定义日志项。 有关详细信息，请参阅 [Integration Services (SSIS) 日志记录](../../integration-services/performance/integration-services-ssis-logging.md)。  
  
|日志项|说明|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|指示发生了任务正在监视的事件。|  
|**WMIEventWatcherTimedout**|指示任务已超时。|  
|**WMIEventWatcherWatchingForWMIEvents**|指示任务已开始执行 WQL 查询。 日志项包括查询。|  
  
## <a name="configuration-of-the-wmi-event-watcher-task"></a>WMI 事件观察器任务的配置  
 可以通过以下方式配置 WMI 数据读取器任务：  
  
-   指定要使用的 WMI 连接管理器。  
  
-   指定 WQL 查询的源。 源可以在任务外部，比如变量或文件，否则可把查询存储在任务属性中。  
  
-   指定在 WMI 事件发生时任务所执行的操作。 可以记录事件通知和事件后的状态，也可以引发自定义 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 事件来提供与 WMI 事件、通知和事件后的状态相关的信息。  
  
-   定义任务对事件的响应方式。 可根据事件的不同将任务配置为成功或失败，或只是让任务重新监视事件。  
  
-   指定当 WMI 查询超时的时候任务所执行的操作。可以记录超时和超时后的状态，也可以引发自定义的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 事件，以指示 WMI 事件已超时并记录超时和超时状态。  
  
-   定义任务对超时的响应方式。可将任务配置为成功或失败，或只是让任务重新监视事件。  
  
-   指定任务监视事件的次数。  
  
-   指定超时。  
  
 如果源是文件，则 WMI 事件观察器任务使用文件连接管理器连接到该文件。 有关详细信息，请参阅 [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md)。  
  
 WMI 事件观察器任务使用 WMI 连接管理器连接到它从中读取 WMI 信息的服务器。 有关详细信息，请参阅 [WMI Connection Manager](../../integration-services/connection-manager/wmi-connection-manager.md)。  
  
 可以通过 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器或以编程方式来设置属性。  
  
 有关可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置的属性的详细信息，请单击以下主题：  
  
-   [“表达式”页](../../integration-services/expressions/expressions-page.md)  
  
 有关如何在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 设计器中设置这些属性的详细信息，请单击下列主题：  
  
-   [设置任务或容器的属性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-wmi-event-watcher-task"></a>WMI 事件观察器任务的编程配置  
 有关以编程方式设置这些属性的详细信息，请单击以下主题：  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.WmiEventWatcherTask.WmiEventWatcherTask>  
  
## <a name="wmi-event-watcher-task-editor-general-page"></a>WMI 事件观察器任务编辑器（“常规”页）
  可以使用 **“WMI 事件观察器任务编辑器”** 对话框的 **“常规”** 页，对 WMI 事件观察器任务进行命名和说明。  
  
 有关 WMI 查询语言 (WQL) 的详细信息，请参阅 MSDN 库中的 Windows Management Instrumentation 主题 [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)（利用 WQL 进行查询）。  
  
### <a name="options"></a>选项  
 **名称**  
 为 WMI 事件观察器任务提供唯一的名称。 此名称用作任务图标中的标签。  
  
> [!NOTE]  
>  任务名称在一个包内必须是唯一的。  
  
 **说明**  
 键入 WMI 事件观察器任务的说明。  
  
## <a name="wmi-event-watcher-task-editor-wmi-options-page"></a>WMI 事件观察器任务编辑器（“WMI 选项”页）
  可以使用“WMI 事件观察器任务编辑器”  对话框的“WMI 选项”  页，指定 Windows Management Instrumentation 查询语言 (WQL) 查询的源以及 WMI 事件观察器任务响应 Microsoft Windows Instrumentation (WMI) 事件的方式。  
  
 有关 WMI 查询语言 (WQL) 的详细信息，请参阅 MSDN 库中的 Windows Management Instrumentation 主题 [Querying with WQL](/windows/win32/wmisdk/querying-with-wql)（利用 WQL 进行查询）。  
  
### <a name="static-options"></a>静态选项  
 **WMIConnectionName**  
 从列表中选择 WMI 连接管理器，或单击 \<**New WMI Connection...**> 创建新的连接管理器。  
  
 **相关主题：** [WMI 连接管理器](../../integration-services/connection-manager/wmi-connection-manager.md)、[WMI 连接管理器编辑器](../connection-manager/wmi-connection-manager.md)  
  
 **WQLQuerySourceType**  
 选择任务运行的 WQL 查询的源类型。 此属性具有下表所列的选项。  
  
|值|说明|  
|-----------|-----------------|  
|**直接输入**|为 WQL 查询设置源。 选择此值将显示动态选项 **WQLQuerySource**。|  
|**文件连接**|选择包含 WQL 查询的文件。 选择此值将显示动态选项 **WQLQuerySource**。|  
|**变量**|将源设置为定义 WQL 查询的变量。 选择此值将显示动态选项 **WQLQuerySource**。|  
  
 **ActionAtEvent**  
 指定 WMI 事件是记录事件并启动 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 操作，还是只记录事件。  
  
 **AfterEvent**  
 指定任务在收到 WMI 事件之后是成功还是失败，或者任务是否继续监视事件的再次发生。  
  
 **ActionAtTimeout**  
 指定任务是记录 WMI 查询超时并启动 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 事件作为响应，还是只记录超时。  
  
 **AfterTimeout**  
 指定任务对超时的响应是成功还是失败，或者任务是否继续监视超时的再次发生。  
  
 **NumberOfEvents**  
 指定要监视的事件数。  
  
 **超时**  
 指定等待事件发生的秒数。 值为 0 表示实际上无超时。  
  
### <a name="wqlquerysource-dynamic-options"></a>WQLQuerySource 动态选项  
  
#### <a name="wqlquerysource--direct-input"></a>WQLQuerySource = 直接输入  
 **WQLQuerySource**  
 提供查询，或单击省略号按钮 (...) 并使用“WQL 查询”对话框输入查询  。  
  
#### <a name="wqlquerysource--file-connection"></a>WQLQuerySource = 文件连接  
 **WQLQuerySource**  
 从列表中选择文件连接管理器，或单击“\<**New connection...**>”创建新的连接管理器。  
  
 **相关主题：** [文件连接管理器](../../integration-services/connection-manager/file-connection-manager.md)、[文件连接管理器编辑器](../connection-manager/file-connection-manager.md)  
  
#### <a name="wqlquerysource--variable"></a>WQLQuerySource = 变量  
 **WQLQuerySource**  
 在列表中选择变量，或单击“\<**New variable...**>创建一个新变量。  
  
 **相关主题：** [Integration Services &#40;SSIS&#41; 变量](../../integration-services/integration-services-ssis-variables.md)、[添加变量](../integration-services-ssis-variables.md)