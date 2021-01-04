---
title: 获取架构和架构集合
description: 描述如何使用 GetSchema 方法从数据库检索和限制架构信息。
ms.date: 11/26/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 113ff460017cb5fabddd4763b28294ef6f19954c
ms.sourcegitcommit: 2add15a99df7b85d271adb261523689984dfd134
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97051281"
---
# <a name="get-schema-and-schema-collections"></a>获取架构和架构集合

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Microsoft SqlClient Data Provider for SQL Server 中的 SqlConnection 类实现 GetSchema 方法，该方法用于检索与当前连接的数据库有关的架构信息，从 GetSchema 方法返回的架构信息以 <xref:System.Data.DataTable> 的形式传入  。 GetSchema 方法属于重载方法，提供可选的参数来指定要返回的架构集合以及限制返回的信息量。

## <a name="specifying-the-schema-collections"></a>指定架构集合

GetSchema 方法的第一个可选参数是以字符串形式指定的集合名称。 有两种类型的架构集合：所有提供程序通用的通用架构集合以及每个提供程序特定的特定架构集合。  

可以通过调用没有参数或包含架构集合名称“MetaDataCollections”的 GetSchema 方法来查询 Microsoft SqlClient Data Provider for SQL Server，以确定支持的架构集合列表。 此时将返回 <xref:System.Data.DataTable>，包含支持的架构集合列表、每个架构集合支持的限制数以及所使用的标识符部分数。  

### <a name="retrieving-schema-collections-example"></a>限制架构集合示例

以下示例演示如何使用 Microsoft SqlClient Data Provider for SQL Server 的 <xref:Microsoft.Data.SqlClient.SqlConnection> 类的 <xref:Microsoft.Data.SqlClient.SqlConnection.GetSchema%2A> 方法来检索与 AdventureWorks 示例数据库中包含的所有表有关的架构信息：  

[!code-csharp[SqlClient GetSchema#1](~/../sqlclient/doc/samples/SqlConnection_GetSchema_Tables.cs#1)]  

## <a name="see-also"></a>另请参阅

- [检索数据库架构信息](retrieving-database-schema-information.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
