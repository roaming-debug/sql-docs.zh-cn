---
title: 使用 IRow::GetColumns (OLE DB 驱动程序)
description: 了解如何在 OLE DB Driver for SQL Server 中使用 IRow::GetColumns 访问行中的所有列。 IRow 允许对列进行只进顺序访问。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- fetching rows
- IRow interface
- single row fetching [OLE DB Driver for SQL Server]
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- GetColumns method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f69afdc20abb4890765cd7cdfe1aef362dde5b9f
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750857"
---
# <a name="using-irowgetcolumns"></a>使用 IRow::GetColumns
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  IRow 实现允许对列进行按顺序的只进访问。 可以一次调用 IRow::GetColumns 以访问行中的所有列，也可以多次调用 IRow::GetColumns，每次访问行中的几个列。  
  
 对 IRow::GetColumns 的多次调用不应当重叠。 例如，如果对 IRow::GetColumns 的第一次调用检索第 1、2 和 3 列，则对 IRow::GetColumns 的第二次调用应调用第 4、5 和 6 列。 如果后来对 IRow::GetColumns 的调用发生重叠，则状态标志（DBCOLUMNACCESS 中的 dwstatus 字段）将设置为 DBSTATUS_E_UNAVAILABLE。  
  
## <a name="see-also"></a>另请参阅  
 [使用 IRow 提取单行](../../oledb/ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
  
