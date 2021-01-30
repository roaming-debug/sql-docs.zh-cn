---
description: SQLSetConfigMode 函数
title: SQLSetConfigMode 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConfigMode
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConfigMode
helpviewer_keywords:
- SQLSetConfigMode function [ODBC]
ms.assetid: 09eb88ea-b6f6-4eca-b19d-0951cebc6c0a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a136a476b56f108bef77395f0ee4b13ae23b10a0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192462"
---
# <a name="sqlsetconfigmode-function"></a>SQLSetConfigMode 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLSetConfigMode** 设置配置模式，用于指示 Odbc.ini 条目列出 DSN 值在系统信息中的位置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLSetConfigMode(  
     UWORD     wConfigMode);  
```  
  
## <a name="arguments"></a>参数  
 *wConfigMode*  
 送安装程序配置模式 (参阅 "注释" ) 。 *WConfigMode* 中的值可以是：  
  
 ODBC_USER_DSN  
  
 ODBC_SYSTEM_DSN  
  
 ODBC_BOTH_DSN  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLSetConfigMode** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_INVALID_PARAM_SEQUENCE|参数序列无效|*WConfigMode* 参数不包含 ODBC_USER_DSN、ODBC_SYSTEM_DSN 或 ODBC_BOTH_DSN。|  
  
## <a name="comments"></a>注释  
 此函数用于设置 Odbc.ini 条目列出 DSN 值在系统信息中的位置。 如果 ODBC_USER_DSN *wConfigMode* ，则 Dsn 是用户 dsn，而函数从 HKEY_CURRENT_USER 中的 Odbc.ini 条目读取。 如果 ODBC_SYSTEM_DSN，则 DSN 是系统 DSN，而函数从 HKEY_LOCAL_MACHINE 中的 Odbc.ini 条目读取。 如果 ODBC_BOTH_DSN，则会尝试 HKEY_CURRENT_USER，如果失败，则使用 HKEY_LOCAL_MACHINE。  
  
 此函数不影响 **SQLCreateDataSource** 和 **SQLDriverConnect**。 当驱动程序从注册表中读取数据时，必须设置配置模式，方法是调用 **SQLGetPrivateProfileString** ，或通过调用 **SQLWritePrivateProfileString** 写入注册表。 对 **SQLGetPrivateProfileString** 和 **SQLWritePrivateProfileString** 的调用使用配置模式来了解要对哪个注册表部分进行操作。  
  
> [!CAUTION]  
>  仅在必要时才应调用 **SQLSetConfigMode** ;如果未正确设置模式，ODBC 安装程序可能无法正常运行。  
  
 **SQLSetConfigMode** 会直接修改配置模式。 这与通过调用 **SQLConfigDataSource** 修改配置模式的过程分离。 对 **SQLConfigDataSource** 的调用设置配置模式，以便在修改 DSN 时区分用户和系统 dsn。 在返回之前， **SQLConfigDataSource** 会将配置模式重置为 BOTHDSN。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|创建数据源|[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|  
|使用连接字符串或对话框连接到数据源|[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|检索配置模式|[SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)|
