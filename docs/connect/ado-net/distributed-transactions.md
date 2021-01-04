---
title: 分布式事务
description: 介绍如何在 ADO.NET 中执行分布式事务
ms.date: 11/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 77ed8486f8b059f12fe7178826d81a743a97bbc4
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559229"
---
# <a name="distributed-transactions"></a>分布式事务

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

事务是一组相关的任务，作为独立于其他任务的独立单元成功（提交）或失败（中止）。 分布式事务是影响多个资源的事务。 要提交分布式事务，所有参与者都必须保证对数据的任何更改是永久的。 即使发生系统崩溃或其他不可预见的事件，更改也必须是永久的。 即使只有一个参与者无法保证这一点，整个事务也将失败，在事务范围内对数据的任何更改均将回滚。 

> [!NOTE]
> 如果 `DataReader` 在事务处于活动状态时启动，此时若尝试提交或回滚事务，将会引发异常。

## <a name="working-with-systemtransactions"></a>使用 System.Transactions

在 .NET 中，分布式事务通过 <xref:System.Transactions> 命名空间中的 API 进行管理。 如果涉及多个永久资源管理器，<xref:System.Transactions> API 会将分布式事务处理委托给事务监视器，例如 Microsoft 分布式事务协调程序 (MS DTC)。 有关详细信息，请参阅[事务基础知识](/dotnet/framework/data/transactions/transaction-fundamentals)。

ADO.NET 2.0 引入了对使用 `EnlistTransaction` 方法在分布式事务中进行登记的支持，该方法会登记 <xref:System.Transactions.Transaction> 实例中的连接。 在以前版本的 ADO.NET 中，分布式事务中的显式登记使用连接的 `EnlistDistributedTransaction` 方法执行，以登记 <xref:System.EnterpriseServices.ITransaction> 实例中的连接，为了向后兼容，也支持该方法。 有关企业服务事务的详细信息，请参阅[与企业服务和 COM+ 事务的互操作性](/dotnet/framework/data/transactions/interoperability-with-enterprise-services-and-com-transactions)。

在对 SQL Server 数据库使用 Microsoft SqlClient Data Provider for SQL Server 提供的 <xref:System.Transactions> 事务时，将自动使用轻型 <xref:System.Transactions.Transaction>。 该事务可以根据需要提升为完全分布式事务。 有关详细信息，请参阅[与 SQL Server 的 System.Transactions 集成](system-transactions-integration-with-sql-server.md)。

## <a name="automatically-enlisting-in-a-distributed-transaction"></a>自动在分布式事务中登记

自动登记是将 ADO.NET 连接与 `System.Transactions` 集成的默认（和首选）方法。 如果连接对象确定事务处于活动状态，用 `System.Transaction` 术语来说是指 `Transaction.Current` 不为 Null，则连接对象会自动在现有分布式事务中登记。 自动事务登记在连接打开时进行。 之后，即使在事务范围内执行命令，也不会进行自动事务登记。 你可以通过将 `Enlist=false` 指定为 <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> 的连接字符串参数来禁用现有事务的自动登记。

## <a name="manually-enlisting-in-a-distributed-transaction"></a>在分布式事务中手动登记

如果禁用自动登记，或者你需要登记在打开连接后启动的事务，则可以使用 Microsoft SqlClient Data Provider for SQL Server 的 <xref:Microsoft.Data.SqlClient.SqlConnection> 对象的 `EnlistTransaction` 方法在现有分布式事务中登记。 在现有分布式事务中登记可以确保当提交或回滚了事务时，也提交或回滚对数据源所做作的代码修改。

在分布式事务中登记尤其适用于为业务对象建立池连接。 如果业务对象使用打开的连接建立池连接，自动事务登记只有在该连接打开时才会进行。 如果使用池中的业务对象执行多个事务，则该对象的打开连接不自动登记在新启动的事务中。 在这种情况下，可以对该连接禁用自动事务登记，并使用 `EnlistTransaction` 在事务中登记连接。

`EnlistTransaction` 使用类型为 <xref:System.Transactions.Transaction> 的单一参数，该参数引用现有的事务。 在调用连接的 `EnlistTransaction` 方法之后，所有使用该连接在数据源上进行的修改均将加入事务中。 传递空值将取消该连接在当前分布式事务登记中的登记。 注意，在调用 `EnlistTransaction` 之前连接必须打开。

> [!NOTE]
> 在某个事务中显式登记了连接之后，在该事务完成之前，连接将无法取消登记或在另一个事务中登记。

> [!CAUTION]
> 如果连接已使用连接的 `EnlistTransaction` 方法开始了某个事务，<xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> 将引发异常。 但是，如果事务是在数据源上开始的本地事务（例如使用 <xref:Microsoft.Data.SqlClient.SqlCommand> 显式执行 BEGIN TRANSACTION 语句），`EnlistTransaction` 将回滚该本地事务并根据请求在现有分布式事务中登记。 你不会接收本地事务已回滚的通知，必须管理任何未使用 <xref:Microsoft.Data.SqlClient.SqlConnection.BeginTransaction%2A> 开始的本地事务。 如果你在 SQL Server 中使用 Microsoft SqlClient Data Provider for SQL Server，那么在尝试登记时将会引发异常。 所有其他情况将无法发现。  

## <a name="promotable-transactions-in-sql-server"></a>SQL Server 中的可提升事务

SQL Server 支持可提升的事务，在此类事务中，本地轻型事务可以仅在需要时自动提升为分布式事务。 可提升的事务不会调用分布式事务增加的系统开销，除非需要增加的系统开销。 有关详细信息和代码示例，请参阅[与 SQL Server 的 System.Transactions 集成](system-transactions-integration-with-sql-server.md)。

## <a name="configuring-distributed-transactions"></a>配置分布式事务

 为了使用分布式事务，您可能需要在网络上启用 MS DTC。 如果已启用 Windows 防火墙，则必须允许 MS DTC 服务使用网络或打开 MS DTC 端口。  
  
## <a name="see-also"></a>另请参阅

- [事务和并发性](transactions-and-concurrency.md)
- [System.Transactions 与 SQL Server 的集成](system-transactions-integration-with-sql-server.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
