---
description: 在 SQL Server Native Client 中使用 IRow：： GetColumns
title: '使用 IRow：： GetColumns (Native Client OLE DB 提供程序) '
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- fetching rows
- IRow interface
- single row fetching [SQL Server Native Client]
- OLE DB rowsets, fetching
- rowsets [OLE DB], fetching
- GetColumns method
ms.assetid: 1f5d2e03-e6fe-4ea1-b71d-55d02b5d59ae
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f22a575a09d12bf03ebd435f67a35261a5045925
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751827"
---
# <a name="using-irowgetcolumns-in-sql-server-native-client"></a>在 SQL Server Native Client 中使用 IRow：： GetColumns
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  IRow 实现允许对列进行按顺序的只进访问。 可以一次调用 IRow::GetColumns 以访问行中的所有列，也可以多次调用 IRow::GetColumns，每次访问行中的几个列。  
  
 对 IRow::GetColumns 的多次调用不应当重叠。 例如，如果对 IRow::GetColumns 的第一次调用检索第 1、2 和 3 列，则对 IRow::GetColumns 的第二次调用应调用第 4、5 和 6 列。 如果后来对 IRow::GetColumns 的调用发生重叠，则状态标志（DBCOLUMNACCESS 中的 dwstatus 字段）将设置为 DBSTATUS_E_UNAVAILABLE。  
  
## <a name="see-also"></a>另请参阅  
 [使用 IRow 提取单行](../../relational-databases/native-client-ole-db-rowsets/fetching-a-single-row-with-irow.md)  
  
  
