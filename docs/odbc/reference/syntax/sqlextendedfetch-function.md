---
description: SQLExtendedFetch 函数
title: SQLExtendedFetch 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLExtendedFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLExtendedFetch
helpviewer_keywords:
- SQLExtendedFetch function [ODBC]
ms.assetid: 940b5cf7-581c-4ede-8533-c67d5e9ef488
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 403b8d426049ba6a333920d472c2e69ed279d761
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179864"
---
# <a name="sqlextendedfetch-function"></a>SQLExtendedFetch 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性：已弃用  
  
 **摘要**  
 **SQLExtendedFetch** 从结果集中提取指定的数据行集，并返回所有绑定列的数据。 可在绝对或相对位置或按书签指定行集。  
  
> [!NOTE]
>  在 ODBC 3.x *中，* **SQLExtendedFetch** 已被 **SQLFetchScroll** 取代。 ODBC 2.x *应用程序不* 应调用 **SQLExtendedFetch**;应改为调用 **SQLFetchScroll**。 当使用 ODBC *2.x 驱动程序* 时，驱动程序管理器会将 **SQLFetchScroll** 映射到 **SQLExtendedFetch** 。 如果 ODBC *2.x 驱动程序* 要使用 odbc 1.x 应用程序调用它，则应该支持 **SQLExtendedFetch** 。 有关详细信息，请参阅附录 G：驱动程序准则中的 "注释" 和 "注释" 和 " [阻止游标" 和向后兼容性](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) ，以实现向后兼容性。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLExtendedFetch(  
      SQLHSTMT         StatementHandle,  
      SQLUSMALLINT     FetchOrientation,  
      SQLLEN           FetchOffset,  
      SQLULEN *        RowCountPtr,  
      SQLUSMALLINT *   RowStatusArray);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *FetchOrientation*  
 送提取的类型。 这与 **SQLFetchScroll** 中的 *FetchOrientation* 相同。  
  
 *FetchOffset*  
 送要提取的行数。 这与 **SQLFetchScroll** 中的 *FetchOffset* 相同，但有一个例外。 SQL_FETCH_BOOKMARK *FetchOrientation* 时， *FetchOffset* 为固定长度书签，而不是书签偏移量。 换言之， **SQLExtendedFetch** 从此参数而不是 SQL_ATTR_FETCH_BOOKMARK_PTR 语句属性中检索书签。 它不支持可变长度书签，并且不支持从书签 (0) 以外的偏移量提取行集。  
  
 *RowCountPtr*  
 输出指向缓冲区的指针，该缓冲区用于返回实际提取的行数。 此缓冲区的使用方式与 SQL_ATTR_ROWS_FETCHED_PTR 语句特性指定的缓冲区相同。 此缓冲区仅由 **SQLExtendedFetch** 使用。 它不是由 **SQLFetch** 或 **SQLFetchScroll** 使用。  
  
 *RowStatusArray*  
 输出指向数组的指针，将在该数组中返回每行的状态。 此数组的使用方式与 SQL_ATTR_ROW_STATUS_PTR 语句特性指定的数组相同。  
  
 但是，此数组的地址未存储在 IRD 的 "SQL_DESC_STATUS_ARRAY_PTR" 字段中。 此外，此数组仅由 **SQLExtendedFetch** 和 **SQLBulkOperations** 使用，其 *操作* 在 **SQLExtendedFetch** 后调用 SQL_ADD 或 **SQLSetPos** 。 它不由 **SQLFetch** 或 **SQLFetchScroll** 使用，并且在 **SQLFetch** 或 **SQLFetchScroll** 后调用时， **SQLBulkOperations** 或 **SQLSetPos** 不会使用此名称。 如果在调用 SQL_ADD 的 *操作* 之前调用了，**则在调用** 任何 fetch 函数之前，也不会使用此方法。 换句话说，它仅用于语句状态 S7。 它不用于语句状态 S5 或 S6。 有关详细信息，请参阅附录 B： ODBC 状态转换表中的 [语句转换](../../../odbc/reference/appendixes/statement-transitions.md) 。  
  
 应用程序应提供 *RowStatusArray* 参数中的有效指针;如果不是，则 **SQLExtendedFetch** 的行为和对 **SQLBulkOperations** 或 **SQLSetPos** 的调用的 **行为不确定** 。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLExtendedFetch** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过调用 **SQLError** 获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLExtendedFetch** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。 如果单个列发生错误，则可以使用 SQL_DIAG_COLUMN_NUMBER 的 *DiagIdentifier* 调用 **SQLGetDiagField** ，以确定发生错误的列;可以通过 SQL_DIAG_ROW_NUMBER 的 *DiagIdentifier* 调用和 **SQLGetDiagField** 来确定包含该列的行。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|为列返回的字符串或二进制数据导致截断非空白字符或非空的二进制数据。 如果它是一个字符串值，则它将被右截断。 如果该值为数值，则该数字的小数部分将被截断。   (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S01|行中的错误|提取一行或多行时出错。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S06|尝试在结果集返回第一个行集之前提取|当当前位置超出第一行时，请求的行集与结果集的开头重叠，并且 *FetchOrientation* 为 SQL_PRIOR 或 SQL_RELATIVE *FetchOrientation* 的值为负 *FetchOffset* ，其绝对值小于或等于当前 SQL_ROWSET_SIZE。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S07|小数截断|为列返回的数据被截断。 对于数值数据类型，数值的小数部分被截断。 对于包含时间部分的时间、时间戳和间隔数据类型，时间的小数部分将被截断。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|数据值无法转换为 **SQLBindCol** 中 *TargetType* 指定的 C 数据类型。|  
|07009|描述符索引无效|列0与 **SQLBindCol** 绑定，而 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22002|需要指示器变量，但未提供|将 NULL 数据提取到 **SQLBindCol** 设置为 null 指针的 *StrLen_or_IndPtr* 列中。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|22003|数值超出范围|如果一个或多个列的数值或字符串)  (返回数值，则会导致整个 (，而不是小数部分) 要截断的数字。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) <br /><br /> 有关详细信息，请参阅附录 D：数据类型中 [间隔和数值数据类型的准则](../../../odbc/reference/appendixes/guidelines-for-interval-and-numeric-data-types.md) 。|  
|22007|Datetime 格式无效|结果集中的字符列已绑定到日期、时间或时间戳 C 结构，列中的值分别为无效的日期、时间或时间戳。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|22012|被零除|返回了算术表达式中的值，从而导致被零除。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|22015|间隔字段溢出|从精确数值或间隔 SQL 类型赋值到 interval C 类型会导致前导字段的有效位丢失。<br /><br /> 将数据提取到 interval C 类型时，interval C 类型中没有 SQL 类型的值的表示形式。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|22018|转换规范的字符值无效|C 类型是精确或近似数字、日期时间或间隔数据类型;列的 SQL 类型是字符数据类型;列中的值不是绑定 C 类型的有效文本。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|24000|无效的游标状态|*StatementHandle* 处于已执行状态，但没有与 *StatementHandle* 关联的结果集。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中 **SQLError** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** ，然后在 *StatementHandle* 上再次调用了该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLExtendedFetch** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 指定的 *StatementHandle* 未处于执行状态。 调用函数时，无需先调用 **SQLExecDirect**、 **SQLExecute** 或 catalog 函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 在调用 **SQLFetch** 或 **SQLFetchScroll** 后，调用 *StatementHandle* ，并且在用 SQL_CLOSE 选项调用 **SQLFreeStmt** 之前调用了 **SQLExtendedFetch** 。<br /><br /> 在调用 **SQLFetch**、 **SQLFetchScroll** 或 **SQLExtendedFetch** 之前，已为语句调用 (DM) **SQLBulkOperations** **，然后调用 SQLExtendedFetch，** 并使用 SQL_CLOSE 选项调用 **SQLFreeStmt** 。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY106|提取类型超出范围| (DM) 为参数 *FetchOrientation* 指定的值无效。  (参阅 "注释"。) <br /><br /> 参数 *FetchOrientation* 为 SQL_FETCH_BOOKMARK，SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF。<br /><br /> SQL_CURSOR_TYPE 语句选项的值 SQL_CURSOR_FORWARD_ONLY，而参数 *FetchOrientation* 的值不是 SQL_FETCH_NEXT 的。<br /><br /> 参数 *FetchOrientation* 已 SQL_FETCH_RESUME。|  
|HY107|行值超出范围|用 SQL_CURSOR_TYPE 语句选项指定的值已 SQL_CURSOR_KEYSET_DRIVEN，但用 SQL_KEYSET_SIZE 语句特性指定的值大于0且小于用 SQL_ROWSET_SIZE 语句特性指定的值。|  
|HY111|书签值无效|参数 *FetchOrientation* 为 SQL_FETCH_BOOKMARK， *FetchOffset* 参数中指定的书签无效。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持指定的提取类型。<br /><br /> 驱动程序或数据源不支持 **SQLBindCol** 中 *TargetType* 的组合指定的转换和相应列的 SQL 数据类型。 仅当该列的 SQL 数据类型映射到特定于驱动程序的 SQL 数据类型时，此错误才适用。|  
|HYT00|超时时间已到|在数据源返回结果集之前，查询超时期限已过期。 超时期限通过 **SQLSetStmtOption** 设置，SQL_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 **SQLExtendedFetch** 的行为与 **SQLFetchScroll** 的行为相同，但有以下例外：  
  
-   **SQLExtendedFetch** 和 **SQLFetchScroll** 使用不同方法返回提取的行数。 **SQLExtendedFetch** 返回在 *\* RowCountPtr* 中提取的行数;**SQLFetchScroll** 返回 SQL_ATTR_ROWS_FETCHED_PTR 所指向的缓冲区直接提取的行数。 有关详细信息，请参阅 *RowCountPtr* 参数。  
  
-   **SQLExtendedFetch** 和 **SQLFetchScroll** 返回不同数组中每行的状态。 有关详细信息，请参阅 *RowStatusArray* 参数。  
  
-   **SQLExtendedFetch** 和 **SQLFetchScroll** 在 SQL_FETCH_BOOKMARK *FetchOrientation* 时，使用不同的方法来检索书签。 **SQLExtendedFetch** 不支持长度可变的书签，也不支持从书签以外的偏移量提取行集。 有关详细信息，请参阅 *FetchOffset* 参数。  
  
-   **SQLExtendedFetch** 和 **SQLFetchScroll** 使用不同的行集大小。 **SQLExtendedFetch** 使用 SQL_ROWSET_SIZE 语句特性的值，而 **SQLFetchScroll** 使用 SQL_ATTR_ROW_ARRAY_SIZE 语句特性的值。  
  
-   **SQLExtendedFetch** 的错误处理语义略有不同， **SQLFetchScroll**。 有关详细信息，请参阅 [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)的 "评论" 部分中的 "错误处理"。  
  
-   **SQLExtendedFetch** 不支持 SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性)  (绑定偏移量。  
  
-   对 **SQLExtendedFetch** 的调用不能与 **SQLFetch** 或 **SQLFetchScroll** 的调用混合，如果在调用任何 fetch 函数之前调用了 **SQLBulkOperations** ，则在关闭并重新打开该游标之前，将无法调用 **SQLExtendedFetch** 。 也就是说，只能在语句状态 S7 中调用 **SQLExtendedFetch** 。 有关详细信息，请参阅附录 B： ODBC 状态转换表中的 [语句转换](../../../odbc/reference/appendixes/statement-transitions.md) 。  
  
 当应用程序在使用 ODBC *2.x 驱动程序* 的情况下调用 **SQLFetchScroll** 时，驱动程序管理器会将此调用映射到 **SQLExtendedFetch**。 有关详细信息，请参阅 [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)中的 "SQLFETCHSCROLL 和 ODBC *2.x 驱动程序*"。  
  
 在 ODBC 2.x *中，调用* 了 **SQLExtendedFetch** 来提取多行，并调用 **SQLFetch** 来提取单个行。 另一方面 *，在* ODBC 3.x 中，可以调用 **SQLFetch** 来提取多个行。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|执行 bulk insert、update 或 delete 操作|[SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回有关结果集中的列的信息|[SQLDescribeCol 函数](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|返回结果集列的数目|[SQLNumResultCols 函数](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|定位游标，刷新行集中的数据，或更新或删除结果集中的数据|[SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
