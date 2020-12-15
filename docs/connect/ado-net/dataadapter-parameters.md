---
title: DataAdapter 参数
description: 了解 DbDataAdapter 的属性，这些属性从数据源返回数据并管理对数据源所做的更改。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: d72660a55fa047864148c90ae4087782302adc22
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96772170"
---
# <a name="dataadapter-parameters"></a>DataAdapter 参数

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

<xref:System.Data.Common.DbDataAdapter> 具有四个用于从数据源检索数据和更新数据源中数据的属性：<xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> 属性返回数据源中的数据；<xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>、<xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> 和 <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> 属性用于管理数据源中的更改。

> [!NOTE]
> 调用 `SelectCommand` 的 `Fill` 方法之前必须设置 `DataAdapter` 属性。 在调用 `InsertCommand` 的 `UpdateCommand` 方法之前必须设置 `DeleteCommand`、`Update` 或 `DataAdapter` 属性，具体取决于对 <xref:System.Data.DataTable> 中的数据做了哪些更改。 例如，如果已添加行，在调用 `InsertCommand` 之前必须设置 `Update`。 当 `Update` 正在处理已插入、已更新或已删除的行时，`DataAdapter` 将使用相应的 `Command` 属性来处理该操作。 有关已修改行的当前信息将通过 `Command` 集合传递到 `Parameters` 对象。

更新数据源中的行时，调用 UPDATE 语句，该语句使用唯一标识符来标识表中要更新的行。 该唯一标识符通常是主键字段的值。 UPDATE 语句使用的参数既包含唯一标识符又包含要更新的列和值，如下面的 Transact-SQL 语句所示。

```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  

> [!NOTE]
> 参数占位符的语法取决于数据源。 此示例显示 SQL Server 数据源的占位符。

在此示例中，`CompanyName` 字段使用 `CustomerID` 等于 `@CustomerID` 参数值的行中的 `@CompanyName` 参数值来进行更新。 这些参数使用 <xref:Microsoft.Data.SqlClient.SqlParameter> 对象的 <xref:Microsoft.Data.SqlClient.SqlParameter.SourceColumn%2A> 属性从已修改的行中检索相关信息。 下面是上一示例 UPDATE 语句的参数。 代码假定变量 `adapter` 表示有效的 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 对象。

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#2](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#2)]

`Add` 集合的 `Parameters` 方法接受参数的名称、数据类型、大小（如果适用于该类型）以及 <xref:System.Data.Common.DbParameter.SourceColumn%2A> 中的 `DataTable` 的名称。 请注意，<xref:System.Data.Common.DbParameter.SourceVersion%2A> 参数的 `@CustomerID` 设置为 `Original`。 这样可以保证，如果标识列的值已经在修改后的 <xref:System.Data.DataRow> 中被更改，就一定会更新数据源中的现有行。 在这种情况下，`Original` 行值将匹配数据源中的当前值，而 `Current` 行值将包含更新的值。 没有设置 `SourceVersion` 参数的 `@CompanyName`，而将使用默认的 `Current` 行值。

> [!NOTE]
> 对于 `DataAdapter` 的 `Fill` 操作和 `DataReader` 的 `Get` 方法，将从 Microsoft SqlClient Data Provider for SQL Server 中返回的类型来推断 .NET 类型。 推断的 .NET 类型和 Microsoft SQL Server 数据类型的访问器方法在 [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)中说明。

## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn，Parameter.SourceVersion

`SourceColumn` 和 `SourceVersion` 可以作为自变量传递给 `Parameter` 构造函数，也可以设置为现有 `Parameter` 的属性。 `SourceColumn` 是将要从中检索 <xref:System.Data.DataColumn> 值的 <xref:System.Data.DataRow> 中的 `Parameter` 的名称。 `SourceVersion` 指定 `DataRow` 用于检索该值的 `DataAdapter` 版本。

下表显示可以与 <xref:System.Data.DataRowVersion> 一起使用的 `SourceVersion` 枚举值。

|DataRowVersion 枚举|描述|  
|--------------------------------|-----------------|  
|`Current`|该参数使用列的当前值。 这是默认值。|  
|`Default`|该参数使用列的 `DefaultValue`。|  
|`Original`|该参数使用列的原始值。|  
|`Proposed`|该参数使用建议值。|  

下一节中的 `SqlClient` 代码示例为 <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> 定义了一个参数，在该示例中 `CustomerID` 列用作以下两个参数的 `SourceColumn`：`@CustomerID` (`SET CustomerID = @CustomerID`) 和 `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`)。 `@CustomerID` 参数用于将 CustomerID 列更新为 `DataRow` 中的当前值。 因此，使用 `SourceVersion` 为 `Current` 的 `CustomerID` `SourceColumn`。 `@OldCustomerID` 参数用于标识数据源中的当前行。 由于在该行的 `Original` 版本中找到了匹配列值，所以将使用 `SourceColumn` 为 `CustomerID` 的相同 `SourceVersion` (`Original`)。

## <a name="work-with-sqlclient-parameters"></a>使用 SqlClient 参数

下面的示例演示如何创建 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 并将 <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> 设置为 <xref:System.Data.MissingSchemaAction.AddWithKey>，以便从数据库中检索其他架构信息。 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>、<xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>、<xref:Microsoft.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> 和 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> 属性集及其相应的 <xref:Microsoft.Data.SqlClient.SqlParameter> 对象已添加到 <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A> 集合。 该方法返回一个 `SqlDataAdapter` 对象。

[!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter#1](~/../sqlclient/doc/samples/SqlDataAdapter_SqlDataAdapter.cs#1)]

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [命令和参数](commands-parameters.md)
- [通过 DataAdapter 更新数据源](update-data-sources-with-dataadapters.md)
- [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
