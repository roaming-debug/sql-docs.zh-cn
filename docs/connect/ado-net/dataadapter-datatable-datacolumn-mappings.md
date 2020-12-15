---
title: DataAdapter、DataTable 和 DataColumn 映射
description: 介绍如何为 DataAdapter 设置 DataTableMapping 和 ColumnMapping。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: d023260a-a66a-4c39-b8f4-090cd130e730
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e8b2f84fbbf888fc67cdb8f945d7f0c887c14eaa
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772201"
---
# <a name="dataadapter-datatable-and-datacolumn-mappings"></a>DataAdapter、DataTable 和 DataColumn 映射

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:Microsoft.Data.SqlClient.SqlDataAdapter> 在其 <xref:System.Data.Common.DataAdapter.TableMappings%2A> 属性中包含零个或更多个 <xref:System.Data.Common.DataTableMapping> 对象的集合。 DataTableMapping 提供对数据源的查询所返回的数据与 <xref:System.Data.DataTable> 之间的主映射。 DataTableMapping 名称可以代替 DataTable 名称传递到 DataAdapter 的 <xref:System.Data.Common.DbDataAdapter.Fill%2A> 方法  。 以下示例为 Authors 表创建名为 AuthorsMapping 的 DataTableMapping  。

```csharp
workAdapter.TableMappings.Add("AuthorsMapping", "Authors");
```

DataTableMapping 使你能够使用 DataTable 中与数据库中的列名不同的列名 。 当该表被更新时，DataAdapter 将使用此映射来匹配列。

> [!NOTE]
> 如果在调用 DataAdapter 的 Fill 或 Update 方法时未指定 TableName 或 DataTableMapping 名称，DataAdapter 将查找名为“Table”的 DataTableMapping      。 如果该 DataTableMapping 不存在，DataTable 的 TableName 将为“Table”  。 可以通过创建名为“Table”的 DataTableMapping 来指定默认的 DataTableMapping 。

以下代码示例（从 <xref:System.Data.Common> 命名空间）创建一个 DataTableMapping 并通过将其命名为“Table”来使其成为指定 DataAdapter 的默认映射 。 然后，该示例将查询结果中第一个表（Northwind 数据库的 Customers 表）中的列映射到 <xref:System.Data.DataSet> 的 Northwind Customers 表中的一组更为易记的名称  。 对于未映射的列，将使用数据源中的列名称。

[!code-csharp[SqlDataAdapter_TableMappings#1](~/../sqlclient/doc/samples/SqlDataAdapter_TableMappings.cs#1)]

在更为先进的情况下，可以决定需要使用相同的 DataAdapter 来支持为不同的表加载不同的映射。 若要完成此任务，请添加附加的 DataTableMapping 对象。

当 Fill 方法以 DataSet 实例和 DataTableMapping 名称的形式进行传递时，如果存在具有该名称的映射，则使用该映射；否则将使用具有该名称的 DataTable   。

以下示例创建一个名称为 Customers 而 DataTable 名称为 BizTalkSchema 的 DataTableMapping   。 然后，该示例将 SELECT 语句所返回的行映射到 BizTalkSchema DataTable 。

[!code-csharp[SqlDataAdapter_TableMappings#2](~/../sqlclient/doc/samples/SqlDataAdapter_TableMappings.cs#2)]

> [!NOTE]
> 如果没有为列映射提供源列名称，则会自动生成默认名称。 如果没有为列映射提供源列，则会给列映射提供递增的默认名称 SourceColumn N，这些名称从 SourceColumn1 开始。

> [!NOTE]
> 如果没有为表映射提供源表名称，则会自动生成默认名称。 如果没有为表映射提供源表名称，则会给该表映射提供递增的默认名称 SourceTable N，这些名称从 SourceTable1 开始。

> [!NOTE]
> 我们建议避免采用列映射的 SourceColumn N 的命名约定，或表映射的 SourceTable N 的命名约定，因为提供的名称可能会与 ColumnMappingCollection 中的现有默认列映射名称或 DataTableMappingCollection 中的现有默认表映射名称冲突 。 如果提供的名称已经存在，将引发异常。

## <a name="handle-multiple-result-sets"></a>处理多个结果集

如果 SelectCommand 返回多个表，Fill 将自动使用递增值为 DataSet 中的表生成表名称，这些表名称从指定表名称开始（从 TableName1 开始），并以 TableName N 格式继续   。 可以使用表映射将自动生成的表名称映射到要为 DataSet 中的表指定的名称。 例如，对于返回两个表（Customers 和 Orders）的 SelectCommand，可对 Fill 发出以下调用   。

```csharp
adapter.Fill(customersDataSet, "Customers");
```

DataSet 中创建了两个表：Customers 和 Customers1 。 可以使用表映射来确保第二个表名为 Orders 而不是 Customers1 。 若要完成此任务，请将 Customers1 的源表映射到 DataSet 表 Orders，如以下示例所示  。

[!code-csharp[SqlDataAdapter_TableMappings#3](~/../sqlclient/doc/samples/SqlDataAdapter_TableMappings.cs#3)]

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
