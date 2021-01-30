---
description: 字符串函数
title: 字符串函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], string functions
- string functions [ODBC]
ms.assetid: 270f669e-8aab-4db0-95a4-f2b3c69538b3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fe6c50c7ad96f1025b27f60e2de74ba31f60698f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202513"
---
# <a name="string-functions"></a>字符串函数
下表列出了字符串操作函数。 应用程序可以通过使用 SQL_STRING_FUNCTIONS *信息类型* 调用 **SQLGetInfo** 来确定驱动程序支持的字符串函数。  
  
## <a name="remarks"></a>备注  
 表示为 *string_exp* 的参数可以是列的名称、 *字符字符串* 或其他标量函数的结果，其中，基础数据类型可表示为 SQL_CHAR、SQL_VARCHAR 或 SQL_LONGVARCHAR。  
  
 表示为 *character_exp* 的参数是可变长度字符串。  
  
 表示为 *开始*、 *长度* 或 *计数* 的参数可以是 *数字文本* 或其他标量函数的结果，其中，基础数据类型可表示为 SQL_TINYINT、SQL_SMALLINT 或 SQL_INTEGER。  
  
 此处列出的字符串函数从1开始;也就是说，字符串中的第一个字符是字符1。  
  
 ODBC 3.0 中添加了 BIT_LENGTH、CHAR_LENGTH、CHARACTER_LENGTH、OCTET_LENGTH 和位置字符串标量函数，以使其符合 SQL-92。  
  
|功能|说明|  
|--------------|-----------------|  
| (ODBC) 1.0 _string_exp_ **)** 的 **ASCII (**|以整数形式返回 *string_exp* 最左侧字符的 ASCII 代码值。|  
|**BIT_LENGTH (** _STRING_EXP_ **)**  (ODBC 3.0) |返回字符串表达式的长度（以位为单位）。<br /><br /> 仅对字符串数据类型有效，因此将不会隐式地将 *string_exp* 转换为字符串，而是返回给定的任何数据类型的 (内部) 大小。|  
|**CHAR (** _代码_ **)**  (ODBC 1.0) |返回具有由 *代码* 指定的 ASCII 代码值的字符。 *代码* 的值应介于0到255之间;否则，返回值取决于数据源。|  
|**CHAR_LENGTH (** _STRING_EXP_ **)**  (ODBC 3.0) |如果字符串表达式的数据类型为，则返回字符串表达式的长度（以字符为字符）; 否则返回。否则，将返回字符串表达式的长度（以字节为单位） (最小整数不小于位数除以 8) 。  (此函数与 CHARACTER_LENGTH 函数相同。 ) |  
|**CHARACTER_LENGTH (** _STRING_EXP_ **)**  (ODBC 3.0) |如果字符串表达式的数据类型为，则返回字符串表达式的长度（以字符为字符）; 否则返回。否则，将返回字符串表达式的长度（以字节为单位） (最小整数不小于位数除以 8) 。  (此函数与 CHAR_LENGTH 函数相同。 ) |  
|**CONCAT (** _string_exp1_，_string_exp2_ **)** (ODBC 1.0) |返回一个字符串，该字符串是将 *string_exp2* 连接到 *string_exp1* 的结果。 生成的字符串依赖于 DBMS。 例如，如果 *string_exp1* 表示的列包含 null 值，则 DB2 将返回 null，但 SQL Server 返回非 NULL 字符串。|  
|**差异 (** _string_exp1_，_string_exp2_ **)** (ODBC 2.0) |返回一个整数值，该值指示对于 *string_exp1* 和 *string_exp2*，SOUNDEX 函数返回的值之间的差异。|  
|**插入 (** _string_exp1_、 *start*、 *length*、 _string_exp2_ **)** (ODBC 1.0) |返回一个字符串，其中的 *长度* 字符已从 *string_exp1* 中删除，从 *开始*，开始， *string_exp2* 已插入 *string_exp，* 从 *开始*。|  
|**LCASE (** _STRING_EXP_ **)** (ODBC 1.0) |返回一个字符串，该字符串等于 *string_exp* 中的所有大写字符都转换为小写的字符串。|  
|**左 (** _string_exp_， _计数_**)** (ODBC 1.0) |返回 *string_exp* 最左端的 *计数* 字符。|  
| (ODBC 1.0 string_exp **(长度**  **)**) |返回 string_exp 中的字符数 *，* 不包括尾随空格。<br /><br /> **长度** 仅接受字符串。 因此，会将 *string_exp* 隐式转换为字符串，并返回此字符串的长度 (不是数据类型) 的内部大小。|  
|**找到 (** _string_exp1_， *string_exp2*[， *start*]**)** (ODBC 1.0) |返回 *string_exp1* 在 *string_exp2* 中的第一个匹配项的起始位置。 搜索第一次出现的 *string_exp1* 将以 *string_exp2* 中的第一个字符位置开始，除非指定了可选参数 *start*。 如果指定了 *start* ，搜索将以 *start* 值所指示的字符位置开始。 *String_exp2* 中的第一个字符位置由值1指示。 如果在 *string_exp2* 中找不到 *string_exp1* ，则返回值0。<br /><br /> 如果应用程序可以调用带有 *string_exp1*、 *string_exp2* 和 *start* 参数的定位标量函数，则使用 SQL_STRING_FUNCTIONS *选项* 调用 **SQLGetInfo** 时，驱动程序将返回 SQL_FN_STR_LOCATE。 如果应用程序只能调用带有 *string_exp1* 和 *STRING_EXP2* 参数的定位标量函数，则使用 SQL_STRING_FUNCTIONS *选项* 调用 **SQLGetInfo** 时，驱动程序将返回 SQL_FN_STR_LOCATE_2。 支持调用带有两个或三个参数的定位函数的驱动程序同时返回 SQL_FN_STR_LOCATE 和 SQL_FN_STR_LOCATE_2。|  
|**LTRIM (** _STRING_EXP_ **)** (ODBC 1.0) |返回删除了前导空格的 *string_exp* 字符。|  
|**OCTET_LENGTH (** _STRING_EXP_ **)** (ODBC 3.0) |返回字符串表达式的长度（以字节为单位）。 结果为不小于位数除以 8 所得数的最小整数。<br /><br /> 仅对字符串数据类型有效，因此将不会隐式地将 *string_exp* 转换为字符串，而是返回给定的任何数据类型的 (内部) 大小。|  
|**将 (** _character_exp_ 位置 _character_exp_ **)** (ODBC 3.0) |返回第二个字符表达式中第一个字符表达式的位置。 结果是具有实现定义的精度和小数位数为0的精确数值。|  
|**重复 (** _string_exp，_  **)** (ODBC 1.0) |返回由 *string_exp* 重复 *计数* 时间组成的字符串。|  
|**替换 (** _string_exp1_， *string_exp2* _string_exp3_ **)** (ODBC 1.0) |搜索 *string_exp2* *string_exp1* foroccurrences，并将替换为 *string_exp3*。|  
|**RIGHT (** _string_exp_，  **)** (ODBC 1.0) |返回 *string_exp* 最右侧的 *计数* 字符。|  
|**RTRIM (** _STRING_EXP_ **)** (ODBC 1.0) |返回删除了尾随空格 *string_exp* 的字符。|  
|_String_exp_ **)** 的 **SOUNDEX (** (ODBC 2.0) |返回一个与数据源相关的字符串，该字符串表示 *string_exp* 中单词的声音。 例如，SQL Server 返回一个4位数的 SOUNDEX 代码;Oracle 返回每个单词的拼音表示形式。|  
| (ODBC 2.0 **)** **(** _计数_ 的空间) |返回由 *count* 空格组成的字符串。|  
|**子字符串 (** _string_exp_、 *START*、length **)** (ODBC 1.0) |返回从 *string_exp* 派生的字符串，从 " *start* " 中指定的字符位置开始， *长度* 字符。|  
|**UCASE (** _STRING_EXP_ **)** (ODBC 1.0) |返回一个字符串，该字符串等于 *string_exp* 中的所有小写字符均转换为大写的字符串。|
