---
title: SqlClient 中的数据跟踪
description: 介绍用于 SQL Server 的 Microsoft SqlClient 数据提供程序如何提供内置数据跟踪功能。
ms.date: 12/04/2020
ms.assetid: a6a752a5-d2a9-4335-a382-b58690ccb79f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: fc8b5a7ca06af3c3e3ea83fcb747f79517e5ef7a
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744355"
---
# <a name="data-tracing-in-sqlclient"></a>SqlClient 中的数据跟踪

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

.NET 具有用于 SQL Server 的 Microsoft SqlClient 数据提供程序和 SQL Server 网络协议所支持的内置数据跟踪功能。

跟踪数据访问 API 调用可以帮助诊断以下问题：

- 客户端程序和数据库之间的架构不匹配。

- 数据库不可用或网络库问题。

- 不正确的 SQL，无论是硬编码还是由应用程序生成。

- 不正确的编程逻辑。

- 用于 SQL Server 的 Microsoft SqlClient 数据提供程序和你自己的组件之间的交互导致的问题。

为了支持不同的跟踪技术，跟踪是可扩展的，这样，开发人员可以在任何应用程序堆栈级别上跟踪问题。 用于 SQL Server 的 Microsoft SqlClient 数据提供程序利用通用跟踪和检测 API。

有关在 .NET 中设置和配置托管跟踪的更多信息，请参阅[跟踪数据访问](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10))。

## <a name="access-diagnostic-information-in-the-extended-events-log"></a>访问扩展事件日志中的诊断信息

在用于 SQL Server 的 Microsoft SqlClient 数据提供程序中，使用[数据访问跟踪](/previous-versions/sql/sql-server-2012/hh880086(v=msdn.10))可以更轻松将客户端事件与诊断信息关联（如服务器连接环形缓冲区中的连接失败以及扩展事件日志中的应用程序性能信息）。 有关读取扩展事件日志的信息，请参阅[查看事件会话数据](/previous-versions/sql/sql-server-2012/hh710068(v=sql.110))。

对于连接操作，用于 SQL Server 的 Microsoft SqlClient 数据提供程序将发送客户端连接 ID。 如果连接失败，你可以访问连接环形缓冲区（[在 SQL Server 2008 中使用连接环形缓冲区解决连接问题](/archive/blogs/sql_protocols/connectivity-troubleshooting-in-sql-server-2008-with-the-connectivity-ring-buffer)），查找 `ClientConnectionID` 字段并获取有关连接失败的诊断信息。 仅当发生错误时，才在环形缓冲区中记录客户端连接 ID。 如果在发送预登录数据包前连接失败，将不会生成客户端连接 ID。 客户端连接 ID 是 16 字节的 GUID。 如果 `client_connection_id` 操作已添加到扩展事件会话中的事件，则还可以在扩展事件目标输出中找到查找客户端连接 ID。 如果需要进一步的客户端驱动程序诊断帮助，可以启用数据访问跟踪并重新运行连接命令，然后观察 `ClientConnectionID` 字段。

还可以通过使用 `SqlConnection.ClientConnectionID` 属性以编程方式获取客户端连接 ID。

> [!NOTE]
> 用于 SQL Server 的 Microsoft SqlClient 数据提供程序支持自 2.1.0 版本后的服务器进程 ID。 可以使用 `SqlConnection.ServerProcessId` 属性以编程方式获取它。

`ClientConnectionID` 和 `ServerProcessId` 可用于成功建立连接的 <xref:Microsoft.Data.SqlClient.SqlConnection> 对象。 如果连接尝试失败，则 `ClientConnectionID` 可通过 `SqlException.ToString` 提供。

用于 SQL Server 的 Microsoft SqlClient 数据提供程序还会发送特定于线程的活动 ID。 如果开始会话时启用了 TRACK_CAUSALITY 选项，则会在扩展的事件会话中捕获活动 ID。 对于活动连接的性能问题，您可以从客户端的数据访问跟踪（`ActivityID` 字段）中获取活动 ID，然后在扩展事件输出中找到活动 ID。 扩展事件中的活动 ID 是一个 16 字节 GUID（与客户端连接 ID 的 GUID 不同），附有一个 4 字节的序列号。 该序列号表示某个请求在线程内的顺序，并指示线程的批处理和 RPC 语句的相对顺序。 启用数据访问跟踪以及数据访问跟踪配置字中的第 18 位处于打开状态时，当前可以针对 SQL 批处理语句和 RPC 请求选择发送 `ActivityID`。

以下 SQL 语句是一个使用 Transact-SQL 启动扩展事件会话的示例，该会话将存储在环形缓冲区中，并将记录从客户端针对 RPC 和批处理操作发送的活动 ID。

```sql
create event session MySession on server
add event connectivity_ring_buffer_recorded,
add event sql_statement_starting (action (client_connection_id)),
add event sql_statement_completed (action (client_connection_id)),
add event rpc_starting (action (client_connection_id)),
add event rpc_completed (action (client_connection_id))
add target ring_buffer with (track_causality=on)
```

## <a name="see-also"></a>另请参阅
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
