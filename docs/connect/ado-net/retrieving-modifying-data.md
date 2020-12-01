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
ms.openlocfilehash: cbe61aafd8dcd1681230c355187a65a231535e00
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126354"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>在 ADO.NET 中检索和修改数据

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

任何数据库应用程序的一项主要功能是连接数据源并检索数据源中包含的数据。 SqlClient 数据提供程序充当应用程序和数据源之间的桥梁，使你可以执行命令以及使用 DataReader 或 DataAdapter 检索数据。 任何数据库应用程序的一项关键功能是更新数据库中存储的数据的能力。 在 Microsoft SqlClient Data Provider for SQL Server 中，更新数据时会使用 DataAdapter、<xref:System.Data.DataSet> 和 Command 对象；此外，还可能会使用事务。

## <a name="in-this-section"></a>本节内容

[连接到数据源](connecting-to-data-source.md) 介绍如何建立与数据源的连接以及如何使用连接事件。

[连接字符串](connection-strings.md) 包含介绍使用连接字符串的各个方面的主题，包括连接字符串关键字、安全信息以及存储和检索连接字符串。

[连接池](connection-pooling.md) 介绍 Microsoft SqlClient Data Provider for SQL Server 的连接池。

## <a name="see-also"></a>另请参阅

- [ADO.NET 中的数据类型映射](data-type-mappings-ado-net.md)
- [SQL Server 和 ADO.NET](./sql/index.md)
