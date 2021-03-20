---
title: 重新同步行（OLE DB 驱动程序）
description: 为了更新行集中的数据，OLE DB Driver for SQL Server 只在 SQL Server 游标支持的行集上支持 IRowsetResynch。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- synchronization [OLE DB]
- IRowsetResynch interface
- resynchronizing rows
- data updates [SQL Server], OLE DB
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f1553c5f077f41212583bbfb47d0fad4a73621c6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755967"
---
# <a name="updating-data-in-rowsets---resynchronizing-rows"></a>更新行集中的数据 - 重新同步行
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  OLE DB Driver for SQL Server 仅对于 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 游标支持的行集支持 IRowsetResynch。 IRowsetResynch 并不是需要时就可用  。 使用者在打开行集前必须请求该接口。  
  
## <a name="see-also"></a>另请参阅  
 [更新行集中的数据](../../oledb/ole-db-rowsets/updating-data-in-rowsets.md)  
  
  
