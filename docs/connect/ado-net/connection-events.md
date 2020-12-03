---
title: 连接事件
description: 用于从数据源中检索信息性消息并确定其状态是否已被更改的连接事件。
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 5a29de74-acfc-4134-8616-829dd7ce0710
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 67b805e4ec95047b843e6b72ba10dc8fee4688d5
ms.sourcegitcommit: debaff72dbfae91b303f0acd42dd6d99e03135a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96419818"
---
# <a name="connection-events"></a>连接事件

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Microsoft SqlClient Data Provider for SQL Server 中的 Connection 对象有两个事件，可用于从数据源中检索信息性消息或确定 Connection 的状态是否已被更改。 下表描述 Connection 对象的这些事件。

|事件|描述|  
|-----------|-----------------|  
|**InfoMessage**|当从数据源中返回信息性消息时发生。 信息性消息是数据源中不会引发异常的消息。|  
|**StateChange**|当 Connection 的状态更改时发生。|  

## <a name="working-with-the-infomessage-event"></a>使用 InfoMessage 事件

您可以使用 <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 对象的 <xref:Microsoft.Data.SqlClient.SqlConnection> 事件从 SQL Server 数据源中检索警告和信息性消息。 从数据源返回的严重程度为 11 到 16 的错误将引发异常。 但是，<xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 事件可用于从数据源中获取与错误无关联的消息。 对于 Microsoft SQL Server，任何严重程度等于或小于 10 的错误都将被视为信息性消息，将使用 <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 事件来捕获。 有关详细信息，请参阅[数据库引擎错误严重性](/sql/relational-databases/errors-events/database-engine-error-severities)一文。

<xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 事件接收 <xref:Microsoft.Data.SqlClient.SqlInfoMessageEventArgs> 对象，该对象在其 Errors 属性中包含来自数据源的消息的集合。 你可以查询此集合中的 Error 对象，以获取错误编号和消息文本以及错误的来源。 Microsoft SqlClient Data Provider for SQL Server 还包括有关该消息来自的数据库、存储过程和行号的详细信息。

### <a name="example"></a>示例

以下代码示例显示如何为 <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 事件添加事件处理程序。

[!code-csharp[SqlConnection_._InfoMessage#1](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#1)]

## <a name="handling-errors-as-infomessages"></a>将错误作为信息性消息处理

通常，只有从服务器发出的信息性消息和警告消息才会触发 <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 事件。 但是，真正的错误发生时，启动服务器操作的 ExecuteNonQuery 或 ExecuteReader 方法将暂停执行，并引发异常。

如果无论服务器生成任何错误都要继续处理命令中的语句的其他部分，请将 <xref:Microsoft.Data.SqlClient.SqlConnection.FireInfoMessageEventOnUserErrors%2A> 的 <xref:Microsoft.Data.SqlClient.SqlConnection> 属性设置为 `true`。 这样做会使连接对错误触发 <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 事件，而不是引发异常并中断处理。 客户端应用程序可以处理此事件并对错误情况做出响应。

> [!NOTE]
> 严重程度等于或大于 17 的错误会造成服务器停止处理命令，这种错误必须作为异常来处理。 在这种情况下，无论如何在 <xref:Microsoft.Data.SqlClient.SqlConnection.InfoMessage> 事件中处理该错误，都会引发异常。

## <a name="working-with-the-statechange-event"></a>使用 StateChange 事件

StateChange 事件在 Connection 的状态更改时发生。 StateChange 事件接收 <xref:System.Data.StateChangeEventArgs>，使你能够使用 OriginalState 和 CurrentState 属性来确定 Connection 状态的更改。 OriginalState 属性是一个 <xref:System.Data.ConnectionState> 枚举，指示更改前的 Connection 状态。 CurrentState 是一个 <xref:System.Data.ConnectionState> 枚举，指示更改后的 Connection 状态。

以下代码示例在 Connection 的状态更改时使用 StateChange 事件将消息写入控制台。

[!code-csharp[SqlConnection_._StateChange#2](~/../sqlclient/doc/samples/SqlConnection_InfoMessage_StateChange.cs#2)]

## <a name="see-also"></a>另请参阅

- [连接到数据源](connecting-to-data-source.md)
