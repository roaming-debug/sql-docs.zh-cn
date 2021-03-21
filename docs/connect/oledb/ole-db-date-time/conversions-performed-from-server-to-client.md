---
title: 在服务器和客户端之间执行的转换 | Microsoft Docs
description: 了解在 SQL Server 与使用 OLE DB Driver for SQL Server 编写的客户端应用程序之间执行的日期/时间转换。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB], server to client
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4ed1a55fe249df7ed06b6fbbd084848fcad80779
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751997"
---
# <a name="conversions-performed-from-server-to-client"></a>在服务器和客户端之间执行的转换
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  本文说明在 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]（或更高版本）与使用适用于 SQL Server 的 OLE DB 驱动程序编写的客户端应用程序之间执行的日期/时间转换。  
  
## <a name="conversions"></a>转换  
 下表说明了返回到客户端的类型与绑定中的类型之间的转换。 对于输出参数，如果已调用 ICommandWithParameters::SetParameterInfo，并且在 pwszDataSourceType  中指定的类型与服务器上的实际类型不匹配，该服务器将执行隐式转换，并且返回到客户端的类型将与通过 ICommandWithParameters::SetParameterInfo 指定的类型匹配。 如果服务器的转换规则与本文描述的不同，这可能会导致意外转换结果出现。 例如，在必须提供默认日期时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 使用 1900-1-1，而不是 1899-12-30。  
  
|转换后 -><br /><br /> 源|DATE|DBDATE|DBTIME|DBTIME2|DBTIMESTAMP|DBTIMESTAMPOFFSET|FILETIME|BYTES|VARIANT|SSVARIANT|BSTR|STR|WSTR|  
|----------------------|----------|------------|------------|-------------|-----------------|-----------------------|--------------|-----------|-------------|---------------|----------|---------|----------|  
|Date|1, 7|OK|-|-|1|1, 3|1, 7|-|OK (VT_BSTR)|OK|OK|4|4|  
|时间|5, 6, 7|-|9|OK|6|3, 6|5, 6|-|OK (VT_BSTR)|OK|OK|4|4|  
|Smalldatetime|7|8|9, 10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|Datetime|5, 7|8|9, 10|10|OK|3|7|-|7 (VT_DATE)|OK|OK|4|4|  
|Datetime2|5, 7|8|9, 10|10|7|3|5, 7|-|OK (VT_BSTR)|OK|OK|4|4|  
|Datetimeoffset|5, 7, 11|8, 11|9, 10, 11|10, 11|7, 11|OK|5, 7, 11|-|OK (VT_BSTR)|OK|OK|4|4|  
|Char、Varchar、<br /><br /> Nchar、Nvarchar|7, 13|12|12, 9|12|12|12|7, 13|空值|不适用|不适用|不适用|不适用|空值|  
|Sql_variant<br /><br /> (datetime)|7|8|9, 10|10|OK|3|7|-|7(VT_DATE)|OK|OK|4|4|  
|Sql_variant<br /><br /> (smalldatetime)|7|8|9, 10|10|OK|3|7|-|7(VT_DATE)|OK|OK|4|4|  
|Sql_variant<br /><br /> (date)|1, 7|OK|2|2|1|1, 3|1, 7|-|OK(VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (time)|5, 6, 7|2|6|OK|6|3, 6|5, 6|-|OK(VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (datetime2)|5, 7|8|9, 10|10|OK|3|5, 7|-|OK(VT_BSTR)|OK|OK|4|4|  
|Sql_variant<br /><br /> (datetimeoffset)|5, 7, 11|8, 11|9, 10, 11|10, 11|7, 11|OK|5, 7, 11|-|OK(VT_BSTR)|OK|OK|4|4|  
  
## <a name="key-to-symbols"></a>符号含义  
  
|符号|含义|  
|------------|-------------|  
|OK|不需要任何转换。|  
|-|不支持任何转换。 如果在调用 IAccessor::CreateAccessor 时验证绑定，则在 rgStatus  中返回 DBBINDSTATUS_UPSUPPORTEDCONVERSION。 当延迟取值函数验证时，则设置 DBSTATUS_E_BADACCESSOR。|  
|1|时间字段设置为零。|  
|2|设置 DBSTATUS_E_CANTCONVERTVALUE。|  
|3|时区设置为零。|  
|4|如果客户端缓冲区不够大，则设置 DBSTATUS_S_TRUNCATED。 如果服务器类型包含秒的小数部分，结果字符串中的位数与服务器类型的小数位数完全匹配。|  
|5|截断的秒或秒的小数部分将被忽略。|  
|6|除非源为字符串时间文字，并且目标为 DBTYPE_DATE，否则将日期设置为当前日期。 这种情况下，将使用 1899-12-30。|  
|7|如果值溢出，则设置 DBSTATUS_E_DATAOVERFLOW。|  
|8|忽略时间字段。|  
|9|忽略秒的小数部分字段。|  
|10|忽略日期部分。|  
|11|将时间转换为客户端时区。 如果在此转换过程中出现错误，则设置 DBSTATUS_E_DATAOVERFLOW。|  
|12|字符串分析为 ISO 文字并转换为目标类型。 如果上述操作失败，该字符串则分析为 OLE 日期文字（还包含时间部分），并从 OLE 日期 (DBTYPE_DATE) 转换为目标类型。 字符串必须符合成功分析 ISO 格式所允许的目标类型的文字语法。 若要成功分析 OLE，字符串必须符合可由 OLE 识别的语法。 如果无法分析该字符串，则设置 DBSTATUS_E_CANTCONVERTVALUE。 如果任一部分的值超出范围，则设置 DBSTATUS_E_DATAOVERFLOW。|  
|13|字符串分析为 ISO 文字并转换为目标类型。 如果上述操作失败，该字符串则分析为 OLE 日期文字（还包含时间部分），并从 OLE 日期 (DBTYPE_DATE) 转换为目标类型。 除非目标为 DBTYPE_DATE 或 DBTYPE_DBTIMESTAMP，否则该字符串必须符合日期时间文字的语法。 在这种情况下，允许日期时间或时间文字，以便成功分析 ISO 格式。 若要成功分析 OLE，字符串必须符合可由 OLE 识别的语法。 如果无法分析该字符串，则设置 DBSTATUS_E_CANTCONVERTVALUE。 如果任一部分的值超出范围，则设置 DBSTATUS_E_DATAOVERFLOW。|  
  
## <a name="see-also"></a>另请参阅  
 [绑定和转换 (OLE DB)](../../oledb/ole-db-date-time/conversions-ole-db.md)  
  
  
