---
title: 通过查询结果分页
description: 提供一个以数据页形式查看查询结果的示例。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: fa360c46-e5f8-411e-a711-46997771133d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 2de305e1069964f2d1a67b7f201578c7448d3e09
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772168"
---
# <a name="paging-through-a-query-result"></a>通过查询结果分页

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

查询结果分页是以较小数据子集（即页）的形式返回查询结果的过程。 它通常用于以易于管理的小块形式向用户显示结果。

<xref:Microsoft.Data.SqlClient.SqlDataAdapter> 提供了通过 <xref:System.Data.Common.DbDataAdapter.Fill%2A> 方法的重载来仅返回一页数据的功能。 但是，对于大量的查询结果，它可能并不是首选的分页方法，因为 DataAdapter 虽然仅使用所请求的记录来填充目标 <xref:System.Data.DataTable> 或 <xref:System.Data.DataSet>，但仍会使用返回整个查询的资源。

若要在从数据源中返回一页数据时不使用返回整个查询的资源，请为查询指定附加条件，使返回的行数减少到只返回所需的行。

若要使用 <xref:System.Data.Common.DbDataAdapter.Fill%2A> 方法返回一页数据，请指定 startRecord 参数（代表该数据页中的第一个记录），并指定 maxRecords 参数（代表该数据页中的记录数） 。

## <a name="example"></a>示例

以下代码示例显示如何使用 <xref:System.Data.Common.DbDataAdapter.Fill%2A> 方法来返回查询结果（页大小为 5 个记录）的第一页。

[!code-csharp[SqlDataAdapter_Paging#1](~/../sqlclient/doc/samples/SqlDataAdapter_Paging.cs#1)]

在上例中，DataSet 只填充了 5 个记录，但却返回了整个 Orders 表 。 若要用相同的 5 个记录填充 DataSet 但仅返回这 5 个记录，请在 SQL 语句中使用 `TOP` 和 `WHERE` 子句，如以下代码示例所示。

[!code-csharp[SqlDataAdapter_Paging#2](~/../sqlclient/doc/samples/SqlDataAdapter_Paging.cs#2)]

> [!NOTE]
> 当通过这种方式分页浏览查询结果时，必须保留对行进行排序的 `unique identifier`，以便将唯一 ID 传递给命令以返回下一页记录，如下面的代码示例所示。

[!code-csharp[SqlDataAdapter_Paging#4](~/../sqlclient/doc/samples/SqlDataAdapter_Paging.cs#4)]

若要使用接受 startRecord 和 maxRecords 参数的 Fill 方法的重载来返回 `next page of records`，请使当前记录索引按页大小递增，并填充该表  。

> [!NOTE]
> 请记住，即使仅在 DataSet 中添加一页记录，数据库服务器仍会返回全部查询结果。

在以下代码示例中，先清除表行，然后再用下一页数据填充这些表行。 您可能需要在本地缓存中保留一定数量的返回行，以减少与数据库服务器的往返次数。

[!code-csharp[SqlDataAdapter_Paging#3](~/../sqlclient/doc/samples/SqlDataAdapter_Paging.cs#3)]

若要返回下一页记录而不让数据库服务器返回整个查询，请指定对 SQL SELECT 语句的限制条件。 由于上例保留了返回的最后一个记录，因此可以在 WHERE 子句中使用它来指定查询的起点，如以下代码示例所示。

[!code-csharp[SqlDataAdapter_Paging#5](~/../sqlclient/doc/samples/SqlDataAdapter_Paging.cs#5)]

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
