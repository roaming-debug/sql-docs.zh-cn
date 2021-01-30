---
description: WHERE 子句限制
title: WHERE 子句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, WHERE clause limitations
- WHERE clause limitations [ODBC]
ms.assetid: 46b54f74-e4a3-4318-87cf-8a97c38a2718
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 80a0d29b57e5498c6d14e5d1e79600751d6f85c3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176505"
---
# <a name="where-clause-limitations"></a>WHERE 子句限制
WHERE 子句中的最大子句数为40。  
  
 LONGVARBINARY 和 LONGVARCHAR 列可以与长度最大为255个字符的文本进行比较，但不能使用参数进行比较。
