---
description: SQLGetPrivateProfileString 函数
title: SQLGetPrivateProfileString 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetPrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetPrivateProfileString
helpviewer_keywords:
- SQLGetPrivateProfileString function [ODBC]
ms.assetid: b72ca065-4d67-48df-baac-e18379a8320a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6ec5dfa03f04b004f704ad707fb7916d40e69716
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186195"
---
# <a name="sqlgetprivateprofilestring-function"></a>SQLGetPrivateProfileString 函数
**度**  
 引入的版本： ODBC 2。0  
  
 **摘要**  
 **SQLGetPrivateProfileString** 获取与系统信息的值相对应的值或数据的名称列表。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
int SQLGetPrivateProfileString(  
     LPCSTR   lpszSection,  
     LPCSTR   lpszEntry,  
     LPCSTR   lpszDefault,  
     LPCSTR   RetBuffer,  
     INT      cbRetBuffer,  
     LPCSTR   lpszFilename);  
```  
  
## <a name="arguments"></a>参数  
 *lpszSection*  
 送指向以 null 结尾的字符串，该字符串指定包含密钥名称的部分。 如果此参数为 NULL，则该函数将文件中的所有节名称复制到提供的缓冲区中。  
  
 *lpszEntry*  
 送指向以 null 结尾的字符串，该字符串包含要检索其关联字符串的键名称。 如果此参数为 NULL，则 *lpszSection* 参数指定的部分中的所有键名称将复制到 *RetBuffer* 参数指定的缓冲区中。  
  
 *lpszDefault*  
 送指向以 null 结尾的字符串，如果在初始化文件中找不到该键，则为指定键指定默认值。 此参数不能为 NULL。  
  
 *RetBuffer*  
 输出指向接收检索到的字符串的缓冲区。  
  
 *cbRetBuffer*  
 送指定由 *RetBuffer* 参数指向的缓冲区的大小（以字符为字符）。  
  
 *lpszFilename*  
 送指向以 null 结尾的字符串，该字符串命名初始化文件。 如果此参数不包含文件的完整路径，则会搜索默认目录。  
  
## <a name="returns"></a>返回  
 **SQLGetPrivateProfileString** 返回一个整数值，该值指示读取的字符数。  
  
## <a name="diagnostics"></a>诊断  
 调用 **SQLGetPrivateProfileString** 失败时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLGetPrivateProfileString** 是一种简单的方法，可将来自 Microsoft® Windows®的驱动程序和驱动程序安装 dll 移植到 MICROSOFT windows NT®/Windows 2000。 对于从 Odbc.ini 文件检索配置文件字符串的 **调用，应** 将调用替换为对 **SQLGetPrivateProfileString** 的调用。 **SQLGetPrivateProfileString** 调用 WIN32® API 中的函数来检索所请求的值或数据的名称，这些名称与系统信息的 Odbc.ini 子项的值相对应。  
  
 **SQLSetConfigMode**) 设置的配置模式 (指示列出 DSN 值的 Odbc.ini 条目在系统信息中的位置。 如果 DSN 是用户 DSN (配置模式 USERDSN_ONLY) ，则该函数从 HKEY_CURRENT_USER 中的 Odbc.ini 项进行读取。 如果 DSN 是系统 DSN (SYSTEMDSN_ONLY) ，则该函数将从 HKEY_LOCAL_MACHINE 中的 Odbc.ini 条目读取。 如果配置模式为 BOTHDSN，则尝试 HKEY_CURRENT_USER，如果失败，则使用 HKEY_LOCAL_MACHINE。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|向系统信息写入值|[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|
