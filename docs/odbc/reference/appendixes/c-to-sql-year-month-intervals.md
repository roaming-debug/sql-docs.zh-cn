---
description: 从 C 到 SQL：年月间隔
title: C 到 SQL： Year-Month 间隔 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- converting data from c to SQL types [ODBC], year-month intervals
- intervals [ODBC], converting
- year-month intervals [ODBC]
- data conversions from C to SQL types [ODBC], year-month intervals
ms.assetid: a0eb7b55-9db0-4375-9210-bddec4593880
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0748585beeae18a0b159cf131a67b4b5ea3cdd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158575"
---
# <a name="c-to-sql-year-month-intervals"></a>从 C 到 SQL：年月间隔
每月间隔 ODBC C 数据类型的标识符为：  
  
 SQL_C_INTERVAL_MONTH SQL_C_INTERVAL_YEAR SQL_C_INTERVAL_YEAR_TO_MONTH  
  
 下表显示了可转换年月间隔 C 数据的 ODBC SQL 数据类型。 有关表中的列和字词的说明，请参阅 [将数据从 C 转换为 SQL 数据类型](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)。  
  
|SQL 类型标识符|测试|SQLSTATE|  
|-------------------------|----------|--------------|  
|SQL_CHAR [a]<br /><br /> SQL_VARCHAR [a]<br /><br /> SQL_LONGVARCHAR [a]|列字节长度 >= 字符字节长度<br /><br /> 列字节长度 < 字符字节长度 [a]<br /><br /> 数据值不是有效的间隔文本|不适用<br /><br /> 22001<br /><br /> 22015|  
|SQL_WCHAR [a]<br /><br /> SQL_WVARCHAR [a]<br /><br /> SQL_WLONGVARCHAR [a]|列字符长度 >= 数据的字符长度<br /><br /> 列字符长度 < 数据的字符长度 [a]<br /><br /> 数据值不是有效的间隔文本|不适用<br /><br /> 22001<br /><br /> 22015|  
|SQL_TINYINT [b]<br /><br /> SQL_SMALLINT [b]<br /><br /> SQL_INTEGER [b]<br /><br /> SQL_BIGINT [b]<br /><br /> SQL_NUMERIC [b]<br /><br /> SQL_DECIMAL [b]|单字段间隔的转换未导致整数被截断<br /><br /> 转换导致整数被截断|不适用<br /><br /> 22003|  
|SQL_INTERVAL_MONTH<br /><br /> SQL_INTERVAL_YEAR<br /><br /> SQL_INTERVAL_YEAR_TO_MONTH|数据值已转换而不截断任何字段<br /><br /> 转换过程中一个或多个数据值字段被截断|不适用<br /><br /> 22015|  
  
 [a] 所有 C 间隔数据类型都可以转换为字符数据类型。  
  
 [b] 如果间隔结构中的 "类型" 字段的间隔是 (SQL_YEAR 或 SQL_MONTH) 的单个字段，则可以将 "间隔 C 类型" 转换为任何精确数值 (SQL_TINYINT、SQL_SMALLINT、SQL_INTEGER、SQL_BIGINT、SQL_DECIMAL 或 SQL_NUMERIC) 。  
  
 Interval C 类型的默认转换为相应的年-月间隔 SQL 类型。  
  
 驱动程序在从 interval C 数据类型转换数据时忽略长度/指示器值，并假定数据缓冲区的大小为时间间隔 C 数据类型的大小。 长度/指示器值传入 **SQLPutData** 中的 *StrLen_or_Ind* 参数和在 **SQLBindParameter** 中通过 *StrLen_or_IndPtr* 参数指定的缓冲区中。 数据缓冲区是通过 **SQLPutData** 中的 *DataPtr* 参数和 **SQLBindParameter** 中的 *ParameterValuePtr* 参数指定的。
