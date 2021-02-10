---
description: 语句转换
title: 语句转换 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], statement
- state transitions [ODBC], statement
- statement transitions [ODBC]
ms.assetid: 3d70e0e3-fe83-4b4d-beac-42c82495a05b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 904b1116be5e0daec2fce61e4a988c9b957322ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016237"
---
# <a name="statement-transitions"></a>语句转换
ODBC 语句具有以下状态。  
  
|状态|说明|  
|-----------|-----------------|  
|S0|未分配语句。  (连接状态必须是 C4、C5 或 C6。 有关详细信息，请参阅 [连接转换](../../../odbc/reference/appendixes/connection-transitions.md)。 ) |  
|S1|已分配语句。|  
|S2|预定义语句。 不会创建任何结果集。|  
|S3|预定义语句。 将创建一个 (可能为空的) 结果集。|  
|S4|已执行语句，但未创建结果集。|  
|S5|已执行语句，并且 (可能为空，因为创建了结果集) 。 游标已打开并定位在结果集的第一行之前。|  
|S6|与 **SQLFetch** 或 **SQLFetchScroll** 定位的光标。|  
|S7|光标定位到 **SQLExtendedFetch**。|  
|S8|函数需要数据。 尚未调用 **SQLParamData** 。|  
|S9|函数需要数据。 尚未调用 **SQLPutData** 。|  
|S10|函数需要数据。 已调用 **SQLPutData** 。|  
|S11|仍在执行。 如果异步执行的函数返回 SQL_STILL_EXECUTING，则语句将处于此状态。 在执行接受语句句柄的任何函数时，语句暂时处于此状态。 状态 S11 中的临时居住不显示在 **SQLCancel** 状态表除外的任何状态表中。 当语句暂时处于 S11 状态时，可以通过从另一个线程调用 **SQLCancel** 来取消该函数。|  
|S12|异步执行已取消。 在 S12 中，应用程序必须调用已取消的函数，直到它返回 SQL_STILL_EXECUTING 以外的值。 仅当函数返回 SQL_ERROR 和 SQLSTATE HY008 (操作取消) 时，才已成功取消该函数。 如果返回任何其他值（如 SQL_SUCCESS），则取消操作将失败，并且该函数将正常执行。|  
  
 状态 S2 和 S3 称为准备状态，通过 S7 将 S5 声明为游标状态，将 S8 通过 S10 状态视为需要数据状态，并将 S11 和 S12 状态视为异步状态。 在其中每个组中，仅当组中的每个状态都不同时，转换才会单独显示;大多数情况下，每个组中的每个状态的转换都是相同的。  
  
 下表显示了每个 ODBC 函数如何影响语句状态。  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]，[5]，[6]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[2]，[5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|S1 [3]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[4]，[5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 此行显示 *HandleType* SQL_HANDLE_STMT 时的转换。  
  
 [4] 此行显示 *HandleType* SQL_HANDLE_DESC 时的转换。  
  
 [5] 调用 **SQLAllocHandle** 时，如果 *OutputHandlePtr* 指向有效的句柄，则会覆盖该句柄，而不考虑该句柄以前的内容，并可能导致 ODBC 驱动程序出现问题。 这是不正确的 ODBC 应用程序编程，用为 *\* OutputHandlePtr* 定义的同一个应用程序变量来调用 **SQLAllocHandle** 两次，而不调用 **SQLFreeHandle** 来释放该句柄，然后再重新分配它。 以这种方式覆盖 ODBC 句柄可能会导致 ODBC 驱动程序部分出现不一致的行为或错误。  
  
## <a name="sqlbindcol"></a>SQLBindCol  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbindparameter"></a>SQLBindParameter  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbrowseconnect-sqlconnect-and-sqldriverconnect"></a>SQLBrowseConnect、SQLConnect 和 SQLDriverConnect  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|08002|08002|08002|08002|08002|08002|08002|  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|查看下一个表|HY010|NS [c] HY010 o|  
  
## <a name="sqlbulkoperations-cursor-states"></a>SQLBulkOperations (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[s] S8 [d] S11 [x]|--[s] S8 [d] S11 [x]|HY010|  
  
## <a name="sqlcancel"></a>SQLCancel  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|S1 [1] S2 [nr] 和 [2] S3 [r] 和 [2] S5 [3] 和 [5] S6 ( [3] 或 [4] ) 和 [6] S7 [4] 和 [7]|查看下一个表|  
  
 [1]   **SQLExecDirect** 返回 SQL_NEED_DATA。  
  
 [2]   **SQLExecute** 返回 SQL_NEED_DATA。  
  
 [3]   **SQLBulkOperations** 返回 SQL_NEED_DATA。  
  
 [4]   **SQLSetPos** 返回 SQL_NEED_DATA。  
  
 [5] 未调用   **SQLFetch**、 **SQLFetchScroll** 或 **SQLExtendedFetch** 。  
  
 已调用 [6]   **SQLFetch** 或 **SQLFetchScroll** 。  
  
 已调用 [7]   **SQLExtendedFetch** 。  
  
## <a name="sqlcancel-asynchronous-states"></a>SQLCancel (异步状态)   
  
|S11<br /><br /> 仍在执行|S12<br /><br /> 已取消异步|  
|-----------------------------|-----------------------------|  
|NS [1] S12 [2]|S12|  
  
 [1] 执行函数时，语句暂时处于 S11 状态。 **SQLCancel** 是从其他线程调用的。  
  
 [2] 该语句处于状态 S11，因为一个以异步方式返回的函数 SQL_STILL_EXECUTING。  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|24000|24000|24000|S1 [np] S3 [p]|HY010|HY010|  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|查看下一个表|24000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqlcolattribute-prepared-states"></a>SQLColAttribute (准备好的状态)   
  
|S2<br /><br /> 无结果|S3<br /><br /> 结果|  
|-----------------------|--------------------|  
|--[1] 07005 [2]|--[s] S11 x|  
  
 [1]   *FieldIdentifier* SQL_DESC_COUNT。  
  
 未 SQL_DESC_COUNT [2]   *FieldIdentifier* 。  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges、SQLColumns、SQLForeignKeys、SQLGetTypeInfo、SQLPrimaryKeys、SQLProcedureColumns、SQLProcedures、SQLSpecialColumns、SQLStatistics、SQLTablePrivileges 和 SQLTables  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
| (IH) |S5 [s] S11 [x]|S1 [e] S5 [s] S11 [x]|S1 [e] 和 [1] S5 [s] 和 [1] S11 [x] 和 [1] 24000 [2]|查看下一个表|HY010|NS [c] HY010 o|  
  
 [1] 当前结果为最后一个或唯一的结果，或者没有当前结果。 有关多个结果的详细信息，请参阅 [多个结果](../../../odbc/reference/develop-app/multiple-results.md)。  
  
 [2] 当前结果不是最后一个结果。  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables-cursor-states"></a>SQLColumnPrivileges、SQLColumns、SQLForeignKeys、SQLGetTypeInfo、SQLPrimaryKeys、SQLProcedureColumns、SQLProcedures、SQLSpecialColumns、SQLStatistics、SQLTablePrivileges 和 SQLTables (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000 [1]|24000|  
  
 [1] 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器会返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则驱动程序将返回此错误。  
  
## <a name="sqlcopydesc"></a>SQLCopyDesc  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH [1]|--|--|--|--|HY010|NS [c] 和 [3] HY010 [o] 或 [4]|  
|IH [2]|HY010|查看下一个表|24000|--[s] S11 x|HY010|NS [c] 和 [3] HY010 [o] 或 [4]|  
  
 [1] 在 *SourceDescHandle* 参数是 ARD、APD 或 IPD 时，该行显示转换。  
  
 [2] 当 *SourceDescHandle* 参数是 IRD 时，该行显示转换。  
  
 [3] *SourceDescHandle* 和 *TargetDescHandle* 参数与异步运行的 **SQLCopyDesc** 函数相同。  
  
 [4] *SourceDescHandle* 参数或 *TargetDescHandle* 参数 (或) 两者都不同于异步运行的 **SQLCopyDesc** 函数。  
  
## <a name="sqlcopydesc-prepared-states"></a>SQLCopyDesc (准备好的状态)   
  
|S2<br /><br /> 无结果|S3<br /><br /> 结果|  
|-----------------------|--------------------|  
|24000 [1]|--[s] S11 [x]|  
  
 2当 *SourceDescHandle* 参数是 IRD 时，该行显示转换。  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources 和 SQLDrivers  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqldescribecol"></a>SQLDescribeCol  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|查看下一个表|24000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqldescribecol-prepared-states"></a>SQLDescribeCol (准备好的状态)   
  
|S2<br /><br /> 无结果|S3<br /><br /> 结果|  
|-----------------------|--------------------|  
|07005|--[s] S11 [x]|  
  
## <a name="sqldescribeparam"></a>SQLDescribeParam  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|HY010|HY010|HY010|NS [c] HY010 [o]|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|S0 [1]|S0 [1]|S0 [1]|S0 [1]| (HY010) | (HY010) |  
  
 [1] 调用 **SQLDisconnect** 将释放与该连接关联的所有语句。 而且，这会将连接状态返回到 C2;在语句状态为 S0 之前，连接状态必须为 C4。  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--[2] 或 [3] S1 [1]|--[3] S1 [np] 和 ( [1] 或 [2] ) S1 [p] 和 [1] S2 [p] 和 [2]|--[3] S1 [np] 和 ( [1] 或 [2] ) S1 [p] 和 [1] S3 [p] 和 [2]| (HY010) | (HY010) |  
  
 [1] *CompletionType* 参数为 SQL_COMMIT， **SQLGetInfo** 返回 SQL_CURSOR_COMMIT_BEHAVIOR 信息类型的 SQL_CB_DELETE，或者 *CompletionType* 参数为 SQL_ROLLBACK， **SQLGetInfo** 返回 SQL_CB_DELETE 信息类型的 SQL_CURSOR_ROLLBACK_BEHAVIOR。  
  
 [2] *CompletionType* 参数为 SQL_COMMIT， **SQLGetInfo** 返回 SQL_CURSOR_COMMIT_BEHAVIOR 信息类型的 SQL_CB_CLOSE，或者 *CompletionType* 参数为 SQL_ROLLBACK， **SQLGetInfo** 返回 SQL_CB_CLOSE 信息类型的 SQL_CURSOR_ROLLBACK_BEHAVIOR。  
  
 [3] *CompletionType* 参数为 SQL_COMMIT， **SQLGetInfo** 返回 SQL_CURSOR_COMMIT_BEHAVIOR 信息类型的 SQL_CB_PRESERVE，或者 *CompletionType* 参数为 SQL_ROLLBACK， **SQLGetInfo** 返回 SQL_CB_PRESERVE 信息类型的 SQL_CURSOR_ROLLBACK_BEHAVIOR。  
  
## <a name="sqlexecdirect"></a>SQLExecDirect  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
| (IH) |S4 [s] 和 [nr] S5 [s] 和 [r] S8 [d] S11 [x]|--[e] 和 [1] S1 [e] 和 [2] S4 [s] 和 [nr] S5 [s] 和 [r] S8 [d] S11 [x]|--[e]、[1] 和 [3] S1 [e]、[2] 和 [3] S4 [s]、[nr] 和 [3] S5 [s]、[r] 和 [3] S8 [d] 和 [3] 24000 [4]|查看下一个表|HY010|NS [c] HY010 [o]|  
  
 [1] 驱动程序管理器返回了错误。  
  
 [2] 驱动程序管理器未返回该错误。  
  
 [3] 当前结果为最后一个或唯一的结果，或者没有当前结果。 有关多个结果的详细信息，请参阅 [多个结果](../../../odbc/reference/develop-app/multiple-results.md)。  
  
 [4] 当前结果不是最后一个结果。  
  
## <a name="sqlexecdirect-cursor-states"></a>SQLExecDirect (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000 [1]|24000|  
  
 [1] 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器会返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则由驱动程序返回。  
  
## <a name="sqlexecute"></a>SQLExecute  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
| (IH) | (HY010) |查看下一个表|S2 [e]，p，和 [1] S4 [s]，[p]，[nr]，[1] S5 [s]，[p]，[r] 和 [1] S8 [d]，[p]，and [1] S11 [x]，[p]，[1] 24000 [p] 和 [2] HY010 [np]|请参阅游标状态表|HY010|NS [c] HY010 [o]|  
  
 [1] 当前结果为最后一个或唯一的结果，或者没有当前结果。 有关多个结果的详细信息，请参阅 [多个结果](../../../odbc/reference/develop-app/multiple-results.md)。  
  
 [2] 当前结果不是最后一个结果。  
  
## <a name="sqlexecute-prepared-states"></a>SQLExecute (准备好的状态)   
  
|S2<br /><br /> 无结果|S3<br /><br /> 结果|  
|-----------------------|--------------------|  
|S4 [s] S8 [d] S11 [x]|S5 [s] S8 [d] S11 [x]|  
  
## <a name="sqlexecute-cursor-states"></a>SQLExecute (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000 [p] HY010 [np]|24000 [p]，[1] HY010 [np]|24000 [p] HY010 [np]|  
  
 [1] 如果 **SQLFetch** 或 **SQLFetchScroll** 未返回 SQL_NO_DATA，驱动程序管理器会返回此错误，如果 **SQLFetch** 或 **SQLFetchScroll** 已 SQL_NO_DATA 返回，则由驱动程序返回。  
  
## <a name="sqlextendedfetch"></a>SQLExtendedFetch  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S1010|S1010|24000|查看下一个表|S1010|NS [c] S1010 [o]|  
  
## <a name="sqlextendedfetch-cursor-states"></a>SQLExtendedFetch (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S7 [s] 或 [nf-e] S11 [x]|S1010|--[s] 或 [nf-e] S11 [x]|  
  
## <a name="sqlfetch-and-sqlfetchscroll"></a>SQLFetch 和 SQLFetchScroll  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|查看下一个表|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlfetch-and-sqlfetchscroll-cursor-states"></a>SQLFetch 和 SQLFetchScroll (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S6 [s] 或 [nf-e] S11 [x]|--[s] 或 [nf-e] S11 [x]|HY010|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|HY010|HY010|HY010|HY010|HY010|HY010|  
|IH [2]|S0|S0|S0|S0|HY010|HY010|  
|--[3]|--|--|--|--|--|--|  
  
 [1] 当 *HandleType* SQL_HANDLE_ENV 或 SQL_HANDLE_DBC 时，该行显示转换。  
  
 [2] 在 SQL_HANDLE_STMT *HandleType* 时，该行显示转换。  
  
 [3] 此行显示 *HandleType* SQL_HANDLE_DESC 并显式分配描述符时的转换。  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH [1]|--|--|S1 [np] S2 [p]|S1 [np] S3 [p]|HY010|HY010|  
|IH [2]|--|--|--|--|HY010|HY010|  
  
 [1] 在 SQL_CLOSE *选项* 时，该行显示转换。  
  
 [2] 当 *选项* 为 SQL_UNBIND 或 SQL_RESET_PARAMS 时，该行显示转换。 如果 *选项* 参数为 SQL_DROP，而基础 *驱动程序为 ODBC 1.x 驱动程序* ，则驱动程序管理器会将此项映射到对 **SQLFreeHandle** 的调用，并将 *HandleType* 设置为 SQL_HANDLE_STMT。 有关详细信息，请参阅 [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)的转换表。  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetcursorname"></a>SQLGetCursorName  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlgetdata"></a>SQLGetData  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|查看下一个表|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlgetdata-cursor-states"></a>SQLGetData (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|--[s] 或 [nf-e] S11 [x] 24000 [b] HY109 [i]|--[s] 或 [nf-e] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField 和 SQLGetDescRec  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1] 或 [2] HY010 [3]|查看下一个表|--[1] 或 [2] 24000 [3]|--[1]、[2] 或 [3] S11 [3] 和 [x]|HY010|NS [c] 或 [4] HY010 [o] 和 [5]|  
  
 [1] *DescriptorHandle* 参数是 APD 或 ARD。  
  
 [2] *DescriptorHandle* 参数是 IPD。  
  
 [3] *DescriptorHandle* 参数是 IRD。  
  
 [4] *DescriptorHandle* 参数与异步运行的 **SQLGetDescField** 或 **SQLGetDescRec** 函数中的 *DescriptorHandle* 参数相同。  
  
 [5] *DescriptorHandle* 参数与异步运行的 **SQLGetDescField** 或 **SQLGetDescRec** 函数中的 *DescriptorHandle* 参数不同。  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec-prepared-states"></a>SQLGetDescField 和 SQLGetDescRec (准备好的状态)   
  
|S2<br /><br /> 无结果|S3<br /><br /> 结果|  
|-----------------------|--------------------|  
|--[1]、[2] 或 [3] S11 [2] 和 [x]|--[1]、[2] 或 [3] S11 [x]|  
  
 [1] *DescriptorHandle* 参数是 APD 或 ARD。  
  
 [2] *DescriptorHandle* 参数是 IPD。  
  
 [3] *DescriptorHandle* 参数是 IRD。 请注意，当 *DescriptorHandle* 为 IRD 时，这些函数始终返回状态 S2 SQL_NO_DATA。  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField 和 SQLGetDiagRec  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--|--|--|  
|IH [2]|--[3]|--[3]|--|--|--[3]|--[3]|  
  
 [1] 当 *HandleType* 为 SQL_HANDLE_ENV、SQL_HANDLE_DBC 或 SQL_HANDLE_DESC 时，该行显示转换。  
  
 [2] 在 SQL_HANDLE_STMT *HandleType* 时，该行显示转换。  
  
 [3] 当 SQL_DIAG_ROW_COUNT *DiagIdentifier* 时， **SQLGetDiagField** 始终返回此状态的错误。  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1] 24000 [2]|--[1] 24000 [2]|--[1] 24000 [2]|查看下一个表|HY010|HY010|  
  
 [1] 语句特性未 SQL_ATTR_ROW_NUMBER。  
  
 [2] 语句属性已 SQL_ATTR_ROW_NUMBER。  
  
## <a name="sqlgetstmtattr-cursor-states"></a>SQLGetStmtAttr (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[1] 24000 [2]|--[1] 或 ( [v] 和 [2] ) 24000 [b] 和 [2] HY109 [i] 和 [2]|--[i] 或 ( [v] 和 [2] ) 24000 [b] 和 [2] HY109 [1] 和 [2]|  
  
 [1] 未 SQL_ATTR_ROW_NUMBER *特性* 参数。  
  
 [2] *特性* 参数 SQL_ATTR_ROW_NUMBER。  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
| (IH) |--[1]|--[1]|--[s] 和 [2] S1 [nf-e]，[np]，[4] S2 [nf-e]，[p]，and [4] S5 [s] 和 [3] S11 [x]|S1 [nf-e]，[np]，and [4] S3 [nf-e]，[p] 和 [4] S4 [s] 和 [2] S5 [s] 和 [3] S11 [x]|HY010|NS [c] HY010 [o]|  
  
 [1] 函数始终返回此状态 SQL_NO_DATA。  
  
 [2] 下一个结果是行计数。  
  
 [3] 下一个结果是一个结果集。  
  
 [4] 当前结果为最后一个结果。  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlnumparams"></a>SQLNumParams  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlnumresultcols"></a>SQLNumResultCols  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata"></a>SQLParamData  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|HY010|HY010|查看下一个表|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata-need-data-states"></a>SQLParamData (需要数据状态)   
  
|S8<br /><br /> 需要数据|S9<br /><br /> 必须放入|S10<br /><br /> 可以将|  
|----------------------|---------------------|---------------------|  
|S1 [e] 和 [1] S2 [e]，[nr]，和 [2] S3 [e]，[r]，[2] S5 [e] 和 [4] S6 [e] 和 [5] S7 [e] 和 [3] S9 [d] S11 [x]|HY010|S1 [e] 和 [1] S2 [e]，[nr]，和 [2] S3 [e]，[r]，[2] S4 [s]，[nr]， ( [1] 或 [2] ) S5 [s]，[r]、和 ( [1] 或 [2] ) S5 ( [s] 或 [e] ) 和 [4] S6 ( [s] 或 [e] ) 和 [5] S7 ( [s] 或 [e] S11 [x]|  
  
 [1]   **SQLExecDirect** 返回 SQL_NEED_DATA。  
  
 [2]   **SQLExecute** 返回 SQL_NEED_DATA。  
  
 [3] 已从状态 S7 调用   **SQLSetPos** ，并返回 SQL_NEED_DATA。  
  
 [4]   **SQLBulkOperations** 已从 S5 状态调用，并返回 SQL_NEED_DATA。  
  
 [5] 已从状态 S6 调用了   **SQLSetPos** 或 **SQLBulkOperations** ，并返回 SQL_NEED_DATA。  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
| (IH) |S2 [s] 和 [nr] S3 [s] 和 [r] S11 [x]|--[s] 或 ( [e] 和 [1] ) S1 [e] 和 [2] S11 [x]|S1 [e] 和 [3] S2 [s]，[nr]，[3] S3 [s]，[r]，[3] S11 [x] 和 [3] 24000 [4]|查看下一个表|HY010|NS [c] HY010 [o]|  
  
 [1] 除了验证 SQLSTATE was HY009 [无效参数值] 或 HY090 [无效字符串或缓冲区长度] ) 的 (语句外，准备失败。  
  
 [2] 验证语句时，准备失败， (未 HY009 [无效的参数值] 或 HY090 [无效的字符串或缓冲区长度] ) 。  
  
 [3] 当前结果为最后一个或唯一的结果，或者没有当前结果。 有关多个结果的详细信息，请参阅 [多个结果](../../../odbc/reference/develop-app/multiple-results.md)。  
  
 [4] 当前结果不是最后一个结果。  
  
## <a name="sqlprepare-cursor-states"></a>SQLPrepare (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|24000|24000|  
  
## <a name="sqlputdata"></a>SQLPutData  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|HY010|HY010|查看下一个表|NS [c] HY010 [o]|  
  
## <a name="sqlputdata-need-data-states"></a>SQLPutData (需要数据状态)   
  
|S8<br /><br /> 需要数据|S9<br /><br /> 必须放入|S10<br /><br /> 可以将|  
|----------------------|---------------------|---------------------|  
|HY010|S1 [e] 和 [1] S2 [e]，[nr]，和 [2] S3 [e]，[r]，[2] S5 [e] 和 [4] S6 [e] 和 [5] S7 [e] 和 [3] S10 [s] S11 [x]|--[s] S1 [e] 和 [1] S2 [e]，[nr]，[2] S3 [e]，[r]，and [2] S5 [e] 和 [4] S6 [e] 和 [3] S11 [x] HY011 [6]|  
  
 [1]   **SQLExecDirect** 返回 SQL_NEED_DATA。  
  
 [2]   **SQLExecute** 返回 SQL_NEED_DATA。  
  
 [3] 已从状态 S7 调用   **SQLSetPos** ，并返回 SQL_NEED_DATA。  
  
 [4]   **SQLBulkOperations** 已从 S5 状态调用，并返回 SQL_NEED_DATA。  
  
 [5] 已从状态 S6 调用了   **SQLSetPos** 或 **SQLBulkOperations** ，并返回 SQL_NEED_DATA。  
  
 [6] 对 **SQLPutData** 的一个或多个调用返回 SQL_SUCCESS，然后使用 *StrLen_or_Ind* **设置为 SQL_NULL_DATA** 对同一参数进行调用。  
  
## <a name="sqlrowcount"></a>SQLRowCount  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
| (IH) | (HY010) | (HY010) |--|--| (HY010) | (HY010) |  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--[2] 24000 [3]|HY010|HY010|  
  
 [1] 此行显示 *属性* 为连接属性时的转换。 对于 "当 *属性* 为语句时转换" 属性，请参阅 **SQLSetStmtAttr** 的语句转换表。  
  
 [2] 未 SQL_ATTR_CURRENT_CATALOG *特性* 参数。  
  
 [3] *特性* 参数 SQL_ATTR_CURRENT_CATALOG。  
  
## <a name="sqlsetcursorname"></a>SQLSetCursorName  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|24000|24000|HY010|HY010|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField 和 SQLSetDescRec  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH [1]|--|--|--|--|HY010|HY010|  
  
 [1] 此行显示转换，其中 *DescriptorHandle* 参数是 **SQLSetDescField** 的 ARD、APD、IPD 或 (，当 *IRD* 参数为 SQL_DESC_ARRAY_STATUS_PTR 或 SQL_DESC_ROWS_PROCESSED_PTR 时) FieldIdentifier。 当 *FieldIdentifier* 为任何其他值时，为 IRD 调用 **SQLSetDescField** 是错误的。  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|HY011|HY011|HY011|HY011|Y011|HY01|HY011|  
  
## <a name="sqlsetpos"></a>SQLSetPos  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24000|查看下一个表|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlsetpos-cursor-states"></a>SQLSetPos (游标状态)   
  
|S5<br /><br /> 已打开|S6<br /><br /> SQLFetch 或 SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
  
|S0<br /><br /> 未分配|S1<br /><br /> 已分配|S2-S3<br /><br /> Prepared|S4<br /><br /> 执行|S5-S7<br /><br /> 游标|S8-S10<br /><br /> 需要数据|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--[1] HY011 [2]|--[1] 24000 [2]|--[1] 24000 [2]|HY010 [np] 或 [1] HY011 [p] 和 [2]|HY010 [np] 或 [1] HY011 [p] 和 [2]|  
  
 [1] *特性* 参数不是 SQL_ATTR_CONCURRENCY、SQL_ATTR_CURSOR_TYPE、SQL_ATTR_SIMULATE_CURSOR、SQL_ATTR_USE_BOOKMARKS、SQL_ATTR_CURSOR_SCROLLABLE 或 SQL_ATTR_CURSOR_SENSITIVITY。  
  
 [2] *特性* 参数 SQL_ATTR_CONCURRENCY、SQL_ATTR_CURSOR_TYPE、SQL_ATTR_SIMULATE_CURSOR、SQL_ATTR_USE_BOOKMARKS、SQL_ATTR_CURSOR_SCROLLABLE 或 SQL_ATTR_CURSOR_SENSITIVITY。
