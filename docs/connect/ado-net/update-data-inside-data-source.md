---
title: 更新数据源中的数据
description: 描述如何执行对数据库中的数据进行修改的命令或存储过程。
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 55c545e5-dcd5-4323-a5b9-3825c2157462
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: aca7b1adb8ce91a12832bccfd6cbd27b07229d22
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771477"
---
# <a name="updating-data-in-a-data-source"></a>更新数据源中的数据

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

修改数据的 SQL 语句（如 INSERT、UPDATE 或 DELETE）不返回行。 同样，许多存储过程执行操作但不返回行。 要执行不返回行的命令，使用相应 SQL 命令创建一个 Command 对象，并创建一个 Connection，包括所有必需的 Parameters。 使用 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象的 <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> 方法执行命令。

> [!NOTE]
> ExecuteNonQuery 方法返回一个整数，表示受已执行的语句或存储过程影响的行数。 如果执行了多个语句，则返回的值为受所有已执行语句影响的记录的总数。

## <a name="example"></a>示例

以下代码示例执行一个 INSERT 语句，以使用 ExecuteNonQuery 将一个记录插入数据库中。
  
[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#1)]

以下代码示例执行由[执行编录操作](perform-catalog-operations.md)中的示例代码创建的存储过程。 该存储过程没有返回任何行，因此将使用 ExecuteNonQuery 方法，但该存储过程会接收输入参数并返回输出参数和返回值。

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#2](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#2)]

## <a name="see-also"></a>请参阅

- [使用命令修改数据](use-commands-to-modify-data.md)
- [通过 DataAdapter 更新数据源](update-data-sources-with-dataadapters.md)
- [命令和参数](commands-parameters.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
