---
description: SQLGetStmtOption 映射
title: SQLGetStmtOption 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetStmtOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLGetStmtOption
ms.assetid: fa599517-3f3e-4dad-a65a-b8596ae3f330
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa5d59afe92dbbf6a2e0d2d2cc08abb1a529fe5b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202721"
---
# <a name="sqlgetstmtoption-mapping"></a>SQLGetStmtOption 映射
当应用程序对不支持 *的 ODBC 1.x* 驱动程序调用 **SQLGetStmtOption** 时，调用  
  
```  
SQLGetStmtOption(hstmt, fOption, pvParam)  
```  
  
 将如下所示：  
  
-   如果 *fOption* 指示一个返回字符串的 ODBC 定义的语句选项，则驱动程序管理器将调用  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
-   如果 *fOption* 指示一个返回32位整数值的 ODBC 定义的语句选项，则驱动程序管理器将调用  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, 0, NULL)  
    ```  
  
-   如果 *fOption* 指示驱动程序定义的语句选项，驱动程序管理器将调用  
  
    ```  
    SQLGetStmtAttr(StatementHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
 在上述三种情况下， *StatementHandle* 参数设置为 *hstmt* 中的值， *特性* 参数设置为 *fOption* 中的值， *将 valueptr* 参数设置为与 *pvParam* 相同的值。  
  
 对于 ODBC 定义的字符串连接选项，驱动程序管理器会将对 **SQLGetConnectAttr** 的调用中的 *BufferLength* 参数设置为预定义的最大长度 (SQL_MAX_OPTION_STRING_LENGTH) ;对于非字符串连接选项， *BufferLength* 设置为0。  
  
 *ODBC 2.x* 中已弃用 SQL_GET_BOOKMARK 语句选项。 要使 ODBC *2.x 驱动程序* 与使用 SQL_GET_BOOKMARK *的 odbc 2.x* 应用程序一起工作，必须支持 SQL_GET_BOOKMARK。 若要使用 *odbc 2.x 驱动程序*，必须支持将 SQL_USE_BOOKMARKS 设置为 SQL_UB_ON，并应公开固定长度书签。 如果 ODBC 1.x *驱动程序* 仅支持可变长度书签，而不支持固定长度书签，则在 odbc 2.x *应用程序* 尝试将 SQL_USE_BOOKMARKS 到 SQL_UB_ON 时，它必须返回 SQLSTATE HYC00 (未实现的可选功能) 。  
  
 对于 ODBC 1.x 驱动程序， *驱动程序管理* 器不再检查 SQL_STMT_OPT_MIN 和 SQL_STMT_OPT_MAX 之间的 *选项* ，或该选项是否大于 SQL_CONNECT_OPT_DRVR_START。 驱动程序必须选中此选项。
