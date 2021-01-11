---
title: SqlClient 流式处理支持
description: 讨论如何编写在不将其完全加载到内存的情况下从 SQL Server 流式处理数据的应用程序。
ms.date: 12/04/2020
ms.assetid: c449365b-470b-4edb-9d61-8353149f5531
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c5ae05856f3f1e01d5831a6e80338f19e3994966
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744346"
---
# <a name="sqlclient-streaming-support"></a>SqlClient 流式处理支持

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

SQL Server 和应用程序之间的流式处理支持支持服务器上的非结构化数据（文档、图像和媒体文件）。 SQL Server 数据库可以存储二进制大型对象 (BLOB)，但检索 BLOB 会使用大量内存。

针对 SQL Server 的流式处理支持简化了对数据进行流式处理的应用程序的编写，无需完全将数据加载到内存中，从而减少了内存溢出异常。

流式处理支持还会使中间层应用程序能够更好地进行缩放，尤其是在业务对象连接到 Azure SQL 以发送、检索和操作大型 BLOB 的情况下。

> [!WARNING]
> 支持流式处理的成员用于从查询中检索数据，并将参数传递给查询和存储过程。 流式处理功能可解决基本的 OLTP 和数据迁移方案，适用于本地和非本地数据迁移环境。

## <a name="streaming-support-from-sql-server"></a>SQL Server 中的流式处理支持

SQL Server 的流式处理支持在 <xref:System.Data.Common.DbDataReader> 和 <xref:Microsoft.Data.SqlClient.SqlDataReader> 类中引入新功能，以便获取 <xref:System.IO.Stream>、<xref:System.Xml.XmlReader> 和 <xref:System.IO.TextReader>对象并对其做出反应。 这些类用于检索查询中的数据。 因此，SQL Server 中的流式处理支持可解决 OLTP 方案，且适用于本地和非本地环境。

为了启用 SQL Server 中的流式处理支持，将以下成员添加到了 <xref:Microsoft.Data.SqlClient.SqlDataReader>：

- <xref:Microsoft.Data.SqlClient.SqlDataReader.IsDBNullAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValue%2A?displayProperty=nameWithType>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetFieldValueAsync%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetStream%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetTextReader%2A>

- <xref:Microsoft.Data.SqlClient.SqlDataReader.GetXmlReader%2A>

为了启用 SQL Server 中的流式处理支持，将以下成员添加到了 <xref:System.Data.Common.DbDataReader>：

- <xref:System.Data.Common.DbDataReader.GetFieldValue%2A>

- <xref:System.Data.Common.DbDataReader.GetStream%2A>

- <xref:System.Data.Common.DbDataReader.GetTextReader%2A>

## <a name="streaming-support-to-sql-server"></a>SQL Server 的流式处理支持

对 SQL Server 的流式处理支持位于 <xref:Microsoft.Data.SqlClient.SqlParameter> 类，因此它可以接受 <xref:System.Xml.XmlReader>、<xref:System.IO.Stream> 和 <xref:System.IO.TextReader> 对象并做出反应。 <xref:Microsoft.Data.SqlClient.SqlParameter> 用于将参数传递给查询和存储过程。

> [!NOTE]
> 释放 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象或调用 <xref:Microsoft.Data.SqlClient.SqlCommand.Cancel%2A> 必须取消任何流操作。 如果应用程序发送 <xref:System.Threading.CancellationToken>，则不保证取消。

以下 <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> 类型将接受 <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> 的 <xref:System.IO.Stream>：

- **二进制**

- VarBinary

以下 <xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> 类型将接受 <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A> 的 <xref:System.IO.TextReader>：

- **Char**

- NChar

- NVarChar

- **Xml**

Xml<xref:Microsoft.Data.SqlClient.SqlParameter.SqlDbType%2A> 类型将接受 <xref:System.Xml.XmlReader> 的 <xref:Microsoft.Data.SqlClient.SqlParameter.Value%2A>。

<xref:Microsoft.Data.SqlClient.SqlParameter.SqlValue%2A> 可接受类型 <xref:System.Xml.XmlReader>、<xref:System.IO.TextReader> 和 <xref:System.IO.Stream> 的值。

<xref:System.Xml.XmlReader>、<xref:System.IO.TextReader> 和 <xref:System.IO.Stream> 对象将转移到由 <xref:Microsoft.Data.SqlClient.SqlParameter.Size%2A> 定义的值。

## <a name="sample----streaming-from-sql-server"></a>示例 -- 来自 SQL Server 的流式处理

使用以下 Transact-SQL 创建示例数据库：

```sql
CREATE DATABASE [Demo]
GO
USE [Demo]
GO
CREATE TABLE [Streams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX),
[bindata] VARBINARY(MAX),
[xmldata] XML)
GO
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'This is a test', 0x48656C6C6F, N'<test>value</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Hello, World!', 0x54657374696E67, N'<test>value2</test>')
INSERT INTO [Streams] (textdata, bindata, xmldata) VALUES (N'Another row', 0x666F6F626172, N'<fff>bbb</fff><fff>bbc</fff>')
GO
```

该示例说明如何执行以下功能：

- 通过提供用于检索大型文件的异步方法来避免阻止用户接口线程。

- 在 .NET 中传输 SQL Server 的大型文本文件。

- 在 .NET 中传输 SQL Server 的大型 XML 文件。

- 检索 SQL Server 中的数据。

- 将一个 SQL Server 数据库中的大型文件 (BLOB) 传输到另一个数据库而不会用尽内存。

[!code-csharp[SqlClient_Streaming_FromServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_FromServer.cs#1)]

## <a name="sample----streaming-to-sql-server"></a>示例 -- 流式传输到 SQL Server

使用以下 Transact-SQL 创建示例数据库：

```sql
CREATE DATABASE [Demo2]
GO
USE [Demo2]
GO
CREATE TABLE [BinaryStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
CREATE TABLE [TextStreams] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[textdata] NVARCHAR(MAX))
GO
CREATE TABLE [BinaryStreamsCopy] (
[id] INT PRIMARY KEY IDENTITY(1, 1),
[bindata] VARBINARY(MAX))
GO
```

该示例说明如何执行以下功能：

- 在 .NET 中将大型 BLOB 传输到 SQL Server。

- 在 .NET 中将大型文本文件传输到 SQL Server。

- 使用新异步功能来传输大型 BLOB。

- 使用新异步功能和 await 关键字来传输大型 BLOB。

- 取消大型 BLOB 传输。

- 使用新的异步功能从一个 SQL Server 流式传输到另一个 SQL Server。

[!code-csharp[SqlClient_Streaming_ToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ToServer.cs#1)]

## <a name="sample----streaming-from-one-sql-server-to-another-sql-server"></a>示例 -- 从一个 SQL Server 流式处理到另一个 SQL Server

此示例演示如何以异步方式将大型 BLOB 从一个 SQL Server 流式传输到另一个 SQL Server，支持取消。

> [!NOTE]
> 在运行以下示例之前，请确保已创建 Demo 和 Demo2 数据库。

[!code-csharp[SqlClient_Streaming_ServerToServer#1](~/../sqlclient/doc/samples/SqlClient_Streaming_ServerToServer.cs#1)]

## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
