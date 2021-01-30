---
description: SQLGetData 函数
title: SQLGetData 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetData
helpviewer_keywords:
- SQLGetData function [ODBC]
ms.assetid: e3c1356a-5db7-4186-85fd-8b74633317e8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 42b81d50a83fef0be7c13604902cfe67b2d2dd89
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201966"
---
# <a name="sqlgetdata-function"></a>SQLGetData 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetData** 检索结果集中单个列的数据，或在 **SQLParamData** 返回 SQL_PARAM_DATA_AVAILABLE 之后为单个参数检索数据。 可以多次调用此方法，以便在部分中检索可变长度数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetData(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   Col_or_Param_Num,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *Col_or_Param_Num*  
 送若要检索列数据，它是要为其返回数据的列的编号。 结果集列按递增的列顺序编号，从1开始。 Bookmark 列的列号为 0;仅当启用书签时才能指定。  
  
 对于检索参数数据，它是参数的序号，从1开始。  
  
 *TargetType*  
 送**TargetValuePtr* 缓冲区的 C 数据类型的类型标识符。 有关有效 C 数据类型和类型标识符的列表，请参阅附录 D：数据类型中的 [C 数据类型](../../../odbc/reference/appendixes/c-data-types.md) 部分。  
  
 如果 SQL_ARD_TYPE *TargetType* ，驱动程序将使用在 ARD 的 SQL_DESC_CONCISE_TYPE 字段中指定的类型标识符。 如果 SQL_APD_TYPE *TargetType* ，则 **SQLGetData** 将使用 **SQLBindParameter** 中指定的相同 C 数据类型。 否则，在 **SQLGetData** 中指定的 c 数据类型将重写 **SQLBindParameter** 中指定的 c 数据类型。 如果 SQL_C_DEFAULT，则驱动程序将基于源的 SQL 数据类型选择默认的 C 数据类型。  
  
 还可以指定扩展的 C 数据类型。 有关详细信息，请参阅 [ODBC 中的 C 数据类型](../../../odbc/reference/develop-app/c-data-types-in-odbc.md)。  
  
 *TargetValuePtr*  
 输出指向要返回数据的缓冲区的指针。  
  
 *TargetValuePtr* 不能为 NULL。  
  
 *BufferLength*  
 送**TargetValuePtr* 缓冲区的长度（以字节为单位）。  
  
 驱动程序使用 *BufferLength* 来避免 \* 在返回长度可变的数据（如字符或二进制数据）时，写入超过 *TargetValuePtr* 缓冲区的末尾。 请注意，在将字符数据返回到 TargetValuePtr 时，驱动程序会对 null 终止字符进行计数 \* 。 *因此， *TargetValuePtr* 必须包含 null 终止字符的空间，否则，驱动程序会截断数据。  
  
 当驱动程序返回固定长度的数据（如整数或日期结构）时，驱动程序将忽略 *BufferLength* ，并假定缓冲区足以容纳数据。 因此，应用程序必须为固定长度的数据分配足够大的缓冲区，否则驱动程序将写入超过缓冲区的末尾。  
  
 当 *BufferLength* 小于0但当 *BufferLength* 为0时， **SQLGetData** 将返回 SQLSTATE HY090 (字符串或缓冲区长度无效) 。  
  
 *StrLen_or_IndPtr*  
 输出一个指针，指向要返回其长度或指示器值的缓冲区。 如果这是 null 指针，则不返回长度或指示器值。 当提取的数据为空时，这将返回错误。  
  
 **SQLGetData** 可以返回长度/指示器缓冲区中的以下值：  
  
-   可供返回的数据的长度  
  
-   SQL_NO_TOTAL  
  
-   SQL_NULL_DATA  
  
 有关详细信息，请参阅本主题中的 [使用长度/指示器值](../../../odbc/reference/develop-app/using-length-and-indicator-values.md) 和 "注释"。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetData** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetData** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|并非指定列 *Col_or_Param_Num* 的所有数据都可以在对函数的单个调用中进行检索。 SQL_NO_TOTAL 或当前调用 **SQLGetData** 之前指定列中剩余数据的长度，将在 \* *StrLen_or_IndPtr* 中返回。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) <br /><br /> 有关对单个列使用多个 **SQLGetData** 调用的详细信息，请参阅 "注释"。|  
|01S07|小数截断|为一列或多列返回的数据被截断。 对于数值数据类型，数值的小数部分被截断。 对于包含时间部分的时间、时间戳和间隔数据类型，时间的小数部分将被截断。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|结果集中的列的数据值无法转换为参数 *TargetType* 指定的 C 数据类型。|  
|07009|描述符索引无效|为参数 *Col_or_Param_Num* 指定的值为0，而 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF。<br /><br /> 为参数 *Col_or_Param_Num* 指定的值大于结果集中的列数。<br /><br /> *Col_or_Param_Num* 值不等于可用的参数的序号。<br /><br /> 已绑定指定列 (DM) 。 此说明不适用于为 **SQLGetInfo** 中的 "SQL_GETDATA_EXTENSIONS" 选项返回 SQL_GD_BOUND 位掩码的驱动程序。<br /><br />  (DM) 指定的列数小于或等于最大绑定列号。 此说明不适用于为 **SQLGetInfo** 中的 "SQL_GETDATA_EXTENSIONS" 选项返回 SQL_GD_ANY_COLUMN 位掩码的驱动程序。<br /><br />  (DM) 应用程序已为当前行调用 **SQLGetData** ;当前调用中指定的列号小于上一调用中指定的列号;而且，驱动程序不会在 **SQLGetInfo** 中返回 SQL_GETDATA_EXTENSIONS 选项的 SQL_GD_ANY_ORDER 位掩码。<br /><br />  (DM) *TargetType* 参数已 SQL_ARD_TYPE，并且 ARD 中的 *Col_or_Param_Num* 描述符记录未通过一致性检查。<br /><br />  (DM) *TargetType* 参数已 SQL_ARD_TYPE，并且 ARD 的 "SQL_DESC_COUNT" 字段中的值小于 " *Col_or_Param_Num* " 参数。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22002|需要指示器变量，但未提供|*StrLen_or_IndPtr* 为 null 指针，并且检索到 null 数据。|  
|22003|数值超出范围|返回 (为列的数值或字符串) 的数值将导致整个 (，而不是小数部分) 要截断的数字。<br /><br /> 有关详细信息，请参阅 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。|  
|22007|Datetime 格式无效|结果集中的字符列已绑定到 C 日期、时间或时间戳结构，并且列中的值为无效的日期、时间或时间戳。 有关详细信息，请参阅 [附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)。|  
|22012|被零除|返回了导致被零除的算术表达式的值。|  
|22015|间隔字段溢出|从精确数值或间隔 SQL 类型赋值到 interval C 类型会导致前导字段的有效位丢失。<br /><br /> 将数据返回到时间间隔 C 类型时，interval C 类型中没有 SQL 类型的值的表示形式。|  
|22018|转换规范的字符值无效|结果集中的字符列已返回到字符 C 缓冲区，列包含的字符在缓冲区的字符集中没有表示形式。<br /><br /> C 类型是精确或近似数字、日期时间或间隔数据类型;列的 SQL 类型是字符数据类型;列中的值不是绑定 C 类型的有效文本。|  
|24000|无效的游标状态| (DM) 调用函数，而无需先调用 **SQLFetch** 或 **SQLFetchScroll** 将游标定位到所需的数据行。<br /><br />  (DM) *StatementHandle* 处于已执行状态，但没有与 *StatementHandle* 关联的结果集。<br /><br /> 已调用 *StatementHandle* 和 **SQLFetch** 或 **SQLFetchScroll** 上打开了游标，但游标位于结果集的开头之前或结果集的结尾之后。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY003|程序类型超出范围| (DM) 参数 *TargetType* 不是有效的数据类型，SQL_C_DEFAULT，SQL_ARD_TYPE (检索列数据时) 或 SQL_APD_TYPE (检索参数数据。<br /><br />  (DM) 参数 *Col_or_Param_Num* 为0，并且参数 *TargetType* 未 SQL_C_BOOKMARK 用于固定长度书签或长度可变的书签的 SQL_C_VARBOOKMARK。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** ，然后在 *StatementHandle* 上再次调用了该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** ，然后在 *StatementHandle* 上再次调用该函数。|  
|HY009|空值指针的使用无效| (DM) 参数 *TargetValuePtr* 为 null 指针。|  
|HY010|函数序列错误| (DM) 指定的 *StatementHandle* 未处于执行状态。 调用函数时，无需先调用 **SQLExecDirect**、 **SQLExecute** 或 catalog 函数。<br /><br />  (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLGetData** 函数时，此异步函数仍在执行。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) *StatementHandle* 处于已执行状态，但没有与 *StatementHandle* 关联的结果集。<br /><br /> 调用 **SQLExeceute**、 **SQLExecDirect** 或 **SQLMoreResults** 的操作返回 SQL_PARAM_DATA_AVAILABLE，但调用 **SQLGetData** ，而不是 **SQLParamData**。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效| (DM) 为参数 *BufferLength* 指定的值小于0。<br /><br /> 为参数 *BufferLength* 指定的值小于4， *Col_or_Param_Num* 参数设置为0，驱动程序为 ODBC *2.x 驱动程序* 。|  
|HY109|游标位置无效|游标被 **SQLSetPos**、 **SQLFetch**、 **SQLFetchScroll** 或 **SQLBulkOperations**) 的位置 (，而该行已被删除或无法提取。<br /><br /> 游标是一个只进游标，行集大小大于1。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持在 **SQLFetchScroll** 中使用具有多个行的 **SQLGetData** 。 此说明不适用于为 **SQLGetInfo** 中的 "SQL_GETDATA_EXTENSIONS" 选项返回 SQL_GD_BLOCK 位掩码的驱动程序。<br /><br /> 驱动程序或数据源不支持通过 *TargetType* 参数和相应列的 SQL 数据类型的组合指定的转换。 仅当该列的 SQL 数据类型映射到特定于驱动程序的 SQL 数据类型时，此错误才适用。<br /><br /> 驱动程序仅支持 ODBC 2.x *，而* 参数 *TargetType* 是以下项之一：<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> 在附录 D：数据类型中， [c 数据类型](../../../odbc/reference/appendixes/c-data-types.md) 中列出的任何时间间隔 c 数据类型。<br /><br /> 驱动程序仅支持3.50 之前的 ODBC 版本，并 SQL_C_GUID 参数 *TargetType* 。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 相对应的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 **SQLGetData** 返回指定列中的数据。 只有从 **SQLFetch**、 **SQLFetchScroll** 或 **SQLExtendedFetch** 的结果集中提取一个或多个行后，才能调用 **SQLGetData** 。 如果可变长度数据太大，无法在对 **SQLGetData** (的单次调用中返回，原因是应用程序) 存在限制， **SQLGetData** 可以在部分中检索它。 可以绑定行中的某些列并为其他列调用 **SQLGetData** ，尽管这会受到某些限制。 有关详细信息，请参阅 [获取长数据](../../../odbc/reference/develop-app/getting-long-data.md)。  
  
 有关将 **SQLGetData** 与流式处理输出参数一起使用的信息，请参阅 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  
  
## <a name="using-sqlgetdata"></a>使用 SQLGetData  
 如果驱动程序不支持对 **SQLGetData** 的扩展，则该函数只能返回其数字大于最后一个绑定列的未绑定列的数据。 此外，在数据行中，每次调用 **SQLGetData** 时 *Col_or_Param_Num* 参数的值必须大于或等于上一次调用中 *Col_or_Param_Num* 的值;也就是说，必须按递增的列号顺序检索数据。 最后，如果不支持任何扩展，并且行集大小大于1，则不能调用 **SQLGetData** 。  
  
 驱动程序可以放宽其中的任何限制。 若要确定驱动程序放宽的限制，应用程序需要使用以下任意 SQL_GETDATA_EXTENSIONS 选项调用 **SQLGetInfo** ：  
  
-   SQL_GD_OUTPUT_PARAMS = **SQLGetData** 可调用以返回输出参数值。 有关详细信息，请参阅 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  
  
-   SQL_GD_ANY_COLUMN。 如果返回此选项，则可以为任何未绑定的列（包括在最后一个绑定列之前的列）调用 **SQLGetData** 。  
  
-   SQL_GD_ANY_ORDER。 如果返回此选项，则可以按任意顺序为未绑定列调用 **SQLGetData** 。  
  
-   SQL_GD_BLOCK。 如果 SQL_GETDATA_EXTENSIONS InfoType 的 **SQLGetInfo** 返回此选项，则当行集大小大于1时，该驱动程序支持对 **SQLGetData** 的调用，且应用程序可以使用 SQL_POSITION 选项调用 **SQLSetPos** ，以便在调用 SQLGetData 之前将游标置于正确的行上 **。**  
  
-   SQL_GD_BOUND。 如果返回此选项，则可以为绑定列和未绑定列调用 **SQLGetData** 。  
  
 这些限制有两个例外，驱动程序可以放宽这些限制。 首先，如果行集大小大于1，则永远不应为只进游标调用 **SQLGetData** 。 其次，如果驱动程序支持书签，则它必须始终支持对列0调用 **SQLGetData** 的能力，即使它不允许应用程序在最后一个绑定列之前为其他列调用 **SQLGetData** 。 当应用程序使用 ODBC 2 时 (*。 x* *驱动程序在* 调用 **SQLFetch** 后， **SQLGetData** 将成功返回书签 *COL_OR_PARAM_NUM* 等于0的书签，因为 **SQLFetch** 由 odbc 1.x 驱动程序管理器映射到 **SQLEXTENDEDFETCH** ，其中 *FetchOrientation* *的 SQLGetData* ，Col_or_Param_Num *为* 0，则 odbc SQL_FETCH_NEXT *1.x 驱动程序* 管理器 **会将 SQLGetStmtOption** 映射 **到 fOption** 。 ) SQL_GET_BOOKMARK  
  
 **SQLGetData** 不能用于检索刚刚插入的行的书签，只需使用 SQL_ADD 选项调用 **SQLBulkOperations** ，因为游标不会定位在该行上。 应用程序可以在使用 SQL_ADD 调用 **SQLBulkOperations** 之前，通过绑定列0来检索此类行的书签，在这种情况下， **SQLBulkOperations** 将返回绑定缓冲区中的书签。 然后，可以通过 SQL_FETCH_BOOKMARK 调用 **SQLFetchScroll** ，以在该行上重新定位光标。  
  
 如果 *TargetType* 参数是 interval 数据类型，则默认间隔的默认间隔 (2) 和默认间隔秒精度 (6) ，分别在 ARD 的 "SQL_DESC_DATETIME_INTERVAL_PRECISION" 和 "SQL_DESC_PRECISION" 字段中进行设置，用于数据。 如果 *TargetType* 参数是 SQL_C_NUMERIC 的数据类型，则用于数据的默认精度 (驱动程序定义) 和默认小数位数 (0) SQL_DESC_PRECISION 和 SQL_DESC_SCALE 字段中设置）用于数据。 如果任何默认的精度或小数位数不合适，则应用程序应通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 显式设置相应的描述符字段。 它可以将 SQL_DESC_CONCISE_TYPE 字段设置为 SQL_C_NUMERIC 并使用 SQL_ARD_TYPE 的 *TargetType* 参数调用 **SQLGetData** ，这将导致使用描述符字段中的精度和小数位数值。  
  
> [!NOTE]
>  在 ODBC 2.x *中，应用* 程序将 *TargetType* 设置为 SQL_C_DATE、SQL_C_TIME 或 SQL_C_TIMESTAMP，以指示 \* *TargetValuePtr* 是日期、时间或时间戳结构。 在 ODBC 3.x *中，应用* 程序将 *TargetType* 设置为 SQL_C_TYPE_DATE、SQL_C_TYPE_TIME 或 SQL_C_TYPE_TIMESTAMP。 根据应用程序和驱动程序版本的需要，驱动程序管理器根据需要进行适当的映射。  
  
## <a name="retrieving-variable-length-data-in-parts"></a>在部分中检索 Variable-Length 数据  
 **SQLGetData** 可用于从包含可变长度数据的列中检索数据，也就是说，列的 SQL 数据类型的标识符 SQL_CHAR、SQL_VARCHAR、SQL_LONGVARCHAR、SQL_WCHAR、SQL_WVARCHAR、SQL_WLONGVARCHAR、SQL_BINARY、SQL_VARBINARY、SQL_LONGVARBINARY 或驱动程序特定的标识符，用于可变长度类型。  
  
 若要在部分中检索列中的数据，应用程序将对同一列连续多次调用 **SQLGetData** 。 每次调用时， **SQLGetData** 将返回数据的下一部分。 应用程序需要重新组装部分，小心从字符数据的中间部分删除 null 终止字符。 如果有更多的数据要返回，或者没有为终止字符分配足够的缓冲区，则 **SQLGetData** 将返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01004 (数据) 截断。 当它返回数据的最后一部分时， **SQLGetData** 将返回 SQL_SUCCESS。 在从列中检索数据的最后一个有效调用上，不能返回 SQL_NO_TOTAL 或零，因为应用程序将无法知道应用程序缓冲区中的数据量是否有效。 如果在此后调用 **SQLGetData** ，则它将返回 SQL_NO_DATA。 有关详细信息，请参阅下一节 "用 SQLGetData 检索数据"。  
  
 可变长度书签可以按 **SQLGetData** 在部件中返回。 与其他数据一样，对 **SQLGetData** 的调用将返回 SQLSTATE 01004 (字符串数据，当有更多数据需要返回时，会将) 和 SQL_SUCCESS_WITH_INFO 截断。 这不同于通过调用 **SQLFetch** 或 **SQLFetchScroll** 来截断可变长度书签的情况，这将返回 SQL_ERROR 和 SQLSTATE 22001 (字符串数据，并在) 右截断。  
  
 **SQLGetData** 不能用于返回部分中的固定长度数据。 如果在一行中对包含固定长度数据的列调用 **SQLGetData** ，则它将为第一个之后的所有调用返回 SQL_NO_DATA。  
  
## <a name="retrieving-streamed-output-parameters"></a>检索流式处理输出参数  
 如果驱动程序支持流式输出参数，则应用程序可以多次调用带有小缓冲区的 **SQLGetData** 来检索大型参数值。 有关流式处理输出参数的详细信息，请参阅 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  
  
## <a name="retrieving-data-with-sqlgetdata"></a>用 SQLGetData 检索数据  
 为了返回指定列的数据， **SQLGetData** 执行以下一系列步骤：  
  
1.  如果已返回列的所有数据，则返回 SQL_NO_DATA。  
  
2.  \*如果数据为 NULL，则将 *StrLen_or_IndPtr* 设置为 SQL_NULL_DATA。 如果数据为 NULL 并且 *StrLen_or_IndPtr* 为 null 指针，则 **SQLGetData** 将返回所需的 SQLSTATE 22002 (指示器变量，但未提供) 。  
  
     如果列的数据不为 NULL，则 **SQLGetData** 将继续执行到步骤3。  
  
3.  如果 SQL_ATTR_MAX_LENGTH 语句特性设置为一个非零值，则如果该列包含字符或二进制数据，并且以前未对列调用 **SQLGetData** ，则数据将被截断为 SQL_ATTR_MAX_LENGTH 字节。  
  
    > [!NOTE]  
    >  SQL_ATTR_MAX_LENGTH 语句特性用于减少网络流量。 它通常由数据源实现，该数据源在网络中返回数据之前将其截断。 驱动程序和数据源不需要支持。 因此，若要保证数据被截断到特定大小，应用程序应分配该大小的缓冲区，并在 *BufferLength* 参数中指定大小。  
  
4.  将数据转换为 *TargetType* 中指定的类型。 将为该数据类型提供默认的精度和小数位数。 如果 SQL_ARD_TYPE *TargetType* ，则使用 ARD 的 SQL_DESC_CONCISE_TYPE 字段中的数据类型。 如果 SQL_ARD_TYPE 了 *TargetType* ，则会根据 SQL_DESC_CONCISE_TYPE 字段中的数据类型，为数据提供 ARD 的 SQL_DESC_DATETIME_INTERVAL_PRECISION、SQL_DESC_PRECISION 和 SQL_DESC_SCALE 字段的精度和小数位数。 如果任何默认的精度或小数位数不合适，则应用程序应通过调用 **SQLSetDescField** 或 **SQLSetDescRec** 显式设置相应的描述符字段。  
  
5.  如果将数据转换为可变长度的数据类型（如字符或二进制）， **SQLGetData** 会检查数据长度是否超过 *BufferLength*。 如果字符数据的长度 (包括 null 终止字符) 超过 *BufferLength*，则 **SQLGetData** 会将数据截断为 *BufferLength* ，使其不超过 null 终止字符的长度。 然后，它将终止数据。 如果二进制数据的长度超过数据缓冲区的长度，则 **SQLGetData** 会将其截断为 *BufferLength* 字节。  
  
     如果提供的数据缓冲区太小，无法容纳 null 终止字符，则 **SQLGetData** 将返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01004。  
  
     **SQLGetData** 从不截断转换为固定长度数据类型的数据;它始终假定 **TargetValuePtr* 的长度为数据类型的大小。  
  
6.  将转换后的 (，可能会在 TargetValuePtr 中截断) 的数据 \* 。 请注意， **SQLGetData** 无法从行返回数据。  
  
7.  将数据的长度置于 \* *StrLen_or_IndPtr* 中。 如果 *StrLen_or_IndPtr* 为 null 指针，则 **SQLGetData** 不返回长度。  
  
    -   对于 "字符" 或 "二进制" 数据，这是转换后的数据长度和由于 *BufferLength* 而进行截断之前的时间长度。 如果驱动程序在转换后无法确定数据的长度，则有时会出现长数据的情况，它将返回 SQL_SUCCESS_WITH_INFO，并将长度设置为 SQL_NO_TOTAL。  (最后一次调用 **SQLGetData** 时，必须始终返回数据的长度，而不是零或 SQL_NO_TOTAL。 ) 如果由于 SQL_ATTR_MAX_LENGTH 语句特性而导致数据被截断，则此特性的值（而不是实际的长度）将放入 \* *StrLen_or_IndPtr* 中。 这是因为该属性设计为在转换前截断服务器上的数据，因此驱动程序无法确定实际长度。 对于同一列连续调用 **SQLGetData** 时，这是在当前调用开始时可用的数据的长度;也就是说，长度随每个后续调用而减少。  
  
    -   对于所有其他数据类型，这是转换后的数据长度;也就是说，它是数据转换到的类型的大小。  
  
8.  如果在转换过程中数据被截断而不丢失重要性 (例如，将实数1.234 转换为整数 1) 时，或者因为 *BufferLength* 太小而 (，例如，将字符串 "abcdef" 放置在4字节缓冲区中) ， **SQLGETDATA** 将返回 SQLSTATE 01004 (数据截断) 和 SQL_SUCCESS_WITH_INFO。 如果由于 SQL_ATTR_MAX_LENGTH 语句特性而导致数据被截断而不会失去重要，则 **SQLGetData** 将返回 SQL_SUCCESS，并且不会返回 SQLSTATE 01004 (数据被截断) 。  
  
 如果在绑定) 列上调用 **SQLGetData** ，则绑定数据缓冲区的内容 (如果 **SQLGetData** 未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则不确定长度/指示器缓冲区。  
  
 对 **SQLGetData** 的后续调用将从请求的最后一列中检索数据;之前的偏移变为无效。 例如，在执行以下序列时：  
  
```cpp  
SQLGetData(icol=n), SQLGetData(icol=m), SQLGetData(icol=n)  
```  
  
 对 SQLGetData (icol = n) 的第二次调用从 n 列的开头检索数据。 由于对列之前调用 **SQLGetData** 的任何偏移量不再有效。  
  
## <a name="descriptors-and-sqlgetdata"></a>描述符和 SQLGetData  
 **SQLGetData** 不直接与任何描述符字段交互。  
  
 如果 SQL_ARD_TYPE *TargetType* ，则使用 ARD 的 SQL_DESC_CONCISE_TYPE 字段中的数据类型。 如果 *TargetType* 为 SQL_ARD_TYPE 或 SQL_C_DEFAULT，则根据 SQL_DESC_SCALE 字段中的数据类型，在 ARD 的 SQL_DESC_DATETIME_INTERVAL_PRECISION、SQL_DESC_PRECISION 和 SQL_DESC_CONCISE_TYPE 字段中提供数据的精度和小数位数。  
  
## <a name="code-example"></a>代码示例  
 在下面的示例中，应用程序执行 **SELECT** 语句以返回按姓名、ID 和电话号码排序的客户 id、名称和电话号码的结果集。 对于每个数据行，它会调用 **SQLFetch** 将游标定位到下一行。 它调用 **SQLGetData** 来检索提取的数据;数据缓冲区和返回的字节数是在对 **SQLGetData** 的调用中指定的。 最后，打印每个雇员的姓名、ID 和电话号码。  
  
```cpp  
#define NAME_LEN 50  
#define PHONE_LEN 50  
  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   sCustID, cbName, cbAge, cbBirthday;  
SQLRETURN    retcode;  
SQLHSTMT     hstmt;  
  
retcode = SQLExecDirect(hstmt,  
   "SELECT CUSTID, NAME, PHONE FROM CUSTOMERS ORDER BY 2, 1, 3",  
   SQL_NTS);  
  
if (retcode == SQL_SUCCESS) {  
   while (TRUE) {  
      retcode = SQLFetch(hstmt);  
      if (retcode == SQL_ERROR || retcode == SQL_SUCCESS_WITH_INFO) {  
         show_error();  
      }  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
         /* Get data for columns 1, 2, and 3 */  
  
         SQLGetData(hstmt, 1, SQL_C_ULONG, &sCustID, 0, &cbCustID);  
         SQLGetData(hstmt, 2, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
         SQLGetData(hstmt, 3, SQL_C_CHAR, szPhone, PHONE_LEN,  
            &cbPhone);  
  
         /* Print the row of data */  
  
         fprintf(out, "%-5d %-*s %*s", sCustID, NAME_LEN-1, szName,   
            PHONE_LEN-1, szPhone);  
      } else {  
         break;  
      }  
   }  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|为结果集中的列分配存储|[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|执行不与块光标位置相关的大容量操作|[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|正在取消语句处理|[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|执行 SQL 语句|[SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|按只进方向提取单个数据行或数据块|[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|在执行时发送参数数据|[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|定位游标，刷新行集中的数据，或更新或删除行集中的数据|[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
