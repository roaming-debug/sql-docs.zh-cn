---
description: SQLValidDSN 函数
title: SQLValidDSN 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLValidDSN
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLValidDSN
helpviewer_keywords:
- SQLValidDSN [ODBC]
ms.assetid: 930d1d89-337a-4429-85a2-84ee10555ac9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 62692ed4b5d0ab600ceb36e87e7f796c77120c89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191706"
---
# <a name="sqlvaliddsn-function"></a>SQLValidDSN 函数
**度**  
 引入的版本： ODBC 2。0  
  
 **摘要**  
 在将名称添加到系统信息之前， **SQLValidDSN** 将检查数据源名称的长度和有效性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLValidDSN(  
     LPCSTR    lpszDSN);  
```  
  
## <a name="arguments"></a>参数  
 *lpszDSN*  
 送要检查的数据源名称。  
  
## <a name="returns"></a>返回  
 如果数据源名称有效，则函数返回 TRUE。 如果数据源名称无效或函数调用失败，则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLValidDSN** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 仅当函数调用失败时，才会返回 *\* pfErrorCode* ，如果由于数据源名称无效，则返回 FALSE。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 **SQLValidDSN** 由驱动程序的 [ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) 调用，以检查数据源名称的长度以及数据源名称中各个字符的有效性。 它将检查名称长度是否大于 SQL_MAX_DSN_LENGTH，如 Sqltypes.h 中所定义。  (还会通过 [SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)检查数据源名称的长度。 ) **SQLValidDSN** 检查数据源名称中是否包含以下任何无效字符：  
  
 [ ] { } ( ) , ; ? * = ! \@ \  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|添加、修改或删除数据源|安装程序 DLL 中的[ConfigDSN](../../../odbc/reference/syntax/configdsn-function.md) () |  
|添加、修改或删除数据源|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|  
|将数据源名称写入系统信息|[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|
