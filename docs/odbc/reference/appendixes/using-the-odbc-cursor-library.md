---
description: 使用 ODBC 游标库
title: 使用 ODBC 游标库 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC cursor library [ODBC], using cursor library
- cursor library [ODBC], using cursor library
ms.assetid: 9653f2f8-ccfc-4220-99ef-601dc0fa641c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1658d3c628506f1b5c53a5c9271a10f96fa2ef44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158190"
---
# <a name="using-the-odbc-cursor-library"></a>使用 ODBC 游标库
> [!IMPORTANT]  
>  此功能将在 Windows 的将来版本中删除。 避免在新的开发工作中使用此功能，并计划修改当前使用此功能的应用程序。 Microsoft 建议使用驱动程序的游标功能。  
  
 若要使用 ODBC 游标库，请执行以下操作：  
  
1.  使用 SQL_ATTR_ODBC_CURSORS 的 *属性* 调用 **SQLSetConnectAttr** ，以指定如何将游标库用于特定的连接。 可以始终使用游标库 (SQL_CUR_USE_ODBC) ，仅当驱动程序不支持 (SQL_CUR_USE_IF_NEEDED) 或从不使用 (SQL_CUR_USE_DRIVER) 中的可滚动游标时使用。  
  
2.  调用 **SQLConnect**、 **SQLDriverConnect** 或 **SQLBrowseConnect** 以连接到数据源。  
  
3.  调用 **SQLSetStmtAttr** 来指定游标类型 (SQL_ATTR_CURSOR_TYPE) 、并发 (SQL_ATTR_CONCURRENCY) 和行集大小 (SQL_ATTR_ROW_ARRAY_SIZE) 。 游标库支持只进游标和静态游标。 只进游标必须是只读的，而静态游标可以是只读的，或者可以使用开放式并发控制比较值。  
  
4.  分配一个或多个行集缓冲区，并调用 **SQLBindCol** 一次或多次，将这些缓冲区绑定到结果集列。  
  
5.  通过执行 **SELECT** 语句或过程或通过调用目录函数来生成结果集。 如果应用程序将执行定位的 update 语句，则应执行 **SELECT FOR update** 语句以生成结果集。  
  
6.  调用 **SQLFetch** 或 **SQLFetchScroll** 一次或多次以滚动结果集。  
  
 应用程序可以更改行集缓冲区中的数据值。 若要使用游标库缓存中的数据刷新行集缓冲区，应用程序将调用 **SQLFetchScroll** ，并将 *FetchOrientation* 参数设置为 SQL_FETCH_RELATIVE，并将 *FetchOffset* 参数设置为0。  
  
 若要从未绑定的列中检索数据，应用程序将调用 **SQLSetPos** ，将游标定位到所需的行。 然后，它会调用 **SQLGetData** 来检索数据。  
  
 若要确定从数据源中检索到的行数，应用程序将调用 **SQLRowCount**。
