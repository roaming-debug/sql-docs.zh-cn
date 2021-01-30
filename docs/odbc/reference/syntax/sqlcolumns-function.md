---
description: SQLColumns 函数
title: SQLColumns 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLColumns
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColumns
helpviewer_keywords:
- SQLColumns function [ODBC]
ms.assetid: 4a3618b7-d2b8-43c6-a1fd-7a4e6fa8c7d0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 51a72000e071d89fc3f189b00c6d8491db1e6e9f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201977"
---
# <a name="sqlcolumns-function"></a>SQLColumns 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性：打开组  
  
 **摘要**  
 **SQLColumns** 返回指定表中的列名列表。 驱动程序将此信息作为结果集返回到指定的 *StatementHandle*。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLColumns(  
     SQLHSTMT       StatementHandle,  
     SQLCHAR *      CatalogName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      SchemaName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      TableName,  
     SQLSMALLINT    NameLength3,  
     SQLCHAR *      ColumnName,  
     SQLSMALLINT    NameLength4);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *CatalogName*  
 送目录名称。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时（例如，当驱动程序检索不同 Dbms 的数据时），则空字符串 ( "" ) 指示这些表没有目录。 *CatalogName* 不能包含字符串搜索模式。  
  
> [!NOTE]  
>  如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *CatalogName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *CatalogName* 是普通参数;它按原义处理，其大小写很重要。 有关详细信息，请参阅 [目录函数中的参数](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md)。  
  
 *NameLength1*  
 送**CatalogName* 中的字符的长度。  
  
 *SchemaName*  
 送架构名称的字符串搜索模式。 如果驱动程序支持某些表的架构，而不支持其他表的架构（例如，当驱动程序从不同 Dbms 检索数据时），则空字符串 ( "" ) 指示不具有架构的那些表。  
  
> [!NOTE]  
>  如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *SchemaName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *SchemaName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength2*  
 送**SchemaName* 中的字符的长度。  
  
 *TableName*  
 送表名的字符串搜索模式。  
  
> [!NOTE]  
>  如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *TableName* 将被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *TableName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength3*  
 送**TableName* 的长度（字符）。  
  
 *ColumnName*  
 送列名称的字符串搜索模式。  
  
> [!NOTE]  
>  如果 SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，则 *ColumnName* 被视为标识符，并且其大小写不重要。 如果 SQL_FALSE，则 *ColumnName* 为模式值参数;它按原义处理，其大小写很重要。  
  
 *NameLength4*  
 送**ColumnName* 的长度（字符）。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLColumns** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLColumns** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|在 *StatementHandle* 上打开了游标，并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则由驱动程序返回。<br /><br /> 在 *StatementHandle* 上打开了游标，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|40001|序列化失败|由于另一个事务发生资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY009|空值指针的使用无效|SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE， *CatalogName* 参数为 null 指针，SQL_CATALOG_NAME 的 *InfoType* 返回支持的目录名称。<br /><br />  (DM) SQL_ATTR_METADATA_ID 语句特性设置为 SQL_TRUE，并且 *SchemaName*、 *TableName* 或 *ColumnName* 参数为 null 指针。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLColumns** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 一个名称长度参数的值小于0但不等于 SQL_NTS。|  
|||名称长度参数之一的值超出了相应目录或名称的最大长度值。 可以通过使用 *InfoType* 值调用 **SQLGetInfo** 来获取每个目录或名称的最大长度。  (参阅 "注释"。) |  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|指定了目录名称，并且驱动程序或数据源不支持目录。<br /><br /> 指定了架构名称，并且驱动程序或数据源不支持架构。<br /><br /> 为架构名称、表名称或列名称指定了字符串搜索模式，并且数据源不支持其中一个或多个参数的搜索模式。<br /><br /> 驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 此函数通常在语句执行之前用于从数据源的目录中检索有关一个或多个表的列的信息。 **SQLColumns** 可用于检索 **SQLTables** 返回的所有类型的项的数据。 除了基表以外，这还可能包括 (但并不限于) 视图、同义词、系统表等。 与此相反，函数 **SQLColAttribute** 和 **SQLDescribeCol** 描述结果集中的列，而函数 **SQLNumResultCols** 返回结果集中的列数。 有关详细信息，请参阅 [目录数据的使用](../../../odbc/reference/develop-app/uses-of-catalog-data.md)。  
  
> [!NOTE]  
>  有关 ODBC 目录函数的常规用法、参数和返回数据的详细信息，请参阅 [目录函数](../../../odbc/reference/develop-app/catalog-functions.md)。  
  
 **SQLColumns** 将结果以标准结果集的形式返回，并按 TABLE_CAT、TABLE_SCHEM、TABLE_NAME 和 ORDINAL_POSITION 排序。  
  
> [!NOTE]  
>  当应用程序与 ODBC 2 一起使用时。*x* 驱动程序，则不会在结果集中返回 ORDINAL_POSITION 列。 因此，在使用 ODBC 2 时。*x* 驱动程序， **SQLColumns** 返回的列列表中列的顺序并不一定与应用程序对该表中的所有列执行 SELECT 语句时返回的列的顺序相同。  
  
> [!NOTE]  
>  **SQLColumns** 可能不会返回所有列。 例如，驱动程序可能不返回有关伪列的信息，如 Oracle ROWID。 应用程序可以使用任何有效的列，无论其是否由 **SQLColumns** 返回。  
>   
>  **SQLColumns** 未返回可由 **SQLStatistics** 返回的某些列。 例如， **SQLColumns** 不返回通过表达式或筛选器创建的索引中的列，例如薪金 + 权益或部门 = 0012。  
  
 VARCHAR 列的长度不显示在表中;实际长度取决于数据源。 若要确定 TABLE_CAT、TABLE_SCHEM、TABLE_NAME 和 COLUMN_NAME 列的实际长度，应用程序可以使用 SQL_MAX_CATALOG_NAME_LEN、SQL_MAX_SCHEMA_NAME_LEN、SQL_MAX_TABLE_NAME_LEN 和 SQL_MAX_COLUMN_NAME_LEN 选项调用 **SQLGetInfo** 。  
  
 已为 ODBC 3 重命名了以下列。*x*。 列名称更改不会影响向后兼容性，因为应用程序按列号进行绑定。  
  
|ODBC 2.0 列|ODBC 3。*x* 列|  
|---------------------|-----------------------|  
|TABLE_QUALIFIER|TABLE_CAT|  
|TABLE_OWNER|TABLE_SCHEM|  
|PRECISION|COLUMN_SIZE|  
|LENGTH|BUFFER_LENGTH|  
|SCALE|DECIMAL_DIGITS|  
|RADIX|NUM_PREC_RADIX|  
  
 以下列已添加到 **SQLColumns** for ODBC 3 返回的结果集中。*x*：  

:::row:::
    :::column:::
        CHAR_OCTET_LENGTH  
        COLUMN_DEF  
    :::column-end:::
    :::column:::
        IS_NULLABLE  
        ORDINAL_POSITION  
    :::column-end:::
    :::column:::
        SQL_DATA_TYPE  
        SQL_DATETIME_SUB  
    :::column-end:::
:::row-end:::

 下表列出了结果集中的列。 第18列之外的其他列 (IS_NULLABLE) 可由驱动程序定义。 应用程序应通过从结果集的末尾倒计时而不是指定显式序号位置，来获取对特定于驱动程序的列的访问权限。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
|列名称|列<br /><br /> 数值|数据类型|注释|  
|-----------------|-----------------------|---------------|--------------|  
|TABLE_CAT (ODBC 1.0) |1|Varchar|目录名称;如果不适用于数据源，则为 NULL。 如果驱动程序为某些表（而不是其他表）支持目录，例如当驱动程序从不同 Dbms 检索数据时，它将为没有目录的那些表返回空字符串 ( "" ) 。|  
|TABLE_SCHEM (ODBC 1.0) |2|Varchar|架构名称;如果不适用于数据源，则为 NULL。 如果驱动程序支持某些表的架构，而不支持其他表的架构，例如当驱动程序从不同 Dbms 检索数据时，它将为没有架构的那些表返回空字符串 ( "" ) 。|  
|TABLE_NAME (ODBC 1.0) |3|Varchar not NULL|表名。|  
|COLUMN_NAME (ODBC 1.0) |4|Varchar not NULL|列名称。 对于没有名称的列，驱动程序返回空字符串。|  
|DATA_TYPE (ODBC 1.0) |5|Smallint（非 NULL）|SQL 数据类型。 这可以是 ODBC SQL 数据类型，也可以是特定于驱动程序的 SQL 数据类型。 对于 datetime 和 interval 数据类型，此列将返回简洁的数据类型 (例如 SQL_TYPE_DATE 或 SQL_INTERVAL_YEAR_TO_MONTH，而不是 SQL_DATETIME 或 SQL_INTERVAL) 等 nonconcise 数据类型。 有关有效 ODBC SQL 数据类型的列表，请参阅附录 D：数据类型中的 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) 。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。<br /><br /> 为 ODBC 3 返回的数据类型。*x* 和 ODBC 2。*x* 应用程序可能不同。 有关详细信息，请参阅 [向后兼容性和标准符合性](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md)。|  
|TYPE_NAME (ODBC 1.0) |6|Varchar not NULL|依赖于数据源的数据类型名称;例如，"CHAR"、"VARCHAR"、"MONEY"、"LONG VARBINAR" 或 "CHAR ( ) 用于位数据"。|  
|COLUMN_SIZE (ODBC 1.0) |7|Integer|如果 DATA_TYPE SQL_CHAR 或 SQL_VARCHAR，则此列包含列的最大长度（以字符为限）。 对于 datetime 数据类型，这是在转换为字符时显示值所需的字符总数。 对于数字数据类型，这是列中允许的总位数或总位数，根据 NUM_PREC_RADIX 列。 对于间隔数据类型，这是时间间隔的字符串表示形式的字符表示形式 (按间隔的最大精度定义的字符数，请参阅附录 D：数据类型) 中的 [时间间隔数据类型长度](../../../odbc/reference/appendixes/interval-data-type-length.md) 。 有关详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|BUFFER_LENGTH (ODBC 1.0) |8|Integer|指定 SQL_C_DEFAULT 时，在 SQLGetData、SQLFetch 或 SQLFetchScroll 操作上传输的数据的长度（以字节为单位）。 对于数值数据，此大小可能与数据源中存储的数据的大小不同。 此值可能与字符数据 COLUMN_SIZE 列不同。 有关长度的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|DECIMAL_DIGITS (ODBC 1.0) |9|Smallint|小数点右边的有效位数的总数。 对于 SQL_TYPE_TIME 和 SQL_TYPE_TIMESTAMP，此列包含秒小数部分的位数。 对于其他数据类型，这是数据源中列的小数位数。 对于包含时间部分的间隔数据类型，此列包含小数点右侧的位数 (秒) 秒的小数部分。 对于不包含时间部分的间隔数据类型，此列为0。 有关十进制数字的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。 对于 DECIMAL_DIGITS 不适用的数据类型，将返回 NULL。|  
|NUM_PREC_RADIX (ODBC 1.0) |10|Smallint|对于数字数据类型，为10或2。 如果为10，则 COLUMN_SIZE 和 DECIMAL_DIGITS 中的值提供列允许的小数位数。 例如，DECIMAL (12，5) 列返回 NUM_PREC_RADIX 为10，COLUMN_SIZE 为12，而 DECIMAL_DIGITS 为 5;FLOAT 列可能返回 NUM_PREC_RADIX 10、COLUMN_SIZE 15 和 DECIMAL_DIGITS 为 NULL。<br /><br /> 如果为2，则 COLUMN_SIZE 和 DECIMAL_DIGITS 中的值将给出列中允许的位数。 例如，FLOAT 列可能返回基数2、COLUMN_SIZE 53 和 NULL 的 DECIMAL_DIGITS。<br /><br /> 对于 NUM_PREC_RADIX 不适用的数据类型，将返回 NULL。|  
|可为 NULL (ODBC 1.0) |11|Smallint（非 NULL）|如果列中不能包含 NULL 值，则 SQL_NO_NULLS。<br /><br /> 如果列接受空值，则 SQL_NULLABLE。<br /><br /> 如果不知道列是否接受空值，则 SQL_NULLABLE_UNKNOWN。<br /><br /> 为此列返回的值与为 IS_NULLABLE 列返回的值不同。 可以为 NULL 的列指示列可以接受 Null，但不能确定列不接受 Null 值。 IS_NULLABLE 列表明，列不能接受 Null 值，但却无法指示列是否接受 Null 值。|  
|ODBC 1.0)  (说明|12|Varchar|列的说明。|  
|COLUMN_DEF (ODBC 3.0) |13|Varchar|列的默认值。 如果此列中的值用引号引起来，则应将其解释为字符串。<br /><br /> 如果将 NULL 指定为默认值，则此列为单词 NULL，而不是用引号引起来。 如果在不截断的情况下不能表示默认值，此列将包含截断的，而不包含单引号。 如果未指定默认值，则此列为 NULL。<br /><br /> COLUMN_DEF 的值可以用于生成新的列定义，除非它包含截断的值。|  
|SQL_DATA_TYPE (ODBC 3.0) |14|Smallint（非 NULL）|SQL 数据类型，因为它显示在 IRD 的 "SQL_DESC_TYPE 记录" 字段中。 这可以是 ODBC SQL 数据类型，也可以是特定于驱动程序的 SQL 数据类型。 此列与 DATA_TYPE 列相同，datetime 和 interval 数据类型除外。 此列返回 nonconcise 数据类型 (例如 SQL_DATETIME 或 SQL_INTERVAL) ，而不是将数据类型 (或 SQL_TYPE_DATE 的简单数据类型（例如 SQL_INTERVAL_YEAR_TO_MONTH 或) 或日期时间间隔数据类型。 如果此列返回 SQL_DATETIME 或 SQL_INTERVAL，则可以从 SQL_DATETIME_SUB 列确定特定数据类型。 有关有效 ODBC SQL 数据类型的列表，请参阅附录 D：数据类型中的 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) 。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。<br /><br /> 为 ODBC 3 返回的数据类型。*x* 和 ODBC 2。*x* 应用程序可能不同。 有关详细信息，请参阅 [向后兼容性和标准符合性](../../../odbc/reference/develop-app/backward-compatibility-and-standards-compliance.md)。|  
|SQL_DATETIME_SUB (ODBC 3.0) |15|Smallint|Datetime 和 interval 数据类型的子类型代码。 对于其他数据类型，该列返回 NULL。 有关 datetime 和 interval 子代码的详细信息，请参阅 [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)中的 "SQL_DESC_DATETIME_INTERVAL_CODE"。|  
|CHAR_OCTET_LENGTH (ODBC 3.0) |16|Integer|字符或二进制数据类型列的最大长度（以字节为单位）。 对于所有其他数据类型，此列返回 NULL。|  
|ORDINAL_POSITION (ODBC 3.0) |17|Integer（非 NULL）|表中列的序号位置。 表中的第一列是数字1。|  
|IS_NULLABLE (ODBC 3.0) |18|Varchar|如果列不包含 Null，则为 "否"。<br /><br /> 如果列可以包含 Null，则为 "是"。<br /><br /> 如果为 Null 性为未知，该列将返回零长度字符串。<br /><br /> 根据 ISO 规则确定为 Null 性。 遵从 ISO SQL 标准的 DBMS 不能返回空字符串。<br /><br /> 为此列返回的值与为可为 NULL 的列返回的值不同。  (查看可为 NULL 的列的说明。 ) |  
  
## <a name="code-example"></a>代码示例  
 在下面的示例中，应用程序声明 **SQLColumns** 返回的结果集的缓冲区。 它调用 **SQLColumns** 来返回一个结果集，该结果集描述 EMPLOYEE 表中的每一列。 然后，它调用 **SQLBindCol** 将结果集中的列绑定到缓冲区。 最后，应用程序将每行数据与 **SQLFetch** 一起提取并处理数据。  
  
```cpp  
// SQLColumns_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqlext.h>  
#define STR_LEN 128 + 1  
#define REM_LEN 254 + 1  
  
// Declare buffers for result set data  
SQLCHAR szSchema[STR_LEN];  
SQLCHAR szCatalog[STR_LEN];  
SQLCHAR szColumnName[STR_LEN];  
SQLCHAR szTableName[STR_LEN];  
SQLCHAR szTypeName[STR_LEN];  
SQLCHAR szRemarks[REM_LEN];  
SQLCHAR szColumnDefault[STR_LEN];  
SQLCHAR szIsNullable[STR_LEN];  
  
SQLINTEGER ColumnSize;  
SQLINTEGER BufferLength;  
SQLINTEGER CharOctetLength;  
SQLINTEGER OrdinalPosition;  
  
SQLSMALLINT DataType;  
SQLSMALLINT DecimalDigits;  
SQLSMALLINT NumPrecRadix;  
SQLSMALLINT Nullable;  
SQLSMALLINT SQLDataType;  
SQLSMALLINT DatetimeSubtypeCode;  
  
SQLHSTMT hstmt = NULL;  
  
// Declare buffers for bytes available to return  
SQLINTEGER cbCatalog;  
SQLINTEGER cbSchema;  
SQLINTEGER cbTableName;  
SQLINTEGER cbColumnName;  
SQLINTEGER cbDataType;  
SQLINTEGER cbTypeName;  
SQLINTEGER cbColumnSize;  
SQLLEN cbBufferLength;  
SQLINTEGER cbDecimalDigits;  
SQLINTEGER cbNumPrecRadix;  
SQLINTEGER cbNullable;  
SQLINTEGER cbRemarks;  
SQLINTEGER cbColumnDefault;  
SQLINTEGER cbSQLDataType;  
SQLINTEGER cbDatetimeSubtypeCode;  
SQLINTEGER cbCharOctetLength;  
SQLINTEGER cbOrdinalPosition;  
SQLINTEGER cbIsNullable;  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt = 0;  
   SQLRETURN retcode;  
  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   retcode = SQLColumns(hstmt, NULL, 0, NULL, 0, (SQLCHAR*)"CUSTOMERS", SQL_NTS, NULL, 0);  
  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      // Bind columns in result set to buffers  
      SQLBindCol(hstmt, 1, SQL_C_CHAR, szCatalog, STR_LEN,&cbCatalog);  
      SQLBindCol(hstmt, 2, SQL_C_CHAR, szSchema, STR_LEN, &cbSchema);  
      SQLBindCol(hstmt, 3, SQL_C_CHAR, szTableName, STR_LEN,&cbTableName);  
      SQLBindCol(hstmt, 4, SQL_C_CHAR, szColumnName, STR_LEN, &cbColumnName);  
      SQLBindCol(hstmt, 5, SQL_C_SSHORT, &DataType, 0, &cbDataType);  
      SQLBindCol(hstmt, 6, SQL_C_CHAR, szTypeName, STR_LEN, &cbTypeName);  
      SQLBindCol(hstmt, 7, SQL_C_SLONG, &ColumnSize, 0, &cbColumnSize);  
      SQLBindCol(hstmt, 8, SQL_C_SLONG, &BufferLength, 0, &cbBufferLength);  
      SQLBindCol(hstmt, 9, SQL_C_SSHORT, &DecimalDigits, 0, &cbDecimalDigits);  
      SQLBindCol(hstmt, 10, SQL_C_SSHORT, &NumPrecRadix, 0, &cbNumPrecRadix);  
      SQLBindCol(hstmt, 11, SQL_C_SSHORT, &Nullable, 0, &cbNullable);  
      SQLBindCol(hstmt, 12, SQL_C_CHAR, szRemarks, REM_LEN, &cbRemarks);  
      SQLBindCol(hstmt, 13, SQL_C_CHAR, szColumnDefault, STR_LEN, &cbColumnDefault);  
      SQLBindCol(hstmt, 14, SQL_C_SSHORT, &SQLDataType, 0, &cbSQLDataType);  
      SQLBindCol(hstmt, 15, SQL_C_SSHORT, &DatetimeSubtypeCode, 0, &cbDatetimeSubtypeCode);  
      SQLBindCol(hstmt, 16, SQL_C_SLONG, &CharOctetLength, 0, &cbCharOctetLength);  
      SQLBindCol(hstmt, 17, SQL_C_SLONG, &OrdinalPosition, 0, &cbOrdinalPosition);  
      SQLBindCol(hstmt, 18, SQL_C_CHAR, szIsNullable, STR_LEN, &cbIsNullable);  
  
      while (SQL_SUCCESS == retcode) {  
         retcode = SQLFetch(hstmt);  
         /*  
         if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // show_error();  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
            0;   // Process fetched data  
         else  
            break;  
        */  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回一个或多个列的权限|[SQLColumnPrivileges 函数](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|提取多行数据|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|返回唯一标识行的列，或由事务自动更新的列|[SQLSpecialColumns 函数](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|  
|返回表统计信息和索引|[SQLStatistics 函数](../../../odbc/reference/syntax/sqlstatistics-function.md)|  
|返回数据源中的表的列表|[SQLTables 函数](../../../odbc/reference/syntax/sqltables-function.md)|  
|返回一个或多个表的权限|[SQLTablePrivileges Function（SQLTablePrivileges 函数）](../../../odbc/reference/syntax/sqltableprivileges-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
