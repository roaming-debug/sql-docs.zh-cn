---
title: 数据类型映射
description: 介绍 Microsoft SqlClient Data Provider for SQL Server 使用的数据类型。
ms.date: 11/13/2020
ms.assetid: d4afab94-ada6-4c77-a73c-41f17bae6b5a
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 808acdf89350f539f03f6fdc75f2f7a5ed5b7707
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771609"
---
# <a name="data-type-mappings-in-adonet"></a>ADO.NET 中的数据类型映射

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

ADO.NET 基于通用类型系统，该系统定义在运行时中声明、使用和管理类型的方式。 它由值类型和引用类型组成，这两种类型均派生自 <xref:System.Object> 基类型。 使用数据源时，如果未显式指定数据类型，则从数据提供程序推断出它。 例如，<xref:System.Data.DataSet> 对象独立于任何特定的数据源。 `DataSet` 中的数据从数据源中进行检索，而更改则会使用 `DataAdapter` 持久保存回数据源。 此程序流意味着当 `DataAdapter` 使用数据源中的值填充 `DataSet` 中的 <xref:System.Data.DataTable> 时，`DataTable` 中列的结果数据类型是 .NET Framework 类型，而不是特定于用于连接到数据源的 Microsoft SqlClient Data Provider for SQL Server 的类型。

同样，当 `DataReader` 从数据源返回值时，生成的值将存储在具有 .NET Framework 类型的局部变量中。 对于 `DataAdapter` 的 `Fill` 操作和 `DataReader` 的 `Get` 方法，将从 Microsoft SqlClient Data Provider for SQL Server 中返回的值来推断 .NET Framework 类型。

当所返回值的特定类型已知时，可以使用 `DataReader` 的类型化访问器方法，而不是依靠推断出的数据类型。 类型化访问器方法无需进行额外的类型转换，即可将值作为特定的 .NET Framework 类型返回，从而提供更好的性能。

> [!NOTE]
> Microsoft SqlClient Data Provider for SQL Server 数据类型的 Null 值由 `DBNull.Value` 表示。

## <a name="in-this-section"></a>本节内容

[SQL Server 数据类型映射](sql-server-data-type-mappings.md)列出 <xref:Microsoft.Data.SqlClient> 推断的数据类型映射和数据访问器方法。

[浮点数](floating-point-numbers.md) 介绍开发人员在使用浮点数时经常遇到的问题。

## <a name="see-also"></a>另请参阅

- [SQL Server 数据类型和 ADO.NET](./sql/sql-server-data-types.md)
- [配置参数](configure-parameters.md)
- [检索数据库架构信息](retrieving-database-schema-information.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
