---
title: 检索数据库架构信息
description: 了解如何使用 Microsoft SqlClient Data Provider for SQL Server 检索数据库架构信息。
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 932f4ff2109e3754fb97c79274a5ffb93b9494d3
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051260"
---
# <a name="retrieving-database-schema-information"></a>检索数据库架构信息

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

从数据库获取架构信息通过架构发现过程来完成。 通过架构发现，应用程序可以请求托管提供程序查找并返回有关给定数据库的数据库架构（也称为元数据）名称的信息。 不同的数据库架构元素（例如表、列和存储过程）通过架构集合进行公开。 每个架构集合包含所使用的提供程序特定的各种架构信息。

Microsoft SqlClient Data Provider for SQL Server 实现 SqlConnection 类中的 GetSchema 方法，从 GetSchema 方法返回的架构信息采用 <xref:System.Data.DataTable> 的形式  。 GetSchema 方法属于重载方法，提供可选的参数来指定要返回的架构集合以及限制返回的信息量。 SqlClient 数据提供程序还提供了一种返回描述 SqlDataReader 的列元数据的 DataTable 的 GetSchemaTable 方法 。

## <a name="in-this-section"></a>在本节中

[GetSchema 和架构集合](getschema-and-schema-collections.md)  
描述 GetSchema 方法以及如何使用该方法从数据库检索和限制架构信息。

[架构限制](schema-restrictions.md)  
描述可用于 GetSchema 的架构限制。 

[常用架构集合](common-schema-collections.md)  
描述所有 .NET 托管提供程序支持的所有常用架构集合。  
  
[SQL Server 架构集合](sql-server-schema-collections.md)  
描述 Microsoft SqlClient Data Provider for SQL Server 支持的其他架构集合。 

## <a name="reference"></a>参考

<xref:System.Data.Common.DbConnection.GetSchema%2A>  
描述 <xref:System.Data.Common.DbConnection> 类的 GetSchema 方法。

<xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A>  
描述 <xref:Microsoft.Data.SqlClient.SqlConnection> 类的 GetSchema 方法。

<xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
描述 <xref:System.Data.Common.DbDataReader> 类的 GetSchemaTable 方法。 

<xref:Microsoft.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
描述 <xref:Microsoft.Data.SqlClient.SqlDataReader> 类的 GetSchemaTable 方法。

## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
