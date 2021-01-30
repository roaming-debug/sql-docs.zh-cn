---
description: SQLGetConfigMode 函数
title: SQLGetConfigMode 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetConfigMode
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConfigMode
helpviewer_keywords:
- SQLGetConfigMode function [ODBC]
ms.assetid: b96ab3b8-08d5-4fea-9ffe-e03043efbf2d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0ccd703f6fd8c7b80dec575d85e44f2c955ea669
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203472"
---
# <a name="sqlgetconfigmode-function"></a>SQLGetConfigMode 函数
**度**  
 引入的版本： ODBC 3。0  
  
 **摘要**  
 **SQLGetConfigMode** 检索配置模式，该配置模式指示 Odbc.ini 条目列出 DSN 值在系统信息中的位置。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLGetConfigMode(  
     UWORD *   pwConfigMode);  
```  
  
## <a name="arguments"></a>参数  
 *pwConfigMode*  
 输出指向包含配置模式的缓冲区的指针。  (参阅 "注释"。) *\* PwConfigMode* 中的值可以是：  
  
 ODBC_USER_DSN  
  
 ODBC_SYSTEM_DSN  
  
 ODBC_BOTH_DSN  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetConfigMode** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 此函数用于确定列出 DSN 值的 Odbc.ini 条目在系统信息中所处的位置。 如果 ODBC_USER_DSN *\* pwConfigMode* ，则 DSN 是用户 dsn，而函数从 HKEY_CURRENT_USER 中的 Odbc.ini 条目读取。 如果 ODBC_SYSTEM_DSN，则 DSN 是系统 DSN，而函数从 HKEY_LOCAL_MACHINE 中的 Odbc.ini 条目读取。 如果 ODBC_BOTH_DSN，则会尝试 HKEY_CURRENT_USER，如果失败，则使用 HKEY_LOCAL_MACHINE。  
  
 默认情况下， **SQLGetConfigMode** 返回 ODBC_BOTH_DSN。 如果用户 DSN 或系统 DSN 是通过调用 **SQLConfigDataSource** 创建的，则该函数会将配置模式设置为 ODBC_USER_DSN 或 ODBC_SYSTEM_DSN，以区分用户和系统 dsn，同时修改 DSN。 在返回之前， **SQLConfigDataSource** 会将配置模式重置为 ODBC_BOTH_DSN。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|设置配置模式|[SQLSetConfigMode](../../../odbc/reference/syntax/sqlsetconfigmode-function.md)|
