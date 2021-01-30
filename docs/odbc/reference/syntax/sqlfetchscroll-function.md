---
description: SQLFetchScroll Function（SQLFetchScroll 函数）
title: SQLFetchScroll 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetchScroll
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetchScroll
helpviewer_keywords:
- SQLFetchScroll function [ODBC]
ms.assetid: c0243667-428c-4dda-ae91-3c307616a1ac
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5cfa442ab1ada0b17e06137257b4d17bf37f854c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198536"
---
# <a name="sqlfetchscroll-function"></a>SQLFetchScroll Function（SQLFetchScroll 函数）
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLFetchScroll** 从结果集中提取指定的数据行集，并返回所有绑定列的数据。 可在绝对或相对位置或按书签指定行集。  
  
 使用 ODBC 2.x 驱动程序时，驱动程序管理器将此函数映射到 **SQLExtendedFetch**。 有关详细信息，请参阅 [映射替换函数以实现应用程序的向后兼容性](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLFetchScroll(  
      SQLHSTMT      StatementHandle,  
      SQLSMALLINT   FetchOrientation,  
      SQLLEN        FetchOffset);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *FetchOrientation*  
 送  
  
 提取类型：  
  
 SQL_FETCH_NEXT  
  
 SQL_FETCH_PRIOR  
  
 SQL_FETCH_FIRST  
  
 SQL_FETCH_LAST  
  
 SQL_FETCH_ABSOLUTE  
  
 SQL_FETCH_RELATIVE  
  
 SQL_FETCH_BOOKMARK  
  
 有关详细信息，请参阅 "注释" 部分中的 "定位光标"。  
  
 *FetchOffset*  
 送  
  
 要提取的行数。 此参数的解释取决于 *FetchOrientation* 参数的值。 有关详细信息，请参阅 "注释" 部分中的 "定位光标"。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLFetchScroll** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 HandleType 的 SQL_HANDLE_STMT 和 StatementHandle 的句柄调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLFetchScroll** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。 如果单个列发生错误，则可以使用 SQL_DIAG_COLUMN_NUMBER 的 DiagIdentifier 调用 **SQLGetDiagField** ，以确定发生错误的列;可以通过 SQL_DIAG_ROW_NUMBER 的 DiagIdentifier 调用和 **SQLGetDiagField** 来确定包含该列的行。  
  
 对于可以返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR (除了 01xxx SQLSTATEs) 的所有这些 SQLSTATEs，如果一个或多个（但不是全部）多行操作行出现错误，则返回 SQL_SUCCESS_WITH_INFO，如果单行操作出现错误，则返回 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|为列返回的字符串或二进制数据导致截断非空白字符或非空的二进制数据。 如果它是一个字符串值，则它将被右截断。|  
|01S01|行中的错误|提取一行或多行时出错。<br /><br />  (如果在 ODBC *3.x 应用程序* 使用 odbc *2.x 驱动程序* 时返回此 SQLSTATE，则可以将其忽略。 ) |  
|01S06|尝试在结果集返回第一个行集之前提取|SQL_FETCH_PRIOR FetchOrientation 时，请求的行集与结果集的开头重叠，当前位置超出第一行，当前行的行号小于或等于行集大小。<br /><br /> SQL_FETCH_PRIOR FetchOrientation 时，请求的行集会与结果集的开头重叠，当前位置超出了结果集的末尾，并且行集的大小大于结果集的大小。<br /><br /> SQL_FETCH_RELATIVE 时，请求的行集会与结果集的开头重叠，FetchOffset 为负数，而 FetchOffset 的绝对值小于或等于行集大小。<br /><br /> SQL_FETCH_ABSOLUTE 时，请求的行集会与结果集的开头重叠，FetchOffset 为负数，而 FetchOffset 的绝对值大于结果集的大小，但小于或等于行集大小。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S07|小数截断|为列返回的数据被截断。 对于数值数据类型，数值的小数部分被截断。 对于包含时间部分的时间、时间戳和间隔数据类型，时间的小数部分将被截断。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|结果集中列的数据值无法转换为 **SQLBindCol** 中 *TargetType* 指定的数据类型。<br /><br /> 列0与 SQL_C_BOOKMARK 的数据类型绑定，而 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE。<br /><br /> 列0与 SQL_C_VARBOOKMARK 的数据类型绑定，而 SQL_ATTR_USE_BOOKMARKS 语句特性未设置为 SQL_UB_VARIABLE。|  
|07009|描述符索引无效|驱动程序是不支持 **SQLExtendedFetch** 的 ODBC *2.x 驱动程序*，并且在绑定中为列指定的列号为0。<br /><br /> 已绑定列0，并且 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22001|字符串数据，右截断|为列返回的可变长度书签已截断。|  
|22002|需要指示器变量，但未提供|将 NULL 数据提取到 *StrLen_or_IndPtr* 由 **SQLBindCol** 设置的列中， (SQL_DESC_INDICATOR_PTR 或由 **SQLSetDescField** 或 **SQLSetDescRec**) 设置为 null 指针。|  
|22003|数值超出范围|如果为一个或多个绑定列 (数值或字符串) 返回数值，则会导致整个 (，而不是小数部分) 要截断的数字。<br /><br /> 有关详细信息，请参阅[附录 D：数据类型](../../../odbc/reference/appendixes/appendix-d-data-types.md)中的[将数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md)。|  
|22007|Datetime 格式无效|结果集中的字符列已绑定到日期、时间或时间戳 C 结构，列中的值分别为无效的日期、时间或时间戳。|  
|22012|被零除|返回了算术表达式中的值，从而导致被零除。|  
|22015|间隔字段溢出|从精确数值或间隔 SQL 类型赋值到 interval C 类型会导致前导字段的有效位丢失。<br /><br /> 将数据提取到 interval C 类型时，interval C 类型中没有 SQL 类型的值的表示形式。|  
|22018|转换规范的字符值无效|结果集中的字符列已绑定到字符 C 缓冲区，列包含的字符在缓冲区的字符集中没有表示形式。<br /><br /> C 类型是精确或近似数字、日期时间或间隔数据类型;列的 SQL 类型是字符数据类型;列中的值不是绑定 C 类型的有效文本。|  
|24000|无效的游标状态|*StatementHandle* 处于已执行状态，但没有与 *StatementHandle* 关联的结果集。|  
|40001|序列化失败|执行提取的事务已终止，以防止死锁。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 函数被调用，在完成执行之前，在 *StatementHandle* 上调用了 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用该函数。<br /><br /> 函数被调用，在完成执行之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLFetchScroll** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 指定的 *StatementHandle* 未处于执行状态。 调用函数时，无需先调用 **SQLExecDirect**、 **SQLExecute** 或 catalog 函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br /> 调用 **SQLExtendedFetch** *后，调用了 (* DM) **SQLFetch** ，并调用了 SQL_CLOSE 选项 **SQLFreeStmt** 。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效|SQL_ATTR_USE_BOOKMARK 语句特性设置为 SQL_UB_VARIABLE，列0绑定到一个缓冲区，该缓冲区的长度不等于此结果集的书签的最大长度。  (此长度在 IRD 的 SQL_DESC_OCTET_LENGTH 字段中提供，可以通过调用 **SQLDescribeCol**、 **SQLColAttribute** 或 **SQLGetDescField** 来获取。 ) |  
|HY106|提取类型超出范围|DM) 为参数 FetchOrientation 指定的值无效。<br /><br />  (DM) 参数 FetchOrientation 已 SQL_FETCH_BOOKMARK，并且 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF。<br /><br /> SQL_ATTR_CURSOR_TYPE 语句特性的值 SQL_CURSOR_FORWARD_ONLY，而参数 FetchOrientation 的值不是 SQL_FETCH_NEXT 的。<br /><br /> SQL_ATTR_CURSOR_SCROLLABLE 语句特性的值 SQL_NONSCROLLABLE，而参数 FetchOrientation 的值不是 SQL_FETCH_NEXT 的。|  
|HY107|行值超出范围|用 SQL_ATTR_CURSOR_TYPE 语句特性指定的值已 SQL_CURSOR_KEYSET_DRIVEN，但用 SQL_ATTR_KEYSET_SIZE 语句特性指定的值大于0且小于用 SQL_ATTR_ROW_ARRAY_SIZE 语句特性指定的值。|  
|HY111|书签值无效|参数 FetchOrientation 为 SQL_FETCH_BOOKMARK，SQL_ATTR_FETCH_BOOKMARK_PTR 语句特性中的值指向的书签无效或为 null 指针。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持 **SQLBindCol** 中 *TargetType* 的组合指定的转换和相应列的 SQL 数据类型。|  
|HYT00|超时时间已到|在数据源返回请求的结果集之前，查询超时期限已过期。 超时期限通过 SQLSetStmtAttr 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 **SQLFetchScroll** 从结果集中返回指定的行集。 可以按绝对或相对位置或按书签指定行集。 **SQLFetchScroll** 只能在结果集存在时调用，也就是说，在创建结果集的调用之后，在该结果集上的游标结束之前。 如果绑定了任何列，则它会返回这些列中的数据。 如果应用程序已指定一个指向行状态数组或缓冲区的指针（在其中返回提取的行数）， **SQLFetchScroll** 也将返回此信息。 对 **SQLFetchScroll** 的调用可以与对 **SQLFetch** 的调用混合，但不能与对 **SQLExtendedFetch** 的调用混合。  
  
 有关详细信息，请参阅 [使用块游标](../../../odbc/reference/develop-app/using-block-cursors.md) 和 [使用可滚动游标](../../../odbc/reference/develop-app/using-scrollable-cursors.md)。  
  
## <a name="positioning-the-cursor"></a>定位光标  
 创建结果集时，游标将定位在结果集的开始位置之前。 **SQLFetchScroll** 根据 *FetchOrientation* 和 *FetchOffset* 参数的值定位块游标，如下表所示。 确定新行集开头的确切规则如下一节所示。  
  
|FetchOrientation|含义|  
|----------------------|-------------|  
|SQL_FETCH_NEXT|返回下一个行集。 这等效于调用 **SQLFetch**。<br /><br /> **SQLFetchScroll** 忽略 *FetchOffset* 的值。|  
|SQL_FETCH_PRIOR|返回前面的行集。<br /><br /> **SQLFetchScroll** 忽略 *FetchOffset* 的值。|  
|SQL_FETCH_RELATIVE|从当前行集的开头返回行集 *FetchOffset* 。|  
|SQL_FETCH_ABSOLUTE|返回从行 *FetchOffset* 开始的行集。|  
|SQL_FETCH_FIRST|返回结果集中的第一个行集。<br /><br /> **SQLFetchScroll** 忽略 *FetchOffset* 的值。|  
|SQL_FETCH_LAST|返回结果集中的最后一个完整行集。<br /><br /> **SQLFetchScroll** 忽略 *FetchOffset* 的值。|  
|SQL_FETCH_BOOKMARK|返回 SQL_ATTR_FETCH_BOOKMARK_PTR 语句特性指定的书签中的行集 FetchOffset 行。|  
  
 驱动程序无需支持所有提取方向;应用程序使用 SQL_DYNAMIC_CURSOR_ATTRIBUTES1、SQL_KEYSET_CURSOR_ATTRIBUTES1 或 (SQL_STATIC_CURSOR_ATTRIBUTES1 的信息类型调用 **SQLGetInfo** ，具体取决于游标的类型) 来确定驱动程序所支持的提取方向。 应用程序应查看这些信息类型中的 SQL_CA1_NEXT、SQL_CA1_RELATIVE、SQL_CA1_ABSOLUTE 和 WQL_CA1_BOOKMARK 位掩码。 此外，如果游标是只进的并且 FetchOrientation 不 SQL_FETCH_NEXT，则 **SQLFetchScroll** 将返回 SQLSTATE HY106 (提取类型超出范围) 。  
  
 SQL_ATTR_ROW_ARRAY_SIZE 语句特性指定行集中的行数。 如果 **SQLFetchScroll** 提取的行集与结果集的末尾重叠，则 **SQLFetchScroll** 将返回部分行集。 也就是说，如果 S + R-1 大于 L，其中 S 是要提取的行集的起始行，则 R 是行集的大小，L 是结果集中的最后一行，则仅行集的第一 L + 1 行是有效的。 其余行为空，状态为 SQL_ROW_NOROW。  
  
 **SQLFetchScroll** 返回后，当前行是行集的第一行。  
  
## <a name="cursor-positioning-rules"></a>游标定位规则  
 以下部分介绍 FetchOrientation 的每个值的确切规则。 这些规则使用以下表示法。  
  
|表示法|含义|  
|--------------|-------------|  
|*开始之前*|块游标位于结果集的开头位置之前。 如果新行集的第一行在结果集的开头之前，则 **SQLFetchScroll** 返回 SQL_NO_DATA。|  
|*结束后*|块游标位于结果集的结尾之后。 如果新行集的第一行在结果集的末尾之后，则 **SQLFetchScroll** 返回 SQL_NO_DATA。|  
|*CurrRowsetStart*|当前行集中第一行的编号。|  
|*LastResultRow*|结果集中的最后一行的行号。|  
|*RowsetSize*|行集大小。|  
|*FetchOffset*|*FetchOffset* 参数的值。|  
|*BookmarkRow*|与 SQL_ATTR_FETCH_BOOKMARK_PTR 语句特性指定的书签相对应的行。|  
  
## <a name="sql_fetch_next"></a>SQL_FETCH_NEXT  
 以下规则适用。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|*开始之前*|1|  
|*CurrRowsetStart + RowsetSize*[1] *\< = LastResultRow*|*CurrRowsetStart + RowsetSize*[1]|  
|*CurrRowsetStart + RowsetSize*[1]*> LastResultRow*|*结束后*|  
|*结束后*|*结束后*|  
  
 [1] 如果行集大小自上次调用提取行后发生了更改，则这是与上一调用一起使用的行集大小。  
  
## <a name="sql_fetch_prior"></a>SQL_FETCH_PRIOR  
 以下规则适用。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|*开始之前*|*开始之前*|  
|*CurrRowsetStart = 1*|*开始之前*|  
|*1 < CurrRowsetStart <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*CurrRowsetStart > RowsetSize* <sup>[2]</sup>|*CurrRowsetStart-RowsetSize* <sup>[2]</sup>|  
|*结束后，LastResultRow < RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*结束和 LastResultRow 之后 >= RowsetSize* <sup>[2]</sup>|*LastResultRow-RowsetSize + 1* <sup>[2]</sup>|  
  
 [1]   **SQLFetchScroll** 返回 SQLSTATE 01S06 (尝试在结果集返回第一个行集之前提取) 并 SQL_SUCCESS_WITH_INFO。  
  
 [2] 如果行集大小自上次调用提取行后发生了更改，则这是新的行集大小。  
  
## <a name="sql_fetch_relative"></a>SQL_FETCH_RELATIVE  
 以下规则适用。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|*(在开始和 FetchOffset 之前 > 0) 或 (结束后，FetchOffset < 0)*|*--*<sup>[1]</sup>|  
|*BeforeStart 和 FetchOffset <= 0*|*开始之前*|  
|*CurrRowsetStart = 1，FetchOffset < 0*|*开始之前*|  
|*CurrRowsetStart > 1 和 CurrRowsetStart + FetchOffset < 1，&#124; FetchOffset &#124; > RowsetSize* <sup>[3]</sup>|*开始之前*|  
|*CurrRowsetStart > 1 和 CurrRowsetStart + FetchOffset < 1，&#124; FetchOffset &#124; <= RowsetSize* <sup>[3]</sup>|*1* <sup>[2]</sup>|  
|*1 <= CurrRowsetStart + FetchOffset \< = LastResultRow*|*CurrRowsetStart + FetchOffset*|  
|*CurrRowsetStart + FetchOffset > LastResultRow*|*结束后*|  
|*结束和 FetchOffset 之后 >= 0*|*结束后*|  
  
 [1] ***SQLFetchScroll** _ 返回的行集与将 FetchOrientation 设置为 SQL_FETCH_ABSOLUTE 时调用的行集相同。 有关详细信息，请参阅 "SQL_FETCH_ABSOLUTE" 部分。  
  
 [2] _ *SQLFetchScroll** 返回 SQLSTATE 01S06 (尝试在结果集返回第一个行集之前提取) 和 SQL_SUCCESS_WITH_INFO。  
  
 [3] 如果行集大小自上次调用提取行后发生了更改，则这是新的行集大小。  
  
## <a name="sql_fetch_absolute"></a>SQL_FETCH_ABSOLUTE  
 以下规则适用。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|*FetchOffset < 0 和 &#124; FetchOffset &#124; <= LastResultRow*|*LastResultRow + FetchOffset + 1*|  
|*FetchOffset < 0 和 &#124; FetchOffset &#124; > LASTRESULTROW 和 &#124; FetchOffset &#124; > RowsetSize* <sup>[2]</sup>|*开始之前*|  
|*FetchOffset < 0 和 &#124; FetchOffset &#124; > LASTRESULTROW and &#124; FetchOffset &#124; <= RowsetSize* <sup>[2]</sup>|*1* <sup>[1]</sup>|  
|*FetchOffset = 0*|*开始之前*|  
|*1 <= FetchOffset \< = LastResultRow*|*FetchOffset*|  
|*FetchOffset > LastResultRow*|*结束后*|  
  
 [1]   **SQLFetchScroll** 返回 SQLSTATE 01S06 (尝试在结果集返回第一个行集之前提取) 并 SQL_SUCCESS_WITH_INFO。  
  
 [2] 如果行集大小自上次调用提取行后发生了更改，则这是新的行集大小。  
  
 对动态游标执行的绝对提取无法提供所需的结果，因为动态游标中的行位置不确定。 此类操作等效于首先提取，然后是提取相对;这不是原子操作，这与静态游标上的绝对提取相同。  
  
## <a name="sql_fetch_first"></a>SQL_FETCH_FIRST  
 以下规则适用。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|*任意*|*1*|  
  
## <a name="sql_fetch_last"></a>SQL_FETCH_LAST  
 以下规则适用。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|*RowsetSize* <sup>[1]</sup> <= LastResultRow|*LastResultRow-RowsetSize + 1* <sup>[1]</sup>|  
|*RowsetSize* <sup>[1]</sup> > LastResultRow|*1*|  
  
 [1] 如果行集大小自上次调用提取行后发生了更改，则这是新的行集大小。  
  
## <a name="sql_fetch_bookmark"></a>SQL_FETCH_BOOKMARK  
 以下规则适用。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|*BookmarkRow + FetchOffset < 1*|*开始之前*|  
|*1 <= BookmarkRow + FetchOffset \< = LastResultRow*|*BookmarkRow + FetchOffset*|  
|*BookmarkRow + FetchOffset > LastResultRow*|*结束后*|  
  
 有关书签的信息，请参阅 [书签 (ODBC) ](../../../odbc/reference/develop-app/bookmarks-odbc.md)。  
  
## <a name="effect-of-deleted-added-and-error-rows-on-cursor-movement"></a>游标移动上已删除、添加和错误行的效果  
 静态游标和由键集驱动的游标有时会检测添加到结果集中的行并删除从结果集中删除的行。 通过使用 SQL_STATIC_CURSOR_ATTRIBUTES2 和 SQL_KEYSET_CURSOR_ATTRIBUTES2 选项调用 **SQLGetInfo** ，并查看 SQL_CA2_SENSITIVITY_ADDITIONS、SQL_CA2_SENSITIVITY_DELETIONS 和 SQL_CA2_SENSITIVITY_UPDATES 位屏蔽，应用程序将确定特定驱动程序实现的游标是否执行此操作。 对于可以检测已删除的行并将其删除的驱动程序，以下各段说明了此行为的影响。 对于可以检测已删除的行但不能删除它们的驱动程序，删除对游标移动没有影响，并且以下段落不适用。  
  
 如果游标检测到添加到结果集中的行，或删除从结果集中删除的行，则其似乎仅在提取数据时才检测这些更改。 这包括：调用 **SQLFetchScroll** 时，如果将 FetchOrientation 设置为 SQL_FETCH_RELATIVE 并将 FetchOffset 设置为0，以重新提取同一行集，但不包括在将 SQLSetPos 设置为 SQL_REFRESH 时调用 fOption 的情况。 在后一种情况下，行集缓冲区中的数据将被刷新，而不是制约，并且删除的行不会从结果集中删除。 因此，在将行从当前行集中删除或插入行时，游标不会修改行集缓冲区。 相反，它会在提取之前包含已删除行的任何行集时检测更改，或者现在包含插入的行。  
  
 例如：  
  
```cpp  
// Fetch the next rowset.  
SQLFetchScroll(hstmt, SQL_FETCH_NEXT, 0);  
// Delete third row of the rowset. Does not modify the rowset buffers.  
SQLSetPos(hstmt, 3, SQL_DELETE, SQL_LOCK_NO_CHANGE);  
// The third row has a status of SQL_ROW_DELETED after this call.  
SQLSetPos(hstmt, 3, SQL_REFRESH, SQL_LOCK_NO_CHANGE);  
// Refetch the same rowset. The third row is removed, replaced by what  
// was previously the fourth row.  
SQLFetchScroll(hstmt, SQL_FETCH_RELATIVE, 0);  
```  
  
 当 **SQLFetchScroll** 返回具有相对于当前行集的位置的新行集时，即 FetchOrientation SQL_FETCH_NEXT、SQL_FETCH_PRIOR 或 SQL_FETCH_RELATIVE-在计算新行集的起始位置时，它不包含对当前行集的更改。 但是，如果在当前行集的外部进行了更改，它就会包含这些更改。 此外，当 **SQLFetchScroll** 返回一个新行集，它具有独立于当前行集的位置（即，FetchOrientation 为 SQL_FETCH_FIRST、SQL_FETCH_LAST、SQL_FETCH_ABSOLUTE 或 SQL_FETCH_BOOKMARK）时，即使它们在当前行集中，它也会包含能够检测的所有更改。  
  
 在确定新添加的行是位于当前行集的内部还是外部时，会将部分行集视为结束于最后一个有效行;即，不 SQL_ROW_NOROW 其行状态的最后一行。 例如，假设游标能够检测新添加的行，当前行集是部分行集，应用程序添加新行，并且游标将这些行添加到结果集的末尾。 如果应用程序调用 **SQLFetchScroll** ，并将 FetchOrientation 设置为 SQL_FETCH_NEXT，则 **SQLFetchScroll** 将从第一个新添加的行开始返回行集。  
  
 例如，假设当前行集的行数为21到30，行集大小为10，游标将删除从结果集中删除的行，并且游标检测添加到结果集中的行。 下表显示了在各种情况下 **SQLFetchScroll** 返回的行。  
  
|更改|提取类型|FetchOffset|新行集 [1]|  
|------------|----------------|-----------------|---------------------|  
|删除第21行|NEXT|0|31到40|  
|删除第31行|NEXT|0|32至41|  
|在第21行和第22行之间插入行|NEXT|0|31到40|  
|在行30和31之间插入行|NEXT|0|插入的行，31到39|  
|删除第21行|PRIOR|0|11到20|  
|删除第20行|PRIOR|0|10到19|  
|在第21行和第22行之间插入行|PRIOR|0|11到20|  
|在行20和21之间插入行|PRIOR|0|12到20个，插入的行|  
|删除第21行|RELATIVE|0|22到 31<sup>[2]</sup>|  
|删除第21行|RELATIVE|1|22到31|  
|在第21行和第22行之间插入行|RELATIVE|0|21，插入行，22到29|  
|在第21行和第22行之间插入行|RELATIVE|1|22到31|  
|删除第21行|ABSOLUTE|21|22到 31<sup>[2]</sup>|  
|删除第22行|ABSOLUTE|21|21、23到31|  
|在第21行和第22行之间插入行|ABSOLUTE|22|插入的行，22到29|  
  
 [1] 在插入或删除任何行之前，此列使用行号。  
  
 [2] 在本例中，游标尝试返回第21行开始的行。 由于已删除第21行，因此它返回的第一行是第22行。  
  
 错误行 (也就是说，状态为 SQL_ROW_ERROR) 的行不影响游标移动。 例如，如果当前行集从第11行开始，并 SQL_ROW_ERROR 第11行的状态，则调用 **SQLFetchScroll** ，将 FetchOrientation 设置 SQL_FETCH_RELATIVE 为 ""，并将 FetchOffset 设置为 "5"，则将返回第16行开始的行集，就像第11行的状态为 "已 SQL_SUCCESS 时"。  
  
## <a name="returning-data-in-bound-columns"></a>返回绑定列中的数据  
 **SQLFetchScroll** 以与 **SQLFetch** 相同的方式返回绑定列中的数据。 有关详细信息，请参阅 [SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)中的 "返回绑定列中的数据"。  
  
 如果未绑定列，则 **SQLFetchScroll** 不返回数据，而是将块光标移至指定位置。 是否可以使用 **SQLGetData** 从块游标的未绑定列检索数据取决于驱动程序。 如果对 **SQLGetInfo** 的调用返回 SQL_GETDATA_EXTENSIONS 信息类型的 SQL_GD_BLOCK 位，则支持此功能。  
  
## <a name="buffer-addresses"></a>缓冲区地址  
 **SQLFetchScroll** 使用相同的公式来确定数据和长度/指示器缓冲区为 **SQLFetch** 的地址。 有关详细信息，请参阅 [SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)中的 "缓冲区地址"。  
  
## <a name="row-status-array"></a>行状态数组  
 **SQLFetchScroll** 以与 SQLFetch 相同的方式设置行状态数组中的值。 有关详细信息，请参阅 [SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)中的 "行状态数组"。  
  
## <a name="rows-fetched-buffer"></a>提取的行缓冲区  
 **SQLFetchScroll** 以与 **SQLFetch** 相同的方式返回在提取的行中提取的行数。 有关详细信息，请参阅 [SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)中的 "提取的行数"。  
  
## <a name="error-handling"></a>错误处理  
 当应用程序在 ODBC 1.x 驱动程序中调用 **SQLFetchScroll** 时，驱动程序管理器将在驱动程序中调用 **SQLFetchScroll** 。 当应用程序在 ODBC 2.x 驱动程序中调用 **SQLFetchScroll** 时，驱动程序管理器将在驱动程序中调用 SQLExtendedFetch。 由于 **SQLFetchScroll** 和 SQLExtendedFetch 以略有不同的方式处理错误，因此当应用程序在 odbc 2.X 和 odbc 2.x 驱动程序中调用 **SQLFetchScroll** 时，会看到略微不同的错误行为。  
  
 **SQLFetchScroll** 返回错误和警告的方式与 **SQLFetch** 相同;有关详细信息，请参阅 **SQLFetch** 中的 "错误处理"。 **SQLExtendedFetch** 返回错误的方式与 **SQLFetch** 相同，但以下情况除外：  
  
 当发生适用于行集中特定行的警告时，SQLExtendedFetch 会将行状态数组中的相应条目设置为 SQL_ROW_SUCCESS，而不是 SQL_ROW_SUCCESS_WITH_INFO。  
  
 如果行集中的每一行发生错误，则 SQLExtendedFetch 将返回 SQL_SUCCESS_WITH_INFO，而不是 SQL_ERROR。  
  
 在应用于单个行的每一组状态记录中，SQLExtendedFetch 返回的第一个状态记录必须在) 行中包含 SQLSTATE 01S01 (错误; **SQLFetchScroll** 不会返回此 SQLSTATE。 如果 SQLExtendedFetch 无法返回其他 SQLSTATEs，它仍必须返回此 SQLSTATE。  
  
## <a name="sqlfetchscroll-and-optimistic-concurrency"></a>SQLFetchScroll 和开放式并发  
 如果游标使用乐观并发，即，SQL_ATTR_CONCURRENCY 语句特性的值为 SQL_CONCUR_VALUES 或 SQL_CONCUR_ROWVER **SQLFetchScroll** 将更新数据源用来检测行是否已更改的开放式并发值。 只要 **SQLFetchScroll** 提取新行集，就会发生这种情况，包括 refetches 当前行集。  (将其设置为 SQL_FETCH_RELATIVE 并将 FetchOffset 设置为0，则调用此方法。 )   
  
## <a name="sqlfetchscroll-and-odbc-2x-drivers"></a>SQLFetchScroll 和 ODBC 2.x 驱动程序  
 当应用程序在 ODBC 2.x 驱动程序中调用 **SQLFetchScroll** 时，驱动程序管理器会将此调用映射到 **SQLExtendedFetch**。 它为 **SQLExtendedFetch** 的参数传递以下值。  
  
|SQLExtendedFetch 参数|值|  
|-------------------------------|-----------|  
|StatementHandle|**SQLFetchScroll** 中的 StatementHandle。|  
|FetchOrientation|**SQLFetchScroll** 中的 FetchOrientation。|  
|FetchOffset|如果未 SQL_FETCH_BOOKMARK FetchOrientation，则使用 **SQLFetchScroll** 中 FetchOffset 参数的值。<br /><br /> 如果 SQL_FETCH_BOOKMARK FetchOrientation，则使用存储在由 SQL_ATTR_FETCH_BOOKMARK_PTR 语句特性指定的地址的值。|  
|RowCountPtr|SQL_ATTR_ROWS_FETCHED_PTR 语句特性指定的地址。|  
|RowStatusArray|SQL_ATTR_ROW_STATUS_PTR 语句特性指定的地址。|  
  
 有关详细信息，请参阅附录 G：驱动程序准则中的 [块游标、可滚动游标和后向兼容性](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) 。  
  
## <a name="descriptors-and-sqlfetchscroll"></a>描述符和 SQLFetchScroll  
 **SQLFetchScroll** 与描述符的交互方式与 **SQLFetch** 相同。 有关详细信息，请参阅 [SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)中的 "描述符和 SQLFetchScroll" 部分。  
  
## <a name="code-example"></a>代码示例  
 请参阅按 [列绑定](../../../odbc/reference/develop-app/column-wise-binding.md)、按 [行绑定](../../../odbc/reference/develop-app/row-wise-binding.md)、 [定位更新和删除语句](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md)，并 [通过 SQLSetPos 更新行集中的行](../../../odbc/reference/develop-app/updating-rows-in-the-rowset-with-sqlsetpos.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|执行 bulk insert、update 或 delete 操作|[SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回有关结果集中的列的信息|[SQLDescribeCol 函数](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|按只进方向提取单个行或数据块|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|在语句上关闭游标|[SQLFreeStmt 函数](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|返回结果集列的数目|[SQLNumResultCols 函数](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|定位游标，刷新行集中的数据，或更新或删除结果集中的数据|[SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
|设置语句特性|[SQLSetStmtAttr 函数](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
