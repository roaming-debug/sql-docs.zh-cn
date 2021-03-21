---
title: 类型支持，ODBC 日期和时间
description: 了解支持 SQL Server 日期和时间数据类型的 ODBC 类型，包括字符串、文本和数据结构的数据类型映射和格式。
ms.custom: ''
ms.date: 12/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- date/time [ODBC], data type support
- ODBC, date/time improvements
ms.assetid: 8e0d9ba2-3ec1-4680-86e3-b2590ba8e2e9
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8acb8060cb543d264f7237bb720d54ce5b9daedd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756117"
---
# <a name="data-type-support-for-odbc-date-and-time-improvements"></a>针对 ODBC 日期/时间改进的数据类型支持
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题提供有关支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日期和时间数据类型的 ODBC 类型的信息。  
  
## <a name="data-type-mapping-in-parameters-and-resultsets"></a>参数和结果集中的数据类型映射  
 除了 ODBC 数据类型（SQL_TYPE_TIMESTAMP 和 SQL_TIMESTAMP）以外，在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 中还添加了两种新数据类型以公开新的服务器类型：  
  
-   SQL_SS_TIME2  
  
-   SQL_SS_TIMESTAMPOFFSET  
  
 下表显示完整的服务器类型映射。 注意，该表的某些单元格包含两个条目；在这些情况下，第一个是针对 ODBC 3.0 的值，第二个是针对 ODBC 2.0 的值。  
  
|SQL Server 数据类型|SQL 数据类型|值|  
|--------------------------|-------------------|-----------|  
|Datetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
|Smalldatetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
|日期|SQL_TYPE_DATE<br /><br /> SQL_DATE|91 (sql .h) <br /><br /> 9 (sqltypes.h) |  
|时间|SQL_SS_TIME2|-154 (SQLNCLI.MSI) |  
|DatetimeOFFSET|SQL_SS_TIMESTAMPOFFSET|-155 (SQLNCLI.h)|  
|Datetime2|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|93 (sql.h)<br /><br /> 11 (sqlext.h)|  
||||

 下表列出了相应的结构和 ODBC C 类型。 由于 ODBC 不允许驱动程序定义的 C 类型，因此将 SQL_C_BINARY 作为二进制结构用于 time 和 datetimeoffset。  
  
|SQL 数据类型|内存布局|默认 C 数据类型|值 (sqlext.h)|  
|-------------------|-------------------|-------------------------|------------------------|  
|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|SQL_TIMESTAMP_STRUCT<br /><br /> TIMESTAMP_STRUCT|SQL_C_TYPE_TIMESTAMP<br /><br /> SQL_C_TIMESTAMP|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|  
|SQL_TYPE_DATE<br /><br /> SQL_DATE|SQL_DATE_STRUCT<br /><br /> DATE_STRUCT|SQL_C_TYPE_DATE<br /><br /> SQL_C_DATE|SQL_TYPE_DATE<br /><br /> SQL_DATE|  
|SQL_SS_TIME2|SQL_SS_TIME2_STRUCT|SQL_C_SS_TIME2<br /><br /> SQL_C_BINARY（ODBC 3.5 和更早版本）|0x4000 (sqlncli.h)<br /><br /> SQL_BINARY (-2)|  
|SQL_SS_TIMESTAMPOFFSET|SQL_SS_TIMESTAMPOFFSET_STRUCT|SQL_C_SS_TIMESTAMPOFFSET<br /><br /> SQL_C_BINARY（ODBC 3.5 和更早版本）|0x4001 (sqlncli.h)<br /><br /> SQL_BINARY (-2)|  
|||||

 指定 SQL_C_BINARY 绑定时，将执行对齐检查，并对不正确的对齐报错。 该错误的 SQLSTATE 将是 IM016，并显示消息“结构对齐不正确”。  
  
## <a name="data-formats-strings-and-literals"></a>数据格式：字符串和文字  
 下表显示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型、ODBC 数据类型和 ODBC 字符串文字之间的映射。  
  
|SQL Server 数据类型|ODBC 数据类型|客户端转换的字符串格式|  
|--------------------------|--------------------|------------------------------------------|  
|datetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|'yyyy-mm-dd hh:mm:ss[.999]'<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对于 Datetime 最多支持三位数字的秒小数部分。|  
|Smalldatetime|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|'yyyy-mm-dd hh:hh:ss'<br /><br /> 此数据类型精确到 1 分钟。 秒部分在输出中将为零，在输入中由服务器进行四舍五入。|  
|日期|SQL_TYPE_DATE<br /><br /> SQL_DATE|'yyyy-mm-dd'|  
|时间|SQL_SS_TIME2|'hh:mm:ss[.9999999]'<br /><br /> 可以选择指定最多达到七位数字的秒小数部分。|  
|Datetime2|SQL_TYPE_TIMESTAMP<br /><br /> SQL_TIMESTAMP|"yyyy-mm-dd hh： mm： ss [. 维]"<br /><br /> 可以选择指定最多达到七位数字的秒小数部分。|  
|DatetimeOFFSET|SQL_SS_TIMESTAMPOFFSET|'yyyy-mm-dd hh:mm:ss[.9999999] +/- hh:mm'<br /><br /> 可以选择指定最多达到七位数字的秒小数部分。|  
||||

 日期/时间文字的 ODBC 转义序列没有更改。  
  
 结果中秒的小数部分始终使用点 (.)，而不是冒号 (:)。  
  
 返回到应用程序的字符串值始终与给定列的长度相同。 年、月、日、小时、分钟和秒部分将以前置零填充至其最大宽度，并且 datetime 值中的日期和时间之间有一个空格。 在 datetimeoffset 值的时间和时区偏移量之间也有一个空格。 时区偏移量前面始终有一个符号；如果偏移量是零，则该符号是加号 (+)。 如有必要，秒的小数部分将填充尾随零，直到达到列的定义精度。 对于 datetime 列，秒的小数部分有三位数。 对于 smalldatetime 列，秒部分没有小数位，并且秒将始终是零。  
  
 空字符串不是有效的日期/时间文字，它不表示 NULL 值。 将空字符串转换到日期/时间值的尝试将导致 SQLState 22018 错误并显示消息“为转换指定的字符值无效。”。  
  
 从字符串参数进行转换应当得到相同格式的字符串，不过，包含零小时和零分钟的时区的符号可以是加号或减号，并且允许秒的小数部分的尾随零最多可达 9 位数。 可以使用小数点结束时间部分，不带秒小数部分。  
  
 当前，驱动程序允许标点字符前后有额外的空白，并且时间和时区偏移量之间的空格是可选的。 但是，这可能在未来版本中更改；应用程序不应当依赖于当前行为。  
  
## <a name="data-formats-data-structures"></a>数据格式：数据结构  
 在下面描述的结构中，ODBC 指定了来自公历的以下约束：  
  
-   “月”范围为从 1 到 12。  
  
-   “日”字段范围为 1 到所在月包含的天数，必须与“年”和“月”字段一致，考虑闰年。  
  
-   “小时”范围为从 0 到 23。  
  
-   “分钟”范围为从 0 到 59。  
  
-   秒范围是 0 到 61.9(n)。 这允许最多两个闰秒以便与恒星时间保持同步。  
  
     注意，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不允许闰秒，因此大于 59 的秒值将导致服务器错误。  
  
 以下现有 ODBC 结构的实现已修改，以支持新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 日期和时间数据类型。 不过未更改定义。  
  
-   DATE_STRUCT  
  
-   TIME_STRUCT  
  
-   TIMESTAMP_STRUCT  
  
 还有两个新结构：  
  
-   SQL_SS_TIME2_STRUCT  
  
-   SQL_SS_TIMESTAMPOFFSET_STRUCT  
  
### <a name="sql_ss_time2_struct"></a>SQL_SS_TIME2_STRUCT  
 此结构在 32 位和 64 位操作系统中都填充到 12 个字节。  
  
```cpp
typedef struct tagSS_TIME2_STRUCT {  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;  
} SQL_SS_TIME2_STRUCT;  
```  
  
### <a name="sql_ss_timestampoffset_struct"></a>SQL_SS_TIMESTAMPOFFSET_STRUCT  
  
```cpp
typedef struct tagSS_TIMESTAMPOFFSET_STRUCT {  
   SQLSMALLINT year;  
   SQLUSMALLINT month;  
   SQLUSMALLINT day;  
   SQLUSMALLINT hour;  
   SQLUSMALLINT minute;  
   SQLUSMALLINT second;  
   SQLUINTEGER fraction;  
   SQLSMALLINT timezone_hour;  
   SQLSMALLINT timezone_minute;  
} SQL_SS_TIMESTAMPOFFSET_STRUCT;  
```  
  
 如果 **timezone_hour** 为负数，则 **timezone_minute** 必须为负数或零。 如果 **timezone_hour** 为正，则 **timezone_minute** 必须为正数或零。 如果 **timezone_hour** 为零，则 **timezone_minute** 的范围可以是从-59 到 + 59 的任何值。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC&#41;&#40;的日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
