---
description: 滚动和提取行
title: 滚动和提取行 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [SQL Server]
- SQL Server Native Client ODBC driver, cursors
- SQLFetchScroll function
- ODBC applications, cursors
- cursors [ODBC], fetching rows
- ODBC cursors, fetching rows
- cursors [ODBC], scrolling rows
- fetching [ODBC]
- ODBC cursors, scrolling rows
ms.assetid: 9109f10d-326b-4a6d-8c97-831f60da8c4c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 88b9ef45cbfcd4bb87c7827d4601fa6c4e545bd0
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756167"
---
# <a name="scrolling-and-fetching-rows"></a>滚动和提取行
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  若要使用可滚动游标，ODBC 应用程序必须：  
  
-   使用 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)设置光标功能。  
  
-   使用 **SQLExecute** 或 **SQLExecDirect** 打开游标。  
  
-   使用 **SQLFetch** 或 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)滚动和提取行。  
  
 **SQLFetch** 和 **SQLFetchSroll** 一次可以提取行块。 返回的行数是通过使用 **SQLSetStmtAttr** 设置 SQL_ATTR_ROW_ARRAY_SIZE 参数指定的。  
  
 ODBC 应用程序可以使用 **SQLFetch** 来提取只进游标。  
  
 **SQLFetchScroll** 用于在游标周围滚动。 **SQLFetchScroll** 支持提取下一个、以前的、第一个和最后一个行集， (提取从当前行集开头开始的行集 *n* 行) 和绝对提取 (从第 *n* 行开始) 提取行集。 如果在绝对提取中 *n* 为负，则从结果集的末尾开始对行进行计数。 绝对提取行 -1 表示提取从结果集中最后一行开始的行集。  
  
 仅对其块游标功能（如报表）使用 **SQLFetchScroll** 的应用程序可能只使用用于提取下一个行集的选项传递结果集。 另一方面，基于屏幕的应用程序可以利用 **SQLFetchScroll** 的所有功能。 如果应用程序将行集大小设置为屏幕上显示的行数，并将屏幕缓冲区绑定到结果集，则可以直接将滚动条操作转换为对 **SQLFetchScroll** 的调用。  
  
|滚动条操作|SQLFetchScroll 滚动选项|  
|--------------------------|-------------------------------------|  
|向上翻页|SQL_FETCH_PRIOR|  
|向下翻页|SQL_FETCH_NEXT|  
|向上移动一行|FetchOffset 等于-1 的 SQL_FETCH_RELATIVE|  
|向下移动一行|SQL_FETCH_RELATIVE，FetchOffset 等于 1|  
|滚动框移到顶部|SQL_FETCH_FIRST|  
|滚动框移到底部|SQL_FETCH_LAST|  
|滚动框位于随机位置|SQL_FETCH_ABSOLUTE|  
  
## <a name="in-this-section"></a>本节内容  
  
-   [在 ODBC 中为行加书签](../../relational-databases/native-client-odbc-cursors/scrolling-and-fetching-rows-bookmarking-rows-in-odbc.md)  
  
## <a name="see-also"></a>另请参阅  
 [使用游标 &#40;ODBC&#41;](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  
