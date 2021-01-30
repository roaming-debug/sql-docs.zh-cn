---
description: 驱动程序的用途
title: 驱动程序的作用 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- scrollable cursors [ODBC]
- cursors [ODBC], backward compatibility
- compatibility [ODBC], cursors
- backward compatibility [ODBC], cursors
- block cursors [ODBC]
ms.assetid: 75dcdea6-ff6b-4ac8-aa11-a1f9edbeb8e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 37c6c84b74e6b89dc9b31a342e9c47c8a63eb030
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158181"
---
# <a name="what-the-driver-does"></a>驱动程序的用途
下表总结 *了 ODBC 2.x* 驱动程序应为块和可滚动游标实现的函数和语句属性。  
  
|函数或<br /><br /> 语句特性|注释|  
|-----------------------------------------|--------------|  
|SQL_ATTR_ROW_STATUS_PTR|设置由 **SQLFetch** 和 **SQLFetchScroll** 填充的行状态数组的地址。 如果在语句状态 S6 中调用 **SQLSetPos** ，则此数组也由 **SQLSetPos** 填充。 如果在状态 S7 中调用 **SQLSetPos** ，则不会填充此数组，但会填充 **SQLExtendedFetch** 的 *RowStatusArray* 参数所指向的数组。 有关详细信息，请参阅附录 B： ODBC 状态转换表中的 [语句转换](../../../odbc/reference/appendixes/statement-transitions.md) 。|  
|SQL_ATTR_ROWS_FETCHED_PTR|设置缓冲区的地址，其中 **SQLFetch** 和 **SQLFetchScroll** 返回提取的行数。 如果调用 **SQLExtendedFetch** ，则不填充此缓冲区，而 *RowCountPtr* 参数则指向提取的行数。|  
|SQL_ATTR_ROW_ARRAY_SIZE|设置 **SQLFetch** 和 **SQLFetchScroll** 使用的行集大小。|  
|SQL_ROWSET_SIZE|设置 **SQLExtendedFetch** 使用的行集大小。 如果 ODBC 2.x 驱动程序要使用调用 **SQLExtendedFetch** 或 **SQLSetPos***的 odbc 1.x* *应用程序，* 则会实现此操作。|  
|**SQLBulkOperations**|如果 ODBC 1.x *驱动程序* 应与使用 **SQLSetPos** 的 odbc 2.x 应用程序一起使用，并且 *该应用程序* 使用的是 SQL_ADD 的 *操作*，则该驱动程序必须支持 **SQLSetPos** 的 SQL_ADD *操作* 以及具有 SQL_ADD *操作* 的 **SQLBulkOperations** 。|  
|**SQLExtendedFetch**|返回指定的行集。 如果 ODBC 2.x 驱动程序要使用调用 **SQLExtendedFetch** 或 **SQLSetPos***的 odbc 1.x* *应用程序，* 则会实现此操作。 下面是实现的详细信息：<br /><br /> -驱动程序从 SQL_ROWSET_SIZE 语句特性的值检索行集大小。<br />-驱动程序从 *RowStatusArray* 参数而不是 SQL_ATTR_ROW_STATUS_PTR 语句属性中检索行状态数组的地址。 对 **SQLExtendedFetch** 的调用中的 *RowStatusArray* 参数不能为 null 指针。  (请 *注意，在 ODBC 3.x* 中，SQL_ATTR_ROW_STATUS_PTR 语句特性可以是 null 指针。 ) <br />-驱动程序从 *RowCountPtr* 参数而不是 SQL_ATTR_ROWS_FETCHED_PTR 语句属性中检索已提取缓冲区的行的地址。<br />-驱动程序在行) 中返回 SQLSTATE 01S01 (错误，指示通过调用 **SQLExtendedFetch** 获取行时出现错误。 仅当调用 **SQLExtendedFetch** 时 *，ODBC 1.x* 驱动程序应返回 SQLSTATE 01S01 () 错误，而不应在调用 **SQLFetch** 或 **SQLFetchScroll** 时返回。 若要保留向后兼容性，在 **SQLExtendedFetch** 返回行) 中的 SQLSTATE 01S01 (错误时，驱动程序管理器不会根据 [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)的 "状态记录序列" 一节中所述的规则对错误队列中的状态记录进行排序。|  
|**SQLFetch**|返回下一个行集。 下面是实现的详细信息：<br /><br /> -驱动程序从 SQL_ATTR_ROW_ARRAY_SIZE 语句特性的值检索行集大小。<br />-驱动程序从 SQL_ATTR_ROW_STATUS_PTR 语句属性中检索行状态数组的地址。<br />-驱动程序从 SQL_ATTR_ROWS_FETCHED_PTR 语句属性中检索已提取缓冲区的行的地址。<br />-应用程序可以在 **SQLFetchScroll** 和 **SQLFetch** 之间混合调用。<br />-   如果绑定列0， **SQLFetch** 将返回书签。<br />-   可以调用 **SQLFetch** 来返回多行。<br />-驱动程序不会在行) 中返回 SQLSTATE 01S01 (错误，以指示通过调用 **SQLFetch** 获取行时出现错误。|  
|**SQLFetchScroll**|返回指定的行集。 下面是实现的详细信息：<br /><br /> -驱动程序从 SQL_ATTR_ROW_ARRAY_SIZE 语句属性中检索行集大小。<br />-驱动程序从 SQL_ATTR_ROW_STATUS_PTR 语句属性中检索行状态数组的地址。<br />-驱动程序从 SQL_ATTR_ROWS_FETCHED_PTR 语句属性中检索已提取缓冲区的行的地址。<br />-应用程序可以在 **SQLFetchScroll** 和 **SQLFetch** 之间混合调用。<br />-驱动程序不会在行) 中返回 SQLSTATE 01S01 (错误，以指示通过调用 **SQLFetchScroll** 获取行时出现错误。|  
|**SQLSetPos**|执行各种定位操作。 下面是实现的详细信息：<br /><br /> -可在语句状态 S6 或 S7 中调用此项。 有关更多详细信息，请参阅附录 B： ODBC 状态转换表中的 [语句转换](../../../odbc/reference/appendixes/statement-transitions.md) 。<br />-如果在语句状态 S5 或 S6 中调用这种情况，驱动程序将 SQL_ATTR_ROWS_FETCHED_PTR 从 SQL_ATTR_ROW_STATUS_PTR 语句特性中检索行状态数组的行集大小和行状态数组的地址。<br />-如果在语句状态 S7 中调用此，则驱动程序将 SQL_ROWSET_SIZE 从 **SQLExtendedFetch** 的 *RowStatusArray* 参数中检索行状态数组的行集大小和行状态数组的地址。<br />-驱动程序在行) 中返回 SQLSTATE 01S01 (错误，以指示在调用函数时，通过调用 **SQLSetPos** 来执行批量操作时出现错误。 若要保留向后兼容性，在 **SQLSetPos** 返回行) 中的 SQLSTATE 01S01 (错误时，驱动程序管理器不会根据 [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)的 "状态记录顺序" 一节中所述的规则对错误队列中的状态记录进行排序。<br />-如果 *驱动程序应* 与使用 SQL_ADD 的 *操作* 参数调用 **SQLSetPos** 的 ODBC 1.x 应用程序一起使用，则驱动程序必须支持 SQL_ADD 的 *操作* 参数的 **SQLSetPos** 。|
