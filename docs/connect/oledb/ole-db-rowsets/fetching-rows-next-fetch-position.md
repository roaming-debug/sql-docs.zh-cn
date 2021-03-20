---
title: 下次提取位置 (OLE DB driver) | Microsoft Docs
description: OLE DB Driver for SQL Server 跟踪下一个提取位置，以便对 GetNextRows 方法的一系列调用能够读取整个行集。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- fetching rows
- OLE DB rowsets, fetching
- next fetch position
- rowsets [OLE DB], fetching
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c2917b9ce4377b77f45b8a288f459113da5744f9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753077"
---
# <a name="fetching-rows---next-fetch-position-ole-db-driver"></a>提取行 - 下次提取位置 (OLE DB driver)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  适用于 SQL Server 的 OLE DB 驱动程序对下次提取位置保持跟踪，以便对 GetNextRows 方法的一系列调用（无需跳过、更改方向或干预对 FindNextRow、Seek 或 RestartPosition 方法的调用）读取整个行集，而不跳过或重复任何行     。 可以通过调用 IRowset::GetNextRows、IRowset::RestartPosition 或 IRowsetIndex::Seek，或通过调用 FindNextRow 且 pBookmark 值为空来更改下次提取位置      。 调用 FindNextRow 且 pBookmark 值不为空时，不会影响下次提取位置   。  
  
## <a name="see-also"></a>另请参阅  
 [提取行](../../oledb/ole-db-rowsets/fetching-rows.md)  
  
  
