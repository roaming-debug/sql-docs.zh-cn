---
title: 批处理使用 DataAdapter 的操作
description: 介绍从 DataSet 应用更新时通过减少到 SQL Server 的往返次数来提高应用程序性能。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: e72ed5af-b24f-486c-8429-c8fd2208f844
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1b720dff0678c68396fe7d8cf1f60f3ade70dd9d
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772206"
---
# <a name="batch-operations-using-dataadapters"></a>批处理使用 DataAdapter 的操作

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

通过 ADO.NET 中的批处理支持，<xref:System.Data.Common.DataAdapter> 可以将 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 中的 INSERT、UPDATE 和 DELETE 操作分组发向服务器，而不是每次发送一项操作。 因为减少了与服务器的往返次数，通常可以大大提高性能。 Microsoft SqlClient data provider for SQL Server (<xref:Microsoft.Data.SqlClient>) 支持批量更新。

在 ADO.NET 的以前版本中用 <xref:System.Data.DataSet> 中的更改更新数据库时，`Update` 的 `DataAdapter` 方法执行一次会向数据库中更新一行。 当该方法循环访问指定 <xref:System.Data.DataTable> 中的各行时，它会检查每个 <xref:System.Data.DataRow> 以查看其是否已被修改。 如果行已被修改，它会调用相应的 `UpdateCommand`、`InsertCommand` 或 `DeleteCommand`，具体取决于该行的 <xref:System.Data.DataRow.RowState%2A> 属性值。 每行更新都需要通过网络往返访问一次数据库。

在 Microsoft SqlClient Data Provider for SQL Server 中，<xref:Microsoft.Data.SqlClient.SqlDataAdapter> 公开了 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateBatchSize%2A> 属性。 将 `UpdateBatchSize` 设置为正整数值可使对数据库的更新以指定大小的批处理形式发送。 例如，将 `UpdateBatchSize` 设置为 10 可将 10 个单独的语句编成一组并作为单个批处理进行提交。 将 `UpdateBatchSize` 设置为 0 可使 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 使用服务器能够处理的最大批大小。 将其设置为 1 可禁用批处理更新，因为这时一次只发送一行。

> [!NOTE]
> 执行极大的批处理会降低性能。 因此，在实现应用程序前应进行测试以得到最佳的批大小。

## <a name="use-the-updatebatchsize-property"></a>使用 UpdateBatchSize 属性

启用批处理更新时，的 <xref:System.Data.IDbCommand.UpdatedRowSource%2A>、`UpdateCommand` 和 `InsertCommand` 的 `DeleteCommand` 属性值应设置为 <xref:System.Data.UpdateRowSource.None> 或 <xref:System.Data.UpdateRowSource.OutputParameters>。 执行批处理更新时，命令的 <xref:System.Data.IDbCommand.UpdatedRowSource%2A> 或 <xref:System.Data.UpdateRowSource.FirstReturnedRecord> 的 <xref:System.Data.UpdateRowSource.Both> 属性值无效。
  
下面的过程演示 `UpdateBatchSize` 属性的用法。 该过程采用两个自变量，一个是 <xref:System.Data.DataSet> 对象，它具有表示 Production.ProductCategory 表中 ProductCategoryID 和 Name 字段的列；另一个是表示批大小的整数（批处理中的行数）  。 代码创建一个新的 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 对象，并设置其 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A>、<xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 和 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> 属性。 代码假定 <xref:System.Data.DataSet> 对象具有经过修改的行。 它设置 `UpdateBatchSize` 属性并执行更新。

[!code-csharp[SqlDataAdapter_Batch#1](~/../sqlclient/doc/samples/SqlDataAdapter_Batch.cs#1)]

## <a name="handle-batch-update-related-events-and-errors"></a>处理与批量更新相关的事件和错误

DataAdapter 包含两个与更新有关的事件：RowUpdating 和 RowUpdated 。 有关详细信息，请参阅[处理 DataAdapter 事件](handle-dataadapter-events.md)。

### <a name="event-behavior-changes-with-batch-updates"></a>使用批量更新的事件行为变更

启用批处理时，在单个数据库操作中可更新多行。 因此，每个批处理只发生一次 `RowUpdated` 事件，而对于处理每一行，`RowUpdating` 事件都会发生。 禁用批处理时，这两个事件一对一交错触发，即一行触发一个 `RowUpdating` 事件和一个 `RowUpdated` 事件，下一行触发一个 `RowUpdating` 事件和一个 `RowUpdated` 事件，直到处理完所有行。

### <a name="access-updated-rows"></a>访问更新的行

禁用批处理时，可以使用 <xref:System.Data.Common.RowUpdatedEventArgs.Row%2A> 类的 <xref:System.Data.Common.RowUpdatedEventArgs> 属性访问要进行更新的行。

启用批处理时，会为多行生成单个 `RowUpdated` 事件。 因此，每一行的 `Row` 属性值为空。 但仍会为每一行生成 `RowUpdating` 事件。 使用 <xref:System.Data.Common.RowUpdatedEventArgs.CopyToRows%2A> 类的 <xref:System.Data.Common.RowUpdatedEventArgs> 方法可以通过将对行的引用复制到一个数组来访问已处理的行。 如果没有要进行处理的行，`CopyToRows` 将引发一个 <xref:System.ArgumentNullException>。 在调用 <xref:System.Data.Common.RowUpdatedEventArgs.RowCount%2A> 方法之前，使用 <xref:System.Data.Common.RowUpdatedEventArgs.CopyToRows%2A> 属性可返回已处理行的数目。

### <a name="handle-data-errors"></a>处理数据错误

执行批处理与执行每个单独的语句具有相同的效果。 各语句按照其添加到批处理中的顺序执行。 在批处理模式下处理错误的方式与禁用批处理模式时相同。 每一行均单独处理。 只有在数据库中经过成功处理的行才能在 <xref:System.Data.DataRow> 内的相应 <xref:System.Data.DataTable> 中更新。

> [!NOTE]
> Microsoft SqlClient Data Provider SQL Server 和后端数据库服务器确定哪些 SQL 构造支持执行批处理。 如果为执行提交了不支持的语句，则可能引发异常。

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [通过 DataAdapter 更新数据源](update-data-sources-with-dataadapters.md)
- [处理 DataAdapter 事件](handle-dataadapter-events.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
