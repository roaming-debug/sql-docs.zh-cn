---
description: SQLWriteFileDSN 函数
title: SQLWriteFileDSN 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWriteFileDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWriteFileDSN
helpviewer_keywords:
- SQLWriteFileDSN [ODBC]
ms.assetid: 9e18f56f-1061-416b-83d4-ffeec42ab5a9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f778c2669a55e3f63c1946d2b4323ae64242c533
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191671"
---
# <a name="sqlwritefiledsn-function"></a>SQLWriteFileDSN 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLWriteFileDSN** 将信息写入文件 DSN。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLWriteFileDSN(  
     LPCSTR     lpszFileName,  
     LPCSTR     lpszAppName,  
     LPCSTR     lpszKeyName,  
     LPCSTR     lpszString);  
```  
  
## <a name="arguments"></a>参数  
 *lpszFileName*  
 送指向文件 DSN 名称的指针。 DSN 扩展会附加到尚未包含 DSN 扩展的所有文件名。  
  
 *lpszAppName*  
 送指向应用程序名称的指针。 这是 ODBC 部分的 "ODBC"。  
  
 *lpszKeyName*  
 送一个指针，指向要读取的键的名称。 有关保留关键字，请参阅 "注释"。  
  
 *lpszString*  
 输出指向与要写入的密钥关联的字符串。 此参数指向的字符串的最大长度为32767个字节。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLWriteFileDSN** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_PATH|安装路径无效|在 *lpszFileName* 参数中指定的文件名的路径无效。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*LpszAppName*、 *lpszKeyName* 或 *lpszString* 参数为 NULL。|  
  
## <a name="comments"></a>注释  
 ODBC 保留用于存储连接信息的 "节名称 [ODBC]"。 本部分的保留关键字与为 **SQLDriverConnect** 中的连接字符串预留的关键字相同。  (有关详细信息，请参阅 [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md) 函数说明。 )   
  
 应用程序可以使用这些保留关键字将信息直接写入文件 DSN。 如果应用程序需要创建或修改与文件 DSN 关联的无 DSN 连接字符串，则它可以为 [ODBC] 部分中的任何保留连接字符串关键字调用 **SQLWriteFileDSN** 。  
  
 如果 *lpszString* 参数为 null 指针，则将从 .dsn 文件中删除 *lpszKeyName* 参数指向的关键字。 如果 *lpszString* 和 *lpszKeyName* 参数均为 null 指针，则将从 .Dsn 文件中删除 *lpszAppName* 参数指向的部分。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|读取文件 Dsn 中的信息|[SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)|
