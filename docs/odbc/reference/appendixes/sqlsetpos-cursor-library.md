---
description: SQLSetPos（游标库）
title: SQLSetPos (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetPos function [ODBC], Cursor Library
ms.assetid: 574399c3-2bb2-4d19-829c-7c77bd82858d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bf97529f4fd411581738459accffc805a444a8ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202620"
---
# <a name="sqlsetpos-cursor-library"></a>SQLSetPos（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLSetPos** 函数。 有关 **SQLSetPos** 的常规信息，请参阅 [SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)。  
  
 游标库仅支持 **SQLSetPos** 中的 *操作* 参数的 SQL_POSITION 操作。 它仅支持 *LockType* 参数的 SQL_LOCK_NO_CHANGE 值。  
  
 如果该驱动程序不支持大容量操作，则调用 **SQLSetPos** *时，游标* 库将返回 SQLSTATE HYC00 (驱动程序) 。 不建议使用此驱动程序行为。  
  
 游标库不支持对 **SQLSetPos** 的调用中的 SQL_UPDATE 和 SQL_DELETE 操作。 游标库通过创建一个使用 WHERE 子句的搜索的 update 或 delete 语句来实现定位的 update 或 delete 语句，该语句枚举为每个绑定列存储在其缓存中的值。 有关详细信息，请参阅 [处理定位的 Update 和 Delete 语句](../../../odbc/reference/appendixes/processing-positioned-update-and-delete-statements.md)。  
  
 如果驱动程序不支持静态游标，则使用游标库的应用程序只应对由 **SQLExtendedFetch** 或 **SQLFetchScroll**（而不是 **SQLFetch**）提取的行集调用 **SQLSetPos** 。 游标库通过对驱动程序中的行集大小为 1) 的 **SQLFetch** (进行重复调用来实现 **SQLExtendedFetch** 和 **SQLFetchScroll** 。 另一方面，游标库将对 **SQLFetch** 的调用传递给驱动程序。 如果在驱动程序不支持静态游标的情况下，在 **SQLFetch** 提取的多行行集上调用 **SQLSetPos** ，则调用将失败，因为 **SQLSetPos** 无法与只进游标一起使用。 即使应用程序已成功调用 **SQLSetStmtAttr** 以将 SQL_ATTR_CURSOR_TYPE 设置为 SQL_CURSOR_STATIC （即使驱动程序不支持静态游标），也会发生这种情况。
