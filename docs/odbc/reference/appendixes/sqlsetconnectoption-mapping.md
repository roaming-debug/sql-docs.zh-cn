---
description: SQLSetConnectOption 映射
title: SQLSetConnectOption 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectOption function [ODBC], mapping
- mapping deprecated functions [ODBC], SQLSetConnectOption
ms.assetid: a1b325cf-0c42-41c1-b141-b5a4fee7e708
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 919220fed87878bf0bcf23359f0714f702f9a6a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202661"
---
# <a name="sqlsetconnectoption-mapping"></a>SQLSetConnectOption 映射
当使用 ODBC 2 时。*x* *应用程序通过 ODBC 1.x 驱动程序* 调用 **SQLSetConnectOption** ，调用  
  
```  
SQLSetConnectOption(hdbc, fOption, vParam)  
```  
  
 将如下所示：  
  
-   如果 *fOption* 指示需要字符串的 ODBC 定义的连接属性，则驱动程序管理器将调用  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, SQL_NTS)  
    ```  
  
-   如果 *fOption* 指示一个返回32位整数值的 ODBC 定义的连接属性，则驱动程序管理器将调用  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0)  
    ```  
  
-   如果 *fOption* 指示驱动程序定义的连接属性，则驱动程序管理器将调用  
  
    ```  
    SQLSetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength)  
    ```  
  
 在上述三种情况下， *ConnectionHandle* 参数设置为 *hdbc* 中的值， *特性* 参数设置为 *fOption* 中的值， *将 valueptr* 参数设置为与 *vParam* 相同的值。  
  
 由于驱动程序管理器不知道驱动程序定义的连接属性是否需要字符串或32位整数值，因此必须为 **SQLSetConnectAttr** 的 *BufferLength* 参数传递有效值。 如果驱动程序已为驱动程序定义的连接属性定义了特殊语义并且需要使用 **SQLSetConnectOption** 进行调用，则该驱动程序必须支持 **SQLSetConnectOption**。  
  
 如果为 ODBC 2，则为。*x* 应用程序调用 **SQLSETCONNECTOPTION** 来设置 odbc *1.x 驱动程序* 中的驱动程序特定的语句选项，并且该选项是在 odbc 2 中定义的。*x* 版本的驱动程序，应为 ODBC *1.x 驱动程序* 中的选项定义新的清单常量。 如果在对 **SQLSetConnectOption** 的调用中使用旧清单常量，驱动程序管理器将调用 **SQLSetConnectAttr** ，并将 **StringLength** 参数设置为0。  
  
 对于 ODBC *2.x 驱动程序，驱动程序管理* 器不再检查 *fOption* 是否在 SQL_CONN_OPT_MIN 和 SQL_CONN_OPT_MAX 之间，或者是否大于 SQL_CONNECT_OPT_DRVR_START。  
  
## <a name="setting-statement-options-on-the-connection-level"></a>设置连接级别的语句选项  
 在 ODBC 2 中。*x*，应用程序可以调用 **SQLSetConnectOption** 来设置语句选项。 完成此操作后，驱动程序将为以后为该连接分配的任何语句的默认值建立语句选项。 它是驱动程序定义的，无论驱动程序是否为任何与指定连接关联的现有语句设置语句选项。  
  
 ODBC 2.x 已弃用此 *功能。* ODBC 2.x *驱动程序* 只需支持设置 ODBC 2。如果要使用 ODBC 2，则为连接级别的 *x* 语句特性。用于执行此操作的 *x* 应用程序。 ODBC 2.x *应用程序* 决不应在连接级别设置语句属性。 ODBC 2.x *语句特性* 不能在连接级别设置，但 SQL_ATTR_METADATA_ID 和 SQL_ATTR_ASYNC_ENABLE 属性（它们都是连接属性和语句特性）除外，可以在连接级别或语句级别设置。
