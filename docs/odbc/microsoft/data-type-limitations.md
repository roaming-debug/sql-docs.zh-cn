---
description: 数据类型限制
title: 数据类型限制 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC desktop database drivers [ODBC], data types
- data types [ODBC], desktop database drivers
- desktop database drivers [ODBC], data types
ms.assetid: 81c4eab7-1f6b-47a0-b940-89d6c6a14dae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9081f52e7cf79613b9021ce7b883a1720da468c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176530"
---
# <a name="data-type-limitations"></a>数据类型限制
Microsoft ODBC 桌面数据库驱动程序对数据类型有以下限制：  
  
|数据类型|说明|  
|---------------|-----------------|  
|所有数据类型|如果类型转换失败，则可能导致受影响的列被设置为 NULL。|  
|BINARY|创建长度为零的二进制列实际上返回255字节的二进制列。|  
|DATE|DATE 数据类型无法转换为另一种数据类型 (或其自身) CONVERT 函数。|  
|小数 (精确数值) |不支持。|  
|Floating-Point 数据类型|浮点数中的小数位数可能受 Windows "控制面板" 的 "国际" 部分中设置的数字格式的限制。|  
|NUMERIC|支持最大精度和28的小数位数。|  
|TIMESTAMP|TIMESTAMP 数据类型不能通过 CONVERT 函数转换为自身。|  
|TINYINT|TINYINT 值始终是无符号值。|  
|Zero-Length 字符串|使用 dBASE、Microsoft Excel、Paradox 或 Textdriver 时，将长度为零的字符串插入列实际上是插入 NULL。|
