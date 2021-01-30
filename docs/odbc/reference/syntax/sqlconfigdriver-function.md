---
description: SQLConfigDriver 函数
title: SQLConfigDriver 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDriver
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDriver
helpviewer_keywords:
- SQLConfigDriver function [ODBC]
ms.assetid: 4f681961-ac9f-4d88-b065-5258ba112642
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: abe28e8d870216cf090e57a93e0c75766db08231
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165331"
---
# <a name="sqlconfigdriver-function"></a>SQLConfigDriver 函数
**度**  
 引入的版本： ODBC 2。5  
  
 **摘要**  
 **SQLConfigDriver** 加载适当的驱动程序安装程序 DLL 并调用 **ConfigDriver** 函数。  
  
 还可以 [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md)访问 **SQLConfigDriver** 功能。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLConfigDriver(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszArgs,  
     LPSTR    lpszMsg,  
     WORD     cbMsgMax,  
     WORD *   pcbMsgOut);  
```  
  
## <a name="arguments"></a>参数  
 *hwndParent*  
 送父窗口句柄。 如果句柄为 null，则该函数将不会显示任何对话框。  
  
 *fRequest*  
 送请求的类型。 *fRequest* 必须包含下列值之一：  
  
 ODBC_CONFIG_DRIVER：更改驱动程序使用的连接池超时值。  
  
 ODBC_INSTALL_DRIVER：安装新的驱动程序。  
  
 ODBC_REMOVE_DRIVER：删除现有的驱动程序。  
  
 此选项也可以是特定于驱动程序的，在这种情况下，第一个选项的 *fRequest* 必须从 ODBC_CONFIG_DRIVER_MAX + 1 开始。 任何附加选项的 *fRequest* 还必须以大于 ODBC_CONFIG_DRIVER_MAX + 1 的值开头。  
  
 *lpszDriver*  
 送在系统信息中注册的驱动程序的名称。  
  
 *lpszArgs*  
 送一个以 null 结尾的字符串，其中包含特定于驱动程序的 *fRequest* 的参数。  
  
 *lpszMsg*  
 输出一个以 null 结尾的字符串，其中包含驱动程序安装程序的输出消息。  
  
 *cbMsgMax*  
 送LpszMsg 的长度 *。*  
  
 *pcbMsgOut*  
 输出可在 *lpszMsg* 中返回的总字节数。 如果可返回的字节数大于或等于 *cbMsgMax*，则 *lpszMsg* 中的输出消息将被截断为 *cbMsgMax* 减 null 终止字符。 *PcbMsgOut* 参数可以为 null 指针。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLConfigDriver** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_BUFF_LEN|缓冲区长度无效|*LpszMsg* 参数无效。|  
|ODBC_ERROR_INVALID_HWND|无效的窗口句柄|*HwndParent* 参数无效。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*FRequest* 参数不是以下项之一：<br /><br /> ODBC_INSTALL_DRIVER ODBC_REMOVE_DRIVER<br /><br /> *FRequest* 参数是一个小于或等于 ODBC_CONFIG_DRIVER_MAX 的特定于驱动程序的选项。|  
|ODBC_ERROR_INVALID_NAME|无效的驱动程序或转换器名称|*LpszDriver* 参数无效。 在注册表中找不到它。|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|关键字-值对无效|*LpszArgs* 参数包含语法错误。|  
|ODBC_ERROR_REQUEST_FAILED|*请求* 失败|安装程序无法执行 *fRequest* 参数请求的操作。 对 **ConfigDriver** 的调用失败。|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|无法加载驱动程序或转换器安装程序库|无法加载驱动程序安装库。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLConfigDriver** 允许应用程序调用驱动程序的 **ConfigDriver** 例程，无需知道名称并加载特定于驱动程序的安装 DLL。 安装程序会在安装了驱动程序安装程序 DLL 之后调用此函数。 调用程序应注意此函数可能不适用于所有驱动程序。 在这种情况下，调用程序应继续运行而不会出错。  
  
## <a name="driver-specific-options"></a>Driver-Specific 选项  
 应用程序可以使用 *fRequest* 参数请求驱动程序公开的特定于驱动程序的功能。 第一个选项的 *fRequest* 将是 ODBC_CONFIG_DRIVER_MAX + 1，其他选项将从该值递增1。 此函数的驱动程序所需的任何参数都应在 *lpszArgs* 参数中传递的以 null 结尾的字符串中提供。 提供此类功能的驱动程序应维护特定于驱动程序选项的表。 驱动程序文档中应完整记录这些选项。 使用特定于驱动程序的选项的应用程序编写人员应该知道这种用法会使应用程序的互操作性更低。  
  
## <a name="setting-connection-pooling-timeout"></a>设置连接池超时  
 设置驱动程序的配置时，可以设置 "连接池超时" 属性。 使用 ODBC_CONFIG_DRIVER 的 *fRequest* 调用 **SQLConfigDriver** ，并将 *lpszArgs* 设置为 **CPTimeout**。 **CPTimeout** 确定连接可以在连接池中保留的时间段（不使用）。 当超时到期时，连接将关闭并从池中删除。 默认超时值为60秒。  
  
 当调用 **SQLConfigDriver** 并将 *fRequest* 设置为 ODBC_INSTALL_DRIVER 或 ODBC_REMOVE_DRIVER 时，驱动程序管理器将加载相应的驱动程序安装程序 DLL 并调用 **ConfigDriver** 函数。 当使用 ODBC_CONFIG_DRIVER 的 *fRequest* 调用 **SQLConfigDriver** 时，将在 ODBC 安装程序中执行所有处理，因此无需加载驱动程序安装 DLL。  
  
## <a name="messages"></a>消息  
 驱动程序安装例程可以将文本消息作为以 null 结尾的字符串发送到 *lpszMsg* 缓冲区。 如果 **ConfigDriver** 函数大于或等于 *cbMsgMax* 字符，则该消息将被截断为 *cbMsgMax* 减去 null 终止字符。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|添加、修改或删除驱动程序|安装程序 DLL 中的[ConfigDriver](../../../odbc/reference/syntax/configdriver-function.md) () |  
|删除默认数据源|[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|
