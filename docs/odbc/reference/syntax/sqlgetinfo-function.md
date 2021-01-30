---
description: SQLGetInfo 函数
title: SQLGetInfo 函数 |Microsoft Docs
ms.custom: ''
ms.date: 05/28/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetInfo
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetInfo
helpviewer_keywords:
- SQLGetInfo function [ODBC]
ms.assetid: 49dceccc-d816-4ada-808c-4c6138dccb64
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c8ef573f639e10f8b3d0d3c5c1276bcc14a8a378
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180975"
---
# <a name="sqlgetinfo-function"></a>SQLGetInfo 函数

**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetInfo** 返回有关驱动程序的常规信息以及与连接关联的数据源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetInfo(  
     SQLHDBC         ConnectionHandle,  
     SQLUSMALLINT    InfoType,  
     SQLPOINTER      InfoValuePtr,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>参数  

 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *InfoType*  
 送信息的类型。  
  
 *InfoValuePtr*  
 输出指向要返回信息的缓冲区的指针。 根据请求的 *InfoType* ，返回的信息将是以下内容之一： null 结尾的字符串、SQLUSMALLINT 值、SQLUINTEGER 位掩码、SQLUINTEGER 标志、SQLUINTEGER 二进制值或 sqlulen 生成值。  
  
 如果 *InfoType* 参数 SQL_DRIVER_HDESC 或 SQL_DRIVER_HSTMT，则 *InfoValuePtr* 参数既是输入参数又是输出参数。 有关详细信息，请参阅此函数说明后面的 SQL_DRIVER_HDESC 或 SQL_DRIVER_HSTMT 描述符 (。 )   
  
 如果 *InfoValuePtr* 为 NULL，则 *StringLengthPtr* 仍将返回 (排除字符数据的 NULL 终止字符之外的总字节数) 可在 *InfoValuePtr* 所指向的缓冲区中返回。  
  
 *BufferLength*  
 送\* *InfoValuePtr* 缓冲区的长度。 如果 *\* InfoValuePtr* 中的值不是字符串或 *InfoValuePtr* 为 Null 指针，则忽略 *BufferLength* 参数。 该驱动程序假设 *\* InfoValuePtr* 的大小为 SQLUSMALLINT 或 SQLUINTEGER，具体取决于 *InfoType*。 如果在调用 **SQLGetInfoW**) 时， *\* InfoValuePtr* 是 Unicode 字符串 (，则 *BufferLength* 参数必须是偶数; 如果不是，则 SQLSTATE HY090 (返回的字符串或缓冲区长度) 无效。  
  
 *StringLengthPtr*  
 输出指向缓冲区的指针，该缓冲区返回的字节总数 (排除在 **InfoValuePtr* 中可返回的字符数据) 的 null 终止字符。  
  
 对于字符数据，如果可返回的字节数大于或等于 *BufferLength*，则 InfoValuePtr 中的信息将 \* 截断为 *BufferLength* 字节减去 null 终止字符的长度，并且由驱动程序以 null 结尾。  
  
 对于所有其他类型的数据， *BufferLength* 的值将被忽略，驱动程序假定 InfoValuePtr 的大小 \* 为 SQLUSMALLINT 或 SQLUINTEGER，具体取决于 *InfoType*。  
  
## <a name="returns"></a>返回  

 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  

 当 **SQLGetInfo** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DBC 和 *ConnectionHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetInfo** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|缓冲区 \* *InfoValuePtr* 不够大，无法返回所有请求的信息。 因此，此信息已被截断。 在 **StringLengthPtr* 中返回所请求信息的未截断形式的长度。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08003|连接未打开| (DM) *InfoType* 中请求的信息类型需要打开的连接。 对于 ODBC 保留的信息类型，只有在没有打开的连接的情况下才能返回 SQL_ODBC_VER。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY024|属性值无效| (DM) *InfoType* 参数已 SQL_DRIVER_HSTMT，并且 *InfoValuePtr* 指向的值不是有效的语句句柄。<br /><br />  (DM) *InfoType* 参数已 SQL_DRIVER_HDESC，并且 *InfoValuePtr* 指向的值不是有效的描述符句柄。|  
|HY090|字符串或缓冲区长度无效| (DM) 为参数 *BufferLength* 指定的值小于0。<br /><br />  (DM) 为 *BufferLength* 指定的值是奇数，而 *\* InfoValuePtr* 是 Unicode 数据类型。|  
|HY096|信息类型超出范围|为参数 *InfoType* 指定的值对于该驱动程序支持的 ODBC 版本无效。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](sqlendtran-function.md)。|  
|HYC00|未实现可选字段|为参数 *InfoType* 指定的值是驱动程序不支持的特定于驱动程序的值。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *ConnectionHandle* 相对应的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  

 本部分后面的 "信息类型" 中显示了当前定义的信息类型;应该会定义更多的，以利用不同的数据源。 信息类型的范围由 ODBC 保留;驱动程序开发人员必须在开放组中保留其自己的特定于驱动程序的使用值。 **SQLGetInfo** 不执行 Unicode 转换或 *thunk* (请参阅附录 A： *Odbc 程序员参考* 的 [odbc 错误代码](../appendixes/appendix-a-odbc-error-codes.md)) 用于驱动程序定义的 *InfoTypes*。 有关详细信息，请参阅 [驱动程序特定的数据类型、描述符类型、信息类型、诊断类型和属性](../develop-app/driver-specific-data-types-descriptor-information-diagnostic.md)。 InfoValuePtr 中返回的信息的格式 \* 取决于所请求的 *InfoType* 。 **SQLGetInfo** 将返回以五种不同格式之一返回的信息：  
  
- 以 null 结尾的字符串  
  
- SQLUSMALLINT 值  
  
- SQLUINTEGER 位掩码  
  
- SQLUINTEGER 值  
  
- SQLUINTEGER 二进制值  
  
 类型的说明中注明了以下每种信息类型的格式。 应用程序必须在 **InfoValuePtr* 中相应地强制转换返回的值。 有关应用程序如何从 SQLUINTEGER 位掩码检索数据的示例，请参阅 "代码示例"。  
  
 对于下表中定义的每个信息类型，驱动程序必须返回一个值。 如果信息类型不适用于驱动程序或数据源，则驱动程序将返回下表中列出的值之一。  

|信息类型|值|
|-|-|
|字符串 ( "Y" 或 "N" ) |"N"|
|字符串 (不是 "Y" 或 "N" ) |空字符串|
|SQLUSMALLINT|0|
|SQLUINTEGER 位掩码或 SQLUINTEGER 二进制值|0L|
  
 例如，如果数据源不支持过程，则 **SQLGetInfo** 将返回下表中列出的值，以获取与过程相关的 *InfoType* 的值。  

|InfoType|值|
|-|-|
|SQL_PROCEDURES|"N"|
|SQL_ACCESSIBLE_PROCEDURES|"N"|
|SQL_MAX_PROCEDURE_NAME_LEN|0|
|SQL_PROCEDURE_TERM|空字符串|
  
 **SQLGetInfo** 返回 SQLSTATE HY096 (的值的参数) 值无效 *，这些* 值在为 ODBC 使用而保留的信息类型范围中，但不是由驱动程序支持的 ODBC 版本定义的。 若要确定驱动程序符合的 ODBC 版本，应用程序需要使用 SQL_DRIVER_ODBC_VER 信息类型调用 **SQLGetInfo** 。 **SQLGetInfo** 返回 SQLSTATE HYC00 (可选功能，该功能未实现) 的值，这些值在为驱动程序特定用途而保留但不受驱动程序 *支持的信息* 类型范围内。  
  
 对 **SQLGetInfo** 的所有调用都需要打开连接，但当 *InfoType* 为 SQL_ODBC_VER 时，这将返回驱动程序管理器的版本。  
  
## <a name="information-types"></a>信息类型  

 本部分列出了 **SQLGetInfo** 支持的信息类型。 信息类型按明确分组，并按字母顺序列出。 还 *列出了为* ODBC 3.x 添加或重命名的信息类型。  
  
## <a name="driver-information"></a>驱动程序信息  

 *InfoType* 参数的以下值返回有关 ODBC 驱动程序的信息，如活动语句的数目、数据源名称和接口标准符合性级别：  

:::row:::
    :::column:::
        SQL_ACTIVE_ENVIRONMENTS  
        SQL_ASYNC_DBC_FUNCTIONS  
        SQL_ASYNC_MODE  
        SQL_ASYNC_NOTIFICATION  
        SQL_BATCH_ROW_COUNT  
        SQL_BATCH_SUPPORT  
        SQL_DATA_SOURCE_NAME  
        SQL_DRIVER_AWARE_POOLING_SUPPORTED  
        SQL_DRIVER_HDBC  
        SQL_DRIVER_HDESC  
        SQL_DRIVER_HENV  
        SQL_DRIVER_HLIB  
        SQL_DRIVER_HSTMT  
        SQL_DRIVER_NAME  
        SQL_DRIVER_ODBC_VER  
        SQL_DRIVER_VER  
        SQL_DYNAMIC_CURSOR_ATTRIBUTES1  
        SQL_DYNAMIC_CURSOR_ATTRIBUTES2  
        SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1  
    :::column-end:::
    :::column:::
        SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2  
        SQL_FILE_USAGE  
        SQL_GETDATA_EXTENSIONS  
        SQL_INFO_SCHEMA_VIEWS  
        SQL_KEYSET_CURSOR_ATTRIBUTES1  
        SQL_KEYSET_CURSOR_ATTRIBUTES2  
        SQL_MAX_ASYNC_CONCURRENT_STATEMENTS  
        SQL_MAX_CONCURRENT_ACTIVITIES  
        SQL_MAX_DRIVER_CONNECTIONS  
        SQL_ODBC_INTERFACE_CONFORMANCE  
        SQL_ODBC_STANDARD_CLI_CONFORMANCE  
        SQL_ODBC_VER  
        SQL_PARAM_ARRAY_ROW_COUNTS  
        SQL_PARAM_ARRAY_SELECTS  
        SQL_ROW_UPDATES  
        SQL_SEARCH_PATTERN_ESCAPE  
        SQL_SERVER_NAME  
        SQL_STATIC_CURSOR_ATTRIBUTES1  
        SQL_STATIC_CURSOR_ATTRIBUTES2  
    :::column-end:::
:::row-end:::

> [!NOTE]  
> 实现 **SQLGetInfo** 时，驱动程序可以通过最大程度地减少从服务器发送或请求信息的次数来提高性能。  
  
## <a name="dbms-product-information"></a>DBMS 产品信息  

 以下 *InfoType* 参数值将返回有关 dbms 产品的信息，例如 dbms 名称和版本：  

:::row:::
    :::column:::
        SQL_DATABASE_NAME  
        SQL_DBMS_NAME  
    :::column-end:::
    :::column:::
        SQL_DBMS_VER  
    :::column-end:::
:::row-end:::

## <a name="data-source-information"></a>数据源信息  

 以下 *InfoType* 参数值将返回有关数据源的信息，例如游标特性和事务功能：  

:::row:::
    :::column:::
        SQL_ACCESSIBLE_PROCEDURES  
        SQL_ACCESSIBLE_TABLES  
        SQL_BOOKMARK_PERSISTENCE  
        SQL_CATALOG_TERM  
        SQL_COLLATION_SEQ  
        SQL_CONCAT_NULL_BEHAVIOR  
        SQL_CURSOR_COMMIT_BEHAVIOR  
        SQL_CURSOR_ROLLBACK_BEHAVIOR  
        SQL_CURSOR_SENSITIVITY  
        SQL_DATA_SOURCE_READ_ONLY  
        SQL_DEFAULT_TXN_ISOLATION  
        SQL_DESCRIBE_PARAMETER  
    :::column-end:::
    :::column:::
        SQL_MULT_RESULT_SETS  
        SQL_MULTIPLE_ACTIVE_TXN  
        SQL_NEED_LONG_DATA_LEN  
        SQL_NULL_COLLATION  
        SQL_PROCEDURE_TERM  
        SQL_SCHEMA_TERM  
        SQL_SCROLL_OPTIONS  
        SQL_TABLE_TERM  
        SQL_TXN_CAPABLE  
        SQL_TXN_ISOLATION_OPTION  
        SQL_USER_NAME  
    :::column-end:::
:::row-end:::

## <a name="supported-sql"></a>支持的 SQL  

 以下 *InfoType* 参数值将返回有关数据源支持的 SQL 语句的信息。 这些信息类型描述的每个功能的 SQL 语法是 SQL-92 语法。 这些信息类型不会详尽描述整个 SQL-92 语法。 相反，它们描述了语法的这些部分，数据源通常提供不同级别的支持。 具体说来，涵盖了 SQL-92 中的大部分 DDL 语句。  
  
 应用程序应从 SQL_SQL_CONFORMANCE 信息类型确定支持的常规语法级别，并使用其他信息类型来确定所规定的标准符合性级别的变化。  

:::row:::
    :::column:::
        SQL_AGGREGATE_FUNCTIONS  
        SQL_ALTER_DOMAIN  
        SQL_ALTER_SCHEMA  
        SQL_ALTER_TABLE  
        SQL_ANSI_SQL_DATETIME_LITERALS  
        SQL_CATALOG_LOCATION  
        SQL_CATALOG_NAME  
        SQL_CATALOG_NAME_SEPARATOR  
        SQL_CATALOG_USAGE  
        SQL_COLUMN_ALIAS  
        SQL_CORRELATION_NAME  
        SQL_CREATE_ASSERTION  
        SQL_CREATE_CHARACTER_SET  
        SQL_CREATE_COLLATION  
        SQL_CREATE_DOMAIN  
        SQL_CREATE_SCHEMA  
        SQL_CREATE_TABLE  
        SQL_CREATE_TRANSLATION  
        SQL_DDL_INDEX  
        SQL_DROP_ASSERTION  
        SQL_DROP_CHARACTER_SET  
        SQL_DROP_COLLATION  
        SQL_DROP_DOMAIN  
        SQL_DROP_SCHEMA  
    :::column-end:::
    :::column:::
        SQL_DROP_TABLE  
        SQL_DROP_TRANSLATION  
        SQL_DROP_VIEW  
        SQL_EXPRESSIONS_IN_ORDERBY  
        SQL_GROUP_BY  
        SQL_IDENTIFIER_CASE  
        SQL_IDENTIFIER_QUOTE_CHAR  
        SQL_INDEX_KEYWORDS  
        SQL_INSERT_STATEMENT  
        SQL_INTEGRITY  
        SQL_KEYWORDS  
        SQL_LIKE_ESCAPE_CLAUSE  
        SQL_NON_NULLABLE_COLUMNS  
        SQL_OJ_CAPABILITIES  
        SQL_ORDER_BY_COLUMNS_IN_SELECT  
        SQL_OUTER_JOINS  
        SQL_PROCEDURES  
        SQL_QUOTED_IDENTIFIER_CASE  
        SQL_SCHEMA_USAGE  
        SQL_SPECIAL_CHARACTERS  
        SQL_SQL_CONFORMANCE  
        SQL_SUBQUERIES  
        SQL_UNION  
    :::column-end:::
:::row-end:::

## <a name="sql-limits"></a>SQL 限制  

 *InfoType* 参数的以下值返回有关应用于 SQL 语句中的标识符和子句的限制的信息，例如标识符的最大长度和选择列表中的最大列数。 驱动程序或数据源可以施加限制。  

:::row:::
    :::column:::
        SQL_MAX_BINARY_LITERAL_LEN  
        SQL_MAX_CATALOG_NAME_LEN  
        SQL_MAX_CHAR_LITERAL_LEN  
        SQL_MAX_COLUMN_NAME_LEN  
        SQL_MAX_COLUMNS_IN_GROUP_BY  
        SQL_MAX_COLUMNS_IN_INDEX  
        SQL_MAX_COLUMNS_IN_ORDER_BY  
        SQL_MAX_COLUMNS_IN_SELECT  
        SQL_MAX_COLUMNS_IN_TABLE  
        SQL_MAX_CURSOR_NAME_LEN  
    :::column-end:::
    :::column:::
        SQL_MAX_IDENTIFIER_LEN  
        SQL_MAX_INDEX_SIZE  
        SQL_MAX_PROCEDURE_NAME_LEN  
        SQL_MAX_ROW_SIZE  
        SQL_MAX_ROW_SIZE_INCLUDES_LONG  
        SQL_MAX_SCHEMA_NAME_LEN  
        SQL_MAX_STATEMENT_LEN  
        SQL_MAX_TABLE_NAME_LEN  
        SQL_MAX_TABLES_IN_SELECT  
        SQL_MAX_USER_NAME_LEN  
    :::column-end:::
:::row-end:::

## <a name="scalar-function-information"></a>标量函数信息  

 以下 *InfoType* 参数值将返回有关数据源和驱动程序支持的标量函数的信息。 有关标量函数的详细信息，请参阅 [附录 E：标量函数](../appendixes/appendix-e-scalar-functions.md)。  

:::row:::
    :::column:::
        SQL_CONVERT_FUNCTIONS  
        SQL_NUMERIC_FUNCTIONS  
        SQL_STRING_FUNCTIONS  
        SQL_SYSTEM_FUNCTIONS  
    :::column-end:::
    :::column:::
        SQL_TIMEDATE_ADD_INTERVALS  
        SQL_TIMEDATE_DIFF_INTERVALS  
        SQL_TIMEDATE_FUNCTIONS  
    :::column-end:::
:::row-end:::

## <a name="conversion-information"></a>转换信息  

 *InfoType* 参数的以下值返回 SQL 数据类型的列表，数据源可将指定的 sql 数据类型转换为 **转换** 标量函数：  

:::row:::
    :::column:::
        SQL_CONVERT_BIGINT  
        SQL_CONVERT_BINARY  
        SQL_CONVERT_BIT  
        SQL_CONVERT_CHAR  
        SQL_CONVERT_DATE  
        SQL_CONVERT_DECIMAL  
        SQL_CONVERT_DOUBLE  
        SQL_CONVERT_FLOAT  
        SQL_CONVERT_INTEGER  
        SQL_CONVERT_INTERVAL_DAY_TIME  
        SQL_CONVERT_INTERVAL_YEAR_MONTH  
    :::column-end:::
    :::column:::
        SQL_CONVERT_LONGVARBINARY  
        SQL_CONVERT_LONGVARCHAR  
        SQL_CONVERT_NUMERIC  
        SQL_CONVERT_REAL  
        SQL_CONVERT_SMALLINT  
        SQL_CONVERT_TIME  
        SQL_CONVERT_TIMESTAMP  
        SQL_CONVERT_TINYINT  
        SQL_CONVERT_VARBINARY  
        SQL_CONVERT_VARCHAR  
    :::column-end:::
:::row-end:::

## <a name="information-types-added-for-odbc-3x"></a>为 ODBC 3.x 添加的信息类型  

 为 ODBC 3.x 添加了以下 *InfoType* 参数值：  

:::row:::
    :::column:::
        SQL_ACTIVE_ENVIRONMENTS  
        SQL_AGGREGATE_FUNCTIONS  
        SQL_ALTER_DOMAIN  
        SQL_ALTER_SCHEMA  
        SQL_ANSI_SQL_DATETIME_LITERALS  
        SQL_ASYNC_DBC_FUNCTIONS  
        SQL_ASYNC_MODE  
        SQL_ASYNC_NOTIFICATION  
        SQL_BATCH_ROW_COUNT  
        SQL_BATCH_SUPPORT  
        SQL_CATALOG_NAME  
        SQL_COLLATION_SEQ  
        SQL_CONVERT_INTERVAL_DAY_TIME  
        SQL_CONVERT_INTERVAL_YEAR_MONTH  
        SQL_CREATE_ASSERTION  
        SQL_CREATE_CHARACTER_SET  
        SQL_CREATE_COLLATION  
        SQL_CREATE_DOMAIN  
        SQL_CREATE_SCHEMA  
        SQL_CREATE_TABLE  
        SQL_CREATE_TRANSLATION  
        SQL_CURSOR_SENSITIVITY  
        SQL_DDL_INDEX  
        SQL_DESCRIBE_PARAMETER  
        SQL_DM_VER  
    :::column-end:::
    :::column:::
        SQL_DRIVER_AWARE_POOLING_SUPPORTED  
        SQL_DRIVER_HDESC  
        SQL_DROP_ASSERTION  
        SQL_DROP_CHARACTER_SET  
        SQL_DROP_COLLATION  
        SQL_DROP_DOMAIN  
        SQL_DROP_SCHEMA  
        SQL_DROP_TABLE  
        SQL_DROP_TRANSLATION  
        SQL_DROP_VIEW  
        SQL_DYNAMIC_CURSOR_ATTRIBUTES1  
        SQL_DYNAMIC_CURSOR_ATTRIBUTES2  
        SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1  
        SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2  
        SQL_INFO_SCHEMA_VIEWS  
        SQL_INSERT_STATEMENT  
        SQL_KEYSET_CURSOR_ATTRIBUTES1  
        SQL_KEYSET_CURSOR_ATTRIBUTES2  
        SQL_MAX_ASYNC_CONCURRENT_STATEMENTS  
        SQL_MAX_IDENTIFIER_LEN  
        SQL_PARAM_ARRAY_ROW_COUNTS  
        SQL_PARAM_ARRAY_SELECTS  
        SQL_STATIC_CURSOR_ATTRIBUTES1  
        SQL_STATIC_CURSOR_ATTRIBUTES2  
        SQL_XOPEN_CLI_YEAR  
    :::column-end:::
:::row-end:::

## <a name="information-types-renamed-for-odbc-3x"></a>为 ODBC 2.x 重命名的信息类型  

 为 ODBC 2.x 重命名了以下 *InfoType* 参数值。  

|旧名称|新名称|  
|-|-|  
|SQL_ACTIVE_CONNECTIONS|SQL_MAX_DRIVER_CONNECTIONS|
|SQL_ACTIVE_STATEMENTS|SQL_MAX_CONCURRENT_ACTIVITIES|
|SQL_MAX_OWNER_NAME_LEN|SQL_MAX_SCHEMA_NAME_LEN|
|SQL_MAX_QUALIFIER_NAME_LEN|SQL_MAX_CATALOG_NAME_LEN|
|SQL_ODBC_SQL_OPT_IEF|SQL_INTEGRITY|
|SQL_OWNER_TERM|SQL_SCHEMA_TERM|
|SQL_OWNER_USAGE|SQL_SCHEMA_USAGE|
|SQL_QUALIFIER_LOCATION|SQL_CATALOG_LOCATION|
|SQL_QUALIFIER_NAME_SEPARATOR|SQL_CATALOG_NAME_SEPARATOR|
|SQL_QUALIFIER_TERM|SQL_CATALOG_TERM|
|SQL_QUALIFIER_USAGE|SQL_CATALOG_USAGE|
  
## <a name="information-types-deprecated-in-odbc-3x"></a>ODBC 2.x 中弃用的信息类型  

 ODBC 2.x 中的 *InfoType* 参数值已弃用。 ODBC 2.x 驱动程序必须继续支持这些信息类型，以实现与 ODBC 2.x 应用程序的向后兼容性。  (有关这些类型的详细信息，请参阅附录 G：驱动程序准则中的 [SQLGetInfo 支持](../appendixes/sqlgetinfo-support.md) ，以获得向后兼容性。 )   

:::row:::
    :::column:::
        SQL_FETCH_DIRECTION  
        SQL_LOCK_TYPES  
        SQL_ODBC_API_CONFORMANCE  
        SQL_ODBC_SQL_CONFORMANCE  
    :::column-end:::
    :::column:::
        SQL_POS_OPERATIONS  
        SQL_POSITIONED_STATEMENTS  
        SQL_SCROLL_CONCURRENCY  
        SQL_STATIC_SENSITIVITY  
    :::column-end:::
:::row-end:::

## <a name="information-type-descriptions"></a>信息类型说明  

下表按字母顺序列出了每个信息类型、在其中引入了它的 ODBC 版本及其说明。  
  
|信息类型|ODBC 版本|说明|
|-|-|-|
|SQL_ACCESSIBLE_PROCEDURES|1.0|字符串： "Y" （如果用户可执行 **SQLProcedures** 返回的所有过程）;如果可能会返回用户无法执行的过程，则为 "N"。|
|SQL_ACCESSIBLE_TABLES|1.0|字符串： "Y"。如果保证用户 **选择** 了 **SQLTables** 返回的所有表的权限，则为;如果可能存在用户无法访问的表，则为 "N"。|
|SQL_ACTIVE_ENVIRONMENTS|3.0|一个 SQLUSMALLINT 值，指定驱动程序可以支持的最大活动环境数。 如果没有指定限制或限制未知，此值将设置为零。|
|SQL_AGGREGATE_FUNCTIONS|3.0|SQLUINTEGER 位掩码枚举对聚合函数的支持：<br/>SQL_AF_ALL<br/>SQL_AF_AVG<br/>SQL_AF_COUNT<br/>SQL_AF_DISTINCT<br/>SQL_AF_MAX<br/>SQL_AF_MIN<br/>SQL_AF_SUM<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回受支持的所有选项。|
|SQL_ALTER_DOMAIN|3.0|SQLUINTEGER 位掩码，根据数据源所支持的 **ALTER DOMAIN** 语句（如 SQL-92 中所定义）枚举子句。 与 SQL 92 完全兼容级别的驱动程序将始终返回所有的位屏蔽。 如果返回值为 "0"，则表示不支持 **ALTER DOMAIN** 语句。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_AD_ADD_DOMAIN_CONSTRAINT = (完整级别支持添加域约束) <br/>支持 SQL_AD_ADD_DOMAIN_DEFAULT = \<alter domain> \<set domain default clause> (完全级别) <br/>SQL_AD_CONSTRAINT_NAME_DEFINITION = \<constraint name definition clause> 支持命名域约束 (中间级别) <br/>支持 SQL_AD_DROP_DOMAIN_CONSTRAINT = \<drop domain constraint clause> (完全级别) <br/>支持 SQL_AD_DROP_DOMAIN_DEFAULT = \<alter domain> \<drop domain default clause> (完全级别) <br/><br/>\<constraint attributes>如果 \<add domain constraint> (SQL_AD_ADD_DOMAIN_CONSTRAINT 位设置) ，则以下位指定支持的（如果支持）：<br/>SQL_AD_ADD_CONSTRAINT_DEFERRABLE (完整级别) <br/>SQL_AD_ADD_CONSTRAINT_NON_DEFERRABLE (完整级别) <br/>SQL_AD_ADD_CONSTRAINT_INITIALLY_DEFERRED (完整级别) <br/>SQL_AD_ADD_CONSTRAINT_INITIALLY_IMMEDIATE (完整级别) |
|SQL_ALTER_TABLE|2.0|SQLUINTEGER 位掩码，用于枚举数据源支持的 **ALTER TABLE** 语句中的子句。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定支持的子句：<br/>支持 SQL_AT_ADD_COLUMN_COLLATION = \<add column> 子句，具有指定列排序规则的工具可 (完整级别)  (ODBC 3.0) <br/>支持 SQL_AT_ADD_COLUMN_DEFAULT = \<add column> 子句，该工具用于指定列默认值 (FIPS 过渡级别)  (ODBC 3.0) <br/>\<add column>)  (ODBC 3.0 (FIPS 过渡级别支持 SQL_AT_ADD_COLUMN_SINGLE =) <br/>支持 SQL_AT_ADD_CONSTRAINT = \<add column> 子句，该工具用于指定列约束 (FIPS 过渡级别)  (ODBC 3.0) <br/>SQL_AT_ADD_TABLE_CONSTRAINT = \<add table constraint> 子句支持 (FIPS 过渡级别)  (ODBC 3.0) <br/>SQL_AT_CONSTRAINT_NAME_DEFINITION = \<constraint name definition> 支持)  (ODBC 3.0 (中间级别命名列和表约束) <br/>SQL_AT_DROP_COLUMN_CASCADE = \<drop column> CASCADE (FIPS 过渡级别)  (ODBC 3.0) <br/>\<alter column> \<drop column default clause>)  (ODBC) 3.0 (SQL_AT_DROP_COLUMN_DEFAULT = 支持<br/>支持 SQL_AT_DROP_COLUMN_RESTRICT = \<drop column> RESTRICT (FIPS 过渡级别)  (ODBC 3.0) <br/>SQL_AT_DROP_TABLE_CONSTRAINT_CASCADE (ODBC 3.0) <br/>支持 SQL_AT_DROP_TABLE_CONSTRAINT_RESTRICT = \<drop column> RESTRICT (FIPS 过渡级别)  (ODBC 3.0) <br/>\<alter column> \<set column default clause>)  (ODBC) 3.0 (SQL_AT_SET_COLUMN_DEFAULT = 支持<br/><br/>如果 (SQL_AT_ADD_CONSTRAINT 位设置) ，则以下位指定支持指定 \<constraint attributes> 列或表约束：<br/> (ODBC 3.0 (SQL_AT_CONSTRAINT_INITIALLY_DEFERRED 完全级别) ) <br/> (ODBC 3.0 (SQL_AT_CONSTRAINT_INITIALLY_IMMEDIATE 完全级别) ) <br/> (ODBC 3.0 (SQL_AT_CONSTRAINT_DEFERRABLE 完全级别) ) <br/> (ODBC 3.0 (SQL_AT_CONSTRAINT_NON_DEFERRABLE 完全级别) ) |
|SQL_ASYNC_DBC_FUNCTIONS|3.8|一个 SQLUINTEGER 值，该值指示驱动程序是否可以在连接句柄上异步执行函数。<br/><br/>SQL_ASYNC_DBC_CAPABLE = 驱动程序可以异步执行连接函数。<br/>SQL_ASYNC_DBC_NOT_CAPABLE = 驱动程序无法以异步方式执行连接函数。|
|SQL_ASYNC_MODE|3.0|一个 SQLUINTEGER 值，该值指示驱动程序中的异步支持级别：<br/><br/>SQL_AM_CONNECTION = 支持连接级别的异步执行。 与给定连接句柄关联的所有语句句柄都处于异步模式或全部处于同步模式。 当相同连接上的另一个语句句柄处于同步模式时，连接上的语句句柄不能处于异步模式，反之亦然。<br/>支持 SQL_AM_STATEMENT = 语句级别的异步执行。 与连接句柄关联的某些语句句柄可以处于异步模式，而相同连接上的其他语句句柄处于同步模式。<br/>SQL_AM_NONE = 不支持异步模式。|
|SQL_ASYNC_NOTIFICATION|3.8|一个 SQLUINTEGER 值，该值指示驱动程序是否支持异步通知：<br/><br/>SQL_ASYNC_NOTIFICATION_CAPABLE = 驱动程序支持异步执行通知。<br/>SQL_ASYNC_NOTIFICATION_NOT_CAPABLE = 驱动程序不支持异步执行通知。<br/><br/>ODBC 异步操作有两种类别：连接级别异步操作和语句级异步操作。  如果驱动程序返回 SQL_ASYNC_NOTIFICATION_CAPABLE，则该驱动程序必须支持它可异步执行的所有 Api 的通知。|
|SQL_BATCH_ROW_COUNT|3.0|枚举与行计数的可用性有关的驱动程序行为的 SQLUINTEGER 位掩码。 以下位掩码与信息类型一起使用：<br/><br/>SQL_BRC_ROLLED_UP = 连续 INSERT、DELETE 或 UPDATE 语句的行计数汇总到其中。 如果未设置此位，则每个语句都可以使用行计数。<br/>当在存储过程中执行批处理时，SQL_BRC_PROCEDURES = 行计数（如果有）可用。 如果行计数可用，则可以根据 SQL_BRC_ROLLED_UP 位汇总或单独汇总它们。<br/>SQL_BRC_EXPLICIT = 行计数（如果有）在通过调用 **SQLExecute** 或 **SQLExecDirect** 直接执行批处理时可用。 如果行计数可用，则可以根据 SQL_BRC_ROLLED_UP 位汇总或单独汇总它们。|
|SQL_BATCH_SUPPORT|3.0|SQLUINTEGER 位掩码，用于枚举驱动程序对批的支持。 以下位掩码用于确定支持的级别：<br/><br/>SQL_BS_SELECT_EXPLICIT = 驱动程序支持可以具有结果集生成语句的显式批处理。<br/>SQL_BS_ROW_COUNT_EXPLICIT = 驱动程序支持可具有行计数生成语句的显式批处理。<br/>SQL_BS_SELECT_PROC = 驱动程序支持可以具有结果集生成语句的显式过程。<br/>SQL_BS_ROW_COUNT_PROC = 驱动程序支持可包含行计数生成语句的显式过程。|
|SQL_BOOKMARK_PERSISTENCE|2.0|一个 SQLUINTEGER 位掩码，用于枚举书签要保存到的操作。 以下位掩码与标志一起使用，以确定书签的保留顺序：<br/><br/>SQL_BP_CLOSE = 在应用程序使用 SQL_CLOSE 选项调用 **SQLFreeStmt** 或 **SQLCloseCursor** 关闭与语句关联的游标后，书签是有效的。<br/>SQL_BP_DELETE = 在删除行后，该行的书签是有效的。<br/>SQL_BP_DROP = 书签在应用 **程序使用 SQL_HANDLE_STMT 的** *HandleType* 删除语句后有效。<br/>SQL_BP_TRANSACTION = 在应用程序提交或回滚事务后，书签是有效的。<br/>SQL_BP_UPDATE = 行的书签在更新行中的任何列后有效，包括键列。<br/>SQL_BP_OTHER_HSTMT = 与一个语句相关联的书签可与其他语句一起使用。 除非指定 SQL_BP_CLOSE 或 SQL_BP_DROP，否则必须打开第一条语句中的游标。|
|SQL_CATALOG_LOCATION|2.0|一个 SQLUSMALLINT 值，指示目录在限定的表名中的位置：<br/><br/>SQL_CL_START<br/>SQL_CL_END<br/>例如，Xbase 驱动程序返回 SQL_CL_START，因为目录 (目录) 名称位于表名的开头，如 \EMPDATA\EMP. 中所示。.DBF. ORACLE 服务器驱动程序返回 SQL_CL_END，因为目录位于表名称的末尾，如中所示 ADMIN.EMP@EMPDATA 。<br/><br/>与 SQL 92 完全符合级别的驱动程序将始终返回 SQL_CL_START。 如果数据源不支持目录，则返回值0。 若要确定是否支持目录，应用程序需要使用 SQL_CATALOG_NAME 信息类型调用 **SQLGetInfo** 。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_QUALIFIER_LOCATION 重命名为 odbc 3.0。|
|SQL_CATALOG_NAME|3.0|如果服务器支持目录名称，则为字符串： "Y"; 否则为 "N"。<br/><br/>与 SQL 92 完全符合级别的驱动程序将始终返回 "Y"。|
|SQL_CATALOG_NAME_SEPARATOR|1.0|字符串：数据源定义为目录名称和它后面或前面的限定名称元素之间的分隔符的字符。<br/><br/>如果数据源不支持目录，则返回空字符串。 若要确定是否支持目录，应用程序需要使用 SQL_CATALOG_NAME 信息类型调用 **SQLGetInfo** 。 与 SQL 92 完全符合级别的驱动程序将始终返回 "."。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_QUALIFIER_NAME_SEPARATOR 重命名为 odbc 3.0。|
|SQL_CATALOG_TERM|1.0|包含一个目录的数据源供应商名称的字符串;例如 "数据库" 或 "目录"。 此字符串可以是大写、小写还是混合大小写。<br/><br/>如果数据源不支持目录，则返回空字符串。 若要确定是否支持目录，应用程序需要使用 SQL_CATALOG_NAME 信息类型调用 **SQLGetInfo** 。 与 SQL 92 完全符合级别的驱动程序将始终返回 "catalog"。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_QUALIFIER_TERM 重命名为 odbc 3.0。|
|SQL_CATALOG_USAGE|2.0|SQLUINTEGER 位掩码，用于枚举可使用目录的语句。<br/><br/>以下位掩码用于确定可以使用目录的位置：<br/>在所有数据操作语言语句中均支持 SQL_CU_DML_STATEMENTS = 目录： **选择**、 **插入**、 **更新**、 **删除**，如果受支持，则 **选择** 更新和定位更新和删除语句。<br/>ODBC 过程调用语句中支持 SQL_CU_PROCEDURE_INVOCATION = 目录。<br/>所有表定义语句均支持 SQL_CU_TABLE_DEFINITION = 目录： **CREATE TABLE**、 **CREATE VIEW**、 **ALTER TABLE**、 **drop table** 和 **drop VIEW**。<br/>所有索引定义语句均支持 SQL_CU_INDEX_DEFINITION = 目录： **创建索引** 和 **DROP INDEX**。<br/>所有权限定义语句均支持 SQL_CU_PRIVILEGE_DEFINITION = 目录： **GRANT** 和 **REVOKE**。<br/><br/>如果数据源不支持目录，则返回值0。 若要确定是否支持目录，应用程序需要使用 SQL_CATALOG_NAME 信息类型调用 **SQLGetInfo** 。 符合 SQL-92 完全符合级别的驱动程序将始终返回一个位掩码并设置所有这些位。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_QUALIFIER_USAGE 重命名为 odbc 3.0。|
|SQL_COLLATION_SEQ|3.0|排序规则序列的名称。 这是一个字符串，它指示此服务器的默认字符集的默认排序规则名称 (例如，"ISO 8859-1" 或 "EBCDIC) "。 如果这是未知的，则返回空字符串。 与 SQL 92 完全符合级别的驱动程序将始终返回非空字符串。|
|SQL_COLUMN_ALIAS|2.0|字符串： "Y" （如果数据源支持列别名）;否则为 "N"。<br/><br/>列别名是可使用 AS 子句为选择列表中的列指定的备用名称。 符合 SQL-92 项级别的驱动程序将始终返回 "Y"。|
|SQL_CONCAT_NULL_BEHAVIOR|1.0|一个 SQLUSMALLINT 值，该值指示数据源如何处理 NULL 值字符数据类型列与非 NULL 值字符数据类型列的串联：<br/>SQL_CB_NULL = Result 为 NULL 值。<br/>SQL_CB_NON_NULL = Result 与非 NULL 值列或列串联。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回 SQL_CB_NULL。|
|SQL_CONVERT_BIGINT<br/>SQL_CONVERT_BINARY<br/>SQL_CONVERT_BIT<br/>SQL_CONVERT_CHAR<br/>SQL_CONVERT_GUID<br/>SQL_CONVERT_DATE<br/>SQL_CONVERT_DECIMAL<br/>SQL_CONVERT_DOUBLE<br/>SQL_CONVERT_FLOAT<br/>SQL_CONVERT_INTEGER<br/>SQL_CONVERT_INTERVAL_YEAR_MONTH<br/>SQL_CONVERT_INTERVAL_DAY_TIME<br/>SQL_CONVERT_LONGVARBINARY<br/>SQL_CONVERT_LONGVARCHAR<br/>SQL_CONVERT_NUMERIC<br/>SQL_CONVERT_REAL<br/>SQL_CONVERT_SMALLINT<br/>SQL_CONVERT_TIME<br/>SQL_CONVERT_TIMESTAMP<br/>SQL_CONVERT_TINYINT<br/>SQL_CONVERT_VARBINARY<br/>SQL_CONVERT_VARCHAR|1.0|SQLUINTEGER 位掩码。 位掩码使用 *InfoType* 中名为的类型的数据的 **CONVERT** 标量函数指示数据源支持的转换。 如果位掩码等于零，则数据源不支持从命名类型的数据进行任何转换，包括转换为同一数据类型。<br/><br/>例如，若要确定数据源是否支持 SQL_INTEGER 数据到 SQL_BIGINT 数据类型的转换，应用程序需要使用 SQL_CONVERT_INTEGER 的 *InfoType* 调用 **SQLGetInfo** 。 应用程序使用返回的位掩码和 SQL_CVT_BIGINT 执行 **和** 操作。 如果生成的值为非零值，则支持转换。<br/><br/>以下位掩码用于确定支持哪些转换：<br/>SQL_CVT_BIGINT (ODBC 1.0) <br/>SQL_CVT_BINARY (ODBC 1.0) <br/>SQL_CVT_BIT (ODBC 1.0) <br/>SQL_CVT_GUID (ODBC 3.5) <br/>SQL_CVT_CHAR (ODBC 1.0) <br/>SQL_CVT_DATE (ODBC 1.0) <br/>SQL_CVT_DECIMAL (ODBC 1.0) <br/>SQL_CVT_DOUBLE (ODBC 1.0) <br/>SQL_CVT_FLOAT (ODBC 1.0) <br/>SQL_CVT_INTEGER (ODBC 1.0) <br/>SQL_CVT_INTERVAL_YEAR_MONTH (ODBC 3.0) <br/>SQL_CVT_INTERVAL_DAY_TIME (ODBC 3.0) <br/>SQL_CVT_LONGVARBINARY (ODBC 1.0) <br/>SQL_CVT_LONGVARCHAR (ODBC 1.0) <br/>SQL_CVT_NUMERIC (ODBC 1.0) <br/>SQL_CVT_REAL (ODBC 1.0) <br/>SQL_CVT_SMALLINT (ODBC 1.0) <br/>SQL_CVT_TIME (ODBC 1.0) <br/>SQL_CVT_TIMESTAMP (ODBC 1.0) <br/>SQL_CVT_TINYINT (ODBC 1.0) <br/>SQL_CVT_VARBINARY (ODBC 1.0) <br/>SQL_CVT_VARCHAR (ODBC 1.0) |
|SQL_CONVERT_FUNCTIONS|1.0|SQLUINTEGER 位掩码，用于枚举驱动程序和关联数据源支持的标量转换函数。<br/><br/>以下位掩码用于确定支持哪些转换函数：<br/>SQL_FN_CVT_CASTSQL_FN_CVT_CONVERT|
|SQL_CORRELATION_NAME|1.0|一个 SQLUSMALLINT 值，该值指示是否支持表相关名称：<br/>不支持 SQL_CN_NONE = 相关名称。<br/>支持 SQL_CN_DIFFERENT = 相关名称，但必须与它们所表示的表的名称不同。<br/>SQL_CN_ANY = 相关名称受支持，并且可以是任何有效的用户定义名称。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回 SQL_CN_ANY。|
|SQL_CREATE_ASSERTION|3.0|SQLUINTEGER 位掩码，根据数据源支持的 **CREATE ASSERTION** 语句（如 SQL-92 中所定义）枚举子句。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CA_CREATE_ASSERTION<br/><br/>如果支持显式指定约束属性的功能，则以下位指定支持的约束属性 (参阅 SQL_ALTER_TABLE 并 SQL_CREATE_TABLE 信息类型) ：<br/>SQL_CA_CONSTRAINT_INITIALLY_DEFERRED<br/>SQL_CA_CONSTRAINT_INITIALLY_IMMEDIATE<br/>SQL_CA_CONSTRAINT_DEFERRABLE<br/>SQL_CA_CONSTRAINT_NON_DEFERRABLE<br/><br/>符合 SQL-92 完全符合级别的驱动程序将始终返回所有这些选项的受支持。 如果返回值为 "0"，则表示不支持 **CREATE ASSERTION** 语句。|
|SQL_CREATE_CHARACTER_SET|3.0|SQLUINTEGER 位掩码，用于枚举 **CREATE 字符集** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CCS_CREATE_CHARACTER_SET<br/>SQL_CCS_COLLATE_CLAUSE<br/>SQL_CCS_LIMITED_COLLATION<br/><br/>符合 SQL-92 完全符合级别的驱动程序将始终返回所有这些选项的受支持。 如果返回值为 "0"，则表示不支持 **CREATE 字符集** 语句。|
|SQL_CREATE_COLLATION|3.0|SQLUINTEGER 位掩码，用于枚举 **CREATE 排序规则** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CCOL_CREATE_COLLATION<br/><br/>SQL-92 完全符合级别的驱动程序将始终返回此选项（受支持）。 如果返回值为 "0"，则表示不支持 **CREATE 排序规则** 语句。|
|SQL_CREATE_DOMAIN|3.0|SQLUINTEGER 位掩码，用于枚举 **CREATE DOMAIN** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CDO_CREATE_DOMAIN = CREATE DOMAIN 语句 (中级) 支持。<br/>SQL_CDO_CONSTRAINT_NAME_DEFINITION = \<constraint name definition> 支持 (中间级别) 命名域约束。<br/><br/>以下位指定了创建列约束的能力：<br/>SQL_CDO_DEFAULT = (中间级别支持指定域约束) <br/>SQL_CDO_CONSTRAINT = (中间级别支持指定域默认值) <br/>SQL_CDO_COLLATION = (完整级别支持指定域排序规则) <br/><br/>如果 (SQL_CDO_DEFAULT 设置) ，则以下位指定支持的约束属性：<br/>SQL_CDO_CONSTRAINT_INITIALLY_DEFERRED (完整级别) <br/>SQL_CDO_CONSTRAINT_INITIALLY_IMMEDIATE (完整级别) <br/>SQL_CDO_CONSTRAINT_DEFERRABLE (完整级别) <br/>SQL_CDO_CONSTRAINT_NON_DEFERRABLE (完整级别) <br/><br/>如果返回值为 "0"，则表示不支持 **CREATE DOMAIN** 语句。|
|SQL_CREATE_SCHEMA|3.0|SQLUINTEGER 位掩码，用于枚举 **CREATE SCHEMA** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CS_CREATE_SCHEMA<br/>SQL_CS_AUTHORIZATION<br/>SQL_CS_DEFAULT_CHARACTER_SET<br/><br/>与 SQL-92 中级相容的驱动程序将始终返回 SQL_CS_CREATE_SCHEMA，并 SQL_CS_AUTHORIZATION 支持的选项。 还必须在 SQL-92 项级别支持这些项，但不一定是 SQL 语句。 符合 SQL-92 完全符合级别的驱动程序将始终返回所有这些选项的受支持。|
|SQL_CREATE_TABLE|3.0|SQLUINTEGER 位掩码，根据数据源支持的 SQL-92 中所定义，枚举 **CREATE TABLE** 语句中的子句。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CT_CREATE_TABLE = CREATE TABLE 语句受支持。  (条目级别) <br/>SQL_CT_TABLE_CONSTRAINT = (FIPS 过渡级别支持指定表约束) <br/>SQL_CT_CONSTRAINT_NAME_DEFINITION = \<constraint name definition> 子句支持 (中间级别的列和表约束) <br/><br/>以下位指定创建临时表的能力：<br/>SQL_CT_COMMIT_PRESERVE = 在提交时保留删除的行。  (完全级别) <br/>SQL_CT_COMMIT_DELETE = 在提交时删除删除的行。  (完全级别) <br/>SQL_CT_GLOBAL_TEMPORARY = 可以创建全局临时表。  (完全级别) <br/>SQL_CT_LOCAL_TEMPORARY = 可以创建本地临时表。  (完全级别) <br/><br/>以下位指定了创建列约束的能力：<br/>SQL_CT_COLUMN_CONSTRAINT = (FIPS 过渡级别支持指定列约束) <br/>SQL_CT_COLUMN_DEFAULT = (FIPS 过渡级别支持指定列默认值) <br/>SQL_CT_COLUMN_COLLATION = 指定列排序规则支持 (完全级别) <br/><br/>如果支持指定列或表约束，以下位将指定支持的约束属性：<br/>SQL_CT_CONSTRAINT_INITIALLY_DEFERRED (完整级别) <br/>SQL_CT_CONSTRAINT_INITIALLY_IMMEDIATE (完整级别) <br/>SQL_CT_CONSTRAINT_DEFERRABLE (完整级别) <br/>SQL_CT_CONSTRAINT_NON_DEFERRABLE (完整级别) |
|SQL_CREATE_TRANSLATION|3.0|SQLUINTEGER 位掩码，用于枚举 **CREATE 转换** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CTR_CREATE_TRANSLATION<br/><br/>与 SQL-92 完全符合级别的驱动程序将始终返回这些选项（如果支持）。 如果返回值为 "0"，则表示不支持 **CREATE 平移** 语句。|
|SQL_CREATE_VIEW|3.0|SQLUINTEGER 位掩码，用于枚举 **CREATE VIEW** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_CV_CREATE_VIEW<br/>SQL_CV_CHECK_OPTION<br/>SQL_CV_CASCADED<br/>SQL_CV_LOCAL<br/><br/>如果返回值为 "0"，则表示不支持 **CREATE VIEW** 语句。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回 SQL_CV_CREATE_VIEW，并 SQL_CV_CHECK_OPTION 支持的选项。<br/><br/>符合 SQL-92 完全符合级别的驱动程序将始终返回所有这些选项的受支持。|
|SQL_CURSOR_COMMIT_BEHAVIOR|1.0|一个 SQLUSMALLINT 值，指示 **commit** 操作如何影响数据源中的游标和预定义语句， (在提交事务) 时数据源的行为。<br/><br/>此属性的值将反映下一设置的当前状态： SQL_COPT_SS_PRESERVE_CURSORS。<br/>SQL_CB_DELETE = 关闭游标并删除预定义语句。 若要再次使用该游标，应用程序必须 reprepare，然后重新执行该语句。<br/>SQL_CB_CLOSE = 关闭游标。 对于预定义语句，应用程序可以对语句调用 **SQLExecute** ，而无需再次调用 **SQLPrepare** 。 SQL ODBC 驱动程序的默认值为 SQL_CB_CLOSE。 这意味着，在提交事务时，SQL ODBC 驱动程序会关闭游标 () 。<br/>SQL_CB_PRESERVE = 保持游标的位置与 **提交** 操作之前相同。 应用程序可以继续获取数据，也可以关闭游标并重新执行语句，而无需 repreparing。|
|SQL_CURSOR_ROLLBACK_BEHAVIOR|1.0|一个 SQLUSMALLINT 值，指示 **回滚** 操作如何影响数据源中的游标和预定义语句：<br/>SQL_CB_DELETE = 关闭游标并删除预定义语句。 若要再次使用该游标，应用程序必须 reprepare，然后重新执行该语句。<br/>SQL_CB_CLOSE = 关闭游标。 对于预定义语句，应用程序可以对语句调用 **SQLExecute** ，而无需再次调用 **SQLPrepare** 。<br/>SQL_CB_PRESERVE = 保持游标的位置与 **回滚** 操作之前相同。 应用程序可以继续获取数据，也可以关闭游标并重新执行语句，而无需 repreparing。|
|SQL_CURSOR_SENSITIVITY|3.0|一个 SQLUINTEGER 值，指示对游标敏感性的支持：<br/>SQL_INSENSITIVE = 语句句柄上的所有游标都显示结果集，而不反映同一事务中的任何其他游标对其所做的任何更改。<br/>SQL_UNSPECIFIED = 未指定语句句柄上的游标是否会使同一事务中的另一个游标对结果集所做的更改可见。 语句句柄上的游标可能会使无、部分或所有此类更改可见。<br/>SQL_SENSITIVE = 游标对同一事务中的其他游标所做的更改是敏感的。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回受支持的 SQL_UNSPECIFIED 选项。<br/><br/>与 SQL 92 完全符合级别的驱动程序将始终返回受支持的 SQL_INSENSITIVE 选项。|
|SQL_DATA_SOURCE_NAME|1.0|一个字符串，其中包含在连接过程中使用的数据源名称。 如果应用程序调用了 **SQLConnect**，则这是 *szDSN* 参数的值。 如果应用程序调用了 **SQLDriverConnect** 或 **SQLBrowseConnect**，则这是传递给驱动程序的连接字符串中 DSN 关键字的值。 如果连接字符串不包含 **DSN** 关键字 (例如，当它包含 **DRIVER** 关键字) 时，这是一个空字符串。|
|SQL_DATA_SOURCE_READ_ONLY|1.0|字符串。 如果数据源设置为只读模式，则为 "Y"; 否则为 "N"。<br/><br/>此特征仅适用于数据源本身;它不是启用数据源访问权限的驱动程序的特性。 可读/写的驱动程序可用于只读数据源。 如果驱动程序为只读，则其所有数据源都必须为只读，并且必须返回 SQL_DATA_SOURCE_READ_ONLY。|
|SQL_DATABASE_NAME|1.0|一个字符串，其中包含当前使用的当前数据库的名称，前提是该数据源定义了名为 "database" 的命名对象。<br/><br/>在 ODBC 3.x 中，还可以通过使用 SQL_ATTR_CURRENT_CATALOG 的 *属性* 参数调用 **SQLGetConnectAttr** 来返回为此 *InfoType* 返回的值。|
|SQL_DATETIME_LITERALS|3.0|SQLUINTEGER 位掩码，用于枚举数据源支持的 SQL-92 日期时间文本。 请注意，这些是 SQL-92 规范中列出的日期时间文本，并且独立于 ODBC 定义的 datetime 文本转义子句。 有关 ODBC datetime 文本转义子句的详细信息，请参阅 [日期、时间和时间戳文本](../develop-app/date-time-and-timestamp-literals.md)。<br/><br/> 符合 FIPS 过渡级别的驱动程序将始终在以下列表中的位位掩码中返回 "1" 值。 如果值为 "0"，则表示不支持 SQL-92 datetime 文本。<br/><br/>以下位掩码用于确定受支持的文本：<br/>SQL_DL_SQL92_DATE<br/>SQL_DL_SQL92_TIME<br/>SQL_DL_SQL92_TIMESTAMP<br/>SQL_DL_SQL92_INTERVAL_YEAR<br/>SQL_DL_SQL92_INTERVAL_MONTH<br/>SQL_DL_SQL92_INTERVAL_DAY<br/>SQL_DL_SQL92_INTERVAL_HOUR<br/>SQL_DL_SQL92_INTERVAL_MINUTE<br/>SQL_DL_SQL92_INTERVAL_SECOND<br/>SQL_DL_SQL92_INTERVAL_YEAR_TO_MONTH<br/>SQL_DL_SQL92_INTERVAL_DAY_TO_HOUR<br/>SQL_DL_SQL92_INTERVAL_DAY_TO_MINUTE<br/>SQL_DL_SQL92_INTERVAL_DAY_TO_SECOND<br/>SQL_DL_SQL92_INTERVAL_HOUR_TO_MINUTE<br/>SQL_DL_SQL92_INTERVAL_HOUR_TO_SECOND<br/>SQL_DL_SQL92_INTERVAL_MINUTE_TO_SECOND|
|SQL_DBMS_NAME|1.0|一个字符串，其中包含驱动程序所访问的 DBMS 产品的名称。|
|SQL_DBMS_VER|1.0|一个字符串，指示驱动程序所访问的 DBMS 产品的版本。 版本的格式为 # #. # #. # # # #，其中前两位数字是主版本，接下来的两位数字是次版本号，最后四位数字是发布版本。 驱动程序必须以这种形式呈现 DBMS 产品版本，但也可以追加 DBMS 特定于产品的版本。 例如，"04.01.0000 Rdb 4.1"。|
|SQL_DDL_INDEX|3.0|一个 SQLUINTEGER 值，指示支持创建和删除索引：<br/>SQL_DI_CREATE_INDEX<br/>SQL_DI_DROP_INDEX|
|SQL_DEFAULT_TXN_ISOLATION|1.0|指示驱动程序或数据源支持的默认事务隔离级别的 SQLUINTEGER 值; 如果数据源不支持事务，则为零。 以下术语用于定义事务隔离级别：<br/>**脏读** 事务1更改行。 事务2在事务1提交更改之前读取已更改的行。 如果事务1回滚更改，则事务2将读取被视为永远不存在的行。<br/>**不可重复的读取** 事务1读取一行。 事务2更新或删除该行并提交此更改。 如果事务1尝试重新读取该行，则它将接收不同的行值或发现该行已被删除。<br/>**虚拟** 事务1读取一组满足某些搜索条件的行。 事务2通过与搜索条件匹配的插入或更新)  (生成一个或多个行。 如果事务1重新执行读取行的语句，则它会接收一组不同的行。<br/><br/>如果数据源支持事务，则驱动程序将返回下列位掩码之一：<br/>SQL_TXN_READ_UNCOMMITTED = 脏读、不可重复读取和幻像是可能的。<br/>SQL_TXN_READ_COMMITTED = 不可能进行脏读。 可以不重复的读取和幻像。<br/>SQL_TXN_REPEATABLE_READ = 不可能进行脏读和非重复读取。 幻像是可能的。<br/>SQL_TXN_SERIALIZABLE = 可序列化事务。 可序列化事务不允许脏读、不可重复读或幻像。|
|SQL_DESCRIBE_PARAMETER|3.0|字符串： "Y" （如果可以描述参数）;"N"，否则为。<br/><br/>SQL-92 完全符合级别的驱动程序通常将返回 "Y"，因为它将支持 **描述输入** 语句。 但这不会直接指定基础 SQL 支持，但是，即使在符合 SQL-92 的完全一致的驱动程序中，也可能不支持描述参数。|
|SQL_DM_VER|3.0|一个字符串，其中包含驱动程序管理器的版本。 版本的格式为 # #. # #. # # # #. # # # #，其中：<br/>第一组两位数字是主 ODBC 版本，由常量 SQL_SPEC_MAJOR 提供。<br/>第二组两位数字是次 ODBC 版本，由常量 SQL_SPEC_MINOR 提供。<br/>第三组四位数是驱动程序管理器的主要内部版本号。<br/>最后一组四位数是驱动程序管理器的次要版本号。<br/>Windows 7 驱动程序管理器版本为03.80。 Windows 8 驱动程序管理器版本为03.81。|
|SQL_DRIVER_AWARE_POOLING_SUPPORTED|3.8|一个 SQLUINTEGER 值，该值指示驱动程序是否支持驱动程序感知池。  (有关详细信息，请参阅 [识别驱动程序的连接池](../develop-app/driver-aware-connection-pooling.md)。<br/><br/>SQL_DRIVER_AWARE_POOLING_CAPABLE 指示驱动程序可以支持驱动程序感知池机制。<br/>SQL_DRIVER_AWARE_POOLING_NOT_CAPABLE 指示驱动程序无法支持驱动程序感知池机制。<br/><br/>驱动程序无需实现 SQL_DRIVER_AWARE_POOLING_SUPPORTED，驱动程序管理器将不会服从驱动程序的返回值。|
|SQL_DRIVER_HDBCSQL_DRIVER_HENV|1.0|SQLULEN 生成值，驱动程序的环境句柄或连接句柄，由参数 *InfoType* 决定。<br/><br/>这些信息类型由驱动程序管理器单独实现。|
|SQL_DRIVER_HDESC|3.0|SQLULEN 生成值，驱动程序管理器描述符句柄决定的驱动程序描述符句柄，必须将其传递到 \* 应用程序的 *InfoValuePtr* 中的输入。 在这种情况下， *InfoValuePtr* 既是输入参数又是输出参数。 在 InfoValuePtr 中传递的输入说明符句柄 \* 必须显式或隐式分配在 *ConnectionHandle* 上。<br/><br/>应用程序在使用此信息类型调用 **SQLGetInfo** 之前，应生成驱动程序管理器描述符句柄的副本，以确保在输出时不覆盖该句柄。<br/><br/>此信息类型由驱动程序管理器单独实现。|
|SQL_DRIVER_HLIB|2.0|一个 SQLULEN 生成值，它在 Microsoft Windows 操作系统上加载驱动程序 DLL 时返回到驱动程序管理器的加载库中的 *hinst* ，或在另一个操作系统上的等效值。 句柄仅对对 **SQLGetInfo** 调用中指定的连接句柄有效。<br/><br/>此信息类型由驱动程序管理器单独实现。|
|SQL_DRIVER_HSTMT|1.0|SQLULEN 生成值，由驱动程序管理器语句句柄决定的驱动程序语句句柄，该句柄必须传递到 \* 应用程序的 *InfoValuePtr* 中的输入。 在这种情况下， *InfoValuePtr* 既是输入参数又是输出参数。 \*必须已在参数 *ConnectionHandle* 上分配 *InfoValuePtr* 中传递的输入语句句柄。<br/><br/>应用程序在使用此信息类型调用 **SQLGetInfo** 之前，应生成驱动程序管理器的语句句柄的副本，以确保在输出时不覆盖该句柄。<br/><br/>此信息类型由驱动程序管理器单独实现。|
|SQL_DRIVER_NAME|1.0|一个字符串，其中包含用于访问数据源的驱动程序的文件名。|
|SQL_DRIVER_ODBC_VER|2.0|一个字符串，其中包含驱动程序支持的 ODBC 版本。 版本的格式为 # #. # #，其中前两位数字是主版本，接下来的两位数字是次版本。 SQL_SPEC_MAJOR 和 SQL_SPEC_MINOR 定义主版本号和次版本号。 对于本手册中所述的 ODBC 版本，这些版本为3，0，驱动程序应返回 "03.00"。<br/><br/>ODBC 驱动程序管理器不会修改 SQLGetInfo (SQL_DRIVER_ODBC_VER) 的返回值以保持现有应用程序的向后兼容性。 驱动程序指定将返回的值。 但是，当应用程序调用 **SQLSetEnvAttr** 将 SQL_ATTR_ODBC_VERSION 设置为3.8 时，支持 C 数据类型扩展性的驱动程序必须返回 3.8 (或更高) 。 有关详细信息，请参阅 [ODBC 中的 C 数据类型](../develop-app/c-data-types-in-odbc.md)。|
|SQL_DRIVER_VER|1.0|一个字符串，其中包含驱动程序的版本和（可选）驱动程序的说明。 版本的格式至少为 # #. # #. # # # #，其中前两位数字是主版本，接下来的两位数字是次版本号，最后四位数字是发布版本。|
|SQL_DROP_ASSERTION|3.0|SQLUINTEGER 位掩码，枚举 **DROP ASSERTION** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DA_DROP_ASSERTION<br/><br/>SQL-92 完全符合级别的驱动程序将始终返回此选项（受支持）。|
|SQL_DROP_CHARACTER_SET|3.0|SQLUINTEGER 位掩码，用于枚举 **DROP 字符集** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DCS_DROP_CHARACTER_SET<br/><br/>SQL-92 完全符合级别的驱动程序将始终返回此选项（受支持）。|
|SQL_DROP_COLLATION|3.0|SQLUINTEGER 位掩码，用于枚举 **删除排序规则** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DC_DROP_COLLATION<br/><br/>SQL-92 完全符合级别的驱动程序将始终返回此选项（受支持）。|
|SQL_DROP_DOMAIN|3.0|SQLUINTEGER 位掩码，用于枚举 **删除域** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DD_DROP_DOMAIN<br/>SQL_DD_CASCADE<br/>SQL_DD_RESTRICT<br/><br/>与 SQL-92 中间级别相容的驱动程序将始终返回所有支持的选项。|
|SQL_DROP_SCHEMA|3.0|SQLUINTEGER 位掩码，根据数据源支持的 SQL-92 中所定义，枚举 **DROP SCHEMA** 语句中的子句。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DS_DROP_SCHEMA<br/>SQL_DS_CASCADE<br/>SQL_DS_RESTRICT<br/><br/>与 SQL-92 中间级别相容的驱动程序将始终返回所有支持的选项。|
|SQL_DROP_TABLE|3.0|SQLUINTEGER 位掩码，用于枚举 **DROP TABLE** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DT_DROP_TABLE<br/>SQL_DT_CASCADE<br/>SQL_DT_RESTRICT<br/><br/>符合 FIPS 转换级别的驱动程序将始终返回所有支持的选项。|
|SQL_DROP_TRANSLATION|3.0|SQLUINTEGER 位掩码，用于枚举 **DROP 转换** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DTR_DROP_TRANSLATION<br/><br/>SQL-92 完全符合级别的驱动程序将始终返回此选项（受支持）。|
|SQL_DROP_VIEW|3.0|SQLUINTEGER 位掩码，用于枚举 **DROP VIEW** 语句中的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>以下位掩码用于确定支持的子句：<br/>SQL_DV_DROP_VIEW<br/>SQL_DV_CASCADE<br/>SQL_DV_RESTRICT<br/><br/>符合 FIPS 转换级别的驱动程序将始终返回所有支持的选项。|
|SQL_DYNAMIC_CURSOR_ATTRIBUTES1|3.0|SQLUINTEGER 位掩码，描述驱动程序支持的动态游标的特性。 此位掩码包含特性的第一个子集;对于第二个子集，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES2。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA1_NEXT = 当游标为动态游标时，对 **SQLFetchScroll** 的调用中支持 SQL_FETCH_NEXT 的 *FetchOrientation* 参数。<br/>如果游标为动态游标，则在对 **SQLFetchScroll** 的调用中支持 SQL_CA1_ABSOLUTE = *FetchOrientation* 参数的 SQL_FETCH_FIRST、SQL_FETCH_LAST 和 SQL_FETCH_ABSOLUTE。  (将提取的行集与当前游标位置无关 ) <br/>如果游标为动态游标 SQL_FETCH_RELATIVE，则在对 **SQLFetchScroll** 的调用中支持 SQL_CA1_RELATIVE = *FetchOrientation* SQL_FETCH_PRIOR 参数。  (要提取的行集取决于当前游标位置。 请注意，这与 SQL_FETCH_NEXT 分隔开来，因为在只进游标中，仅支持 SQL_FETCH_NEXT。 ) <br/>SQL_CA1_BOOKMARK = 当游标为动态游标时，对 **SQLFetchScroll** 的调用中支持 SQL_FETCH_BOOKMARK 的 *FetchOrientation* 参数。<br/>SQL_CA1_LOCK_EXCLUSIVE = 当游标为动态游标时，对 **SQLSetPos** 的调用中支持 SQL_LOCK_EXCLUSIVE 的 *LockType* 参数。<br/>SQL_CA1_LOCK_NO_CHANGE = 当游标为动态游标时，对 **SQLSetPos** 的调用中支持 SQL_LOCK_NO_CHANGE 的 *LockType* 参数。<br/>SQL_CA1_LOCK_UNLOCK = 当游标为动态游标时，对 **SQLSetPos** 的调用中支持 SQL_LOCK_UNLOCK 的 *LockType* 参数。<br/>SQL_CA1_POS_POSITION = 当光标为动态游标时，对 **SQLSetPos** 的调用中支持 SQL_POSITION 的 *操作* 参数。<br/>SQL_CA1_POS_UPDATE = 当光标为动态游标时，对 **SQLSetPos** 的调用中支持 SQL_UPDATE 的 *操作* 参数。<br/>SQL_CA1_POS_DELETE = 当光标为动态游标时，对 **SQLSetPos** 的调用中支持 SQL_DELETE 的 *操作* 参数。<br/>SQL_CA1_POS_REFRESH = 当光标为动态游标时，对 **SQLSetPos** 的调用中支持 SQL_REFRESH 的 *操作* 参数。<br/>SQL_CA1_POSITIONED_UPDATE = 在游标为动态游标时支持当前 SQL 语句的更新。  (符合 SQL-92 项级别的驱动程序将始终返回此选项（如果支持）。 ) <br/>SQL_CA1_POSITIONED_DELETE = A DELETE，当光标为动态游标时，支持当前的 SQL 语句。  (符合 SQL-92 项级别的驱动程序将始终返回此选项（如果支持）。 ) <br/>SQL_CA1_SELECT_FOR_UPDATE = 当光标为动态游标时，支持 SELECT FOR UPDATE SQL 语句。  (符合 SQL-92 项级别的驱动程序将始终返回此选项（如果支持）。 ) <br/>SQL_CA1_BULK_ADD = 当光标为动态游标时，对 **SQLBulkOperations** 的调用中支持 SQL_ADD 的 *操作* 参数。<br/>SQL_CA1_BULK_UPDATE_BY_BOOKMARK = 当光标为动态游标时，对 **SQLBulkOperations** 的调用中支持 SQL_UPDATE_BY_BOOKMARK 的 *操作* 参数。<br/>SQL_CA1_BULK_DELETE_BY_BOOKMARK = 当光标为动态游标时，对 **SQLBulkOperations** 的调用中支持 SQL_DELETE_BY_BOOKMARK 的 *操作* 参数。<br/>SQL_CA1_BULK_FETCH_BY_BOOKMARK = 当光标为动态游标时，对 **SQLBulkOperations** 的调用中支持 SQL_FETCH_BY_BOOKMARK 的 *操作* 参数。<br/><br/>与 SQL-92 中间级别相容的驱动程序通常会返回受支持的 SQL_CA1_NEXT、SQL_CA1_ABSOLUTE 和 SQL_CA1_RELATIVE 选项，因为它支持通过嵌入式 SQL FETCH 语句滚动的游标。 但这并不直接确定基础 SQL 支持，但可能不支持可滚动游标，即使对于符合 SQL-92 的中间级别驱动程序也是如此。|
|SQL_DYNAMIC_CURSOR_ATTRIBUTES2|3.0|SQLUINTEGER 位掩码，描述驱动程序支持的动态游标的特性。 此位掩码包含特性的第二个子集;对于第一个子集，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES1。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA2_READ_ONLY_CONCURRENCY = 不允许任何更新的只读动态游标。  (可以为动态游标) SQL_CONCUR_READ_ONLY SQL_ATTR_CONCURRENCY 语句特性。<br/>SQL_CA2_LOCK_CONCURRENCY = 使用足以确保行可更新的最小锁定级别的动态游标。  (可以为动态游标 SQL_CONCUR_LOCK SQL_ATTR_CONCURRENCY 语句特性。 ) 这些锁必须与 SQL_ATTR_TXN_ISOLATION 连接属性设置的事务隔离级别一致。<br/>SQL_CA2_OPT_ROWVER_CONCURRENCY = 支持使用乐观并发控制比较行版本的动态游标。  (可以为动态游标 SQL_CONCUR_ROWVER SQL_ATTR_CONCURRENCY 语句特性。 ) <br/>SQL_CA2_OPT_VALUES_CONCURRENCY = 支持使用开放式并发控制比较值的动态游标。  (可以为动态游标 SQL_CONCUR_VALUES SQL_ATTR_CONCURRENCY 语句特性。 ) <br/>SQL_CA2_SENSITIVITY_ADDITIONS = 添加的行对动态游标可见;光标可以滚动到这些行。 将这些行添加到游标的 (取决于驱动程序。 ) <br/>SQL_CA2_SENSITIVITY_DELETIONS = 删除的行不再可用于动态游标，并且不会在结果集中保留 "洞";动态游标从已删除的行滚动后，它无法返回到该行。<br/>SQL_CA2_SENSITIVITY_UPDATES = 对行的更新对动态游标可见;如果动态游标从滚动并返回到更新的行，则游标返回的数据是更新的数据，而不是原始数据。<br/>SQL_CA2_MAX_ROWS_SELECT = 当光标为动态游标时，SQL_ATTR_MAX_ROWS 语句特性会影响 **SELECT** 语句。<br/>SQL_CA2_MAX_ROWS_INSERT = 当光标为动态游标时，SQL_ATTR_MAX_ROWS 语句特性会影响 **INSERT** 语句。<br/>SQL_CA2_MAX_ROWS_DELETE = 当游标为动态游标时，SQL_ATTR_MAX_ROWS 语句特性会影响 **DELETE** 语句。<br/>SQL_CA2_MAX_ROWS_UPDATE = 当游标为动态游标时，SQL_ATTR_MAX_ROWS 语句特性会影响 **UPDATE** 语句。<br/>SQL_CA2_MAX_ROWS_CATALOG = 当光标为动态游标时，SQL_ATTR_MAX_ROWS 语句特性会影响 **目录** 结果集。<br/>SQL_CA2_MAX_ROWS_AFFECTS_ALL = 当光标为动态游标时，SQL_ATTR_MAX_ROWS 语句特性会影响 **SELECT**、 **INSERT**、 **DELETE** 和 **UPDATE** 语句以及 **目录** 结果集。<br/>SQL_CA2_CRC_EXACT = 当光标为动态游标时，SQL_DIAG_CURSOR_ROW_COUNT 诊断字段中提供准确的行计数。<br/>SQL_CA2_CRC_APPROXIMATE = 当光标为动态游标时，SQL_DIAG_CURSOR_ROW_COUNT 诊断字段中会提供大约的行计数。<br/>SQL_CA2_SIMULATE_NON_UNIQUE = 此驱动程序不保证当游标为动态游标时，模拟定位的 update 或 delete 语句仅影响一行;应用程序负责保证这一点。  (如果语句影响多个行，则 **SQLExecute** 或 **SQLEXECDIRECT** 将返回 SQLSTATE 01001 [Cursor operation 矛盾]。 ) 若要设置此行为，应用程序将调用 **SQLSetStmtAttr** ，并将 SQL_ATTR_SIMULATE_CURSOR 特性设置为 SQL_SC_NON_UNIQUE。<br/>SQL_CA2_SIMULATE_TRY_UNIQUE = 如果游标是动态游标，则驱动程序将尝试保证模拟定位的 update 或 delete 语句仅影响一行。 驱动程序始终执行此类语句，即使它们可能会影响多个行（如没有唯一键时）也是如此。  (如果语句影响多个行，则 **SQLExecute** 或 **SQLEXECDIRECT** 将返回 SQLSTATE 01001 [Cursor operation 矛盾]。 ) 若要设置此行为，应用程序将调用 **SQLSetStmtAttr** ，并将 SQL_ATTR_SIMULATE_CURSOR 特性设置为 SQL_SC_TRY_UNIQUE。<br/>SQL_CA2_SIMULATE_UNIQUE = 驱动程序保证模拟定位的 update 或 delete 语句在游标为动态游标时只会影响一行。 如果驱动程序无法保证给定语句的这种情况， **SQLExecDirect** 或 **SQLPREPARE** 将返回 SQLSTATE 01001 (游标操作冲突) 。 若要设置此行为，应用程序将调用 **SQLSetStmtAttr** ，并将 SQL_ATTR_SIMULATE_CURSOR 特性设置为 SQL_SC_UNIQUE。|
|SQL_EXPRESSIONS_IN_ORDERBY|1.0|如果数据源在 **ORDER BY** 列表中支持表达式，则为字符串： "Y";如果不是，则为 "N"。|
|SQL_FILE_USAGE|2.0|一个 SQLUSMALLINT 值，指示单层驱动程序如何直接处理数据源中的文件：<br/>SQL_FILE_NOT_SUPPORTED = 此驱动程序不是单层驱动程序。 例如，ORACLE 驱动程序是一种双层驱动程序。<br/>SQL_FILE_TABLE = 单层驱动程序以表的形式处理数据源中的文件。 例如，Xbase 驱动程序将每个 Xbase 文件视为一个表。<br/>SQL_FILE_CATALOG = 单层驱动程序将数据源中的文件视为一个目录。 例如，Microsoft Access 驱动程序将每个 Microsoft Access 文件视为一个完整数据库。<br/><br/>应用程序可以使用它来确定用户将如何选择数据。 例如，Xbase 用户通常会将数据视为文件中存储的数据，而 ORACLE 和 Microsoft Access 用户通常会将数据视为表中存储的数据。<br/><br/>当用户选择 Xbase 数据源时，应用程序可能会显示 "Windows **文件打开** 通用" 对话框;当用户选择 Microsoft Access 或 ORACLE 数据源时，应用程序可以显示自定义的 " **选择表** " 对话框。|
|SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1|3.0|描述驱动程序所支持的只进游标属性的 SQLUINTEGER 位掩码。 此位掩码包含特性的第一个子集;对于第二个子集，请参阅 SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA1_NEXT<br/>SQL_CA1_LOCK_EXCLUSIVE<br/>SQL_CA1_LOCK_NO_CHANGE<br/>SQL_CA1_LOCK_UNLOCK<br/>SQL_CA1_POS_POSITION<br/>SQL_CA1_POS_UPDATE<br/>SQL_CA1_POS_DELETE<br/>SQL_CA1_POS_REFRESH<br/>SQL_CA1_POSITIONED_UPDATE<br/>SQL_CA1_POSITIONED_DELETE<br/>SQL_CA1_SELECT_FOR_UPDATE<br/>SQL_CA1_BULK_ADD<br/>SQL_CA1_BULK_UPDATE_BY_BOOKMARK<br/>SQL_CA1_BULK_DELETE_BY_BOOKMARK<br/>SQL_CA1_BULK_FETCH_BY_BOOKMARK<br/><br/>有关这些位掩码的说明，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES1 (并在 "说明") 中将 "动态游标" 替换为 "只进游标"。|
|SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2|3.0|描述驱动程序所支持的只进游标属性的 SQLUINTEGER 位掩码。 此位掩码包含特性的第二个子集;对于第一个子集，请参阅 SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES1。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA2_READ_ONLY_CONCURRENCY<br/>SQL_CA2_LOCK_CONCURRENCY<br/>SQL_CA2_OPT_ROWVER_CONCURRENCY<br/>SQL_CA2_OPT_VALUES_CONCURRENCY<br/>SQL_CA2_SENSITIVITY_ADDITIONS<br/>SQL_CA2_SENSITIVITY_DELETIONS<br/>SQL_CA2_SENSITIVITY_UPDATES<br/>SQL_CA2_MAX_ROWS_SELECT<br/>SQL_CA2_MAX_ROWS_INSERT<br/>SQL_CA2_MAX_ROWS_DELETE<br/>SQL_CA2_MAX_ROWS_UPDATE<br/>SQL_CA2_MAX_ROWS_CATALOG<br/>SQL_CA2_MAX_ROWS_AFFECTS_ALL<br/>SQL_CA2_CRC_EXACT<br/>SQL_CA2_CRC_APPROXIMATE<br/>SQL_CA2_SIMULATE_NON_UNIQUE<br/>SQL_CA2_SIMULATE_TRY_UNIQUE<br/>SQL_CA2_SIMULATE_UNIQUE<br/><br/>有关这些位掩码的说明，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES2 (并在 "说明") 中将 "动态游标" 替换为 "只进游标"。|
|SQL_GETDATA_EXTENSIONS|2.0|枚举 **SQLGetData** 的扩展的 SQLUINTEGER 位掩码。<br/><br/>以下位掩码与标志一起使用，以确定驱动程序为 **SQLGetData** 支持的常见扩展：<br/>对于任何未绑定的列（包括最后绑定列之前的列），都可以调用 SQL_GD_ANY_COLUMN = **SQLGetData** 。 请注意，必须按升序列数的顺序调用列，除非还返回 SQL_GD_ANY_ORDER。<br/>可以按任意顺序为未绑定列调用 SQL_GD_ANY_ORDER = **SQLGetData** 。 请注意，只能为最后一个绑定列之后的列调用 **SQLGetData** ，除非还返回 SQL_GD_ANY_COLUMN。<br/>SQL_GD_BLOCK = **SQLGetData** 可为块 (中任何行中的未绑定列调用，其中行集大小在使用 **SQLSetPos** 定位到该行后大于 1) 数据。<br/>除了未绑定的列之外，还可以为绑定列调用 SQL_GD_BOUND = **SQLGetData** 。 驱动程序无法返回该值，除非它还返回 SQL_GD_ANY_COLUMN。<br/>SQL_GD_OUTPUT_PARAMS = **SQLGetData** 可调用以返回输出参数值。 有关详细信息，请参阅 [使用 SQLGetData 检索输出参数](../develop-app/retrieving-output-parameters-using-sqlgetdata.md)。<br/><br/>**SQLGetData** 是只需从在上一次绑定列后发生的未绑定列返回数据的情况下，将按列号递增的顺序调用，而不是在行块的行中。<br/><br/>如果驱动程序支持书签 (固定长度或可变长度) ，则它必须支持对列0调用 **SQLGetData** 。 无论驱动程序为使用 SQL_GETDATA_EXTENSIONS *InfoType* 调用 **SQLGetInfo** 的情况如何返回，都需要此支持。|
|SQL_GROUP_BY|2.0|一个 SQLUSMALLINT 值，该值指定 **GROUP BY** 子句中的列与选择列表中的非聚合列之间的关系：<br/>SQL_GB_COLLATE = 可以在每个分组列的末尾指定 **COLLATE** 子句。  (ODBC 3.0) <br/>不支持 SQL_GB_NOT_SUPPORTED = **GROUP BY** 子句。  (ODBC 2.0) <br/>SQL_GB_GROUP_BY_EQUALS_SELECT = **GROUP by** 子句必须包含选择列表中的所有非聚合列。 它不能包含任何其他列。 例如， **选择 "部门"、"部门"、"最大 (薪金") 按部门分组**。  (ODBC 2.0) <br/>SQL_GB_GROUP_BY_CONTAINS_SELECT = **GROUP by** 子句必须包含选择列表中的所有非聚合列。 它可以包含未在选择列表中的列。 例如， **选择 "部门"、"部门"、"最大 (薪金"、"部门"、"年龄")**。  (ODBC 2.0) <br/>SQL_GB_NO_RELATION = **GROUP by** 子句中的列，并且选择列表不相关。 选择列表中 nongrouped、非聚合列的含义依赖于数据源。 例如， **选择 "部门"、"部门"、"雇员组"、"年龄**"。  (ODBC 2.0) <br/><br/>符合 SQL-92 项级别的驱动程序将始终返回受支持的 SQL_GB_GROUP_BY_EQUALS_SELECT 选项。 与 SQL 92 完全符合级别的驱动程序将始终返回受支持的 SQL_GB_COLLATE 选项。 如果不支持任何选项，则数据源不支持 **GROUP by** 子句。|
|SQL_IDENTIFIER_CASE|1.0|SQLUSMALLINT 值如下所示：<br/>SQL_IC_UPPER = SQL 中的标识符不区分大小写，并且以大写形式存储在系统目录中。<br/>SQL_IC_LOWER = SQL 中的标识符不区分大小写，并以小写形式存储在系统目录中。<br/>SQL_IC_SENSITIVE = SQL 中的标识符区分大小写，并以混合大小写的形式存储在系统目录中。<br/>SQL_IC_MIXED = SQL 中的标识符不区分大小写，并以混合大小写的形式存储在系统目录中。<br/><br/>由于 SQL-92 中的标识符从不区分大小写，因此严格符合 SQL-92 (任何) 级别的驱动程序将不会返回支持的 SQL_IC_SENSITIVE 选项。|
|SQL_IDENTIFIER_QUOTE_CHAR|1.0|在 SQL 语句中用作带引号 (分隔) 标识符的起始和结束分隔符的字符串。 作为参数传递给 ODBC 函数的 (标识符不必用引号引起来。 ) 如果数据源不支持带引号的标识符，则返回空白。<br/><br/>当连接属性 SQL_ATTR_METADATA_ID 设置为 SQL_TRUE 时，还可以使用此字符串对目录函数参数进行引用。<br/><br/>由于 SQL-92 中的标识符引号字符是双引号 ( ") ，因此严格符合 SQL-92 的驱动程序将始终返回双引号字符。|
|SQL_INDEX_KEYWORDS|3.0|SQLUINTEGER 位掩码，用于枚举 CREATE INDEX 语句中受驱动程序支持的关键字：<br/>SQL_IK_NONE = 不支持任何关键字。<br/>支持 SQL_IK_ASC = ASC 关键字。<br/>支持 SQL_IK_DESC = DESC 关键字。<br/>SQL_IK_ALL = 支持所有关键字。<br/><br/>若要查看 CREATE INDEX 语句是否受支持，应用程序需要使用 SQL_DLL_INDEX 信息类型调用 **SQLGetInfo** 。|
|SQL_INFO_SCHEMA_VIEWS|3.0|SQLUINTEGER 位掩码，用于枚举驱动程序支持的 INFORMATION_SCHEMA 中的视图。 中的视图和的内容 INFORMATION_SCHEMA 是在 SQL-92 中定义的。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定受支持的视图：<br/>SQL_ISV_ASSERTIONS = 标识给定用户拥有的目录断言。  (完全级别) <br/>SQL_ISV_CHARACTER_SETS = 标识给定用户可以访问的目录字符集。  (中级) <br/>SQL_ISV_CHECK_CONSTRAINTS = 标识给定用户拥有的 CHECK 约束。  (中级) <br/>SQL_ISV_COLLATIONS = 标识给定用户可以访问的目录的字符排序规则。  (完全级别) <br/>SQL_ISV_COLUMN_DOMAIN_USAGE = 标识依赖于在目录中定义的域并由给定用户拥有的目录的列。  (中级) <br/>SQL_ISV_COLUMN_PRIVILEGES = 标识对给定用户可用或授予的持久表的列的特权。  (FIPS 过渡级别) <br/>SQL_ISV_COLUMNS = 标识给定用户可以访问的持久表的列。  (FIPS 过渡级别) <br/>SQL_ISV_CONSTRAINT_COLUMN_USAGE = 类似于 CONSTRAINT_TABLE_USAGE 视图，则针对给定用户拥有的各种约束标识列。  (中级) <br/>SQL_ISV_CONSTRAINT_TABLE_USAGE = 标识由约束 (使用的表) 引用、唯一和断言，并由给定用户拥有。  (中级) <br/>SQL_ISV_DOMAIN_CONSTRAINTS = 用于标识) 目录中的域约束 (，可由给定用户访问。  (中级) <br/>SQL_ISV_DOMAINS = 标识在目录中定义的、可供用户访问的域。  (中级) <br/>SQL_ISV_KEY_COLUMN_USAGE = 标识在目录中定义的、由给定用户约束为键的列。  (中级) <br/>SQL_ISV_REFERENTIAL_CONSTRAINTS = 标识给定用户拥有的引用约束。  (中级) <br/>SQL_ISV_SCHEMATA = 标识给定用户拥有的架构。  (中级) <br/>SQL_ISV_SQL_LANGUAGES = 标识 SQL 实现所支持的 SQL 一致性级别、选项和方言。  (中级) <br/>SQL_ISV_TABLE_CONSTRAINTS = 标识给定用户拥有的表约束。  (中级) <br/>SQL_ISV_TABLE_PRIVILEGES = 标识对给定用户可用或授予的持久表的特权。  (FIPS 过渡级别) <br/>SQL_ISV_TABLES = 标识在目录中定义的、给定用户可以访问的持久表。  (FIPS 过渡级别) <br/>SQL_ISV_TRANSLATIONS = 标识给定用户可以访问的目录的字符转换。  (完全级别) <br/>SQL_ISV_USAGE_PRIVILEGES = 标识给定用户可以或拥有的目录对象的使用权限。  (FIPS 过渡级别) <br/>SQL_ISV_VIEW_COLUMN_USAGE = 标识给定用户拥有的目录视图所依赖的列。  (中级) <br/>SQL_ISV_VIEW_TABLE_USAGE = 标识给定用户拥有的目录视图所依赖的表。  (中级) <br/>SQL_ISV_VIEWS = 标识在此目录中定义的、给定用户可以访问的已查看表。  (FIPS 过渡级别) |
|SQL_INSERT_STATEMENT|3.0|指示支持 **INSERT** 语句的 SQLUINTEGER 位掩码：<br/>SQL_IS_INSERT_LITERALS<br/>SQL_IS_INSERT_SEARCHED<br/>SQL_IS_SELECT_INTO<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回受支持的所有选项。|
|SQL_INTEGRITY|1.0|字符串： "Y" （如果数据源支持完整性增强功能）;如果不是，则为 "N"。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_ODBC_SQL_OPT_IEF 重命名为 odbc 3.0。|
|SQL_KEYSET_CURSOR_ATTRIBUTES1|3.0|SQLUINTEGER 位掩码，描述驱动程序支持的键集游标的特性。 此位掩码包含特性的第一个子集;对于第二个子集，请参阅 SQL_KEYSET_CURSOR_ATTRIBUTES2。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA1_NEXT<br/>SQL_CA1_ABSOLUTE<br/>SQL_CA1_RELATIVE<br/>SQL_CA1_BOOKMARK<br/>SQL_CA1_LOCK_EXCLUSIVE<br/>SQL_CA1_LOCK_NO_CHANGE<br/>SQL_CA1_LOCK_UNLOCK<br/>SQL_CA1_POS_POSITION<br/>SQL_CA1_POS_UPDATE<br/>SQL_CA1_POS_DELETE<br/>SQL_CA1_POS_REFRESH<br/>SQL_CA1_POSITIONED_UPDATE<br/>SQL_CA1_POSITIONED_DELETE<br/>SQL_CA1_SELECT_FOR_UPDATE<br/>SQL_CA1_BULK_ADD<br/>SQL_CA1_BULK_UPDATE_BY_BOOKMARK<br/>SQL_CA1_BULK_DELETE_BY_BOOKMARK<br/>SQL_CA1_BULK_FETCH_BY_BOOKMARK<br/><br/>有关这些位掩码的说明，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES1 (，并在说明) 中将 "动态游标" 替换为 "键集驱动游标"。<br/><br/>与 SQL-92 中间级别相容的驱动程序通常会返回受支持的 SQL_CA1_NEXT、SQL_CA1_ABSOLUTE 和 SQL_CA1_RELATIVE 选项，因为该驱动程序支持通过嵌入式 SQL FETCH 语句滚动的游标。 但这并不直接确定基础 SQL 支持，但可能不支持可滚动游标，即使对于符合 SQL-92 的中间级别驱动程序也是如此。|
|SQL_KEYSET_CURSOR_ATTRIBUTES2|3.0|SQLUINTEGER 位掩码，描述驱动程序支持的键集游标的特性。 此位掩码包含特性的第二个子集;对于第一个子集，请参阅 SQL_KEYSET_CURSOR_ATTRIBUTES1。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA2_READ_ONLY_CONCURRENCY<br/>SQL_CA2_LOCK_CONCURRENCY<br/>SQL_CA2_OPT_ROWVER_CONCURRENCY<br/>SQL_CA2_OPT_VALUES_CONCURRENCY<br/>SQL_CA2_SENSITIVITY_ADDITIONS<br/>SQL_CA2_SENSITIVITY_DELETIONS<br/>SQL_CA2_SENSITIVITY_UPDATES<br/>SQL_CA2_MAX_ROWS_SELECT<br/>SQL_CA2_MAX_ROWS_INSERT<br/>SQL_CA2_MAX_ROWS_DELETE<br/>SQL_CA2_MAX_ROWS_UPDATE<br/>SQL_CA2_MAX_ROWS_CATALOG<br/>SQL_CA2_MAX_ROWS_AFFECTS_ALL<br/>SQL_CA2_CRC_EXACT<br/>SQL_CA2_CRC_APPROXIMATE<br/>SQL_CA2_SIMULATE_NON_UNIQUE<br/>SQL_CA2_SIMULATE_TRY_UNIQUE<br/>SQL_CA2_SIMULATE_UNIQUE<br/><br/>有关这些位掩码的说明，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES1 (，并在说明) 中将 "动态游标" 替换为 "键集驱动游标"。|
|SQL_KEYWORDS|2.0|一个字符串，其中包含以逗号分隔的所有数据源特定关键字的列表。 此列表不包含特定于 ODBC 或数据源和 ODBC 所使用的关键字的关键字。 此列表表示所有保留关键字;可互操作的应用程序在对象名称中不应使用这些词。<br/><br/>有关 ODBC 关键字的列表，请参阅[附录 C： SQL 语法](../appendixes/appendix-c-sql-grammar.md)中的[保留关键字](../appendixes/reserved-keywords.md)。 **#Define** 值 SQL_ODBC_KEYWORDS 包含以逗号分隔的 ODBC 关键字列表。|
|SQL_LIKE_ESCAPE_CLAUSE|2.0|字符串： "Y" （如果数据源支持 **类似** 谓词中的百分号字符的转义符 (% ) 和下划线 (_) ，驱动程序支持 ODBC 语法来定义 **like** 谓词转义符;否则为 "N"。|
|SQL_MAX_ASYNC_CONCURRENT_STATEMENTS|3.0|一个 SQLUINTEGER 值，该值指定在给定连接上驱动程序可以支持的异步模式下活动并发语句的最大数目。 如果没有特定限制或限制未知，则此值为零。|
|SQL_MAX_BINARY_LITERAL_LEN|2.0|一个 SQLUINTEGER 值，指定最大长度 (十六进制字符数，不包括 SQL 语句中二进制文本的 **SQLGetTypeInfo**) 所返回的文本前缀和后缀。 例如，二进制文本0xFFAA 的长度为4。 如果没有最大长度或长度未知，此值将设置为零。|
|SQL_MAX_CATALOG_NAME_LEN|1.0|一个 SQLUSMALLINT 值，该值指定数据源中目录名称的最大长度。 如果没有最大长度或长度未知，此值将设置为零。<br/><br/>符合 FIPS 完全符合级别的驱动程序将返回至少128。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_MAX_QUALIFIER_NAME_LEN 重命名为 odbc 3.0。|
|SQL_MAX_CHAR_LITERAL_LEN|2.0|一个 SQLUINTEGER 值，指定 (的最大字符数，不包括 SQL 语句中字符文本的 **SQLGetTypeInfo**) 返回的文本前缀和后缀。 如果没有最大长度或长度未知，此值将设置为零。|
|SQL_MAX_COLUMN_NAME_LEN|1.0|一个 SQLUSMALLINT 值，该值指定数据源中列名的最大长度。 如果没有最大长度或长度未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少18个。 符合 FIPS 中间级别的驱动程序将返回至少128。|
|SQL_MAX_COLUMNS_IN_GROUP_BY|2.0|指定 **GROUP by** 子句中允许的最大列数的 SQLUSMALLINT 值。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少6个。 符合 FIPS 中间级别的驱动程序将返回至少15个。|
|SQL_MAX_COLUMNS_IN_INDEX|2.0|一个 SQLUSMALLINT 值，指定索引中允许的最大列数。 如果没有指定限制或限制未知，此值将设置为零。|
|SQL_MAX_COLUMNS_IN_ORDER_BY|2.0|一个 SQLUSMALLINT 值，该值指定 **ORDER by** 子句中允许的最大列数。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少6个。 符合 FIPS 中间级别的驱动程序将返回至少15个。|
|SQL_MAX_COLUMNS_IN_SELECT|2.0|一个 SQLUSMALLINT 值，该值指定选择列表中允许的最大列数。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少100。 符合 FIPS 中间级别的驱动程序将返回至少250。|
|SQL_MAX_COLUMNS_IN_TABLE|2.0|一个 SQLUSMALLINT 值，指定表中允许的最大列数。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少100。 符合 FIPS 中间级别的驱动程序将返回至少250。|
|SQL_MAX_CONCURRENT_ACTIVITIES|1.0|一个 SQLUSMALLINT 值，指定驱动程序可支持的连接的最大活动语句数。 如果语句具有挂起的结果，则该语句将定义为 "活动"，术语 "结果" 是指 **选择** 操作中的行或受 **插入**、 **更新** 或 **删除** 操作影响的行 (例如行计数) 或处于 NEED_DATA 状态。 此值可以反映驱动程序或数据源施加的限制。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_ACTIVE_STATEMENTS 重命名为 odbc 3.0。|
|SQL_MAX_CURSOR_NAME_LEN|1.0|一个 SQLUSMALLINT 值，该值指定数据源中游标名称的最大长度。 如果没有最大长度或长度未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少18个。 符合 FIPS 中间级别的驱动程序将返回至少128。|
|SQL_MAX_DRIVER_CONNECTIONS|1.0|一个 SQLUSMALLINT 值，指定驱动程序可为环境支持的最大活动连接数。 此值可以反映驱动程序或数据源施加的限制。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_ACTIVE_CONNECTIONS 重命名为 odbc 3.0。|
|SQL_MAX_IDENTIFIER_LEN|3.0|SQLUSMALLINT，它指示数据源为用户定义的名称提供的最大字符大小。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少18个。 符合 FIPS 中间级别的驱动程序将返回至少128。|
|SQL_MAX_INDEX_SIZE|2.0|一个 SQLUINTEGER 值，指定索引的组合字段中允许的最大字节数。 如果没有指定限制或限制未知，此值将设置为零。|
|SQL_MAX_PROCEDURE_NAME_LEN|1.0|一个 SQLUSMALLINT 值，该值指定数据源中过程名称的最大长度。 如果没有最大长度或长度未知，此值将设置为零。|
|SQL_MAX_ROW_SIZE|2.0|一个 SQLUINTEGER 值，指定表中单个行的最大长度。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少2000。 符合 FIPS 中间级别的驱动程序将返回至少8000。|
|SQL_MAX_ROW_SIZE_INCLUDES_LONG|3.0|字符串： "Y"，如果为 SQL_MAX_ROW_SIZE 信息类型返回的最大行大小包含行中所有 SQL_LONGVARCHAR 和 SQL_LONGVARBINARY 列的长度，则为; 否则为。否则为 "N"。|
|SQL_MAX_SCHEMA_NAME_LEN|1.0|一个 SQLUSMALLINT 值，该值指定数据源中架构名称的最大长度。 如果没有最大长度或长度未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少18个。 符合 FIPS 中间级别的驱动程序将返回至少128。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_MAX_OWNER_NAME_LEN 重命名为 odbc 3.0。|
|SQL_MAX_STATEMENT_LEN|2.0|一个 SQLUINTEGER 值，该值指定最大长度 (字符数，包括 SQL 语句的空白) 。 如果没有最大长度或长度未知，此值将设置为零。|
|SQL_MAX_TABLE_NAME_LEN|1.0|一个 SQLUSMALLINT 值，该值指定数据源中的表名的最大长度。 如果没有最大长度或长度未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少18个。 符合 FIPS 中间级别的驱动程序将返回至少128。|
|SQL_MAX_TABLES_IN_SELECT|2.0|一个 SQLUSMALLINT 值，指定 **SELECT** 语句的 **FROM** 子句中允许的最大表数。 如果没有指定限制或限制未知，此值将设置为零。<br/><br/>符合 FIPS 条目级别的驱动程序将返回至少15个。 符合 FIPS 中间级别的驱动程序将返回至少50。|
|SQL_MAX_USER_NAME_LEN|2.0|一个 SQLUSMALLINT 值，该值指定数据源中用户名称的最大长度。 如果没有最大长度或长度未知，此值将设置为零。|
|SQL_MULT_RESULT_SETS|1.0|字符串： "Y" （如果数据源支持多个结果集，则为 "N"）。<br/><br/>有关多个结果集的详细信息，请参阅 [多个结果](../develop-app/multiple-results.md)。|
|SQL_MULTIPLE_ACTIVE_TXN|1.0|字符串： "Y" 如果驱动程序同时支持多个活动事务，则在任何时候都只能有一个事务处于活动状态，则为 "N"。<br/><br/>对于分布式事务，返回的此信息类型的信息不适用。|
|SQL_NEED_LONG_DATA_LEN|2.0|字符串： "Y" 如果数据源需要长数据值的长度 (则在将该值发送到数据源之前，数据类型 SQL_LONGVARCHAR、SQL_LONGVARBINARY 或 long 特定于数据源的数据类型) ，否则为 "N"。 有关详细信息，请参阅 [SQLBindParameter 函数](sqlbindparameter-function.md) 和 [SQLSetPos 函数](sqlsetpos-function.md)。|
|SQL_NON_NULLABLE_COLUMNS|1.0|一个 SQLUSMALLINT 值，该值指定数据源是否支持列中的 NOT NULL：<br/>SQL_NNC_NULL = 所有列都必须可以为 null。<br/>SQL_NNC_NON_NULL = 列不能为 null。  (数据源在 **CREATE TABLE** 语句中支持 **NOT NULL** 列约束。 ) <br/><br/>符合 SQL-92 项级别的驱动程序将返回 SQL_NNC_NON_NULL。|
|SQL_NULL_COLLATION|2.0|一个 SQLUSMALLINT 值，该值指定在结果集中对 Null 进行排序的位置：<br/>SQL_NC_END = Null 在结果集的末尾进行排序，而不考虑 ASC 或 DESC 关键字。<br/>SQL_NC_HIGH = Null 在结果集的高端排序，具体取决于 ASC 或 DESC 关键字。<br/>SQL_NC_LOW = Null 在结果集的低端排序，具体取决于 ASC 或 DESC 关键字。<br/>SQL_NC_START = Null 在结果集的开头进行排序，而不考虑 ASC 或 DESC 关键字。|
|SQL_NUMERIC_FUNCTIONS|1.0|注意：信息类型是在 ODBC 1.0 中引入的;每个位掩码都带有引入它的版本标记。<br/><br/>SQLUINTEGER 位掩码，用于枚举驱动程序和关联数据源支持的标量数值函数。<br/><br/>以下位掩码用于确定支持哪些数值函数：<br/>SQL_FN_NUM_ABS (ODBC 1.0) <br/>SQL_FN_NUM_ACOS (ODBC 1.0) <br/>SQL_FN_NUM_ASIN (ODBC 1.0) <br/>SQL_FN_NUM_ATAN (ODBC 1.0) <br/>SQL_FN_NUM_ATAN2 (ODBC 1.0) <br/>SQL_FN_NUM_CEILING (ODBC 1.0) <br/>SQL_FN_NUM_COS (ODBC 1.0) <br/>SQL_FN_NUM_COT (ODBC 1.0) <br/>SQL_FN_NUM_DEGREES (ODBC 2.0) <br/>SQL_FN_NUM_EXP (ODBC 1.0) <br/>SQL_FN_NUM_FLOOR (ODBC 1.0) <br/>SQL_FN_NUM_LOG (ODBC 1.0) <br/>SQL_FN_NUM_LOG10 (ODBC 2.0) <br/>SQL_FN_NUM_MOD (ODBC 1.0) <br/>SQL_FN_NUM_PI (ODBC 1.0) <br/>SQL_FN_NUM_POWER (ODBC 2.0) <br/>SQL_FN_NUM_RADIANS (ODBC 2.0) <br/>SQL_FN_NUM_RAND (ODBC 1.0) <br/>SQL_FN_NUM_ROUND (ODBC 2.0) <br/>SQL_FN_NUM_SIGN (ODBC 1.0) <br/>SQL_FN_NUM_SIN (ODBC 1.0) <br/>SQL_FN_NUM_SQRT (ODBC 1.0) <br/>SQL_FN_NUM_TAN (ODBC 1.0) <br/>SQL_FN_NUM_TRUNCATE (ODBC 2.0) |
|SQL_ODBC_INTERFACE_CONFORMANCE|3.0|一个 SQLUINTEGER 值，该值指示驱动程序符合的 ODBC *1.x 接口级别。*<br/><br/>SQL_OIC_CORE：应满足所有 ODBC 驱动程序的最低级别。 此级别包括基本接口元素，如连接函数、用于准备和执行 SQL 语句的函数、基本的结果集元数据函数、基本目录函数等。<br/>SQL_OIC_LEVEL1：一个级别，包括核心标准符合性级别功能，还包括可滚动的游标、书签、定位更新和删除等。<br/>SQL_OIC_LEVEL2：一个级别，包括1级标准符合性级别功能，以及敏感游标等高级功能;按书签更新、删除和刷新;存储过程支持;主键和外键的目录函数;多目录支持;依此类推。<br/><br/>有关详细信息，请参阅 [接口一致性级别](../develop-app/interface-conformance-levels.md)。|
|SQL_ODBC_VER|1.0|一个字符串，其中包含驱动程序管理器所符合的 ODBC 版本。 版本的格式为 # #. # #. 0000，其中前两位数字是主版本，接下来的两位数字是次版本。 这只在驱动程序管理器中实现。|
|SQL_OJ_CAPABILITIES|2.01|SQLUINTEGER 位掩码，用于枚举驱动程序和数据源支持的外部联接的类型。 以下位掩码用于确定受支持的类型：<br/>SQL_OJ_LEFT = 支持左外部联接。<br/>SQL_OJ_RIGHT = 支持右外部联接。<br/>SQL_OJ_FULL = 支持完全外部联接。<br/>支持 SQL_OJ_NESTED = 嵌套的外部联接。<br/>SQL_OJ_NOT_ORDERED = 外部联接的 ON 子句中的列名在 **外部** 联接子句中的顺序不必与其各自的表名相同。<br/>SQL_OJ_INNER = 内部表 (左外部联接中的右表或右外部联接中的左表) 也可用于内部联接。 这不适用于没有内部表的完全外部联接。<br/>SQL_OJ_ALL_COMPARISON_OPS = ON 子句中的比较运算符可以是任何 ODBC 比较运算符。 如果未设置此位，则在外部联接中只能使用 equals (=) 比较运算符。<br/><br/>如果这些选项均未按支持返回，则不支持外部联接子句。<br/><br/>有关 SQL-92 定义的 SELECT 语句中关系联接运算符支持的信息，请参阅 SQL_SQL92_RELATIONAL_JOIN_OPERATORS。|
|SQL_ORDER_BY_COLUMNS_IN_SELECT|2.0|字符串： "Y"，如果 **ORDER by** 子句中的列必须在选择列表中，则为; 否则为。否则为 "N"。|
|SQL_PARAM_ARRAY_ROW_COUNTS|3.0|SQLUINTEGER 枚举驱动程序的属性，该属性与参数化执行中的行计数的可用性有关。 具有以下值：<br/>SQL_PARC_BATCH = 单个行计数适用于每个参数集。 这在概念上等效于生成一批 SQL 语句的驱动程序，每个参数集对应于数组中的一个。 可以使用 SQL_PARAM_STATUS_PTR 描述符字段检索扩展的错误信息。<br/>SQL_PARC_NO_BATCH = 只有一个行计数可用，这是从为整个参数数组执行语句时生成的累计行计数。 这在概念上等效于将语句和完整参数数组视为一个原子单元。 处理错误的方式与执行一个语句的方式相同。|
|SQL_PARAM_ARRAY_SELECTS|3.0|SQLUINTEGER 枚举驱动程序的属性，该属性与参数化执行中的结果集的可用性有关。 具有以下值：<br/>SQL_PAS_BATCH = 每组参数均提供一个结果集。 这在概念上等效于生成一批 SQL 语句的驱动程序，每个参数集对应于数组中的一个。<br/>SQL_PAS_NO_BATCH = 只有一个结果集可用，该结果集表示为完整的参数数组执行语句时产生的累积结果集。 这在概念上等效于将语句和完整参数数组视为一个原子单元。<br/>SQL_PAS_NO_SELECT = 驱动程序不允许使用参数数组来执行结果集生成语句。|
|SQL_POS_OPERATIONS|2.0|枚举 **SQLSetPos** 中的支持操作的 SQLINTEGER 位掩码。<br/><br/>以下位掩码与标志一起使用，以确定支持的选项。<br/>SQL_POS_POSITION (ODBC 2.0) <br/>SQL_POS_REFRESH (ODBC 2.0) <br/>SQL_POS_UPDATE (ODBC 2.0) <br/>SQL_POS_DELETE (ODBC 2.0) <br/>SQL_POS_ADD (ODBC 2.0) |
|SQL_PROCEDURE_TERM|1.0|一个字符串，其中包含一个过程的数据源供应商名称;例如，"数据库过程"、"存储过程"、"过程"、"包" 或 "存储查询"。|
|SQL_PROCEDURES|1.0|字符串：如果数据源支持过程并且驱动程序支持 ODBC 过程调用语法，则为 "Y";否则为 "N"。|
|SQL_QUOTED_IDENTIFIER_CASE|2.0|SQLUSMALLINT 值如下所示：<br/>SQL_IC_UPPER = SQL 中带引号的标识符不区分大小写，并且以大写形式存储在系统目录中。<br/>SQL_IC_LOWER = SQL 中带引号的标识符不区分大小写，并以小写形式存储在系统目录中。<br/>SQL_IC_SENSITIVE = SQL 中带引号的标识符区分大小写，并以混合大小写形式存储在系统目录中。  (在兼容 SQL 92 的数据库中，带引号的标识符始终区分大小写。 ) <br/>SQL_IC_MIXED = SQL 中带引号的标识符不区分大小写，并以混合大小写形式存储在系统目录中。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回 SQL_IC_SENSITIVE。|
|SQL_ROW_UPDATES|1.0|字符串： "Y" （如果由键集驱动或混合的游标维护所有已提取行的行版本或值），因此可以检测自上次提取行后任何用户对行所做的任何更新。  (这仅适用于更新，而不是删除或插入。 ) 驱动程序在调用 **SQLFetchScroll** 时，可以将 SQL_ROW_UPDATED 标志返回到行状态数组。 否则为 "N"。|
|SQL_SCHEMA_TERM|1.0|包含架构的数据源供应商名称的字符串;例如，"owner"、"Authorization ID" 或 "Schema"。<br/><br/>字符串可在大写、小写或混合大小写的情况下返回。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回 "架构"。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_OWNER_TERM 重命名为 odbc 3.0。|
|SQL_SCHEMA_USAGE|2.0|SQLUINTEGER 位掩码，用于枚举可使用架构的语句：<br/>SQL_SU_DML_STATEMENTS = 在所有数据操作语言语句中支持架构： **选择**、 **插入**、 **更新**、 **删除**，如果受支持，则 **选择** 更新和定位更新和删除语句。<br/>SQL_SU_PROCEDURE_INVOCATION = 在 ODBC 过程调用语句中支持架构。<br/>SQL_SU_TABLE_DEFINITION = 在所有表定义语句中支持架构： **CREATE TABLE**、 **CREATE VIEW**、 **ALTER TABLE**、 **drop table** 和 **drop VIEW**。<br/>SQL_SU_INDEX_DEFINITION = 在所有索引定义语句中支持架构： **创建索引** 和 **DROP INDEX**。<br/>SQL_SU_PRIVILEGE_DEFINITION = 在所有权限定义语句中支持架构： **GRANT** 和 **REVOKE**。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回受支持的 SQL_SU_DML_STATEMENTS、SQL_SU_TABLE_DEFINITION 和 SQL_SU_PRIVILEGE_DEFINITION 选项。<br/><br/>此 *InfoType* 已从 Odbc 2.0 *InfoType* SQL_OWNER_USAGE 重命名为 odbc 3.0。|
|SQL_SCROLL_OPTIONS|1.0|注意：信息类型是在 ODBC 1.0 中引入的;每个位掩码都带有引入它的版本标记。<br/><br/>SQLUINTEGER 位掩码，用于枚举可滚动游标支持的滚动选项。<br/><br/>以下位掩码用于确定受支持的选项：<br/>SQL_SO_FORWARD_ONLY = 游标只向前滚动。  (ODBC 1.0) <br/>SQL_SO_STATIC = 结果集中的数据是静态的。  (ODBC 2.0) <br/>SQL_SO_KEYSET_DRIVEN = 驱动程序将保存并使用结果集中每一行的键。  (ODBC 1.0) <br/>SQL_SO_DYNAMIC = 驱动程序保留行集中每一行的键 (键集大小与) 的行集大小相同。  (ODBC 1.0) <br/>SQL_SO_MIXED = 驱动程序保留键集中每一行的键，并且键集大小大于行集大小。 游标在键集内是键集驱动的，在键集外是动态的。  (ODBC 1.0) <br/><br/>有关可滚动游标的详细信息，请参阅可 [滚动游标](../develop-app/scrollable-cursors.md)。|
|SQL_SEARCH_PATTERN_ESCAPE|1.0|指定驱动程序所支持的内容的字符串，该字符串允许使用模式匹配元字符下划线 (_) 和百分号 (% ) 作为搜索模式中的有效字符。 此转义符仅适用于支持搜索字符串的那些目录函数参数。 如果此字符串为空，则驱动程序不支持搜索模式转义符。<br/><br/>因为此信息类型并不表示对 **LIKE** 谓词中转义符的一般支持，所以 SQL-92 不包括此字符串的要求。<br/><br/>此 *InfoType* 限制为目录函数。 有关在搜索模式字符串中使用转义符的说明，请参阅 [模式值参数](../develop-app/pattern-value-arguments.md)。|
|SQL_SERVER_NAME|1.0|具有实际数据源特定服务器名称的字符串;在 **SQLConnect**、 **SQLDriverConnect** 和 **SQLBrowseConnect** 期间使用数据源名称时非常有用。|
|SQL_SPECIAL_CHARACTERS|2.0|一个字符串，其中包含所有特殊字符 (即，除 a 到 z、A 到 Z、0到9和下划线) 之外的所有字符，这些字符可用于数据源上的标识符名称（如表名称、列名称或索引名称）中。 例如，"# $ ^"。 如果标识符包含一个或多个这些字符，则标识符必须是分隔的标识符。|
|SQL_SQL_CONFORMANCE|3.0|指示驱动程序支持的 SQL 92 级别的 SQLUINTEGER 值：<br/>SQL_SC_SQL92_ENTRY = 条目级别 SQL-92 兼容。<br/>SQL_SC_FIPS127_2_TRANSITIONAL = 符合 FIPS 127-2 转换级别。<br/>SQL_SC_SQL92_FULL = 完全级别的 SQL-92 兼容。<br/>SQL_SC_ SQL92_INTERMEDIATE = 中间级别 SQL-92 兼容。|
|SQL_SQL92_DATETIME_FUNCTIONS|3.0|SQLUINTEGER 位掩码，用于枚举驱动程序和关联数据源支持的日期时间标量函数，如 SQL-92 中所定义。<br/><br/>以下位掩码用于确定支持的日期时间函数：<br/>SQL_SDF_CURRENT_DATE<br/>SQL_SDF_CURRENT_TIME<br/>SQL_SDF_CURRENT_TIMESTAMP|
|SQL_SQL92_FOREIGN_KEY_DELETE_RULE|3.0|在 **DELETE** 语句中枚举外键支持的规则的 SQLUINTEGER 位掩码，如 SQL-92 中所定义。<br/><br/>以下位掩码用于确定数据源支持的子句：<br/>SQL_SFKD_CASCADE<br/>SQL_SFKD_NO_ACTION<br/>SQL_SFKD_SET_DEFAULT<br/>SQL_SFKD_SET_NULL<br/><br/>符合 FIPS 转换级别的驱动程序将始终返回所有支持的选项。|
|SQL_SQL92_FOREIGN_KEY_UPDATE_RULE|3.0|在 **UPDATE** 语句中枚举外键支持的规则的 SQLUINTEGER 位掩码，如 SQL-92 中所定义。<br/><br/>以下位掩码用于确定数据源支持的子句：<br/>SQL_SFKU_CASCADE<br/>SQL_SFKU_NO_ACTION<br/>SQL_SFKU_SET_DEFAULT<br/>SQL_SFKU_SET_NULL<br/><br/>符合 SQL-92 完全符合级别的驱动程序将始终返回所有这些选项的受支持。|
|SQL_SQL92_GRANT|3.0|SQLUINTEGER 位掩码，用于枚举 **GRANT** 语句中支持的子句（如 SQL-92 中所定义）。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定数据源支持的子句：<br/>SQL_SG_DELETE_TABLE (条目级别) <br/>SQL_SG_INSERT_COLUMN (的中间级别) <br/>SQL_SG_INSERT_TABLE (条目级别) <br/>SQL_SG_REFERENCES_TABLE (条目级别) <br/>SQL_SG_REFERENCES_COLUMN (条目级别) <br/>SQL_SG_SELECT_TABLE (条目级别) <br/>SQL_SG_UPDATE_COLUMN (条目级别) <br/>SQL_SG_UPDATE_TABLE (条目级别) <br/>SQL_SG_USAGE_ON_DOMAIN (FIPS 过渡级别) <br/>SQL_SG_USAGE_ON_CHARACTER_SET (FIPS 过渡级别) <br/>SQL_SG_USAGE_ON_COLLATION (FIPS 过渡级别) <br/>SQL_SG_USAGE_ON_TRANSLATION (FIPS 过渡级别) <br/>SQL_SG_WITH_GRANT_OPTION (条目级别) |
|SQL_SQL92_NUMERIC_VALUE_FUNCTIONS|3.0|SQLUINTEGER 位掩码，用于枚举由 SQL-92 中定义的驱动程序和关联数据源支持的数值标量函数。<br/><br/>以下位掩码用于确定支持哪些数值函数：<br/>SQL_SNVF_BIT_LENGTH<br/>SQL_SNVF_CHAR_LENGTH<br/>SQL_SNVF_CHARACTER_LENGTH<br/>SQL_SNVF_EXTRACT<br/>SQL_SNVF_OCTET_LENGTH<br/>SQL_SNVF_POSITION|
|SQL_SQL92_PREDICATES|3.0|SQLUINTEGER 位掩码，用于枚举 **SELECT** 语句中支持的谓词，如 SQL-92 中所定义。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定数据源支持的选项：<br/>SQL_SP_BETWEEN (条目级别) <br/>SQL_SP_COMPARISON (条目级别) <br/>SQL_SP_EXISTS (条目级别) <br/>SQL_SP_IN (条目级别) <br/>SQL_SP_ISNOTNULL (条目级别) <br/>SQL_SP_ISNULL (条目级别) <br/>SQL_SP_LIKE (条目级别) <br/>SQL_SP_MATCH_FULL (完整级别) <br/>SQL_SP_MATCH_PARTIAL (完整级别) <br/>SQL_SP_MATCH_UNIQUE_FULL (完整级别) <br/>SQL_SP_MATCH_UNIQUE_PARTIAL (完整级别) <br/>SQL_SP_OVERLAPS (FIPS 过渡级别) <br/>SQL_SP_QUANTIFIED_COMPARISON (条目级别) <br/>SQL_SP_UNIQUE (条目级别) |
|SQL_SQL92_RELATIONAL_JOIN_OPERATORS|3.0|SQLUINTEGER 位掩码，用于枚举 **SELECT** 语句中支持的关系联接运算符，如 SQL-92 中所定义。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定数据源支持的选项：<br/>SQL_SRJO_CORRESPONDING_CLAUSE (的中间级别) <br/>SQL_SRJO_CROSS_JOIN (完整级别) <br/>SQL_SRJO_EXCEPT_JOIN (的中间级别) <br/>SQL_SRJO_FULL_OUTER_JOIN (的中间级别) <br/>SQL_SRJO_INNER_JOIN (FIPS 过渡级别) <br/>SQL_SRJO_INTERSECT_JOIN (的中间级别) <br/>SQL_SRJO_LEFT_OUTER_JOIN (FIPS 过渡级别) <br/>SQL_SRJO_NATURAL_JOIN (FIPS 过渡级别) <br/>SQL_SRJO_RIGHT_OUTER_JOIN (FIPS 过渡级别) <br/>SQL_SRJO_UNION_JOIN (完整级别) <br/><br/>SQL_SRJO_INNER_JOIN 指示支持 **内部** 联接语法，而不是内部联接功能。 对 **内部联接** 语法的支持是 FIPS 过渡，而对内部联接功能的支持则是 **进入**。|
|SQL_SQL92_REVOKE|3.0|SQLUINTEGER 位掩码，用于枚举 **REVOKE** 语句中支持的子句（如 SQL-92 中所定义），受数据源支持。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定数据源支持的子句：<br/>SQL_SR_CASCADE (FIPS 过渡级别) <br/>SQL_SR_DELETE_TABLE (条目级别) <br/>SQL_SR_GRANT_OPTION_FOR (的中间级别) <br/>SQL_SR_INSERT_COLUMN (的中间级别) <br/>SQL_SR_INSERT_TABLE (条目级别) <br/>SQL_SR_REFERENCES_COLUMN (条目级别) <br/>SQL_SR_REFERENCES_TABLE (条目级别) <br/>SQL_SR_RESTRICT (FIPS 过渡级别) <br/>SQL_SR_SELECT_TABLE (条目级别) <br/>SQL_SR_UPDATE_COLUMN (条目级别) <br/>SQL_SR_UPDATE_TABLE (条目级别) <br/>SQL_SR_USAGE_ON_DOMAIN (FIPS 过渡级别) <br/>SQL_SR_USAGE_ON_CHARACTER_SET (FIPS 过渡级别) <br/>SQL_SR_USAGE_ON_COLLATION (FIPS 过渡级别) <br/>SQL_SR_USAGE_ON_TRANSLATION (FIPS 过渡级别) |
|SQL_SQL92_ROW_VALUE_CONSTRUCTOR|3.0|SQLUINTEGER 位掩码，用于枚举 **SELECT** 语句中支持的行值构造函数表达式，如 SQL-92 中所定义。 以下位掩码用于确定数据源支持的选项：<br/>SQL_SRVC_VALUE_EXPRESSION<br/>SQL_SRVC_NULL<br/>SQL_SRVC_DEFAULT<br/>SQL_SRVC_ROW_SUBQUERY|
|SQL_SQL92_STRING_FUNCTIONS|3.0|SQLUINTEGER 位掩码，用于枚举驱动程序和关联数据源支持的字符串标量函数，如 SQL-92 中所定义。<br/><br/>以下位掩码用于确定支持的字符串函数：<br/>SQL_SSF_CONVERT<br/>SQL_SSF_LOWERSQL_SSF_UPPER<br/>SQL_SSF_SUBSTRING<br/>SQL_SSF_TRANSLATE<br/>SQL_SSF_TRIM_BOTH<br/>SQL_SSF_TRIM_LEADING<br/>SQL_SSF_TRIM_TRAILING|
|SQL_SQL92_VALUE_EXPRESSIONS|3.0|SQLUINTEGER 位掩码，用于枚举 SQL-92 中定义的受支持的值表达式。<br/><br/>必须支持此功能的 SQL-92 或 FIPS 一致性级别，每个位掩码旁边的括号中会显示该级别。<br/><br/>以下位掩码用于确定数据源支持的选项：<br/>SQL_SVE_CASE (的中间级别) <br/>SQL_SVE_CAST (FIPS 过渡级别) <br/>SQL_SVE_COALESCE (的中间级别) <br/>SQL_SVE_NULLIF (的中间级别) |
|SQL_STANDARD_CLI_CONFORMANCE|3.0|SQLUINTEGER 位掩码，用于枚举驱动程序符合的 CLI 标准或标准。 以下位掩码用于确定驱动程序符合的级别：<br/>SQL_SCC_XOPEN_CLI_VERSION1：驱动程序符合开放组 CLI 版本1。<br/>SQL_SCC_ISO92_CLI：驱动程序符合 ISO 92 CLI。|
|SQL_STATIC_CURSOR_ATTRIBUTES1|3.0|描述驱动程序所支持的静态游标属性的 SQLUINTEGER 位掩码。 此位掩码包含特性的第一个子集;对于第二个子集，请参阅 SQL_STATIC_CURSOR_ATTRIBUTES2。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA1_NEXT<br/>SQL_CA1_ABSOLUTE<br/>SQL_CA1_RELATIVE<br/>SQL_CA1_BOOKMARK<br/>SQL_CA1_LOCK_NO_CHANGE<br/>SQL_CA1_LOCK_EXCLUSIVE<br/>SQL_CA1_LOCK_UNLOCK<br/>SQL_CA1_POS_POSITION<br/>SQL_CA1_POS_UPDATE<br/>SQL_CA1_POS_DELETE<br/>SQL_CA1_POS_REFRESH<br/>SQL_CA1_POSITIONED_UPDATE<br/>SQL_CA1_POSITIONED_DELETE<br/>SQL_CA1_SELECT_FOR_UPDATE<br/>SQL_CA1_BULK_ADD<br/>SQL_CA1_BULK_UPDATE_BY_BOOKMARK<br/>SQL_CA1_BULK_DELETE_BY_BOOKMARK<br/>SQL_CA1_BULK_FETCH_BY_BOOKMARK<br/><br/>有关这些位掩码的说明，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES1 (并在 "说明") 中将 "静态游标" 替换为 "动态游标"。<br/><br/>与 SQL-92 中间级别相容的驱动程序通常会返回受支持的 SQL_CA1_NEXT、SQL_CA1_ABSOLUTE 和 SQL_CA1_RELATIVE 选项，因为该驱动程序支持通过嵌入式 SQL FETCH 语句滚动的游标。 但这并不直接确定基础 SQL 支持，但可能不支持可滚动游标，即使对于符合 SQL-92 的中间级别驱动程序也是如此。|
|SQL_STATIC_CURSOR_ATTRIBUTES2|3.0|描述驱动程序所支持的静态游标属性的 SQLUINTEGER 位掩码。 此位掩码包含特性的第二个子集;对于第一个子集，请参阅 SQL_STATIC_CURSOR_ATTRIBUTES1。<br/><br/>以下位掩码用于确定受支持的属性：<br/>SQL_CA2_READ_ONLY_CONCURRENCY<br/>SQL_CA2_LOCK_CONCURRENCY<br/>SQL_CA2_OPT_ROWVER_CONCURRENCY<br/>SQL_CA2_OPT_VALUES_CONCURRENCY<br/>SQL_CA2_SENSITIVITY_ADDITIONS<br/>SQL_CA2_SENSITIVITY_DELETIONS<br/>SQL_CA2_SENSITIVITY_UPDATES<br/>SQL_CA2_MAX_ROWS_SELECT<br/>SQL_CA2_MAX_ROWS_INSERT<br/>SQL_CA2_MAX_ROWS_DELETE<br/>SQL_CA2_MAX_ROWS_UPDATE<br/>SQL_CA2_MAX_ROWS_CATALOG<br/>SQL_CA2_MAX_ROWS_AFFECTS_ALL<br/>SQL_CA2_CRC_EXACT<br/>SQL_CA2_CRC_APPROXIMATE<br/>SQL_CA2_SIMULATE_NON_UNIQUE<br/>SQL_CA2_SIMULATE_TRY_UNIQUE<br/>SQL_CA2_SIMULATE_UNIQUE<br/><br/>有关这些位掩码的说明，请参阅 SQL_DYNAMIC_CURSOR_ATTRIBUTES2 (并在 "说明") 中将 "静态游标" 替换为 "动态游标"。|
|SQL_STRING_FUNCTIONS|1.0|注意：信息类型是在 ODBC 1.0 中引入的;每个位掩码都带有引入它的版本标记。<br/><br/>SQLUINTEGER 位掩码，用于枚举驱动程序和关联数据源支持的标量字符串函数。<br/><br/>以下位掩码用于确定支持的字符串函数：<br/>SQL_FN_STR_ASCII (ODBC 1.0) <br/>SQL_FN_STR_BIT_LENGTH (ODBC 3.0) <br/>SQL_FN_STR_CHAR (ODBC 1.0) <br/>SQL_FN_STR_CHAR_LENGTH (ODBC 3.0) <br/>SQL_FN_STR_CHARACTER_LENGTH (ODBC 3.0) <br/>SQL_FN_STR_CONCAT (ODBC 1.0) <br/>SQL_FN_STR_DIFFERENCE (ODBC 2.0) <br/>SQL_FN_STR_INSERT (ODBC 1.0) <br/>SQL_FN_STR_LCASE (ODBC 1.0) <br/>SQL_FN_STR_LEFT (ODBC 1.0) <br/>SQL_FN_STR_LENGTH (ODBC 1.0) <br/>SQL_FN_STR_LOCATE (ODBC 1.0) <br/>SQL_FN_STR_LTRIM (ODBC 1.0) <br/>SQL_FN_STR_OCTET_LENGTH (ODBC 3.0) <br/>SQL_FN_STR_POSITION (ODBC 3.0) <br/>SQL_FN_STR_REPEAT (ODBC 1.0) <br/>SQL_FN_STR_REPLACE (ODBC 1.0) <br/>SQL_FN_STR_RIGHT (ODBC 1.0) <br/>SQL_FN_STR_RTRIM (ODBC 1.0) <br/>SQL_FN_STR_SOUNDEX (ODBC 2.0) <br/>SQL_FN_STR_SPACE (ODBC 2.0) <br/>SQL_FN_STR_SUBSTRING (ODBC 1.0) <br/>SQL_FN_STR_UCASE (ODBC 1.0) <br/><br/>如果应用程序可以调用带有 *string_exp1*、 *string_exp2* 和 *start* 参数的 **查找** 标量函数，则驱动程序将返回 SQL_FN_STR_LOCATE 位掩码。 如果应用程序只能调用带有 *string_exp1* 和 *STRING_EXP2* 参数的定位标量函数，则驱动程序将返回 SQL_FN_STR_LOCATE_2 位掩码。 完全支持 **定位** 标量函数的驱动程序将返回这两个位掩码。<br/><br/> (有关详细信息，请参阅附录 E "标量函数" 中的 [字符串函数](../appendixes/string-functions.md) 。) |
|SQL_SUBQUERIES|2.0|枚举支持子查询的谓词的 SQLUINTEGER 位掩码：<br/>SQL_SQ_CORRELATED_SUBQUERIES<br/>SQL_SQ_COMPARISON<br/>SQL_SQ_EXISTS<br/>SQL_SQ_INSQL_SQ_QUANTIFIED<br/><br/>SQL_SQ_CORRELATED_SUBQUERIES 位掩码指示支持子查询的所有谓词都支持相关子查询。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回一个位掩码，其中的所有位均已设置。|
|SQL_SYSTEM_FUNCTIONS|1.0|SQLUINTEGER 位掩码，用于枚举驱动程序和关联数据源支持的标量系统函数。<br/><br/>以下位掩码用于确定受支持的系统函数：<br/>SQL_FN_SYS_DBNAME<br/>SQL_FN_SYS_IFNULL<br/>SQL_FN_SYS_USERNAME|
|SQL_TABLE_TERM|1.0|包含表的数据源供应商名称的字符串;例如，"table" 或 "file"。<br/><br/>此字符串可以是大写、小写或混合大小写。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回 "表"。|
|SQL_TIMEDATE_ADD_INTERVALS|2.0|SQLUINTEGER 位掩码，枚举 TIMESTAMPADD 标量函数的驱动程序和关联数据源支持的时间戳间隔。<br/><br/>以下位掩码用于确定支持哪些时间间隔：<br/>SQL_FN_TSI_FRAC_SECOND<br/>SQL_FN_TSI_SECOND<br/>SQL_FN_TSI_MINUTE<br/>SQL_FN_TSI_HOUR<br/>SQL_FN_TSI_DAY<br/>SQL_FN_TSI_WEEK<br/>SQL_FN_TSI_MONTH<br/>SQL_FN_TSI_QUARTER<br/>SQL_FN_TSI_YEAR<br/><br/>符合 FIPS 转换级别的驱动程序将始终返回一个位掩码，其中设置了所有这些位。|
|SQL_TIMEDATE_DIFF_INTERVALS|2.0|SQLUINTEGER 位掩码，枚举 TIMESTAMPDIFF 标量函数的驱动程序和关联数据源支持的时间戳间隔。<br/><br/>以下位掩码用于确定支持哪些时间间隔：<br/>SQL_FN_TSI_FRAC_SECOND<br/>SQL_FN_TSI_SECOND<br/>SQL_FN_TSI_MINUTE<br/>SQL_FN_TSI_HOUR<br/>SQL_FN_TSI_DAY<br/>SQL_FN_TSI_WEEK<br/>SQL_FN_TSI_MONTH<br/>SQL_FN_TSI_QUARTER<br/>SQL_FN_TSI_YEAR<br/><br/>符合 FIPS 转换级别的驱动程序将始终返回一个位掩码，其中设置了所有这些位。|
|SQL_TIMEDATE_FUNCTIONS|1.0|注意：信息类型是在 ODBC 1.0 中引入的;每个位掩码都带有引入它的版本标记。<br/><br/>SQLUINTEGER 位掩码，用于枚举驱动程序和关联数据源支持的标量日期和时间函数。<br/><br/>以下位掩码用于确定支持的日期和时间函数：<br/>SQL_FN_TD_CURRENT_DATE (ODBC 3.0) <br/>SQL_FN_TD_CURRENT_TIME (ODBC 3.0) <br/>SQL_FN_TD_CURRENT_TIMESTAMP (ODBC 3.0) <br/>SQL_FN_TD_CURDATE (ODBC 1.0) <br/>SQL_FN_TD_CURTIME (ODBC 1.0) <br/>SQL_FN_TD_DAYNAME (ODBC 2.0) <br/>SQL_FN_TD_DAYOFMONTH (ODBC 1.0) <br/>SQL_FN_TD_DAYOFWEEK (ODBC 1.0) <br/>SQL_FN_TD_DAYOFYEAR (ODBC 1.0) <br/>SQL_FN_TD_EXTRACT (ODBC 3.0) <br/>SQL_FN_TD_HOUR (ODBC 1.0) <br/>SQL_FN_TD_MINUTE (ODBC 1.0) <br/>SQL_FN_TD_MONTH (ODBC 1.0) <br/>SQL_FN_TD_MONTHNAME (ODBC 2.0) <br/>SQL_FN_TD_NOW (ODBC 1.0) <br/>SQL_FN_TD_QUARTER (ODBC 1.0) <br/>SQL_FN_TD_SECOND (ODBC 1.0) <br/>SQL_FN_TD_TIMESTAMPADD (ODBC 2.0) <br/>SQL_FN_TD_TIMESTAMPDIFF (ODBC 2.0) <br/>SQL_FN_TD_WEEK (ODBC 1.0) <br/>SQL_FN_TD_YEAR (ODBC 1.0) |
|SQL_TXN_CAPABLE|1.0|注意：信息类型是在 ODBC 1.0 中引入的;每个返回值都带有引入它的版本进行标记。<br/><br/>描述驱动程序或数据源中的事务支持的 SQLUSMALLINT 值：<br/>SQL_TC_NONE = 不支持事务。  (ODBC 1.0) <br/>SQL_TC_DML = 事务只能包含数据操作语言 (DML) 语句 (**选择**、 **插入**、 **更新**、 **删除**) 。 数据定义语言 (在事务中遇到的 DDL) 语句将导致错误。  (ODBC 1.0) <br/>SQL_TC_DDL_COMMIT = 事务只能包含 DML 语句。 DDL 语句 (在事务中遇到的 **CREATE TABLE**、 **DROP INDEX** 等) 导致提交事务。  (ODBC 2.0) <br/>SQL_TC_DDL_IGNORE = 事务只能包含 DML 语句。 在事务中遇到的 DDL 语句将被忽略。  (ODBC 2.0) <br/>SQL_TC_ALL = 事务可以按任意顺序包含 DDL 语句和 DML 语句。  (ODBC 1.0) <br/><br/> (因为在 SQL-92 中必须支持事务，因此，符合 SQL-92 的驱动程序 [任何级别] 将永远不会返回 SQL_TC_NONE。 ) |
|SQL_TXN_ISOLATION_OPTION|1.0|SQLUINTEGER 位掩码，用于枚举驱动程序或数据源中可用的事务隔离级别。<br/><br/>以下位掩码与标志一起使用，以确定支持的选项：<br/>SQL_TXN_READ_UNCOMMITTED<br/>SQL_TXN_READ_COMMITTED<br/>SQL_TXN_REPEATABLE_READ<br/>SQL_TXN_SERIALIZABLE<br/><br/>有关这些隔离级别的说明，请参阅 SQL_DEFAULT_TXN_ISOLATION 的说明。<br/><br/>若要设置事务隔离级别，应用程序将调用 **SQLSetConnectAttr** 来设置 SQL_ATTR_TXN_ISOLATION 特性。 有关详细信息，请参阅 [SQLSetConnectAttr 函数](sqlsetconnectattr-function.md)。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回受支持的 SQL_TXN_SERIALIZABLE。 符合 FIPS 转换级别的驱动程序将始终返回所有支持的选项。|
|SQL_UNION|2.0|SQLUINTEGER 位掩码，枚举对 **UNION** 子句的支持：<br/>SQL_U_UNION = 数据源支持 **UNION** 子句。<br/>SQL_U_UNION_ALL = 数据源支持 **UNION** 子句中的 **ALL** 关键字。 在这种情况下， (**SQLGetInfo** 将同时返回 SQL_U_UNION 和 SQL_U_UNION_ALL ) 。<br/><br/>符合 SQL-92 项级别的驱动程序将始终返回这两个选项（如果支持）。|
|SQL_USER_NAME|1.0|使用特定数据库中的名称的字符串，该字符串可能不同于登录名。|
|SQL_XOPEN_CLI_YEAR|3.0|一个字符串，该字符串指示 ODBC 驱动程序管理器的版本完全符合的开放组规范的发布年份。|
  
## <a name="example"></a>示例  

 **SQLGetInfo** 返回受支持选项的列表作为 **INFOVALUEPTR* 中的 SQLUINTEGER 位掩码。 每个选项的位掩码与标志一起使用，以确定是否支持该选项。  
  
 例如，应用程序可以使用以下代码来确定与连接关联的驱动程序是否支持 SUBSTRING 标量函数。  
  
 有关使用 **SQLGetInfo** 的另一个示例，请参阅 [SQLTables 函数](sqltables-function.md)。  
  
```cpp  
SQLUINTEGER fFuncs;  
  
SQLGetInfo(hdbc,  
           SQL_STRING_FUNCTIONS,  
           (SQLPOINTER)&fFuncs,  
           sizeof(fFuncs),  
           NULL);  
  
// SUBSTRING supported  
if (fFuncs & SQL_FN_STR_SUBSTRING)  
   ;   // do something  
  
// SUBSTRING not supported  
else  
   ;   // do something else  
```  
  
## <a name="related-functions"></a>相关函数  

 返回连接属性的设置  
 [SQLGetConnectAttr 函数](sqlgetconnectattr-function.md)  
  
 确定驱动程序是否支持函数  
 [SQLGetFunctions 函数](sqlgetfunctions-function.md)  
  
 返回语句特性的设置  
 [SQLGetStmtAttr 函数](sqlgetstmtattr-function.md)  
  
 返回有关数据源的数据类型的信息  
 [SQLGetTypeInfo 函数](sqlgettypeinfo-function.md)  
  
## <a name="see-also"></a>另请参阅  

 [ODBC API 参考](odbc-api-reference.md)  
 [ODBC 头文件](../install/odbc-header-files.md)
