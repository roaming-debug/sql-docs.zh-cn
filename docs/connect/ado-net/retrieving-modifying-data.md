---
title: 检索和修改数据
description: 在 .NET Framework 中，Microsoft SqlClient Data Provider for SQL Server 充当应用程序和数据源之间的桥梁，用于读取和更新数据。
ms.date: 11/13/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: d6e4d6c298c632c446e1671b5d9adabaa19e0776
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2020
ms.locfileid: "96761485"
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

## <a name="see-also"></a>请参阅

- [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)
- [SQL Server 和 ADO.NET](./sql/index.md)
- [用于 SQL Server 的 Microsoft ADO.NET](microsoft-ado-net-sql-server.md)
