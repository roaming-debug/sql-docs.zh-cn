---
description: 时间、日期和时间间隔函数
title: 时间、日期和时间间隔函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], time functions
- functions [ODBC], date functions
- interval functions [ODBC]
- functions [ODBC], interval functions
- time functions [ODBC]
- date functions [ODBC]
ms.assetid: bdf054a0-7aba-4e99-a34a-799917376fd5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e73d295613f3f11b72317d20b49a6c75be802cbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202501"
---
# <a name="time-date-and-interval-functions"></a>时间、日期和时间间隔函数
下表列出了 ODBC 标量函数集中包括的时间和日期函数。 应用程序可以通过使用 SQL_TIMEDATE_FUNCTIONS *信息类型* 调用 **SQLGetInfo** 来确定驱动程序支持的时间和日期函数。  
  
 表示为 *timestamp_exp* 的参数可以是列的名称、其他标量函数的结果 *、odbc-escape、* *odbc-escape* 或 *odbc 时间戳的转义*，其中，基础数据类型可表示为 SQL_CHAR、SQL_VARCHAR、SQL_TYPE_TIME、SQL_TYPE_DATE 或 SQL_TYPE_TIMESTAMP。  
  
 表示为 *date_exp* 的参数可以是列的名称、其他标量函数的结果，也可以是 *odbc 日期转义* 或 *odbc 时间戳转义*，其中，基础数据类型可表示为 SQL_CHAR、SQL_VARCHAR、SQL_TYPE_DATE 或 SQL_TYPE_TIMESTAMP。  
  
 表示为 *time_exp* 的参数可以是列的名称、其他标量函数的结果，也可以是 *odbc 时间转义* 或 *odbc 时间戳转义*，其中，基础数据类型可表示为 SQL_CHAR、SQL_VARCHAR、SQL_TYPE_TIME 或 SQL_TYPE_TIMESTAMP。  
  
 ODBC 3.0 中添加了 CURRENT_DATE、CURRENT_TIME 和 CURRENT_TIMESTAMP timedate.cpl 标量函数，以使其符合 SQL-92。  
  
|功能|说明|  
|--------------|-----------------|  
|CURRENT_DATE (ODBC 3.0 **( )**) |返回当前日期。|  
|**CURRENT_TIME [ (** *时间精度* **) ]** (ODBC 3.0) |返回当前本地时间。 *时间-精度* 参数确定返回值的秒精度。|  
|**CURRENT_TIMESTAMP**<br /> **[ (** *时间戳-精度* **) ]** (ODBC 3.0) |以时间戳值的形式返回当前本地日期和本地时间。 *Timestamp-精度* 参数确定所返回时间戳的秒精度。|  
| (ODBC 1.0 **) 的 CURDATE (**) |返回当前日期。|  
| (ODBC 1.0 **) 的 CURTIME (**) |返回当前本地时间。|  
|**DAYNAME (** *DATE_EXP* **)** (ODBC 2.0) |返回一个字符串，该字符串包含当天的特定于数据源的名称 (例如，星期日到周六或 Sun。 返回 Sunday 到 Saturday 或 Sun. 到 Sat.； 对于使用英语的数据源，或 Sonntag 到 Samstag 的数据源，该数据源使用德语) 作为 *date_exp* 的日部分。|  
|**DAYOFMONTH (** *DATE_EXP* **)** (ODBC 1.0) |返回一个月中的第几天，基于 *date_exp* 中的 "月" 字段作为1-31 范围内的整数值。|  
|**DAYOFWEEK (** *DATE_EXP* **)** (ODBC 1.0) |基于 *date_exp* 中的周字段返回一周中的第几天作为1-7 范围内的整数值，其中1表示星期日。|  
|**DAYOFYEAR (** *DATE_EXP* **)** (ODBC 1.0) |基于 *date_exp* 中的年字段返回一年中的第几天作为1-366 范围内的整数值。|  
| *从**提取* (提取-源 **)** (ODBC 3.0) |返回 *提取源* 的 *提取字段* 部分。 *提取-source* 参数是 datetime 或 interval 表达式。 *提取字段* 参数可以是以下关键字之一：<br /><br /> 每月日小时分钟秒<br /><br /> 返回的值的精度是实现定义的。 除非指定了 SECOND，否则小数位数为0，在这种情况下，小数位数不小于 *提取源* 字段的秒的小数部分精度。|  
| (ODBC) 1.0 *time_exp* **)** **小时 (**|基于 *time_exp* 中的小时字段返回以0-23 范围内的整数值形式的小时。|  
|**)** (ODBC 1.0 *time_exp* **(分钟**) |基于 *time_exp* 中的分钟字段返回分钟数，该值为0-59 范围内的整数值。|  
| (ODBC 1.0 *date_exp* **)** **月 (**) |将 *date_exp* 中的 "月份" 字段返回月份，该值为1-12 范围内的整数值。|  
|**MONTHNAME (** *DATE_EXP* **)** (ODBC 2.0) |返回一个字符串，该字符串包含月份的特定于数据源的名称 (例如，一月到十二月或 Jan. 到十二月。对于使用英语的数据源，或为使用德语) 的数据源的数据源 *date_exp* 的月份部分，为 Januar 到 Dezember。|  
|**现在 ( )** (ODBC 1.0) |以时间戳值的形式返回当前日期和时间。|  
|**季度 (** *DATE_EXP* **)** (ODBC 1.0) |返回 *date_exp* 中的季度作为1-4 范围内的整数值，其中1表示1月1日到3月31日。|  
| (ODBC 1.0 time_exp 的 **第二 (**  **)**) |基于作为0-59 范围内的整数值 *time_exp* 中的第二个字段返回第二个。|
|**TIMESTAMPADD (** *interval*， *INTEGER_EXP*， *timestamp_exp* **)** (ODBC 2.0) |返回通过将类型 *间隔* *integer_exp* 间隔添加到 *timestamp_exp* 来计算的时间戳。 *Interval* 的有效值为以下关键字：<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> 其中，秒的小数部分以十亿分之一表示。 例如，下面的 SQL 语句将返回每个雇员的姓名，并返回一年的周年周年日：<br /><br /> `SELECT NAME, {fn  TIMESTAMPADD(SQL_TSI_YEAR, 1, HIRE_DATE)} FROM  EMPLOYEES`<br /><br /> 如果 *timestamp_exp* 是时间值， *间隔* 指定日、周、月、季度或年，则在计算生成的时间戳之前， *timestamp_exp* 的日期部分将设置为当前日期。<br /><br /> 如果 *timestamp_exp* 是日期值， *间隔* 指定秒、秒、分钟或小时，则在计算生成的时间戳之前 *timestamp_exp* 的时间部分设置为0。<br /><br /> 应用程序通过使用 SQL_TIMEDATE_ADD_INTERVALS 选项调用 **SQLGetInfo** 来确定数据源支持的时间间隔。|  
|**TIMESTAMPDIFF (** *interval*， *TIMESTAMP_EXP1*， *timestamp_exp2* **)** (ODBC 2.0) |返回 *timestamp_exp2* 大于 *timestamp_exp1* 的类型 *间隔* 的整数间隔数。 *Interval* 的有效值为以下关键字：<br /><br /> SQL_TSI_FRAC_SECOND<br /><br /> SQL_TSI_SECOND<br /><br /> SQL_TSI_MINUTE<br /><br /> SQL_TSI_HOUR<br /><br /> SQL_TSI_DAY<br /><br /> SQL_TSI_WEEK<br /><br /> SQL_TSI_MONTH<br /><br /> SQL_TSI_QUARTER<br /><br /> SQL_TSI_YEAR<br /><br /> 其中，秒的小数部分以十亿分之一表示。 例如，以下 SQL 语句将返回每个雇员的姓名以及他或她采用的年数：<br /><br /> `SELECT NAME, {fn  TIMESTAMPDIFF(SQL_TSI_YEAR, {fn CURDATE()}, HIRE_DATE)} FROM EMPLOYEES`<br /><br /> 如果两个时间戳表达式都是时间值，而 *interval* 指定了天数、周数、月数、季度数或年数，则在计算时间戳之间的差异之前，该时间戳的日期部分将设置为当前日期。<br /><br /> 如果两个时间戳表达式均为日期值， *间隔* 指定小数、秒、分钟或小时，则在计算时间戳之间的差值之前，该时间戳的时间部分设置为0。<br /><br /> 应用程序通过使用 SQL_TIMEDATE_DIFF_INTERVALS 选项调用 **SQLGetInfo** 来确定数据源支持的时间间隔。|  
| (ODBC) 1.0 date_exp **(周**  **)**|基于 *date_exp* 中的周字段返回一年中的第几周作为1-53 范围内的整数值。|  
| (ODBC 1.0 的 **年份 (** *date_exp* **)**) |基于 *date_exp* 中的年份字段返回以整数值形式的年份。 范围依赖于数据源。|
