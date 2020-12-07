---
title: 执行目录操作
description: 描述如何执行对数据库架构进行修改的命令。
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: e60f542f-6271-495b-a9e4-48553481c2a3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: 85e68bd77b11fd70e4071d7ae67ebf26c643b540
ms.sourcegitcommit: debaff72dbfae91b303f0acd42dd6d99e03135a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96428184"
---
# <a name="performing-catalog-operations"></a>执行目录操作

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

若要执行对数据库或编录进行修改的命令（如 CREATE TABLE 或 CREATE PROCEDURE 语句），请使用相应的 SQL 语句和 Connection 对象创建一个 Command 对象。 使用 <xref:Microsoft.Data.SqlClient.SqlCommand> 对象的 <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> 方法执行命令。

## <a name="example"></a>示例

以下代码示例在 Microsoft SQL Server 数据库中创建一个存储过程。

[!code-csharp[DataWorks SqlCommand.ExecuteNonQuery#3](~/../sqlclient/doc/samples/SqlCommand_ExecuteNonQuery_SP_DML.cs#3)]

## <a name="see-also"></a>请参阅

- [使用命令修改数据](use-commands-to-modify-data.md)
- [命令和参数](commands-parameters.md)
