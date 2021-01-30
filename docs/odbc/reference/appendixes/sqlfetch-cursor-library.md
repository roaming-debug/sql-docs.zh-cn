---
description: SQLFetch（游标库）
title: SQLFetch (游标库) |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFetch function [ODBC], Cursor Library
ms.assetid: 35a0d493-778b-4fb1-84ee-a13540e2fe0e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7e0ed0b94d4ad66c22ef1a29055c7a0144e4f941
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202857"
---
# <a name="sqlfetch-cursor-library"></a>SQLFetch（游标库）
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 本主题讨论如何在游标库中使用 **SQLFetch** 函数。 有关 **SQLFetch** 的常规信息，请参阅 [SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)。  
  
 使用游标库时，对 **SQLFetch** 的调用不能与 **SQLFetchScroll** 或 **SQLExtendedFetch** 的调用混合使用。  
  
 如果调用 **SQLFetch** 并将 SQL_ATTR_ROW_ARRAY_SIZE 设置为大于1的值，则游标库将传递对驱动程序的调用。 如果驱动程序为 ODBC 2，则为。*x* 驱动程序，将忽略行集的大小，而对 **SQLFetch** 的调用将返回一行数据。  
  
 如果游标库与 ODBC 2 一起使用。*x* 驱动程序，调用 **SQLFetch** 时，不会使用由 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性定义)  (的绑定偏移量。  
  
 加载游标库时，应用程序无法调用 **SQLFetch** 来提取书签列。 游标库将对 **SQLFetch** 的调用传递给驱动程序，但函数调用启用书签并绑定书签列由游标库截获。
