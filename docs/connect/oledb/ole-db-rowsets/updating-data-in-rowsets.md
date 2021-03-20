---
title: 更新行集中的数据（OLE DB 驱动程序）
description: 了解 OLE DB Driver for SQL Server 在使用者更新包含该数据的可修改行集时如何更新 SQL Server 数据。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- updating data [SQL Server]
- rowsets [OLE DB], updating data
- OLE DB Driver for SQL Server, rowsets
- OLE DB rowsets, updating data
- OLE DB Driver for SQL Server, data updates
- data updates [SQL Server], OLE DB
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3a9bb3a04af6f75eda05a46da655a971bd60cc71
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755767"
---
# <a name="updating-data-in-rowsets"></a>更新行集中的数据
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  适用于 SQL Server 的 OLE DB 驱动程序在使用者更新包含该数据的可修改行集时更新 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据。 当使用者请求支持 IRowsetChange 或 IRowsetUpdate 接口时，将创建一个可修改的行集   。  
  
 可由 OLE DB Driver for SQL Server 修改的所有行集均使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 游标来支持该行集。 行集属性 DBPROP_LOCKMODE 更改游标中的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 并发控制行为，并确定在可更新行集中提取行集的行和生成数据完整性错误的行为。  
  
 OLE DB Driver for SQL Server 支持在更新前后同步行。  
  
> [!NOTE]  
>  可使用 IRowChange::SetColumns 设置行对象的一个或多个指定列的值。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [更新 SQL Server 游标中的数据](../../oledb/ole-db-rowsets/updating-data-in-sql-server-cursors.md)  
  
-   [重新同步行](../../oledb/ole-db-rowsets/updating-data-in-rowsets-resynchronizing-rows.md)  
  
## <a name="see-also"></a>另请参阅  
 [行集](../../oledb/ole-db-rowsets/rowsets.md)  
  
  
