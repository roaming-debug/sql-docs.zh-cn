---
description: 连接转换
title: 连接转换 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], connection
- connection transitions [ODBC]
- state transitions [ODBC], connection
ms.assetid: 6b6e1a47-4a52-41c8-bb9e-7ddeae09913e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 57135740af1c8b7a4b8637b33adacf280c025e6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071761"
---
# <a name="connection-transitions"></a>连接转换
ODBC 连接具有以下状态。  
  
|状态|说明|  
|-----------|-----------------|  
|C0|未分配环境，未分配的连接|  
|C1|已分配环境，未分配连接|  
|C2|已分配环境，已分配连接|  
|C3|连接函数需要数据|  
|C4|连接的连接|  
|C5|已连接的连接，已分配语句|  
|C6|连接的连接，事务正在进行。 连接可能处于非 C6 状态，并且不会在该连接上分配语句。 例如，假设连接处于手动提交模式，并且处于 state。 如果分配了语句并 (启动事务) ，然后释放该语句，则该事务将保持活动状态，但不会在该连接上使用任何语句。|  
  
 下表显示了每个 ODBC 函数如何影响连接状态。  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|C0<br /><br /> 无 Env。|C1 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|--------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|C1 [1]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
| (IH) [2]|C2|--[5]|--[5]|--[5]|--[5]|--[5]|  
| (IH) [3]| (IH) | (08003) | (08003) |C5|--[5]|--[5]|  
| (IH) [4]| (IH) | (08003) | (08003) |--[5]|--[5]|--[5]|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 此行显示 *HandleType* SQL_HANDLE_STMT 时的转换。  
  
 [4] 此行显示 *HandleType* SQL_HANDLE_DESC 时的转换。  
  
 [5] 调用 **SQLAllocHandle** 时，如果 *OutputHandlePtr* 指向有效的句柄，则会覆盖该句柄，而不考虑以前的内容 ofthat 句柄，并可能会导致 ODBC 驱动程序出现问题。 这是不正确的 ODBC 应用程序编程，用为 *\* OutputHandlePtr* 定义的同一个应用程序变量来调用 **SQLAllocHandle** 两次，而不调用 **SQLFreeHandle** 来释放该句柄，然后再重新分配它。 以这种方式覆盖 ODBC 句柄可能会导致 ODBC 驱动程序部分出现不一致的行为或错误。  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) |C3 [d] C4 [s]|--[d] C2 [e] C4 [s]| (08002) | (08002) | (08002) |  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (IH) | (IH) | (IH) |--|--[1] C5 [2]|  
  
 [1] 连接处于手动提交模式。  
  
 [2] 连接处于自动提交模式。  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges、SQLColumns、SQLForeignKeys、SQLGetTypeInfo、SQLPrimaryKeys、SQLProcedureColumns、SQLProcedures、SQLSpecialColumns、SQLStatistics、SQLTablePrivileges 和 SQLTables  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (IH) | (IH) | (IH) |--[1] C6 [2]|--|  
  
 [1] 连接处于自动提交模式，或数据源未开始事务。  
  
 [2] 连接处于手动提交模式，并且数据源开始了一个事务。  
  
## <a name="sqlconnect"></a>SQLConnect  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) |C4| (08002) | (08002) | (08002) | (08002) |  
  
## <a name="sqlcopydesc-sqlgetdescfield-sqlgetdescrec-sqlsetdescfield-and-sqlsetdescrec"></a>SQLCopyDesc、SQLGetDescField、SQLGetDescRec、SQLSetDescField 和 SQLSetDescRec  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (IH) | (IH) |--[1]|--|--|  
  
 [1] 在此状态下，可用于应用程序的唯一描述符是显式分配的描述符。  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources 和 SQLDrivers  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) |--|--|--|--|--|--|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (08003) |C2|C2|C2|25000|  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) |C4 s--n [f]| (08002) | (08002) | (08002) | (08002) |  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) [1]|--[3]|--[3]|--[3]|--|--|--[4] 或 ( [5]，[6]，[8] ) C4 [5] 和 [7] C5 [5]，[6]，和 [9]|  
| (IH) [2]| (IH) | (08003) | (08003) |--|--|C5|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 因为连接未处于连接状态，所以它不受事务的影响。  
  
 [4] 连接上的提交或回滚失败。 在这种情况下，函数将返回 SQL_ERROR。  
  
 [5] 在连接上成功提交或回滚。 如果提交或回滚在另一个连接上失败，则函数将返回 SQL_ERROR; 如果在所有连接上完成提交或回滚，则函数返回 SQL_SUCCESS。  
  
 [6] 连接上至少分配了一个语句。  
  
 [7] 没有在连接上分配语句。  
  
 [8] 连接至少具有一个已打开的游标的语句，在提交或回滚事务时，数据源会保留游标，无论 *CompletionType* 是 SQL_COMMIT 还是 SQL_ROLLBACK) ，都适用 (。 有关详细信息，请参阅 SQL_CURSOR_COMMIT_BEHAVIOR 和 SQL_CURSOR_ROLLBACK_BEHAVIOR [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)中的属性。  
  
 [9] 如果连接的所有语句都有打开的游标，则提交或回滚事务时，不会保留游标。  
  
## <a name="sqlexecdirect-and-sqlexecute"></a>SQLExecDirect 和 SQLExecute  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (IH) | (IH) | (IH) |--[1] C6 [2] C6 [3]|--|  
  
 [1] 连接处于自动提交模式，而执行的语句不是 *游标**规范* (例如 SELECT 语句) ;或者连接处于手动提交模式，而执行的语句未开始事务。  
  
 [2] 连接处于自动提交模式，而执行的语句是 *游标**规范* (例如 SELECT 语句) 。  
  
 [3] 连接处于手动提交模式，并且数据源开始了一个事务。  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) [1]|C0| (HY010) | (HY010) | (HY010) | (HY010) | (HY010) |  
| (IH) [2]| (IH) | (C1) | (HY010) | (HY010) | (HY010) | (HY010) |  
| (IH) [3]| (IH) | (IH) | (IH) | (IH) |C4 [5]--[6]|--[7] C4 [5] 和 [8] C5 [6] 和 [8]|  
| (IH) [4]| (IH) | (IH) | (IH) |--|--|--|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 此行显示 *HandleType* SQL_HANDLE_STMT 时的转换。  
  
 [4] 此行显示 *HandleType* SQL_HANDLE_DESC 时的转换。  
  
 [5] 连接上只分配有一个语句。  
  
 [6] 连接上分配了多个语句。  
  
 [7] 连接处于手动提交模式。  
  
 [8] 连接处于自动提交模式。  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) [1]| (IH) | (IH) | (IH) | (IH) |--|C5 [3]--[4]|  
| (IH) [2]| (IH) | (IH) | (IH) | (IH) |--|--|  
  
 [1] 在 SQL_CLOSE *选项* 参数时，该行显示事务。  
  
 [2] 当 *选项* 参数 SQL_UNBIND 或 SQL_RESET_PARAMS 时，该行显示事务。  
  
 [3] 连接处于自动提交模式，并且除此外的任何语句上都未打开任何游标。  
  
 [4] 连接处于手动提交模式，或处于自动提交模式，并且至少在一个其他语句上打开了游标。  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|HY010|--|--|--|  
  
 [1] *属性* 参数 SQL_ATTR_ACCESS_MODE、SQL_ATTR_AUTOCOMMIT、SQL_ATTR_LOGIN_TIMEOUT、SQL_ATTR_ODBC_CURSORS、SQL_ATTR_TRACE 或 SQL_ATTR_TRACEFILE 或已为连接属性设置的值。  
  
 [2] *属性* 参数不是 SQL_ATTR_ACCESS_MODE、SQL_ATTR_AUTOCOMMIT、SQL_ATTR_LOGIN_TIMEOUT、SQL_ATTR_ODBC_CURSORS、SQL_ATTR_TRACE 或 SQL_ATTR_TRACEFILE，并且没有为连接属性设置值。  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField 和 SQLGetDiagRec  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) [1]|--|--|--|--|--|--|  
| (IH) [2]| (IH) |--|--|--|--|--|  
| (IH) [3]| (IH) | (IH) | (IH) | (IH) |--|--|  
| (IH) [4]| (IH) | (IH) | (IH) |--|--|--|  
  
 [1] 此行显示 SQL_HANDLE_ENV *HandleType* 时的转换。  
  
 [2] 在 SQL_HANDLE_DBC *HandleType* 时，该行显示转换。  
  
 [3] 此行显示 *HandleType* SQL_HANDLE_STMT 时的转换。  
  
 [4] 此行显示 *HandleType* SQL_HANDLE_DESC 时的转换。  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|HY010|HY010|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|08003|--|--|--|  
  
 [1] *InfoType* 参数 SQL_ODBC_VER。  
  
 [2] *InfoType* 参数未 SQL_ODBC_VER。  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (IH) | (IH) | (IH) |--[1] C6 [2]|--[3] C5 [1]|  
  
 [1] 连接处于自动提交模式，并且对 **SQLMoreResults** 的调用未初始化对游标规范的结果集的处理。  
  
 [2] 连接处于自动提交模式，并且对 **SQLMoreResults** 的调用已经初始化了游标规范的结果集的处理。  
  
 [3] 连接处于手动提交模式。  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (08003) | (08003) |--|--|--|  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (IH) | (IH) | (IH) |--[1] C6 [2]|--|  
  
 [1] 连接处于自动提交模式，或数据源未开始事务。  
  
 [2] 连接处于手动提交模式，并且数据源开始了一个事务。  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|HY010|--[3] 08002 [4] HY011 [5]|--[3] 08002 [4] HY011 [5]|--[3] 和 [6] C5 [8] 08002 [4] HY011 [5] 或 [7]|  
  
 [1] *特性* 参数不是 SQL_ATTR_TRANSLATE_LIB 或 SQL_ATTR_TRANSLATE_OPTION。  
  
 [2] *特性* 参数 SQL_ATTR_TRANSLATE_LIB 或 SQL_ATTR_TRANSLATE_OPTION。  
  
 [3] *特性* 参数不是 SQL_ATTR_ODBC_CURSORS 或 SQL_ATTR_PACKET_SIZE。  
  
 [4] *特性* 参数 SQL_ATTR_ODBC_CURSORS。  
  
 [5] *特性* 参数 SQL_ATTR_PACKET_SIZE。  
  
 [6] 找不到 *属性* 参数 SQL_ATTR_AUTOCOMMIT，或 *属性* 参数 SQL_ATTR_AUTOCOMMIT 并且设置此属性未提交事务。  
  
 [7] *特性* 参数 SQL_ATTR_TXN_ISOLATION。  
  
 [8] *特性* 参数 SQL_ATTR_AUTOCOMMIT，并设置此特性已提交事务。  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) |--|--| (HY010) |--|--|--|  
  
## <a name="all-other-odbc-functions"></a>所有其他 ODBC 函数  
  
|C0<br /><br /> 无 Env。|C1<br /><br /> 未分配|C2<br /><br /> 已分配|C3<br /><br /> 需要数据|C4<br /><br /> 连续|C5<br /><br /> 语句|C6<br /><br /> 事务|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
| (IH) | (IH) | (IH) | (IH) | (IH) |--|--|
