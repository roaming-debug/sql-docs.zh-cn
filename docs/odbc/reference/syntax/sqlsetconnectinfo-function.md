---
description: SQLSetConnectInfo 函数
title: SQLSetConnectInfo 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectInfo function [ODBC]
ms.assetid: 0782a1c3-c5d1-499b-a8ba-134162db9990
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1930d16605c1b2a02db08560549019afa656ca52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192426"
---
# <a name="sqlsetconnectinfo-function"></a>SQLSetConnectInfo 函数
**度**  
 引入的版本： ODBC 3.81 标准符合性： ODBC  
  
 **摘要**  
 **SQLSetConnectInfo** 用于将数据源、用户 ID 和密码设置为应用程序的 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 调用的连接信息令牌。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN  SQLSetConnectInfo(  
                SQLHDBC_INFO_TOKEN   TokenHandle,  
                WCHAR *              ServerName,  
                SQLSMALLINT          NameLength1,  
                WCHAR *              UserName,  
                SQLSMALLINT          NameLength2,  
                WCHAR *              Authentication,  
                SQLSMALLINT          NameLength3 );  
```  
  
## <a name="arguments"></a>参数  
 *TokenHandle*  
 送标记句柄。  
  
 *ServerName*  
 送数据源名称。 数据可以与程序位于同一台计算机上，也可以位于网络中某个位置的另一台计算机上。 有关应用程序如何选择数据源的信息，请参阅 [选择数据源或驱动程序](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)。  
  
 *NameLength1*  
 送**ServerName* 的长度（字符）。  
  
 *UserName*  
 送用户标识符。  
  
 *NameLength2*  
 送长度 **用户名* （字符）。  
  
 *身份验证*  
 送身份验证字符串通常 (密码) 。  
  
 *NameLength3*  
 送**身份验证* 的长度（字符）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 与 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)输入验证错误相同，不同之处在于，驱动程序管理器将使用的 **HandleType** SQL_HANDLE_DBC_INFO_TOKEN 和 *hDbcInfoToken* 的 **句柄**。  
  
## <a name="remarks"></a>备注  
 每当驱动程序返回 SQL_ERROR 或 SQL_INVALID_HANDLE 时，驱动程序管理器会将错误返回到应用程序 (的 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 或 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)) 。  
  
 每当驱动程序返回 SQL_SUCCESS_WITH_INFO 时，驱动程序管理器都将从 *hDbcInfoToken* 获取诊断信息，并将 SQL_SUCCESS_WITH_INFO 返回到 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 和 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)中的应用程序。  
  
 应用程序不应直接调用此函数。 支持驱动程序感知连接池的 ODBC 驱动程序必须实现此功能。  
  
 包括用于 ODBC 驱动程序开发的 sqlspi。  
  
## <a name="see-also"></a>另请参阅  
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [在 ODBC 驱动程序中开发连接池感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
