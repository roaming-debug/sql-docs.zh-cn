---
description: date (Transact-SQL)
title: date (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- date_TSQL
- date
dev_langs:
- TSQL
helpviewer_keywords:
- data types [SQL Server], date
- date and time [SQL Server], date
- dates [SQL Server], data types
- date data type [SQL Server]
- data types [SQL Server], date and time
ms.assetid: c963e8b4-5a85-4bd0-9d48-3f8da8f6516b
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 68268a384ab4d89944a3c26322555326948629f2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97408053"
---
# <a name="date-transact-sql"></a>date (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

定义 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的日期。

## <a name="date-description"></a>date 说明
  
|properties|值|  
|--------------|-----------|  
|语法|**date**|  
|使用情况|DECLARE \@MyDate date<br /><br /> CREATE TABLE Table1 ( Column1 date)|  
|默认的字符串文字格式<br /><br /> （用于下级客户端）|YYYY-MM-DD<br /><br /> 有关详细信息，请参阅后面的“下级客户端的向后兼容性”部分。|  
|范围|0001-01-01 到 9999-12-31（对于 Informatica，为 1582-10-15 到 9999-12-31）<br /><br /> 公元 1 年 1 月 1 日（公历纪元）到公元 9999 年 12 月 31 日（对于 Informatica，公元 1582 年 10 月 15 日到公元 9999 年 12 月 31 日）|  
|各元素的范围|YYYY 是表示年份的四位数字，范围为从 0001 到 9999。 对于 Informatica，YYYY 限为 1582 年到 9999 年。<br /><br /> MM 是表示指定年份中的月份的两位数字，范围为从 01 到 12。<br /><br /> DD 是表示指定月份几号的两位数字，介于 01 和 31 之间（具体视月份而定）。|  
|字符长度|10 位|  
|精度、小数位数|10, 0|  
|存储大小|固定 3 个字节|  
|存储结构|1、3 字节整数存储日期。|  
|精确度|一天|  
|默认值|1900-01-01<br /><br /> 此值用于从 time 隐式转换到 datetime2 或 datetimeoffset 时追加的日期部分  。|  
|日历|公历|  
|用户定义的秒的小数部分精度|否|  
|时区偏移量感知和保留|否|  
|夏时制感知|否|  
  
## <a name="supported-string-literal-formats-for-date"></a>date 支持的字符串文字格式
以下各表显示了适用于 date 数据类型的有效字符串文字格式。
  
|Numeric|说明|  
|-------------|-----------------|  
|mdy<br /><br /> [m]m/dd/[yy]yy<br /><br /> [m]m-dd-[yy]yy<br /><br /> [m]m.dd.[yy]yy<br /><br /> myd<br /><br /> mm/[yy]yy/dd<br /><br /> mm-[yy]yy/dd<br /><br /> [m]m.[yy]yy.dd<br /><br /> dmy<br /><br /> dd/[m]m/[yy]yy<br /><br /> dd-[m]m-[yy]yy<br /><br /> dd.[m]m.[yy]yy<br /><br /> dym<br /><br /> dd/[yy]yy/[m]m<br /><br /> dd-[yy]yy-[m]m<br /><br /> dd.[yy]yy.[m]m<br /><br /> ymd<br /><br /> [yy]yy/[m]m/dd<br /><br /> [yy]yy-[m]m-dd<br /><br /> [yy]yy-[m]m-dd|[m]m、dd 和 [yy]yy 在字符串中表示月、日和年，使用斜线 (/)、连字符 (-) 或句点 (.) 作为分隔符。<br /><br /> 仅支持四位或两位数字表示的年份。 请尽可能使用以四位数字表示的年份。 若要从 0001 到 9999 之间指定一个整数来表示缩略形式的年份，以将两位数的年份解释为四位数的年份，请使用[配置“two digit year cutoff”（两位数年份截止）服务器配置选项](../../database-engine/configure-windows/configure-the-two-digit-year-cutoff-server-configuration-option.md)。<br /><br /> **注意！** 对于 Informatica，YYYY 限为 1582 年到 9999 年。<br /><br /> 一个比截止年份的后两位数字小或者与其相等的两位数年份与该截止年份处于同一个世纪。 而一个比截止年份的后两位数字大的两位数年份所处的世纪为截止年份所处世纪的上一个世纪。 例如，如果“两位数年份截止”为默认值 2049，则两位数年份 49 被解释为 2049 年，而两位数年份 50 被解释为 1950 年。<br /><br /> 默认日期格式由当前语言设置决定。 可以通过使用 [SET LANGUAGE](../../t-sql/statements/set-language-transact-sql.md) 和 [SET DATEFORMAT](../../t-sql/statements/set-dateformat-transact-sql.md) 语句更改日期格式。<br /><br /> date 不支持 ydm 格式。|  
  
|字母|说明|  
|------------------|-----------------|  
|mon [dd][,] yyyy<br /><br /> mon dd[,] [yy]<br /><br /> mon yyyy [dd]<br /><br /> [dd] mon[,] yyyy<br /><br /> dd mon[,][yy]yy<br /><br /> dd [yy]yy mon<br /><br /> [dd] yyyy mon<br /><br /> yyyy mon [dd]<br /><br /> yyyy [dd] mon|mon 表示采用当前语言的完整月份名称或月份缩写。 逗号是可选的，且忽略大小写。<br /><br /> 为避免不确定性，请使用四位数年份。<br /><br /> 如果没有指定日，则默认为当月第一天。|  
  
|ISO 8601|说明|  
|--------------|----------------|  
|YYYY-MM-DD<br /><br /> YYYYMMDD|与 SQL 标准相同。 此格式是唯一定义为国际标准的格式。|  
  
|未分隔的|说明|  
|-----------------|-----------------|  
|[yy]yymmdd<br /><br /> yyyy[mm][dd]|可用四位、六位或八位数字来指定 date 数据。 六位或八位字符串始终解释为 ymd。 月和日必须始终是两位数字。 四位字符串被解释为年。|  
  
|ODBC|说明|  
|----------|-----------------|  
|{ d 'yyyy-mm-dd' }|特定于 ODBC API。|  
  
|W3C XML 格式|说明|  
|--------------------|-----------------|  
|yyyy-mm-ddTZD|支持使用 XML/SOAP。<br /><br /> TZD 是时区指示符（Z 或 +hh:mm 或 -hh:mm）：<br /><br /> -   hh:mm 表示时区偏移量。 hh 是两位数，范围为 0 到 14，它表示时区偏移量中的小时数。<br />-   MM 是两位数，范围为 0 到 59，它表示时区偏移量中的额外分钟数。<br />-   +（加）或 -（减）是时区偏移量中必须包含的符号。 这两个符号表示，为了得出本地时间，将用协调世界时 (UTC) 加上或减去时区偏移量。 时区偏移量的有效范围为 -14:00 到 +14:00。|  
  
## <a name="ansi-and-iso-8601-compliance"></a>对 ANSI 和 ISO 8601 的遵从性  
date 符合公历的 ANSI SQL 标准定义：“备注 85 - Datetime 数据类型将允许存储日期范围从 0001–01–01 CE 到 9999–12–31 CE 之间的公历格式的日期。”
  
用于下级客户端的默认字符串文本格式遵循 SQL 标准格式（定义为 YYYY-MM-DD）。 该格式与 ISO 8601 对 DATE 的定义相同。
  
> [!NOTE]  
>  对于 Informatica，范围限为 1582-10-15（公元 1582 年 10 月 15 日）到 9999-12-31（公元 9999 年 12 月 31 日）。  
  
## <a name="backward-compatibility-for-down-level-clients"></a>下级客户端的向后兼容性
某些下级客户端不支持 time、date、datetime2 和 datetimeoffset 数据类型。 下表显示了 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上级实例与下级客户端之间的类型映射。
  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型|传递给下级客户端的默认字符串文字格式|下级 ODBC|下级 OLEDB|下级 JDBC|下级 SQLCLIENT|  
| --- | --- | --- | --- | --- | --- |
|**time**|hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**date**|YYYY-MM-DD|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetime2**|YYYY-MM-DD hh:mm:ss[.nnnnnnn]|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
|**datetimeoffset**|YYYY-MM-DD hh:mm:ss[.nnnnnnn] [+&#124;-]hh:mm|SQL_WVARCHAR 或 SQL_VARCHAR|DBTYPE_WSTR 或 DBTYPE_STR|Java.sql.String|String 或 SqString|  
  
## <a name="converting-date-and-time-data"></a>转换日期和时间数据
转换为日期和时间数据类型时，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 拒绝它无法识别为日期或时间的所有值。 有关对日期和时间数据使用 CAST 和 CONVERT 函数的信息，请参阅 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)。  
  
### <a name="converting-date-to-other-date-and-time-types"></a>将 date 转换为其他日期和时间类型

此部分介绍了将 date 数据类型转换为其他日期和时间数据类型时会发生什么。
  
转换到 time(n) 时，转换失败，并引发错误消息 206：“操作数类型冲突: date 与 time 不兼容”。
  
如果转换到 datetime，会复制日期。 下面的代码显示将 `date` 值转换为 `datetime` 值的结果。
  
```sql
DECLARE @date date= '12-10-25';  
DECLARE @datetime datetime= @date;  
  
SELECT @date AS '@date', @datetime AS '@datetime';  
  
--Result  
--@date      @datetime  
------------ -----------------------  
--2025-12-10 2025-12-10 00:00:00.000  
--  
--(1 row(s) affected)  
```  
  
如果转换为 smalldatetime，date 值位于 [smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md) 范围内，日期部分被复制，且时间部分设置为 00:00:00.000。 当 date 值不在 smalldatetime 值范围之内时，引发错误消息 242 ：“将 date 数据类型转换为 smalldatetime 数据类型导致值超出范围”；且 smalldatetime 值设置为 NULL。 下面的代码显示将 `date` 值转换为 `smalldatetime` 值的结果。
  
```sql
DECLARE @date date= '1912-10-25';  
DECLARE @smalldatetime smalldatetime = @date;  
  
SELECT @date AS '@date', @smalldatetime AS '@smalldatetime';  
  
--Result  
--@date      @smalldatetime  
------------ -----------------------  
--1912-10-25 1912-10-25 00:00:00  
--  
--(1 row(s) affected)  
```  
  
转换到 datetimeoffset(n) 时，会复制日期，且时间设置为 00:00.0000000 +00:00。 下面的代码显示将 `date` 值转换为 `datetimeoffset(3)` 值的结果。
  
```sql
DECLARE @date date = '1912-10-25';  
DECLARE @datetimeoffset datetimeoffset(3) = @date;  
  
SELECT @date AS '@date', @datetimeoffset AS '@datetimeoffset';  
  
--Result  
--@date      @datetimeoffset  
------------ ------------------------------  
--1912-10-25 1912-10-25 00:00:00.000 +00:00  
--  
--(1 row(s) affected)  
```  
  
转换到 datetime2(n) 时，会复制日期部分，时间部分设置为“00:00.000000”。 下面的代码显示将 `date` 值转换为 `datetime2(3)` 值的结果。
  
```sql
DECLARE @date date = '1912-10-25'  
DECLARE @datetime2 datetime2(3) = @date;  
  
SELECT @date AS '@date', @datetime2 AS '@datetime2(3)';  
  
--Result  
--@date      @datetime2(3)  
------------ -----------------------  
--1912-10-25 1912-10-25 00:00:00.000  
--  
--(1 row(s) affected)  
```  
  
### <a name="converting-string-literals-to-date"></a>将字符串文字转换为 date
如果字符串所有部分的格式都有效，可以将字符串文本转换为日期和时间类型。 否则，将引发运行时错误。 如果从日期和时间类型隐式或显式转换为字符串文本，但未指定样式，将采用当前会话的默认格式。 下表显示用于将字符串文字转换为 date 数据类型的规则。
  
|输入字符串文字|**date**|  
|---|---|
|ODBC DATE|ODBC 字符串文字映射到 datetime 数据类型。 任何从 ODBC DATETIME 文本到 date 类型的赋值操作，都会导致在 datetime 与转换规则定义的类型之间进行隐式转换。|  
|ODBC TIME|请参阅前面的 ODBC DATE 规则。|  
|ODBC DATETIME|请参阅前面的 ODBC DATE 规则。|  
|仅 DATE|无庸赘述|  
|仅 TIME|提供默认值。|  
|仅 TIMEZONE|提供默认值。|  
|DATE + TIME|使用输入字符串的 DATE 部分。|  
|DATE + TIMEZONE|不允许。|  
|TIME + TIMEZONE|提供默认值。|  
|DATE + TIME + TIMEZONE|将使用本地 DATETIME 的 DATE 部分。|  
  
## <a name="examples"></a>示例  
下例比较了将一个字符串分别转换为各种 date 和 time 数据类型时所产生的结果。
  
```sql
SELECT   
     CAST('2007-05-08 12:35:29. 1234567 +12:15' AS time(7)) AS 'time'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS date) AS 'date'   
    ,CAST('2007-05-08 12:35:29.123' AS smalldatetime) AS   
        'smalldatetime'   
    ,CAST('2007-05-08 12:35:29.123' AS datetime) AS 'datetime'   
    ,CAST('2007-05-08 12:35:29. 1234567 +12:15' AS datetime2(7)) AS   
        'datetime2'  
    ,CAST('2007-05-08 12:35:29.1234567 +12:15' AS datetimeoffset(7)) AS   
        'datetimeoffset';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
|数据类型|输出|  
|---------------|------------|  
|**time**|12:35:29. 1234567|  
|**date**|2007-05-08|  
|**smalldatetime**|2007-05-08 12:35:00|  
|**datetime**|2007-05-08 12:35:29.123|  
|**datetime2**|2007-05-08 12:35:29. 1234567|  
|**datetimeoffset**|2007-05-08 12:35:29.1234567 +12:15|  

在 SQL Server 2008 中首次引入。

## <a name="see-also"></a>另请参阅
[CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)
  
  
