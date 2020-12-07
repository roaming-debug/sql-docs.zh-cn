---
title: 从数据库获取单个值
description: 了解如何在 ADO.NET 中返回单个值。 此示例代码返回已插入记录的标识列值。
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: b38526cd-a62a-48cb-822a-e91dfa68e02d
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: c4815d048e5648e2c89c2cc32b8f159cc515f2b5
ms.sourcegitcommit: debaff72dbfae91b303f0acd42dd6d99e03135a2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96428181"
---
# <a name="obtaining-a-single-value-from-a-database"></a>从数据库获取单个值

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

您可能需要返回只是单个值的数据库信息，而不需要返回表或数据流形式的数据库信息。 例如，可能需要返回 COUNT(\*)、SUM(Price) 或 AVG(Quantity) 等聚合函数的结果。 Command 对象使用 ExecuteScalar 方法提供了返回单个值的功能。 ExecuteScalar 方法以标量值的形式返回结果集第一行的第一列的值。

## <a name="example"></a>示例

下面的代码示例使用 <xref:Microsoft.Data.SqlClient.SqlCommand> 在数据库中插入一个新值。 使用 <xref:Microsoft.Data.SqlClient.SqlCommand.ExecuteScalar%2A> 方法可返回已插入记录的标识列值。

[!code-csharp[DataWorks SqlCommand.ExecuteScalar#1](~/../sqlclient/doc/samples/SqlCommand_ExecuteScalar_Return_Id.cs#1)]

## <a name="see-also"></a>请参阅

- [命令和参数](commands-parameters.md)
- [执行命令](execute-command.md)
