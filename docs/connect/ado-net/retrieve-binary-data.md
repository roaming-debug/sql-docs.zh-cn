---
title: 检索二进制数据
description: 说明如何使用 `CommandBehavior`.`SequentialAccess` 检索二进制数据或大数据结构 以修改 `DataReader` 的默认行为。
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 56c5a9e3-31f1-482f-bce0-ff1c41a658d0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 1033a6b5394d92a45cd19d70f4dd6c250ec37b62
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744354"
---
# <a name="retrieve-binary-data"></a>检索二进制数据

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

默认情况下，DataReader 在整个数据行可用时立即以行的形式加载传入数据。 但是，对于二进制大对象 (BLOB) 则需要进行不同的处理，因为它们可能包含数十亿字节的数据，而单个行中无法包含如此多的数据。 Command.ExecuteReader 方法具有一个重载，它将采用 <xref:System.Data.CommandBehavior> 参数来修改 DataReader 的默认行为。 可以将 <xref:System.Data.CommandBehavior.SequentialAccess> 传递到 ExecuteReader 方法来修改 DataReader 的默认行为，使其按照顺序在接收到数据时立即将其加载，而不是加载数据行。 这是加载 BLOB 或其他大数据结构的理想方案。

> [!NOTE]
> 在将 DataReader 设置为使用 SequentialAccess 时，务必要注意访问所返回字段的顺序。 DataReader 的默认行为是在整个行可用时立即加载该行，这样可以在读取下一行之前按任何顺序访问所返回的字段。 但是，当使用 SequentialAccess 时，必须按顺序访问由 DataReader 返回的字段。 例如，如果查询返回三个列，其中第三列是 BLOB，则必须在访问第三个字段中的 BLOB 数据之前返回第一个和第二个字段的值。 如果在访问第一个或第二个字段之前访问第三个字段，则第一个和第二个字段值将不再可用。 这是因为 SequentialAccess 已修改 DataReader，使其按顺序返回数据，当 DataReader 已经读取超过特定数据时，该数据将不可用。

在访问 BLOB 字段中的数据时，请使用 DataReader 的 GetBytes 或 GetChars 类型化访问器，它们将用数据来填充数组。 还可以对字符数据使用 GetString；但是为了节省系统资源，你可能不希望将整个 BLOB 值加载到单个字符串变量中。 您可以指定要返回的特定数据缓冲区大小，以及从返回的数据中读取的第一个字节或字符的起始位置。 GetBytes 和 GetChars 将返回一个 `long` 值，它表示返回的字节或字符数。 如果将一个空数组传递给 GetBytes 或 GetChars，则返回的长值将是 BLOB 中字节或字符的总数。 您可以选择将数组中的某个索引指定为所读取数据的起始位置。

## <a name="example"></a>示例

下面的示例从 [pubs 示例数据库](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs)返回发布者 ID 和徽标。 发行者 ID (`pub_id`) 是字符字段，而徽标则是图像，属于 BLOB。 由于 logo 字段是位图，因此该示例使用 GetBytes 返回二进制数据。 请注意，由于必须按顺序访问字段，所以将在访问徽标之前访问当前数据行的发行者 ID。

[!code-csharp[SqlCommand_ExecuteReader_SequentialAccess#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteReader_SequentialAccess.cs#1)]

## <a name="see-also"></a>另请参阅

- [SQL Server 二进制和大值数据](./sql/sql-server-binary-large-value-data.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
