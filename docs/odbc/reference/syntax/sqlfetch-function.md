---
description: SQLFetch 函数
title: SQLFetch 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFetch
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFetch
helpviewer_keywords:
- SQLFetch function [ODBC]
ms.assetid: 6c6611d2-bc6a-4390-87c9-1c5dd9cfe07c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 095a1ca4a67122344926d9ca363dd9f05205c48b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209795"
---
# <a name="sqlfetch-function"></a>SQLFetch 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLFetch** 从结果集中提取下一个数据行集，并返回所有绑定列的数据。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLFetch(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_NO_DATA、SQL_STILL_EXECUTING、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLFetch** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *句柄* *StatementHandle* 调用 [SQLGetDiagRec 函数](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLFetch** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。 如果单个列发生错误，则可以使用 SQL_DIAG_COLUMN_NUMBER 的 *DiagIdentifier* 调用 [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md) ，以确定发生错误的列;可以通过 SQL_DIAG_ROW_NUMBER 的 *DiagIdentifier* 调用和 **SQLGetDiagField** 来确定包含该列的行。  
  
 对于可以返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR (除了 01xxx SQLSTATEs) 的所有这些 SQLSTATEs，如果一个或多个（但不是全部）多行操作行出现错误，则返回 SQL_SUCCESS_WITH_INFO，如果单行操作出现错误，则返回 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01004|字符串数据，右截断|为列返回的字符串或二进制数据导致截断非空白字符或非空的二进制数据。 如果它是一个字符串值，则它将被右截断。|  
|01S01|行中的错误|提取一行或多行时出错。<br /><br />  (如果在 ODBC *3.x 应用程序* 使用 odbc *2.x 驱动程序* 时返回此 SQLSTATE，则可以将其忽略。 ) |  
|01S07|小数截断|为列返回的数据被截断。 对于数值数据类型，数值的小数部分被截断。 对于包含时间部分的时间、时间戳和间隔数据类型，时间的小数部分将被截断。<br /><br />  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|07006|受限制的数据类型属性冲突|结果集中列的数据值无法转换为 **SQLBindCol** 中 *TargetType* 指定的数据类型。<br /><br /> 列0与 SQL_C_BOOKMARK 的数据类型绑定，而 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_VARIABLE。<br /><br /> 列0与 SQL_C_VARBOOKMARK 的数据类型绑定，而 SQL_ATTR_USE_BOOKMARKS 语句特性未设置为 SQL_UB_VARIABLE。|  
|07009|描述符索引无效|驱动程序是不支持 **SQLExtendedFetch** 的 ODBC *2.x 驱动程序*，并且在绑定中为列指定的列号为0。<br /><br /> 已绑定列0，并且 SQL_ATTR_USE_BOOKMARKS 语句特性设置为 SQL_UB_OFF。|  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|22001|字符串数据，右截断|为列返回的可变长度书签已截断。|  
|22002|需要指示器变量，但未提供|将 NULL 数据提取到 *StrLen_or_IndPtr* 由 **SQLBindCol** 设置的列中， (SQL_DESC_INDICATOR_PTR 或由 **SQLSetDescField** 或 **SQLSetDescRec**) 设置为 null 指针。|  
|22003|数值超出范围|对于一个或多个绑定列，将数值返回为数值或字符串将导致整个 (，而不是将被截断的数字) 部分的小数部分。<br /><br /> 有关详细信息，请参阅附录 D：数据类型中的 [将数据从 SQL 转换为 C 数据类型](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) 。|  
|22007|Datetime 格式无效|结果集中的字符列已绑定到日期、时间或时间戳 C 结构，列中的值分别为无效的日期、时间或时间戳。|  
|22012|被零除|返回了算术表达式中的值，从而导致被零除。|  
|22015|间隔字段溢出|从精确数值或间隔 SQL 类型赋值到 interval C 类型会导致前导字段的有效位丢失。<br /><br /> 将数据提取到 interval C 类型时，interval C 类型中没有 SQL 类型的值的表示形式。|  
|22018|转换规范的字符值无效|结果集中的字符列已绑定到字符 C 缓冲区，列包含的字符在缓冲区的字符集中没有表示形式。<br /><br /> C 类型是精确或近似数字、日期时间或间隔数据类型;列的 SQL 类型是字符数据类型;列中的值不是绑定 C 类型的有效文本。|  
|24000|无效的游标状态|*StatementHandle* 处于已执行状态，但没有与 *StatementHandle* 关联的结果集。|  
|40001|序列化失败|执行提取的事务已终止，以防止死锁。|  
|40003|语句完成情况未知|在执行此函数的过程中关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 调用 **SQLFetch** 函数，并在其完成执行之前，对 *StatementHandle* 调用 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用 **SQLFetch** 函数。<br /><br /> 或者，在执行完 **SQLFetch** 函数之前，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLFetch** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 指定的 *StatementHandle* 未处于执行状态。 调用函数时，无需先调用 **SQLExecDirect**、 **SQLExecute** 或 catalog 函数。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br /> 调用 **SQLExtendedFetch** *后，调用了 (* DM) **SQLFetch** ，并调用了 SQL_CLOSE 选项 **SQLFreeStmt** 。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY090|字符串或缓冲区长度无效|SQL_ATTR_USE_BOOKMARK 语句特性设置为 SQL_UB_VARIABLE，列0绑定到一个缓冲区，该缓冲区的长度不等于此结果集的书签的最大长度。  (此长度在 IRD 的 SQL_DESC_OCTET_LENGTH 字段中提供，可以通过调用 **SQLDescribeCol**、 **SQLColAttribute** 或 **SQLGetDescField** 来获取。 ) |  
|HY107|行值超出范围|用 SQL_ATTR_CURSOR_TYPE 语句特性指定的值已 SQL_CURSOR_KEYSET_DRIVEN，但用 SQL_ATTR_KEYSET_SIZE 语句特性指定的值大于0且小于用 SQL_ATTR_ROW_ARRAY_SIZE 语句特性指定的值。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持 **SQLBindCol** 中 *TargetType* 的组合指定的转换和相应列的 SQL 数据类型。|  
|HYT00|超时时间已到|在数据源返回请求的结果集之前，查询超时期限已过期。 超时期限通过 SQLSetStmtAttr 设置，SQL_ATTR_QUERY_TIMEOUT。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 **SQLFetch** 返回结果集中的下一个行集。 它只能在结果集存在时调用：即，在创建结果集的调用之后，在该结果集上的游标结束之前。 如果绑定了任何列，则它会返回这些列中的数据。 如果应用程序已指定一个指向行状态数组或缓冲区的指针（在其中返回提取的行数）， **SQLFetch** 也将返回此信息。 对 **SQLFetch** 的调用可以与对 **SQLFetchScroll** 的调用混合，但不能与对 **SQLExtendedFetch** 的调用混合。 有关详细信息，请参阅 [提取数据行](../../../odbc/reference/develop-app/fetching-a-row-of-data.md)。  
  
 如果 ODBC 1.x 应用程序适用于 ODBC 2.x 驱动程序，则驱动程序管理器会将 **SQLFetch** 调用映射到支持 **SQLExtendedFetch** 的 odbc 2.X *驱动程序的* **SQLExtendedFetch** 。 如果 ODBC 2.x 驱动程序不支持 **SQLExtendedFetch**，则驱动程序管理器会将 **SQLFetch** 调用映射到 ODBC *2.X 驱动程序* 中的 **SQLFetch** ，*该驱动程序* 只能提取一行。  
  
 有关详细信息，请参阅附录 G：驱动程序准则中的 [块游标、可滚动游标和后向兼容性](../../../odbc/reference/appendixes/block-cursors-scrollable-cursors-and-backward-compatibility.md) 。  
  
## <a name="positioning-the-cursor"></a>定位光标  
 创建结果集时，游标将定位在结果集的开始位置之前。 **SQLFetch** 提取下一个行集。 它等效于调用 **SQLFetchScroll** ，并将 *FetchOrientation* 设置为 SQL_FETCH_NEXT。 有关游标的详细信息，请参阅 [游标](../../../odbc/reference/develop-app/cursors.md) 和 [块游标](../../../odbc/reference/develop-app/block-cursors.md)。  
  
 SQL_ATTR_ROW_ARRAY_SIZE 语句特性指定行集中的行数。 如果 **SQLFetch** 提取的行集与结果集的末尾重叠，则 **SQLFetch** 将返回部分行集。 也就是说，如果 S + R-1 大于 L，其中 S 是要提取的行集的起始行，则 R 是行集的大小，L 是结果集中的最后一行，则仅行集的第一 L + 1 行是有效的。 其余行为空，状态为 SQL_ROW_NOROW。  
  
 **SQLFetch** 返回后，当前行是行集的第一行。  
  
 下表中列出的规则根据此部分的第二个表中列出的条件，描述了调用 **SQLFetch** 后的游标定位。  
  
|条件|新行集的第一行|  
|---------------|-----------------------------|  
|开始之前|1|  
| \< CurrRowsetStart = *LastResultRow-RowsetSize*[1]|*CurrRowsetStart*  + *RowsetSize*[2]|  
|*CurrRowsetStart*  > *LastResultRow-RowsetSize*[1]|结束后|  
|结束后|结束后|  
  
 [1] 如果行集大小在提取之间发生更改，则这是与上一次提取一起使用的行集大小。  
  
 [2] 如果行集大小在提取之间发生更改，则这是用于新提取的行集大小。  
  
|表示法|含义|  
|--------------|-------------|  
|开始之前|块游标位于结果集的开头位置之前。 如果新行集的第一行在结果集的开头之前，则 **SQLFetch** 返回 SQL_NO_DATA。|  
|结束后|块游标位于结果集的结尾之后。 如果新行集的第一行在结果集的末尾之后，则 **SQLFetch** 返回 SQL_NO_DATA。|  
|*CurrRowsetStart*|当前行集中第一行的编号。|  
|*LastResultRow*|结果集中的最后一行的行号。|  
|*RowsetSize*|行集大小。|  
  
 例如，假设有一个结果集包含100行，行集大小为5。 下表显示了 **SQLFetch** 为不同起始位置返回的行集和返回代码。  
  
|当前行集|返回代码|新建行集|提取的行数|  
|--------------------|-----------------|----------------|------------------------|  
|开始之前|SQL_SUCCESS|1到5|5|  
|1到5|SQL_SUCCESS|6到10|5|  
|52至56|SQL_SUCCESS|57至61|5|  
|91至95|SQL_SUCCESS|96至100|5|  
|93至97|SQL_SUCCESS|98到100。 行状态数组的第4行和第5行设置为 SQL_ROW_NOROW。|3|  
|96至100|SQL_NO_DATA|无。|0|  
|99至100|SQL_NO_DATA|无。|0|  
|结束后|SQL_NO_DATA|无。|0|  
  
## <a name="returning-data-in-bound-columns"></a>返回绑定列中的数据  
 当 **SQLFetch** 返回每行时，它会将绑定到该列的缓冲区中的每个绑定列的数据放入其中。 如果没有绑定列， **SQLFetch** 将不返回任何数据，而是向前移动块游标。 仍可使用 **SQLGetData** 检索数据。 如果游标是多行游标 (也就是说，SQL_ATTR_ROW_ARRAY_SIZE 大于 1) ，则 SQL_GD_BLOCK 只有当使用 SQL_GETDATA_EXTENSIONS 的 *InfoType* 调用 **SQLGetInfo** 时，才能调用 **SQLGetData** 。  (有关详细信息，请参阅 [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)。 )   
  
 对于行中的每个绑定列， **SQLFetch** 执行以下操作：  
  
1.  设置要 SQL_NULL_DATA 的长度/指示器缓冲区，如果数据为 NULL，则转到下一列。 如果数据为空且未绑定任何长度/指示器缓冲区，则 **SQLFetch** 将返回所需的 SQLSTATE 22002 (指示器变量，但未为行提供) 并继续转到下一行。 有关如何确定长度/指示器缓冲区的地址的信息，请参阅 [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)中的 "缓冲区地址"。  
  
     如果列的数据不为 NULL，则 **SQLFetch** 将继续执行步骤2。  
  
2.  如果 SQL_ATTR_MAX_LENGTH 语句特性设置为一个非零值，并且列包含字符或二进制数据，则数据将被截断为 SQL_ATTR_MAX_LENGTH 字节。  
  
    > [!NOTE]  
    >  SQL_ATTR_MAX_LENGTH 语句特性用于减少网络流量。 它通常由数据源实现，该数据源通过网络返回数据之前将其截断。 驱动程序和数据源不需要支持。 因此，若要保证数据被截断到特定大小，应用程序应分配该大小的缓冲区，并在 **SQLBindCol** 的 *cbValueMax* 参数中指定大小。  
  
3.  将数据转换为 **SQLBindCol** 中由 *TargetType* 指定的类型。  
  
4.  如果将数据转换为可变长度的数据类型（如字符或二进制）， **SQLFetch** 会检查数据长度是否超过数据缓冲区的长度。 如果字符数据的长度 (包括 null 终止字符) 超过数据缓冲区的长度，则 **SQLFetch** 会将数据截断到数据缓冲区的长度，使其长度小于 null 终止字符的长度。 然后，它将终止数据。 如果二进制数据的长度超过数据缓冲区的长度，则 **SQLFetch** 会将其截断为数据缓冲区的长度。 数据缓冲区的长度是通过 **SQLBindCol** 中的 *BufferLength* 指定的。  
  
     **SQLFetch** 从不截断转换为固定长度数据类型的数据;它始终假定数据缓冲区的长度为数据类型的大小。  
  
5.  将转换后的 (和可能被截断的数据缓冲区中的) 数据。 有关如何确定数据缓冲区的地址的信息，请参阅 [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)中的 "缓冲区地址"。  
  
6.  将数据的长度放入长度/指示器缓冲区。 如果指示器指针和长度指针均设置为相同的缓冲区 (调用 **SQLBindCol**) ，则在缓冲区中写入有效数据的长度，并且 SQL_NULL_DATA 写入空数据的缓冲区中。 如果没有绑定长度/指示器缓冲区，则 **SQLFetch** 不返回长度。  
  
    -   对于字符或二进制数据，这是转换后的数据长度，在截断之前，因为数据缓冲区太小。 如果驱动程序在转换后无法确定数据的长度，有时会出现长数据的情况，它会将长度设置为 SQL_NO_TOTAL。 如果由于 SQL_ATTR_MAX_LENGTH 语句特性而导致数据被截断，则此属性的值将放入长度/指示器缓冲区而不是实际长度中。 这是因为该属性设计为在转换前截断服务器上的数据，以便驱动程序无法确定实际长度。  
  
    -   对于所有其他数据类型，这是转换后的数据长度;也就是说，它是数据转换到的类型的大小。  
  
     有关如何确定长度/指示器缓冲区的地址的信息，请参阅 [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)中的 "缓冲区地址"。  
  
7.  如果在转换期间数据被截断 (例如，在转换) 时，实数1.234 被截断为整数1，则 **SQLFetch** 将返回 SQLSTATE 01S07 (分段截断) 和 SQL_SUCCESS_WITH_INFO。 如果由于数据缓冲区的长度太小而导致数据被截断 (例如，字符串 "abcdef" 放在4字节缓冲区中) ，则 **SQLFetch** 将返回 SQLSTATE 01004 (数据截断) 和 SQL_SUCCESS_WITH_INFO。 如果由于 SQL_ATTR_MAX_LENGTH 语句特性而导致数据被截断，则 **SQLFetch** 将返回 SQL_SUCCESS，并且不会返回 SQLSTATE 01S07 (小数截断) 或 SQLSTATE 01004 (数据截断) 的数据。 如果在转换过程中数据被截断 (例如，如果将大于100000的 SQL_INTEGER 值转换为 SQL_C_TINYINT) ，则在行集大小为 1 (时， **SQLFetch** 将返回 SQLSTATE 22003) 数值超出范围 SQL_ERROR; 如果行集大小大于1，则将 () SQL_SUCCESS_WITH_INFO。  
  
 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则绑定数据缓冲区和长度/指示器缓冲区的内容不确定。  
  
## <a name="row-status-array"></a>行状态数组  
 行状态数组用于返回行集中每行的状态。 此数组的地址是用 SQL_ATTR_ROW_STATUS_PTR 语句特性指定的。 数组由应用程序分配，并且必须具有与 SQL_ATTR_ROW_ARRAY_SIZE 语句特性指定的数目相同的元素。 它的值由 **SQLFetch**、 **SQLFetchScroll** 和 **SQLBulkOperations** 或 **SQLSetPos** (设置，除非在游标被 **SQLExtendedFetch**) 定位之后调用了这些值。 如果 SQL_ATTR_ROW_STATUS_PTR 语句特性的值为 null 指针，则这些函数不返回行状态。  
  
 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO，则不定义行状态数组缓冲区的内容。  
  
 行状态数组中将返回以下值。  
  
|行状态数组值|说明|  
|----------------------------|-----------------|  
|SQL_ROW_SUCCESS|该行已成功提取，自从上次从此结果集中提取行后，该行尚未发生更改。|  
|SQL_ROW_SUCCESS_WITH_INFO|该行已成功提取，自从上次从此结果集中提取行后，该行尚未发生更改。 但对于该行，返回了警告。|  
|SQL_ROW_ERROR|提取行时出错。|  
|SQL_ROW_UPDATED [1]、[2] 和 [3]|该行已成功提取，自从上次从此结果集中提取行后已发生更改。 如果再次从此结果集中提取行或通过 **SQLSetPos** 刷新该行，状态将更改为该行的新状态。|  
|SQL_ROW_DELETED [3]|该行自上次从此结果集中提取后已被删除。|  
|SQL_ROW_ADDED [4]|该行由 **SQLBulkOperations** 插入。 如果再次从此结果集中提取行或通过 **SQLSetPos** 刷新该行，则其状态 SQL_ROW_SUCCESS。|  
|SQL_ROW_NOROW|行集与结果集的末尾重叠，并且未返回对应行状态数组的此元素的行。|  
  
 [1] 对于键集、混合和动态游标，如果更新键值，则会将数据行视为已被删除并添加新行。  
  
 [2] 某些驱动程序无法检测到数据的更新，因此无法返回此值。 若要确定驱动程序是否可以检测到制约行的更新，应用程序需要使用 SQL_ROW_UPDATES 选项调用 **SQLGetInfo** 。  
  
 [3]   **SQLFetch** 只能在与对 **SQLFetchScroll** 的调用混合时返回此值。 这是因为 **SQLFetch** 会在结果集中向前移动，并在独占使用时，不重新提取任何行。 由于没有制约的行， **SQLFetch** 不检测对以前提取的行所做的更改。 但是，如果 **SQLFetchScroll** 将游标定位在之前提取的行之前，并使用 **SQLFetch** 提取这些行，则 **SQLFetch** 可以检测对这些行所做的任何更改。  
  
 [4] 仅由 SQLBulkOperations 返回。 不是由 **SQLFetch** 或 **SQLFetchScroll** 设置的。  
  
### <a name="rows-fetched-buffer"></a>提取的行缓冲区  
 "提取的行" 缓冲区用于返回提取的行数，包括因为在提取数据时出错而未返回数据的行数。 换言之，它是行状态数组中的值未 SQL_ROW_NOROW 的行数。 此缓冲区的地址是用 SQL_ATTR_ROWS_FETCHED_PTR 语句特性指定的。 缓冲区由应用程序分配。 它由 **SQLFetch** 和 **SQLFetchScroll** 设置。 如果 SQL_ATTR_ROWS_FETCHED_PTR 语句特性的值为 null 指针，则这些函数不返回提取的行数。 若要确定结果集中的当前行数，应用程序可以使用 SQL_ATTR_ROW_NUMBER 特性调用 **SQLGetStmtAttr** 。  
  
 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO （但返回了 SQL_NO_DATA，则在此情况下，提取的行缓冲区中的值将设置为0），则不会定义所提取的行的内容。  
  
### <a name="error-handling"></a>错误处理  
 错误和警告可应用于单个行或整个函数。 有关诊断记录的详细信息，请参阅 [诊断](../../../odbc/reference/develop-app/diagnostics.md) 和 [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)。  
  
#### <a name="errors-and-warnings-on-the-entire-function"></a>针对整个函数的错误和警告  
 如果错误适用于整个函数（如 SQLSTATE HYT00 (超时过期) 或 SQLSTATE 24000 (无效的游标状态) ， **SQLFetch** 将返回 SQL_ERROR 和适用的 SQLSTATE。 行集缓冲区的内容未定义，并且光标位置保持不变。  
  
 如果警告适用于整个函数，则 **SQLFetch** 将返回 SQL_SUCCESS_WITH_INFO 和适用的 SQLSTATE。 适用于整个函数的警告的状态记录将在应用于单个行的状态记录之前返回。  
  
#### <a name="errors-and-warnings-in-individual-rows"></a>各个行中的错误和警告  
 如果错误 (如 SQLSTATE 22012 (被零除) # A3 或警告 (如 SQLSTATE 01004 (数据被截断) # A7 适用于单个行，则 **SQLFetch** 会执行以下操作：  
  
-   将行状态数组的相应元素设置为 SQL_ROW_ERROR 错误或警告 SQL_ROW_SUCCESS_WITH_INFO。  
  
-   添加零个或多个包含 SQLSTATEs 错误或警告的状态记录。  
  
-   设置状态记录中的行号和列号字段。 如果 **SQLFetch** 无法确定行号或列号，则将其设置为分别 SQL_ROW_NUMBER_UNKNOWN 或 SQL_COLUMN_NUMBER_UNKNOWN。 如果状态记录不适用于特定列，则 **SQLFetch** 会将列号设置为 SQL_NO_COLUMN_NUMBER。  
  
 **SQLFetch** 在提取行集中的所有行之前继续提取行。 除非行集中的每一行都出现错误，否则它将返回 SQL_SUCCESS_WITH_INFO (不包括状态为 SQL_ROW_NOROW) 的行，在这种情况下，它将返回 SQL_ERROR。 具体而言，如果行集的大小为1，并且该行中发生错误，则 **SQLFetch** 将返回 SQL_ERROR。  
  
 **SQLFetch** 按行号顺序返回状态记录。 也就是说，如果有任何) ，它将返回未知行 (的所有状态记录;接下来，它将返回第一行 (的所有状态记录，如果有任何) ，则它将返回第二行的所有状态记录 (如有) 等。 每行的状态记录根据排序状态记录的普通规则进行排序;有关详细信息，请参阅 [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)中的 "状态记录序列"。  
  
### <a name="descriptors-and-sqlfetch"></a>描述符和 SQLFetch  
 以下各节介绍了 **SQLFetch** 如何与描述符交互。  
  
#### <a name="argument-mappings"></a>参数映射  
 该驱动程序不根据 **SQLFetch** 的参数设置任何描述符字段。  
  
#### <a name="other-descriptor-fields"></a>其他描述符字段  
 **SQLFetch** 使用以下描述符字段。  
  
|描述符字段|Desc.|字段|设置为|  
|----------------------|-----------|--------------|-----------------|  
|SQL_DESC_ARRAY_SIZE|ARD|标头的值开始缓存响应|SQL_ATTR_ROW_ARRAY_SIZE 语句特性|  
|SQL_DESC_ARRAY_STATUS_PTR|IRD|标头的值开始缓存响应|SQL_ATTR_ROW_STATUS_PTR 语句特性|  
|SQL_DESC_BIND_OFFSET_PTR|ARD|标头的值开始缓存响应|SQL_ATTR_ROW_BIND_OFFSET_PTR 语句特性|  
|SQL_DESC_BIND_TYPE|ARD|标头的值开始缓存响应|SQL_ATTR_ROW_BIND_TYPE 语句特性|  
|SQL_DESC_COUNT|ARD|标头的值开始缓存响应|**SQLBindCol** 的 *ColumnNumber* 参数|  
|SQL_DESC_DATA_PTR|ARD|records|**SQLBindCol** 的 *TargetValuePtr* 参数|  
|SQL_DESC_INDICATOR_PTR|ARD|records|**SQLBindCol** 中的 *StrLen_or_IndPtr* 参数|  
|SQL_DESC_OCTET_LENGTH|ARD|records|**SQLBindCol** 中的 *BufferLength* 参数|  
|SQL_DESC_OCTET_LENGTH_PTR|ARD|records|**SQLBindCol** 中的 *StrLen_or_IndPtr* 参数|  
|SQL_DESC_ROWS_PROCESSED_PTR|IRD|标头的值开始缓存响应|SQL_ATTR_ROWS_FETCHED_PTR 语句特性|  
|SQL_DESC_TYPE|ARD|records|**SQLBindCol** 中的 *TargetType* 参数|  
  
 所有描述符字段也可以通过 **SQLSetDescField** 设置。  
  
#### <a name="separate-length-and-indicator-buffers"></a>分隔长度和指示器缓冲区  
 应用程序可以绑定单个缓冲区或两个单独的缓冲区，这些缓冲区可用于保存长度和指示器值。 当应用程序调用 **SQLBindCol** 时，驱动程序会将 ARD 的 SQL_DESC_OCTET_LENGTH_PTR 和 SQL_DESC_INDICATOR_PTR 字段设置为同一个地址，该地址将传入 *StrLen_or_IndPtr* 参数。 当应用程序调用 **SQLSetDescField** 或 **SQLSetDescRec** 时，它可以将这两个字段设置为不同地址。  
  
 **SQLFetch** 确定应用程序是否指定了单独的长度和指示器缓冲区。 在这种情况下，当数据不为 NULL 时， **SQLFetch** 会将指示器缓冲区设置为0，并返回长度缓冲区中的长度。 当数据为 NULL 时， **SQLFetch** 会将指示器缓冲区设置为 SQL_NULL_DATA，而不会修改长度缓冲区。  
  
### <a name="code-example"></a>代码示例  
 请参阅 [SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)、 [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)、 [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)和 [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)。  
  
### <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到结果集中的列|[SQLBindCol 函数](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|返回有关结果集中的列的信息|[SQLDescribeCol 函数](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|在语句上关闭游标|[SQLFreeStmt 函数](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|提取部分或全部数据列|[SQLGetData 函数](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
|返回结果集列的数目|[SQLNumResultCols 函数](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|  
|准备要执行的语句|[SQLPrepare 函数](../../../odbc/reference/syntax/sqlprepare-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
