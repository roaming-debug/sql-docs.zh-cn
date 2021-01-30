---
description: SQLSetStmtOption 映射
title: SQLSetStmtOption 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLSetStmtOption
- SQLSetStmtOption function [ODBC], mapping
ms.assetid: 6a9921aa-8a53-4668-9b13-87164062f1e5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8907de6d6ac80737ba0bb47ca3c6954a482c857f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202581"
---
# <a name="sqlsetstmtoption-mapping"></a>SQLSetStmtOption 映射
当应用程序 *通过 ODBC 1.x* 驱动程序调用 **SQLSetStmtOption** 时，调用  
  
```  
SQLSetStmtOption(StatementHandle, fOption, vParam)  
```  
  
 将如下所示：  
  
-   如果 *fOption* 指示作为字符串的 ODBC 定义的语句特性，则驱动程序管理器将调用  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, SQL_NTS)  
    ```  
  
-   如果 *fOption* 指示一个返回32位整数值的 ODBC 定义的语句特性，则驱动程序管理器将调用  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, 0)  
    ```  
  
-   如果 *fOption* 指示驱动程序定义的语句特性，则驱动程序管理器将调用  
  
    ```  
    SQLSetStmtAttr(StatementHandle, fOption, ValuePtr, BufferLength)  
    ```  
  
 在上述三种情况下， **StatementHandle** 参数设置为 *hstmt* 中的值， *特性* 参数设置为 *fOption* 中的值， *将 valueptr* 参数设置为 *vParam* 值。  
  
 由于驱动程序管理器不知道驱动程序定义的语句特性是否需要字符串或32位整数值，因此必须为 **SQLSetStmtAttr** 的 *StringLength* 参数传递有效值。 如果驱动程序已为驱动程序定义的语句特性定义了特殊语义并且需要使用 **SQLSetStmtOption** 进行调用，则该驱动程序必须支持 **SQLSetStmtOption**。  
  
 如果应用程序调用 **SQLSetStmtOption** 来 *设置 odbc 2.x 驱动程序中* 的驱动程序特定的语句选项，并且该选项是在 odbc 2.x 版本的 *驱动程序中* 定义的，则应 *为 odbc 1.x* 驱动程序中的选项定义新的清单常量。 如果在对 **SQLSetStmtOption** 的调用中使用旧清单常量，驱动程序管理器将调用 **SQLSetStmtAttr** ，并将 *StringLength* 参数设置为0。  
  
 当应用程序调用 **SQLSetStmtAttr** 将 SQL_ATTR_USE_BOOKMARKS 设置为 *在 ODBC 1.x* 驱动程序中 SQL_UB_ON 时，SQL_ATTR_USE_BOOKMARKS 语句特性将设置为 SQL_UB_FIXED。 SQL_UB_ON 与 SQL_UB_FIXED 相同。 驱动程序管理器将 SQL_UB_FIXED 传递给驱动程序。 ODBC 2.x 中的 SQL_UB_FIXED 已 *弃用，但 odbc 2.x**驱动程序* 必须实现它才能与使用固定长度书签 *的 ODBC 2.x* 应用程序一起工作。  
  
 对于 ODBC 1.x 驱动程序， *驱动程序管理* 器不再 *检查是否在* SQL_STMT_OPT_MIN 和 SQL_STMT_OPT_MAX 之间或大于 SQL_CONNECT_OPT_DRVR_START。
