---
title: 命令和参数
description: 了解如何使用用于 Microsoft SqlClient Data Provider for SQL Server 的 Command 对象来运行命令，并从数据源返回结果。
ms.date: 11/25/2020
ms.assetid: b623f810-d871-49a5-b0f5-078cc3c34db6
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 931c36619f5eaed0159ee04db3a08eb745634698
ms.sourcegitcommit: debaff72dbfae91b303f0acd42dd6d99e03135a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96428193"
---
# <a name="commands-and-parameters"></a>命令和参数

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

建立与数据源的连接后，可以使用 <xref:System.Data.Common.DbCommand> 对象来执行命令并从数据源中返回结果。 可以使用用于 Microsoft SqlClient Data Provider for SQL Server 的命令构造函数之一创建命令。 构造函数可以采用可选自变量，如要在数据源中执行的 SQL 语句、<xref:System.Data.Common.DbConnection> 对象或 <xref:System.Data.Common.DbTransaction> 对象。

您也可以将这些对象配置为命令的属性。 也可以使用 <xref:System.Data.Common.DbConnection.CreateCommand%2A> 对象的 `DbConnection` 方法创建用于特定连接的命令。 由命令执行的 SQL 语句可以使用 <xref:System.Data.Common.DbCommand.CommandText%2A> 属性进行配置。 Microsoft SqlClient Data Provider for SQL Server 具有对象 <xref:Microsoft.Data.SqlClient.SqlCommand>。

## <a name="in-this-section"></a>在本节中

[执行命令](execute-command.md)介绍 ADO.NET `Command` 对象，以及如何使用它对数据源执行查询和命令。

[配置参数](configure-parameters.md)介绍 `Command` 参数的用法，包括方向、数据类型和参数语法。

[使用 CommandBuilder 生成命令](generate-commands-with-commandbuilders.md)  
说明如何使用命令生成器为具有单表 SELECT 命令的 `DataAdapter` 自动生成 INSERT、UPDATE 和 DELETE 命令。

[从数据库获取单个值](obtain-single-value-from-database.md)介绍如何使用 `Command` 对象的 `ExecuteScalar` 方法从数据库查询中返回单个值。

[使用命令修改数据](use-commands-to-modify-data.md)说明如何使用 Microsoft SqlClient data provider for SQL Server 执行存储过程或数据定义语言 (DDL) 语句。

## <a name="see-also"></a>请参阅

- [连接到数据源](connecting-to-data-source.md)
