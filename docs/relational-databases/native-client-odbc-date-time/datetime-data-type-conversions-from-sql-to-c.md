---
description: 由 SQL 到 C 的 datetime 数据类型转换
title: 从 SQL 到 C 的转换 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- conversions [ODBC], SQL to C
ms.assetid: 059431e2-a65c-4587-ba4a-9929a1611e96
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c7434c34d2e39ae85ecc11ebf5603f825672a919
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438536"
---
# <a name="datetime-data-type-conversions-from-sql-to-c"></a>由 SQL 到 C 的 datetime 数据类型转换
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  下表列出了从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日期/时间类型转换为 C 类型时要注意的问题。  
  
## <a name="conversions"></a>转换  
  
|   | SQL_C_DATE | SQL_C_TIME | SQL_C_TIMESTAMP | SQL_C_SS_TIME2 | SQL_C_SS_TIMESTAMPOFFSET | SQL_C_BINARY | SQL_C_CHAR | SQL_C_WCHAR |
| - | ---------- | ---------- | --------------- | -------------- | ------------------------ | ------------ | ---------- | ----------- |
| **SQL_CHAR** |2、3、4、5|2、3、6、7、8|2、3、9、10、11|2、3、6、7|2、3、9、10、11|1|1|1|  
| **SQL_WCHAR** |2、3、4、5|2、3、6、7、8|2、3、9、10、11|2、3、6、7|2、3、9、10、11|1|1|1|  
| **SQL_TYPE_DATE** |OK|12|13|12|13、23|14|16|16|  
| **SQL_SS_TIME2** |12|8|15|OK|10，23|17|16|16|  
| **SQL_TYPE_TIMESTAMP** |18|7、8|OK|7|23|19|16|16|  
| **SQL_SS_TIMESTAMPOFFSET** |18、22|7、8、20|20|7、20|OK|21|16|16|  
  
## <a name="key-to-symbols"></a>符号含义  
  
|符号|含义|  
|------------|-------------|  
|OK|无转换问题。|  
|1|应用 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 之前的规则。|  
|2|忽略前导空格和尾随空格。|  
|3|字符串解析为日期、时间、时区或时区偏移量，且秒的小数部分允许多达 9 位。 如果解析时区偏移量，则时间将转换为客户端时区。 如果在此转换过程中出现错误，将生成包含 SQLSTATE 22018 和消息 "日期时间字段溢出" 的诊断记录。|  
|4|如果该值不是有效的日期、时间戳或时间戳偏移量值，则生成具有 SQLSTATE 22018 和消息“为转换指定的字符值无效”的诊断记录。|  
|5|如果时间为非零，则生成具有 SQLSTATE 01S07 和消息“截断小数部分”的诊断记录。|  
|6|如果该值不是有效的时间、时间戳或时间戳偏移量值，则生成具有 SQLSTATE 22018 和消息“为转换指定的字符值无效”的诊断记录。|  
|7|忽略日期部分。|  
|8|如果秒的小数部分为非零，则生成具有 SQLSTATE 01S07 和消息“截断小数部分”的诊断记录。|  
|9|如果该值不是有效的日期、时间、时间戳或 timestampoffset 值，则会生成包含 SQLSTATE 22018 的诊断记录和消息 "对于 cast 规范无效的字符值"。|  
|10|如果该值是有效的时间，则日期部分设置为当前客户端的日期。|  
|11|如果该值是有效的日期，则时间设置为零。|  
|12|生成具有 SQLSTATE 07006 和消息“受限制的数据类型属性冲突”的诊断记录。|  
|13|时间设置为零。|  
|14|如果缓冲区不足以容纳 SQL_DATE_STRUCT，则生成具有 SQLSTATE 22003 和消息“数值超出了范围”的诊断记录。|  
|15|日期设置为当前客户端的日期。|  
|16|如果缓冲区不足以容纳转换后的字符串值，只能容纳秒的小数部分，则会发生截断，且生成具有 SQLSTATE 01004 和消息“字符串数据，右端被截断”的诊断记录。 如果缓冲区不足以容纳未截断日期、时间或偏移量部分的字符串值，则生成具有 SQLSTATE 22003 和消息“数值超出了范围”的诊断记录。 请注意，对于日期和时间戳偏移量，由于转换后的字符串最右侧部分不包含秒的小数部分，因此不可能为 SQLSTATE 01004。 因此，任何截断都会造成数据丢失。|  
|17|如果缓冲区不足以容纳 SQL_SS_TIME2_STRUCT，则生成具有 SQLSTATE 22003 和消息“数值超出了范围”的诊断记录。|  
|18|如果时间为非零，则生成具有 SQLSTATE 01S07 和消息“截断小数部分”的诊断记录。|  
|19|如果服务器类型为 datetime 或 smalldatetime，则值对应于 TDS 连网格式，对于 smalldatetime 为 4 字节值，对于 datetime 为 8 字节值。<br /><br /> 如果服务器类型为 datetime2，则按照 SQL_TIMESTAMP_STRUCT 返回值。 如果缓冲区不足以容纳返回值，则生成具有 SQLSTATE 22003 和消息“数值超出了范围”的诊断记录。|  
|20|将时间转换为客户端时区。 如果在此转换过程中发生错误，则生成具有 SQLSTATE 22008 和消息“日期时间字段溢出”的诊断记录。|  
|21|如果缓冲区足以容纳 SQL_SS_TIMESTAMPOFFSET_STRUCT，则按照 SQL_SS_TIMESTAMPOFFSET_STRUCT 返回值。 否则，生成具有 SQLSTATE 22003 和消息“数值超出了范围”的诊断记录。|  
|22|在提取日期之前，将值转换为客户端时区。 这提供了与其他时间戳偏移量类型转换的一致性。 如果在此转换过程中发生错误，则生成具有 SQLSTATE 22008 和消息“日期时间字段溢出”的诊断记录。 这可能会导致日期与通过简单截断获得的值不同。|  
  
 本主题中的表格列出了返回客户端的类型与绑定中的类型之间的转换。 对于输出参数，如果 SQLBindParameter 中指定的服务器类型与服务器上的实际类型不匹配，则服务器将执行隐式转换，并且返回给客户端的类型将与通过 SQLBindParameter 指定的类型匹配。 当服务器的转换规则与上表中列出的规则不同时，这可能会导致意外的转换结果。 例如，在必须提供默认日期时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 1900-1-1，而不是当前日期。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC&#41;&#40;的日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
  
  
