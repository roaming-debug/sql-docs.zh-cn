---
title: 配置参数
description: 命令对象使用参数将值传递给 SQL 语句或存储过程，在 ADO.NET 中提供类型检查和验证。
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 537d8a2c-d40b-4000-83eb-bc1fcc93f707
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 12dfcb26484b468bed9f9262403806ea88829385
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595776"
---
# <a name="configuring-parameters"></a>配置参数

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

通过提供类型检查和验证，命令对象可使用参数来将值传递给 SQL 语句或存储过程。 与命令文本不同，参数输入被视为文本值，而不是可执行代码。 这样可帮助抵御“SQL 注入”攻击，这种攻击的攻击者会将命令插入 SQL 语句，从而危及服务器的安全。

参数化命令还可提高查询执行性能，因为它们可帮助数据库服务器将传入命令与适当的缓存查询计划进行准确匹配。 有关详细信息，请参阅[执行计划的缓存和重用](../../relational-databases/query-processing-architecture-guide.md#execution-plan-caching-and-reuse)和[重用参数和执行计划](../../relational-databases/query-processing-architecture-guide.md#PlanReuse)。 除具备安全和性能优势外，参数化命令还提供一种用于组织传递到数据源的值的便捷方法。

<xref:System.Data.Common.DbParameter> 对象可以通过使用其构造函数来创建，或者也可以通过调用 <xref:System.Data.Common.DbCommand.DbParameterCollection%2A> 集合的 `Add` 方法以将该对象添加到 <xref:System.Data.Common.DbParameterCollection> 来创建。 `Add` 方法将构造函数实参或现有形参对象用作输入，具体取决于数据提供程序。

## <a name="supply-the-parameterdirection-property"></a>提供 ParameterDirection 属性

在添加参数时，您必须为输入参数以外的参数提供一个 <xref:System.Data.ParameterDirection> 属性。 下表显示了可用于 `ParameterDirection` 枚举的 <xref:System.Data.ParameterDirection> 值。

|成员名称|描述|
|-----------------|-----------------|
|<xref:System.Data.ParameterDirection.Input>|该参数为输入参数。 这是默认值。|
|<xref:System.Data.ParameterDirection.InputOutput>|该参数可执行输入和输出。|
|<xref:System.Data.ParameterDirection.Output>|该参数为输出参数。|
|<xref:System.Data.ParameterDirection.ReturnValue>|该参数表示从某操作（如存储过程、内置函数或用户定义的函数）返回的值。|

## <a name="work-with-parameter-placeholders"></a>使用参数占位符

参数占位符的语法取决于数据源。 Microsoft SqlClient Data Provider for SQL Server 以不同方式处理命名和指定参数以及参数占位符。 SqlClient 数据提供程序以 `@`parametername 格式使用命名参数。

## <a name="specify-parameter-data-types"></a>指定参数数据类型

参数的数据类型特定于 Microsoft SqlClient Data Provider for SQL Server。 在将值传递给数据源之前，指定类型会将 `Parameter` 的值转换为 Microsoft SqlClient Data Provider for SQL Server。 也可以通过通用的方式指定 `Parameter` 的类型，方法是将 `DbType` 对象的 `Parameter` 属性设置为特定的 <xref:System.Data.DbType>。

`Parameter` 对象的 Microsoft SqlClient Data Provider for SQL Server 类型是根据 `Parameter` 对象 `Value` 的 .NET Framework 类型或 `Parameter` 对象的 `DbType` 推断出来的。 下表显示了根据作为 `Parameter` 值传递的对象或指定的 `Parameter` 推断出的 `DbType`类型。

|.NET 类型|DbType|SqlDbType|
|-------------------------|------------|---------------|
|<xref:System.Boolean>|布尔|bit|
|<xref:System.Byte>|Byte|TinyInt|
|byte[]|二进制|Varbinary。 如果字节数组大于 VarBinary 的最大大小（8000 字节），此隐式转换将失败。 对于大于 8000 字节的字节数组，请显式设置 <xref:System.Data.SqlDbType>。|
|<xref:System.Char>| |不支持从 char 推断 <xref:System.Data.SqlDbType> 。|
|<xref:System.DateTime>|DateTime|DateTime|
|<xref:System.DateTimeOffset>|DateTimeOffset|SQL Server 2008 中的 DateTimeOffset。 SQL Server 2008 以前的 SQL Server 版本不支持从 DateTimeOffset 推断 <xref:System.Data.SqlDbType> 。|
|<xref:System.Decimal>|小数|小数|
|<xref:System.Double>|Double|Float|
|<xref:System.Single>|Single|Real|
|<xref:System.Guid>|Guid|UniqueIdentifier|
|<xref:System.Int16>|Int16|SmallInt|
|<xref:System.Int32>|Int32|int|
|<xref:System.Int64>|Int64|BigInt|
|<xref:System.Object>|对象|变量|
|<xref:System.String>|String|NVarChar。 如果字符串大于 NVarChar 的最大大小（4000 个字符），此隐式转换将失败。 对于大于 4000 个字符的字符串，请显式设置 <xref:System.Data.SqlDbType>。|
|<xref:System.TimeSpan>|时间|SQL Server 2008 中的 Time。 SQL Server 2008 以前的 SQL Server 版本不支持从 TimeSpan 推断 <xref:System.Data.SqlDbType> 。|
|<xref:System.UInt16>|UInt16|不支持从 UInt16 推断 <xref:System.Data.SqlDbType> 。|
|<xref:System.UInt32>|UInt32|不支持从 UInt32 推断 <xref:System.Data.SqlDbType> 。|
|<xref:System.UInt64>|UInt64|不支持从 UInt64 推断 <xref:System.Data.SqlDbType> 。|
||AnsiString|VarChar|
||AnsiStringFixedLength|Char|
||货币|Money|
||Date|SQL Server 2008 中的 Date。 SQL Server 2008 以前的 SQL Server 版本不支持从 Date 推断 <xref:System.Data.SqlDbType> 。|
||SByte|不支持从 SByte 推断 <xref:System.Data.SqlDbType> 。|
||StringFixedLength|NChar|
||时间|SQL Server 2008 中的 Time。 SQL Server 2008 以前的 SQL Server 版本不支持从 Time 推断 <xref:System.Data.SqlDbType> 。|
||VarNumeric|不支持从 VarNumeric 推断 <xref:System.Data.SqlDbType> 。|
|用户定义类型（带有 <xref:Microsoft.SqlServer.Server.SqlUserDefinedAggregateAttribute>的对象）|SqlClient 始终返回 Object|如果 <xref:Microsoft.SqlServer.Server.SqlUserDefinedTypeAttribute> 存在，则为 `SqlDbType.Udt`；否则为 `Variant`|

> [!NOTE]
> 从小数转换到其他类型是缩窄转换，这种转换会将小数值舍入到最近的接近零的整数值。 如果无法以目标类型表示转换结果，则会引发 <xref:System.OverflowException> 。

> [!NOTE]
> 将空参数值发送到服务器时，必须指定 <xref:System.DBNull>，而不是 `null`（在 Visual Basic 中为 `Nothing`）。 系统中的 null 值是一个不具有任何值的空对象。 <xref:System.DBNull> 用于表示 null 值。

## <a name="derive-parameter-information"></a>派生参数信息

还可以使用 `DbCommandBuilder` 类从存储过程派生参数。 `SqlCommandBuilder` 类提供静态方法，`DeriveParameters`，它自动填充使用存储过程中的参数信息的命令对象的参数集合。 请注意， `DeriveParameters` 会覆盖此命令的任何现有参数信息。

> [!NOTE]
> 派生参数信息会影响性能，因为它需要对数据源进行额外的往返访问，以检索信息。 如果参数信息在设计时是已知的，则可以通过显式设置参数来提高应用程序的性能。

有关详细信息，请参阅[使用 CommandBuilders 生成命令](generate-commands-with-commandbuilders.md)。

## <a name="using-parameters-with-a-sqlcommand-and-a-stored-procedure"></a>对 SqlCommand 和存储过程使用参数

在数据驱动的应用程序中，存储过程具有许多优势。 通过利用存储过程，数据库操作可以包装在单个命令中，为获取最佳性能而进行优化并通过附加的安全性得到增强。 尽管可以通过在 SQL 语句中传递后接参数自变量的存储过程名称来调用相应的存储过程，但如果使用 ADO.NET <xref:System.Data.Common.DbCommand> 对象的 <xref:System.Data.Common.DbCommand.Parameters%2A> 集合，则可以让你更为明确地定义存储过程参数，并访问输出参数和返回值。

> [!NOTE]
> 参数化语句在服务器上通过使用 `sp_executesql,` 执行，sp_executesql 允许重复使用查询计划。 `sp_executesql` 批处理命令中的本地光标或变量对于调用 `sp_executesql`的批处理命令是不可见的。 数据库上下文中的更改只持续到 `sp_executesql` 语句的结尾。 有关详细信息，请参阅 [sp_executesql (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)。

对 <xref:Microsoft.Data.SqlClient.SqlCommand> 使用参数以执行 SQL Server 存储过程时，添加到 <xref:Microsoft.Data.SqlClient.SqlCommand.Parameters%2A> 集合中的参数的名称必须与存储过程中参数标记的名称相匹配。 Microsoft SqlClient Data Provider for SQL Server 不支持用于向 SQL 语句或存储过程传递参数的问号 (?) 占位符。 它将存储过程中的参数视为命名参数，并搜索匹配的参数标记。 例如，通过使用名为 `CustOrderHist` 的参数定义 `@CustomerID`存储过程。 您的代码在执行该存储过程时，它也必须使用名为 `@CustomerID`的参数。

```sql
CREATE PROCEDURE dbo.CustOrderHist @CustomerID varchar(5)
```

### <a name="example"></a>示例

此示例演示了如何调用 `Northwind` 示例数据库中的 SQL Server 存储过程。 存储过程的名称为 `dbo.SalesByCategory` ，它具有名为 `@CategoryName` 的输入参数，其数据类型为 `nvarchar(15)`。 该代码在 using 代码块内创建一个新 <xref:Microsoft.Data.SqlClient.SqlConnection> ，以便在过程结束时释放连接。 会创建 <xref:Microsoft.Data.SqlClient.SqlCommand> 和 <xref:Microsoft.Data.SqlClient.SqlParameter> 对象，并设置其属性。 <xref:Microsoft.Data.SqlClient.SqlDataReader> 会执行 `SqlCommand` 并从存储过程返回结果集，以在控制台窗口中显示相关输出。

> [!NOTE]
> 您可以选择使用任一重载构造函数在一个语句中设置多个属性，而不是创建 `SqlCommand` 和 `SqlParameter` 对象，然后在各个语句中设置属性。

[!code-csharp[DataWorks SqlClient.StoredProcedure#1](~/../sqlclient/doc/samples/SqlCommand_StoredProcedure.cs#1)]

## <a name="see-also"></a>请参阅

- [命令和参数](commands-parameters.md)
- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)