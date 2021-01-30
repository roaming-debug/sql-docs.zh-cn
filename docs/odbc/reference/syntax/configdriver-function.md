---
description: ConfigDriver 函数
title: ConfigDriver 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- ConfigDriver
helpviewer_keywords:
- ConfigDriver [ODBC]
ms.assetid: 9473f48f-bcae-4784-89c1-7839bad4ed13
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5abe589b84eff738aeaf41139082781254fb0488
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174733"
---
# <a name="configdriver-function"></a>ConfigDriver 函数
**度**  
 引入的版本： ODBC 2。5  
  
 **摘要**  
 **ConfigDriver** 允许安装程序执行安装和卸载功能，而无需程序调用 **ConfigDSN**。 此函数将执行特定于驱动程序的函数，例如创建驱动程序特定的系统信息并在安装过程中执行 DSN 转换，以及在卸载过程中清除系统信息修改。 此函数由驱动程序安装程序 DLL 或单独的安装程序 DLL 公开。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL ConfigDriver(  
      HWND    hwndParent,  
      WORD    fRequest,  
      LPCSTR  lpszDriver,  
      LPCSTR  lpszArgs,  
      LPSTR   lpszMsg,  
      WORD    cbMsgMax,  
      WORD *  pcbMsgOut);  
```  
  
## <a name="arguments"></a>参数  
 *hwndParent*  
 送父窗口句柄。 如果句柄为 null，则该函数将不会显示任何对话框。  
  
 *fRequest*  
 送请求的类型。 *FRequest* 参数必须包含以下值之一：  
  
 ODBC_INSTALL_DRIVER：安装新的驱动程序。  
  
 ODBC_REMOVE_DRIVER：删除驱动程序。  
  
 此选项也可以是特定于驱动程序的，在这种情况下，第一个选项的 *fRequest* 参数必须从 ODBC_CONFIG_DRIVER_MAX + 1 开始。 任何附加选项的 *fRequest* 参数也必须以大于 ODBC_CONFIG_DRIVER_MAX + 1 的值开头。  
  
 *lpszDriver*  
 送在系统信息的 Odbcinst.ini 密钥中注册的驱动程序的名称。  
  
 *lpszArgs*  
 送一个以 null 结尾的字符串，其中包含特定于驱动程序的 *fRequest* 的参数。  
  
 *lpszMsg*  
 输出一个以 null 结尾的字符串，其中包含驱动程序安装程序的输出消息。  
  
 *cbMsgMax*  
 送 *LpszMsg* 的长度。  
  
 *pcbMsgOut*  
 输出可在 *lpszMsg* 中返回的总字节数。  
  
 如果可返回的字节数大于或等于 *cbMsgMax*，则 *lpszMsg* 中的输出消息将被截断为 *cbMsgMax* 减 null 终止字符。 *PcbMsgOut* 参数可以为 null 指针。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **ConfigDriver** 返回 FALSE 时，将通过调用 **SQLPostInstallerError** 将关联的 *\* pfErrorCode* 值发布到安装程序错误缓冲区，并可通过调用 **SQLInstallerError** 获取该值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_HWND|无效的窗口句柄|*HwndParent* 参数无效。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*FRequest* 参数不是以下项之一：<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> 特定于驱动程序的选项小于或等于 ODBC_CONFIG_DRIVER_MAX。|  
|ODBC_ERROR_INVALID_NAME|无效的驱动程序或转换器名称|*LpszDriver* 参数无效。 在注册表中找不到它。|  
|ODBC_ERROR_REQUEST_FAILED|*请求* 失败|无法执行 *fRequest* 参数请求的操作。|  
|ODBC_ERROR_DRIVER_SPECIFIC|驱动程序或转换器特定的错误|未定义 ODBC 安装程序错误的特定于驱动程序的错误。 对 **SQLPostInstallerError** 函数的调用中的 *SzError* 参数应包含特定于驱动程序的错误消息。|  
  
## <a name="comments"></a>注释  
  
### <a name="driver-specific-options"></a>Driver-Specific 选项  
 应用程序可以使用 *fRequest* 参数请求驱动程序公开的特定于驱动程序的功能。 第一个选项的 *fRequest* 将 ODBC_CONFIG_DRIVER_MAX 加1，其他选项将从该值递增1。 此函数的驱动程序所需的任何参数都应在 *lpszArgs* 参数中传递的以 null 结尾的字符串中提供。 提供此类功能的驱动程序应维护特定于驱动程序选项的表。 驱动程序文档中应完整记录这些选项。 使用特定于驱动程序的选项的应用程序编写人员应该知道这会使应用程序的互操作性更低。  
  
### <a name="messages"></a>消息  
 驱动程序安装例程可以将文本消息作为以 null 结尾的字符串发送到 *lpszMsg* 缓冲区。 如果 **ConfigDriver** 函数大于或等于 *cbMsgMax* 字符，则该消息将被截断为 *cbMsgMax* 减去 null 终止字符。
