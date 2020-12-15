---
description: SQLSetDescRec
title: SQLSetDescRec |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLSetDescRec function
ms.assetid: 203d02a2-aa09-462b-a489-a2cdd6f6023b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce62781859e07eda250a6ea5d0c84016a0216163
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438680"
---
# <a name="sqlsetdescrec"></a>SQLSetDescRec
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题讨论特定于 Native Client 的 SQLSetDescRec 功能 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
## <a name="sqlsetdescrec-and-table-valued-parameters"></a>SQLSetDescRec 和表值参数  
 SQLSetDescRec 可用于为表值参数和表值参数列设置描述符字段。 表值参数列仅在将描述符标头字段 SQL_SOPT_SS_PARAM_FOCUS 设置为特定记录（其 SQL_DESC_TYPE 设置为 SQL_SS_TABLE）的序数时可用。 有关 SQL_SOPT_SS_PARAM_FOCUS 的详细信息，请参阅 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)。  
  
 下表介绍了参数和描述符字段之间的映射。  
  
|参数|非表值参数类型的相关属性，包括表值参数列|表值参数的相关属性|  
|---------------|--------------------------------------------------------------------------------------------------------|----------------------------------------------------|  
|*类型*|SQL_DESC_TYPE|SQL_SS_TABLE|  
|*子类型*|忽略|对于 SQL_DATETIME 或 SQL_INTERVAL 类型的记录，请将它设置为 SQL_DESC_DATETIME_INTERVAL_CODE。|  
|*时长*|SQL_DESC_OCTET_LENGTH|表值参数类型名称的长度。 如果类型名称是以 null 结束，则它可为 SQL_NTS；如果不需要表值参数类型名称，则为零。|  
|*精度*|SQL_DESC_PRECISION|SQL_DESC_ARRAY_SIZE|  
|*可伸缩*|SQL_DESC_SCALE|未使用。 此参数应为零。|  
|*DataPtr*|APD 中的 SQL_DESC_DATA_PTR|SQL_CA_SS_TYPE_NAME<br /><br /> 对于存储过程调用，此参数是可选的。如果不需要此参数，则可以指定为 NULL。 对于非过程调用的 SQL 语句，必须指定此参数。<br /><br /> *DataPtr* 还充当唯一值，应用程序可在使用可变行绑定时用于标识此表值参数。|  
|*StringLengthPtr*|SQL_DESC_OCTET_LENGTH_PTR|SQL_DESC_OCTET_LENGTH_PTR<br /><br /> 对于表值参数，它是要传输的行数或 SQL_DATA_AT_EXEC。 这是一个指向值的指针，该值包含要与 SQLExecDirect 传输的行数。|  
|*IndicatorPtr*|SQL_DESC_INDICATOR_PTR|SQL_DESC_INDICATOR_PTR|  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqlsetdescrec-support-for-enhanced-date-and-time-features"></a>SQLSetDescRec 对日期和时间增强功能的支持  
 日期/时间类型所允许的值如下所示：  
  
| Attribute | *类型* | *子类型* | *时长* | *精度* | *缩放* |
| --------- | ------ | --------- | -------- | ----------- | ------- |
|datetime|SQL_DATETIME|SQL_CODE_TIMESTAMP|4|3|3|  
|smalldatetime|SQL_SQL_DATETIME|SQL_CODE_TIMESTAMP|8|0|0|  
|date|SQL_DATETIME|SQL_CODE_DATE|6|0|0|  
|time|SQL_SS_TIME2|0|10|0..7|0..7|  
|datetime2|SQL_DATETIME|SQL_CODE_TIMESTAMP|16|0..7|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|0|20|0..7|0..7|  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlsetdescrec-support-for-large-clr-udts"></a>SQLSetDescRec 对大型 CLR UDT 的支持  
 **SQLSetDescRec** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLSetDescRec](../../odbc/reference/syntax/sqlsetdescrec-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
