---
description: SQLRowCount Function（SQLRowCount 函数）
title: SQLRowCount 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLRowCount
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLRowCount
helpviewer_keywords:
- SQLRowCount function [ODBC]
ms.assetid: 61e00a8a-9b3b-45b9-b397-7fe818822416
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1fea56b7bf7b54bf4d1aa02efa2c4cb9ea1978e9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192471"
---
# <a name="sqlrowcount-function"></a>SQLRowCount Function（SQLRowCount 函数）
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLRowCount** 返回受 **更新**、 **插入** 或 **删除** 语句影响的行数; **SQLBulkOperations** 中的 SQL_ADD、SQL_UPDATE_BY_BOOKMARK 或 SQL_DELETE_BY_BOOKMARK 操作;或 **SQLSetPos** 中的 SQL_UPDATE 或 SQL_DELETE 操作。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLRowCount(  
      SQLHSTMT   StatementHandle,  
      SQLLEN *   RowCountPtr);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄。  
  
 *RowCountPtr*  
 输出指向要返回行计数的缓冲区。 对于 **UPDATE**、 **INSERT** 和 **DELETE** 语句，对于 **SQLBulkOperations** 中的 SQL_ADD、SQL_UPDATE_BY_BOOKMARK 和 SQL_DELETE_BY_BOOKMARK 操作，以及对于 **SQLSetPos** 中的 SQL_UPDATE 或 SQL_DELETE 操作，在 **RowCountPtr* 中返回的值是受请求影响的行数; 如果受影响的行数不可用，则为-1。  
  
 当调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations**、 **SQLSetPos 或 SQLMoreResults** 时，诊断数据结构的 SQL_DIAG_ROW_COUNT 字段将设置为行计数，而行计数则以与实现相关的方式缓存。 **SQLRowCount** 返回缓存的行计数值。 在将语句句柄设置回准备就绪或已分配状态、语句被重新进行或调用 **SQLCloseCursor** 之前，缓存行计数值有效。 请注意，如果在设置 SQL_DIAG_ROW_COUNT 字段后调用了函数，则 **SQLRowCount** 返回的值可能与 SQL_DIAG_ROW_COUNT 字段中的值不同，因为 SQL_DIAG_ROW_COUNT 字段由任何函数调用重置为0。  
  
 对于其他语句和函数，驱动程序可能会定义在 RowCountPtr 中返回的值 \* 。 例如，在提取行之前，某些数据源可能能够返回 **SELECT** 语句或目录函数返回的行数。  
  
> [!NOTE]  
>  在提取结果集中的多个数据源之前，无法返回结果集中的行数;为了实现最大的互操作性，应用程序不应依赖于这种行为。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLRowCount** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLRowCount** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLRowCount** 函数时，此异步函数仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。<br /><br />  (DM) 在调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** for *StatementHandle* 之前调用了该函数。<br /><br />  (DM) 为 *StatementHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 如果对语句句柄执行的最后一个 SQL 语句不是 **UPDATE**、 **INSERT** 或 **DELETE** 语句，或者对 **SQLBulkOperations** 的前一次调用中的 *操作* 参数不是 SQL_ADD、SQL_UPDATE_BY_BOOKMARK 或 SQL_DELETE_BY_BOOKMARK，或者以前对 **SQLSetPos** 的调用中的 *操作* 参数不是 SQL_UPDATE 还是 SQL_DELETE，则 **RowCountPtr* 的值是由驱动程序定义的。 有关详细信息，请参阅 [确定受影响的行数](../../../odbc/reference/develop-app/determining-the-number-of-affected-rows.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|执行 SQL 语句|[SQLExecDirect 函数](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|执行已准备的 SQL 语句|[SQLExecute 函数](../../../odbc/reference/syntax/sqlexecute-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
