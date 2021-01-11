---
title: 使用存储过程修改数据
description: 说明如何使用存储过程的输入参数和输出参数在数据库中插入行，同时返回新标识值。
ms.date: 12/04/2020
dev_langs:
- csharp
ms.assetid: 7d8e9a46-1af6-4a02-bf61-969d77ae07e0
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: f04884302bb1f13852097182d6ebc8e06570c66b
ms.sourcegitcommit: 4419e99d77ee2c73f9da1559c7944f7702f2de30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/23/2020
ms.locfileid: "97744349"
---
# <a name="modify-data-with-stored-procedures"></a>使用存储过程修改数据

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

存储过程可以接受数据作为输入参数并可以返回数据作为输出参数、结果集或返回值。 下面的示例演示用于 SQL Server 的 Microsoft SqlClient 数据提供程序如何发送和接收输入参数、输出参数和返回值。 该示例将一个新记录插入到表中，其中主键列为标识列。

> [!NOTE]
> 如果使用存储过程来编辑或删除使用 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 的数据，请确保在存储过程定义中不使用 SET NOCOUNT ON。 这将使返回的受影响的行数为零，`DataAdapter` 会将其解释为并发冲突。 在这种情况下，将引发 <xref:System.Data.DBConcurrencyException>。

## <a name="example"></a>示例

此示例使用以下存储过程将一个新类别插入到 Northwind“类别”表。 存储过程获取“CategoryName”列中的值作为输入参数，并使用 SCOPE_IDENTITY() 函数来检索标识字段“CategoryID”的新值，然后在输出参数中返回该新值。 RETURN 语句使用 \@\@ROWCOUNT 函数返回所插入的行数。

```sql
CREATE PROCEDURE dbo.InsertCategory  
  @CategoryName nvarchar(15),  
  @Identity int OUT  
AS  
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)  
SET @Identity = SCOPE_IDENTITY()  
RETURN @@ROWCOUNT  
```  

下面的代码示例使用上面显示的 `InsertCategory` 存储过程作为 <xref:Microsoft.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 的 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 的来源。 如果在将记录插入到数据库后调用 `@Identity` 的 <xref:System.Data.DataSet> 方法，`Update` 中将会反映出 <xref:Microsoft.Data.SqlClient.SqlDataAdapter> 输出参数。 此代码还会检索返回值。

[!code-csharp[DataWorks SqlClient.SprocIdentityReturn#1](~/../sqlclient/doc/samples/SqlDataAdapter_SPIdentityReturn.cs#1)]

## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [DataAdapter 和 DataReader](dataadapters-datareaders.md)
- [执行命令](execute-command.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
