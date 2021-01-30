---
description: SQLFreeConnect 映射
title: SQLFreeConnect 映射 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- mapping deprecated functions [ODBC], SQLFreeConnect
- SQLFreeConnect function [ODBC], mapping
ms.assetid: 8a844538-93c0-4709-bab6-35c45e771d80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e63a4609636d1fbea5ddf83ff629bf50a827cd4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202839"
---
# <a name="sqlfreeconnect-mapping"></a>SQLFreeConnect 映射
当应用程序 *通过 ODBC 1.x* 驱动程序调用 **SQLFreeConnect** 时，调用  
  
```  
SQLFreeConnect(hdbc)   
```  
  
 映射到  
  
```  
SQLFreeHandle(SQL_HANDLE_DBC,Handle)  
```  
  
 如果将 *Handle* 参数设置为 *hdbc* 中的值，则为。
