---
description: SQLPoolConnect 函数
title: SQLPoolConnect 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLPoolConnect function [ODBC]
ms.assetid: 41322737-890d-4a81-aed2-06cc3d546962
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8317e159a5a438bb22e8077f3a6635a79f9baefd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204562"
---
# <a name="sqlpoolconnect-function"></a>SQLPoolConnect 函数
**度**  
 引入的版本： ODBC 3.8 标准符合性： ODBC  
  
 **摘要**  
 如果不能重复使用池中的连接， **SQLPoolConnect** 将用于创建新连接。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN  SQLPoolConnect(  
                SQLHDBC              hDbc,  
                SQLHDBC_INFO_TOKEN   hDbcInfoToken,  
                WCHAR *              wszOutConnectString,  
                SQLSMALLINT          cchConnectStringBuffer,  
                SQLSMALLINT *        cchConnectStringLen );  
```  
  
## <a name="arguments"></a>参数  
 *hDbc*  
 送连接句柄。  
  
 *hDbcInfoToken*  
 送新应用程序连接请求的标记句柄。  
  
 *wszOutConnectString*  
 输出指向已完成连接字符串的缓冲区的指针。 成功连接到目标数据源后，此缓冲区包含完整的连接字符串。 应用程序应为此缓冲区分配至少1024个字符。  
  
 如果 *wszOutConnectString* 为 NULL，则 *cchConnectStringLen* 仍返回 (字符数据的 null 终止字符以外的字符总数) 可在 *wszOutConnectString* 所指向的缓冲区中返回。  
  
 *cchConnectStringBuffer*  
 送**WszOutConnectString* 缓冲区的长度（以字符为限）。  
  
 *cchConnectStringLen*  
 输出指向缓冲区的指针，将在此缓冲区中返回 (不包括 null 终止字符) 可在 wszOutConnectString 中返回的字符总数 \* 。 如果可返回的字符数大于或等于 *cchConnectStringBuffer*，则 wszOutConnectString 中已完成的连接字符串将 \* 截断为 *cchConnectStringBuffer* 减去 null 终止字符的长度。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 类似于 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)的任何输入验证错误，只不过驱动程序管理器将使用 SQL_HANDLE_DBC_INFO_TOKEN 的 **HandleType** 和 *hDbcInfoToken* 的 **句柄**。  
  
## <a name="remarks"></a>备注  
 驱动程序管理器保证 *hDbc* 和 *HDBCINFOTOKEN* 的父 HENV 句柄相同。  
  
 与 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)不同，不存在用于提示用户输入连接信息的 *DriverCompletion* 参数。 在共用情况下，不允许出现提示对话框。  
  
 应用程序不应直接调用此函数。 支持驱动程序感知连接池的 ODBC 驱动程序必须实现此功能。  
  
 每当驱动程序返回 SQL_ERROR 或 SQL_INVALID_HANDLE 时，驱动程序管理器会将错误返回到应用程序 (的 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 或 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)) 。  
  
 每当驱动程序返回 SQL_SUCCESS_WITH_INFO 时，驱动程序管理器都将从 *hDbcInfoToken* 获取诊断信息，并将 SQL_SUCCESS_WITH_INFO 返回到 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 和 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)中的应用程序。  
  
 当应用程序使用 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)时， *WSZOUTCONNECTSTRING* 将为 null 缓冲区 (最后三个参数将全部设置为 NULL、0、null) 。 否则，驱动程序必须返回输出连接字符串，它将返回到应用程序的 [SQLDriverConnect 函数](../../../odbc/reference/syntax/sqldriverconnect-function.md) 调用中。  
  
 包括用于 ODBC 驱动程序开发的 sqlspi。  
  
## <a name="see-also"></a>另请参阅  
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [在 ODBC 驱动程序中开发连接池感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
