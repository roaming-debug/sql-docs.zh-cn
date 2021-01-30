---
description: 从 SQL 到 C 的数据转换示例
title: SQL 到 C 数据转换示例 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data conversions from SQL to C types [ODBC], examples
- converting data from SQL to C types [ODBC], examples
ms.assetid: 0190c76c-7f9b-42f4-be9d-cef7284840fd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f75b048dd9e40a0bef20640b73322c5c3b6e663
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203053"
---
# <a name="sql-to-c-data-conversion-examples"></a>从 SQL 到 C 的数据转换示例

下表中所示的示例说明了驱动程序如何将 SQL 数据转换为 C 数据：  
  
|SQL 类型<br /><br /> 标识符|SQL 数据<br /><br /> value|C 类型<br /><br /> 标识符|Buffer<br /><br /> length|**TargetValuePtr*|SQLSTATE|  
|-----------------------------|------------------------|---------------------------|-----------------------|------------------------|--------------|  
|SQL_CHAR|abcdef|SQL_C_CHAR|7|abcdef\0 [a]|不适用|  
|SQL_CHAR|abcdef|SQL_C_CHAR|6|abcde\0 [a]|01004|  
|SQL_DECIMAL|1234.56|SQL_C_CHAR|8|1234.56 \ 0 [a]|不适用|  
|SQL_DECIMAL|1234.56|SQL_C_CHAR|5|1234 \ 0 [a]|01004|  
|SQL_DECIMAL|1234.56|SQL_C_CHAR|4|----|22003|  
|SQL_DECIMAL|1234.56|SQL_C_FLOAT|已忽略|1234.56|不适用|  
|SQL_DECIMAL|1234.56|SQL_C_SSHORT|已忽略|1234|01S07|  
|SQL_DECIMAL|1234.56|SQL_C_STINYINT|已忽略|----|22003|  
|SQL_DOUBLE|1.2345678|SQL_C_DOUBLE|已忽略|1.2345678|不适用|  
|SQL_DOUBLE|1.2345678|SQL_C_FLOAT|已忽略|1.234567|不适用|  
|SQL_DOUBLE|1.2345678|SQL_C_STINYINT|已忽略|1|不适用|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|11|1992-12-31 \ 0 [a]|不适用|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_CHAR|10|-----|22003|  
|SQL_TYPE_DATE|1992-12-31|SQL_C_TIMESTAMP|已忽略|1992，12，31，0，0，0，0 [b]|不适用|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23：45：55.12|SQL_C_CHAR|23|1992-12-31 23：45： 55.12 \ 0 [a]|不适用|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23：45：55.12|SQL_C_CHAR|22|1992-12-31 23：45： 55.1 \ 0 [a]|01004|  
|SQL_TYPE_TIMESTAMP|1992-12-31 23：45：55.12|SQL_C_CHAR|18|----|22003|  
  
 [a] "\ 0" 表示 null 终止字节。 驱动程序始终以 null 终止 SQL_C_CHAR 数据。  
  
 [b] 此列表中的数字是存储在 TIMESTAMP_STRUCT 结构的字段中的数字。
