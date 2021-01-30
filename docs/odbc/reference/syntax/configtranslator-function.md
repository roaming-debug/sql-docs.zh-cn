---
description: ConfigTranslator 函数
title: ConfigTranslator 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigTranslator
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigTranslator
helpviewer_keywords:
- ConfigTranslator [ODBC]
ms.assetid: 7c22f07e-36de-425b-aa67-e32a84afae92
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d15cbb5e43f8d893d38aaa086f0d6f039e2b2a93
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174705"
---
# <a name="configtranslator-function"></a>ConfigTranslator 函数
**度**  
 引入的版本： ODBC 2。0  
  
 **摘要**  
 **ConfigTranslator** 返回翻译人员的默认翻译选项。 它可以在转换器 DLL 或单独的安装 DLL 中。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL ConfigTranslator(  
     HWND     hwndParent,  
     DWORD *  pvOption);  
```  
  
## <a name="arguments"></a>参数  
 *hwndParent*  
 送父窗口句柄。 如果句柄为 null，则该函数将不会显示任何对话框。  
  
 *pvOption*  
 输出32位转换选项。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **ConfigTranslator** 返回 FALSE 时，将通过调用 **SQLPostInstallerError** 将关联的 *\* pfErrorCode* 值发布到安装程序错误缓冲区，并可通过调用 **SQLInstallerError** 获取该值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|无效的窗口句柄|*HwndParent* 参数无效或为 NULL。|  
|ODBC_ERROR_DRIVER_SPECIFIC|驱动程序或转换器特定的错误|未定义 ODBC 安装程序错误的特定于驱动程序的错误。 对 **SQLPostInstallerError** 函数的调用中的 *SzError* 参数应包含特定于驱动程序的错误消息。|  
|ODBC_ERROR_INVALID_OPTION|转换选项无效|*PvOption* 参数包含无效的值。|  
  
## <a name="comments"></a>注释  
 如果转换器仅支持一个转换选项，则 **ConfigTranslator** 将返回 TRUE，并将 *pvOption* 设置为32位选项。 否则，它会确定要使用的默认转换选项。 **ConfigTranslator** 可以显示一个对话框，用户可以使用该对话框选择默认的翻译选项。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|获取翻译选项|[SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|选择转换器|[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|  
|设置翻译选项|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|
