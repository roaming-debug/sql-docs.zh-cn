---
description: SQLRemoveDefaultDataSource 函数
title: SQLRemoveDefaultDataSource 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRemoveDefaultDataSource
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLRemoveDefaultDataSource
helpviewer_keywords:
- SQLRemoveDefaultDataSource function [ODBC]
ms.assetid: db803266-57df-4864-a41b-901247549c1f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 326522986785d7e76bc781fa4cc912401f2c237e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192513"
---
# <a name="sqlremovedefaultdatasource-function"></a>SQLRemoveDefaultDataSource 函数
**度**  
 引入的版本： ODBC 1.0，不推荐使用  
  
 **摘要**  
 在 ODBC 3.0 中，已使用 ODBC_REMOVE_DEFAULT_DSN 的 *fRequest* 参数调用 [SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)的 **SQLRemoveDefaultDataSource** 函数。 如果 ODBC 2.x 安装程序调用了此函数，则 *odbc 安装程序* 将映射到以下 **SQLConfigDataSource** 调用：  
  
```cpp  
SQLConfigDataSource (NULL, ODBC_REMOVE_DEFAULT_DSN, NULL, NULL)  
```
