---
title: 按 DataReader 检索数据
description: 了解如何通过此示例代码在 ADO.NET 中使用 DataReader 检索数据。 DataReader 提供未缓冲的数据流。
ms.date: 11/30/2020
dev_langs:
- csharp
ms.assetid: 97afc121-fb8b-465b-bab3-6d844420badb
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: e7a618ef92a9f4a4cc969112886a4246ad25adc6
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97559199"
---
# <a name="retrieve-data-by-a-datareader"></a>按 DataReader 检索数据

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

若要使用 DataReader 检索数据，请创建 Command 对象的实例，然后通过调用 Command.ExecuteReader 创建一个 DataReader，以便从数据源检索行   。 DataReader 提供未缓冲的数据流，该数据流使过程逻辑可以有效地按顺序处理从数据源中返回的结果。

> [!NOTE]
> 由于数据不在内存中缓存，所以在检索大量数据时，DataReader 是一种适合的选择。

下面的示例演示如何使用 DataReader，其中 `reader` 表示有效的 DataReader，而 `command` 表示有效的 Command 对象。  

```csharp
reader = command.ExecuteReader();  
```

使用 DataReader.Read 方法从查询结果中获取行。 通过向 DataReader 传递列的名称或序号，可以访问返回行的每一列。 不过，为了实现最佳性能，DataReader 提供了一系列方法，将使你能够访问其本机数据类型（GetDateTime、GetDouble、GetGuid、GetInt32 等）的列值    。 有关数据提供程序特定的 DataReaders 的类型化访问器方法列表，请参阅 <xref:Microsoft.Data.SqlClient.SqlDataReader>。 已知基础数据类型时，如果使用类型化访问器方法，将减少在检索列值时所需的类型转换量。  

以下示例循环访问一个 DataReader 对象，并从每个行中返回两个列。  

[!code-csharp[DataWorks SqlClient.HasRows#1](~/../sqlclient/doc/samples/SqlDataReader_HasRows.cs#1)]

## <a name="closing-the-datareader"></a>关闭 DataReader  

每次使用完 DataReader 对象后都应调用 Close 方法 。

> [!NOTE]
> 如果 Command 包含输出参数或返回值，那么在 DataReader 关闭之前，将无法访问这些值 。  

> [!IMPORTANT]
> 当 DataReader 打开时，该 DataReader 将以独占方式使用 Connection  。 在原始 DataReader 关闭之前，将无法对 Connection 执行任何命令（包括创建另一个 DataReader）  。  

> [!NOTE]
> 不要在类的 Finalize 方法中对 Connection、DataReader 或任何其他托管对象调用 Close 或 Dispose    。 在终结器中，仅释放类直接拥有的非托管资源。 如果类不拥有任何非托管资源，则不要在类定义中包含 Finalize 方法。 有关详细信息，请参阅[垃圾回收](/dotnet/standard/garbage-collection/index)。
 
## <a name="retrieve-multiple-result-sets-using-nextresult"></a>使用 NextResult 检索多个结果集

如果 DataReader 返回多个结果集，请调用 NextResult 方法来按顺序循环访问这些结果集 。 以下示例显示 <xref:Microsoft.Data.SqlClient.SqlDataReader> 如何使用 <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteReader%2A> 方法处理两个 SELECT 语句的结果。  

[!code-csharp[DataWorks SqlClient.NextResult#1](~/../sqlclient/doc/samples/SqlDataReader_NextResult.cs#1)]

## <a name="get-schema-information-from-the-datareader"></a>从 DataReader 中获取架构信息  

当 DataReader 打开时，可以使用 GetSchemaTable 方法检索有关当前结果集的架构信息 。 GetSchemaTable 将返回一个填充了行和列的 <xref:System.Data.DataTable> 对象，这些行和列包含当前结果集的架构信息。 对于结果集的每一列，DataTable 都包含一行。 架构表的每一列都映射到在结果集的行中返回的列的属性，其中 ColumnName 是属性的名称，而列的值为属性的值。 以下示例为 DataReader 编写架构信息。  

[!code-csharp[DataWorks SqlClient.GetSchemaTable#1](~/../sqlclient/doc/samples/SqlDataReader_GetSchemaTable.cs#1)]

## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [命令和参数](commands-parameters.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
