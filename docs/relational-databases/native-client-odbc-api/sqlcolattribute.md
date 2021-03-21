---
description: SQLColAttribute
title: SQLColAttribute |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLColAttribute function
ms.assetid: a5387d9e-a243-4cfe-b786-7fad5842b1d6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 4d62e13e7bed632ffed99ef0eab8ddf5cbb02a59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104755087"
---
# <a name="sqlcolattribute"></a>SQLColAttribute
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  您可以使用 **SQLColAttribute** 检索已准备或执行的 ODBC 语句的结果集列的属性。 在已准备好的语句上调用 **SQLColAttribute** 将导致往返 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序接收语句执行过程中的结果集列数据，因此在 **SQLExecute** 或 **SQLExecDirect** 完成后调用 **SQLColAttribute** 不涉及服务器往返。  
  
> [!NOTE]  
>  ODBC 列标识符属性并非可用于所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 结果集。  
  
|字段标识符|说明|  
|----------------------|-----------------|  
|SQL_COLUMN_TABLE_NAME|可用于从生成服务器游标的语句检索的结果集，或从包含 FOR BROWSE 子句的已执行 SELECT 语句检索的结果集。|  
|SQL_DESC_BASE_COLUMN_NAME|可用于从生成服务器游标的语句检索的结果集，或从包含 FOR BROWSE 子句的已执行 SELECT 语句检索的结果集。|  
|SQL_DESC_BASE_TABLE_NAME|可用于从生成服务器游标的语句检索的结果集，或从包含 FOR BROWSE 子句的已执行 SELECT 语句检索的结果集。|  
|SQL_DESC_CATALOG_NAME|数据库名称。 可用于从生成服务器游标的语句检索的结果集，或从包含 FOR BROWSE 子句的已执行 SELECT 语句检索的结果集。|  
|SQL_DESC_LABEL|可用于所有结果集。 此值与 SQL_DESC_NAME 字段的值完全相同。<br /><br /> 仅当某列是表达式的结果，并且表达式不包含标签分配时，此字段的长度才为零。|  
|SQL_DESC_NAME|可用于所有结果集。 此值与 SQL_DESC_LABEL 字段的值完全相同。<br /><br /> 仅当某列是表达式的结果，并且表达式不包含标签分配时，此字段的长度才为零。|  
|SQL_DESC_SCHEMA_NAME|所有者名称。 可用于从生成服务器游标的语句检索的结果集，或从包含 FOR BROWSE 子句的已执行 SELECT 语句检索的结果集。<br /><br /> 仅当在 SELECT 语句中为列指定所有者名称时才可用。|  
|SQL_DESC_TABLE_NAME|可用于从生成服务器游标的语句检索的结果集，或从包含 FOR BROWSE 子句的已执行 SELECT 语句检索的结果集。|  
|SQL_DESC_UNNAMED|对于结果集中的所有列均为 SQL_NAMED，除非某列是不包含标签分配作为表达式一部分的表达式的结果。 当 SQL_DESC_UNNAMED 返回 SQL_UNNAMED 时，所有 ODBC 列标识符属性对于该列都包含零长度字符串。|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 当为已准备但未执行的语句调用 **SQLColAttribute** 时，NATIVE Client ODBC 驱动程序使用 SET FMTONLY 语句来减少服务器开销。  
  
 对于大值类型， **SQLColAttribute** 将返回以下值：  
  
|字段标识符|更改说明|  
|----------------------|---------------------------|  
|SQL_DESC_DISPLAY_SIZE|这是要显示列中数据所需的最大字符数。 对于大值类型列，返回的值为 SQL_SS_LENGTH_UNLIMITED。|  
|SQL_DESC_LENGTH|返回结果集中该列的实际长度。 对于大值类型列，返回的值为 SQL_SS_LENGTH_UNLIMITED。|  
|SQL_DESC_OCTET_LENGTH|返回大值类型列的最大长度。 SQL_SS_LENGTH_UNLIMITED 用于指示大小不受限制。|  
|SQL_DESC_PRECISION|对于大值类型列，返回值 SQL_SS_LENGTH_UNLIMITED。|  
|SQL_DESC_TYPE|对于大值类型，返回 SQL_VARCHAR、SQL_WVARCHAR 和 SQL_VARBINARY。|  
|SQL_DESC_TYPE_NAME|对于大值类型，返回“varchar”、“varbinary”和“nvarchar”。|  
  
 对于所有版本，当已准备的一批 SQL 语句生成多个结果集时，只为第一个结果集报告列属性。  
  
 以下列属性是由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驱动程序公开的扩展。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native CLIENT ODBC 驱动程序将返回 *NumericAttrPtr* 参数中的所有值。 这些值作为 SDWORD（signed long，有符号长值）返回，但 SQL_CA_SS_COMPUTE_BYLIST 除外，它是指向 WORD 数组的指针。  
  
|字段标识符|返回的值|  
|----------------------|--------------------|  
|SQL_CA_SS_COLUMN_HIDDEN*|如果所引用的列为隐藏主键（创建隐藏主键是为了支持包含 FOR BROWSE 的 Transact-SQL SELECT 语句）的一部分，则为 TRUE。|  
|SQL_CA_SS_COLUMN_ID|当前 Transact-SQL SELECT 语句内 COMPUTE 子句结果列的序号位置。|  
|SQL_CA_SS_COLUMN_KEY*|如果所引用的列是该行的主键的一部分且 Transact-SQL SELECT 语句包含 FOR BROWSE，则为 TRUE。|  
|SQL_CA_SS_COLUMN_OP|一个整数，用于指定负责 COMPUTE 子句列中的值的聚合运算符。 整数值的定义位于 sqlncli.h 中。|  
|SQL_CA_SS_COLUMN_ORDER|该列在 ODBC 或 Transact-SQL SELECT 语句的 ORDER BY 子句中的序号位置。|  
|SQL_CA_SS_COLUMN_SIZE|将从列中检索的数据值绑定到 SQL_C_BINARY 变量所需的最大长度（以字节为单位）。|  
|SQL_CA_SS_COLUMN_SSTYPE|SQL Server 列中存储的数据的本机数据类型。 类型值的定义位于 sqlncli.h 中。|  
|SQL_CA_SS_COLUMN_UTYPE|SQL Server 列的用户定义数据类型的基本数据类型。 类型值的定义位于 sqlncli.h 中。|  
|SQL_CA_SS_COLUMN_VARYLEN|如果列的数据在长度方面可能变化，则为 TRUE，否则为 FALSE。|  
|SQL_CA_SS_COMPUTE_BYLIST|指向一个 WORD（unsigned short，无符号短值）数组的指针，该数组指定在 COMPUTE 子句的 BY 短语中使用的列。 如果 COMPUTE 子句未指定 BY 短语，则返回 NULL 指针。<br /><br /> 数组的第一个元素包含 BY 列表列的计数。 其他元素为列序号。|  
|SQL_CA_SS_COMPUTE_ID|*computeid* 是当前 transact-sql SELECT 语句中 COMPUTE 子句的结果的行。|  
|SQL_CA_SS_NUM_COMPUTES|在当前 Transact-SQL SELECT 语句中指定的 COMPUTE 子句的数量。|  
|SQL_CA_SS_NUM_ORDERS|在 ODBC 或 Transact-SQL SELECT 语句的 ORDER BY 子句中指定的列数。|  
  
 \*   如果语句属性 SQL_SOPT_SS_HIDDEN_COLUMNS 设置为 SQL_HC_ON，则可用。  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了驱动程序特定的描述符字段，以提供额外的信息来分别表示 XML 架构集合名称、架构名称和目录名称。 如果这些属性包含非字母数字字符，则它们不需要引号或转义符。 下表列出这些新的描述符字段：  
  
|列名称|类型|说明|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_CATALOG_NAME|CharacterAttributePtr|在其中定义 XML 架构集合名称的目录的名称。 如果找不到目录名称，则此变量包含空字符串。<br /><br /> 将从 IRD 的 SQL_DESC_SS_XML_SCHEMACOLLECTION_CATALOG_NAME 记录字段（此字段是一个读写字段）中返回此信息。|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_SCHEMA_NAM E|CharacterAttributePtr|在其中定义 XML 架构集合名称的架构的名称。 如果找不到架构名称，则此变量包含空字符串。<br /><br /> 将从 IRD 的 SQL_DESC_SS_XML_SCHEMACOLLECTION_SCHEMA_NAME 记录字段（此字段是一个读写字段）中返回此信息。|  
|SQL_CA_SS_XML_SCHEMACOLLECTION_NAME|CharacterAttributePtr|XML 架构集合的名称。 如果找不到此名称，则此变量包含空字符串。<br /><br /> 将从 IRD 的 SQL_DESC_SS_XML_SCHEMACOLLECTION_NAME 记录字段（此字段是一个读写字段）中返回此信息。|  
  
 此外，[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了新的驱动程序特定的描述符字段，以便为结果集的用户定义类型 (UDT) 列或为存储过程或参数化查询的 UDT 参数提供附加信息。 如果这些属性包含非字母数字字符，则它们不需要引号或转义符。 下表列出这些新的描述符字段：  
  
|列名|类型|说明|  
|-----------------|----------|-----------------|  
|SQL_CA_SS_UDT_CATALOG_NAME|CharacterAttributePtr|包含 UDT 的目录的名称。|  
|SQL_CA_SS_UDT_SCHEMA_NAME|CharacterAttributePtr|包含 UDT 的架构的名称。|  
|SQL_CA_SS_UDT_TYPE_NAME|CharacterAttributePtr|UDT 的名称。|  
|SQL_CA_SS_UDT_ASSEMBLY_TYPE_NAME|CharacterAttributePtr|UDT 的程序集完全限定名称。|  
  
 现有的描述符字段标识符 SQL_DESC_TYPE_NAME 用于指示 UDT 的名称。 UDT 类型列的 SQL_DESC_TYPE 字段为 SQL_SS_UDT。  
  
## <a name="sqlcolattribute-support-for-enhanced-date-and-time-features"></a>SQLColAttribute 对日期和时间增强功能的支持  
 有关为日期/时间类型返回的值，请参阅 [参数和结果元数据](../../relational-databases/native-client-odbc-date-time/metadata-parameter-and-result.md)中的 "IRD 字段中返回的信息" 部分。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlcolattribute-support-for-large-clr-udts"></a>SQLColAttribute 对大型 CLR UDT 的支持  
 **SQLColAttribute** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="sqlcolattribute-support-for-sparse-columns"></a>SQLColAttribute 对于稀疏列的支持  
 SQLColAttribute 查询新的实现行描述符 (IRD) 字段 SQL_CA_SS_IS_COLUMN_SET，以确定列是否为 **column_set** 列。  
  
 有关详细信息，请参阅 [稀疏列支持 &#40;ODBC&#41;](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLColAttribute 函数](../../odbc/reference/syntax/sqlcolattribute-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)   
 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)  
  
