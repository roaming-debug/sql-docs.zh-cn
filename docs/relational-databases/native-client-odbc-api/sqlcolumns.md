---
description: SQLColumns
title: SQLColumns |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLColumns function
ms.assetid: 69d3af44-8196-43ab-8037-cdd06207b171
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 73bdc833b31251e3cf0747aca19371ad2b0cb839
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473768"
---
# <a name="sqlcolumns"></a>SQLColumns
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  **SQLColumns** 返回 SQL_SUCCESS *CatalogName*、 *TableName* 或 *ColumnName* 参数是否存在值。 当在这些参数中使用了无效值时， **SQLFetch** 将返回 SQL_NO_DATA。  
  
> [!NOTE]  
>  对于大值类型，将返回值为 SQL_SS_LENGTH_UNLIMITED 的所有长度参数。  
  
 可以对静态服务器游标执行 **SQLColumns** 。 尝试对可更新的 (动态或键集) 游标执行 **SQLColumns** 将返回 SQL_SUCCESS_WITH_INFO，指示游标类型已更改。  
  
 通过接受由两部分组成的 *CatalogName* 参数的名称： *Linked_Server_Name*，> Native Client ODBC 驱动程序支持链接服务器上的表的报告信息。  
  
 对于 ODBC 2。*x* 应用程序未在 *TableName* 中使用通配符， **SQLColumns** 返回有关名称与 *TableName* 匹配并且由当前用户拥有的所有表的信息。 如果当前用户拥有的表的名称与 *tablename* 参数的名称匹配，则 **SQLColumns** 将返回其他用户所拥有的、表名称与 *tablename* 参数匹配的所有表的相关信息。 对于 ODBC 2。使用通配符的 *x* 应用程序， **SQLColumns** 返回其名称与 *TableName* 匹配的所有表。 对于 ODBC 3。*x* 应用程序 **SQLColumns** 返回其名称与 *TableName* 匹配的所有表，而不考虑所有者或是否使用通配符。  
  
 下表列出了结果集返回的列：  
  
|列名称|说明|  
|-----------------|-----------------|  
|DATA_TYPE|返回 **VARCHAR (max)** 数据类型 SQL_VARCHAR、SQL_VARBINARY 或 SQL_WVARCHAR。|  
|TYPE_NAME|为 varchar 返回 "varchar"、"varbinary" 或 "nvarchar" **(max)**、 **varbinary (max)** 和 **nvarchar (max)** 数据类型。|  
|COLUMN_SIZE|对于 **varchar (max)** 数据类型返回 SQL_SS_LENGTH_UNLIMITED，指示列的大小不受限制。|  
|BUFFER_LENGTH|对于 **varchar (max)** 数据类型返回 SQL_SS_LENGTH_UNLIMITED，指示缓冲区的大小不受限制。|  
|SQL_DATA_TYPE|返回 **VARCHAR (max)** 数据类型 SQL_VARCHAR、SQL_VARBINARY 或 SQL_WVARCHAR。|  
|CHAR_OCTET_LENGTH|返回字符或二进制列的最大长度。 返回 0 表示大小不受限制。|  
|SS_XML_SCHEMACOLLECTION_CATALOG_NAME|返回在其中定义 XML 架构集合名称的目录的名称。 如果找不到目录名称，则此变量包含空字符串。|  
|SS_XML_SCHEMACOLLECTION_SCHEMA_NAME|返回在其中定义 XML 架构集合名称的架构的名称。 如果找不到架构名称，则此变量包含空字符串。|  
|SS_XML_SCHEMACOLLECTION_NAME|返回 XML 架构集合的名称。 如果找不到此名称，则此变量包含空字符串。|  
|SS_UDT_CATALOG_NAME|包含 UDT（用户定义类型）的目录的名称。|  
|SS_UDT_SCHEMA_NAME|包含 UDT 的架构的名称。|  
|SS_UDT_ASSEMBLY_TYPE_NAME|UDT 的程序集限定名称。|  
  
 对于 Udt，现有 TYPE_NAME 列用于指示 UDT 的名称;因此，不应将它的其他列添加到 **SQLColumns** 或 [SQLProcedureColumns](../../relational-databases/native-client-odbc-api/sqlprocedurecolumns.md)的结果集中。 UDT 列或参数的 DATA_TYPE 为 SQL_SS_UDT。  
  
 对于 UDT 参数，您可以使用上面定义的特定于驱动程序的新描述符来获取或设置 UDT 的额外元数据数据，条件是服务器返回或需要此信息。  
  
 当客户端连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 并调用 SQLColumns 时，对目录输入参数使用 NULL 或通配符值将不会返回其他目录中的信息。 而只返回有关当前目录的信息。 客户端可以首先调用 SQLTables 来确定所需的表所在的目录。 然后，客户端可以在其对 SQLColumns 的调用中使用目录输入参数的目录值来检索该表中的列的相关信息。  
  
## <a name="sqlcolumns-and-table-valued-parameters"></a>SQLColumns 和表值参数  
 SQLColumns 返回的结果集取决于 SQL_SOPT_SS_NAME_SCOPE 的设置。 有关详细信息，请参阅 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)。 已针对表值参数添加以下列：  
  
|列名称|数据类型|目录|  
|-----------------|---------------|--------------|  
|SS_IS_COMPUTED|Smallint|对于 TABLE_TYPE 中的列，如果该列是一个计算列，则为 SQL_TRUE；否则为 SQL_FALSE。|  
|SS_IS_IDENTITY|Smallint|如果该列为标识列，则为 SQL_TRUE；否则为 SQL_FALSE。|  
  
 有关表值参数的详细信息，请参阅 [ODBC&#41;&#40;表值参数 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)。  
  
## <a name="sqlcolumns-support-for-enhanced-date-and-time-features"></a>SQLColumns 对日期和时间增强功能的支持  
 有关为日期/时间类型返回的值的信息，请参阅 [目录元数据](../../relational-databases/native-client-odbc-date-time/metadata-catalog.md)。  
  
 有关详细信息，请参阅 [ODBC&#41;&#40;日期和时间改进 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlcolumns-support-for-large-clr-udts"></a>SQLColumns 对大型 CLR UDT 的支持  
 **SQLColumns** 支持 (udt) 的大型 CLR 用户定义类型。 有关详细信息，请参阅 [ODBC&#41;&#40;的大型 CLR User-Defined 类型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="sqlcolumns-support-for-sparse-columns"></a>SQLColumns 对稀疏列的支持  
 已将两个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特定列添加到 SQLColumns 的结果集中：  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|SS_IS_SPARSE|**Smallint**|如果该列为稀疏列，则为 SQL_TRUE；否则为 SQL_FALSE。|  
|SS_IS_COLUMN_SET|**Smallint**|如果该列是 **column_set** 列，则 SQL_TRUE;否则，SQL_FALSE。|  
  
 与 ODBC 规范一致，SS_IS_SPARSE 和 SS_IS_COLUMN_SET 会出现在已添加到早于的版本的所有驱动程序特定列之前 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以及 ODBC 本身所强制的所有列之后。  
  
 SQLColumns 返回的结果集取决于 SQL_SOPT_SS_NAME_SCOPE 的设置。 有关详细信息，请参阅 [SQLSetStmtAttr](../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md)。  
  
 有关 ODBC 中稀疏列的详细信息，请参阅 [稀疏列支持 &#40;odbc&#41;](../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLColumns 函数](../../odbc/reference/syntax/sqlcolumns-function.md)   
 [ODBC API 实现细节](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
