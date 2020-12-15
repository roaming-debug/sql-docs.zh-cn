---
description: 增强的日期和时间类型的大容量复制更改（OLE DB 和 ODBC）
title: 大容量复制，增强类型，OLE DB
ms.custom: ''
ms.date: 12/18/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC, bulk copy operations
ms.assetid: c29e0f5e-9b3c-42b3-9856-755f4510832f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: dcb0ada0f90cb93ffa4f6e09ba94d8e167508938
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440023"
---
# <a name="bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc"></a>增强的日期和时间类型的大容量复制更改（OLE DB 和 ODBC）
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题描述为支持大容量复制功能而增强的日期/时间功能。 本主题中的信息对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 中的 OLE DB 和 ODBC 均适用。  
  
## <a name="format-files"></a>格式化文件  
 在以交互方式生成格式化文件时，下表描述用于指定日期/时间类型和相应的宿主文件数据类型名称的输入。  
  
|文件存储类型|宿主文件数据类型|响应提示：“请输入 <field_name> [\<default>] 字段的文件存储类型:”|  
|-----------------------|-------------------------|-----------------------------------------------------------------------------------------------------|  
|Datetime|SQLDATETIME|d|  
|Smalldatetime|SQLDATETIM4|D|  
|Date|SQLDATE|de|  
|时间|SQLTIME|te|  
|Datetime2|SQLDATETIME2|d2|  
|Datetimeoffset|SQLDATETIMEOFFSET|do|  
||||

 XML 格式化文件 XSD 将增加以下内容：  
  
```xml
<xs:complexType name="SQLDATETIME2">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATETIMEOFFSET">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLDATE">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="SQLTIME">  
    <xs:complexContent>  
        <xs:extension base="bl:Fixed"/>  
    </xs:complexContent>  
</xs:complexType>  
```  
  
## <a name="character-data-files"></a>字符数据文件  
 在字符数据文件中，日期和时间值按数据类型的 "数据格式：字符串和文字" 一节中所述的方式表示，该部分针对 ODBC 的 [Odbc 日期和时间改进](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md) ，或对 OLE DB 的 [OLE DB 日期和时间改进的数据类型支持](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md) 。  
  
 在本机数据解答中，四个新类型的日期和时间值表示为其 TDS 表示形式，其小数位数为 7 (，因为这是受支持的最大值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，bcp 数据文件不存储这些列的小数位数) 。 对于现有 datetime 和 smalldatetime 类型或其表格格式数据流 (TDS) 表示形式的存储没有变化   。  
  
 针对 OLE DB 的不同存储类型的存储大小如下：  
  
|文件存储类型|存储大小（以字节为单位）|  
|-----------------------|---------------------------|  
|datetime|8|  
|smalldatetime|4|  
|date|3|  
|time|6|  
|datetime2|9|  
|datetimeoffset|11|  
|||

 针对 ODBC 的大小如下。 请注意，不必在格式化文件或数据文件中存储精度，因为 BCP.exe 将始终从服务器检索精度。  
  
|文件存储类型|存储大小（以字节为单位）|存储格式|  
|-----------------------|---------------------------|--------------------|  
|datetime (d)|8|TDS|  
|smalldatetime (D)|4|TDS|  
|date (de)|3|TDS|  
|time (te)|6|TDS|  
|datetime2 (d2)|9|TDS|  
|datetimeoffset (do)|11|TDS|  
||||

## <a name="bcp-types-in-sqlnclih"></a>sqlncli.h 中的 BCP 类型  
 以下类型在 sqlncli.h 中定义，以便用于对 ODBC 的 BCP API 扩展。 这些类型使用 OLE DB 中 IBCPSession::BCPColFmt 的 eUserDataType  参数进行传递。  
  
|文件存储类型|宿主文件数据类型|键入 sqlncli.msi 以用于 IBCPSession：： BCPColFmt|值|  
|-----------------------|-------------------------|-----------------------------------------------------------|-----------|  
|Datetime|SQLDATETIME|BCP_TYPE_SQLDATETIME|0x3d|  
|Smalldatetime|SQLDATETIM4|BCP_TYPE_SQLDATETIME4|0x3a|  
|Date|SQLDATE|BCP_TYPE_SQLDATE|0x28|  
|时间|SQLTIME|BCP_TYPE_SQLTIME|0x29|  
|Datetime2|SQLDATETIME2|BCP_TYPE_SQLDATETIME2|0x2a|  
|Datetimeoffset|SQLDATETIMEOFFSET|BCP_TYPE_SQLDATETIMEOFFSET|0x2b|  
|||||

## <a name="bcp-data-type-conversions"></a>BCP 数据类型转换  
 下表提供转换信息。  
  
 **OLE DB 说明** 以下转换由 IBCPSession 执行。 IRowsetFastLoad 使用[从客户端转换到服务器](../../relational-databases/native-client-ole-db-date-time/conversions-performed-from-client-to-server.md)中定义的 OLE DB 转换。 请注意，日期时间值将舍入为 1 秒的 1/300，smalldatetime 值在执行下述客户端转换后将秒设置为零。 日期时间舍入将传播至小时和分钟，而非日期。  
  
|转换后 -><br /><br /> 源|date|time|smalldatetime|datetime|datetime2|datetimeoffset|char|wchar|  
|------------------------|----------|----------|-------------------|--------------|---------------|--------------------|----------|-----------|  
|Date|1|-|1,6|1,6|1,6|1,5,6|1,3|1,3|  
|时间|空值|1,10|1,7,10|1,7,10|1,7,10|1,5,7,10|1,3|1,3|  
|Smalldatetime|1,2|1,4,10|1|1|1,10|1,5,10|1,11|1,11|  
|datetime|1,2|1,4,10|1,12|1|1,10|1,5,10|1,11|1,11|  
|Datetime2|1,2|1,4,10|1,10 (ODBC)1,12 (OLE DB)|1,10|1,10|1,5,10|1,3|1,3|  
|Datetimeoffset|1,2,8|1,4,8,10|1,8,10|1,8,10|1,8,10|1,10|1,3|1,3|  
|Char/wchar (date)|9|-|9,6 (ODBC)9,6,12 (OLE DB)|9,6 (ODBC)9,6,12 (OLE DB)|9,6|9,5,6|不可用|空值|  
|Char/wchar (time)|-|9,10|9,7,10 (ODBC)9,7,10,12 (OLE DB)|9,7,10 (ODBC)9,7,10, 12 (OLE DB)|9,7,10|9,5,7,10|不可用|空值|  
|Char/wchar (datetime)|9,2|9,4,10|9,10 (ODBC)9,10,12 (OLE DB)|9,10 (ODBC)9,10,12 (OLE DB)|9,10|9,5,10|不可用|空值|  
|Char/wchar (datetimeoffset)|9,2,8|9,4,8,10|9,8,10 (ODBC)9,8,10,12 (OLE DB)|9,8,10 (ODBC)9,8,10,12 (OLE DB)|9,8,10|9,10|不可用|空值|  
||||||||||

#### <a name="key-to-symbols"></a>符号含义  
  
|符号|含义|  
|------------|-------------|  
|-|不支持任何转换。<br /><br /> 生成 ODBC 诊断记录，同时还生成 SQLSTATE 07006 和消息“受限制的数据类型属性冲突”。|  
|1|如果提供的数据无效，则生成 ODBC 诊断记录，同时还生成 SQLSTATE 22007 和消息“日期时间格式无效”。 对于 datetimeoffset 值，在转换为 UTC 后时间部分必须处于规定范围内，即使不要求转换为 UTC。 这是因为 TDS 和服务器始终规范化 UTC 的 datetimeoffset 值中的时间。 因此，在转换为 UTC 后，客户端必须检查时间部分是否处于支持的范围内。|  
|2|忽略时间部分。|  
|3|对于 ODBC，如果发生截断且发生数据丢失，则会生成包含 SQLSTATE 22001 和消息 "字符串数据，右截断" 的诊断记录，该小数位数) 根据下表的目标列的大小确定 (小数位数。 对于大于表中范围的列大小，则暗指小数位数为 7。 此转换应允许最高 9 位的秒的小数形式位数，这是 ODBC 允许的最大位数。<br /><br /> **类型：** DBTIME2<br /><br /> **暗指的小数位数 0** 8<br /><br /> **隐含比例 1. 7** 10，16<br /><br /> <br /><br /> **类型：** DBTIMESTAMP<br /><br /> **表示确定位数 0：** 19<br /><br /> **隐含比例 1 .. 7：** 21 .. 27<br /><br /> <br /><br /> **类型：** DBTIMESTAMPOFFSET<br /><br /> **表示确定位数 0：** 26<br /><br /> **默示比例为 1 .. 7：** 28 .. 34<br /><br /> 对于 OLE DB，如果发生具有数据丢失的截断，则会发出错误。 对于 datetime2，秒的小数部分位数（小数位数）是由目标列的大小确定（以下表为依据）。 对于大于表中范围的列大小，则暗指小数位数为 9。 此转换应允许最高 9 位的秒的小数部分位数，这是 OLE DB 允许的最大位数。<br /><br /> **类型：** DBTIME2<br /><br /> **暗指的小数位数 0** 8<br /><br /> **暗指的小数位数 1..9** 1..9<br /><br /> <br /><br /> **类型：** DBTIMESTAMP<br /><br /> **表示确定位数 0：** 19<br /><br /> **表示确定位数 1..9：** 21..29<br /><br /> <br /><br /> **类型：** DBTIMESTAMPOFFSET<br /><br /> **表示确定位数 0：** 26<br /><br /> **表示确定位数 1..9：** 28..36|  
|4|忽略日期部分。|  
|5|时区设置为 UTC（例如 00:00）。|  
|6|时间设置为零。|  
|7|日期设置为 1900-01-01。|  
|8|忽略时区偏移量。|  
|9|根据遇到的第一个标点符号以及出现的剩余部分，该字符串将被分析和转换为 date、datetime、datetimeoffset 或 time 值。 然后，根据本主题末尾的表中针对此过程发现的源类型的规则，此字符串将转换为目标类型。 如果提供的数据不能在分析时无错，或者任何组成部分超出允许的范围，或者不存在从文字类型到目标类型的转换，则发出错误 (OLE DB)，或者生成 ODBC 诊断记录，同时还生成 SQLSTATE 22018 和消息“为转换指定的字符值无效”。 对于 datetime 和 smalldatetime 参数，如果年份处于这些类型支持的范围外，则发出错误 (OLE DB)，或者生成 ODBC 诊断记录，同时还生成 SQLSATE 22007 和消息“日期时间格式无效”。<br /><br /> 对于 datetimeoffset，在转换为 UTC 后该值必须处于规定范围内，即使不要求转换为 UTC。 这是因为 TDS 和服务器始终规范化 UTC 的 datetimeoffset 值中的时间，因此在转换为 UTC 后，客户端必须确认时间部分处于支持的范围内。 如果该值不在支持的 UTC 范围内，则发出错误 (OLE DB)，或者生成 ODBC 诊断记录，同时还生成 SQLSTATE 22007 和消息“日期时间格式无效”。|  
|10|如果在客户端到服务器转换时发生具有数据丢失的截断，则发出错误 (OLE DB)，或者生成 ODBC 诊断记录，同时还生成 SQLSTATE 22008 和消息“日期时间字段溢出”。 如果值处于服务器使用的 UTC 范围可表示的范围外，也会发生此错误。 如果在服务器到客户端转换时发生秒或秒的小数部分截断，则只会显示警告。|  
|11|如果发生具有数据丢失的截断，则生成诊断记录。<br /><br /> 在服务器到客户端转换时，这是警告 (ODBC SQLSTATE S1000)。<br /><br /> 在客户端到服务器转换时，这是错误 (ODBC SQLSTATE 22001)。|  
|12|秒设置为零，秒的小数部分被放弃。 可能没有截断错误。|  
|空值|现有 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和更早版本的行为将保持。|  
|||

## <a name="see-also"></a>另请参阅  
 [ODBC&#41;&#40;的日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)   
 [日期和时间改进 (OLE DB)](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
