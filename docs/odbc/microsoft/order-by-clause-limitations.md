---
description: ORDER BY 子句限制
title: ORDER BY 子句限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC SQL grammar, ORDER BY clause limitations
- ORDER BY clause limitations [ODBC]
ms.assetid: fd4ddc7c-9c7e-4a0c-a781-e5427dfb2e18
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 51ec06b42e3aca1c48ef243c7032608d28b21a08
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158194"
---
# <a name="order-by-clause-limitations"></a>ORDER BY 子句限制
如果 SELECT 语句包含 GROUP BY 子句和 ORDER BY 子句，则 ORDER BY 子句只能在结果集中包含列，或在 GROUP BY 子句中包含表达式。
