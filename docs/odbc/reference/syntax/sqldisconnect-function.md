---
description: SQLDisconnect 函数
title: SQLDisconnect 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDisconnect
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLDisconnect
helpviewer_keywords:
- SQLDisconnect function [ODBC]
ms.assetid: 9e84a58e-db48-4821-a0cd-5c711fcbe36b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a5f246c3d5aafbc02a138e809a0a07805f8ae30f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165319"
---
# <a name="sqldisconnect-function"></a>SQLDisconnect 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLDisconnect** 关闭与特定连接句柄关联的连接。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLDisconnect(  
     SQLHDBC     ConnectionHandle);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR、SQL_INVALID_HANDLE 或 SQL_STILL_EXECUTING。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLDisconnect** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DBC 和 *ConnectionHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLDisconnect** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|01002|断开连接错误|断开连接时出错。 但断开连接成功。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08003|连接未打开| (DM) 参数 *ConnectionHandle* 中指定的连接未打开。|  
|25000|事务状态无效|自变量 *ConnectionHandle* 指定的连接上存在事务。 事务仍处于活动状态。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY008|操作已取消|已为 *ConnectionHandle* 启用异步处理。 在 *ConnectionHandle* 上调用 [SQLCancelHandle 函数](../../../odbc/reference/syntax/sqlcancelhandle-function.md)之前，调用了函数并在该函数之前调用了已完成。 然后，在 *ConnectionHandle* 上再次调用该函数。<br /><br /> 调用了函数，并且在完成执行 **SQLCancelHandle** 之前，从多线程应用程序中的另一个线程调用了 *ConnectionHandle* 。|  
|HY010|函数序列错误| (DM) 为与 *ConnectionHandle* 关联的 *StatementHandle* 调用了异步执行的函数，并且在调用 **SQLDisconnect** 时仍在执行。<br /><br />  (DM) 异步执行的函数 (不是为 *ConnectionHandle* 调用了这一) ，并且在调用此函数时仍在执行。<br /><br /> 为与 *StatementHandle* 关联的 *ConnectionHandle* 调用了 (DM) **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前已过期，连接仍处于活动状态。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *ConnectionHandle* 关联的驱动程序不支持该函数。|  
|IM017|在异步通知模式下禁用轮询|无论何时使用通知模型，都将禁用轮询。|  
|IM018|尚未调用 **SQLCompleteAsync** 来完成此句柄上先前的异步操作。|如果句柄上的上一个函数调用返回 SQL_STILL_EXECUTING 并且启用了通知模式，则必须在句柄上调用 **SQLCompleteAsync** ，以执行后处理并完成操作。|  
  
## <a name="comments"></a>注释  
 如果应用程序在 **SQLBrowseConnect** 返回 SQL_NEED_DATA 之后和返回不同的返回代码之前调用 **SQLDisconnect** ，则驱动程序将取消连接浏览进程，并将连接返回到未连接状态。  
  
 如果应用程序在存在与连接句柄关联的不完整事务的情况下调用 **SQLDisconnect** ，则驱动程序将返回 SQLSTATE 25000 (无效事务状态) ，这指示该事务不变且连接处于打开状态。 未完成的事务是指尚未使用 **SQLEndTran** 提交或回滚的事务。  
  
 如果应用程序在释放与该连接相关联的所有语句之前调用 **SQLDisconnect** ，则该驱动程序在成功断开与数据源的连接后，将释放已在该连接上显式分配的这些语句和所有描述符。 但是，如果与连接关联的一个或多个语句仍以异步方式执行，则 **SQLDisconnect** 将返回 SQLSTATE 值为 HY010 (函数序列错误) SQL_ERROR。 此外，如果连接处于挂起状态，或者 **SQLCancelHandle** 成功取消了 **SQLDisconnect** ，则 **SQLDisconnect** 将释放所有关联的语句和已在该连接上显式分配的所有描述符。  
  
 有关应用程序如何使用 **SQLDisconnect** 的信息，请参阅 [与数据源或驱动程序断开连接](../../../odbc/reference/develop-app/disconnecting-from-a-data-source-or-driver.md)。  
  
## <a name="disconnecting-from-a-pooled-connection"></a>断开与池连接的连接  
 如果为共享环境启用了连接池，并且应用程序对该环境中的连接调用 **SQLDisconnect** ，则连接将返回到连接池，并且仍可用于使用相同共享环境的其他组件。  
  
## <a name="code-example"></a>代码示例  
 请参阅 [示例 ODBC Program](../../../odbc/reference/sample-odbc-program.md)、 [SQLBrowseConnect 函数](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)和 [SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)。  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|连接到数据源|[SQLConnect 函数](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|使用连接字符串或对话框连接到数据源|[SQLDriverConnect 函数](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|执行提交或回滚操作|[SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)|  
|释放连接句柄|[SQLFreeConnect 函数](../../../odbc/reference/syntax/sqlfreeconnect-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
