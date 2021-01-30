---
description: SQLGetInstalledDrivers 函数
title: SQLGetInstalledDrivers 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetInstalledDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetInstalledDrivers
helpviewer_keywords:
- SQLGetInstalledDrivers function [ODBC]
ms.assetid: a1983a2e-0edf-422e-bd1b-ec5db40a34bc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5f770254874b09158632ebaee071964c00265a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180951"
---
# <a name="sqlgetinstalleddrivers-function"></a>SQLGetInstalledDrivers 函数
**度**  
 引入的版本： ODBC 1。0  
  
 **摘要**  
 **SQLGetInstalledDrivers** 读取系统信息的 [ODBC 驱动程序] 部分，并返回已安装驱动程序的说明的列表。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
BOOL SQLGetInstalledDrivers(  
     LPSTR   lpszBuf,  
     WORD    cbBufMax,  
     WORD *  pcbBufOut);  
```  
  
## <a name="arguments"></a>参数  
 *lpszBuf*  
 输出已安装驱动程序的说明的列表。 有关列表结构的详细信息，请参阅 "注释"。  
  
 *cbBufMax*  
 送 *LpszBuf* 的长度。  
  
 *pcbBufOut*  
 输出 (排除在 *lpszBuf* 中返回的 null 终止字节) 的总字节数。 如果可返回的字节数大于或等于 *cbBufMax*，则 *lpszBuf* 中的驱动程序说明列表将被截断为 *cbBufMax* 减 null 终止字符。 *PcbBufOut* 参数可以为 null 指针。  
  
## <a name="returns"></a>返回  
 如果此函数成功，则返回 TRUE，否则返回 FALSE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetInstalledDrivers** 返回 FALSE 时，可以通过调用 **SQLInstallerError** 获取关联的 *\* pfErrorCode* 值。 下表列出了可由 **SQLInstallerError** 返回的 *\* pfErrorCode* 值，并说明了此函数的上下文中的每个值。  
  
|*\*pfErrorCode*|错误|说明|  
|---------------------|-----------|-----------------|  
|ODBC_ERROR_GENERAL_ERR|常规安装程序错误|出现错误，但没有特定的安装程序错误。|  
|ODBC_ERROR_INVALID_BUFF_LEN|缓冲区长度无效|*LpszBuf* 参数为 NULL 或无效，或者 *cbBufMax* 参数小于或等于0。|  
|ODBC_ERROR_COMPONENT_NOT_FOUND|在注册表中找不到组件|安装程序无法在注册表中找到 [ODBC 驱动程序] 部分。|  
|ODBC_ERROR_OUT_OF_MEM|内存不足|由于内存不足，安装程序无法执行此功能。|  
  
## <a name="comments"></a>注释  
 每个驱动程序说明都以 null 字节结束，整个列表以 null 字节结束。  (即，两个 null 字节标记列表的末尾。 ) 如果分配的缓冲区不够大，无法容纳整个列表，则列表会被截断，并且不会出错。 如果将 null 指针作为 *lpszBuf* 传入，将返回错误。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|返回驱动程序说明和属性|[SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|
