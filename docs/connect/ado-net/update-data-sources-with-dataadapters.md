---
title: 通过 DataAdapter 更新数据源
description: 了解 DataAdapter 的 Update 方法如何将 DataSet 中的更改解析回 ADO.NET 应用程序中的数据源。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: d1bd9a8c-0e29-40e3-bda8-d89176b72fb1
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 6f2feb876d0f232f4d7951de8ee1cc84587e6486
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771323"
---
# <a name="update-data-sources-with-dataadapters"></a>通过 DataAdapter 更新数据源

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

调用 `Update` 的 <xref:System.Data.Common.DataAdapter> 方法可以将 <xref:System.Data.DataSet> 中的更改解析回数据源。 与 `Update` 方法类似，`Fill` 方法将 `DataSet` 的实例和可选的 <xref:System.Data.DataTable> 对象或 `DataTable` 名称用作自变量。 `DataSet` 实例是包含已做的更改的 `DataSet`，`DataTable` 标识从其中检索这些更改的表。 如果未指定 `DataTable`，则使用 `DataTable` 中的第一个 `DataSet`。

当调用 `Update` 方法时，`DataAdapter` 会分析已做的更改并执行相应的命令（INSERT、UPDATE 或 DELETE）。 当 `DataAdapter` 遇到对 <xref:System.Data.DataRow> 所做的更改时，它将使用 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>、<xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> 或 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> 来处理该更改。

通过使用这些属性，可以在设计时指定命令语法，并在可能的情况下使用存储过程，从而最大程度地提高 ADO.NET 应用程序的性能。 在调用 `Update` 之前，必须显式设置这些命令。 如果调用了 `Update` 但不存在用于特定更新的相应命令（例如，不存在用于已删除行的 `DeleteCommand`），则会引发异常。

> [!IMPORTANT]
> 如果使用 SQL Server 存储过程来编辑或删除使用 `DataAdapter` 的数据，请确保在存储过程定义中不使用 `SET NOCOUNT ON`。 这将使返回的受影响的行数为零，`DataAdapter` 会将其解释为并发冲突。 在这种情况下，将引发 <xref:System.Data.DBConcurrencyException>。

可以使用命令参数为 `DataSet` 中每个已修改的行指定 SQL 语句或存储过程的输入和输出值。 有关详细信息，请参阅 [DataAdapter 参数](dataadapter-parameters.md)。

> [!NOTE]
> 必须了解在 <xref:System.Data.DataTable> 中删除行和移除行之间的差异。 当调用 `Remove` 或 `RemoveAt` 方法时，会立即移除该行。 如果随后将 `DataTable` 或 `DataSet` 传递到 `DataAdapter` 并调用 `Update`，则后端数据源中的任何对应行都不会受到影响。 当您使用 `Delete` 方法时，该行仍将保留在 `DataTable` 中并会标记为删除。 如果随后将 `DataTable` 或 `DataSet` 传递到 `DataAdapter` 并调用 `Update`，则后端数据源中对应的行会被删除。

如果 `DataTable` 映射到单个数据库表或从单个数据库表生成，则可以利用 <xref:System.Data.Common.DbCommandBuilder> 对象为 `DeleteCommand` 自动生成 `InsertCommand`、`UpdateCommand` 和 `DataAdapter` 对象。 有关详细信息，请参阅[使用 CommandBuilders 生成命令](generate-commands-with-commandbuilders.md)。

## <a name="use-updatedrowsource-to-map-values-to-a-dataset"></a>使用 UpdatedRowSource 将值映射到 DataSet

通过使用 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象的 <xref:Microsoft.Data.SqlClient.SqlCommand.UpdatedRowSource%2A> 属性，可以控制如何在调用 `DataAdapter` 的 <xref:System.Data.Common.DbDataAdapter.Update%2A> 方法之后将从数据源返回的值映射回 `DataTable`。 通过将 `UpdatedRowSource` 属性设置为 <xref:System.Data.UpdateRowSource> 枚举值之一，您可以控制是忽略由 `DataAdapter` 命令返回的输出参数还是将其应用于 `DataSet` 中已更改的行。 还可以指定是否将返回的第一行（如果存在）应用于 `DataTable` 中已更改的行。

下表说明 `UpdateRowSource` 枚举的不同值，并说明它们如何影响与 `DataAdapter` 一起使用的命令的行为。

|UpdatedRowSource 枚举|描述|
|----------------------------------|-----------------|
|<xref:System.Data.UpdateRowSource.Both>|输出参数和返回的结果集的第一行都可以映射到 `DataSet` 中已更改的行。|
|<xref:System.Data.UpdateRowSource.FirstReturnedRecord>|只有返回的结果集的第一行中的数据才可以映射到 `DataSet` 中已更改的行。|
|<xref:System.Data.UpdateRowSource.None>|忽略任何输出参数或返回的结果集中的行。|
|<xref:System.Data.UpdateRowSource.OutputParameters>|只有输出参数才可以映射到 `DataSet` 中已更改的行。|

`Update` 方法会将更改解析回数据源；但在上次填充 `DataSet` 后，其他客户端可能已修改了数据源中的数据。 若要使用当前数据刷新 `DataSet`，请使用 `DataAdapter` 和 `Fill` 方法。 新行将添加到该表中，更新的信息将并入现有行。

`Fill` 方法通过检查 `DataSet` 中行的主键值以及 `SelectCommand` 返回的行来确定是要添加新行还是更新现有行。 如果 `Fill` 方法遇到 `DataSet` 中某行的主键值与 `SelectCommand` 返回结果中某行的主键值相匹配，则它将用 `SelectCommand` 返回的行中的信息更新现有行，并将现有行的 <xref:System.Data.DataRow.RowState%2A> 设置为 `Unchanged`。 如果 `SelectCommand` 返回的行所具有的主键值与 `DataSet` 中行的任何主键值都不匹配，则 `Fill` 方法将添加 `RowState` 为 `Unchanged` 的新行。

> [!NOTE]
> 如果 `SelectCommand` 返回 OUTER JOIN 的结果，则 `DataAdapter` 将不会为生成的 `DataTable` 设置 `PrimaryKey` 值。 您必须自己定义 `PrimaryKey` 以确保正确解析重复行。

若要处理在调用 `Update` 方法时可能发生的异常，可以使用 `RowUpdated` 事件响应更新行时发生的错误（请参阅[处理 DataAdapter 事件](handle-dataadapter-events.md)），也可以在调用 `Update` 之前将 <xref:System.Data.Common.DataAdapter.ContinueUpdateOnError%2A> 设置为 `true`，并在更新完成后响应特定行的 `RowError` 属性中存储的错误信息。

> [!NOTE]
> 对 `DataSet`、`DataTable` 或 `DataRow` 调用 `AcceptChanges` 将导致 `DataRow` 的所有 `Original` 值被 `DataRow` 的 `Current` 值覆盖。 如果修改了唯一标识该行的字段值，则在调用 `AcceptChanges` 后，`Original` 值将不再匹配数据源中的值。 在对 `DataAdapter` 的 `Update` 方法的调用过程中，将为每一行自动调用 `AcceptChanges`。 在调用 Update 方法期间，通过先将 `AcceptChangesDuringUpdate` 的 `DataAdapter` 属性设置为 false，或为 `RowUpdated` 事件创建一个事件处理程序并将 <xref:System.Data.Common.RowUpdatedEventArgs.Status%2A> 设置为 <xref:System.Data.UpdateStatus.SkipCurrentRow>，可以保留原始值。 有关详细信息，请参阅[处理 DataAdapter 事件](handle-dataadapter-events.md)。

下面的示例演示如何通过显式设置 `DataAdapter` 的 `UpdateCommand` 并调用其 `Update` 方法对已修改的行执行更新。

> [!NOTE]
> `UPDATE statement` 的 `WHERE clause` 中指定的参数设置为使用 `SourceColumn` 的 `Original` 值。 这一点很重要，因为 `Current` 值可能已被修改，可能会不匹配数据源中的值。 `Original` 值是用于从数据源填充 `DataTable` 的值。

[!code-csharp[SqlDataAdapter_Update#1](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#1)]

## <a name="autoincrement-columns"></a>AutoIncrement 列

如果数据源中的表具有自动递增列，则可以通过以下方式填充 `DataSet` 中的列：作为存储过程的输出参数返回自动递增值并将其映射到表中的一列、返回由存储过程或 SQL 语句返回的结果集第一行中的自动递增值或者使用 `RowUpdated` 的 `DataAdapter` 事件来执行其他 SELECT 语句。 有关详细信息和示例，请参阅[检索标识或自动编号值](retrieve-identity-or-autonumber-values.md)。

## <a name="ordering-of-inserts-updates-and-deletes"></a>插入、更新和删除的顺序

在许多情况下，以何种顺序向数据源发送通过 `DataSet` 所做的更改是非常重要的。 例如，如果更新了现有行的主键值，并且添加了以新主键值作为外键的新行，则务必要在处理插入之前处理更新。

可以使用 `Select` 的 `DataTable` 方法来返回仅引用具有特定 `DataRow` 的 `RowState` 数组。 然后可以将返回的 `DataRow` 数组传递给 `Update` 的 `DataAdapter` 方法来处理已修改的行。 通过指定要更新的行的子集，可以控制处理插入、更新和删除的顺序。

## <a name="example"></a>示例

例如，以下代码确保首先处理表中已删除的行，然后处理已更新的行，然后处理已插入的行。

[!code-csharp[SqlDataAdapter_Update#2](~/../sqlclient/doc/samples/SqlDataAdapter_Update.cs#2)]

## <a name="use-a-dataadapter-to-retrieve-and-update-data"></a>使用 DataAdapter 来检索和更新数据

您可以使用 DataAdapter 来检索和更新数据。

- 该示例使用 `DataAdapter.AcceptChangesDuringFill` 克隆数据库中的数据。 如果将属性设置为 false，则在填充表时不会调用 AcceptChanges，并且新添加的行将被视为插入的行 。 因此，此示例使用这些行将新行插到数据库中。

- 这些示例使用 `DataAdapter.TableMappings` 来定义源表和 DataTable 之间的映射。

- 该示例使用 `DataAdapter.FillLoadOption` 来确定适配器如何从 DbDataReader 填充 DataTable 。 创建 DataTable 时，只能通过将属性设置为 LoadOption.Upsert 或 LoadOption.PreserveChanges 来将数据从数据库写入当前版本或原始版本 。

- 该示例还将通过使用 `DbDataAdapter.UpdateBatchSize` 执行批处理操作来更新表。

在编译并运行此示例之前，您需要创建示例数据库：

```sql
USE [master]
GO

CREATE DATABASE [MySchool]

GO

USE [MySchool]
GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Course]([CourseID] [nvarchar](10) NOT NULL,
[Year] [smallint] NOT NULL,
[Title] [nvarchar](100) NOT NULL,
[Credits] [int] NOT NULL,
[DepartmentID] [int] NOT NULL,
 CONSTRAINT [PK_Course] PRIMARY KEY CLUSTERED
(
[CourseID] ASC,
[Year] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[Department]([DepartmentID] [int] IDENTITY(1,1) NOT NULL,
[Name] [nvarchar](50) NOT NULL,
[Budget] [money] NOT NULL,
[StartDate] [datetime] NOT NULL,
[Administrator] [int] NULL,
 CONSTRAINT [PK_Department] PRIMARY KEY CLUSTERED
(
[DepartmentID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY]

GO

INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1045', 2012, N'Calculus', 4, 7)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C1061', 2012, N'Physics', 4, 1)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2021', 2012, N'Composition', 3, 2)
INSERT [dbo].[Course] ([CourseID], [Year], [Title], [Credits], [DepartmentID]) VALUES (N'C2042', 2012, N'Literature', 4, 2)

SET IDENTITY_INSERT [dbo].[Department] ON

INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (1, N'Engineering', 350000.0000, CAST(0x0000999C00000000 AS DateTime), 2)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (2, N'English', 120000.0000, CAST(0x0000999C00000000 AS DateTime), 6)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (4, N'Economics', 200000.0000, CAST(0x0000999C00000000 AS DateTime), 4)
INSERT [dbo].[Department] ([DepartmentID], [Name], [Budget], [StartDate], [Administrator]) VALUES (7, N'Mathematics', 250024.0000, CAST(0x0000999C00000000 AS DateTime), 3)
SET IDENTITY_INSERT [dbo].[Department] OFF

ALTER TABLE [dbo].[Course]  WITH CHECK ADD  CONSTRAINT [FK_Course_Department] FOREIGN KEY([DepartmentID])
REFERENCES [dbo].[Department] ([DepartmentID])
GO
ALTER TABLE [dbo].[Course] CHECK CONSTRAINT [FK_Course_Department]
GO
```

[!code-csharp[SqlDataAdapter_Properties#1](~/../sqlclient/doc/samples/SqlDataAdapter_Properties.cs#1)]

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [检索标识或自动编号值](retrieve-identity-or-autonumber-values.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
