---
description: 定长书签
title: Fixed-Length 书签 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- backward compatibility [ODBC], bookmarks
- bookmarks [ODBC]
- compatibility [ODBC], bookmarks
- fixed-length bookmarks [ODBC]
ms.assetid: cbd8185e-fb03-408f-b80b-1a2e164534fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2d5eb7acd97a83be0d150cec8b19a5a3e25a7bf6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194807"
---
# <a name="fixed-length-bookmarks"></a>定长书签
如果 ODBC 2.x *驱动程序* 应与使用固定长度书签的 odbc *2.x 应用程序* 一起使用，则驱动程序必须支持以下各项：  
  
-   SQL_UB_ON 作为 SQL_USE_BOOKMARKS 语句选项的值。 *ODBC 2.x* 中 (SQL_UB_ON 已弃用 )   
  
-   SQL_GET_BOOKMARK 语句选项。
