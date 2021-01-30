---
description: SQLFetchScroll（游标库）
title: SQLFetchScroll (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function [ODBC], Cursor Library
ms.assetid: 4417e57c-31dd-475e-8fe9-eab00a459c80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 719504a6e1e412439314ee6971cf41e05805f592
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202844"
---
# <a name="sqlfetchscroll-cursor-library"></a>SQLFetchScroll（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLFetchScroll** 函数。 有关 **SQLFetchScroll** 的常规信息，请参阅 [SQLFetchScroll 函数](../../../odbc/reference/syntax/sqlfetchscroll-function.md)。  
  
 游标库通过在驱动程序中重复调用 **SQLFetch** 来实现 **SQLFetchScroll** 。 它将从驱动程序检索的数据传输到应用程序提供的行集缓冲区。 它还将数据缓存在内存和磁盘文件中。 当应用程序请求新的行集时，游标库会根据需要从驱动程序检索它 (如果以前未获取) 或缓存 (（如果之前已获取) ）。 最后，游标库维护缓存数据的状态，并将此信息返回到行状态数组中的应用程序。  
  
 使用游标库时，对 **SQLFetchScroll** 的调用不能与 **SQLFetch** 或 **SQLExtendedFetch** 的调用混合使用。  
  
 使用游标库时，ODBC 2 同时支持对 **SQLFetchScroll** 的调用。*x* 和 ODBC 3。*x* 驱动程序。  
  
## <a name="rowset-buffers"></a>行集缓冲区  
 游标库优化了从驱动程序到应用程序提供的行集缓冲区的数据传输，前提是：  
  
-   应用程序使用按行绑定。  
  
-   应用程序声明用于保存行数据的结构中的字段之间没有未使用的字节。  
  
-   其中， **SQLFetch** 或 **SQLFetchScroll** 的字段将返回列的长度/指示器，并在该列的缓冲区之后，在下一列的缓冲区之前。 这些字段是可选的。  
  
 当应用程序请求新行集时，游标库会根据需要从缓存中检索数据，并从驱动程序中检索数据。 如果新旧行集重叠，则游标库可以通过重复使用行集缓冲区的重叠部分中的数据来优化其性能。 因此，对行集缓冲区未保存的更改将丢失，除非新的和旧的行集重叠，并且更改位于行集缓冲区的重叠部分中。 若要保存更改，应用程序会提交定位的 update 语句。  
  
 请注意，当应用程序调用 **SQLFetchScroll** 并将 *FetchOrientation* 参数设置为 SQL_FETCH_RELATIVE 并且 *FetchOffset* 参数设置为0时，游标库始终使用缓存中的数据刷新行集缓冲区。  
  
 游标库支持使用 SQL_ATTR_ROW_ARRAY_SIZE 的 *属性* 调用 **SQLSetStmtAttr** ，以便在打开游标时更改行集的大小。 新的行集大小将在下一次调用 **SQLFetchScroll** 时生效。  
  
## <a name="result-set-membership"></a>结果集成员身份  
 仅当应用程序请求数据时，游标库才从驱动程序中检索数据。 根据数据源和 SQL_CONCURRENCY 语句特性的设置，这会产生以下后果：  
  
-   游标库检索到的数据可能不同于执行语句时可用的数据。 例如，打开游标后，某些驱动程序可以检索在当前光标位置之外的某个点插入的行。  
  
-   结果集中的数据可能被游标库的数据源锁定，因此对于其他用户不可用。  
  
 在游标库缓存了一行数据后，它将无法检测到对基础数据源中的行所做的更改 (除了定位的更新和在同一游标缓存上运行的删除) 。 出现这种情况的原因是，对 **SQLFetchScroll** 的调用，游标库从不 refetches 数据源中的数据。 相反，它会从其缓存中 refetches 数据。  
  
## <a name="scrolling"></a>滚动  
 游标库支持 **SQLFetchScroll** 中的以下提取类型。  
  
|游标类型|提取类型|  
|-----------------|-----------------|  
|只进|SQL_FETCH_NEXT|  
|静态|SQL_FETCH_NEXT<br /><br /> SQL_FETCH_PRIOR<br /><br /> SQL_FETCH_FIRST<br /><br /> SQL_FETCH_LAST<br /><br /> SQL_FETCH_RELATIVE<br /><br /> SQL_FETCH_ABSOLUTE<br /><br /> SQL_FETCH_BOOKMARK|  
  
## <a name="errors"></a>错误  
 调用 **SQLFetchScroll** 时，如果对 **SQLFetch** 的调用之一返回 SQL_ERROR，则游标库将按如下所示继续。 完成这些步骤后，游标库将继续进行处理。  
  
1.  调用 **SQLGetDiagRec** 从驱动程序获取错误信息，并将此信息作为诊断记录发布到驱动程序管理器中。  
  
2.  将诊断记录中的 SQL_DIAG_ROW_NUMBER 字段设置为合适的值。  
  
3.  将诊断记录中的 SQL_DIAG_COLUMN_NUMBER 字段设置为适当的值（如果适用）;否则，它会将其设置为0。  
  
4.  将行状态数组中的错误行的值设置为 SQL_ROW_ERROR。  
  
 在游标库在其 **SQLFetchScroll** 的实现中多次调用了 **SQLFetch** 后，对 **SQLFetch** 的调用之一返回的任何错误或警告都将位于诊断记录中，并可通过对 **SQLGetDiagRec** 的调用来检索。 如果数据在提取时被截断，则截断后的数据现在将驻留在游标库的缓存中。 对 **SQLFetchScroll** 的后续调用将滚动到具有截断数据的行，将返回截断的数据，并且不会引发任何警告，因为数据是从游标库的缓存中提取的。 若要跟踪返回的数据的长度以便它能够确定缓冲区中返回的数据是否已截断，应用程序应绑定长度/指示器缓冲区。  
  
## <a name="bookmark-operations"></a>书签操作  
 游标库支持使用 SQL_FETCH_BOOKMARK 的 *FetchOrientation* 调用 **SQLFetchScroll** 。 它还支持在 *FetchOffset* 参数中指定可用于书签操作的偏移量。 这是游标库支持的唯一书签操作。 游标库不支持调用 **SQLBulkOperations**。  
  
 如果应用程序已设置 SQL_ATTR_USE_BOOKMARKS 语句特性并绑定到 bookmark 列，则游标库将生成一个固定长度的书签，并将其返回给应用程序。 游标库创建并维护其使用的书签;它不使用在数据源中维护的书签。 如果调用 **SQLFetchScroll** 来检索已经从数据源中提取的数据块，则将从游标库缓存中检索数据。 因此，在对 SQL_FETCH_BOOKMARK **SQLFetchScroll** 的调用 *中使用的* 书签必须通过游标库来创建和维护。  
  
## <a name="interaction-with-other-functions"></a>与其他函数交互  
 在准备或执行任何定位的 update 或 delete 语句之前，应用程序必须调用 **SQLFetch** 或 **SQLFetchScroll** 。
