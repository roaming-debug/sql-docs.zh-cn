---
description: SQLWritePrivateProfileString 函数
title: SQLWritePrivateProfileString 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLWritePrivateProfileString
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLWritePrivateProfileString
helpviewer_keywords:
- SQLWritePrivateProfileString [ODBC]
ms.assetid: 526f36a4-92ed-4874-9725-82d27c0b86f9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f001e3a4ad2b6e3e4aad3f34e8a87be1c7b2b12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191648"
---
# <a name="sqlwriteprivateprofilestring-function"></a>SQLWritePrivateProfileString 函数
**度**  
 引入的版本： ODBC 2。0  
  
 **摘要**  
 **SQLWritePrivateProfileString** 将值名称和数据写入系统信息的 Odbc.ini 子项中。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLWritePrivateProfileString(  
     LPCSTR     lpszSection,  
     LPCSTR     lpszEntry,  
     LPCSTR     lpszString,  
     LPCSTR     lpszFilename);  
```  
  
## <a name="arguments"></a>参数  
 *lpszSection*  
 送指向以 null 结尾的字符串，该字符串包含要将字符串复制到其中的节的名称。 如果该节不存在，则创建它。 节的名称独立于大小写;字符串可以是任何大写和小写字母的组合。  
  
 *lpszEntry*  
 送指向以 null 结尾的字符串，该字符串包含要与字符串关联的密钥的名称。 如果该键不存在于指定的节中，则会创建它。 如果此参数为 NULL，则删除整个部分（包括节中的所有条目）。  
  
 *lpszString*  
 送指向要写入文件的以 null 结尾的字符串。 如果此参数为 NULL，则将删除 *lpszEntry* 参数指向的键。  
  
 *lpszFilename*  
 输出指向以 null 结尾的字符串，该字符串命名初始化文件。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLWritePrivateProfileString** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_REQUEST_FAILED|申请失败。|无法写入请求的系统信息。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLWritePrivateProfileString** 是一种简单的方法，可将来自 Microsoft® Windows®的驱动程序和驱动程序安装 dll 移植到 MICROSOFT windows NT®/Windows 2000。 应将对 **WritePrivateProfileString** 的调用写入到 Odbc.ini 文件中，并将其替换为对 **SQLWritePrivateProfileString** 的调用。 **SQLWritePrivateProfileString** 调用 WIN32® API 中的函数，将指定的值名称和数据添加到系统信息的 Odbc.ini 子项中。  
  
 配置模式指示 Odbc.ini 条目列出 DSN 值在系统信息中的位置。 如果 DSN 是用户 DSN (状态变量 USERDSN_ONLY) ，则该函数将写入 HKEY_CURRENT_USER 中的 Odbc.ini 条目。 如果 DSN 是系统 DSN (SYSTEMDSN_ONLY) ，则该函数将写入 HKEY_LOCAL_MACHINE 中的 Odbc.ini 条目。 如果状态变量为 BOTHDSN，则尝试 HKEY_CURRENT_USER，如果失败，则使用 HKEY_LOCAL_MACHINE。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|从系统信息中获取值|[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|
