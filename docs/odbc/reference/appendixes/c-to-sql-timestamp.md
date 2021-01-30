---
description: 从 C 到 SQL：时间戳
title: 从 C 到 SQL：时间戳 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from C to SQL types [ODBC], timestamp
- timestamp data type [ODBC]
- converting data from c to SQL types [ODBC], timestamp
ms.assetid: 0e08bfff-68f9-4648-9558-09b57fea08ad
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 54632816fb7013639e1cdb761fbc8df53c461a0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158585"
---
# <a name="c-to-sql-timestamp"></a>从 C 到 SQL：时间戳
Timestamp ODBC C 数据类型的标识符是：  
  
 SQL_C_TYPE_TIMESTAMP  
  
 下表显示了可转换 timestamp C 数据的 ODBC SQL 数据类型。 有关表中的列和字词的说明，请参阅 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)。  
  
|SQL 类型标识符|测试|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR<br /><br /> SQL_VARCHAR<br /><br /> SQL_LONGVARCHAR|列字节长度 >= 字符字节长度<br /><br /> 19 <= 列字节长度 < 字符字节长度<br /><br /> 列字节长度 < 19<br /><br /> 数据值不是有效的时间戳|不适用<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_WCHAR<br /><br /> SQL_WVARCHAR<br /><br /> SQL_WLONGVARCHAR|列字符长度 >= 数据的字符长度<br /><br /> 19 <= 列字符长度 < 字符长度<br /><br /> 列字符长度 < 19<br /><br /> 数据值不是有效的时间戳|不适用<br /><br /> 22001<br /><br /> 22001<br /><br /> 22008|  
|SQL_TYPE_DATE|时间字段为零<br /><br /> 时间字段不为零<br /><br /> 数据值未包含有效日期|不适用<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIME|秒的小数部分字段为零 [a]<br /><br /> 秒的小数部分字段不为零 [a]<br /><br /> 数据值未包含有效时间|不适用<br /><br /> 22008<br /><br /> 22007|  
|SQL_TYPE_TIMESTAMP|不截断秒的小数部分字段<br /><br /> 截断秒的小数部分字段<br /><br /> 数据值不是有效的时间戳|不适用<br /><br /> 22008<br /><br /> 22007|  
  
 [a] 忽略时间戳结构的日期字段。  
  
 有关 SQL_C_TIMESTAMP 结构中的有效值的信息，请参阅本附录前面的 [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md)。  
  
 当时间戳 C 数据转换为字符 SQL 数据时，生成的字符数据在 "*yyyy* - *mm* - *dd* *hh*：*mm*：*ss*[.*f ...*] "格式。  
  
 驱动程序在从 timestamp C 数据类型转换数据时忽略长度/指示器值，并假定数据缓冲区的大小为时间戳 C 数据类型的大小。 长度/指示器值传入 **SQLPutData** 中的 *StrLen_or_Ind* 参数和在 **SQLBindParameter** 中通过 *StrLen_or_IndPtr* 参数指定的缓冲区中。 数据缓冲区是通过 **SQLPutData** 中的 *DataPtr* 参数和 **SQLBindParameter** 中的 *ParameterValuePtr* 参数指定的。
