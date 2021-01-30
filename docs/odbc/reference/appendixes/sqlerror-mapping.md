---
description: SQLError 映射
title: SQLError 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLError
- SQLError function [ODBC], mapping
ms.assetid: 802ac711-7e5d-4152-9698-db0cafcf6047
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 95cb5d4c22ff0baec48e20da2be582d0fbab97c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202880"
---
# <a name="sqlerror-mapping"></a>SQLError 映射
当应用程序 *通过 ODBC 1.x* 驱动程序调用 **SQLError** 时，调用  
  
```  
SQLError(henv, hdbc, hstmt, szSqlState, pfNativeError, szErrorMsg, cbErrorMsgMax, pcbErrorMsg)   
```  
  
 映射到  
  
```  
SQLGetDiagRec(HandleType, Handle, RecNumber, szSqlstate, pfNativeErrorPtr, szErrorMsg, cbErrorMsgMax, pcbErrorMsg)  
```  
  
 根据需要，将 *HandleType* 参数设置为值 SQL_HANDLE_ENV、SQL_HANDLE_DBC 或 SQL_HANDLE_STMT，并根据需要将 *HANDLE* 参数设置为 *henv*、 *hdbc* 或 *hstmt* 中的值。 *RecNumber* 参数由驱动程序管理器决定。
