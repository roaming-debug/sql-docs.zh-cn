---
description: SQLFreeStmt 函数
title: SQLFreeStmt 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLFreeStmt
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLFreeStmt
helpviewer_keywords:
- SQLFreeStmt function [ODBC]
ms.assetid: 03408162-8b63-4470-90c4-e6c7d8d33892
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b0dcab0413f353b4c600964dd38818df37f86bfe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201558"
---
# <a name="sqlfreestmt-function"></a>SQLFreeStmt 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLFreeStmt** 停止与特定语句关联的处理、关闭与该语句关联的任何打开的游标、放弃挂起的结果，或者，还可以释放与该语句句柄关联的所有资源。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLFreeStmt(  
     SQLHSTMT       StatementHandle,  
     SQLUSMALLINT   Option);  
```  
  
## <a name="arguments"></a>参数  
 *StatementHandle*  
 送语句句柄  
  
 *选项*  
 送以下选项之一：  
  
 SQL_ 关闭：如果) 定义了一个游标，则关闭与该 (*StatementHandle* 关联的游标，并放弃所有挂起的结果。 稍后，应用程序可以使用相同或不同的参数值再次执行 **SELECT** 语句来重新打开此游标。 如果没有打开的游标，则此选项不会影响应用程序。 还可以调用 **SQLCloseCursor** 以关闭游标。 有关详细信息，请参阅 [关闭光标](../../../odbc/reference/develop-app/closing-the-cursor.md)。  
  
 SQL_DROP：不推荐使用此选项。 使用 SQL_DROP *选项* 的调用 **SQLFreeStmt** 在驱动程序管理器中映射到 [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)。  
  
 SQL_UNBIND：将 ARD 的 SQL_DESC_COUNT 字段设置为0，释放由 **SQLBindCol** 绑定的针对给定 *StatementHandle* 的所有列缓冲区。 这不取消绑定书签列;为此，"书签" 列 ARD 的 "SQL_DESC_DATA_PTR" 字段设置为 NULL。 请注意，如果在由多个语句共享的显式分配的描述符上执行此操作，则该操作将影响所有共享描述符的语句的绑定。 有关详细信息，请参阅 [检索结果 (基本) 的概述 ](../../../odbc/reference/develop-app/retrieving-results-basic.md)。  
  
 SQL_RESET_PARAMS：将 APD 的 SQL_DESC_COUNT 字段设置为0，释放由 **SQLBindParameter** 为给定的 *StatementHandle* 设置的所有参数缓冲区。 如果在由多个语句共享的显式分配的描述符上执行此操作，此操作将影响共享该描述符的所有语句的绑定。 有关详细信息，请参阅 [绑定参数](../../../odbc/reference/develop-app/binding-parameters-odbc.md)。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLFreeStmt** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_STMT 和 *StatementHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLFreeStmt** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------------|-----------|-----------------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) 为与 *StatementHandle* 关联的连接句柄调用了异步执行函数。 调用 **SQLFreeStmt** 时仍在执行此异步函数。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在为所有流式处理参数检索数据之前，调用此函数时， *选项* 设置为 SQL_RESET_PARAMS。<br /><br />  (DM) 为 *StatementHandle* 调用了异步执行函数，并且在调用此函数时仍在执行。<br /><br />  (DM) 为 *StatementHandle* 调用 **SQLExecute**、 **SQLExecDirect**、 **SQLBulkOperations** 或 **SQLSetPos** ，并返回 SQL_NEED_DATA。 在为所有执行时数据参数或列发送数据之前，将调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY092|选项类型超出范围| (DM) 为参数 *选项* 指定的值不是：<br /><br /> SQL_CLOSE SQL_DROP SQL_UNBIND SQL_RESET_PARAMS|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
|IM001|驱动程序不支持此功能| (DM) 与 *StatementHandle* 关联的驱动程序不支持该函数。|  
  
## <a name="comments"></a>注释  
 与 SQL_CLOSE 选项一起调用 **SQLFreeStmt** 等效于调用 **SQLCloseCursor**，只不过如果语句上没有打开的游标，则具有 SQL_CLOSE 的 **SQLFreeStmt** 不会影响应用程序。 如果没有打开的游标，则对 **SQLCloseCursor** 的调用将返回 SQLSTATE 24000 (无效的游标状态) 。  
  
 应用程序在释放后不应使用语句句柄;驱动程序管理器不会检查函数调用中的句柄的有效性。  
  
## <a name="example"></a>示例  
 自由句柄是一种很好的编程做法。 但是，为简单起见，下面的示例不包含用于释放已分配的句柄的代码。 有关如何释放句柄的示例，请参阅 [SQLFreeHandle 函数](../../../odbc/reference/syntax/sqlfreehandle-function.md)。  
  
```cpp  
// SQLFreeStmt.cpp  
// compile with: user32.lib odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   // declare and initialize the environment, connection, statement handles  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   SQLRETURN retCode;  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen;  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, (SQLCHAR *)"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1024 + 1, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
   retCode = SQLFreeStmt(hstmt, SQL_UNBIND);  
   retCode = SQLFreeStmt(hstmt, SQL_RESET_PARAMS);  
}  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|分配句柄|[SQLAllocHandle 函数](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|正在取消语句处理|[SQLCancel 函数](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|关闭游标|[SQLCloseCursor 函数](../../../odbc/reference/syntax/sqlclosecursor-function.md)|  
|释放句柄|[SQLFreeHandle 函数](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|设置游标名称|[SQLSetCursorName 函数](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
