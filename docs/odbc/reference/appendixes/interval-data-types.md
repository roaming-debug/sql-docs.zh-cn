---
description: 间隔数据类型
title: 间隔数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- second intervals [ODBC]
- data types [ODBC], interval data types
- interval data type [ODBC]
- day-time intervals [ODBC]
- intervals [ODBC], about intervals
- minute intervals [ODBC]
- day intervals [ODBC]
- year intervals [ODBC]
- month intervals [ODBC]
- interval data type [ODBC], about interval data types
- SQL data types [ODBC], interval
- year-month intervals [ODBC]
- C data types [ODBC], interval
- interval fields [ODBC]
ms.assetid: fba93f65-c1db-44f4-91ba-532f87241cf7
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 41418b59d61b184717c4a3654491154221b9a791
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207141"
---
# <a name="interval-data-types"></a>间隔数据类型
间隔定义为两个日期和时间之间的差值。 间隔用两种不同的方式表示。 一个 *月* 时间间隔表示年和整数个月的时间间隔。 另一种是 *日期时间* 间隔，以天、分钟和秒为单位表达时间间隔。 这两种间隔是不同的，并且不能混合，因为月份的天数可能不同。  
  
 间隔由一组字段组成。 字段中有隐含的排序。 例如，在年到月的间隔中，年份首先出现，然后是月份。 同样，在日常间隔中，字段的顺序为 day、hour 和 minute。 间隔类型中的第一个字段称为 " *前导* 字段" 或 " *高顺序* " 字段。 最后一个字段称为 *结尾* 字段。  
  
 在所有间隔中，前导字段不受公历规则的约束。 例如，在一个小时到分钟的时间间隔内，小时字段不会被限制为介于0到23之间 (包括) ，这通常是如此。 前导字段后面的尾随字段遵循公历的常用约束。 有关详细信息，请参阅本附录后面的 ["公历限制"](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)。  
  
 有13个间隔 SQL 数据类型和13个间隔 C 数据类型。 每个时间间隔 C 数据类型都使用相同的结构 SQL_INTERVAL_STRUCT，以包含时间间隔数据。  (有关详细信息，请参阅下一节 [C 间隔结构](../../../odbc/reference/appendixes/c-interval-structure.md)。 ) 有关 sql 数据类型的详细信息，请参阅 [sql 数据类型](../../../odbc/reference/appendixes/sql-data-types.md);有关 C 数据类型的详细信息，请参阅 [c 数据类型](../../../odbc/reference/appendixes/c-data-types.md)。  
  
|类型标识符|实例|说明|  
|---------------------|-----------|-----------------|  
|MONTH|Year-Month|两个日期之间的月数。|  
|YEAR|Year-Month|两个日期之间的年数。|  
|YEAR_TO_MONTH|Year-Month|两个日期之间的年和月数。|  
|DAY|Day-Time|两个日期之间的天数。|  
|HOUR|Day-Time|两个日期/时间间隔的小时数。|  
|MINUTE|Day-Time|两个日期/时间间隔的分钟数。|  
|SECOND|Day-Time|两个日期/时间之间的秒数。|  
|DAY_TO_HOUR|Day-Time|两个日期/时间之间的天数/小时数。|  
|DAY_TO_MINUTE|Day-Time|两个日期/时间之间的天数/小时/分钟数。|  
|DAY_TO_SECOND|Day-Time|两个日期/时间之间的天数/小时/分钟/秒。|  
|HOUR_TO_MINUTE|Day-Time|两个日期/时间之间的小时数/分钟数。|  
|HOUR_TO_SECOND|Day-Time|两个日期/时间之间的小时/分钟数/秒。|  
|MINUTE_TO_SECOND|Day-Time|两个日期/时间之间的分钟数/秒。|  
  
 本部分包含以下主题。  
  
-   [C 间隔结构](../../../odbc/reference/appendixes/c-interval-structure.md)  
  
-   [间隔数据类型精度](../../../odbc/reference/appendixes/interval-data-type-precision.md)  
  
-   [间隔数据类型长度](../../../odbc/reference/appendixes/interval-data-type-length.md)  
  
-   [间隔文本](../../../odbc/reference/appendixes/interval-literals.md)  
  
-   [重写间隔数据类型的默认前导和秒精度](../../../odbc/reference/appendixes/overriding-default-leading-and-seconds-precision-for-interval-data-types.md)
