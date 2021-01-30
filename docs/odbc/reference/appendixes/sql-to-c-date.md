---
description: 从 SQL 到 C：日期
title: SQL 到 C：日期 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], date
- date data type [ODBC]
- data conversions from SQL to C types [ODBC], date
ms.assetid: 703c7960-9cf4-4d7a-9920-53b29c184f97
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9909d0344c2df53212be54e4c695b121c0717e78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203047"
---
# <a name="sql-to-c-date"></a>从 SQL 到 C：日期
Date ODBC SQL 数据类型的标识符是：  
  
 SQL_TYPE_DATE  
  
 下表显示了 SQL 数据可转换到的 ODBC C 数据类型。 有关表中的列和字词的说明，请参阅将 [数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。  
  
|C 类型标识符|测试|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > 字符字节长度<br /><br /> 11 <= *BufferLength* <= 字符字节长度<br /><br /> *BufferLength* < 11|数据<br /><br /> 截断的数据<br /><br /> Undefined|10<br /><br /> 数据的长度（以字节为单位）<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* > 字符长度<br /><br /> 11 <= *BufferLength* <= 字符长度<br /><br /> *BufferLength* < 11|数据<br /><br /> 截断的数据<br /><br /> Undefined|10<br /><br /> 数据的长度（以字符为长度）<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Data <的字节长度 = *BufferLength*<br /><br /> 数据 > 的字节长度 *BufferLength*|数据<br /><br /> Undefined|数据的长度（以字节为单位）<br /><br /> Undefined|不适用<br /><br /> 22003|  
|SQL_C_TYPE_DATE|None [a]|数据|6 [c]|不适用|  
|SQL_C_TYPE_TIMESTAMP|None [a]|数据 [b]|16 [c]|不适用|  
  
 [a] 此转换将忽略 *BufferLength* 的值。 驱动程序假设大小 **TargetValuePtr* 是 C 数据类型的大小。  
  
 [b] timestamp 结构的时间字段设置为零。  
  
 [c] 这是对应的 C 数据类型的大小。  
  
 SQL 数据转换为字符 C 数据时，生成的字符串为 "*yyyy* - *mm* - *dd*" 格式。 此格式不受 Windows®的 "国家/地区" 设置的影响。
