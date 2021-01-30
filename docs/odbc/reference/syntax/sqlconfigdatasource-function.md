---
description: SQLConfigDataSource Function
title: SQLConfigDataSource 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConfigDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConfigDataSource
helpviewer_keywords:
- SQLConfigDataSource function [ODBC]
ms.assetid: f8d6e342-c010-434e-b1cd-f5371fb50a14
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ddcfac8f1f211884aa3f46db3f56a2762778ed92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165334"
---
# <a name="sqlconfigdatasource-function"></a>SQLConfigDataSource Function
**度**  
 引入的版本： ODBC 1。0  
  
 **摘要**  
 **SQLConfigDataSource** 添加、修改或删除数据源。  
  
 还可以 [ODBCCONF.EXE](../../../odbc/odbcconf-exe.md)访问 **SQLConfigDataSource** 功能。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLConfigDataSource(  
     HWND     hwndParent,  
     WORD     fRequest,  
     LPCSTR   lpszDriver,  
     LPCSTR   lpszAttributes);  
```  
  
## <a name="arguments"></a>参数  
 *hwndParent*  
 送父窗口句柄。 如果句柄为 null，则该函数将不会显示任何对话框。  
  
 *fRequest*  
 送请求的类型。 *FRequest* 参数必须包含以下值之一：  
  
 ODBC_ADD_DSN：添加新用户数据源。  
  
 ODBC_CONFIG_DSN：配置 (修改现有用户数据源) 。  
  
 ODBC_REMOVE_DSN：删除现有用户数据源。  
  
 ODBC_ADD_SYS_DSN：添加新的系统数据源。  
  
 ODBC_CONFIG_SYS_DSN：修改现有的系统数据源。  
  
 ODBC_REMOVE_SYS_DSN：删除现有的系统数据源。  
  
 ODBC_REMOVE_DEFAULT_DSN：从系统信息中删除默认数据源规范部分。  (它还会从系统信息中的 Odbcinst.ini 条目中删除默认的驱动程序规范部分。 此 *fRequest* 执行与不推荐使用的 **SQLRemoveDefaultDataSource** 函数相同的功能。 ) 指定此选项后，对 **SQLConfigDataSource** 的调用中的所有其他参数应为 null;如果这些值不为 NULL，则将被忽略。  
  
 *lpszDriver*  
 送驱动程序描述 (通常会呈现给用户的关联 DBMS) 名称，而不是物理驱动程序名称。  
  
 *lpszAttributes*  
 送以关键字-值对形式的双向、以 null 结尾的属性列表。 有关详细信息，请参阅 [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md)。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。 如果调用此函数时，系统信息中不存在任何条目，则该函数将返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLConfigDataSource** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_HWND|无效的窗口句柄|*HwndParent* 参数无效或为 NULL。|  
|ODBC_ERROR_INVALID_REQUEST_TYPE|请求的类型无效|*FRequest* 参数不是以下项之一：<br /><br /> ODBC_ADD_DSN ODBC_CONFIG_DSN ODBC_REMOVE_DSN ODBC_ADD_SYS_DSN ODBC_CONFIG_SYS_DSN ODBC_REMOVE_SYS_DSN ODBC_REMOVE_DEFAULT_DSN|  
|ODBC_ERROR_INVALID_NAME|无效的驱动程序或转换器名称|*LpszDriver* 参数无效。 在注册表中找不到它。|  
|ODBC_ERROR_INVALID_KEYWORD_VALUE|关键字-值对无效|*LpszAttributes* 参数包含语法错误。|  
|ODBC_ERROR_REQUEST_FAILED|*请求* 失败|安装程序无法执行 *fRequest* 参数请求的操作。 对 **ConfigDSN** 的调用失败。|  
|ODBC_ERROR_LOAD_LIBRARY_FAILED|无法加载驱动程序或转换器安装程序库|无法加载驱动程序安装库。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLConfigDataSource** 使用 *lpszDriver* 的值从系统信息读取驱动程序的安装程序 DLL 的完整路径。 它将加载 DLL 并调用 **ConfigDSN** ，并将其与传递给它的参数相同。  
  
 如果 **SQLConfigDataSource** 找不到或无法加载安装程序 DLL，或如果用户取消了对话框，则返回 FALSE。 否则，它将返回从 **ConfigDSN** 接收的状态。  
  
 **SQLConfigDataSource** 将系统 dsn *FRequest* 映射到用户 dsn *fRequest* (ODBC_ADD_SYS_DSN ODBC_ADD_DSN，ODBC_CONFIG_SYS_DSN 为 ODBC_CONFIG_DSN，ODBC_REMOVE_SYS_DSN ODBC_REMOVE_DSN) 。 为了区分用户和系统 Dsn， **SQLConfigDataSource** 根据下表设置安装程序配置模式。 在返回之前， **SQLConfigDataSource** 会将配置模式重置为 BOTHDSN。 **ConfigDSN** (由驱动程序实现) 应调用 **SQLWriteDSNToIni** 和 **SQLWritePrivateProfileString** 以支持系统 DSN。 有关详细信息，请参阅 [ConfigDSN 函数](../../../odbc/reference/syntax/configdsn-function.md)。  
  
|*fRequest*|配置模式|  
|----------------|------------------------|  
|ODBC_ADD_DSN|USERDSN_ONLY|  
|ODBC_CONFIG_DSN|USERDSN_ONLY|  
|ODBC_REMOVE_DSN|USERDSN_ONLY|  
|ODBC_ADD_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_CONFIG_SYS_DSN|SYSTEMDSN_ONLY|  
|ODBC_REMOVE_SYS_DSN|SYSTEMDSN_ONLY|  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|添加、修改或删除数据源|安装程序 DLL 中的[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) () |  
|从系统信息中删除数据源名称|[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|  
|向系统信息中添加数据源名称|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
