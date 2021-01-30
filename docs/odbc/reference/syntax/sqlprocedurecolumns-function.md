---
description: SQLProcedureColumns Function（SQLProcedureColumns 函数）
title: SQLProcedureColumns 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLProcedureColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLProcedureColumns
helpviewer_keywords:
- SQLProcedureColumns function [ODBC]
ms.assetid: 4ca37b28-a6df-465b-8988-d422d37fc025
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 63025751b1ec5e93f5889d77c373cbedb19bf543
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192579"
---
# <a name="sqlprocedurecolumns-function"></a>SQLProcedureColumns Function（SQLProcedureColumns 函数）
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLProcedureColumns** 返回输入和输出参数的列表，以及构成指定过程的结果集的列。 驱动程序将该信息作为指定语句的结果集返回。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLProcedureColumns(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CatalogName,  
     SQLSMALLINT   NameLength1,  
     SQLCHAR *     SchemaName,  
     SQLSMALLINT   NameLength2,  
     SQLCHAR *     ProcName,  
     SQLSMALLINT   NameLength3,  
     SQLCHAR *     ColumnName,  
     SQLSMALLINT   NameLength4);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *CatalogName*  
 送过程目录名称。 如果驱动程序支持某些过程的目录，而不支持其他过程的目录，例如当驱动程序从不同 Dbms 检索数据时，空字符串 ( "" ) 表示不具有目录的那些过程。 *CatalogName* 不能包含字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *CatalogName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *CatalogName* 是普通参数;它按原义处理，其大小写很重要。 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。  
  
 *NameLength1*  
 送**CatalogName* 中的字符的长度。  
  
 *SchemaName*  
 送过程架构名称的字符串搜索模式。 如果驱动程序支持某些过程的架构，而不支持其他过程的架构，例如当驱动程序从不同 Dbms 检索数据时，空字符串 ( "" ) 表示不具有架构的那些过程。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *SchemaName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *SchemaName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength2*  
 送**SchemaName* 中的字符的长度。  
  
 *ProcName*  
 送过程名称的字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *ProcName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *ProcName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength3*  
 送**ProcName* 中的字符的长度。  
  
 *ColumnName*  
 送列名称的字符串搜索模式。  
  
 如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *ColumnName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *ColumnName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength4*  
 送**ColumnName* 的长度（字符）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLProcedureColumns** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLProcedureColumns** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则由驱动程序返回。<br /><br /> 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中 **SQLError** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效|SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE， *CatalogName* 参数为 null 指针，SQL_CATALOG_NAME 的 *InfoType* 返回支持的目录名称。<br /><br />  (DM) SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，而 *SchemaName*、 *ProcName* 或 *ColumnName* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 SQLProcedureColumns 函数时，此 aynschronous 函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY090|字符串或缓冲区长度无效| (DM) 一个名称长度参数的值小于0但不等于 SQL_NTS。<br /><br /> 名称长度参数之一的值超出了相应的目录、架构、过程或列名称的最大长度值。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|指定了过程目录，并且该驱动程序或数据源不支持目录。<br /><br /> 指定了过程架构，但驱动程序或数据源不支持架构。<br /><br /> 为过程架构、过程名称或列名指定了字符串搜索模式，并且数据源不支持其中一个或多个参数的搜索模式。<br /><br /> 驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|超时期限已到数据源返回结果集之前过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 此函数通常在语句执行之前用于检索有关过程参数的信息，以及构成由过程返回的结果集的列（如果有）。 有关详细信息，请参阅[过程](../../../odbc/reference/develop-app/procedures-odbc.md)。  
  
> [!NOTE]  
>  **SQLProcedureColumns** 可能不会返回过程使用的所有列。 例如，驱动程序可能只返回有关过程所使用的参数的信息，而不返回所生成的结果集中的列的相关信息。  
  
 *SchemaName*、 *ProcName* 和 *ColumnName* 参数接受搜索模式。 有关有效搜索模式的详细信息，请参阅 [模式值参数](../../../odbc/reference/develop-app/pattern-value-arguments.md)。  
  
> [!NOTE]  
>  有关 ODBC 目录函数的常规用法、参数和返回数据的详细信息，请参阅 [目录函数](../../../odbc/reference/develop-app/catalog-functions.md)。  
  
 **SQLProcedureColumns** 将结果以标准结果集的形式返回，并按 PROCEDURE_CAT、PROCEDURE_SCHEM、PROCEDURE_NAME 和 COLUMN_TYPE 排序。 将按以下顺序为每个过程返回列名：返回值的名称、过程调用中每个参数的名称 (按调用顺序) ，然后由过程返回的结果集中的每一列的名称)  (列顺序。  
  
 应用程序应将特定于驱动程序的列绑定到结果集的末尾。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
 若要确定 PROCEDURE_CAT、PROCEDURE_SCHEM、PROCEDURE_NAME 和 COLUMN_NAME 列的实际长度，应用程序可以使用 SQL_MAX_CATALOG_NAME_LEN、SQL_MAX_SCHEMA_NAME_LEN、SQL_MAX_PROCEDURE_NAME_LEN 和 SQL_MAX_COLUMN_NAME_LEN 选项调用 **SQLGetInfo** 。  
  
 已为 ODBC 3 重命名了以下列。*x*。 列名称更改不会影响向后兼容性，因为应用程序按列号进行绑定。  
  
|ODBC 2.0 列|ODBC 3。*x* 列|  
|---------------------|-----------------------|  
|PROCEDURE_QUALIFIER|PROCEDURE_CAT|  
|过程 _OWNER|PROCEDURE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 以下列已添加到 **SQLProcedureColumns** for ODBC 3 返回的结果集中。*x*：  
  
-   COLUMN_DEF  
  
-   DATETIME_CODE  
  
-   CHAR_OCTET_LENGTH  
  
-   ORDINAL_POSITION  
  
-   IS_NULLABLE  
  
 下表列出了结果集中的列。 列19之外的其他列 (IS_NULLABLE) 可由驱动程序定义。 应用程序应通过从结果集的末尾倒计时而不是指定显式序号位置，来获取对驱动程序特定列的访问权限。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
|列名称|列号|数据类型|注释|  
|-----------------|-------------------|---------------|--------------|  
|PROCEDURE_CAT (ODBC 2.0) |1|Varchar|过程目录名称;如果不适用于数据源，则为 NULL。 如果驱动程序支持某些过程的目录，而不支持其他过程的目录，例如当驱动程序从不同 Dbms 检索数据时，它将为没有目录的那些过程返回空字符串 ( "" ) 。|  
|PROCEDURE_SCHEM (ODBC 2.0) |2|Varchar|过程架构名称;如果不适用于数据源，则为 NULL。 如果驱动程序支持某些过程的架构，而不支持其他过程的架构，例如当驱动程序从不同 Dbms 检索数据时，它将为没有架构的那些过程返回空字符串 ( "" ) 。|  
|PROCEDURE_NAME (ODBC 2.0) |3|Varchar not NULL|过程名。 对于没有名称的过程，将返回空字符串。|  
|COLUMN_NAME (ODBC 2.0) |4|Varchar not NULL|过程列名称。 驱动程序为没有名称的过程列返回空字符串。|  
|COLUMN_TYPE (ODBC 2.0) |5|Smallint（非 NULL）|将过程列定义为参数或结果集列：<br /><br /> SQL_PARAM_TYPE_UNKNOWN：过程列是其类型未知的参数。  (ODBC 1.0) <br /><br /> SQL_PARAM_INPUT：过程列是输入参数。  (ODBC 1.0) <br /><br /> SQL_PARAM_INPUT_OUTPUT：过程列是一个输入/输出参数。  (ODBC 1.0) <br /><br /> SQL_PARAM_OUTPUT：过程列为 OUTPUT 参数。  (ODBC 2.0) <br /><br /> SQL_RETURN_VALUE：过程列是过程的返回值。  (ODBC 2.0) <br /><br /> SQL_RESULT_COL：过程列是结果集列。  (ODBC 1.0) |  
|DATA_TYPE (ODBC 2.0) |6|Smallint（非 NULL）|SQL 数据类型。 这可以是 ODBC SQL 数据类型，也可以是特定于驱动程序的 SQL 数据类型。 对于 datetime 和 interval 数据类型，此列返回简洁的数据类型 (例如 SQL_TYPE_TIME 或 SQL_INTERVAL_YEAR_TO_MONTH) 。 有关有效 ODBC SQL 数据类型的列表，请参阅附录 D：数据类型中的 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) 。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。|  
|TYPE_NAME (ODBC 2.0) |7|Varchar not NULL|依赖于数据源的数据类型名称;例如，"CHAR"、"VARCHAR"、"MONEY"、"LONG VARBINARY" 或 "CHAR ( ) 用于位数据"。|  
|COLUMN_SIZE (ODBC 2.0) |8|Integer|数据源上过程列的列大小。 对于列大小不适用的数据类型，将返回 NULL。 有关精度的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|BUFFER_LENGTH (ODBC 2.0) |9|Integer|当指定 SQL_C_DEFAULT 时，在 **SQLGetData** 或 **SQLFetch** 操作上传输的数据的长度（以字节为单位）。 对于数值数据，此大小可能不同于数据源中存储的数据的大小。 有关详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。|  
|DECIMAL_DIGITS (ODBC 2.0) |10|Smallint|数据源中过程列的十进制数字。 对于不适用十进制数字的数据类型，将返回 NULL。 有关十进制数字的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md)。|  
|NUM_PREC_RADIX (ODBC 2.0) |11|Smallint|对于数字数据类型，为10或2。<br /><br /> 如果为10，则 COLUMN_SIZE 和 DECIMAL_DIGITS 中的值提供列允许的小数位数。 例如，DECIMAL (12，5) 列返回 NUM_PREC_RADIX 为10，COLUMN_SIZE 为12，而 DECIMAL_DIGITS 为 5;FLOAT 列可能返回 NUM_PREC_RADIX 10、COLUMN_SIZE 15 和 DECIMAL_DIGITS 为 NULL。<br /><br /> 如果为2，则 COLUMN_SIZE 和 DECIMAL_DIGITS 中的值将给出列中允许的位数。 例如，FLOAT 列可能返回 NUM_PREC_RADIX 2、53 COLUMN_SIZE 和 DECIMAL_DIGITS 为 NULL。<br /><br /> 对于 NUM_PREC_RADIX 不适用的数据类型，将返回 NULL。|  
|可为 NULL (ODBC 2.0) |12|Smallint（非 NULL）|过程列是否接受空值：<br /><br /> SQL_NO_NULLS：过程列不接受 NULL 值。<br /><br /> SQL_NULLABLE：过程列接受 NULL 值。<br /><br /> SQL_NULLABLE_UNKNOWN：如果过程列接受 NULL 值，则未知。|  
|ODBC 2.0)  (说明|13|Varchar|过程列的说明。|  
|COLUMN_DEF (ODBC 3.0) |14|Varchar|列的默认值。<br /><br /> 如果将 NULL 指定为默认值，则此列为单词 NULL，而不是用引号引起来。 如果在不截断的情况下不能表示默认值，则此列包含截断的，且不带引号。 如果未指定默认值，则此列为 NULL。<br /><br /> COLUMN_DEF 的值可以用于生成新的列定义，除非它包含截断的值。|  
|SQL_DATA_TYPE (ODBC 3.0) |15|Smallint（非 NULL）|SQL 数据类型在描述符的 "SQL_DESC_TYPE" 字段中显示的值。 此列与 DATA_TYPE 列相同，datetime 和 interval 数据类型除外。<br /><br /> 对于 datetime 和 interval 数据类型，结果集中的 "SQL_DATA_TYPE" 字段将返回 SQL_INTERVAL 或 SQL_DATETIME，并且 SQL_DATETIME_SUB 字段将返回特定时间间隔或日期时间数据类型的子代码。  (参见 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。 ) |  
|SQL_DATETIME_SUB (ODBC 3.0) |16|Smallint|Datetime 和 interval 数据类型的子类型代码。 对于其他数据类型，该列返回 NULL。|  
|CHAR_OCTET_LENGTH (ODBC 3.0) |17|Integer|字符或二进制数据类型列的最大长度（以字节为单位）。 对于所有其他数据类型，此列返回 NULL。|  
|ORDINAL_POSITION (ODBC 3.0) |18|Integer（非 NULL）|对于输入和输出参数，过程定义中参数的序号位置 (递增参数顺序，从 1) 开始。 对于返回值 (如果任何) ，则返回0。 对于结果集的列，是结果集中列的序号位置，结果集中的第一列为数字1。 如果有多个结果集，则以特定于驱动程序的方式返回列序号位置。|  
|IS_NULLABLE (ODBC 3.0) |19|Varchar|如果列不包含 Null，则为 "否"。<br /><br /> 如果列可以包含 Null，则为 "是"。<br /><br /> 如果为 Null 性为未知，该列将返回零长度字符串。<br /><br /> 根据 ISO 规则确定为 Null 性。 遵从 ISO SQL 标准的 DBMS 不能返回空字符串。<br /><br /> 该列返回的值与 NULLABLE 列返回的值不同。  (查看可为 NULL 的列的说明。 ) |  
  
## <a name="code-example"></a>代码示例  
 请参阅 [过程调用](../../../odbc/reference/develop-app/procedure-calls.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|按只进方向提取单个行或数据块|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|返回数据源中的过程列表|[SQLProcedures 函数](../../../odbc/reference/syntax/sqlprocedures-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
