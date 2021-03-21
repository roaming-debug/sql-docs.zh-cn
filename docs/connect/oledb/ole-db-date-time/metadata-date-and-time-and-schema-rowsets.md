---
title: 日期和时间与架构行集 | Microsoft Docs
description: 了解 OLE DB Driver for SQL Server 中有关 COLUMNS 行集和 PROCEDURE_PARAMETERS 行集的 OLE DB 日期和时间增强功能。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- date/time [OLE DB], schema rowsets
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ab9b7d99c472b3cef932ed2f275000aa8f41695e
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104742067"
---
# <a name="metadata---date-and-time-and-schema-rowsets"></a>元数据 - 日期和时间与架构行集
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  本主题提供有关 COLUMNS 行集和 PROCEDURE_PARAMETERS 行集的信息。 该信息与 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 中引入的 OLE DB 日期和时间增强功能相关。  
  
## <a name="columns-rowset"></a>COLUMNS 行集  
 对于日期/时间类型将返回以下列值：  
  
|列类型|DATA_TYPE|COLUMN_FLAGS, DBCOLUMFLAGS_SS_ISVARIABLESCALE|DATETIME_PRECISION|  
|-----------------|----------------|------------------------------------------------------|-------------------------|  
|date|DBTYPE_DBDATE|Clear|0|  
|time|DBTYPE_DBTIME2|Set|0..7|  
|smalldatetime|DBTYPE_DBTIMESTAMP|Clear|0|  
|datetime|DBTYPE_DBTIMESTAMP|Clear|3|  
|datetime2|DBTYPE_DBTIMESTAMP|Set|0..7|  
|datetimeoffset|DBTYPE_DBTIMESTAMPOFFSET|Set|0..7|  
  
 在 COLUMN_FLAGS 中，对于日期/时间类型，DBCOLUMNFLAGS_ISFIXEDLENGTH 始终为 True，并且以下标记始终为 False：  
  
-   DBCOLUMNFLAGS_CACHEDEFERRED  
  
-   DBCOLUMNFLAGS_ISBOOKMARK  
  
-   DBCOLUMNFLAGS_ISCHAPTER  
  
-   DBCOLUMNFLAGS_ISLONG  
  
-   DBCOLUMNFLAGS_ISROWID  
  
-   DBCOLUMNFLAGS_ISROWVER  
  
-   DBCOLUMNFLAGS_MAYDEFER  
  
 其余的标记（DBCOLUMNFLAGS_ISNULLABLE、DBCOLUMNFLAGS_MAYBENULL、DBCOLUMNFLAGS_WRITE 和 DBCOLUMNFLAGS_WRITEUNKNOWN）可能被设置，具体取决于列的定义方式。  
  
 在 COLUMN_FLAGS 中提供了新标记 DBCOLUMNFLAGS_SS_ISVARIABLESCALE，以允许应用程序确定 DATA_TYPE 为 DBTYPE_DBTIMESTAMP 的那些列的服务器类型。 为标识服务器类型还必须使用 DATETIME_PRECISION。  
  
 仅当连接到 [!INCLUDE[ssKatmai](../../../includes/sskatmai-md.md)] 或更高版本服务器时 DBCOLUMNFLAGS_SS_ISVARIABLESCALE 才有效。 连接到下级服务器时，未定义 DBCOLUMNFLAGS_SS_ISFIXEDSCALE。  
  
## <a name="procedure_parameters-rowset"></a>PROCEDURE_PARAMETERS 行集  
 DATA_TYPE 包含与 COLUMNS 架构行集相同的值，并且 TYPE_NAME 包含服务器类型。  
  
 已添加新列 SS_DATETIME_PRECISION，以便返回类似于 COLUMNS 行集的 DATETIME_PRECISION 列中的类型精度。  
  
## <a name="provider_types-rowset"></a>PROVIDER_TYPES 行集  
 对于日期/时间类型将返回以下行：  
  
|类型 -><br /><br /> 列|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|--------------------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|DATA_TYPE|DBTYPE_DBDATE|DBTYPE_DBTIME2|DBTYPE_DBTIMESTAMP|DBTYPE_DBTIMESTAMP|DBTYPE_DBTIMESTAMP|DBTYPE_DBTIMESTAMPOFFSET|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|'|'|'|'|'|'|  
|LITERAL_SUFFIX|'|'|'|'|'|'|  
|CREATE_PARAMS|Null|scale|Null|Null|scale|scale|  
|IS_NULLABLE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|  
|CASE_SENSITIVE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|Null|Null|Null|Null|Null|Null|  
|FIXED_PREC_SCALE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|AUTO_UNIQUE_VALUE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|LOCAL_TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|MINIMUM_SCALE|Null|0|Null|Null|0|0|  
|MAXIMUM_SCALE|Null|7|Null|Null|7|7|  
|GUID|Null|Null|Null|Null|Null|Null|  
|TYPELIB|Null|Null|Null|Null|Null|Null|  
|VERSION|Null|Null|Null|Null|Null|Null|  
|IS_LONG|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|BEST_MATCH|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE，除非符合以下某项条件：<br /><br /> 是连接到下级服务器的客户端。<br /><br /> 数据类型兼容性连接属性指定兼容性级别为 80。|VARIANT_TRUE，除非符合以下某项条件：<br /><br /> 是连接到下级服务器的客户端。<br /><br /> 数据类型兼容性连接属性指定兼容性级别为 80。|VARIANT_TRUE|  
|IS_FIXEDLENGTH|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|  
  
 对于数值和小数类型，OLE DB 仅定义了 MINIMUM_SCALE 和 MAXIMUM_SCALE，因此适用于 SQL Server 的 OLE DB 驱动程序对 time、datetime2 和 datetimeoffset 使用这些列是非标准行为。  
  
## <a name="see-also"></a>另请参阅  
 [元数据 &#40;OLE DB&#41;](../../oledb/ole-db-date-time/metadata-parameter-and-rowset.md)  
  
  
