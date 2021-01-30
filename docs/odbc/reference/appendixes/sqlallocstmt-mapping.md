---
description: SQLAllocStmt 映射
title: SQLAllocStmt 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLAllocStmt
- SQLAllocStmt function [ODBC], mapping
ms.assetid: a2449dbb-1b6c-4b49-81b9-ebdddd4442fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa2b3392ce9cce9b6cf98d18fe6672bb0034bf2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202959"
---
# <a name="sqlallocstmt-mapping"></a>SQLAllocStmt 映射
当应用程序 *通过 ODBC 1.x* 驱动程序调用 **SQLAllocStmt** 时，将调用：  
  
```  
SQLAllocStmt(hdbc, phstmt)  
```  
  
 驱动程序管理器将按如下所示映射到 **SQLAllocHandle** ：  
  
```  
SQLAllocHandle(SQL_HANDLE_STMT, InputHandle, OutputHandlePtr)  
```  
  
 将 *将 inputhandle* 设置为 *hdbc* ，并将 *OutputHandlePtr* 设置为 *phstmt*。
