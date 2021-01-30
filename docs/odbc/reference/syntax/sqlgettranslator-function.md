---
description: SQLGetTranslator 函数
title: SQLGetTranslator 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTranslator
helpviewer_keywords:
- SQLGetTranslator function [ODBC]
ms.assetid: 33879db3-5ef9-4585-9be5-69376157e017
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5ec960312c652c625a6fb27388bd9c21a09f3206
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189727"
---
# <a name="sqlgettranslator-function"></a>SQLGetTranslator 函数
**度**  
 引入的版本： ODBC 2。0  
  
 **摘要**  
 **SQLGetTranslator** 显示一个对话框，用户可以从中选择转换器。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLGetTranslator(  
     HWND      hwndParent,  
     LPSTR     lpszName,  
     WORD      cbNameMax,  
     WORD *    pcbNameOut,  
     LPSTR     lpszPath,  
     WORD      cbPathMax,  
     WORD *    pcbPathOut,  
     DWORD *   pvOption);  
```  
  
## <a name="arguments"></a>参数  
 *hwndParent*  
 送父窗口句柄。  
  
 *lpszName*  
 [输入/输出]系统信息中的转换器的名称。  
  
 *cbNameMax*  
 送 *LpszName* 缓冲区的最大长度。  
  
 *pcbNameOut*  
 [输入/输出]在 *lpszName* 中) 通过或返回的 null 终止字节 (排除的总字节数。 如果可返回的字节数大于或等于 *cbNameMax*，则 *lpszName* 中的转换器名称将被截断为 *cbNameMax* 减 null 终止字符。 *PcbNameOut* 参数可以为 null 指针。  
  
 *lpszPath*  
 输出翻译 DLL 的完整路径。  
  
 *cbPathMax*  
 送 *LpszPath* 缓冲区的最大长度。  
  
 *pcbPathOut*  
 输出 (排除在 *lpszPath* 中返回的 null 终止字节) 的总字节数。 如果可返回的字节数大于或等于 *cbPathMax*，则 *lpszPath* 中的转换 DLL 路径将截断为 *cbPathMax* 减 null 终止字符。 *PcbPathOut* 参数可以为 null 指针。  
  
 *pvOption*  
 [输出] 32 位转换选项。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE; 如果失败，则返回 FALSE; 如果用户取消对话框，则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetTranslator** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_BUFF_LEN|缓冲区长度无效|*CbNameMax* 或 *cbPathMax* 参数小于或等于0。|  
|ODBC_ERROR_INVALID_HWND|无效的窗口句柄|*HwndParent* 参数无效或为 NULL。|  
|ODBC_ERROR_INVALID_NAME|无效的驱动程序或转换器名称|*LpszName* 参数无效。 在注册表中找不到它。|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|无法加载驱动程序或转换器安装程序库|无法加载转换器库。|  
|ODBC_ERROR_INVALID_OPTION|Transaction 选项无效|*PvOption* 参数包含无效的值。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 如果 *hwndParent* 为 null，或者 *lpszName*、 *lpszPath* 或 *pvOption* 为 null 指针，则 **SQLGetTranslator** 返回 FALSE。 否则，它将在以下对话框中显示已安装的翻译人员的列表。  
  
 ![“选择转换器”对话框](../../../odbc/reference/syntax/media/ch23j.gif "CH23J")  
  
 如果 *lpszName* 包含有效的转换器名称，则将其选中。 否则 \<No Translator> 为选中状态。  
  
 如果用户选择此 \<No Translator> 内容，则不会接触到 *lpszName*、 *lpszPath* 和 *pvOption* 的内容。 **SQLGetTranslator** 将 *pcbNameOut* 和 *pcbPathOut* 设置为0并返回 TRUE。  
  
 如果用户选择转换器，则 **SQLGetTranslator** 会在转换器的安装程序 DLL 中调用 **ConfigTranslator** 。 如果 **ConfigTranslator** 返回 FALSE，则 **SQLGetTranslator** 将返回到其对话框。 如果 **ConfigTranslator** 返回 true，则 **SQLGETTRANSLATOR** 将返回 true，同时返回所选的转换器名称、路径和转换选项。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|配置转换器|[ConfigTranslator](../../../odbc/reference/syntax/configtranslator-function.md)|  
|获取翻译特性|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|设置转换属性|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
