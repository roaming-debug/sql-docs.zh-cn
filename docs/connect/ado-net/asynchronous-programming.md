---
title: 异步编程
description: 了解用于 SQL Server 的 Microsoft SqlClient 数据提供程序中的异步编程。
ms.date: 12/04/2020
ms.assetid: 85da7447-7125-426e-aa5f-438a290d1f77
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6eec681974499219a1ca649f9a47449a27ff4002
ms.sourcegitcommit: 5b2c47ce88f7e56552fd415c32b319009d043b56
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804307"
---
# <a name="asynchronous-programming"></a>异步编程

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

本主题讨论了用于 SQL Server 的 Microsoft SqlClient 数据提供程序 (SqlClient) 中的异步编程支持。

## <a name="legacy-asynchronous-programming"></a>旧异步编程

用于 SQL Server 的 Microsoft SqlClient 数据提供程序包括来自 System.Data.SqlClient 的方法，以便为迁移到 <xref:Microsoft.Data.SqlClient> 的应用程序保持向后兼容性。 不建议将以下旧异步编程方法用于新开发：

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteNonQuery%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteReader%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.BeginExecuteXmlReader%2A?displayProperty=nameWithType>

> [!TIP]
> 在用于 SQL Server 的 Microsoft SqlClient 数据提供程序中，这些旧方法不再需要连接字符串中的 `Asynchronous Processing=true`。

## <a name="asynchronous-programming-features"></a>异步编程功能

这些异步编程功能提供了一种简单的方法来实现代码异步。

有关 .NET 中异步编程的详细信息，请参阅：

- [C# 中的异步编程](/dotnet/csharp/async)

- [使用 Async 和 Await 的异步编程 (Visual Basic)](/dotnet/visual-basic/programming-guide/concepts/async/index)

当用户接口无响应或服务器无法扩展时，很可能需要使代码异步程度更高。 以前，编写异步代码涉及安装回调（也称为延续）来表示异步操作完成后发生的逻辑。 这将增加异步代码结构的复杂性（与同步代码相比）。

无需使用回调即可调用异步方法，也不需要跨多个方法或 lambda 表达式拆分代码。

`async` 修饰符用于指定异步方法。 调用 `async` 方法时，将返回一个任务。 将 `await` 运算符应用到任务时，当前方法会立即退出。 在该任务完成时，执行会在同一方法中恢复。

> [!TIP]
> 在用于 SQL Server 的 Microsoft SqlClient 数据提供程序中，无需异步调用即可设置 `Context Connection` 连接字符串关键字。

调用 `async` 方法不会分配任何附加线程。 结束时，它可以简单地使用现有 I/O 完成线程。

用于 SQL Server 的 Microsoft SqlClient 数据提供程序中的以下方法支持异步编程：

- <xref:System.Data.Common.DbConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteDbDataReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.GetFieldValueAsync%2A>

- <xref:System.Data.Common.DbDataReader.IsDBNullAsync%2A>

- <xref:System.Data.Common.DbDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:System.Data.Common.DbDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlConnection.OpenAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQueryAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalarAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteXmlReaderAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.NextResultAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.ReadAsync%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType>

其他异步成员支持 [SqlClient 流式处理支持](sqlclient-streaming-support.md)。

> [!TIP]
> 异步方法不需要连接字符串中的 `Asynchronous Processing=true`。 在用于 SQL Server 的 Microsoft SqlClient 数据提供程序中，此属性已过时。

### <a name="synchronous-to-asynchronous-connection-open"></a>同步到异步连接打开

可以升级现有应用程序以使用异步功能。 例如，假设应用程序具有同步连接算法，并在每次 UI 线程连接到数据库时加以阻止，连接后，该应用程序将调用向刚登录的用户之外的其他用户发送信号的存储过程。

[!code-csharp[SqlCommand_ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery.cs#1)]

转换为使用异步功能时，程序将如下所示：

[!code-csharp[SqlCommand_ExecuteNonQueryAsync#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQueryAsync.cs#1)]

### <a name="add-the-asynchronous-feature-in-an-existing-application-mixing-old-and-new-patterns"></a>在现有应用程序中添加异步功能（混合旧模式和新模式）

还可以添加异步功能 (SqlConnection::OpenAsync)，无需更改现有异步逻辑。 例如，如果应用程序当前使用：

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#1](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#1)]

可以开始使用异步模式，而不会显著更改现有算法。

[!code-csharp[SqlConnection_OpenAsync_ContinueWith#2](~/../sqlclient/doc/samples/SqlConnection_OpenAsync_ContinueWith.cs#2)]

### <a name="use-the-base-provider-model-and-the-asynchronous-feature"></a>使用基本提供程序模型和异步功能

您可能需要创建一个能够连接到不同数据库并执行查询的工具。 可以使用基本提供程序模型和异步功能。

必须在服务器上启用 Microsoft 分布式事务处理控制器 (MSDTC) 以使用分布式事务。 有关如何启用 MSDTC 的信息，请参阅[如何在 Web 服务器上启用 MSDTC](/previous-versions/commerce-server/dd327979(v=cs.90))。

[!code-csharp[SqlClient_AsyncProgramming_ProviderModel#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_ProviderModel.cs#1)]

### <a name="use-sql-transactions-and-the-new-asynchronous-feature"></a>使用 SQL 事务和新异步功能

[!code-csharp[SqlClient_AsyncProgramming_SqlTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlTransaction.cs#1)]

### <a name="use-distributed-transactions-and-the-new-asynchronous-feature"></a>使用分布式事务和新异步功能

在企业应用程序中，可能需要在某些情况下添加分布式事务，以支持多个数据库服务器之间的事务。 你可以使用 System.Transactions 命名空间并登记分布式事务，如下所示：

[!code-csharp[SqlClient_AsyncProgramming_DistributedTransaction#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_DistributedTransaction.cs#1)]

### <a name="cancel-an-asynchronous-operation"></a>取消异步操作

可通过使用 <xref:System.Threading.CancellationToken> 来取消异步请求。

[!code-csharp[SqlClient_AsyncProgramming_Cancellation#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_Cancellation.cs#1)]

### <a name="asynchronous-operations-with-sqlbulkcopy"></a>使用 SqlBulkCopy 的异步操作

在带有 <xref:Microsoft.Data.SqlClient.SqlBulkCopy.WriteToServerAsync%2A?displayProperty=nameWithType> 的 <xref:Microsoft.Data.SqlClient.SqlBulkCopy?displayProperty=nameWithType> 中也提供异步功能。

[!code-csharp[SqlClient_AsyncProgramming_SqlBulkCopy#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_SqlBulkCopy.cs#1)]

## <a name="asynchronously-use-multiple-commands-with-mars"></a>以异步方式将多个命令与 MARS 结合使用

该示例将打开与 AdventureWorks 数据库的单个连接。 使用 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象时，将创建一个 <xref:Microsoft.Data.SqlClient.SqlDataReader>。 使用阅读器时，打开第二个 <xref:Microsoft.Data.SqlClient.SqlDataReader>，使用第一个 <xref:Microsoft.Data.SqlClient.SqlDataReader> 的数据作为第二个阅读器的 WHERE 子句的输入。

> [!NOTE]
> 下面的示例使用 [AdventureWorks 示例数据库](../../samples/adventureworks-install-configure.md)。 示例代码中提供的连接字符串假定数据库已安装并且在本地计算机上可用。 根据环境需要修改连接字符串。

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommands#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommands.cs#1)]

## <a name="asynchronously-read-and-update-data-with-mars"></a>使用 MARS 以异步方式读取和更新数据

MARS 允许将连接用于读取操作和数据操作语言 (DML) 操作，其中有多个待处理操作。 此功能使应用程序无需处理连接繁忙错误。 此外，MARS 可以替换服务器端游标用户，这通常会消耗更多资源。 最后，因为可以在单个连接上执行多个操作，所以，这些操作可以共享相同的事务上下文，不需要使用 sp_getbindtoken 和 sp_bindsession 系统存储过程。

下面的控制台应用程序演示如何使用两个包含三个 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象的 <xref:Microsoft.Data.SqlClient.SqlDataReader> 对象和一个启用了 MARS 的 <xref:Microsoft.Data.SqlClient.SqlConnection> 对象。 第一个命令对象检索信用评级为 5 的供应商列表。 第二个命令对象使用 <xref:Microsoft.Data.SqlClient.SqlDataReader> 提供的供应商 ID 为第二个 <xref:Microsoft.Data.SqlClient.SqlDataReader> 加载特定供应商的所有产品。 每个产品记录由第二个 <xref:Microsoft.Data.SqlClient.SqlDataReader> 访问。 通过执行计算来确定新的 OnOrderQty。 然后，通过第三个命令对象来使用新值更新 ProductVendor 表。 整个过程发生在单个事务中，该事务在结束时回滚。

> [!NOTE]
> 下面的示例使用 [AdventureWorks 示例数据库](../../samples/adventureworks-install-configure.md)。 示例代码中提供的连接字符串假定数据库已安装并且在本地计算机上可用。 根据环境需要修改连接字符串。

[!code-csharp[SqlClient_AsyncProgramming_MultipleCommandsEx#1](~/../sqlclient/doc/samples/SqlClient_AsyncProgramming_MultipleCommandsEx.cs#1)]

## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
