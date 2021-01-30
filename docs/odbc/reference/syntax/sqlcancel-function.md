---
description: SQLCancel 函数
title: SQLCancel 函数 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCancel
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLCancel
helpviewer_keywords:
- SQLCancel function [ODBC]
ms.assetid: ac0b5972-627f-4440-8c5a-0e8da728726d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c1d630af340bf676deda8c18f415b85aba15403c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185806"
---
# <a name="sqlcancel-function"></a>SQLCancel 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLCancel** 取消对语句的处理。  
  
 若要取消对某个连接或语句的处理，请使用 [SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLCancel(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLCancel** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLCancel** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 参数 *\* MessageText* 缓冲区中的 [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLCancel** 函数时，此异步函数仍在执行。<br /><br />  (DM) 取消操作失败，因为正在对与 *StatementHandle* 关联的连接句柄执行异步操作。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY018|服务器拒绝取消请求|服务器拒绝了取消请求。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 **SQLCancel** 可以对语句取消以下类型的处理：  
  
-   在语句上异步运行的函数。  
  
-   语句上需要数据的函数。  
  
-   在另一线程上的语句上运行的函数。  
  
 在 ODBC 2 中。*x*，如果应用程序在对语句进行处理时调用 **SQLCancel** ，则 **SQLCancel** 与 SQL_CLOSE 选项的 **SQLFreeStmt** 具有相同的效果;此行为仅用于完整性定义，应用程序应调用 **SQLFreeStmt** 或 **SQLCloseCursor** 以关闭游标。  
  
 如果调用 **SQLCancel** 来取消在语句中异步运行的函数或需要数据的语句的函数，则会清除该函数所发布的诊断记录，并且 **SQLCancel** 会发布其自己的诊断记录;但是，当调用 **SQLCancel** 来取消在另一个线程上的语句上运行的函数时，它不会清除正在取消的函数的诊断记录，也不会发布其自己的诊断记录。  
  
## <a name="canceling-asynchronous-processing"></a>取消异步处理  
 在应用程序以异步方式调用函数后，它将重复调用函数以确定其是否已完成处理。 如果该函数仍在处理，则它将返回 SQL_STILL_EXECUTING。 如果函数已完成处理，则将返回不同的代码。  
  
 调用返回 SQL_STILL_EXECUTING 的函数后，应用程序可以调用 **SQLCancel** 来取消函数。 如果取消请求成功，则驱动程序将返回 SQL_SUCCESS。 此消息不表示该函数实际上已被取消;它指示已处理取消请求。 当或如果实际取消函数，则是依赖于驱动程序和依赖于数据源的。 在不 SQL_STILL_EXECUTING 返回代码之前，应用程序必须继续调用原始函数。 如果该函数已成功取消，则返回代码为 SQL_ERROR (操作取消) ，则为 SQLSTATE HY008。 如果该函数完成了其常规处理，则返回代码为 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO 如果该函数成功，则为; 如果该函数成功，则为; 如果该函数失败，则为 SQL_ERROR (操作) 取消的 SQLSTATE。  
  
> [!NOTE]  
>  在 ODBC 3.5 中，对语句进行处理时，对 **SQLCancel** 的调用不会被视为带有 SQL_CLOSE 选项的 **SQLFreeStmt** ，但它根本不起作用。 若要关闭游标，应用程序应调用 **SQLCloseCursor**，而不是 **SQLCancel**。  
  
 有关异步处理的详细信息，请参阅 [异步执行](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)。  
  
## <a name="canceling-functions-that-need-data"></a>取消需要数据的函数  
 在 **SQLExecute** 或 **SQLExecDirect** 返回 SQL_NEED_DATA，并在为所有执行时参数发送数据之前，应用程序可以调用 **SQLCancel** 来取消语句执行。 在取消语句后，应用程序可以再次调用 **SQLExecute** 或 **SQLExecDirect** 。 有关详细信息，请参阅 [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)。  
  
 在 **SQLBulkOperations** 或 **SQLSetPos** 返回 SQL_NEED_DATA，并在为所有执行时数据列发送数据之前，应用程序可以调用 **SQLCancel** 来取消操作。 操作取消后，应用程序可以再次调用 **SQLBulkOperations** 或 **SQLSetPos** ;取消不会影响游标状态或当前游标位置。 有关详细信息，请参阅 [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md) 或 [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)。  
  
## <a name="canceling-functions-executing-on-another-thread"></a>取消在另一个线程上执行的函数  
 在多线程应用程序中，应用程序可以取消在另一个线程上运行的函数。 若要取消函数，应用程序将调用 **SQLCancel** ，该语句句柄与目标函数使用的语句句柄相同，但在另一个线程上调用。 如何取消该函数取决于驱动程序和操作系统。 在取消异步运行的函数时， **SQLCancel** 的返回代码仅指示驱动程序是否已成功处理请求。 只能返回 SQL_SUCCESS 或 SQL_ERROR;不返回任何诊断信息。 如果原始函数被取消，则将返回 SQL_ERROR 和 SQLSTATE HY008 (操作取消) 。  
  
 如果在另一个线程上调用 **SQLCancel** 来取消语句执行时，将执行 SQL 语句，则执行可能会成功并返回 SQL_SUCCESS 同时取消也成功。 在这种情况下，驱动程序管理器假设语句执行操作打开的游标被取消关闭，因此应用程序将不能使用游标。  
  
 有关线程处理的详细信息，请参阅 [多线程处理](../../../odbc/reference/develop-app/multithreading.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|将缓冲区绑定到参数|[SQLBindParameter 函数](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|执行大容量插入或更新操作|[SQLBulkOperations 函数](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|  
|除了 **SQLCancel** 的功能外，还取消在连接句柄上异步运行的函数。|[SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|释放语句句柄|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|获取诊断标头的诊断记录或字段的字段|[SQLGetDiagField 函数](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
|获取诊断数据结构的多个字段|[SQLGetDiagRec 函数](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|  
|返回要为其发送数据的下一个参数|[SQLParamData 函数](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|在执行时发送参数数据|[SQLPutData 函数](../../../odbc/reference/syntax/sqlputdata-function.md)|  
|将光标置于行集中，刷新行集中的数据，或更新或删除结果集中的数据|[SQLSetPos 函数](../../../odbc/reference/syntax/sqlsetpos-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
