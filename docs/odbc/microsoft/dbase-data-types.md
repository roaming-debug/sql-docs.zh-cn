---
description: dBASE 数据类型
title: dBASE 数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- Jet-based ODBC drivers [ODBC], DBasedriver
- desktop database drivers [ODBC], DBasedriver
- DBase driver [ODBC], data types
- data types [ODBC], DBase driver
- dbase data types [ODBC]
- ODBC desktop database drivers [ODBC], DBasedriver
ms.assetid: a0e31e6b-d02b-4ee2-9b37-5baf6a11c0a6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4ef5b42157a011bf719614b7378cd73733e28afc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165428"
---
# <a name="dbase-data-types"></a>dBASE 数据类型
下表显示了如何将 dBASE 数据类型映射到 ODBC SQL 数据类型。 请注意，并非所有 ODBC SQL 数据类型都受支持。  
  
|dBASE 数据类型|ODBC 数据类型|  
|---------------------|--------------------|  
|CHAR|SQL_VARCHAR|  
|DATE|SQL_DATE|  
|FLOAT [1]|SQL_DOUBLE|  
|LOGICAL|SQL_BIT|  
|"|SQL_LONGVARCHAR|  
|数值 (BCD) |SQL_DOUBLE|  
|OLEOBJECT [1]|SQL_LONGBINARY|  
  
 [1] 仅对 dBASE 版本5有效。*x*  
  
 DBASE III 中的精度允许最多具有两位数字指数的数字和包含最多三位数指数的 dBASE IV 数字。 由于数字是以文本形式存储的，因此将其转换为数字。 如果要转换的数字不能容纳在字段中，则可能会出现无法解释的结果。  
  
 当 dBASE 允许使用数值数据类型指定精度和小数位数时，ODBC dBASE 驱动程序不支持该数据类型。 ODBC dBASE 驱动程序始终为数值数据类型返回15的精度，小数位数为0。  
  
 使用数据类型（ODBC dBASE 驱动程序）创建的列映射到 SQL_DOUBLE ODBC 数据类型。 因此，此列中的数据可能会进行舍入。 此行为不同于 dBASE (类型 N) （二进制编码 Decimal (BCD) ）中数值数据类型的行为。  
  
> [!NOTE]  
>  **SQLGetTypeInfo** 返回 ODBC SQL 数据类型。 对于本主题中前面列出的 ODBC SQL 数据类型，支持 *Odbc 程序员参考* 的附录 D 中的所有转换。  
  
 下表显示了对 dBASE 数据类型的限制。  
  
|数据类型|说明|  
|---------------|-----------------|  
|CHAR|创建零或未指定长度的 CHAR 列实际上将返回254字节的列。|  
|加密数据|DBASE 驱动程序不支持加密的 dBASE 表。|  
|LOGICAL|DBASE 驱动程序无法对逻辑列创建索引。|  
|"|备注列的最大长度为65500字节。|  
  
 [数据类型限制](../../odbc/microsoft/data-type-limitations.md)中可以找到更多有关数据类型的限制。
