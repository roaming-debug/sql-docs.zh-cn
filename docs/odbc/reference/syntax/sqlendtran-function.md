---
description: SQLEndTran 函数
title: SQLEndTran 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLEndTran
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLEndTran
helpviewer_keywords:
- SQLEndTran function [ODBC]
ms.assetid: ff375ce1-eb50-4693-b1e6-70181a6dbf9f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2961cab7c061583f68387e29ed564efc16916870
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195594"
---
# <a name="sqlendtran-function"></a>SQLEndTran 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 对于与连接相关联的所有语句， **SQLEndTran** 请求执行提交或回滚操作。 **SQLEndTran** 还可以请求为与环境关联的所有连接执行提交或回滚操作。  
  
> [!NOTE]  
>  有关在 ODBC 3 时驱动程序管理器将此函数映射到的内容的详细信息。*x* 应用程序正在使用 ODBC 2。*x* 驱动程序，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLEndTran(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle,  
     SQLSMALLINT   CompletionType);  
```  
  
## <a name="arguments"></a>参数  
 *HandleType*  
 送句柄类型标识符。 包含 SQL_HANDLE_ENV (如果 *handle* 为环境) 句柄，则为; 如果 *handle* 为连接句柄，则为 SQL_HANDLE_DBC () 。  
  
 *Handle*  
 送 *HandleType* 指示的类型的句柄，指示事务的作用域。 有关详细信息，请参阅 "注释"。  
  
 *CompletionType*  
 送以下两个值之一：  
  
 SQL_COMMIT SQL_ROLLBACK  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_STILL_EXECUTING。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLEndTran** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用相应的 *HandleType* 和 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLEndTran** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08003|连接未打开| (DM) *HandleType* 已 SQL_HANDLE_DBC，并且该 *句柄* 未处于已连接状态。|  
|08007|事务期间连接失败|*HandleType* 是 SQL_HANDLE_DBC 的，并且与 *句柄* 关联的连接在执行函数时失败，并且无法确定请求的 **提交** 或 **回滚** 是否发生在失败之前。|  
|25S01|事务状态未知|*句柄* 中的一个或多个连接无法完成具有指定结果的事务，结果未知。|  
|25S02|事务仍处于活动状态|驱动程序无法保证可以自动完成全局事务中的所有工作，并且该事务仍处于活动状态。|  
|25S03|事务已回滚|驱动程序无法保证可以自动完成全局事务中的所有工作，并且回滚了 *事务中处于* 活动状态的所有工作。|  
|40001|序列化失败|由于另一个事务发生了资源死锁，事务已回滚。|  
|40002|完整性约束冲突|*CompletionType* 是 SQL_COMMIT 的，更改的承诺导致了完整性约束冲突。 因此，事务已回滚。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* SzMessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *ConnectionHandle* 启用异步处理。 在对 *ConnectionHandle* 调用 [SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)之前，调用了函数。 然后，在 *ConnectionHandle* 上再次调用该函数。<br /><br /> 调用了函数，并且在完成执行 **SQLCancelHandle** 之前，从多线程应用程序中的另一个线程调用了 *ConnectionHandle* 。|  
|HY010|函数序列错误| (DM) 为与 *ConnectionHandle* 关联的语句句柄调用了异步执行函数，但在调用 **SQLEndTran** 时仍在执行该函数。<br /><br />  (DM) 异步执行的函数 (不是为 *ConnectionHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br /> 为与 *ConnectionHandle* 关联的语句句柄调用了 (DM) **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) 异步执行的函数 (不是为 *HandleType* 设置为 SQL_HANDLE_DBC 的 *句柄* 调用了) ，并且在调用此函数时仍在执行该函数。<br /><br />  (DM) 为与 *句柄* 关联并返回 SQL_PARAM_DATA_AVAILABLE 的一个语句句柄调用了 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** 。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY012|事务操作代码无效| (DM) 为参数 *CompletionType* 指定的值既不 SQL_COMMIT 也不 SQL_ROLLBACK。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY092|无效的属性/选项标识符| (DM) 为参数 *HandleType* 指定的值既不 SQL_HANDLE_ENV 也不 SQL_HANDLE_DBC。|  
|HY115|对于包含启用了异步函数执行的连接的环境，不允许使用 SQLEndTran|如果对环境中的连接启用了连接函数的异步执行，则不能将 (DM) *HandleType* 设置为 SQL_HANDLE_ENV。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅本主题的 "注释" 部分。|  
|HYC00|未实现的可选功能|驱动程序或数据源不支持 **回滚** 操作。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *ConnectionHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 对于 ODBC 3。*x* 驱动程序，如果 *HandleType* 为 SQL_HANDLE_ENV 并且 *HANDLE* 为有效的环境句柄，则驱动程序管理器将在与环境关联的每个驱动程序中调用 **SQLEndTran** 。 对驱动程序的调用的 *Handle* 参数将是驱动程序的环境句柄。 对于 ODBC 2，则为。*x* 驱动程序，如果 *HandleType* SQL_HANDLE_ENV 是有效的环境句柄，并且 *HANDLE* 为有效的环境句柄，则在该环境中，对于处于连接状态的每个连接，驱动程序管理器将为该环境中的每个连接调用一次 **SQLTransact** 。 每个调用中的 *handle* 参数将是连接的句柄。 无论是哪种情况，驱动程序都将尝试根据 *CompletionType* 的值提交或回滚事务，该环境中的所有连接都处于连接状态。 不活动的连接不会影响事务。  
  
> [!NOTE]  
>  **SQLEndTran** 不能用于在共享环境中提交或回滚事务。 SQLSTATE HY092 (如果在共享环境 *的句柄设置为* 共享环境的句柄或连接的句柄时调用 **SQLEndTran** ，则将返回属性/选项标识符无效) 。  
  
 仅当驱动程序管理器收到每个连接的 SQL_SUCCESS 时，驱动程序管理器才会返回 SQL_SUCCESS。 如果驱动程序管理器接收一个或多个连接上的 SQL_ERROR，则会将 SQL_ERROR 返回到应用程序，并且将诊断信息放置在环境的诊断数据结构中。 若要在提交或回滚操作期间确定哪些连接或连接失败，应用程序可以为每个连接调用 **SQLGetDiagRec** 。  
  
> [!NOTE]  
>  驱动程序管理器不会在所有连接上模拟全局事务，因此不会使用两阶段提交协议。  
  
 如果 SQL_COMMIT *CompletionType* ，则 **SQLEndTran** 会发出对与受影响的连接关联的任何语句上的所有活动操作的提交请求。 如果 SQL_ROLLBACK *CompletionType* ， **SQLEndTran** 将发出对与受影响的连接关联的任何语句上的所有活动操作的回滚请求。 如果没有活动事务，则 **SQLEndTran** 将返回 SQL_SUCCESS，而不会对任何数据源产生任何影响。 有关详细信息，请参阅 [提交和回滚事务](../../../odbc/reference/develop-app/committing-and-rolling-back-transactions.md)。  
  
 如果驱动程序处于手动提交模式 (通过调用 **SQLSetConnectAttr** ，并将 SQL_ATTR_AUTOCOMMIT 属性设置为 SQL_AUTOCOMMIT_OFF) ，在对当前数据源执行可包含在事务中的 SQL 语句时，将隐式启动一个新事务。 有关详细信息，请参阅 [提交模式](../../../odbc/reference/develop-app/commit-mode.md)。  
  
 若要确定事务操作如何影响游标，应用程序需要使用 SQL_CURSOR_ROLLBACK_BEHAVIOR 和 SQL_CURSOR_COMMIT_BEHAVIOR 选项调用 **SQLGetInfo** 。 有关详细信息，请参阅以下段落，并查看 [事务对游标和预定义语句的影响](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md)。  
  
 如果 SQL_CURSOR_ROLLBACK_BEHAVIOR 或 SQL_CURSOR_COMMIT_BEHAVIOR 值等于 SQL_CB_DELETE， **SQLEndTran** 将关闭并删除与该连接关联的所有语句上的所有打开的游标，并放弃所有挂起的结果。 **SQLEndTran** 会在已分配 (未准备好) 状态中保留所有语句;应用程序可以重复使用它们来执行后续的 SQL 请求，或者可以调用 **SQLFreeStmt** 或 **SQLFreeHandle** *HandleType* 的 SQL_HANDLE_STMT 来解除分配它们。  
  
 如果 SQL_CURSOR_ROLLBACK_BEHAVIOR 或 SQL_CURSOR_COMMIT_BEHAVIOR 值等于 SQL_CB_CLOSE，则 **SQLEndTran** 将在与该连接关联的所有语句上关闭所有打开的游标。 **SQLEndTran** 使任何语句都处于准备好的状态;应用程序可以在不首先调用 **SQLPrepare** 的情况下，为与连接关联的语句调用 **SQLExecute** 。  
  
 如果 SQL_CURSOR_ROLLBACK_BEHAVIOR 或 SQL_CURSOR_COMMIT_BEHAVIOR 值等于 SQL_CB_PRESERVE，则 **SQLEndTran** 不会影响与该连接关联的打开的游标。 在调用 **SQLEndTran** 之前，游标保留在它们指向的行上。  
  
 对于支持事务的驱动程序和数据源，如果没有活动事务，则使用 SQL_COMMIT 或 SQL_ROLLBACK 调用 **SQLEndTran** 会返回 SQL_SUCCESS (，指示没有要提交或回滚的工作) 并且不会影响数据源。  
  
 当驱动程序处于自动提交模式时，驱动程序管理器不会在该驱动程序中调用 **SQLEndTran** 。 **SQLEndTran** 始终返回 SQL_SUCCESS，而不考虑是否使用 SQL_COMMIT 或 SQL_ROLLBACK 的 *CompletionType* 调用它。  
  
 不支持事务 (**SQLGetInfo** *选项* 的驱动程序或数据源 SQL_TXN_CAPABLE 为 SQL_TC_NONE) 实际上始终处于自动提交模式，因此无论是否使用 SQL_SUCCESS 或 SQL_COMMIT 的 *CompletionType* 调用 **SQLEndTran** ，都始终返回 SQL_ROLLBACK。 此类驱动程序和数据源不会在请求时实际回滚事务。  
  
## <a name="suspended-state"></a>挂起状态  
 在 Windows 7 之前发布的驱动程序管理器中，如果 **SQLEndTran** 从驱动程序返回 SQL_ERROR，则事务处于活动状态。 但是，事务已在服务器上成功提交，但客户端上的驱动程序未被通知 (例如，因为) 出现网络错误。 这会使连接处于错误的状态。 从 Windows 7 开始，当 **SQLEndTran** 返回 SQL_ERROR 时，连接可能处于挂起状态。 处于挂起状态时，可以调用只读函数。 最终，应用程序应调用挂起的连接上的 **SQLDisconnect** ，以释放资源。  
  
 如果满足以下所有条件，则连接将被置于挂起状态：  
  
-   驱动程序从 **SQLEndTran** 返回 SQL_ERROR。  
  
-   驱动程序为 ODBC 版本3.8 或更高版本。  
  
-   应用程序版本为3.8 或更高版本;或者，重新编译的 ODBC 2.x 或3.x 应用程序成功通过 **SQLCancelHandle** 取消了 **SQLEndTran** 函数。  
  
-   驱动程序未返回下列消息之一，这将确认该事务未完成：  
  
    -   25S03：事务已回滚  
  
    -   40001：序列化失败  
  
    -   40002：完整性约束  
  
    -   HYC00：未实现的可选功能  
  
 如果在环境句柄上调用 **SQLEndTran** ，并且其某个连接满足上述条件，则连接到同一驱动程序的所有连接都将进入挂起状态。  
  
 在应用程序调用挂起的连接上的 **SQLDisconnect** 后，可以使用该连接重新连接到另一个数据源或同一数据源。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|取消在连接句柄上异步运行的函数。|[SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|返回有关驱动程序或数据源的信息|[SQLGetInfo 函数](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|释放句柄|[SQLFreeHandle 函数](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|释放语句句柄|[SQLFreeStmt 函数](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [异步执行（轮询方法）](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
