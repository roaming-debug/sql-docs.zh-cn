---
title: 参数和行集元数据（OLE DB 驱动程序）
description: 了解与 OLE DB Driver for SQL Server 日期和时间增强功能相关的 DBBINDING 类型和类型成员。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- metadata [OLE DB]
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 50f32dde9d640eef6b3f7adeda2a310537e0948a
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742197"
---
# <a name="metadata---parameter-and-rowset"></a>元数据 - 参数和行集
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  本文提供了与 OLE DB 日期和时间增强功能相关的以下类型和类型成员的相关信息。  
  
-   DBBINDING 结构  
  
-   **ICommandWithParameters::GetParameterInfo**  
  
-   **ICommandWithParameters::SetParameterInfo**  
  
-   **IColumnsRowset::GetColumnsRowset**  
  
-   **IColumnsInfo::GetColumnInfo**  
  
## <a name="icommandwithparametersgetparameterinfo"></a>ICommandWithParameters::GetParameterInfo  
 通过 prgParamInfo 在 DBPARAMINFO 结构中返回以下信息  ：  
  
|参数类型|wType |ulParamSize |bPrecision |bScale |dwFlags <br /><br /> DBPARAMFLAGS_SS_ISVARIABLESCALE|  
|--------------------|-------------|-------------------|------------------|--------------|-----------------------------------------------------|  
|date|DBTYPE_DBDATE|6|10|0|Clear|  
|time|DBTYPE_DBTIME2|10|8, 10..16|0..7|Set|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|Clear|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|Clear|  
|datetime2|DBTYPE_DBTIMESTAMP|16|19、21..27|0..7|Set|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|20|26、28..34|0..7|Set|  
  
 请注意，在某些情况下，值范围不是连续的。 这是因为当小数精度大于零时添加了小数点。  
  
 DBPARAMFLAGS_SS_ISVARIABLESCALE 只有在连接到 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]（或更高版本）服务器时才有效。 当连接到下级服务器时，永远不会设置 DBPARAMFLAGS_SS_ISVARIABLESCALE。  
  
## <a name="icommandwithparameterssetparameterinfo-and-implied-parameter-types"></a>ICommandWithParameters::SetParameterInfo 和隐含的参数类型  
 在 DBPARAMBINDINFO 结构中提供的信息必须符合以下规定：  
  
|pwszDataSourceType <br /><br /> （特定于访问接口）|pwszDataSourceType <br /><br /> （一般 OLE DB）|ulParamSize |bScale |  
|----------------------------------------------------|-------------------------------------------------|-------------------|--------------|  
||DBTYPE_DATE|6|忽略|  
|date|DBTYPE_DBDATE|6|忽略|  
||DBTYPE_DBTIME|10|忽略|  
|time|DBTYPE_DBTIME2|10|0..7|  
|smalldatetime||16|忽略|  
|datetime||16|忽略|  
|datetime2 或 DBTYPE_DBTIMESTAMP|DBTYPE_DBTIMESTAMP|16|0..7|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|20|0..7|  
  
 bPrecision  参数将被忽略。  
  
 向服务器发送数据时将忽略“DBPARAMFLAGS_SS_ISVARIABLESCALE”。 通过使用特定于提供程序的类型名称“datetime”和“smalldatetime”，应用程序可以强制使用旧的表格格式数据流 (TDS) 类型   。 当连接到 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]（或更高版本）服务器时，如果类型名称为“datetime2”或“DBTYPE_DBTIMESTAMP”，将使用“datetime2”格式，并在必要时进行隐式服务器转换   。 如果使用提供程序特定的类型名称“datetime”或“smalldatetime”，则将忽略 bScale。 否则，应用程序必须确保正确设置了 bScale  。 对于使用“DBTYPE_DBTIMESTAMP”且从 MDAC 升级的应用程序和 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 的 OLE DB Driver for SQL Server，如果未正确设置 bScale  ，应用程序将失败。 当连接到早于 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 的服务器实例时，采用“DBTYPE_DBTIMESTAMP”且值不为 0 或 3 的 bScale 值不正确，并且将返回 E_FAIL  。  
  
 如果未调用 ICommandWithParameters::SetParameterInfo，则提供程序将根据 IAccessor::CreateAccessor 中指定的绑定类型来表示服务器类型，如下所示：  
  
|绑定类型|pwszDataSourceType <br /><br /> （特定于访问接口）|  
|------------------|----------------------------------------------------|  
|DBTYPE_DATE|datetime2(0)|  
|DBTYPE_DBDATE|date|  
|DBTYPE_DBTIME|time(0)|  
|DBTYPE_DBTIME2|time(7)|  
|DBTYPE_DBTIMESTAMP|datetime2(7)|  
|DBTYPE_DBTIMESTAMPOFFSET|datetimeoffset(7)|  
  
## <a name="icolumnsrowsetgetcolumnsrowset"></a>IColumnsRowset::GetColumnsRowset  
 IColumnsRowset::GetColumnsRowset  返回以下列：  
  
|列类型|DBCOLUMN_TYPE|DBCOLUM_COLUMNSIZE|DBCOLUMN_PRECISION|DBCOLUMN_SCALE、DBCOLUMN_DATETIMEPRECISION|DBCOLUMN_FLAGS、DBCOLUMNFLAGS_SS_ISVARIABLESCALE|  
|-----------------|--------------------|-------------------------|-------------------------|--------------------------------------------------|---------------------------------------------------------|  
|date|DBTYPE_DBDATE|6|10|0|Clear|  
|time|DBTYPE_DBTIME2|10|8, 10..16|0..7|Set|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|Clear|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|Clear|  
|datetime2|DBTYPE_DBTIMESTAMP|16|19、21..27|0..7|Set|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|20|26、28..34|0..7|Set|  
  
 在 DBCOLUMN_FLAGS 中，对于日期/时间类型，DBCOLUMNFLAGS_ISFIXEDLENGTH 始终为 True，而下列标志则始终为 False：  
  
-   DBCOLUMNFLAGS_CACHEDEFERRED  
  
-   DBCOLUMNFLAGS_ISBOOKMARK  
  
-   DBCOLUMNFLAGS_ISCHAPTER  
  
-   DBCOLUMNFLAGS_ISLONG  
  
-   DBCOLUMNFLAGS_ISROWID  
  
-   DBCOLUMNFLAGS_ISROWVER  
  
-   DBCOLUMNFLAGS_MAYDEFER  
  
 可根据列的定义方式和实际查询设置其余标志（DBCOLUMNFLAGS_ISNULLABLE、DBCOLUMNFLAGS_MAYBENULL、DBCOLUMNFLAGS_WRITE 和 DBCOLUMNFLAGS_WRITEUNKNOWN）。  
  
 DBCOLUMN_FLAGS 中提供了新的 DBCOLUMNFLAGS_SS_ISVARIABLESCALE 标志，以使应用程序可以确定列的服务器类型，其中 DBCOLUMN_TYPE 为 DBTYPE_DBTIMESTAMP。 还必须使用 DBCOLUMN_SCALE 或 DBCOLUMN_DATETIMEPRECISION 标识服务器类型。  
  
 仅当连接到 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)]（或更高版本）服务器时 DBCOLUMNFLAGS_SS_ISVARIABLESCALE 才有效。 当连接到下级服务器时，未定义 DBCOLUMNFLAGS_SS_ISVARIABLESCALE。  
  
## <a name="icolumnsinfogetcolumninfo"></a>IColumnsInfo::GetColumnInfo  
 DBCOLUMNINFO 结构返回以下信息：  
  
|参数类型|wType |ulColumnSize |bPrecision |bScale |dwFlags <br /><br /> DBPARAMFLAGS_SS_ISVARIABLESCALE|  
|--------------------|-------------|--------------------|------------------|--------------|-----------------------------------------------------|  
|date|DBTYPE_DBDATE|6|10|0|Clear|  
|time(1..7)|DBTYPE_DBTIME2|10|8, 10..16|0..7|Set|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|Clear|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|Clear|  
|datetime2|DBTYPE_DBTIMESTAMP|16|19、21..27|0..7|Set|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|20|26、28..34|0..7|Set|  
  
 在 dwFlags 中，对于日期/时间类型，DBCOLUMNFLAGS_ISFIXEDLENGTH 始终为 True，而下列标志则始终为 False  ：  
  
-   DBCOLUMNFLAGS_CACHEDEFERRED  
  
-   DBCOLUMNFLAGS_ISBOOKMARK  
  
-   DBCOLUMNFLAGS_ISCHAPTER  
  
-   DBCOLUMNFLAGS_ISLONG  
  
-   DBCOLUMNFLAGS_ISROWID  
  
-   DBCOLUMNFLAGS_ISROWVER、MAYDEFER  
  
 可以设置其余标志（DBCOLUMNFLAGS_ISNULLABLE、DBCOLUMNFLAGS_MAYBENULL、DBCOLUMNFLAGS_WRITE 和 DBCOLUMNFLAGS_WRITEUNKNOWN）。  
  
 dwFlags 中提供了新的 DBCOLUMNFLAGS_SS_ISVARIABLESCALE 标志，以使应用程序可以确定列的服务器类型，其中 wType 为 DBTYPE_DBTIMESTAMP   。 还必须使用 bScale 来标识服务器类型  。  
  
## <a name="see-also"></a>另请参阅  
 [针对 OLE DB 日期和时间改进的数据类型支持](../../oledb/ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md)  
  
  
