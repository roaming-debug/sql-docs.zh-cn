---
description: SQLGetConnectOption 映射
title: SQLGetConnectOption 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLGetConnectOption
- SQLGetConnectOption function [ODBC], mapping
ms.assetid: e3792fe4-a955-473a-a297-c1b2403660c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cdc5f2e9249e262d0d3da9a69607861bfe64bc25
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202800"
---
# <a name="sqlgetconnectoption-mapping"></a>SQLGetConnectOption 映射
当应用程序 *通过 ODBC 1.x* 驱动程序调用 **SQLGetConnectOption** 时，调用  
  
```  
SQLGetConnectOption(hdbc, fOption, pvParam)   
```  
  
 按如下方式映射：  
  
-   如果 *fOption* 指示一个返回字符串的 ODBC 定义连接选项，则驱动程序管理器将调用  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
-   如果 *fOption* 指示一个返回32位整数值的 ODBC 定义的连接选项，则驱动程序管理器将调用  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, 0, NULL)  
    ```  
  
-   如果 *fOption* 指示驱动程序定义的语句选项，驱动程序管理器将调用  
  
    ```  
    SQLGetConnectAttr(ConnectionHandle, Attribute, ValuePtr, BufferLength, NULL)  
    ```  
  
 在上述三种情况下， *ConnectionHandle* 参数设置为 *hdbc* 中的值， *特性* 参数设置为 *fOption* 中的值， *将 valueptr* 参数设置为与 *pvParam* 相同的值。  
  
 对于 ODBC 定义的字符串连接选项，驱动程序管理器会将对 **SQLGetConnectAttr** 的调用中的 *BufferLength* 参数设置为预定义的最大长度 (SQL_MAX_OPTION_STRING_LENGTH) ;对于非字符串连接选项， *BufferLength* 设置为0。  
  
 对于 ODBC 1.x 驱动程序， *驱动程序管理* 器不再 *检查是否在* SQL_CONN_OPT_MIN 和 SQL_CONN_OPT_MAX 之间或大于 SQL_CONNECT_OPT_DRVR_START。 驱动程序必须检查选项值的有效性。
