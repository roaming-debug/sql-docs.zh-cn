---
description: 提取和更新行集 (ODBC)
title: 提取和更新 ODBC)  (的行集 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- rowsets [ODBC]
ms.assetid: cf0eb3b4-8b72-49fc-a845-95edc360cf93
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: adb000e81724279868713fb78d4550d68c308f9b
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104752507"
---
# <a name="fetch-and-update-rowsets-odbc"></a>提取和更新行集 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-fetch-and-update-rowsets"></a>提取和更新行集  
  
1.  （可选）通过 SQL_ROW_ARRAY_SIZE 调用 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 来更改行集中 (R) 的行数。  
  
2.  调用 [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md) 或 [SQLFetchScroll](../../../relational-databases/native-client-odbc-api/sqlfetchscroll.md) 获取行集。  
  
3.  如果使用绑定列，则在行集的绑定列缓冲区中现在可以使用数据值和数据长度。  
  
     如果使用未绑定的列，则对于每个行调用 SQL_POSITION [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md) ，以设置游标位置;然后，对于每个未绑定列：  
  
    -   调用 [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) 一次或多次，以便在行集的最后一个绑定列后获取未绑定列的数据。 对 [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) 的调用应按照递增的列号顺序排列。  
  
    -   多次调用 [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) 以从 text 或 image 列获取数据。  
  
4.  设置任意执行时数据 text 或 image 列。  
  
5.  调用 [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md) 或 [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) 以设置游标位置、刷新、更新、删除或添加行集内)  (s。  
  
     如果执行时数据 text 或 image 列用于某个更新或添加操作，则处理它们。  
  
6.  （可选）执行定位的 UPDATE 或 DELETE 语句，指定 (在 [SQLGetCursorName](../../../relational-databases/native-client-odbc-api/sqlgetcursorname.md) 中可用的游标名) 并在同一连接上使用不同的语句句柄。  
  
## <a name="see-also"></a>另请参阅  
 [使用游标操作指南主题 &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/cursors/using-cursors-how-to-topics-odbc.md)  
  
