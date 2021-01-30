---
description: SQLFreeHandle 函数
title: SQLFreeHandle 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeHandle
helpviewer_keywords:
- SQLFreeHandle function [ODBC]
ms.assetid: 17a6fcdc-b05a-4de7-be93-a316f39696a1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 14c649c894250abdcac54010c0d18444d20b3baa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201572"
---
# <a name="sqlfreehandle-function"></a>SQLFreeHandle 函数
**度**  
 引入的版本： ODBC 3.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLFreeHandle** 释放与特定环境、连接、语句或描述符句柄关联的资源。  
  
> [!NOTE]
>  此函数是用于释放句柄的泛型函数。 它取代了 ODBC 2.0 函数 **SQLFreeConnect** (以释放连接句柄) 并使用 **SQLFreeEnv** (释放环境句柄) 。 **SQLFreeConnect** 和 **SQLFREEENV** 已在 ODBC 2.x 中弃用 *。* **SQLFreeHandle** 还会将 ODBC 2.0 函数 **SQLFreeStmt** (替换为释放语句句柄) 的 "SQL_DROP" *选项* 。 有关详细信息，请参阅 "注释"。 有关 ODBC 3.x 应用程序使用 ODBC 2.x *驱动程序时*，驱动程序管理器将此函数映射到的内容的详细信息，请参阅 [为应用程序的向后兼容性映射替换函数](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md)。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLFreeHandle(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle);  
```  
  
## <a name="arguments"></a>参数  
 *HandleType*  
 送要由 **SQLFreeHandle** 释放的句柄的类型。 必须是以下值之一：  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN 句柄仅由驱动程序管理器和驱动程序使用。 应用程序不应使用此句柄类型。 有关 SQL_HANDLE_DBC_INFO_TOKEN 的详细信息，请参阅 [在 ODBC 驱动程序中开发 Connection-Pool 感知](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)。  
  
 如果 *HandleType* 不是这些值之一，则 **SQLFreeHandle** 将返回 SQL_INVALID_HANDLE。  
  
 *Handle*  
 送要释放的句柄。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
 如果 **SQLFreeHandle** 返回 SQL_ERROR，则句柄仍有效。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLFreeHandle** 返回 SQL_ERROR 时，可能会从 **SQLFreeHandle** 尝试释放的句柄的诊断数据结构中获取关联的 SQLSTATE 值，但无法获取该值。 下表列出了通常由 **SQLFreeHandle** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) *HandleType* 参数已 SQL_HANDLE_ENV，并且至少有一个连接处于已分配或已连接状态。 使用 SQL_HANDLE_ENV 的 *HandleType* 调用 **SQLFreeHandle** 之前，必须为每个连接调用 **SQLDisconnect** 和 **SQLFreeHandle** 的 SQL_HANDLE_DBC *HandleType* 。<br /><br />  (DM) 已 SQL_HANDLE_DBC *HandleType* 参数，并在为连接调用 **SQLDisconnect** 之前调用了该函数。<br /><br />  (DM) *HandleType* 参数已 SQL_HANDLE_DBC。 通过 *句柄* 调用了异步执行的函数，并且在调用此函数时，该函数仍在执行。<br /><br />  (DM) *HandleType* 参数已 SQL_HANDLE_STMT。 已通过语句句柄调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并 SQL_NEED_DATA 返回。 在为所有执行时数据参数或列发送数据之前，将调用此函数。<br /><br />  (DM) *HandleType* 参数已 SQL_HANDLE_STMT。 在语句句柄或关联的连接句柄上调用了异步执行的函数，并且在调用此函数时，该函数仍在执行。<br /><br />  (DM) *HandleType* 参数已 SQL_HANDLE_DESC。 在关联的连接句柄上调用了异步执行的函数;调用此函数时，该函数仍在执行。<br /><br /> 在调用 **SQLFreeHandle** 之前，不会释放所有子公司句柄和其他资源 (DM) 。<br /><br />  (DM) 为与 *句柄* 关联的某个语句句柄调用了 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并将 *HandleType* 设置为 SQL_HANDLE_STMT 或 SQL_HANDLE_DESC 返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|*HandleType* 参数 SQL_HANDLE_STMT 或 SQL_HANDLE_DESC，无法处理函数调用，原因可能是由于内存不足而导致无法访问基础内存对象。|  
|HY017|使用自动分配的描述符句柄无效。| (DM) *handle* 参数设置为自动分配的描述符的句柄。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) *HandleType* 参数已 SQL_HANDLE_DESC，*驱动程序为 ODBC 2.x 驱动程序* 。<br /><br />  (DM) *HandleType* 参数已 SQL_HANDLE_STMT，驱动程序不是有效的 ODBC 驱动程序。|  
  
## <a name="comments"></a>注释  
 **SQLFreeHandle** 用于释放环境、连接、语句和描述符的句柄，如以下各节所述。 有关句柄的一般信息，请参阅 [句柄](../../../odbc/reference/develop-app/handles.md)。  
  
 应用程序在释放后不应使用句柄;驱动程序管理器不会检查函数调用中的句柄的有效性。  
  
## <a name="freeing-an-environment-handle"></a>释放环境句柄  
 在使用 SQL_HANDLE_ENV 的 *HandleType* 调用 **SQLFreeHandle** 之前，应用程序必须为环境中分配的所有连接调用 **SQLFreeHandle** ，其 SQL_HANDLE_DBC 为 *HandleType* 。 否则，对 **SQLFreeHandle** 的调用将返回 SQL_ERROR 并且环境和任何活动连接将保持有效。 有关详细信息，请参阅 [环境](../../../odbc/reference/develop-app/environment-handles.md) 句柄和 [分配环境句柄](../../../odbc/reference/develop-app/allocating-the-environment-handle.md)。  
  
 如果环境是共享环境，则调用 **SQLFreeHandle** 的应用程序（ *HandleType* 为 SQL_HANDLE_ENV）在调用后不再有权访问该环境，但不一定会释放该环境的资源。 对 **SQLFreeHandle** 的调用将减少环境的引用计数。 引用计数由驱动程序管理器维护。 如果不是零，则不会释放共享环境，因为另一个组件仍在使用它。 如果引用计数为零，则释放共享环境的资源。  
  
## <a name="freeing-a-connection-handle"></a>释放连接句柄  
 在使用 SQL_HANDLE_DBC 的 *HandleType* 调用 **SQLFreeHandle** 之前，如果此句柄上有连接，则应用程序必须调用 **SQLDisconnect** *。* 否则，对 **SQLFreeHandle** 的调用将返回 SQL_ERROR 并且连接保持有效。  
  
 有关详细信息，请参阅 [连接处理](../../../odbc/reference/develop-app/connection-handles.md) 和 [断开数据源或驱动程序](../../../odbc/reference/develop-app/disconnecting-from-a-data-source-or-driver.md)的连接。  
  
## <a name="freeing-a-statement-handle"></a>释放语句句柄  
 对 **SQLFreeHandle** 的调用 *HandleType* 为 SQL_HANDLE_STMT 将释放对 **SQLAllocHandle** 的调用分配的所有资源， *HandleType* 的 SQL_HANDLE_STMT。 当应用程序调用 **SQLFreeHandle** 来释放具有挂起结果的语句时，将删除挂起的结果。 当应用程序释放语句句柄时，驱动程序将释放与该句柄关联的四个自动分配的描述符。 有关详细信息，请参阅 [语句句](../../../odbc/reference/develop-app/statement-handles.md) 柄和 [释放语句句柄](../../../odbc/reference/develop-app/freeing-a-statement-handle-odbc.md)。  
  
 请注意， **SQLDisconnect** 会自动删除连接上打开的所有语句和说明符。  
  
## <a name="freeing-a-descriptor-handle"></a>释放描述符句柄  
 使用 SQL_HANDLE_DESC 的 *HandleType* 调用 **SQLFreeHandle** 将释放 *句柄* 中的描述符句柄。 对 **SQLFreeHandle** 的调用不会释放由指针字段引用的应用程序分配的任何内存， (包括 SQL_DESC_DATA_PTR、SQL_DESC_INDICATOR_PTR 和 SQL_DESC_OCTET_LENGTH_PTR *句柄* 的任何说明符记录的) 。 释放句柄时，将释放由不是指针字段的字段的驱动程序分配的内存。 释放用户分配的描述符句柄时，已释放的句柄关联的所有语句都将恢复到其各自的自动分配的描述符句柄。  
  
> [!NOTE]
>  ODBC 2.x *驱动程序* 不支持释放说明符句柄，正如它们不支持分配描述符句柄。  
  
 请注意， **SQLDisconnect** 会自动删除连接上打开的所有语句和说明符。 当应用程序释放语句句柄时，驱动程序将释放与该句柄关联的所有自动生成的说明符。  
  
 有关描述符的详细信息，请参阅 [描述符](../../../odbc/reference/develop-app/descriptors.md)。  
  
## <a name="code-example"></a>代码示例  
 有关其他代码示例，请参阅 [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md) 和 [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)。  
  
### <a name="code"></a>代码  
  
```cpp  
// SQLFreeHandle.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
#include <stdio.h>  
  
int main() {  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
  
   // Initialize the environment, connection, statement handles.  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   // Allocate the environment.  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set environment attributes.  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
  
   // Allocate the connection.  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
   // Set the login timeout.  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
  
   // Let the user select the data source and connect to the database.  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // Free handles, and disconnect.     
   if (hstmt) {   
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
      hstmt = NULL;   
   }  
   if (hdbc) {   
      SQLDisconnect(hdbc);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      hdbc = NULL;   
   }  
   if (henv) {   
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
      henv = NULL;   
   }  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|正在取消语句处理|[SQLCance Functionl](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|设置游标名称|[SQLSetCursorName 函数](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)   
 [示例 ODBC 程序](../../../odbc/reference/sample-odbc-program.md)
