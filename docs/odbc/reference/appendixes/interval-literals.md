---
description: 间隔文本
title: 间隔文本 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data types [ODBC], interval data types
- interval literals [ODBC]
- interval data type [ODBC], literals
ms.assetid: f9e6c3c7-4f98-483f-89d8-ebc5680f021b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1e100fcc54f9ca6cc165eece7637b7fae45f65b4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184412"
---
# <a name="interval-literals"></a>间隔文本
ODBC 要求所有驱动程序都支持将 SQL_CHAR 或 SQL_VARCHAR 数据类型转换为所有 C interval 数据类型。 但是，如果基础数据源不支持时间间隔数据类型，则驱动程序需要知道 SQL_CHAR 字段中的值的正确格式，以便支持这些转换。 同样，ODBC 要求任何 ODBC C 类型都可转换为 SQL_CHAR 或 SQL_VARCHAR，因此驱动程序需要知道字符字段中存储的间隔格式应该是什么格式。 本部分介绍了间隔文本的语法，驱动程序编写器在转换为或从 C 间隔数据类型转换的过程中，需要使用该语法来验证 SQL_CHAR 字段。  
  
> [!NOTE]  
>  "附录 C： SQL 语法" 的 " [间隔文本语法](../../../odbc/reference/appendixes/interval-literal-syntax.md) " 部分显示了间隔文本的完整 BNF 语法。  
  
 若要将间隔文本传递为 SQL 语句的一部分，请为间隔文本定义一个转义子句语法。 有关详细信息，请参阅 [日期、时间和时间戳文本](../../../odbc/reference/develop-app/date-time-and-timestamp-literals.md)。  
  
 间隔文本格式如下：  
  
```  
INTERVAL[<sign>] 'value' <interval qualifier>  
```  
  
 其中 "INTERVAL" 指示字符文本为间隔。 符号可以是加号还是减号;它不在间隔字符串的范围内，并且是可选的。  
  
 间隔限定符可以是单个日期时间字段，也可以是由两个日期时间字段组成的，格式 \<*leading field*> 为：到 \<*trailing field*> 。  
  
-   如果间隔由单个字段组成，则单个字段可以为非第二个字段，该字段可能伴随括号中的可选前导精度。 单日期时间字段也可以是第二个字段，该字段可能伴随可选的前导精度、括号中可选的小数秒精度或同时为两者。 如果一个秒字段有前导精度和小数秒精度，则用逗号分隔。 如果 "秒" 字段的小数部分精度为秒，则它还必须具有前导精度。  
  
-   当间隔由前导字段和尾随字段组成时，前导字段为一个非秒字段，该字段可能伴随括号中的间隔前导字段精度。 尾随字段可以是不是第二个字段，也可以是在括号内带有时间间隔分数秒精度的第二个字段。  
  
 *值* 中的时间间隔字符串用单引号引起来。 它可以是年月文本或日期时间文本。 *值* 中的字符串格式由以下规则确定：  
  
-   字符串包含隐含的每个字段的十进制值 \<*interval* *qualifier*> 。  
  
-   如果间隔精度包含字段 YEAR 和 MONTH，则这些字段的值将用减号分隔。  
  
-   如果间隔精度包含字段 DAY 和 HOUR，则这些字段的值由空格分隔。  
  
-   如果间隔精度包含字段 HOUR，而 (分钟和秒) ，则这些字段的值以冒号分隔。  
  
-   如果间隔精度包含第二个字段，并且表示或隐式秒精度为非零值，则在第二个小数部分的第一个数字之前的字符是句点。  
  
-   字段的长度不能超过两位数，只不过：  
  
    -   "前导" 字段的值可以是所表达或隐含的间隔的精度。  
  
    -   第二个字段的小数部分的长度可以是表示的或隐式的秒精度。  
  
    -   尾随字段遵循公历的一般限制。  (参阅 [公历的约束](../../../odbc/reference/appendixes/constraints-of-the-gregorian-calendar.md)。 )   
  
 下表列出了 ODBC escape 子句中包含的时间间隔的有效间隔文本示例。 Escape 子句的语法如下所示：  
  
> [!NOTE]  
>  *{间隔标记间隔-字符串间隔-限定符}*  
  
|文本转义子句|指定的间隔|  
|---------------------------|------------------------|  
|{INTERVAL "326" 年 (4) }|指定326年的间隔。 间隔前导精度为4。|  
|{INTERVAL "326" 月 (3) }|指定326个月的间隔。 间隔前导精度为3。|  
|{INTERVAL "3261" DAY (4) }|指定间隔为3261天。 间隔前导精度为4。|  
|{INTERVAL "163" 小时 (3) }|指定间隔为163天。 间隔前导精度为3。|  
|{INTERVAL "163" 分钟 (3) }|指定163分钟的间隔。 间隔前导精度为3。|  
|{INTERVAL "223.16" SECOND (3，2) }|指定间隔为223.16 秒。 间隔前导精度为3，秒精度为2。|  
|{INTERVAL "163-11" YEAR (3) 到月}|指定163年和11个月的间隔。 间隔前导精度为3。|  
|{INTERVAL "163 12" DAY (3) 到 HOUR}|指定一个163天到12小时的间隔。 间隔前导精度为3。|  
|{INTERVAL "163 12:39" DAY (3) 到分钟}|指定163天、12小时和39分钟的间隔。 间隔前导精度为3。|  
|{INTERVAL "163 12：39： 59.163" DAY (3) 到第二 (3) }|指定163天、12小时、39分钟和59.163 秒的间隔。 间隔前导精度为3，秒精度为3。|  
|{INTERVAL "163:39" 小时 (3) 到分钟}|指定163小时和39分钟的间隔。 间隔前导精度为3。|  
|{INTERVAL "163：39： 59.163 ' HOUR (3) 到第二 (4) }|指定163小时、39分钟和59.163 秒的间隔。 间隔前导精度为3，秒精度为4。|  
|{INTERVAL "163： 59.163" MINUTE (3) 到第二 (5) }|指定一个163分钟到59.163 秒的间隔。 间隔前导精度为3，秒精度为5。|  
|{INTERVAL-"16 23：39： 56.23" DAY TO SECOND}|指定减去16天、23小时、39分钟和56.23 秒的间隔。 隐含的前导精度是2，隐含的秒精度是6。|  
  
 下表列出了无效间隔文本的示例：  
  
|文本转义子句|无效原因|  
|---------------------------|------------------------|  
|{INTERVAL "163" 小时 (2) }|间隔前导精度是2，但前导字段的值为163。|  
|{INTERVAL "223.16" SECOND (2，2) }<br /><br /> {INTERVAL "223.16" SECOND (3，1) }|在第一个示例中，前导精度太小，第二个示例中的秒精度太小。|  
|{INTERVAL "223.16" SECOND}<br /><br /> {INTERVAL "223" 年}|由于未指定前导精度，因此它的默认值为2，这太小，无法容纳指定的文本。|  
|{INTERVAL "22.1234567" SECOND}|秒精度未指定，因此默认值为6。 该文本在小数点后有7位数字。|  
|{INTERVAL "163-13" YEAR (3) 到月}<br /><br /> {INTERVAL "163 65" DAY (3) 到 HOUR}<br /><br /> {INTERVAL "163 62:39" DAY (3) 到分钟}<br /><br /> {INTERVAL "163 12：125： 59.163" DAY (3) 到第二 (3) }<br /><br /> {INTERVAL "163:144" 小时 (3) 到分钟}<br /><br /> {INTERVAL "163：567： 234.163 ' HOUR (3) 到第二 (4) }<br /><br /> {INTERVAL "163： 591.163" MINUTE (3) 到第二 (5) }|尾随字段不遵循公历的规则。|
