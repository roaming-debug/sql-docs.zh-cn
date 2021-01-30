---
description: SQLSetDriverConnectInfo 函数
title: SQLSetDriverConnectInfo 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetDriverConnectInfo function [ODBC]
ms.assetid: bfd4dfc2-fbca-4ef3-81e5-2706f2389256
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: de0a21b8fa3e620f4a6884311c10c93c36518849
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174683"
---
# <a name="sqlsetdriverconnectinfo-function"></a>SQLSetDriverConnectInfo 函数
**度**  
 引入的版本： ODBC 3.81 标准符合性： ODBC  
  
 **摘要**  
 **SQLSetDriverConnectInfo** 用于为应用程序的 **SQLDriverConnect** 调用将连接字符串设置为连接信息令牌。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN SQLSetDriverConnectInfo(  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              InConnectionString,  
                SQLSMALLINT          StringLength1 );  
```  
  
## <a name="arguments"></a>参数  
 *TokenHandle*  
 送标记句柄。  
  
 *InConnectionString*  
 送完整的连接字符串 (参阅 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) 中的 "注释") 、部分连接字符串或空字符串。  
  
 *StringLength1*  
 送**InConnectionString* 的长度（如果字符串为 Unicode，则为个字符）; 如果 STRING 为 ANSI 或 DBCS，则为字节。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 与与任何输入验证错误相关的 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)相同，不同之处在于，驱动程序管理器将使用 SQL_HANDLE_DBC_INFO_TOKEN 的 **HandleType** 和 *hDbcInfoToken* 的 **句柄**。  
  
## <a name="remarks"></a>备注  
 每当驱动程序返回 SQL_ERROR 或 SQL_INVALID_HANDLE 时，驱动程序管理器会将错误返回到应用程序 (的 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 或 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)) 。  
  
 每当驱动程序返回 SQL_SUCCESS_WITH_INFO 时，驱动程序管理器都将从 *hDbcInfoToken* 获取诊断信息，并将 SQL_SUCCESS_WITH_INFO 返回到 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 和 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)中的应用程序。  
  
 应用程序不应直接调用此函数。 支持驱动程序感知连接池的 ODBC 驱动程序必须实现此功能。  
  
 包括用于 ODBC 驱动程序开发的 sqlspi。  
  
## <a name="see-also"></a>另请参阅  
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [在 ODBC 驱动程序中开发连接池感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
