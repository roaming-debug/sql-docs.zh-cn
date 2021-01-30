---
description: SQLReadFileDSN 函数
title: SQLReadFileDSN 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLReadFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLReadFileDSN
helpviewer_keywords:
- SQLReadFileDSN function [ODBC]
ms.assetid: ead464aa-cdc3-47dd-a0c0-997711205d31
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7a04747258f612a86746642ae11fb06e9b742662
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192541"
---
# <a name="sqlreadfiledsn-function"></a>SQLReadFileDSN 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLReadFileDSN** 读取文件 DSN 中的信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLReadFileDSN(  
     LPCSTR   lpszFileName,  
     LPCSTR   lpszAppName,  
     LPCSTR   lpszKeyName,  
     LPSTR    lpszString,  
     WORD     cbString,  
     WORD *   pcbString);  
```  
  
## <a name="arguments"></a>参数  
 *lpszFileName*  
 送指向包含 .dsn 文件名称的数据缓冲区的指针。 Dsn 扩展将追加到所有不具有 .dsn 扩展名的文件名。 *\* LpszFileName* 中的值必须为以 null 值结束的字符串。  
  
 *lpszAppName*  
 送指向包含应用程序名称的数据缓冲区的指针。 这是 ODBC 部分的 "ODBC"。 *\* LpszAppName* 中的值必须为以 null 值结束的字符串。  
  
 *lpszKeyName*  
 送指向数据缓冲区的指针，该数据缓冲区包含要读取的项的名称。 有关保留关键字，请参阅 "注释"。 *\* LpszAppName* 中的值必须为以 null 值结束的字符串。  
  
 *lpszString*  
 输出指向数据缓冲区的指针，该缓冲区包含与要读取的键关联的字符串。  
  
 如果 *\* lpszFileName* 是有效的 .dsn 文件名，但 *lpszAppName* 参数是 null 指针，并且 *lpszKeyName* 参数为 null 指针，则 *\* lpszString* 包含有效应用程序的列表。 如果 *\* lpszFileName* 是有效的 .dsn 文件名，而 *\* lpszAppName* 是有效的应用程序名称，但 *lpszKeyName* 参数为 null 指针，则 *\* lpszString* 在 dsn 文件的相应部分中包含有效保留关键字的列表（用分号分隔）。 如果 *\* lpszFileName* 是有效的 .dsn 文件名，但 *\* lpszAppName* 为 null 指针，并且 *lpszKeyName* 参数为 null 指针，则 *\* lpszString* 包含 dsn 文件中的部分列表，用分号分隔。  
  
 *cbString*  
 送 *\* LpszString* 缓冲区的长度。  
  
 *pcbString*  
 输出可在 *\* lpszString* 中返回的总字节数。 如果可返回的字节数大于或等于 *cbString*，则 *\* lpszString* 中的输出字符串将截断为 *cbString* 减 null 终止字符。 *PcbString* 参数可以为 null 指针。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLReadFileDSN** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_BUFF_LEN|缓冲区长度无效|*LpszString* 参数为 NULL。<br /><br /> *CbString* 参数小于或等于0。|  
|ODBC_ERROR_INVALID_PATH|安装路径无效|在 *lpszFileName* 参数中指定的文件名的路径无效。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*LpszAppName* 参数为 NULL，而 *lpszKeyName* 参数是有效的。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
|ODBC_ERROR_OUTPUT_STRING_TRUNCATED|输出字符串已截断|*\* LpszString* 中返回的字符串已截断，因为 *cbString* 中的值小于或等于 *\* pcbString* 中的值。|  
|ODBC_ERROR_REQUEST_FAILED|申请失败。|文件 DSN 中不存在关键字。|  
  
## <a name="comments"></a>注释  
 ODBC 保留用于存储连接信息的 "节名称 [ODBC]"。 本部分的保留关键字与为 **SQLDriverConnect** 中的连接字符串预留的关键字相同。  (有关详细信息，请参阅 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) 函数说明。 )   
  
 应用程序可以使用这些保留关键字来读取文件 DSN 中的信息。 如果应用程序想要找出与文件 DSN 关联的无 DSN 连接字符串，则它可以为 [ODBC] 部分中的任何保留连接字符串关键字调用 **SQLReadFileDSN** 。 传入无 DSN 连接的完整连接字符串是 [ODBC] 部分中 (保留和特定于驱动程序的) 中的所有关键字的组合。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将信息写入文件 DSN|[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|
