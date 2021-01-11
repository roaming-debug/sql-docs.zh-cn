---
title: 从 DataAdapter 填充 DataSet
description: 了解如何在 ADO.NET 中从 DataAdapter 填充 DataSet，这提供了与数据源无关的一致关系编程模型。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 3fa0ac7d-e266-4954-bfac-3fbe2f913153
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e6c50bf7255dc77edfd0b93e03dedeec83ed4c4d
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771497"
---
# <a name="populate-a-dataset-from-a-dataadapter"></a>从 DataAdapter 填充 DataSet

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

ADO.NET <xref:System.Data.DataSet> 是数据常驻内存的表示形式，可提供与数据源无关的一致关系编程模型。 `DataSet` 表示整个数据集，其中包含表、约束和表之间的关系。 由于 `DataSet` 独立于数据源，因此 `DataSet` 可以包含应用程序本地的数据，也可以包含来自多个数据源的数据。 与现有数据源的交互通过 `DataAdapter`来控制。

`SelectCommand` 的 `DataAdapter` 属性是一个 `Command` 对象，用于从数据源中检索数据。 `InsertCommand`的 `UpdateCommand`、 `DeleteCommand` 和 `DataAdapter` 属性是 `Command` 对象，用于按照对 `DataSet`中数据的修改来管理对数据源中数据的更新。 [通过 DataAdapter 更新数据源](update-data-sources-with-dataadapters.md)中更详细地介绍了这些属性。

`Fill` 的 `DataAdapter` 方法用于使用 `DataSet` 的 `SelectCommand` 结果填充 `DataAdapter`。 `Fill` 将要填充的 `DataSet` 、和 `DataTable` 对象（或要使用从 `DataTable` 中返回的行来填充的 `SelectCommand`的名称）作为它的参数。

> [!NOTE]
> 使用 `DataAdapter` 检索表的全部内容会花费些时间，尤其是在表中有很多行时。 这是因为访问数据库，定位和处理数据，然后将数据传输到客户端是需要很长时间的。 将表中全部内容提取到客户端还会在服务器上锁定所有行。 若要提高性能，您可以使用 `WHERE` 子句使返回客户端的行数大为减少。 还可以通过只显式列出 `SELECT` 语句要求的列减少返回到客户端的数据量。 另一种好的变通方法是以批次检索行（例如一次检索几百行），并且在客户端完成当前批次后只检索下一批次。

`Fill` 方法使用 `DataReader` 对象来隐式地返回用于在 `DataSet`中创建表的列名称和类型，以及用于填充 `DataSet`中的表行的数据。 表和列仅在不存在时才创建；否则， `Fill` 将使用现有的 `DataSet` 架构。 列类型根据 [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)中的表创建为 .NET Framework 类型。 除非数据源和 `DataAdapter` 中存在主键，否则不会创建主键。`MissingSchemaAction` 设置为 `MissingSchemaAction` **.** `AddWithKey`。 如果 `Fill` 发现某个表存在主键，对于主键列的值与从数据源返回的行的主键列的值匹配的行，将使用数据源中的数据重写 `DataSet` 中的数据。 如果未找到任何主键，则将数据追加到 `DataSet`中的表。 `Fill` 使用填充 `DataSet` 时可能存在的任何映射（请参阅 [DataAdapter、DataTable 和 DataColumn 映射](dataadapter-datatable-datacolumn-mappings.md)）。

> [!NOTE]
> 如果 `SelectCommand` 返回 OUTER JOIN 的结果，则 `DataAdapter` 不会为生成的 `PrimaryKey` 设置 `DataTable`值。 您必须自己定义 `PrimaryKey` 以确保正确解析重复行。

以下代码示例创建了一个 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 实例，使用 Microsoft SQL Server <xref:Microsoft.Data.SqlClient.SqlConnection> 数据库的 `Northwind` 并使用客户列表填充 <xref:System.Data.DataTable> 中的 `DataSet` 。 向 <xref:Microsoft.Data.SqlClient.SqlConnection> 构造函数传递的 SQL 语句和 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 参数用于创建 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> 的 <xref:Microsoft.Data.SqlClient.SqlDataAdapter>属性。

## <a name="example"></a>示例

[!code-csharp[SqlDataAdapter_FillDataSet#1](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#1)]

> [!NOTE]
> 此示例中所示的代码不显式打开和关闭 `Connection`。 如果 `Fill` 方法发现连接尚未打开，它将隐式地打开 `Connection` 正在使用的 `DataAdapter` 。 如果 `Fill` 已打开连接，则它还将在 `Fill` 完成时关闭连接。 当处理单一操作（如 `Fill` 或 `Update`）时，这可以简化您的代码。 但是，如果您在执行多项需要打开连接的操作，则可以通过以下方式提高应用程序的性能：显式调用 `Open` 的 `Connection`方法，对数据源执行操作，然后调用 `Close` 的 `Connection`方法。 应尝试使数据源的连接打开的时间尽可能短，以便释放资源供其他客户端应用程序使用。

## <a name="multiple-result-sets"></a>多个结果集

如果 `DataAdapter` 遇到多个结果集，则将在 `DataSet`中创建多个表。 这些表的命名方式为默认名称 Table 加上 *N*，N 从 0 开始递增，如以 Table0 为第一个表名，依次类推。 如果以参数形式向 `Fill` 方法传递表名，则这些表的命名方式为默认名称 TableName 加上 *N*，N 从 0 开始递增，如以 TableName0 为第一个表名，依次类推。  
  
## <a name="populate-a-dataset-from-multiple-dataadapters"></a>从多个 DataAdapter 填充 DataSet  

一个 `DataSet` 可以与任意数量的 `DataAdapter` 对象一起使用。 每个 `DataAdapter` 都可用于填充一个或多个 `DataTable` 对象并将更新解析回相关数据源。 `DataRelation` 和 `Constraint` 对象可以在本地添加到 `DataSet` ，这样您就可以关联来自不同数据源的数据。 例如， `DataSet` 可以包含来自 Microsoft SQL Server 数据库、通过 OLE DB 公开的 IBM DB2 数据库以及对 XML 进行流处理的数据源的数据。 一个或多个 `DataAdapter` 对象可以处理与每个数据源的通信。  
  
### <a name="example"></a>示例  

以下代码示例从 Microsoft SQL Server 上的 `Northwind` 数据库填充客户列表，从存储在 Microsoft Access 上的 `Northwind` 数据库填充订单列表。 已填充的表通过 `DataRelation`相关联，这样，客户列表将与相应客户的订单一起显示出来。

[!code-csharp[SqlDataAdapter_FillDataSet#2](~/../sqlclient/doc/samples/SqlDataAdapter_FillDataSet.cs#2)]

## <a name="sql-server-decimal-type"></a>SQL Server Decimal 类型

默认情况下，`DataSet` 使用 .NET 数据类型存储数据。 对于大多数应用程序，这些类型都提供了一种方便的数据源信息表示形式。 但是，当数据源中的数据类型是 SQL Server decimal 或 numeric 数据类型时，这种表示形式可能会导致问题。 .NET `decimal` 数据类型最多允许 28 个有效数字，而 SQL Server `decimal` 数据类型则允许 38 个有效数字。 如果 `SqlDataAdapter` 在 `Fill` 操作过程中确定 SQL Server `decimal` 字段的精度大于 28 个字符，则不会将当前行添加到 `DataTable`。 此时将发生 `FillError` 事件，使您能够确定是否将发生精度损失并作出适当的响应。 有关 `FillError` 事件的详细信息，请参阅[处理 DataAdapter 事件](handle-dataadapter-events.md)。 若要获取 SQL Server `decimal` 值，还可以使用 <xref:Microsoft.Data.SqlClient.SqlDataReader> 对象并调用 <xref:Microsoft.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A> 方法。

ADO.NET 还增强了对 `DataSet` 中的 <xref:System.Data.SqlTypes> 的支持。 有关详细信息，请参阅 [SqlTypes and the DataSet](./sql/sqltypes-dataset.md)。

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)
- [多重活动结果集 (MARS)](./sql/multiple-active-result-sets-mars.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
