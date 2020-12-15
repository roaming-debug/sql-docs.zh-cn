---
title: DataAdapter 和 DataReader
description: 了解从数据库中检索数据的 Microsoft SqlClient Data Provider for SQL Server DataReader，以及从数据源检索数据并填充 DataSet 的 DataAdapter。
ms.date: 11/30/2020
ms.assetid: cc952ca2-ec19-46ab-9189-15174b52cb74
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e71f6218c9fecd956a7c287581caa72a1a787462
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772172"
---
# <a name="dataadapters-and-datareaders"></a>DataAdapter 和 DataReader

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

可以使用 Microsoft SqlClient Data Provider for SQL Server DataReader 从数据库中检索只读、只进的数据流。 查询结果在查询执行时返回，在并存储在客户端的网络缓冲区中，直到使用 DataReader 的 Read 方法对它们发出请求 。 使用 DataReader 可以提高应用程序的性能，原因是它只要数据可用就立即检索数据，并且（默认情况下）一次只在内存中存储一行，减少了系统开销。

<xref:System.Data.Common.DataAdapter> 用于从数据源检索数据并填充 <xref:System.Data.DataSet> 中的表。 `DataAdapter` 还可将对 `DataSet` 所做的更改解析回数据源。 `DataAdapter` 使用 Microsoft SqlClient Data Provider for SQL Server 的 `Connection` 对象连接到数据源，并使用 `Command` 对象从数据源检索数据并对其更改进行解析。

.NET 具有一个 <xref:System.Data.Common.DbDataReader> 和一个 <xref:System.Data.Common.DbDataAdapter> 对象：Microsoft SqlClient Data Provider for SQL Server 包括一个 <xref:Microsoft.Data.SqlClient.SqlDataReader> 和一个 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 对象。

## <a name="in-this-section"></a>在本节中

[按 DataReader 检索数据](retrieve-data-by-datareader.md)  
描述 ADO.NET DataReader 对象，并说明如何使用它从数据源返回结果流。

[从 DataAdapter 填充 DataSet](populate-dataset-from-dataadapter.md)  
说明如何通过 `DataSet` 使用表、列和行填充 `DataAdapter`。

[DataAdapter 参数](dataadapter-parameters.md)  
说明如何与 `DataAdapter` 的命令属性一起使用参数，包括如何将 `DataSet` 的列内容映射到命令参数。

[将现有约束添加到 DataSet](add-existing-constraints-to-dataset.md)  
说明如何将现有约束添加到 `DataSet`。

[DataAdapter、DataTable 和 DataColumn 映射](dataadapter-datatable-datacolumn-mappings.md)  
说明如何为 `DataTableMappings` 设置 `ColumnMappings` 和 `DataAdapter`。

[通过查询结果分页](paging-through-query-result.md)  
提供一个以数据页形式查看查询结果的示例。

[通过 DataAdapter 更新数据源](update-data-sources-with-dataadapters.md)  
说明如何使用 `DataAdapter` 将 `DataSet` 中的更改解析回数据库。

[处理 DataAdapter 事件](handle-dataadapter-events.md)  
说明 `DataAdapter` 事件以及如何使用这些事件。

[批处理使用 DataAdapter 的操作](batch-operations-using-dataadapters.md)  
说明在从 `DataSet` 应用更新时，如何通过减少与 SQL Server 之间的往返次数来提高应用程序的性能。

## <a name="see-also"></a>另请参阅

- [连接到数据源](connecting-to-data-source.md)
- [命令和参数](commands-parameters.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
