---
description: 异步执行（轮询方法）
title: " (轮询方法) 的异步执行Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- asynchronous execution [ODBC]
ms.assetid: 8cd21734-ef8e-4066-afd5-1f340e213f9c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b2767a89347329ee084c8b055bcb444dc4e78117
ms.sourcegitcommit: 80701484b8f404316d934ad2a85fd773e26ca30c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93243526"
---
# <a name="asynchronous-execution-polling-method"></a>异步执行（轮询方法）
在 ODBC 3.8 和 Windows 7 SDK 之前，只允许对语句函数执行异步操作。 有关详细信息，请参阅本主题后面的 **异步执行语句操作** 。  
  
 Windows 7 SDK 中的 ODBC 3.8 介绍了与连接相关的操作的异步执行。 有关详细信息，请参阅本主题后面的 **异步执行连接操作** 部分。  
  
 在 Windows 7 SDK 中，对于异步语句或连接操作，应用程序确定使用轮询方法完成了异步操作。 从 Windows 8 SDK 开始，你可以使用通知方法确定异步操作是否已完成。 有关详细信息，请参阅 [异步执行 (通知方法) ](../../../odbc/reference/develop-app/asynchronous-execution-notification-method.md)。  
  
 默认情况下，驱动程序同步执行 ODBC 函数;也就是说，应用程序将调用一个函数，并且在完成执行函数之前，驱动程序不会将控制返回给应用程序。 但是，某些函数可以异步执行;也就是说，应用程序调用函数，并且驱动程序在最小处理后会将控制权返回给应用程序。 然后，应用程序可以在第一个函数仍在执行时调用其他函数。  
  
 大多数在数据源上执行的函数都支持异步执行，如用于建立连接、准备和执行 SQL 语句、检索元数据、提取数据和提交事务的函数。 如果要对数据源执行的任务需要很长时间，例如登录过程或针对大型数据库的复杂查询，则此方法最有用。  
  
 当应用程序使用已为异步处理启用的语句或连接执行函数时，驱动程序将执行最小数量的处理 (如检查) 错误的参数、对数据源进行的操作，并使用 SQL_STILL_EXECUTING 返回代码将控件返回给应用程序。 然后，应用程序执行其他任务。 若要确定异步函数何时完成，应用程序将使用与最初使用的参数相同的参数调用函数，从而定期轮询驱动程序。 如果函数仍在执行，它将返回 SQL_STILL_EXECUTING;如果它已完成执行，则它将返回其返回的代码（例如 SQL_SUCCESS、SQL_ERROR 或 SQL_NEED_DATA）。  
  
 函数是同步执行还是异步执行特定于驱动程序。 例如，假设结果集元数据缓存在驱动程序中。 在这种情况下，执行 **SQLDescribeCol** 所需的时间很短，驱动程序只需执行函数而不是人为延迟执行。 另一方面，如果驱动程序需要从数据源检索元数据，则应在执行此操作时将控件返回给应用程序。 因此，应用程序必须能够在第一次异步执行函数时，处理除 SQL_STILL_EXECUTING 以外的返回代码。  
  
## <a name="executing-statement-operations-asynchronously"></a>异步执行语句操作  
 以下语句函数对数据源执行操作，并且可以异步执行：  

:::row:::
    :::column:::
        [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)  
        [SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)  
        [SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)  
        [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)  
        [SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)  
        [SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)  
        [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)  
        [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)  
        [SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)  
    :::column-end:::
    :::column:::
        [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)  
        [SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)  
        [SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)  
        [SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)  
        [SQLMoreResults](../../../odbc/reference/syntax/sqlmoreresults-function.md)  
        [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)  
        [SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)  
        [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)  
        [SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)  
    :::column-end:::
    :::column:::
        [SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)  
        [SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)  
        [SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)  
        [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)  
        [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)  
        [SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)  
        [SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)  
        [SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)  
        [SQLTables](../../../odbc/reference/syntax/sqltables-function.md)  
    :::column-end:::
:::row-end:::

 异步语句执行基于每个语句或每个连接进行控制，具体取决于数据源。 也就是说，应用程序指定不以异步方式执行特定函数，但对特定语句执行的任何函数都将以异步方式执行。 若要找出受支持的应用程序，应用程序将使用 SQL_ASYNC_MODE 的选项调用 [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) 。 如果支持语句句柄) 的连接级异步执行 (，则返回 SQL_AM_CONNECTION;SQL_AM_STATEMENT 是否支持语句级别的异步执行。  
  
 若要指定要以异步方式执行使用特定语句执行的函数，应用程序需要使用 SQL_ATTR_ASYNC_ENABLE 特性调用 **SQLSetStmtAttr** ，并将其设置为 SQL_ASYNC_ENABLE_ON。 如果支持连接级异步处理，则 SQL_ATTR_ASYNC_ENABLE 语句特性为只读，其值与在其上分配语句的连接的连接属性相同。 它是特定于驱动程序的，无论语句特性的值是在语句分配时间还是之后设置的。 尝试设置它将返回 SQL_ERROR 和 SQLSTATE HYC00 (未) 实现的可选功能。  
  
 若要指定要以异步方式执行使用特定连接执行的函数，应用程序需要使用 SQL_ATTR_ASYNC_ENABLE 特性调用 **SQLSetConnectAttr** ，并将其设置为 SQL_ASYNC_ENABLE_ON。 将为该连接上已分配的所有未来语句句柄启用异步执行;它是驱动程序定义的，此操作是否将启用现有的语句句柄。 如果 SQL_ATTR_ASYNC_ENABLE 设置为 SQL_ASYNC_ENABLE_OFF，则连接上的所有语句都处于同步模式。 如果在连接上存在活动语句时启用了异步执行，则会返回错误。  
  
 若要确定在给定连接上驱动程序可以支持的异步模式下活动并发语句的最大数量，应用程序需要使用 SQL_MAX_ASYNC_CONCURRENT_STATEMENTS 选项调用 **SQLGetInfo** 。  
  
 下面的代码演示了轮询模型的工作方式：  
  
```  
SQLHSTMT  hstmt1;  
SQLRETURN rc;  
  
// Specify that the statement is to be executed asynchronously.  
SQLSetStmtAttr(hstmt1, SQL_ATTR_ASYNC_ENABLE, SQL_ASYNC_ENABLE_ON, 0);  
  
// Execute a SELECT statement asynchronously.  
while ((rc=SQLExecDirect(hstmt1,"SELECT * FROM Orders",SQL_NTS))==SQL_STILL_EXECUTING) {  
   // While the statement is still executing, do something else.  
   // Do not use hstmt1, because it is being used asynchronously.  
}  
  
// When the statement has finished executing, retrieve the results.  
```  
  
 当函数以异步方式执行时，应用程序可以对任何其他语句调用函数。 应用程序还可以对任何连接调用函数，与异步语句关联的函数除外。 但是，在语句操作返回 SQL_STILL_EXECUTING 之后，应用程序只能调用原始函数和以下函数 (语句句柄或其关联的连接、环境句柄) ：  
  
-   [SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)  
  
-   语句句柄上的[SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) ()   
  
-   [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)  
  
-   [SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)  
  
-   [SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)  
  
-   [SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)  
  
-   [SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)  
  
-   [SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)  
  
-   [SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)  
  
-   [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)  
  
-   [SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md)  
  
-   [SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)  
  
 例如，如果应用程序调用带有异步语句的任何其他函数或与该语句关联的连接，则函数返回 SQLSTATE HY010 (函数序列) 错误，例如。  
  
```  
SQLHDBC       hdbc1, hdbc2;  
SQLHSTMT      hstmt1, hstmt2, hstmt3;  
SQLCHAR *     SQLStatement = "SELECT * FROM Orders";  
SQLUINTEGER   InfoValue;  
SQLRETURN     rc;  
  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt2);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc2, &hstmt3);  
  
// Specify that hstmt1 is to be executed asynchronously.  
SQLSetStmtAttr(hstmt1, SQL_ATTR_ASYNC_ENABLE, SQL_ASYNC_ENABLE_ON, 0);  
  
// Execute hstmt1 asynchronously.  
while ((rc = SQLExecDirect(hstmt1, SQLStatement, SQL_NTS)) == SQL_STILL_EXECUTING) {  
   // The following calls return HY010 because the previous call to  
   // SQLExecDirect is still executing asynchronously on hstmt1. The  
   // first call uses hstmt1 and the second call uses hdbc1, on which  
   // hstmt1 is allocated.  
   SQLExecDirect(hstmt1, SQLStatement, SQL_NTS);   // Error!  
   SQLGetInfo(hdbc1, SQL_UNION, (SQLPOINTER) &InfoValue, 0, NULL);   // Error!  
  
   // The following calls do not return errors. They use a statement  
   // handle other than hstmt1 or a connection handle other than hdbc1.  
   SQLExecDirect(hstmt2, SQLStatement, SQL_NTS);   // OK  
   SQLTables(hstmt3, NULL, 0, NULL, 0, NULL, 0, NULL, 0);   // OK  
   SQLGetInfo(hdbc2, SQL_UNION, (SQLPOINTER) &InfoValue, 0, NULL);   // OK  
}  
```  
  
 当应用程序调用函数来确定它是否仍以异步方式执行时，它必须使用原始语句句柄。 这是因为，将基于每个语句跟踪异步执行。 该应用程序还必须为其他参数提供有效值，原始参数将在驱动程序管理器中进行以前的错误检查。 但是，驱动程序检查语句句柄并确定语句是异步执行的，它将忽略所有其他参数。  
  
 当函数以异步方式执行时，即在返回 SQL_STILL_EXECUTING 之后，在返回不同的代码之前，应用程序可以通过使用相同的语句句柄调用 **SQLCancel** 或 **SQLCancelHandle** 来取消它。 这不能保证取消函数执行。 例如，函数可能已经完成。 此外， **SQLCancel** 或 **SQLCancelHandle** 返回的代码仅指示是否已成功取消函数，而不是实际取消函数。 若要确定是否已取消该函数，应用程序将再次调用该函数。 如果该函数被取消，则将返回 SQL_ERROR 和 SQLSTATE HY008 (操作取消) 。 如果未取消该函数，它将返回其他代码，如 SQL_SUCCESS、SQL_STILL_EXECUTING 或 SQL_ERROR 不同的 SQLSTATE。  
  
 若要在驱动程序支持语句级异步处理时禁用特定语句的异步执行，应用程序将使用 SQL_ATTR_ASYNC_ENABLE 特性调用 **SQLSetStmtAttr** ，并将其设置为 SQL_ASYNC_ENABLE_OFF。 如果驱动程序支持连接级别的异步处理，则应用程序会调用 **SQLSetConnectAttr** 将 SQL_ATTR_ASYNC_ENABLE 设置为 SQL_ASYNC_ENABLE_OFF，这将禁用连接上的所有语句的异步执行。  
  
 应用程序应在原始函数的重复循环中处理诊断记录。 如果在执行异步函数时调用 **SQLGetDiagField** 或 **SQLGetDiagRec** ，则它将返回诊断记录的当前列表。 每次重复原始函数调用时，它会清除以前的诊断记录。  
  
## <a name="executing-connection-operations-asynchronously"></a>以异步方式执行连接操作  
 在 ODBC 3.8 之前，允许异步执行与语句相关的操作，例如准备、执行和提取，以及用于目录元数据操作。 从 ODBC 3.8 开始，异步执行也可用于连接相关的操作，如连接、断开连接、提交和回滚。  
  
 有关 ODBC 3.8 的详细信息，请参阅 [odbc 3.8 中的新增功能](../../../odbc/reference/what-s-new-in-odbc-3-8.md)。  
  
 以异步方式执行连接操作在以下情况下非常有用：  
  
-   当少量线程管理大量具有非常高的数据速率的设备时。 若要最大程度地提高响应能力和可伸缩性，所有操作都是异步的。  
  
-   希望在多个连接上重叠数据库操作，以减少已用的传输时间。  
  
-   有效的异步 ODBC 调用和取消连接操作的功能使应用程序能够允许用户取消任何慢速操作，而无需等待超时。  
  
 现在可以异步执行以下函数，这些函数可对连接句柄执行操作：  

:::row:::
    :::column:::
        [SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)  
        [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)  
    :::column-end:::
    :::column:::
        [SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)  
        [SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)  
    :::column-end:::
    :::column:::
        [SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)  
        [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)  
    :::column-end:::
:::row-end:::

 若要确定驱动程序是否支持对这些函数的异步操作，应用程序需要使用 SQL_ASYNC_DBC_FUNCTIONS 调用 **SQLGetInfo** 。 如果支持异步操作，则返回 SQL_ASYNC_DBC_CAPABLE。 如果不支持异步操作，则返回 SQL_ASYNC_DBC_NOT_CAPABLE。  
  
 若要指定以异步方式执行使用特定连接执行的函数，应用程序将调用 **SQLSetConnectAttr** 并将 SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE 特性设置为 SQL_ASYNC_DBC_ENABLE_ON。 建立连接之前设置连接属性始终以同步方式执行。 此外，SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE 将连接属性设置为 " **SQLSetConnectAttr** " 的操作始终同步执行。  
  
 应用程序可以在建立连接前启用异步操作。 因为在建立连接之前，驱动程序管理器无法确定要使用的驱动程序，所以，驱动程序管理器将始终在 **SQLSetConnectAttr** 中返回 success。 但是，如果 ODBC 驱动程序不支持异步操作，则可能无法连接。  
  
 通常，最多可以有一个与特定连接句柄或语句句柄关联的异步执行函数。 但是，一个连接句柄可以有多个关联的语句句柄。 如果在连接句柄上没有执行异步操作，则关联语句句柄可执行异步操作。 同样，如果任何关联语句句柄上都没有正在进行的异步操作，则可以在连接句柄上执行异步操作。 如果尝试使用当前正在执行异步操作的句柄执行异步操作，将返回 HY010，"函数序列错误"。  
  
 如果连接操作返回 SQL_STILL_EXECUTING，则应用程序只能调用该连接句柄的原始函数和以下函数：  
  
-   连接句柄上的 **SQLCancelHandle** ()   
  
-   **SQLGetDiagField**  
  
-   **SQLGetDiagRec**  
  
-   分配 ENV/DBC) 的 **SQLAllocHandle** (  
  
-   分配 ENV/DBC) 的 **SQLAllocHandleStd** (  
  
-   **SQLGetEnvAttr**  
  
-   **SQLGetConnectAttr**  
  
-   **SQLDataSources**  
  
-   **SQLDrivers**  
  
-   **SQLGetInfo**  
  
-   **SQLGetFunctions**  
  
 应用程序应在原始函数的重复循环中处理诊断记录。 如果在执行异步函数时调用 SQLGetDiagField 或 SQLGetDiagRec，则它将返回诊断记录的当前列表。 每次重复原始函数调用时，它会清除以前的诊断记录。  
  
 如果以异步方式打开或关闭连接，则当应用程序接收到原始函数调用中的 SQL_SUCCESS 或 SQL_SUCCESS_WITH_INFO 时，操作即完成。  
  
 新函数已添加到 ODBC 3.8， **SQLCancelHandle** 。 此函数取消 ( **SQLBrowseConnect** 、 **SQLConnect** 、 **SQLDisconnect** 、 **SQLDriverConnect** 、 **SQLEndTran** 和 **SQLSetConnectAttr** ) 的六个连接函数。 应用程序应调用 **SQLGetFunctions** 来确定驱动程序是否支持 **SQLCancelHandle** 。 与 **SQLCancel** 一样，如果 **SQLCancelHandle** 返回成功，则并不意味着该操作已取消。 应用程序应再次调用原始函数以确定操作是否已取消。 **SQLCancelHandle** 可让你取消对连接句柄或语句句柄的异步操作。 使用 **SQLCancelHandle** 取消对语句句柄的操作与调用 **SQLCancel** 相同。  
  
 不需要同时支持 **SQLCancelHandle** 和异步连接操作。 驱动程序可以支持异步连接操作，但不支持 **SQLCancelHandle** ，反之亦然。  
  
 ODBC 1.x 和 ODBC 2.x 应用程序也可以使用 ODBC 3.8 驱动程序和 ODBC 3.8 驱动程序管理器来使用异步连接操作和 **SQLCancelHandle** 。 有关如何使较旧的应用程序在更高版本的 ODBC 版本中使用新功能的信息，请参阅 [兼容性矩阵](../../../odbc/reference/develop-app/compatibility-matrix.md)。  
  
### <a name="connection-pooling"></a>连接池  
 只要启用了连接池，就只支持使用 **SQLConnect** 和 **SQLDriverConnect** ) 建立连接 (，并使用 **SQLDisconnect** 关闭连接。 但应用程序仍然能够处理来自 **SQLConnect** 、 **SQLDriverConnect** 和 **SQLDisconnect** 的 SQL_STILL_EXECUTING 返回值。  
  
 启用连接池时，支持 **SQLEndTran** 和 **SQLSetConnectAttr** 异步操作。  
  
## <a name="examples"></a>示例  
  
### <a name="a-enable-asynchronous-execution-of-connection-functions"></a>A. 启用连接函数的异步执行

 下面的示例演示如何使用 **SQLSetConnectAttr** 来启用与连接相关的函数的异步执行。  
  
```  
BOOL AsyncConnect (SQLHANDLE hdbc)   
{  
   SQLRETURN r;  
   SQLHANDLE hdbc;  
  
   // Enable asynchronous execution of connection functions.  
   // This must be executed synchronously, that is r != SQL_STILL_EXECUTING  
   r = SQLSetConnectAttr(  
         hdbc,   
         SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE,  
         reinterpret_cast<SQLPOINTER> (SQL_ASYNC_DBC_ENABLE_ON),  
         0);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO)   
   {  
      return FALSE;  
   }  
  
   TCHAR szConnStrIn[256] = _T("DSN=AsyncDemo");  
  
   r = SQLDriverConnect(hdbc, NULL, (SQLTCHAR *) szConnStrIn, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);  
  
   if (r == SQL_ERROR)   
   {  
      // Use SQLGetDiagRec to process the error.  
      // If SQLState is HY114, the driver does not support asynchronous execution.  
      return FALSE;  
   }  
  
   while (r == SQL_STILL_EXECUTING)   
   {  
      // Do something else.  
  
      // Check for completion, with the same set of arguments.  
      r = SQLDriverConnect(hdbc, NULL, (SQLTCHAR *) szConnStrIn, SQL_NTS, NULL, 0, NULL, SQL_DRIVER_NOPROMPT);  
   }  
  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO)   
   {  
      return FALSE;  
   }  
  
   return TRUE;  
}  
  
```  
  
### <a name="b-asynchronous-commit-operations"></a>B. 异步提交操作 

 此示例演示异步提交操作。 还可以通过此方法完成回滚操作。  
  
```  
BOOL AsyncCommit ()   
{  
   SQLRETURN r;   
  
   // Assume that SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE is SQL_ASYNC_DBC_ENABLE_ON.  
  
   r = SQLEndTran(SQL_HANDLE_DBC, hdbc, SQL_COMMIT);  
   while (r == SQL_STILL_EXECUTING)   
   {  
      // Do something else.  
  
      // Check for completion with the same set of arguments.  
      r = SQLEndTran(SQL_HANDLE_DBC, hdbc, SQL_COMMIT);  
   }  
  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO)   
   {  
      return FALSE;  
   }  
   return TRUE;  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [执行语句 ODBC](../../../odbc/reference/develop-app/executing-statements-odbc.md)
