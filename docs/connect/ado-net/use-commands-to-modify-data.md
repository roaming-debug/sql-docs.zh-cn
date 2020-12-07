---
title: 使用命令修改数据
description: 说明如何使用数据提供程序来执行存储过程或数据定义语言 (DDL) 语句。
ms.date: 11/25/2020
ms.assetid: f4160389-b9ff-4b74-b655-437c76dcd586
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 03ebdbdd15adfae8e765964e8338f043999319f3
ms.sourcegitcommit: debaff72dbfae91b303f0acd42dd6d99e03135a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96428182"
---
# <a name="using-commands-to-modify-data"></a>使用命令修改数据

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

通过使用 Microsoft SqlClient Data Provider for SQL Server，你可以执行存储过程或数据定义语言语句（例如，CREATE TABLE 和 ALTER COLUMN），以对数据库或目录执行架构操作。 这些命令不会像查询一样返回行，因此 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象提供了 <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> 来处理这些命令。

除了使用 ExecuteNonQuery 来修改架构之外，还可以使用此方法处理那些修改数据但不返回行的 SQL 语句，如 INSERT、UPDATE 和 DELETE。

虽然行不是由 ExecuteNonQuery 方法返回的，但可以通过 Command 对象的 Parameters 集合来传递和返回输入和输出参数以及返回值。

## <a name="in-this-section"></a>在本节中

[更新数据源中的数据](update-data-inside-data-source.md)介绍如何执行修改数据库中的数据的命令或存储过程。

[执行目录操作](perform-catalog-operations.md)介绍如何执行修改数据库架构的命令。

## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-modifying-data.md)
- [命令和参数](commands-parameters.md)
