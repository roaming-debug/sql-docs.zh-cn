---
description: SQLSetScrollOptions 映射
title: SQLSetScrollOptions 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetScrollOptions function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetScrollOptions
ms.assetid: a0fa4510-8891-4a61-a867-b2555bc35f05
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 105443a2b33a1903f1f22affef1c6ca70ee2cfdf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202600"
---
# <a name="sqlsetscrolloptions-mapping"></a>SQLSetScrollOptions 映射
当应用程序 *通过 ODBC 1.x* 驱动程序调用 **SQLSetScrollOptions** 而该驱动程序不支持 **SQLSetScrollOptions** 时，调用  
  
```  
SQLSetScrollOptions(StatementHandle, Concurrency, KeysetSize, RowsetSize)  
```  
  
 将如下所示：  
  
-   调用  
  
    ```  
    SQLGetInfo(ConnectionHandle, InfoType, InfoValuePtr, BufferLength, StringLengthPtr)  
    ```  
  
     将 *InfoType* 参数设置为下表中的一个值，具体取决于 **SQLSetScrollOptions** 中 *KeysetSize* 参数的值。  
  
    |*KeysetSize 参数*|*InfoType 参数*|  
    |---------------------------|-------------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_STATIC|SQL_STATIC_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
    |SQL_SCROLL_DYNAMIC|SQL_DYNAMIC_CURSOR_ATTRIBUTES2|  
    |大于 *RowsetSize* 参数的值|SQL_KEYSET_CURSOR_ATTRIBUTES2|  
  
     如果上表中没有列出 *KeysetSize* 参数的值，则对 **SQLSetScrollOptions** 的调用将返回 SQLSTATE S1107 (行值超出范围) 并且不执行以下任何步骤。  
  
     然后，驱动程序管理器将验证是否根据 **SQLSetScrollOptions** 中的 *Concurrency* 参数的值，在调用 **SQLGetInfo** 时返回的 **InfoValuePtr* 值中设置了相应的位。  
  
    |*并发* 参数|*InfoType* 设置|  
    |----------------------------|------------------------|  
    |SQL_CONCUR_READ_ONLY|SQL_CA2_READ_ONLY_CONCURRENCY|  
    |SQL_CONCUR_LOCK|SQL_CA2_LOCK_CONCURRENCY|  
    |SQL_CONCUR_ROWVER|SQL_CA2_ROWVER_CONCURRENCY|  
    |SQL_CONCUR_VALUES|SQL_CA2_VALUES_CONCURRENCY|  
  
     如果 *concurrency* 参数不是上表中的值之一，则对 **SQLSetScrollOptions** 的调用将返回 SQLSTATE S1108， (Concurrency 选项超出范围) 并且不执行以下任何步骤。 如果前面的) 表中所示的相应位 (未在 **InfoValuePtr* 中设置为与 *Concurrency* 参数对应的值之一，则对 **SQLSETSCROLLOPTIONS** 的调用将返回 SQLSTATE S1C00 (驱动程序无法) ，并且不执行以下任何步骤。  
  
-   调用  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CURSOR_TYPE, ValuePtr, 0)  
    ```  
  
     根据 **SQLSetScrollOptions** 中 *KeysetSize* 参数的值，将 *\* 将 valueptr* 设置为下表中的值之一。  
  
    |*KeysetSize* 参数|*\*ValuePtr*|  
    |---------------------------|------------------|  
    |SQL_SCROLL_FORWARD_ONLY|SQL_CURSOR_FORWARD_ONLY|  
    |SQL_SCROLL_STATIC|SQL_CURSOR_STATIC|  
    |SQL_SCROLL_KEYSET_DRIVEN|SQL_CURSOR_KEYSET_DRIVEN|  
    |SQL_SCROLL_DYNAMIC|SQL_CURSOR_DYNAMIC|  
    |大于 *RowsetSize* 参数的值|SQL_CURSOR_KEYSET_DRIVEN|  
  
-   调用  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_CONCURRENCY, ValuePtr, 0)  
    ```  
  
     在 **SQLSetScrollOptions** 中， *\* 将 valueptr* 设置为 *Concurrency* 参数。  
  
-   如果对 **SQLSetScrollOptions** 的调用中的 *KeysetSize* 参数为正值，则调用  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ATTR_KEYSET_SIZE, ValuePtr, 0)  
    ```  
  
     将 *\* 将 valueptr* 设置为 **SQLSetScrollOptions** 中的 *KeysetSize* 参数。  
  
-   调用  
  
    ```  
    SQLSetStmtAttr(StatementHandle, SQL_ROWSET_SIZE, ValuePtr, 0)  
    ```  
  
     将 *\* 将 valueptr* 设置为 **SQLSetScrollOptions** 中的 *RowsetSize* 参数。  
  
    > [!NOTE]  
    >  当驱动程序管理器为使用不支持 **SQLSetScrollOptions***的 ODBC 1.x 驱动程序的* 应用程序映射 **SQLSetScrollOptions** 时，驱动程序管理器会将 SQL_ROWSET_SIZE 语句选项，而不是 SQL_ATTR_ROW_ARRAY_SIZE 语句特性）设置为 **SQLSetScrollOption** 中的 *RowsetSize* 参数。 因此，在通过调用 **SQLFetch** 或 **SQLFetchScroll** 提取多行时，应用程序不能使用 **SQLSetScrollOptions** 。 它只能在通过调用 **SQLExtendedFetch** 提取多行时使用。
