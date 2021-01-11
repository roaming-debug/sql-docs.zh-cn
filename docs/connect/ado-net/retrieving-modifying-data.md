---
title: 检索和修改数据
description: 在 NET 中，Microsoft SqlClient Data Provider for SQL Server 充当应用程序和数据源之间的桥梁，用于读取和更新数据。
ms.date: 11/30/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 4275b7de0f31d03aa36ef31d8801fcdc0e9ec853
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771531"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>在 ADO.NET 中检索和修改数据

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

任何数据库应用程序的一项主要功能是连接数据源并检索数据源中包含的数据。 SqlClient 数据提供程序充当应用程序和数据源之间的桥梁，使你可以执行命令以及使用 DataReader 或 DataAdapter 检索数据。 任何数据库应用程序的一项关键功能是更新数据库中存储的数据的能力。 在 Microsoft SqlClient Data Provider for SQL Server 中，更新数据时会使用 DataAdapter、<xref:System.Data.DataSet> 和 Command 对象；此外，还可能会使用事务。

## <a name="in-this-section"></a>在本节中

[连接到数据源](connecting-to-data-source.md)  
说明如何建立到数据源的连接及如何使用连接事件。

[连接字符串](connection-strings.md)  
包含说明使用连接字符串（包括连接字符串关键字、安全信息以及存储和检索连接字符串）的各个方面的主题。

[连接池](connection-pooling.md)  
介绍 Microsoft SqlClient Data Provider for SQL Server 的连接池。

[命令和参数](commands-parameters.md)  
包含说明如何创建命令和命令生成器、配置参数以及如何执行命令来检索和修改数据的主题。

[DataAdapter 和 DataReader](dataadapters-datareaders.md)  
包含说明 DataReader、DataAdapter、参数、处理 DataAdapter 事件和执行批操作的主题。

[事务和并发性](transactions-and-concurrency.md)  
包含说明如何执行本地事务、分布式事务及使用开放式并发的主题。

[检索数据库架构信息](retrieving-database-schema-information.md)  
说明如何获取可用数据库或编录、数据库中的表和视图、表存在的约束以及数据源中的其他架构信息。

[DbProviderFactories](dbproviderfactories.md)  
描述提供程序工厂模型及说明如何在 `System.Data.Common` 命名空间中使用基类。  

[检索标识或自动编号值](retrieve-identity-or-autonumber-values.md)  
提供一个示例，该示例将为 SQL Server 表中的“标识”列生成的值映射到表中插入行的某一列。 讨论在 `DataTable` 中合并标识值。  
  
[检索二进制数据](retrieve-binary-data.md)  
说明如何使用 `CommandBehavior`.`SequentialAccess` 检索二进制数据或大数据结构 以修改 `DataReader` 的默认行为。  
  
[使用存储过程修改数据](modify-data-with-stored-procedures.md)  
说明如何使用存储过程的输入参数和输出参数在数据库中插入行，同时返回新标识值。  

[SqlClient 中的数据跟踪](data-tracing.md)  
介绍用于 SQL Server 的 Microsoft SqlClient 数据提供程序如何提供内置数据跟踪功能。  
  
[SqlClient 中的性能计数器](performance-counters.md)  
介绍适用于 SQL Server 的 Microsoft SqlClient 数据提供程序的性能计数器。  
  
[异步编程](asynchronous-programming.md)  
介绍用于 SQL Server 的 Microsoft SqlClient 数据提供程序对异步编程的支持。  
  
[SqlClient 流式处理支持](sqlclient-streaming-support.md)  
讨论如何编写在不将其完全加载到内存的情况下从 SQL Server 流式处理数据的应用程序。  

## <a name="see-also"></a>请参阅

- [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)
- [SQL Server 和 ADO.NET](./sql/index.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
