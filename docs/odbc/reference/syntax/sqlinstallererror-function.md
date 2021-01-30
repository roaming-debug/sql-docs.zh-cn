---
description: SQLInstallerError 函数
title: SQLInstallerError 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLInstallerError
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLInstallerError
helpviewer_keywords:
- SQLInstallerError [ODBC]
ms.assetid: e6474b79-4d55-458f-81ce-abfafe357f83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: dcafadc902504ddfd32fa8b7369780bd5c296a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189676"
---
# <a name="sqlinstallererror-function"></a>SQLInstallerError 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLInstallerError** 返回 ODBC 安装程序函数的错误或状态信息。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
RETCODE SQLInstallerError(  
     WORD      iError,  
     DWORD *   pfErrorCode,  
     LPSTR     lpszErrorMsg,  
     WORD      cbErrorMsgMax,  
     WORD *    pcbErrorMsg);  
```  
  
## <a name="arguments"></a>参数  
 *iError*  
 送错误记录号。 有效数字为1到8。  
  
 *pfErrorCode*  
 输出安装程序错误代码。  (有关详细信息，请参阅 "注释"。)   
  
 *lpszErrorMsg*  
 输出指向错误消息文本的存储区的指针。  
  
 *cbErrorMsgMax*  
 送 *SzErrorMsg* 缓冲区的最大长度。 该值必须小于或等于 SQL_MAX_MESSAGE_LENGTH 减去 null 终止字符。  
  
 *cbErrorMsgMax*  
 送 *SzErrorMsg* 缓冲区的最大长度。 该值必须小于或等于 SQL_MAX_MESSAGE_LENGTH 减去 null 终止字符。  
  
 *pcbErrorMsg*  
 输出一个指针，它指向 (排除 null 终止字符) 可在 *lpszErrorMsg* 中返回的总字节数。 如果可返回的字节数大于或等于 *cbErrorMsgMax*，则 *lpszErrorMsg* 中的错误消息文本将截断为 *cbErrorMsgMax* 减去 null 终止字符字节。 *PcbErrorMsg* 参数可以为 null 指针。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA 或 SQL_ERROR。  
  
## <a name="diagnostics"></a>诊断  
 **SQLInstallerError** 不会为自身发布错误值。 **SQLInstallerError** 在无法检索任何错误信息时返回 SQL_NO_DATA (在这种情况下， *pfErrorCode* 未定义) 。 如果 **SQLInstallerError** 无法访问通常返回 SQL_ERROR 的错误值，则 **SQLInstallerError** 将返回 SQL_ERROR，但不会发布任何错误值。 如果您不知道 (*lpszErrorMsg*) 的警告字符串的长度，则可以将 *LPSZERRORMSG* 设置为 NULL 并调用 **SQLInstallerError**。 然后， **SQLInstallerError** 将返回 *cbErrorMsgMax* 中警告字符串的长度。 如果错误消息的缓冲区太短，则 **SQLInstallerError** 将返回 SQL_SUCCESS_WITH_INFO，并返回 **SQLInstallerError** 的正确 *pfErrorCode* 值。  
  
 若要确定错误消息中是否发生了截断，应用程序可以将 *cbErrorMsgMax* 参数中的值与写入 *pcbErrorMsg* 参数的消息文本的实际长度进行比较。 如果发生截断，应为 *lpszErrorMsg* 分配正确的缓冲区长度，并且应再次通过相应的 *IError* 记录调用 **SQLInstallerError** 。  
  
## <a name="comments"></a>注释  
 如果对 ODBC 安装程序函数的上一个调用返回 FALSE，则应用程序会调用 **SQLInstallerError** 。 仅当函数失败时，ODBC 安装程序和驱动程序或转换器设置函数才会发布零个或多个错误 (将返回 FALSE) ;因此，只有在 ODBC 安装程序函数失败之后，应用程序才会调用 **SQLInstallerError** 。  
  
 每次调用新的安装程序函数时，都会刷新 ODBC 安装程序错误队列。 因此，应用程序无法从上一个安装程序函数调用中检索除之外的函数的错误。  
  
 若要为函数调用检索多个错误，应用程序将多次调用 **SQLInstallerError** 。  
  
 当没有其他信息时， **SQLInstallerError** 返回 SQL_NO_DATA， *pfErrorCode* 参数未定义， *pcbErrorMsg* 参数等于0，并且 *lpszErrorMsg* 参数包含一个 null 终止字符 (，除非 *cbErrorMsgMax* 参数等于 0) 。
