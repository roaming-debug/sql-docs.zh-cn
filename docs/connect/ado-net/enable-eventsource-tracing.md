---
title: 启用 SqlClient 中的事件跟踪
description: 介绍如何通过实现事件侦听器来启用 SqlClient 中的事件跟踪以及如何访问事件数据。
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: c13942dc5633deec4759ebd4c8a5661654c0952b
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837074"
---
# <a name="enable-event-tracing-in-sqlclient"></a>启用 SqlClient 中的事件跟踪

[!INCLUDE [appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE [Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

[Windows 事件跟踪 (ETW)](/windows/win32/etw/event-tracing-portal) 是一种高效的内核级别跟踪功能，通过此功能，你可以记录驱动程序定义的事件以进行调试和测试。 SqlClient 支持在不同的信息级别捕获 ETW 事件。 若要开始捕获事件跟踪，客户端应用程序应侦听来自 SqlClient 的 EventSource 实现的事件：

```
Microsoft.Data.SqlClient.EventSource
```

当前实现支持以下事件关键字：

| 关键字名称 | 值 | 描述 |
| ------------ | ----- | ----------- |
| ExecutionTrace | 1 | 启用命令执行前后捕获启动/停止事件。 |
| 跟踪 | 2 | 启用捕获基本应用程序流跟踪事件。 |
| 范围 | 4 | 启用捕获进入和退出事件 |
| NotificationTrace | 8 | 启用捕获 `SqlNotification` 跟踪事件 |
| NotificationScope | 16 | 启用捕获 `SqlNotification` 范围进入和退出事件 |
| PoolerTrace | 32 | 启用捕获连接池流跟踪事件。 |
| PoolerScope | 64 | 启用捕获连接池范围跟踪事件。 |
| AdvancedTrace | 128 | 启用捕获高级流跟踪事件。 |
| AdvancedTraceBin  | 256 | 启用捕获带有其他信息的高级流跟踪事件。 |
| CorrelationTrace | 512 | 启用捕获关联流跟踪事件。 |
| StateDump | 1024 | 启用捕获完整状态转储 `SqlConnection` |
| SNITrace | 2048 | 启用从托管网络实现捕获流跟踪事件（仅适用于 .NET Core） |
| SNIScope | 4096 | 启用从托管网络实现捕获范围事件（仅适用于 .NET Core） |
|||

## <a name="example"></a>示例

下面的示例对“AdventureWorks”示例数据库上的数据操作启用事件跟踪，并在控制台窗口中显示事件。

[!code-csharp [SqlClientEventSource#1](~/../sqlclient/doc/samples/SqlClientEventSource.cs#1)]

## <a name="event-tracing-support-in-native-sni"></a>本机 SNI 中的事件跟踪支持

Microsoft.Data.SqlClient v2.1.0 扩展了 Microsoft.Data.SqlClient.SNI 和 Microsoft.Data.SqlClient.SNI.runtime 中的事件跟踪支持。 通过将 EventCommand 发送到 `SqlClientEventSource`，可以使用 [Xperf](/windows-hardware/test/wpt/) 和 [PerfView](https://github.com/microsoft/perfview) 工具收集本机 SNI.dll 中的事件。 有效的 EventCommand 值如下所示：

```cs
// Enables trace events:
EventSource.SendCommand(eventSource, (EventCommand)8192, null);

// Enables flow events:
EventSource.SendCommand(eventSource, (EventCommand)16384, null);

// Enables both trace and flow events:
EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
```

以下示例在应用程序面向 .NET Framework 时启用本机 SNI.dll 中的事件跟踪。 

```cs
// Native SNI tracing example
// .NET Framework application
using System;
using System.Diagnostics.Tracing;
using Microsoft.Data.SqlClient;

public class SqlClientListener : EventListener
{
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        if (eventSource.Name.Equals("Microsoft.Data.SqlClient.EventSource"))
        {
            // Enables both trace and flow events
            EventSource.SendCommand(eventSource, (EventCommand)(8192 | 16384), null);
        }
    }
}

class Program
{
    static string connectionString = @"Data Source = localhost; Initial Catalog = AdventureWorks;Integrated Security=true;";

    static void Main(string[] args)
    {
        using (SqlClientListener listener = new SqlClientListener())
        using (SqlConnection connection = new SqlConnection(connectionString))
        {
            connection.Open();
        }        
    }
}
```

### <a name="use-xperf-to-collect-trace-log"></a>使用 Xperf 收集跟踪日志

1. 使用以下命令启动跟踪。

   ```
   xperf -start trace -f myTrace.etl -on *Microsoft.Data.SqlClient.EventSource
   ```

2. 运行本机 SNI 跟踪示例以连接到 SQL Server。

3. 使用以下命令停止跟踪。

   ```
   xperf -stop trace
   ```

4. 使用 PerfView 打开在步骤 1 中指定的 myTrace.etl 文件。 可以通过 `Microsoft.Data.SqlClient.EventSource/SNIScope` 和 `Microsoft.Data.SqlClient.EventSource/SNITrace` 事件名称找到 SNI 跟踪日志。

   ![使用 PerfView 查看 SNI 跟踪文件](media/view-event-trace-native-sni.png)


### <a name="use-perfview-to-collect-trace-log"></a>使用 PerfView 收集跟踪日志

1. 从菜单栏启动 PerfView 并运行 `Collect > Collect`。

2. 配置跟踪文件名、输出路径和提供程序名称。

   ![在收集之前配置 Prefview](media/collect-event-trace-native-sni.png)

3. 开始收集。

4. 运行本机 SNI 跟踪示例以连接到 SQL Server。

5. 使用 PerfView 停止收集。 根据步骤 2 中的配置，生成 PerfViewData.etl 文件需要一段时间。

6. 在 PerfView 中打开 `etl` 文件。 可以通过 `Microsoft.Data.SqlClient.EventSource/SNIScope` 和 `Microsoft.Data.SqlClient.EventSource/SNITrace` 事件名称找到 SNI 跟踪日志。

## <a name="external-resources"></a>外部资源  

有关详细信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[EventSource 类](/dotnet/api/system.diagnostics.tracing.eventsource)|用于创建 ETW 事件。|
|[EventListener 类](/dotnet/api/system.diagnostics.tracing.eventlistener)|提供用于启用和禁用事件源中事件的方法。|
