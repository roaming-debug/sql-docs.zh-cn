---
description: 定位更新 (ODBC)
title: ODBC) 的定位更新 (|Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- SQLSetPos function
- SQLSetCursorName function
- ODBC applications, cursors
- cursors [ODBC], positioned updates
- positioned updates [ODBC]
- ODBC cursors, positioned updates
ms.assetid: ff404e02-630f-474d-b5d4-06442b756991
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae8fe4c4118b0ec3f58b42f01e08f58d6367a5db
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438610"
---
# <a name="positioned-updates-odbc"></a>定位更新 (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 支持通过两种方法在游标中执行定位更新：  
  
-   **SQLSetPos**  
  
-   WHERE CURRENT OF 子句  
  
 更常见的方法是使用 **SQLSetPos**。 它具有以下选项。  
  
 SQL_POSITION  
 将游标定位到当前行集中的特定行。  
  
 SQL_REFRESH  
 使用游标当前所定位行的值刷新被绑定到结果集列上的程序变量。  
  
 SQL_UPDATE  
 使用绑定到结果集列上的程序变量中所存储的值来更新游标中的当前行。  
  
 SQL_DELETE  
 删除游标中的当前行。  
  
 当语句句柄游标属性设置为使用服务器游标时，可以将 **SQLSetPos** 与任何语句结果集一起使用。 结果集列必须绑定到程序变量。 应用程序提取行后，会立即调用 **SQLSetPos** (SQL_POSTION) 将游标定位在该行上。 随后应用程序可以调用 SQLSetPos(SQL_DELETE) 删除当前行，或者可以将新数据值移入绑定程序变量并调用 SQLSetPos(SQL_UPDATE) 更新当前行。  
  
 应用程序可以通过 **SQLSetPos** 更新或删除行集中的任何行。 调用 **SQLSetPos** 是构造和执行 SQL 语句的一种简便方法。 **SQLSetPos** 对当前行集进行操作，并且只能在调用 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)后使用。  
  
 行集大小通过使用 SQL_ATTR_ROW_ARRAY_SIZE 的属性参数对 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 的调用设置。 **SQLSetPos** 仅在调用 **SQLFetch** 或 **SQLFetchScroll** 后使用新的行集大小。 例如，如果行集大小发生更改，则会调用 **SQLSetPos** ，然后调用 **SQLFetch** 或 **SQLFetchScroll** 。 对 **SQLSetPos** 的调用使用旧的行集大小，但 **SQLFetch** 或 **SQLFetchScroll** 使用新的行集大小。  
  
 行集中的第一行的行编号为 1。 **SQLSetPos** 中的 RowNumber 参数必须标识行集中的行;也就是说，其值必须介于1和最近提取的行数之间。 这可能小于行集大小。 如果 RowNumber 为 0，将对行集中的所有行应用操作。  
  
 **SQLSetPos** 的 delete 操作使数据源删除表中的一个或多个选定行。 若要删除 **SQLSetPos** 的行，应用程序将调用 **SQLSetPos** ，并将操作设置 SQL_DELETE 为，并将 RowNumber 设置为要删除的行号。 如果 RowNumber 为 0，将删除行集中的所有行。  
  
 **SQLSetPos** 返回后，删除的行是当前行，其状态为 SQL_ROW_DELETED。 该行不能用于任何其他定位操作，例如对 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) 或 **SQLSetPos** 的调用。  
  
 删除行集的所有行时 (RowNumber 等于 0) 时，应用程序可以通过使用行操作数组来阻止该驱动程序删除某些行，这与 **SQLSetPos** 的更新操作类似。  
  
 每个删除的行都应该是结果集中存在的行。 如果应用程序缓冲区已被提取操作填充，并且保持了行状态数组，则所有这些行位置处的行值都不应为 SQL_ROW_DELETED、SQL_ROW_ERROR 或 SQL_ROW_NOROW。  
  
 通过在 UPDATE、DELETE 和 INSERT 语句中使用 WHERE CURRENT OF 子句，也可以执行定位更新。 其中，CURRENT of 需要在调用 [SQLGetCursorName](../../relational-databases/native-client-odbc-api/sqlgetcursorname.md) 函数时 ODBC 将生成的游标名称，或者通过调用 **SQLSetCursorName** 来指定。 以下是用于在 ODBC 应用程序中执行 WHERE CURRENT OF 更新的一般步骤：  
  
-   调用 **SQLSetCursorName** 为语句句柄建立游标名称。  
  
-   生成包含 FOR UPDATE OF 子句的 SELECT 语句，并执行该语句。  
  
-   调用 **SQLFetchScroll** 可检索行集或 **SQLFetch** 以检索行。  
  
-   调用 **SQLSetPos** (SQL_POSITION) 将游标定位在该行上。  
  
-   使用 **SQLSetCursorName** 设置的游标名称，生成和执行包含 WHERE CURRENT of 子句的 UPDATE 语句。  
  
 或者，在执行 select 语句后（而不是在执行 SELECT 语句之前调用 **SQLSetCursorName** ），可以调用 **SQLGetCursorName** 。 如果未使用 **SQLSetCursorName** 设置游标名称， **SQLGETCURSORNAME** 将返回 ODBC 分配的默认游标名称。  
  
 使用服务器游标时， **SQLSetPos** 的优先级高于当前的。 如果使用 ODBC 游标库中的静态、可更新游标，该游标库通过添加一个 WHERE 子句（带有基础表的键值）实现 WHERE CURRENT OF 更新。 如果表中的键不唯一，这可能导致意外更新。  
  
## <a name="see-also"></a>另请参阅  
 [使用游标 &#40;ODBC&#41;](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  
