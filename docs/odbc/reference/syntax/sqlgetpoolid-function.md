---
description: SQLGetPoolID 函数
title: SQLGetPoolID 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLGetPoolID function [ODBC]
ms.assetid: 95a8666a-ad68-4d89-bf65-f2cc797f8820
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e7bd06a289eab6499995e40a3c6ad9ea56c747e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180923"
---
# <a name="sqlgetpoolid-function"></a>SQLGetPoolID 函数
**度**  
 引入的版本： ODBC 3.81 标准符合性： ODBC  
  
 **摘要**  
 **SQLGetPoolID** 检索池 ID。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN  SQLGetPoolID (  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                POOLID *              pPoolID );  
```  
  
## <a name="arguments"></a>参数  
 *hDbcInfoToken*  
 送包含所有连接信息的令牌句柄。  
  
 *pPoolID*  
 输出池 ID，用于标识可互换 (的一组连接，这些连接可能需要额外的重置) 。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetPoolID** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，驱动程序管理器将使用 SQL_HANDLE_DBC_INFO_TOKEN 的 **HandleType** 和 *hDbcInfoToken* 的 **句柄**。  
  
## <a name="remarks"></a>备注  
 **SQLGetPoolID** 用于从 **SQLSetConnectAttrForDbcInfo**、 **SQLSetDriverConnectInfo** 和 **SQLSetConnectInfo**)  (的一组连接信息获取池 ID。 此池 ID 用于标识可互换使用的一组连接 (可能需要额外的重置) 。 池 ID 将用于标识该组连接的连接池。  
  
 每当驱动程序返回 SQL_ERROR 或 SQL_INVALID_HANDLE 时，驱动程序管理器会将错误返回到应用程序 (的 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 或 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)) 。  
  
 每当驱动程序返回 SQL_SUCCESS_WITH_INFO 时，驱动程序管理器都将从 *hDbcInfoToken* 获取诊断信息，并将 SQL_SUCCESS_WITH_INFO 返回到 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 和 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)中的应用程序。  
  
 应用程序不应直接调用此函数。 支持驱动程序感知连接池的 ODBC 驱动程序必须实现此功能。  
  
 包括用于 ODBC 驱动程序开发的 sqlspi。  
  
## <a name="see-also"></a>另请参阅  
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [在 ODBC 驱动程序中开发连接池感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
