---
title: 将现有约束添加到 DataSet
description: 介绍如何将现有约束添加到 DataSet。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 307d2809-208b-4cf8-b6a9-5d16f15fc16c
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 9471f62c36604cb01ea78f558ac3bfbcb7f4bc01
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772210"
---
# <a name="add-existing-constraints-to-a-dataset"></a>将现有约束添加到 DataSet

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:Microsoft.Data.SqlClient.SqlDataAdapter> 的 <xref:System.Data.Common.DbDataAdapter.Fill%2A> 方法仅使用数据源中的表列和表行来填充 <xref:System.Data.DataSet>；虽然约束通常由数据源来设置，但在默认情况下，Fill 方法不会将此架构信息添加到 DataSet 中 。

若要使用数据源中的现有主键约束信息填充 DataSet，则可以调用 DataAdapter 的 <xref:System.Data.Common.DbDataAdapter.FillSchema%2A> 方法，或者在调用 Fill 之前将 DataAdapter 的 <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> 属性设置为 AddWithKey    。 这将确保 DataSet 中的主键约束反映数据源中的主键约束。

> [!NOTE]
> 外键约束信息不包含在内，必须显式创建。

如果在使用数据填充 DataSet 之前向其中添加架构信息，可以确保将主键约束与 DataSet 中的 <xref:System.Data.DataTable> 对象包含在一起 。 这样，当再次调用来填充 DataSet 时，将使用主键列信息将数据源中的新行与每个 DataTable 中的当前行相匹配，并使用数据源中的数据改写表中的当前数据 。 如果没有架构信息，来自数据源的新行将追加到 DataSet 中，从而导致重复的行。

> [!NOTE]
> 如果数据源中的某列被标识为自动递增列，则 <xref:System.Data.Common.DbDataAdapter.FillSchema%2A> 方法或 <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> 为 AddWithKey 的 <xref:System.Data.Common.DbDataAdapter.Fill%2A> 方法将创建一个 AutoIncrement 属性设置为 `true` 的 DataColumn  。 不过，你需要手动设置 AutoIncrementStep 和 AutoIncrementSeed 值 。

> [!NOTE]
> 当使用 FillSchema 或将 MissingSchemaAction 设置为 AddWithKey 时，需要在数据源中进行额外的处理来确定主键列信息  。 这一额外的处理可能会降低性能。 如果主键信息在设计时已知，为了实现最佳性能，建议显式指定一个或多个主键列。

以下代码示例显示如何使用 <xref:System.Data.Common.DbDataAdapter.FillSchema%2A> 向 DataSet 添加架构信息：

[!code-csharp[SqlDataAdapter_FillDataSet#1](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#1)]

以下代码示例显示如何使用 <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> 属性和 <xref:System.Data.Common.DbDataAdapter.Fill%2A> 方法向 DataSet 添加架构信息：

[!code-csharp[SqlDataAdapter_FillDataSet#2](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#2)]

## <a name="handling-multiple-result-sets"></a>处理多个结果集

如果 DataAdapter 遇到从 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> 中返回的多个结果集，将在 DataSet 中创建多个表 。 将向这些表提供基于零的递增的默认名称 Table N，从“Table”开始，而不是从“Table0”开始。 如果以自变量形式向 <xref:System.Data.Common.DbDataAdapter.FillSchema%2A> 方法传递表名称，则将向这些表提供基于零的递增的名称 TableName N，从“TableName”开始，而不是从“TableName0”开始。

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
