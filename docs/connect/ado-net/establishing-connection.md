---
title: 建立连接
description: 通过 SqlClient 提供程序连接到 SQL Server 的指南。
ms.date: 11/13/2020
dev_langs:
- csharp
ms.assetid: 3af512f3-87d9-4005-9e2f-abb1060ff43f
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4b29191e5f7e42b5057d4258145f7b56001285b5
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126335"
---
# <a name="establishing-connection"></a>建立连接

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

要连接到 Microsoft SQL Server，请使用 Microsoft SqlClient Data Provider for SQL Server 的 <xref:Microsoft.Data.SqlClient.SqlConnection> 对象。 要安全地存储和检索连接字符串，请参阅[保护连接信息](protecting-connection-information.md)。

## <a name="closing-connections"></a>关闭连接

我们建议您在使用完连接时一定要关闭连接，以便连接可以返回池。 如果 Visual Basic 或 C# 的代码中存在 `Using` 块，将自动断开连接，即使发生无法处理的异常。 有关详细信息，请参阅 [using 语句](/dotnet/docs/csharp/language-reference/keywords/using-statement.md)和 [ 语句](/dotnet/docs/visual-basic/language-reference/statements/using-statement.md)。

还可以使用连接对象的 `Close` 或 `Dispose` 方法。 不是显式关闭的连接可能不会添加或返回到池中。 例如，如果连接已超出范围但没有显式关闭，则仅当达到最大池大小而该连接仍然有效时，该连接才会返回到连接池中。

> [!NOTE]
> 不要在类的 `Finalize` 方法中对 Connection、DataReader 或任何其他托管对象调用 `Close` 或 `Dispose`。 在终结器中，仅释放类直接拥有的非托管资源。 如果类不拥有任何非托管资源，则不要在类定义中包含 `Finalize` 方法。 有关详细信息，请参阅[垃圾回收](/dotnet/docs/standard/garbage-collection/index.md)。

> [!NOTE]
> 从连接池中提取连接或将连接返回到连接池时，服务器上不会引发登录和注销事件，这是因为在将连接返回到连接池时实际上并没有将其关闭。 有关详细信息，请参阅 [SQL Server 连接池 (ADO.NET)](sql-server-connection-pooling.md)。

## <a name="connecting-to-sql-server"></a>连接到 SQL Server

有关有效的字符串格式名称和值，请参见 <xref:Microsoft.Data.SqlClient.SqlConnection.ConnectionString%2A> 对象的 <xref:Microsoft.Data.SqlClient.SqlConnection> 属性。 您也可以使用 <xref:Microsoft.Data.SqlClient.SqlConnectionStringBuilder> 类在运行时创建具有有效语法的连接字符串。 有关详细信息，请参阅[连接字符串生成器](connection-string-builders.md)。

以下代码示例演示如何创建并打开与 SQL Server 数据库的连接。

[!code-csharp[SqlConnection.Open#1](~/../sqlclient/doc/samples/SqlConnection_Open.cs#1)]

### <a name="integrated-security-and-aspnet"></a>集成安全性和 ASP.NET

SQL Server 集成安全性（也称为信任连接）在连接到 SQL Server 时有助于提供保护，因为它不会在连接字符串中公开用户 ID 和密码，并且是对连接进行身份验证的推荐方法。 集成安全性使用正在执行的进程的当前安全标识或标记。 对于桌面应用程序，此标识通常是当前登录用户的标识。

ASP.NET 应用程序的安全标识可设置为几个不同的选项之一。 若要更好地了解 ASP.NET 应用程序在连接到 SQL Server 时使用的安全标识，请参阅 [ASP.NET 模拟](/previous-versions/aspnet/xh507fc5(v=vs.100))、[ASP.NET 身份验证](/previous-versions/aspnet/eeyk640h(v=vs.100))和[如何：使用 Windows 集成安全性访问 SQL Server](/previous-versions/aspnet/bsz5788z(v=vs.100))。

## <a name="see-also"></a>另请参阅

- [连接到数据源](connecting-to-data-source.md)
- [连接字符串](connection-strings.md)
