---
description: 从 C 到 SQL 的数据转换示例
title: C 到 SQL 数据转换示例 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], examples
- data conversions from C to SQL types [ODBC], examples
ms.assetid: 9f390afc-d8b8-4286-b559-98b3b8781f3d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ebd11e85864619f8e5a98e2d8d288653bdc892ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199274"
---
# <a name="c-to-sql-data-conversion-examples"></a>从 C 到 SQL 的数据转换示例
下面的示例说明了驱动程序如何将 C 数据转换为 SQL 数据：  
  
|C 类型标识符|C 数据值|SQL 类型<br /><br /> 标识符|列<br /><br /> length|SQL 数据<br /><br /> value|SQLSTATE|  
|-----------------------|------------------|-----------------------------|-----------------------|------------------------|--------------|  
|SQL_C_CHAR|abcdef\0 [a]|SQL_CHAR|6|abcdef|不适用|  
|SQL_C_CHAR|abcdef\0 [a]|SQL_CHAR|5|abcde|22001|  
|SQL_C_CHAR|1234.56 \ 0 [a]|SQL_DECIMAL|8 [b]|1234.56|不适用|  
|SQL_C_CHAR|1234.56 \ 0 [a]|SQL_DECIMAL|7 [b]|1234.5|22001|  
|SQL_C_CHAR|1234.56 \ 0 [a]|SQL_DECIMAL|4|----|22003|  
|SQL_C_FLOAT|1234.56|SQL_FLOAT|不适用|1234.56|不适用|  
|SQL_C_FLOAT|1234.56|SQL_INTEGER|不适用|1234|22001|  
|SQL_C_FLOAT|1234.56|SQL_TINYINT|不适用|----|22003|  
|SQL_C_TYPE_DATE|1992、12、31 [c]|SQL_CHAR|10|1992-12-31|不适用|  
|SQL_C_TYPE_DATE|1992、12、31 [c]|SQL_CHAR|9|----|22003|  
|SQL_C_TYPE_DATE|1992、12、31 [c]|SQL_TIMESTAMP|不适用|1992-12-31 00：00：00。0|不适用|  
|SQL_C_TYPE_TIMESTAMP|1992，12，31，23，45，55，120000000 [d]|SQL_CHAR|22|1992-12-31 23：45：55.12|不适用|  
|SQL_C_TYPE_TIMESTAMP|1992，12，31，23，45，55，120000000 [d]|SQL_CHAR|21|1992-12-31 23：45：55。1|22001|  
|SQL_C_TYPE_TIMESTAMP|1992，12，31，23，45，55，120000000 [d]|SQL_CHAR|18|----|22003|  
  
 [a] "\ 0" 表示 null 终止字节。 只有在 SQL_NTS 数据的长度时，才需要 null 终止字节。  
  
 [b] 除了数字的字节之外，还需要一个字节用于符号，而小数点需要另一个字节。  
  
 [c] 此列表中的数字是存储在 SQL_DATE_STRUCT 结构的字段中的数字。  
  
 [d] 此列表中的数字是存储在 SQL_TIMESTAMP_STRUCT 结构的字段中的数字。
