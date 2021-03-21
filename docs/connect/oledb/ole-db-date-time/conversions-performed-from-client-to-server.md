---
title: 在客户端和服务器之间执行的转换 | Microsoft Docs
description: 了解在使用 OLE DB Driver for SQL Server 编写的客户端应用程序与 SQL Server 之间执行的日期/时间转换。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- conversions [OLE DB], client to server
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9ff13ec5b2df249d31aeb10a32761d119ed418d3
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104748517"
---
# <a name="conversions-performed-from-client-to-server"></a>在客户端和服务器之间执行的转换
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  本文说明在使用适用于 SQL Server 的 OLE DB 驱动程序编写的客户端应用程序与 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]（或更高版本）之间执行的日期/时间转换。  
  
## <a name="conversions"></a>转换  
 本文介绍在客户端上执行的转换。 如果客户端指定的参数的秒的小数部分精度不同于服务器上定义的精度，那么，客户端转换将在该服务器允许成功执行该操作时导致失败。 特别是，客户端将将秒的小数部分的任何截断视为错误，而 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将时间值舍入为最接近的整数秒数。  
  
 如果未调用 ICommandWithParameters::SetParameterInfo，则将 DBTYPE_DBTIMESTAMP 绑定转换为 datetime2  。  
  
|转换后 -><br /><br /> 源|DBDATE (date)|DBTIME (time)|DBTIME2 (time)|DBTIMESTAMP (smalldatetime)|DBTIMESTAMP (datetime)|DBTIMESTAMP (datetime2)|DBTIMESTAMPOFFSET (datetimeoffset)|STR|WSTR|SQLVARIANT<br /><br /> (sql_variant)|  
|----------------------|---------------------|---------------------|----------------------|-----------------------------------|------------------------------|-------------------------------|------------------------------------------|---------|----------|-------------------------------------|  
|DATE|1, 2|1, 3, 4|4, 12|1, 12|1, 12|1, 12|1, 5, 12|1, 12|1, 12|1, 12<br /><br /> datetime2(0)|  
|DBDATE|1|-|-|1, 6|1, 6|1, 6|1, 5, 6|1, 10|1, 10|1<br /><br /> date|  
|DBTIME|-|1|1|1, 7|1, 7|1, 7|1, 5, 7|1, 10|1, 10|1<br /><br /> Time(0)|  
|DBTIME2|-|1, 3|1|1, 7, 10, 14|1, 7, 10, 15|1, 7, 10|1, 5, 7, 10|1, 10, 11|1, 10, 11|1<br /><br /> Time(7)|  
|DBTIMESTAMP|1, 2|1, 3, 4|1, 4, 10|1, 10, 14|1, 10, 15|1, 10|1, 5, 10|1, 10,11|1, 10, 11|1, 10<br /><br /> datetime2(7)|  
|DBTIMESTAMPOFFSET|1, 2, 8|1, 3, 4, 8|1, 4, 8, 10|1, 8, 10, 14|1, 8, 10, 15|1, 8, 10|1, 10|1, 10, 11|1, 10, 11|1, 10<br /><br /> datetimeoffset(7)|  
|FILETIME|1, 2|1, 3, 4|1, 4, 13|1, 13|1, 13|1, 13|1, 5, 13|1, 13|1, 10|1, 13<br /><br /> datetime2(3)|  
|BYTES|-|-|-|-|-|-|-|空值|不适用|空值|  
|VARIANT|1|1|1|1, 10|1, 10|1, 10|1, 10|空值|空值|1, 10|  
|SSVARIANT|1, 16|1, 16|1, 16|1, 10, 16|1, 10, 16|1, 10, 16|1, 10, 16|空值|空值|1, 16|  
|BSTR|1, 9|1, 9|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|空值|不适用|空值|  
|STR|1, 9|1, 9|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|空值|不适用|空值|  
|WSTR|1, 9|1, 9|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|1, 9, 10|空值|不适用|空值|  
  
## <a name="key-to-symbols"></a>符号含义  
  
|符号|含义|  
|------------|-------------|  
|-|不支持任何转换。 如果在调用 IAccessor::CreateAccessor 时验证绑定，则在 rgStatus  中返回 DBBINDSTATUS_UPSUPPORTEDCONVERSION。 当延迟取值函数验证时，则设置 DBSTATUS_E_BADACCESSOR。|  
|空值|不适用。|  
|1|如果提供的数据无效，则设置 DBSTATUS_E_CANTCONVERTVALUE。 在应用转换前验证输入数据，因此即使后续转换忽略某一部分，该部分仍然有效，以使转换成功完成。|  
|2|忽略时间字段。|  
|3|秒的小数部分必须为零，或者设置 DBSTATUS_E_DATAOVERFLOW。|  
|4|忽略日期部分。|  
|5|根据客户端的时区设置设置时区。|  
|6|时间设置为零。|  
|7|日期设置为当前日期。|  
|8|将时间转换为 UTC。 如果在此转换过程中出现错误，则设置 DBSTATUS_E_CANTCONVERTVALUE。|  
|9|字符串分析为 ISO 文字并转换为目标类型。 如果上述操作失败，该字符串则分析为 OLE 日期文字（还包含时间部分），并从 OLE 日期 (DBTYPE_DATE) 转换为目标类型。<br /><br /> 如果目标类型为 DBTIMESTAMP、 **smalldatetime**、 **datetime** 或 **datetime2**，字符串必须符合日期、时间或 **datetime2** 文字的语法或 OLE 可识别的语法。 如果字符串为日期文字，所有时间部分将设置为零。 如果字符串为时间文字，则将日期设置为当前日期。<br /><br /> 对于所有其他目标类型，字符串必须符合目标类型的文字语法。|  
|10|如果截断秒的小数部分，且发生数据丢失，则设置 DBSTATUS_E_DATAOVERFLOW。 对于字符串转换，仅当该字符串符合 ISO 语法时才能执行溢出检查。 如果字符串为 OLE 日期文字，则舍入秒的小数部分。<br /><br /> 对于从 DBTIMESTAMP (datetime) 到 smalldatetime 的转换，适用于 SQL Server 的 OLE DB 驱动程序将自行截断秒的值而不引发 DBSTATUS_E_DATAOVERFLOW 错误。|  
|11|秒的小数部分的位数（小数位数）根据下表的目标列的大小确定。 对于大于表中范围的列大小，则暗指小数位数为 9。 此转换应允许最高 9 位的秒的小数部分位数，这是 OLE DB 允许的最大位数。<br /><br /> 但是，如果源类型为 DBTIMESTAMP 且秒的小数部分为零，则不会生成秒的小数部分位数或小数点。 此行为确保使用早期 OLE DB 访问接口开发的应用程序的向后兼容性。<br /><br /> 列大小为 ~0 则表示 OLE DB 的大小不受限制（即 9 位数，除非应用 DBTIMESTAMP 的三位数规则）。|  
|12|将保留 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 之前的 DBTYPE_DATE 转换语义。 秒的小数部分被截断为零。|  
|13|将保留 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 之前的 DBTYPE_FILETIME 转换语义。 如果使用 Windows FileTimeToSystemTime API，秒的小数部分精度则限制为 1 毫秒。|  
|14|将保留 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 之前的 smalldatetime 转换语义  。 秒设置为零。|  
|15|将保留 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 之前的 datetime 转换语义  。 秒舍入为最接近的 1/300 秒。|  
|16|嵌入 SSVARIANT 客户端结构的指定类型值的转换行为与未嵌入 SSVARIANT 客户端结构的相同类型和相同值的行为相同。|  
  
| 类型 | 长度（以字符为单位） | 缩放 |  
| ---- | ----------------- | ----- |  
|DBTIME2|8, 10..18|0,1..9|  
|DBTIMESTAMP|19, 21..29|0,1..9|  
|DBTIMESTAMPOFFSET|26, 28..36|0,1..9|  
  
## <a name="see-also"></a>另请参阅  
 [绑定和转换 (OLE DB)](../../oledb/ole-db-date-time/conversions-ole-db.md)  
  
  
