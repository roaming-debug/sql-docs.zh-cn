---
title: 大容量复制操作的顺序提示
description: 介绍如何在大容量复制操作中使用顺序提示。
ms.date: 06/15/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: ba652ab9eb1cbe6564a8f1b2aa5d7309cba86ac4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837094"
---
# <a name="order-hints-for-bulk-copy-operations"></a>大容量复制操作的顺序提示

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

大容量复制操作与其他用于将数据加载到 SQL Server 表中的方法相比，具有显著的性能优势。 使用顺序提示可以进一步增强性能。 为大容量复制操作指定顺序提示可以降低将排序数据插入具有聚集索引的表中的时间。

默认情况下，大容量插入操作假设传入数据未排序。 SQL Server 在大容量加载之前强制执行此数据的中间排序。 如果你知道传入数据已排序，则可以使用顺序提示来告诉大容量复制操作有关聚集索引中的任何目标列的排序顺序。
  
## <a name="adding-order-hints-to-a-bulk-copy-operation"></a>向大容量复制操作添加顺序提示  
以下示例将数据从“AdventureWorks”示例数据库中的源表大容量复制到同一个数据库中的目标表。 SqlBulkCopyColumnOrderHint 对象会被创建，用于定义目标表中“ProductNumber”列的排序顺序。 然后，顺序提示会被添加到 SqlBulkCopy 实例，此操作会将相应的顺序提示参数追加到生成的 `INSERT BULK` 查询。

[!code-csharp [SqlBulkCopy.ColumnOrderHint#1](~/../sqlclient/doc/samples/SqlBulkCopy_ColumnOrderHint.cs#1)]

## <a name="next-steps"></a>后续步骤
- [SQL Server 中的大容量复制操作](bulk-copy-operations-sql-server.md)
