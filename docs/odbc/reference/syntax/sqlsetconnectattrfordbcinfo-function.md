---
description: SQLSetConnectAttrForDbcInfo 函数
title: SQLSetConnectAttrForDbcInfo 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectAttrForDbcInfo function [ODBC]
ms.assetid: a28fadb9-b998-472a-b252-709507e92005
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2ca64e1309c939f4b39b8b9a020a1debb1cc10b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192434"
---
# <a name="sqlsetconnectattrfordbcinfo-function"></a>SQLSetConnectAttrForDbcInfo 函数
**度**  
 引入的版本： ODBC 3.81 标准符合性： ODBC  
  
 **摘要**  
 **SQLSetConnectAttrForDbcInfo** 与 **SQLSetConnectAttr** 相同，但它在连接信息标记而不是连接句柄上设置属性。  
  
## <a name="syntax"></a>语法  
  
```cpp
  
SQLRETURN  SQLSetConnectAttrForDbcInfo(  
                SQLHDBC_INFO_TOKEN    hDbcInfoToken,  
                SQLINTEGER            Attribute,  
                SQLPOINTER            ValuePtr,  
                SQLINTEGER            StringLength );  
```  
  
## <a name="arguments"></a>参数  
 *hDbcInfoToken*  
 送标记句柄。  
  
 *Attribute*  
 送要设置的特性。 有效属性的列表是特定于驱动程序的，与 [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)的是相同的。  
  
 *ValuePtr*  
 送指向要与 *属性* 关联的值的指针。 根据 *属性* 的值， *将 valueptr* 将为32位无符号整数值，或者将指向以 null 值结束的字符串。 请注意，如果 *属性* 参数是驱动程序特定的值，则 *将 valueptr* 中的值可能是带符号整数。  
  
 *StringLength*  
 送如果 *attribute* 是 ODBC 定义的属性，并且 *将 valueptr* 指向字符串或二进制缓冲区，则此参数应为 **将 valueptr* 的长度。 对于字符串数据，此参数应包含字符串中的字节数。  
  
 如果 *attribute* 是一个 ODBC 定义的属性，并且 *将 valueptr* 是一个整数，则将忽略 *StringLength* 。  
  
 如果 *attribute* 是驱动程序定义的属性，则应用程序通过设置 *StringLength* 参数来指示属性对驱动程序管理器的性质。 *StringLength* 可具有以下值：  
  
-   如果 *将 valueptr* 是指向字符串的指针，则 *StringLength* 是字符串或 SQL_NTS 的长度。  
  
-   如果 *将 valueptr* 是一个指向二进制缓冲区的指针，则该应用程序会将 SQL_LEN_BINARY_ATTR 的结果 (*长度*) 宏置于 *StringLength* 中。 这会在 *StringLength* 中置入负值。  
  
-   如果 *将 valueptr* 是一个指向字符串或二进制字符串以外的值的指针，则 *StringLength* 的值应 SQL_IS_POINTER。  
  
-   如果 *将 valueptr* 包含固定长度的值，则 *StringLength* 根据需要 SQL_IS_INTEGER 或 SQL_IS_UINTEGER。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 与 [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)相同，不同之处在于，驱动程序管理器将使用 SQL_HANDLE_DBC_INFO_TOKEN 的 **HandleType** 和 *hDbcInfoToken* 的 **句柄**。  
  
## <a name="remarks"></a>备注  
 **SQLSetConnectAttrForDbcInfo** 与 **SQLSetConnectAttr** 相同，但它在连接信息令牌上设置属性，而不是在连接句柄上设置属性。 例如，如果 **SQLSetConnectAttr** 不能识别属性， **SQLSetConnectAttrForDbcInfo** 还应返回该属性 SQL_ERROR。  
  
 当驱动程序返回 SQL_ERROR 或 SQL_INVALID_HANDLE 时，驱动程序应忽略此属性以计算池 ID。 同时，驱动程序管理器将从 *hDbcInfoToken* 获取诊断信息，并将 SQL_SUCCESS_WITH_INFO 返回到 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md) 和 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)中的应用程序。 因此，应用程序可以检索有关无法设置某些属性的详细信息。  
  
 应用程序不应直接调用此函数。 支持驱动程序感知连接池的 ODBC 驱动程序必须实现此功能。  
  
 包括用于 ODBC 驱动程序开发的 sqlspi。  
  
## <a name="see-also"></a>另请参阅  
 [开发 ODBC 驱动程序](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [识别驱动程序的连接池](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [在 ODBC 驱动程序中开发连接池感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
