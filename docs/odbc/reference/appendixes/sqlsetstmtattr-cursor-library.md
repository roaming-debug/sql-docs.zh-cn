---
description: SQLSetStmtAttr（游标库）
title: SQLSetStmtAttr (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetStmtAttr function [ODBC], Cursor Library
ms.assetid: 6018a733-c2c8-4047-92ec-92cf85031767
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8a395052df9eb5623e63463e3e79722769583721
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202592"
---
# <a name="sqlsetstmtattr-cursor-library"></a>SQLSetStmtAttr（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLSetStmtAttr** 函数。 有关 **SQLSetStmtAttr** 的常规信息，请参阅 [SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)。  
  
 游标库支持 **SQLSetStmtAttr** 的以下语句属性：  

:::row:::
    :::column:::
        SQL_ATTR_CONCURRENCY  
        SQL_ATTR_CURSOR_TYPE  
        SQL_ATTR_FETCH_BOOKMARK_PTR  
        SQL_ATTR_PARAM_BIND_OFFSET_PTR  
        SQL_ATTR_PARAM_BIND_TYPE  
    :::column-end:::
    :::column:::
        SQL_ATTR_ROW_BIND_OFFSET_PTR  
        SQL_ATTR_ROW_BIND_TYPE  
        SQL_ATTR_ROWSET_ARRAY_SIZE  
        SQL_ATTR_SIMULATE_CURSOR  
        SQL_ATTR_USE_BOOKMARKS  
    :::column-end:::
:::row-end:::

 游标库仅支持 SQL_ATTR_CURSOR_TYPE 语句特性的 SQL_CURSOR_FORWARD_ONLY 和 SQL_CURSOR_STATIC 值。  
  
 对于只进游标，游标库支持 SQL_ATTR_CONCURRENCY 语句特性的 SQL_CONCUR_READ_ONLY 值。 对于静态游标，游标库支持 SQL_ATTR_CONCURRENCY 语句特性的 SQL_CONCUR_READ_ONLY 和 SQL_CONCUR_VALUES 值。  
  
 游标库仅支持 SQL_ATTR_SIMULATE_CURSOR 语句特性的 SQL_SC_NON_UNIQUE 值。  
  
 尽管 ODBC 规范支持在调用 **SQLFetch** 或 **SQLFetchScroll** 后调用带有 SQL_ATTR_PARAM_BIND_TYPE 或 SQL_ATTR_ROW_BIND_TYPE 属性的 **SQLSetStmtAttr** ，但游标库却不支持。 应用程序必须关闭游标，然后才能更改游标库中的绑定类型。 游标库支持在游标打开时更改 SQL_ATTR_ROW_BIND_OFFSET_PTR、SQL_ATTR_PARAM_BIND_OFFSET_PTR、SQL_ATTR_ROWS_FETCHED_PTR 和 SQL_ATTR_PARAMS_PROCESSED_PTR 语句特性。  
  
 应用程序可以使用 SQL_ATTR_ROW_ARRAY_SIZE 的 **属性** 调用 **SQLSetStmtAttr** ，以便在打开游标时更改行集的大小。 新的行集大小将在下一次调用 **SQLFetchScroll** 或 **SQLFetch** 时生效。  
  
 游标库支持将 SQL_ATTR_PARAM_BIND_OFFSET_PTR 或 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性设置为启用绑定偏移量。 当游标库与 ODBC 2 一起使用时，绑定偏移量不会用于对 **SQLFetch** 的调用。*x* 驱动程序。  
  
 游标库支持将 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE。
