---
description: SQL Server 早期版本的新日期和时间功能 (OLE DB)
title: 以前 SQL Server 版本的日期和时间 OLE DB 功能
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: 96976bac-018c-47cc-b1b2-fa9605eb55e5
author: markingmyname
ms.author: maghan
ms.custom: seo-dt-2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 55a7e519f22bb10a472a8c76bb88bb003ab7ad21
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755907"
---
# <a name="new-date-and-time-features-with-previous-sql-server-versions-ole-db"></a>SQL Server 早期版本的新日期和时间功能 (OLE DB)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  本主题介绍当使用增强的日期和时间功能的客户端应用程序与早于的版本通信时的预期行为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ，以及当使用早于的 Native client 版本编译的客户端 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 向支持日期和时间增强功能的服务器发送命令时的预期行为。  
  
## <a name="down-level-client-behavior"></a>下级客户端行为  
 使用早于的 Native Client 版本的客户端应用程序将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 新的日期/时间类型作为 **nvarchar** 列来查看。 这些列内容是文字表示。 有关详细信息，请参阅 [数据类型支持 OLE DB 日期和时间改进](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md)的 "数据格式：字符串和文字" 部分。 列大小是为列指定的精度的最大文字长度。  
  
 目录 Api 将返回与返回给客户端的下级数据类型代码一致的元数据 (例如， **nvarchar**) 和关联的下层表示形式 (例如，相应的文本) 格式。 不过，返回的数据类型名称将为 real [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 类型名称。  
  
 当下级客户端应用程序针对 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (或更高版本运行时) 服务器上对日期/时间类型进行架构更改时，预期的行为如下所示：  
  
|OLE DB 客户端类型|SQL Server 2005 类型|SQL Server 2008 (或更高版本) 类型|结果转换（服务器到客户端）|参数转换（客户端到服务器）|  
|------------------------|--------------------------|---------------------------------------|--------------------------------------------|-----------------------------------------------|  
|DBTYPE_DBDATE|datetime|日期|确定|确定|  
|DBTYPE_DBTIMESTAMP|||时间字段设置为零。|如果时间字段非零，则 IRowsetChange 将因字符串截断而失败。|  
|DBTYPE_DBTIME||Time(0)|确定|确定|  
|DBTYPE_DBTIMESTAMP|||日期字段设置为当前日期。|如果秒的小数部分为非零，IRowsetChange 将失败，原因是字符串截断。<br /><br /> 日期将被忽略。|  
|DBTYPE_DBTIME||Time(7)|失败-时间文本无效。|确定|  
|DBTYPE_DBTIMESTAMP|||失败-时间文本无效。|确定|  
|DBTYPE_DBTIMESTAMP||Datetime2 (3) |确定|确定|  
|DBTYPE_DBTIMESTAMP||Datetime2 (7) |确定|确定|  
|DBTYPE_DBDATE|Smalldatetime|日期|确定|确定|  
|DBTYPE_DBTIMESTAMP|||时间字段设置为零。|如果时间字段非零，则 IRowsetChange 将因字符串截断而失败。|  
|DBTYPE_DBTIME||Time(0)|确定|确定|  
|DBTYPE_DBTIMESTAMP|||日期字段设置为当前日期。|如果秒的小数部分为非零，IRowsetChange 将失败，原因是字符串截断。<br /><br /> 日期将被忽略。|  
|DBTYPE_DBTIMESTAMP||Datetime2(0)|确定|确定|  
  
 “可以”表示如果已适用于 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]，应继续适用于 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]（或更高版本）。  
  
 只考虑了以下常见的架构更改：  
  
-   使用新类型，这种情况下在逻辑上应用程序只需要一个日期或时间值。 但是，应用程序被强制使用 **datetime** 或 **smalldatetime** ，因为没有可用的单独日期和时间类型。  
  
-   使用新类型以获得其他秒小数精度或准确性。  
  
-   切换到 **datetime2** ，因为这是日期和时间的首选数据类型。  
  
 如果源类型的字符串表示形式大于目标类型的字符串表示形式，则使用通过 ICommandWithParameters：： GetParameterInfo 或架构行集获取的服务器元数据的应用程序通过 ICommandWithParameters：： SetParameterInfo 设置参数类型信息会失败。 例如，如果客户端绑定使用 DBTYPE_DBTIMESTAMP 并且服务器列是日期，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client 会将值转换为 "yyyy-mm-dd hh： mm： ss"，但服务器元数据将作为 **nvarchar (10)** 返回。 生成的溢出会导致 DBSTATUS_E_CATCONVERTVALUE。 IRowsetChange 的数据转换会出现类似的问题，因为行集元数据是通过结果集元数据设置的。  
  
### <a name="parameter-and-rowset-metadata"></a>参数和行集元数据  
 本部分介绍使用早于的 Native Client 版本编译的客户端的参数、结果列和架构行集的元数据 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 。  
  
#### <a name="icommandwithparametersgetparameterinfo"></a>ICommandWithParameters::GetParameterInfo  
 DBPARAMINFO 结构通过 *prgParamInfo* 参数返回以下信息：  
  
|参数类型|wType|ulParamSize|bPrecision|bScale|  
|--------------------|-----------|-----------------|----------------|------------|  
|date|DBTYPE_WSTR|10|~0|~0|  
|time|DBTYPE_WSTR|8, 10..16|~0|~0|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19，21. 27|~0|~0|  
|datetimeoffset|DBTYPE_WSTR|26，28. 34|~0|~0|  
  
 请注意，某些值范围不是连续的，例如，8,10..16 中缺少 9。 这是因为当小数精度大于零时添加了小数点。  
  
#### <a name="icolumnsrowsetgetcolumnsrowset"></a>IColumnsRowset::GetColumnsRowset  
 会返回以下列：  
  
|列类型|DBCOLUMN_TYPE|DBCOLUMN_COLUMNSIZE|DBCOLUMN_PRECISION|DBCOLUMN_SCALE、DBCOLUMN_DATETIMEPRECISION|  
|-----------------|--------------------|--------------------------|-------------------------|--------------------------------------------------|  
|date|DBTYPE_WSTR|10|Null|Null|  
|time|DBTYPE_WSTR|8, 10..16|Null|Null|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19，21. 27|Null|Null|  
|datetimeoffset|DBTYPE_WSTR|26，28. 34|Null|Null|  
  
#### <a name="columnsinfogetcolumninfo"></a>ColumnsInfo::GetColumnInfo  
 DBCOLUMNINFO 结构返回以下信息：  
  
|参数类型|wType|ulColumnSize|bPrecision|bScale|  
|--------------------|-----------|------------------|----------------|------------|  
|date|DBTYPE_WSTR|10|~0|~0|  
|time(1..7)|DBTYPE_WSTR|8, 10..16|~0|~0|  
|smalldatetime|DBTYPE_DBTIMESTAMP|16|16|0|  
|datetime|DBTYPE_DBTIMESTAMP|16|23|3|  
|datetime2|DBTYPE_WSTR|19，21. 27|~0|~0|  
|datetimeoffset|DBTYPE_WSTR|26，28. 34|~0|~0|  
  
### <a name="schema-rowsets"></a>架构行集  
 本部分论述了新数据类型的参数、结果列以及架构行集的元数据。 此信息非常有用，因为你有使用早于 Native Client 的工具开发的客户端提供程序 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
#### <a name="columns-rowset"></a>COLUMNS 行集  
 对于日期/时间类型将返回以下列值：  
  
|列类型|DATA_TYPE|CHARACTER_MAXIMUM_LENGTH|CHARACTER_OCTET_LENGTH|DATETIME_PRECISION|  
|-----------------|----------------|--------------------------------|------------------------------|-------------------------|  
|date|DBTYPE_WSTR|10|20|Null|  
|time|DBTYPE_WSTR|8, 10..16|16，20. 32|Null|  
|smalldatetime|DBTYPE_DBTIMESTAMP|Null|Null|0|  
|datetime|DBTYPE_DBTIMESTAMP|Null|Null|3|  
|datetime2|DBTYPE_WSTR|19，21. 27|38，42。|Null|  
|datetimeoffset|DBTYPE_WSTR|26，28. 34|52，56。|Null|  
  
#### <a name="procedure_parameters-rowset"></a>PROCEDURE_PARAMETERS 行集  
 对于日期/时间类型将返回以下列值：  
  
|列类型|DATA_TYPE|CHARACTER_MAXIMUM_LENGTH|CHARACTER_OCTET_LENGTH|TYPE_NAME<br /><br /> LOCAL_TYPE_NAME|  
|-----------------|----------------|--------------------------------|------------------------------|--------------------------------------|  
|date|DBTYPE_WSTR|10|20|date|  
|time|DBTYPE_WSTR|8, 10..16|16，20. 32|time|  
|smalldatetime|DBTYPE_DBTIMESTAMP|Null|Null|smalldatetime|  
|datetime|DBTYPE_DBTIMESTAMP|Null|Null|datetime|  
|datetime2|DBTYPE_WSTR|19，21. 27|38，42。|datetime2|  
|datetimeoffset|DBTYPE_WSTR|26，28. 34|52，56。|datetimeoffset|  
  
#### <a name="provider_types-rowset"></a>PROVIDER_TYPES 行集  
 对于日期/时间类型将返回以下行：  
  
|类型 -><br /><br /> 列|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|--------------------------|----------|----------|-------------------|--------------|---------------|--------------------|  
|TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|DATA_TYPE|DBTYPE_WSTR|DBTYPE_WSTR|DBTYPE_DBTIMESTAMP|DBTYPE_DBTIMESTAMP|DBTYPE_WSTR|DBTYPE_WSTR|  
|COLUMN_SIZE|10|16|16|23|27|34|  
|LITERAL_PREFIX|'|'|'|'|'|'|  
|LITERAL_SUFFIX|'|'|'|'|'|'|  
|CREATE_PARAMS|Null|Null|Null|Null|Null|Null|  
|IS_NULLABLE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|VARIANT_TRUE|  
|CASE_SENSITIVE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|DB_SEARCHABLE|  
|UNSIGNED_ATTRIBUTE|Null|Null|Null|Null|Null|Null|  
|FIXED_PREC_SCALE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|AUTO_UNIQUE_VALUE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|LOCAL_TYPE_NAME|date|time|smalldatetime|datetime|datetime2|datetimeoffset|  
|MINIMUM_SCALE|Null|Null|Null|Null|Null|Null|  
|MAXIMUM_SCALE|Null|Null|Null|Null|Null|Null|  
|GUID|Null|Null|Null|Null|Null|Null|  
|TYPELIB|Null|Null|Null|Null|Null|Null|  
|VERSION|Null|Null|Null|Null|Null|Null|  
|IS_LONG|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
|BEST_MATCH|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_TRUE|VARIANT_FALSE|VARIANT_FALSE|  
|IS_FIXEDLENGTH|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|VARIANT_FALSE|  
  
## <a name="down-level-server-behavior"></a>下级服务器行为  
 连接到早于的服务器时 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ，尝试使用新的服务器类型名称 (例如，使用 ICommandWithParameters：： SetParameterInfo 或 ITableDefinition：： CreateTable) 会导致 DB_E_BADTYPENAME。  
  
 如果在不使用类型名称的情况下针对参数或结果绑定新类型，且新类型用于隐式指定服务器类型，或服务器类型到客户端类型之间没有有效的转换，则将返回 DB_E_ERRORSOCCURRED，且 DBBINDSTATUS_UNSUPPORTED_CONVERSION 设置为在执行时使用的取值函数的绑定状态。  
  
 如果在连接时支持从缓冲区类型到服务器版本的服务器类型之间的客户端转换，则可以使用所有客户端缓冲区类型。 在此上下文中，如果未调用 ICommandWithParameters：： SetParameterInfo，则 *服务器类型* 表示由 ICommandWithParameters：： SetParameterInfo 指定的类型或由缓冲区类型隐含的类型。 这意味着 DBTYPE_DBTIME2 和 DBTYPE_DBTIMESTAMPOFFSET 可用于下级服务器，或在 DataTypeCompatibility=80 时，对支持的服务器类型的客户端转换成功的情况下，也可使用。 当然，如果服务器类型不正确，且该服务器类型不能执行对实际服务器类型的隐式转换，则服务器仍可能报告错误。  
  
## <a name="ssprop_init_datatypecompatibility-behavior"></a>SSPROP_INIT_DATATYPECOMPATIBILITY 行为  
 将 SSPROP_INIT_DATATYPECOMPATIBILITY 设置为 SSPROPVAL_DATATYPECOMPATIBILITY_SQL2000 时，新的日期/时间类型和关联的元数据将显示在客户端上，因为它们对于下级客户端显示，如 [&#40;OLE DB 和 ODBC&#41;的对增强日期和时间类型的大容量复制更改 ](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md)中所述。  
  
## <a name="comparability-for-irowsetfind"></a>IRowsetFind 的可比性  
 允许对新的日期/时间类型使用所有比较运算符，因为这些比较运算符显示为字符串类型，而不是显示为日期/时间类型。  
  
## <a name="see-also"></a>另请参阅  
 [日期和时间改进 (OLE DB)](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
