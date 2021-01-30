---
description: SQLGetTypeInfo 函数
title: SQLGetTypeInfo 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetTypeInfo
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetTypeInfo
helpviewer_keywords:
- SQLGetTypeInfo function [ODBC]
ms.assetid: bdedb044-8924-4ca4-85f3-8b37578e0257
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 55de323b6eace8fa2aa8b138f30ee93938122bc0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189716"
---
# <a name="sqlgettypeinfo-function"></a>SQLGetTypeInfo 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetTypeInfo** 返回有关数据源支持的数据类型的信息。 驱动程序以 SQL 结果集的形式返回该信息。 数据类型旨在 (DDL) 语句中使用数据定义语言。  
  
> [!IMPORTANT]  
>  应用程序必须使用 **ALTER TABLE** 和 **CREATE TABLE** 语句中 **SQLGetTypeInfo** 结果集的 TYPE_NAME 列中返回的类型名称。 **SQLGetTypeInfo** 可能返回 DATA_TYPE 列中具有相同值的多个行。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetTypeInfo(  
     SQLHSTMT      StatementHandle,  
     SQLSMALLINT   DataType);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送结果集的语句句柄。  
  
 *DataType*  
 送SQL 数据类型。 这必须是 "附录 D：数据类型" 的 " [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) " 部分或特定于驱动程序的 sql 数据类型的值之一。 SQL_ALL_TYPES 指定应返回有关所有数据类型的信息。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetTypeInfo** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetTypeInfo** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|由于实现工作条件，指定的语句特性无效，因此临时替换相似的值。  (调用 **SQLGetStmtAttr** 来确定暂时替换的值。 ) 替换值对 *StatementHandle* 有效，直到关闭游标。 可以更改的语句属性有： SQL_ATTR_CONCURRENCY、SQL_ATTR_CURSOR_TYPE、SQL_ATTR_KEYSET_SIZE、SQL_ATTR_MAX_LENGTH、SQL_ATTR_MAX_ROWS、SQL_ATTR_QUERY_TIMEOUT 和 SQL_ATTR_SIMULATE_CURSOR。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|24000|无效的游标状态|在 StatementHandle 上打开了游标 *，* 并且调用了 **SQLFetch** 或 **SQLFetchScroll** 。 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器将返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则由驱动程序返回。<br /><br /> 已在 *StatementHandle* 上打开一个结果集，但尚未调用 **SQLFetch** 或 **SQLFetchScroll** 。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数期间关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY004|SQL 数据类型无效|为参数 *数据* 类型指定的值既不是有效的 ODBC SQL 数据类型标识符，也不是驱动程序支持的特定于驱动程序的数据类型标识符。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理，然后在完成执行之前调用了函数，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数在完成执行之前被调用，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLGetTypeInfo** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持 SQL_ATTR_CONCURRENCY 和 SQL_ATTR_CURSOR_TYPE 语句特性的当前设置的组合。<br /><br /> SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE，并且 SQL_ATTR_CURSOR_TYPE 语句特性设置为该驱动程序不支持书签的游标类型。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtAttr** 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 相对应的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 **SQLGetTypeInfo** 将结果以标准结果集的形式返回，并按 DATA_TYPE 然后按数据类型映射到相应 ODBC SQL 数据类型的紧密程度进行排序。 数据源定义的数据类型优先于用户定义数据类型。 因此，排序顺序并不一定是一致的，但可以首先通用化为 DATA_TYPE，后跟 TYPE_NAME，两者都是升序的。 例如，假设数据源定义了整数和计数器数据类型，其中计数器是自动递增的，并且还定义了用户定义数据类型 WHOLENUM。 这些值将按 order INTEGER、WHOLENUM 和 COUNTER 返回，因为 WHOLENUM 与 ODBC SQL 数据类型密切对应 SQL_INTEGER，而自动递增的数据类型（即使数据源支持）并不与 ODBC SQL 数据类型密切对应。 有关此信息的使用方式的信息，请参阅 [DDL 语句](../../../odbc/reference/develop-app/ddl-statements.md)。  
  
 如果 *DataType* 参数指定的数据类型对驱动程序所支持的 ODBC 版本有效，但驱动程序不支持该数据类型，则将返回一个空结果集。  
  
> [!NOTE]  
>  有关 ODBC 目录函数的常规用法、参数和返回数据的详细信息，请参阅 [目录函数](../../../odbc/reference/develop-app/catalog-functions.md)。  
  
 已为 ODBC 3 重命名了以下列。*x*。 列名称更改不会影响向后兼容性，因为应用程序按列号进行绑定。  
  
|ODBC 2.0 列|ODBC 3。*x* 列|  
|---------------------|-----------------------|  
|PRECISION|COLUMN_SIZE|  
|MONEY|FIXED_PREC_SCALE|  
|AUTO_INCREMENT|AUTO_UNIQUE_VALUE|  
  
 以下列已添加到 **SQLGetTypeInfo** for ODBC 3 返回的结果集中。*x*：  
  
-   SQL_DATA_TYPE  
  
-   INTERVAL_PRECISION  
  
-   SQL_DATETIME_SUB  
  
-   NUM_PREC_RADIX  
  
 下表列出了结果集中的列。 列19之外的其他列 (INTERVAL_PRECISION) 可由驱动程序定义。 应用程序应通过从结果集的末尾倒计时而不是指定显式序号位置，来获取对驱动程序特定列的访问权限。 有关详细信息，请参阅 [目录函数返回的数据](../../../odbc/reference/develop-app/data-returned-by-catalog-functions.md)。  
  
> [!NOTE]  
>  **SQLGetTypeInfo** 可能不会返回所有数据类型。 例如，驱动程序可能不返回用户定义数据类型。 无论应用程序是否由 **SQLGetTypeInfo** 返回，应用程序都可以使用任何有效的数据类型。 **SQLGetTypeInfo** 返回的数据类型是数据源所支持的数据类型。 它们旨在 (DDL) 语句中使用数据定义语言。 驱动程序可以使用 **SQLGetTypeInfo** 返回的类型以外的数据类型返回结果集数据。 在为目录函数创建结果集时，驱动程序可能使用数据源不支持的数据类型。  
  
|列名称|列<br /><br /> 数值|数据类型|注释|  
|-----------------|-----------------------|---------------|--------------|  
|TYPE_NAME (ODBC 2.0) |1|Varchar not NULL|依赖于数据源的数据类型名称;例如，"CHAR ( # A1"、"VARCHAR ( # A3"、"MONEY"、"LONG VARBINARY" 或 "CHAR ( ) 用于位数据"。 应用程序必须在 **CREATE TABLE** 和 **ALTER TABLE** 语句中使用此名称。|  
|DATA_TYPE (ODBC 2.0) |2|Smallint（非 NULL）|SQL 数据类型。 这可以是 ODBC SQL 数据类型，也可以是特定于驱动程序的 SQL 数据类型。 对于 datetime 或 interval 数据类型，此列返回简洁的数据类型 (例如 SQL_TYPE_TIME 或 SQL_INTERVAL_YEAR_TO_MONTH) 。 有关有效 ODBC SQL 数据类型的列表，请参阅附录 D：数据类型中的 [SQL 数据类型](../../../odbc/reference/appendixes/sql-data-types.md) 。 有关特定于驱动程序的 SQL 数据类型的信息，请参阅驱动程序的文档。|  
|COLUMN_SIZE (ODBC 2.0) |3|Integer|服务器支持的此数据类型的最大列大小。 对于数值数据，这是最大精度。 对于字符串数据，这是长度，以字符为长度。 对于日期时间数据类型，这是字符串表示形式的字符长度（以字符为单位）， (假设秒) 部分组件允许的最大精度。 对于列大小不适用的数据类型，将返回 NULL。 对于间隔数据类型，这是时间间隔文本的字符表示形式的字符数， (由间隔前导精度定义;请参阅附录 D：数据类型) 中的 [时间间隔数据类型长度](../../../odbc/reference/appendixes/interval-data-type-length.md) 。<br /><br /> 有关列大小的详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|LITERAL_PREFIX (ODBC 2.0) |4|Varchar|用于作为文本前缀的一个或多个字符;例如，对于字符数据类型，单引号 ( ") ; 对于 binary 数据类型，则为 0x;对于不适用文本前缀的数据类型，将返回 NULL。|  
|LITERAL_SUFFIX (ODBC 2.0) |5|Varchar|用于终止文本的一个或多个字符;例如，单引号 ( ") 字符数据类型;对于不适用文本后缀的数据类型，将返回 NULL。|  
|CREATE_PARAMS (ODBC 2.0) |6|Varchar|关键字的列表（用逗号分隔），对应于在使用 TYPE_NAME 字段中返回的名称时，应用程序可在括号中指定的每个参数。 列表中的关键字可以是以下任一项：长度、精度或小数位数。 它们按语法要求它们的使用顺序显示。 例如，DECIMAL 的 CREATE_PARAMS 为 "精度，小数位数";VARCHAR 的 CREATE_PARAMS 等于 "length"。 如果数据类型定义没有参数，则返回 NULL; 否则，将返回 NULL。例如，整数。<br /><br /> 驱动程序以使用它的国家/地区的语言提供 CREATE_PARAMS 文本。|  
|可为 NULL (ODBC 2.0) |7|Smallint（非 NULL）|数据类型是否接受 NULL 值：<br /><br /> 如果数据类型不接受 NULL 值，则 SQL_NO_NULLS。<br /><br /> 如果数据类型接受 NULL 值，则 SQL_NULLABLE。<br /><br /> 如果不知道列是否接受空值，则 SQL_NULLABLE_UNKNOWN。|  
|CASE_SENSITIVE (ODBC 2.0) |8|Smallint（非 NULL）|字符数据类型在排序规则和比较中是否区分大小写：<br /><br /> 如果数据类型是字符数据类型且区分大小写，则 SQL_TRUE。<br /><br /> 如果数据类型不是字符数据类型或不区分大小写，则 SQL_FALSE。|  
|可搜索 (ODBC 2.0) |9|Smallint（非 NULL）|在 **WHERE** 子句中使用数据类型的方式：<br /><br /> 如果列不能用于 **WHERE** 子句，则 SQL_PRED_NONE。  (这与 ODBC 2 中的 SQL_UNSEARCHABLE 值相同。*x.x.x.x*) <br /><br /> 如果列可以在 **WHERE** 子句中使用，则 SQL_PRED_CHAR，但只能使用 **LIKE** 谓词。  (这与 ODBC 2 中的 SQL_LIKE_ONLY 值相同。*x.x.x.x*) <br /><br /> SQL_PRED_BASIC 是否可以在 **WHERE** 子句中使用 **除 (比较** 、量化比较、 **介于**、 **DISTINCT**、 **in**、 **MATCH** 和 **UNIQUE**) 之外的所有比较运算符。  (这与 ODBC 2 中的 SQL_ALL_EXCEPT_LIKE 值相同。*x.x.x.x*) <br /><br /> 如果可以在 **WHERE** 子句中将列用于任何比较运算符，则 SQL_SEARCHABLE。|  
|UNSIGNED_ATTRIBUTE (ODBC 2.0) |10|Smallint|数据类型是否为无符号数据类型：<br /><br /> 如果数据类型为无符号，则为 SQL_TRUE。<br /><br /> 如果数据类型已签名，则 SQL_FALSE。<br /><br /> 如果特性不适用于数据类型或数据类型不是数值，则返回 NULL。|  
|FIXED_PREC_SCALE (ODBC 2.0) |11|Smallint（非 NULL）|数据类型是否具有预定义的固定精度和小数位数 (，它们是特定于数据源的) ，如 money 数据类型：<br /><br /> SQL_TRUE 是否具有预定义的固定精度和小数位数。<br /><br /> 如果没有预定义的固定精度和小数位数，则 SQL_FALSE。|  
|AUTO_UNIQUE_VALUE (ODBC 2.0) |12|Smallint|数据类型是否为自动递增：<br /><br /> 如果数据类型为自动递增，则 SQL_TRUE。<br /><br /> 如果数据类型不是自动递增，则 SQL_FALSE。<br /><br /> 如果特性不适用于数据类型或数据类型不是数值，则返回 NULL。<br /><br /> 应用程序可以将值插入具有此属性的列中，但通常不能更新列中的值。<br /><br /> 在自动增量列中插入内容时，会在插入时将唯一值插入列。 增量未定义，但特定于数据源。 应用程序不应假定自动增量列从任何特定的点开始或按任何特定值递增。|  
|LOCAL_TYPE_NAME (ODBC 2.0) |13|Varchar|与数据源相关的数据类型名称的本地化版本。 如果是数据源不支持的本地化名称，则返回 NULL。 此名称仅用于显示，例如在对话框中。|  
|MINIMUM_SCALE (ODBC 2.0) |14|Smallint|数据源中数据类型的最小小数位数。 如果数据类型的小数位数是固定的，则 MINIMUM_SCALE 和 MAXIMUM_SCALE 列将同时包含此值。 例如，SQL_TYPE_TIMESTAMP 列的小数部分的小数位数可能是固定的。 当小数位数不适用时，将返回 NULL。 有关详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|MAXIMUM_SCALE (ODBC 2.0) |15|Smallint|数据源中数据类型的最大刻度。 当小数位数不适用时，将返回 NULL。 如果未在数据源上单独定义最大刻度，而是将其定义为与最大精度相同，则此列包含与 COLUMN_SIZE 列相同的值。 有关详细信息，请参阅附录 D：数据类型中的 [列大小、十进制数字、传输八位字节长度和显示大小](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) 。|  
|SQL_DATA_TYPE (ODBC 3.0) |16|Smallint NOT NULL|SQL 数据类型在描述符的 "SQL_DESC_TYPE" 字段中显示的值。 此列与 DATA_TYPE 列相同，间隔和日期时间数据类型除外。<br /><br /> 对于间隔和日期时间数据类型，结果集中的 "SQL_DATA_TYPE" 字段将返回 SQL_INTERVAL 或 SQL_DATETIME，并且 SQL_DATETIME_SUB 字段将返回特定时间间隔或日期时间数据类型的子代码。  (参见 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。 ) |  
|SQL_DATETIME_SUB (ODBC 3.0) |17|Smallint|当 SQL_DATA_TYPE 的值 SQL_DATETIME 或 SQL_INTERVAL 时，此列将包含 DATETIME/INTERVAL 子代码。 对于 datetime 和 interval 以外的数据类型，此字段为 NULL。<br /><br /> 对于间隔或日期时间数据类型，结果集中的 "SQL_DATA_TYPE" 字段将返回 SQL_INTERVAL 或 SQL_DATETIME，并且 SQL_DATETIME_SUB 字段将返回特定时间间隔或日期时间数据类型的子代码。  (参见 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。 ) |  
|NUM_PREC_RADIX (ODBC 3.0) |18|Integer|如果数据类型为近似数值类型，则此列包含值2以指示 COLUMN_SIZE 指定了位数。 对于精确数值类型，此列包含值10，以指示 COLUMN_SIZE 指定了多个十进制数字。 否则，此列为 NULL。|  
|INTERVAL_PRECISION (ODBC 3.0) |19|Smallint|如果数据类型为 interval 数据类型，则此列包含间隔前导精度的值。  (参见附录 D：数据类型中的 [时间间隔数据类型精度](../../../odbc/reference/appendixes/interval-data-type-precision.md) 。 ) 否则，此列为 NULL。|  
  
 属性信息可以应用于数据类型或结果集中的特定列。 **SQLGetTypeInfo** 返回有关与数据类型关联的属性的信息; **SQLColAttribute** 返回与结果集中的列关联的属性的相关信息。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回有关结果集中的列的信息|[SQLColAttribute 函数](../../../odbc/reference/syntax/sqlcolattribute-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|按只进方向提取单个行或数据块|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|返回有关驱动程序或数据源的信息|[SQLGetInfo 函数](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
