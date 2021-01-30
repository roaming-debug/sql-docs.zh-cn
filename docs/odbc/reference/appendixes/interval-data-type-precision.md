---
description: 间隔数据类型精度
title: 间隔数据类型精度 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- precision [ODBC], interval data types
- seconds precision [ODBC]
- interval data type [ODBC], precision
- precision [ODBC]
- interval leading precision [ODBC]
- interval precision [ODBC]
ms.assetid: eb73bd77-2e7e-4498-a266-4d7c990a0d56
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 103b19e43f4ba0cea160bfd53aa178404adfe781
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208537"
---
# <a name="interval-data-type-precision"></a>间隔数据类型精度
间隔数据类型的精度包括间隔的精度、间隔精度和秒的精度。  
  
 间隔的前导字段是有符号数字。 前导字段的最大位数由称为 *间隔前导精度* 的数量决定，该数量是数据类型声明的一部分。 例如，声明：时间间隔小时 (5) 到分钟，时间间隔的精度为 5;小时字段可采用从-99999 到99999的值。 间隔前导精度包含在描述符记录的 SQL_DESC_DATETIME_INTERVAL_PRECISION 字段中。  
  
 间隔数据类型所组成的字段列表称为 *间隔精度*。 它不是数值，因为术语 "精度" 可能表示。 例如，类型间隔 DAY 到秒的间隔精度是列表 DAY、HOUR、MINUTE、SECOND。 没有包含此值的描述符字段;间隔精度始终可以由 interval 数据类型确定。  
  
 包含第二个字段的任何间隔数据类型都具有 *秒的精度*。 这是秒值的小数部分中允许的小数位数。 这与其他数据类型不同，其中精度指示小数点前的数字位数。 Interval 数据类型的秒精度是小数点后的位数。 例如，如果秒精度设置为6，则 "分数" 字段中的数字123456将解释为. 123456，而数字1230将解释为. 001230。 对于其他数据类型，这称为 "规模"。 间隔秒精度包含在说明符的 SQL_DESC_PRECISION 字段中。 如果 SQL 时间间隔值的秒小数部分的精度大于 C 间隔结构中可以包含的值的精度，则它是驱动程序定义的，即在转换为 C 间隔结构时，是否对 SQL 间隔中的秒数值进行舍入或截断。  
  
 当 SQL_DESC_CONCISE_TYPE 字段设置为 interval 数据类型时，SQL_DESC_TYPE 字段将设置为 SQL_INTERVAL，并将 SQL_DESC_DATETIME_INTERVAL_CODE 设置为 interval 数据类型的代码。 "SQL_DESC_DATETIME_INTERVAL_PRECISION" 字段自动设置为默认间隔为2的默认时间间隔，并且 "SQL_DESC_PRECISION" 字段自动设置为默认间隔秒精度6。 如果这两个值中的任何一个不合适，则应用程序应通过调用 **SQLSetDescField** 显式设置描述符字段。
