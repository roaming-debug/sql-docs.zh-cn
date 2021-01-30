---
description: C 间隔结构
title: C 间隔结构 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval data type [ODBC], structure
- C data types [ODBC], interval
ms.assetid: 52b42b56-50aa-4ce6-8d79-0963c7a71437
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e470d0e30864c953fa3937725bcd5597bf542f41
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212434"
---
# <a name="c-interval-structure"></a>C 间隔结构
" [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md) " 部分中列出的每个 c 间隔数据类型都使用相同的结构来包含时间间隔数据。 调用 **SQLFetch**、 **SQLFetchScroll** 或 **SQLGetData** 时，驱动程序会将数据返回到 SQL_INTERVAL_STRUCT 结构，使用应用程序为 c 数据类型指定的值 (在对 **SQLBindCol**、 **SQLGetData** 或 **SQLBindParameter**) 的调用中，以解释 SQL_INTERVAL_STRUCT 的内容，并使用对应于 C 类型的 *枚举* 值填充结构的 *interval_type* 字段。 请注意，驱动程序不会读取 *interval_type* 字段来确定间隔的类型;它们检索 SQL_DESC_CONCISE_TYPE 描述符字段的值。 当结构用于参数数据时，驱动程序将使用 APD 的 SQL_DESC_CONCISE_TYPE 字段中的应用程序指定的值来解释 SQL_INTERVAL_STRUCT 的内容，即使应用程序将 *interval_type* 字段的值设置为其他值。  
  
 此结构的定义如下：  
  
```  
typedef struct tagSQL_INTERVAL_STRUCT  
{  
   SQLINTERVAL interval_type;   
   SQLSMALLINT interval_sign;  
   union {  
         SQL_YEAR_MONTH_STRUCT   year_month;  
         SQL_DAY_SECOND_STRUCT   day_second;  
         } intval;  
} SQL_INTERVAL_STRUCT;  
typedef enum   
{  
   SQL_IS_YEAR = 1,  
   SQL_IS_MONTH = 2,  
   SQL_IS_DAY = 3,  
   SQL_IS_HOUR = 4,  
   SQL_IS_MINUTE = 5,  
   SQL_IS_SECOND = 6,  
   SQL_IS_YEAR_TO_MONTH = 7,  
   SQL_IS_DAY_TO_HOUR = 8,  
   SQL_IS_DAY_TO_MINUTE = 9,  
   SQL_IS_DAY_TO_SECOND = 10,  
   SQL_IS_HOUR_TO_MINUTE = 11,  
   SQL_IS_HOUR_TO_SECOND = 12,  
   SQL_IS_MINUTE_TO_SECOND = 13  
} SQLINTERVAL;  
  
typedef struct tagSQL_YEAR_MONTH  
{  
   SQLUINTEGER year;  
   SQLUINTEGER month;   
} SQL_YEAR_MONTH_STRUCT;  
  
typedef struct tagSQL_DAY_SECOND  
{  
   SQLUINTEGER day;  
   SQLUINTEGER hour;  
   SQLUINTEGER minute;  
   SQLUINTEGER second;  
   SQLUINTEGER fraction;  
} SQL_DAY_SECOND_STRUCT;  
```  
  
 SQL_INTERVAL_STRUCT 的 " *interval_type* " 字段向应用程序指示在联合中包含的结构，以及结构的相关成员。 如果 "间隔前导" 字段为 "无符号"，则 " *interval_sign* " 字段的值为 SQL_FALSE;如果 SQL_TRUE，则前导字段为负数。 不管 *interval_sign* 的值是什么，前导字段本身中的值始终为无符号值。 *Interval_sign* 字段用作符号位。
