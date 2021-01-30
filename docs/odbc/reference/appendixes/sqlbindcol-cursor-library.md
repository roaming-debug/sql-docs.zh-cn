---
description: SQLBindCol（游标库）
title: SQLBindCol (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLAllocStmt function [ODBC], Cursor Library
ms.assetid: f4dd546a-0a6c-4397-8ee7-fafa6b9da543
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 64b51f58fd51b9bcad1abd86750b024be131a8a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202946"
---
# <a name="sqlbindcol-cursor-library"></a>SQLBindCol（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLBindCol** 函数。 有关 **SQLBindCol** 的常规信息，请参阅 [SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)。  
  
 应用程序为游标库分配一个或多个缓冲区以返回中的当前行集。 它一次或多次调用 **SQLBindCol** ，将这些缓冲区绑定到结果集。  
  
 应用程序可以调用 **SQLBindCol** ，以便在调用 **SQLExtendedFetch**、 **SQLFetch** 或 **SQLFetchScroll** 后重新绑定结果集列，前提是绑定列的 C 数据类型、列大小和小数位数保持不变。 应用程序无需关闭游标即可将列重新绑定到不同地址。  
  
 游标库支持将 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性设置为使用绑定偏移量。 无需调用 (**SQLBindCol** 即可进行此重新绑定。 ) 如果游标库 *与 ODBC 1.x* 驱动程序一起使用，则在调用 **SQLFetch** 时不会使用绑定偏移量。 如果在将游标库 *与 ODBC 2.x* 驱动程序一起使用时调用 **SQLFetch** ，则使用绑定偏移量，因为 **SQLFetch** 随后映射到 **SQLExtendedFetch**。  
  
 游标库支持调用 **SQLBindCol** 来绑定书签列。  
  
 当 *使用 ODBC 2.x* 驱动程序时，当调用 **SQLBindCol** 将书签列的缓冲长度设置为不等于4的值时，游标库将返回 SQLSTATE HY090 (无效的字符串或缓冲区长度) 。 当 *使用 ODBC 1.x* 驱动程序时，游标库允许缓冲区为任意大小。
