---
description: 与 SQL Server 早期版本的增强日期和时间类型行为 (ODBC)
title: 'SQL 版本中的日期时间 (ODBC) '
ms.custom: ''
ms.date: 12/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- date/time [ODBC], enhanced behavior with earlier SQL Server versions
ms.assetid: cd4e137f-dc5e-4df7-bc95-51fe18c587e0
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0d9f25706641a20a59c01d44b487ef692e9cdbb2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483379"
---
# <a name="enhanced-date-and-time-type-behavior-with-previous-sql-server-versions-odbc"></a>与 SQL Server 早期版本的增强日期和时间类型行为 (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题说明当使用增强的日期和时间功能的客户端应用程序与早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 版本通信时的预期行为，以及使用 Microsoft 数据访问组件、Windows 数据访问组件或早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Native Client 版本的客户端应用程序向支持增强的日期和时间功能的服务器发送命令时的预期行为。  
  
## <a name="down-level-client-behavior"></a>下级客户端行为  
 使用早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] Native Client 版本编译的客户端应用程序将新的日期/时间类型看作 nvarchar 列。 列内容是文本表示形式，如 "数据格式：字符串和文字" 一节中的 "数据 [类型支持 ODBC 日期和时间改进](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md)" 一节中所述。 列大小是为列指定的秒小数精度的最大文字长度。  
  
 目录 API 将返回与返回给客户端的下级数据类型代码（例如 nvarchar）和相关的下级表示法（例如相应的文字格式）一致的元数据。 不过，返回的数据类型名称将为 real [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 类型名称。  
  
 由 SQLDescribeCol、SQLDescribeParam、SQGetDescField 和 SQLColAttribute 返回的语句元数据将返回与所有方面的下级类型一致的元数据，包括类型名称。 此类下级类型的一个示例是 **nvarchar**。  
  
 当下级客户端应用程序针对 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (或更高版本运行时) 服务器上对日期/时间类型进行架构更改时，预期的行为如下所示：  
  
|SQL Server 2005 类型|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]（或更高版本）类型|ODBC 客户端类型|结果转换（SQL 到 C）|参数转换（C 到 SQL）|  
|--------------------------|----------------------------------------------|----------------------|------------------------------------|---------------------------------------|  
|datetime|Date|SQL_C_TYPE_DATE|OK|确定 (1) |  
|||SQL_C_TYPE_TIMESTAMP|时间字段设置为零。|成功 (2)<br /><br /> 如果时间字段非零，则失败。 适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]。|  
||Time(0)|SQL_C_TYPE_TIME|OK|确定 (1) |  
|||SQL_C_TYPE_TIMESTAMP|日期字段设置为当前日期。|成功 (2)<br /><br /> 忽略日期。 如果秒的小数部分非零，则将失败。 适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]。|  
||Time(7)|SQL_C_TIME|失败-时间文本无效。|确定 (1) |  
|||SQL_C_TYPE_TIMESTAMP|失败-时间文本无效。|确定 (1) |  
||Datetime2 (3) |SQL_C_TYPE_TIMESTAMP|OK|确定 (1) |  
||Datetime2 (7) |SQL_C_TYPE_TIMESTAMP|OK|由客户端转换将值舍入到 1/300 秒。|  
|Smalldatetime|Date|SQL_C_TYPE_DATE|OK|OK|  
|||SQL_C_TYPE_TIMESTAMP|时间字段设置为零。|成功 (2)<br /><br /> 如果时间字段非零，则失败。 适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]。|  
||Time(0)|SQL_C_TYPE_TIME|OK|OK|  
|||SQL_C_TYPE_TIMESTAMP|日期字段设置为当前日期。|成功 (2)<br /><br /> 忽略日期。 如果秒的小数部分非零，则失败。<br /><br /> 适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]。|  
||Datetime2(0)|SQL_C_TYPE_TIMESTAMP|OK|OK|  
|||||

## <a name="key-to-symbols"></a>符号含义  
  
|符号|含义|  
|------------|-------------|  
|1|如果已适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]，应继续适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的更新版本。|  
|2|已与 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 一起使用的应用程序在与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的更新版本一起使用时，可能失败。|  
|||

 请注意只考虑了常见的架构更改。 以下是常见的更改：  
  
-   使用新类型，这种情况下在逻辑上应用程序只需要一个日期或时间值。 但是，由于缺乏单独的日期和时间类型，会强制应用程序使用 datetime 或 smalldatetime。  
  
-   使用新类型以获得其他秒小数精度或准确性。  
  
-   转换到 datetime2，因为这是首选的日期和时间数据类型。  
  
### <a name="column-metadata-returned-by-sqlcolumns-sqlprocedurecolumns-and-sqlspecialcolumns"></a>SQLColumns、SQLProcedureColumns 和 SQLSpecialColumns 返回的列元数据  
 对于日期/时间类型将返回以下列值：  
  
|列类型|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|-----------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_WVARCHAR|SQL_WVARCHAR|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|COLUMN_SIZE|10|8，10，16|16|23|19、21..27|26、28..34|  
|BUFFER_LENGTH|20|16，20. 32|16|16|38，42，54|52，56。|  
|DECIMAL_DIGITS|Null|Null|0|3|Null|Null|  
|SQL_DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_DATETIME|SQL_DATETIME|SQL_WVARCHAR|SQL_WVARCHAR|  
|SQL_DATETIME_SUB|Null|Null|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|Null|Null|  
|CHAR_OCTET_LENGTH|Null|Null|Null|Null|Null|Null|  
|SS_DATA_TYPE|0|0|111|111|0|0|  
||||||||

 SQLSpecialColumns 不返回 SQL_DATA_TYPE、SQL_DATETIME_SUB、CHAR_OCTET_LENGTH 或 SS_DATA_TYPE。  
  
### <a name="data-type-metadata-returned-by-sqlgettypeinfo"></a>SQLGetTypeInfo 返回的数据类型元数据  
 对于日期/时间类型将返回以下列值：  
  
|列类型|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|-----------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_TYPE_TIMESTAMP|SQL_TYPE_TIMESTAMP|SQL_WVARCHAR|SQL_WVARCHAR|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|'|'|'|'|'|'|  
|LITERAL_SUFFIX|'|'|'|'|'|'|  
|CREATE_PARAMS|Null|Null|Null|Null|Null|Null|  
|NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|SQL_NULLABLE|  
|CASE_SENSITIVE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|可搜索|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|SQL_PRED_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|Null|Null|Null|Null|Null|Null|  
|FXED_PREC_SCALE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|SQL_FALSE|  
|AUTO_UNIQUE_VALUE|Null|Null|Null|Null|Null|Null|  
|LOCAL_TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|MINIMUM_SCALE|Null|Null|0|3|Null|Null|  
|MAXIMUM_SCALE|Null|Null|0|3|Null|Null|  
|SQL_DATA_TYPE|SQL_WVARCHAR|SQL_WVARCHAR|SQL_DATETIME|SQL_DATETIME|SQL_WVARCHAR|SQL_WVARCHAR|  
|SQL_DATETIME_SUB|Null|Null|SQL_CODE_TIMESTAMP|SQL_CODE_TIMESTAMP|Null|Null|  
|NUM_PREC_RADIX|Null|Null|Null|Null|Null|Null|  
|INTERVAL_PRECISION|Null|Null|Null|Null|Null|Null|  
|USERTYPE|0|0|12|22|0|0|  
||||||||

## <a name="down-level-server-behavior"></a>下级服务器行为  
 连接到版本早于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 的服务器实例时，只要试图使用新的服务器类型或相关的元数据代码和描述符字段，都将导致返回 SQL_ERROR。 将生成以下诊断记录：具有 SQLSTATE HY004 和消息“SQL 数据类型对于连接的服务器版本无效”，或具有 07006 和消息“受限制的数据类型属性冲突”。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC&#41;&#40;的日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
