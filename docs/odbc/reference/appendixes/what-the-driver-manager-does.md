---
description: 驱动程序管理器的用途
title: 驱动程序管理器的作用 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- driver manager [ODBC], backward compatibility
- compatibility [ODBC], driver manager
- ODBC driver manager [ODBC]
- backward compatibility [ODBC], driver manager
ms.assetid: 57f65c38-d9ee-46c8-9051-128224a582c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d210d5c202f11151cf22a64362b652958422f513
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158172"
---
# <a name="what-the-driver-manager-does"></a>驱动程序管理器的用途
下表总结了 ODBC 2.x *驱动程序管理* 器如何映射 *对 odbc 2.x* *驱动程序和* odbc 2.x 驱动程序的调用。  
  
|函数或<br /><br /> 语句特性|注释|  
|-----------------------------------------|--------------|  
|SQL_ATTR_FETCH_BOOKMARK_PTR|指向要用于 **SQLFetchScroll** 的书签。 下面是实现的详细信息：<br /><br /> -当应用程序在 ODBC 2.x 驱动程序中设置此项时 *，odbc 1.X* 驱动程序管理器将缓存该 *驱动程序。* 当应用程序稍后调用 **SQLFetchScroll** 时，它将取消引用指针并将值传递给 **SQLExtendedFetch** 的 *FetchOffset* 参数中的 ODBC 2.x 驱动 *程序。*<br />-当应用程序在 ODBC 1.x *驱动程序* 中设置此项时，odbc *3.X 驱动程序* 管理器会将该调用传递给驱动程序。|  
|SQL_ATTR_ROW_STATUS_PTR|指向由 **SQLFetch**、 **SQLFetchScroll**、 **SQLBulkOperations** 和 **SQLSetPos** 填充的行状态数组。 下面是实现的详细信息：<br /><br /> -当应用程序在 ODBC 2.x 驱动程序中设置此值时 *，odbc 2.X* *驱动程序管理* 器会缓存其值。 调用 **SQLFetchScroll** 或 **SQLFetch** 时，它会将此值传递到 **SQLExtendedFetch** 的 *RowStatusArray* 参数中 *的 ODBC 2.x 驱动程序。*<br />-当应用程序在 ODBC 1.x *驱动程序* 中设置此项时，odbc *3.X 驱动程序* 管理器会将该调用传递给驱动程序。<br />-在状态 S6 中，如果应用程序设置 SQL_ATTR_ROW_STATUS_PTR 然后使用 SQL_ADD) 或 **SQLSetPos** 的 *操作* 调用 **SQLBulkOperations** (，而无需先调用 **SQLFetch** 或 **SQLFetchScroll**，则 (返回后，将无法设置 SQLSTATE HY011) 特性。|  
|SQL_ATTR_ROWS_FETCHED_PTR|指向缓冲区，其中 **SQLFetch** 和 **SQLFetchScroll** 返回提取的行数。 下面是实现的详细信息：<br /><br /> -当应用程序在 ODBC 2.x 驱动程序中设置此值时 *，odbc 2.X* *驱动程序管理* 器会缓存其值。 当应用程序调用 **SQLFetch** 或 **SQLFetchScroll** 时，它会将此值传递到 **SQLExtendedFetch** 的 *RowCountPtr* 参数中 *的 ODBC 2.x 驱动程序。*<br />-当应用程序在 ODBC 1.x *驱动程序* 中设置此项时，odbc *3.X 驱动程序* 管理器会将该调用传递给驱动程序。|  
|SQL_ATTR_ROW_ARRAY_SIZE|设置行集大小。 下面是实现的详细信息：<br /><br /> -当应用程序在 ODBC 2.x 驱动程序中设置此项时 *，odbc 1.X* *驱动程序管理* 器会将其映射到 SQL_ROWSET_SIZE 语句属性。<br />-当应用程序在 ODBC 1.x *驱动程序* 中设置此项时，odbc *3.X 驱动程序* 管理器会将该调用传递给驱动程序。<br />-当 *使用 ODBC 1.x 驱动程序的* 应用程序调用 **SQLSetScrollOptions** 时，如果基础驱动程序不支持 **SQLSetScrollOptions**，则会将 SQL_ROWSET_SIZE 设置为 *RowsetSize* 参数中的值。|  
|SQL_ROWSET_SIZE|设置 *ODBC 2.x* 应用程序调用 **SQLExtendedFetch** 时 **SQLExtendedFetch** 使用的行集大小。 下面是实现的详细信息：<br /><br /> -当应用程序设置此项时，无论驱动程序版本如何，ODBC 3.x 驱动程序管理器都将传递对 *驱动程序的* 调用。<br />-当 *使用 ODBC 2.x 驱动程序的* 应用程序调用 **SQLSetScrollOptions** 时，SQL_ROWSET_SIZE 设置为 **RowsetSize** 参数中的值。|  
|**SQLBulkOperations**|执行插入操作，或更新、删除或通过书签操作提取。 下面是实现的详细信息：<br /><br /> -当应用程序在 ODBC 2.x 驱动程序中使用 SQL_ADD 的 *操作* 调用 **SQLBULKOPERATIONS** 时 *，Odbc* *3.x 驱动程序* 管理器会将其映射到 **SQLSetPos** ，*操作* 为 SQL_ADD。<br />-在使用不支持 **SQLSetPos** SQL_ADD *操作* 的 odbc 2.x 驱动程序时， *Odbc 3.x 驱动**程序管理* 器不会将 **SQLSetPos** 的 SQL_ADD *操作* 映射到 **SQLBulkOperations** *，操作的* 操作为 SQL_ADD。 这是因为不能在 S7 中调用 **SQLBulkOperations** ，因为 *在 ODBC 2.x* 中只能调用 **SQLSetPos** 。<br />-如果应用程序在调用 **SQLFetchScroll** 之前 *使用 ODBC 2.x 驱动程序**中的 SQL_ADD* 调用 **SQLBulkOperations** ，*则 odbc 1.x* 驱动程序管理器将返回错误。|  
|**SQLExtendedFetch**|返回指定的行集。 除了刚才提到的限制之外，ODBC 3.x 驱动程序管理器会将对 **SQLExtendedFetch** 的调用传递给 *驱动程序，* 而不考虑驱动程序版本。|  
|**SQLFetch**|返回下一个行集。 下面是实现的详细信息：<br /><br /> -当应用程序在 ODBC 2.x 驱动程序中调用 **SQLFetch** 时 *，odbc* *1.x 驱动程序* 管理器会将其映射到 **SQLExtendedFetch**。 **SQLExtendedFetch** 的 *FetchOrientation* 参数设置为 SQL_FETCH_NEXT。 驱动程序管理器使用 *RowStatusArray* 参数的 SQL_ATTR_ROW_STATUS_PTR 语句特性的缓存值，并为 *RowCountPtr* 参数使用已缓存的 SQL_ATTR_ROWS_FETCHED_PTR 语句特性值。<br />-ODBC 2.x 应用程序可以在 ODBC *2.x 驱动程序* 中混合对 **SQLFetch** 和 **SQLFetchScroll** 的调用，因为当应用程序 *在 odbc* 2.X 驱动程序中调用 SQLFetch 时 *，odbc 2.x* *驱动程序管理* 器会将映射到 **SQLExtendedFetch** 。<br />-如果 ODBC 2.x *驱动程序* 不支持 **SQLExtendedFetch**，则在应用程序在该驱动程序中调用 *该驱动程序* 时，它不会将 **SQLFetch** 或 **SQLFetchScroll** 映射到 **SQLExtendedFetch** 。 如果应用程序尝试将 SQL_ATTR_ROW_ARRAY_SIZE 设置为大于1的值，则将返回未实现) 的 SQLSTATE HYC00 (可选功能。<br />-除了上面所述的限制之外，ODBC 3.x 驱动程序管理器将对 **SQLFetch** 的调用传递给 *驱动程序，* 而不考虑驱动程序版本。|  
|**SQLFetchScroll**|返回指定的行集。 下面是实现的详细信息：<br /><br /> -当应用程序在 ODBC 2.x 驱动程序中调用 **SQLFetchScroll** 时 *，odbc* *1.x 驱动程序* 管理器会将其映射到 **SQLExtendedFetch**。 它使用 *RowStatusArray* 参数的 SQL_ATTR_ROW_STATUS_PTR 语句特性的缓存值，并为 *RowCountPtr* 参数使用已缓存的 SQL_ATTR_ROWS_FETCHED_PTR 语句特性的值。 如果 **SQLFetchScroll** 中的 *FetchOrientation* 参数 SQL_FETCH_BOOKMARK，它将使用 *FetchOffset* 参数的 SQL_ATTR_FETCH_BOOKMARK_PTR 语句特性的缓存值，并在 **SQLFetchScroll** 的 *FetchOffset* 参数不为0时返回错误。<br />-当应用程序在 ODBC 1.x 驱动程序中调用此项时 *，odbc 3.X* 驱动程序管理器会将调用传递到该 *驱动程序。*|  
|**SQLSetPos**|执行各种定位操作。 ODBC 2.x 驱动程序管理器将对 **SQLSetPos** 的调用传递给 *驱动程序，* 而不考虑驱动程序版本。|  
|**SQLSetScrollOptions**|当驱动程序管理器为使用不支持 **SQLSetScrollOptions***的 ODBC 1.x 驱动程序的* 应用程序映射 **SQLSetScrollOptions** 时，驱动程序管理器会将 SQL_ROWSET_SIZE 语句选项，而不是 SQL_ATTR_ROW_ARRAY_SIZE 语句特性）设置为 **SQLSetScrollOption** 中的 *RowsetSize* 参数。 因此，在通过调用 **SQLFetch** 或 **SQLFetchScroll** 提取多行时，应用程序不能使用 **SQLSetScrollOptions** 。 它只能在通过调用 **SQLExtendedFetch** 提取多行时使用。|
