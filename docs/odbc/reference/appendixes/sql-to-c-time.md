---
description: 从 SQL 到 C：时间
title: SQL 到 C：时间 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from SQL to C types [ODBC], time
- time data type [ODBC]
- data conversions from SQL to C types [ODBC], time
ms.assetid: 6dc59973-7bb5-40f1-87c8-5bf68b3bf2ee
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: db1882befdb1b295ce247cb62601bbec4ba3db5f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203011"
---
# <a name="sql-to-c-time"></a>从 SQL 到 C：时间
ODBC SQL 数据类型的时间的标识符为：  
  
 SQL_TYPE_TIME  
  
 下表显示了可将 SQL 数据转换到的 ODBC C 数据类型。 有关表中的列和字词的说明，请参阅将 [数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。  
  
|C 类型标识符|测试|**TargetValuePtr*|**StrLen_or_IndPtr*|SQLSTATE|  
|-----------------------|----------|------------------------|----------------------------|--------------|  
|SQL_C_CHAR|*BufferLength* > 字符字节长度<br /><br /> *9*  <= *BufferLength* <= 字符字节长度<br /><br /> *BufferLength* < 9|数据<br /><br /> 截断的数据 [a]<br /><br /> Undefined|数据的长度（以字节为单位）<br /><br /> 数据的长度（以字节为单位）<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_WCHAR|*BufferLength* > 字符长度<br /><br /> *9*  <= *BufferLength* <= 字符长度<br /><br /> *BufferLength* < 9|数据<br /><br /> 截断的数据 [a]<br /><br /> Undefined|数据的长度（以字符为长度）<br /><br /> 数据的长度（以字符为长度）<br /><br /> Undefined|不适用<br /><br /> 01004<br /><br /> 22003|  
|SQL_C_BINARY|Data <的字节长度 = *BufferLength*<br /><br /> 数据 > 的字节长度 *BufferLength*|数据<br /><br /> Undefined|数据的长度（以字节为单位）<br /><br /> Undefined|不适用<br /><br /> 22003|  
|SQL_C_TYPE_TIME|None [b]|数据|6 [d]|不适用|  
|SQL_C_TYPE_TIMESTAMP|None [b]|数据 [c]|16 [d]|不适用|  
  
 [a] 时间的小数部分被截断。  
  
 [b] 此转换将忽略 *BufferLength* 的值。 驱动程序假设大小 **TargetValuePtr* 是 C 数据类型的大小。  
  
 [c] 时间戳结构的日期字段设置为当前日期，timestamp 结构的秒小数部分字段设置为零。  
  
 [d] 这是对应的 C 数据类型的大小。  
  
 当将 SQL 数据转换为字符 C 数据时，生成的字符串为 "*hh*：*mm*：*ss*" 格式。 此格式不受 Windows®的 "国家/地区" 设置的影响。
