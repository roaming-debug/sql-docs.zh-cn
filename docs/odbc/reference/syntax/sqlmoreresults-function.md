---
description: SQLMoreResults 函数
title: SQLMoreResults 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLMoreResults
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLMoreResults
helpviewer_keywords:
- SQLMoreResults function [ODBC]
ms.assetid: bf169ed5-4d55-412c-b184-12065a726e89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d894cc8cf87a3db794bed763c9bffc596910299a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160801"
---
# <a name="sqlmoreresults-function"></a>SQLMoreResults 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ODBC  
  
 **摘要**  
 **SQLMoreResults** 确定是否在包含 **SELECT**、 **UPDATE**、 **INSERT** 或 **DELETE** 语句的语句上提供更多结果，如果是，则对这些结果初始化处理。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLMoreResults(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_STILL_EXECUTING、SQL_NO_DATA、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_PARAM_DATA_AVAILABLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLMoreResults** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLMoreResults** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01S02|选项值已更改|处理批处理时，语句属性的值已更改。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40003|语句完成情况未知|在执行此函数期间关联的连接失败，无法确定事务的状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *StatementHandle* 启用异步处理。 **SQLMoreResults** 函数在完成执行前调用，在 *StatementHandle* 上调用 **SQLCancel** 或 **SQLCancelHandle** 。 然后，在 *StatementHandle* 上再次调用 **SQLMoreResults** 函数。<br /><br /> **SQLMoreResults** 函数在完成执行之前调用，从多线程应用程序中的另一个线程调用 *StatementHandle* 上的 **SQLCancel** 或 **SQLCancelHandle** 。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLMoreResults** 函数时，此异步函数仍在执行。<br /><br />  (DM) 异步执行的函数 (不是为 *StatementHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 **SELECT** 语句返回结果集。 **UPDATE**、 **INSERT** 和 **DELETE** 语句返回受影响行的计数。 如果这些语句中的任何一个进行了批处理，则将参数数组按参数顺序递增 (按其在批处理) 中出现的顺序进行编号，或在过程中，它们可以返回多个结果集或行计数。 有关语句的批处理和参数数组的信息，请参阅 [SQL 语句的批处理](../../../odbc/reference/develop-app/batches-of-sql-statements.md) 和 [参数值的数组](../../../odbc/reference/develop-app/arrays-of-parameter-values.md)。  
  
 执行批处理后，应用程序定位在第一个结果集上。 应用程序可以在第一个或任何后续的结果集上调用 **SQLBindCol**、 **SQLBulkOperations**、 **SQLFetch**、 **SQLGetData**、 **SQLFetchScroll**、 **SQLSetPos** 和所有元数据函数，就像在只有一个结果集的情况下。 完成第一个结果集后，应用程序将调用 **SQLMoreResults** 以移到下一个结果集。 如果有另一个结果集或计数可用，则 **SQLMoreResults** 将返回 SQL_SUCCESS 并初始化结果集或计数以进行其他处理。 如果在结果集生成语句之间出现任意行计数生成语句，则可以通过调用 **SQLMoreResults** 来逐过程进行。为 **UPDATE**、 **INSERT** 或 **DELETE** 语句调用 **SQLMoreResults** 后，应用程序可以调用 **SQLRowCount**。  
  
 如果当前包含 unfetched 行的结果集，则 **SQLMoreResults** 将丢弃该结果集，并使下一个结果集或计数可用。 如果已处理所有结果， **SQLMoreResults** 将返回 SQL_NO_DATA。 对于某些驱动程序，直到处理完所有结果集和行计数后，输出参数和返回值才可用。 对于此类驱动程序，当 **SQLMoreResults** 返回 SQL_NO_DATA 时，输出参数和返回值将变为可用。  
  
 为之前的结果集建立的所有绑定仍然有效。 如果此结果集的列结构不同，则调用 **SQLFetch** 或 **SQLFetchScroll** 可能导致错误或截断。 若要防止出现这种情况，应用程序必须调用 **SQLBindCol** ，以便在适当的情况下显式重新绑定 (或通过设置描述符字段) 来执行此操作。 或者，应用程序可以使用 SQL_UNBIND *选项* 调用 **SQLFreeStmt** ，以取消绑定所有列缓冲区。  
  
 当应用程序通过调用 **SQLMoreResults** 在批处理中导航时，语句属性（如游标类型、游标并发、键集大小或最大长度）的值可能会更改。 如果发生这种情况， **SQLMoreResults** 将返回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01S02 (选项值已更改) 。  
  
 如果使用 SQL_CLOSE *选项* 调用 **SQLCloseCursor** 或 **SQLFreeStmt** ，则会丢弃作为执行批处理的结果而提供的所有结果集和行计数。 语句句柄返回已分配状态或准备状态。 如果调用 **SQLCancel** ，则在批处理已执行并且语句句柄处于执行、游标定位或异步状态时取消异步执行的函数会导致在取消调用成功的情况下丢弃的批处理生成的所有结果集和行计数。 然后，语句将返回到准备状态或已分配状态。  
  
 如果一批语句或过程使用 **SELECT**、 **UPDATE**、 **INSERT** 和 **DELETE** 语句来混合使用其他 SQL 语句，则这些其他语句不会影响 **SQLMoreResults**。  
  
 有关详细信息，请参阅 [多个结果](../../../odbc/reference/develop-app/multiple-results.md)。  
  
 如果在一批语句中搜索的 update、insert 或 delete 语句不会影响数据源中的任何行，则 **SQLMoreResults** 将返回 SQL_SUCCESS。 这不同于通过 **SQLExecDirect**、 **SQLExecute** 或 **SQLParamData** 执行的搜索的 update、insert 或 delete 语句的情况，如果不影响数据源中的任何行，则返回 SQL_NO_DATA。 如果在调用 **SQLMoreResults** 后，应用程序调用 **SQLRowCount** 来检索行计数，则 **SQLRowCount** 将返回 SQL_NO_DATA。  
  
 有关结果处理函数的有效序列的其他信息，请参阅 [附录 B： ODBC 状态转换表](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md)。  
  
 有关 SQL_PARAM_DATA_AVAILABLE 和流式输出参数的详细信息，请参阅 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)。  
  
## <a name="availability-of-row-counts"></a>行计数的可用性  
 如果批处理包含多个连续行计数生成语句，则可能会将这些行计数汇总成一个行计数。 例如，如果某个批处理包含五个 insert 语句，则某些数据源可以返回五个单独的行计数。 某些其他数据源仅返回一个行计数，表示五个单独行计数之和。  
  
 当某个批处理包含结果集生成和行计数生成语句的组合时，行计数可能会也可能根本不可用。 对于行计数的可用性，驱动程序的行为在通过调用 **SQLGetInfo** 提供的 SQL_BATCH_ROW_COUNT 信息类型中进行枚举。 例如，假设批处理包含 **SELECT**，后跟两个 **INSERT** s，另一个 **选择**。 下面是可能的情况：  
  
-   与这两个 **INSERT** 语句相对应的行计数根本不可用。 第一次调用 **SQLMoreResults** 时，会将您定位到第二个 **SELECT** 语句的结果集。  
  
-   对应于两个 **INSERT** 语句的行计数分别可用。  (调用 **SQLGetInfo** 不会返回 SQL_BATCH_ROW_COUNT 信息类型的 SQL_BRC_ROLLED_UP 位。 ) 第一次调用 **SQLMoreResults** 时，会将您定位到第一次 **插入** 的行计数，第二次调用会将您定位于第二次 **插入** 的行计数。 第三次调用 **SQLMoreResults** 时，会将您定位到第二个 **SELECT** 语句的结果集。  
  
-   对应于两个 **插入** 的行计数汇总到一个可用的单行计数中。  (调用 **SQLGetInfo** 会返回 SQL_BATCH_ROW_COUNT 信息类型的 SQL_BRC_ROLLED_UP 位。 ) 第一次调用 **SQLMoreResults** 时，会将您定位到汇总行计数，而对 **SQLMoreResults** 的第二次调用会将您定位到第二个 **SELECT** 的结果集。  
  
 某些驱动程序使行计数仅适用于显式批处理，不适用于存储过程。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|提取数据块或滚动结果集|[SQLFetchScroll Function（SQLFetchScroll 函数）](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|按只进方向提取单个行或数据块|[SQLFetch 函数](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|提取部分或全部数据列|[SQLGetData 函数](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [使用 SQLGetData 检索输出参数](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
