---
description: SQLGetFunctions 函数
title: SQLGetFunctions 函数 |Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetFunctions
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetFunctions
helpviewer_keywords:
- SQLGetFunctions function [ODBC]
ms.assetid: 0451d2f9-0f4f-46ba-b252-670956a52183
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: edc7b7875e3ee3f7512ee19a62d68a50a1617436
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081358"
---
# <a name="sqlgetfunctions-function"></a>SQLGetFunctions 函数
**度**  
 引入的版本： ODBC 1.0 标准符合性： ISO 92  
  
 **摘要**  
 **SQLGetFunctions** 返回有关驱动程序是否支持特定 ODBC 函数的信息。 此函数在驱动程序管理器中实现;它还可以在驱动程序中实现。 如果驱动程序实现 **SQLGetFunctions**，则驱动程序管理器将调用驱动程序中的函数。 否则，它将执行函数本身。  
  
## <a name="syntax"></a>语法  
  
```cpp  
  
SQLRETURN SQLGetFunctions(  
     SQLHDBC           ConnectionHandle,  
     SQLUSMALLINT      FunctionId,  
     SQLUSMALLINT *    SupportedPtr);  
```  
  
## <a name="arguments"></a>参数  
 *ConnectionHandle*  
 [输入] 连接句柄。  
  
 *FunctionId*  
 送标识相关 ODBC 函数的 **#define** 值; **SQL_API_ODBC3_ALL_FUNCTIONS orSQL_API_ALL_FUNCTIONS**。 ODBC *3.x 应用程序* 使用 **SQL_API_ODBC3_ALL_FUNCTIONS** 来确定对 odbc *2.x 和更* 早版本函数的支持。 ODBC *2.x 应用程序* 使用 **SQL_API_ALL_FUNCTIONS** 来确定对 odbc *2.x 和更* 早版本函数的支持。  
  
 有关标识 ODBC 函数的 **#define** 值的列表，请参阅 "备注" 中的表。  
  
 *SupportedPtr*  
 输出 如果 *FunctionId* 标识单个 ODBC 函数，则 *SUPPORTEDPTR* 指向单个 SQLUSMALLINT 值，如果驱动程序支持指定的函数，则此值为 SQL_TRUE，如果不支持，则 SQL_FALSE。  
  
 如果 SQL_API_ODBC3_ALL_FUNCTIONS *FunctionId* ，则 *SupportedPtr* 会指向一个 SQLSMALLINT 数组，该数组的元素数等于 SQL_API_ODBC3_ALL_FUNCTIONS_SIZE。 驱动程序管理器将此数组视为4000位位图，该位图可用于确定是否支持 ODBC *1.x 或更* 早的函数。 调用 SQL_FUNC_EXISTS 宏来确定函数支持。  (参阅 "注释"。) ODBC *2.x 应用程序* 可以对 odbc *1.x 或 odbc* 2.x 驱动程序使用 SQL_API_ODBC3_ALL_FUNCTIONS 调用 **SQLGetFunctions** *。*  
  
 如果 SQL_API_ALL_FUNCTIONS *FunctionId* ，则 *SupportedPtr* 指向100元素的 SQLUSMALLINT 数组。 数组由 *FunctionId* 用来标识每个 ODBC 函数的 **#define** 值编制索引;数组的某些元素是未使用的，并保留以供将来使用。 如果元素标识驱动程序支持的 ODBC *2.x 或更* 早版本函数，则 SQL_TRUE 元素。 如果它标识驱动程序不支持的 ODBC 函数或未识别 ODBC 函数，则 SQL_FALSE。  
  
 **SupportedPtr* 中返回的数组使用从零开始的索引。  
  
## <a name="returns"></a>返回  
 SQL_SUCCESS、SQL_SUCCESS_WITH_INFO、SQL_ERROR 或 SQL_INVALID_HANDLE。  
  
## <a name="diagnostics"></a>诊断  
 当 **SQLGetFunctions** 返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 时，可以通过使用 *HandleType* 的 SQL_HANDLE_DBC 和 *ConnectionHandle* 的 *句柄* 调用 **SQLGetDiagRec** 来获取关联的 SQLSTATE 值。 下表列出了通常由 **SQLGetFunctions** 返回的 SQLSTATE 值，并对该函数的上下文中的每个值进行了说明：表示法 " (DM) " 位于驱动程序管理器返回的 SQLSTATEs 的说明之前。 除非另有说明，否则与每个 SQLSTATE 值相关联的返回代码将 SQL_ERROR。  
  
|SQLSTATE|错误|说明|  
|--------|-----|-----------|  
|01000|一般警告|驱动程序特定的信息性消息。  (函数返回 SQL_SUCCESS_WITH_INFO。 ) |  
|08S01|通信链接失败|在函数完成处理之前，驱动程序与连接到的数据源之间的通信链接失败。|  
|HY000|常规错误|发生了一个错误，该错误没有特定的 SQLSTATE，没有为其定义实现特定的 SQLSTATE。 *\* MessageText* 缓冲区中的 **SQLGetDiagRec** 返回的错误消息描述了错误及其原因。|  
|HY001|内存分配错误|驱动程序无法分配支持执行或完成此函数所需的内存。|  
|HY010|函数序列错误| (DM) 在 **SQLConnect**、 **SQLBrowseConnect** 或 **SQLDriverConnect** 之前调用 **SQLGetFunctions** 。<br /><br /> 为 *ConnectionHandle* 调用了 (DM) **SQLBrowseConnect** ，并返回 SQL_NEED_DATA。 此函数在 **SQLBrowseConnect** 返回 SQL_SUCCESS_WITH_INFO 或 SQL_SUCCESS 之前被调用。<br /><br />  (DM) 为 *ConnectionHandle* 调用 **SQLExecute**、 **SQLExecDirect** 或 **SQLMoreResults** ，并返回 SQL_PARAM_DATA_AVAILABLE。 在检索所有流式处理参数的数据之前调用此函数。|  
|HY013|内存管理错误|未能处理函数调用，原因可能是由于内存不足而无法访问基础内存对象。|  
|HY095|函数类型超出范围| (DM) 指定了无效的 *FunctionId* 值。|  
|HY117|由于未知的事务状态，连接被挂起。 仅允许断开连接和只读函数。| (DM) 有关挂起状态的详细信息，请参阅 [SQLEndTran 函数](../../../odbc/reference/syntax/sqlendtran-function.md)。|  
|HYT01|连接超时已过期|连接超时期限在数据源响应请求之前过期。 连接超时期限通过 **SQLSetConnectAttr** 设置，SQL_ATTR_CONNECTION_TIMEOUT。|  
  
## <a name="comments"></a>注释  
 **SQLGetFunctions** 始终返回，支持 **SQLGetFunctions**、 **SQLDataSources** 和 **SQLDrivers** 。 这是因为这些函数是在驱动程序管理器中实现的。 如果 Unicode 函数存在，驱动程序管理器会将 ANSI 函数映射到相应的 Unicode 函数，如果 ANSI 函数存在，则将 Unicode 函数映射到相应的 ANSI 函数。 有关应用程序如何使用 **SQLGetFunctions** 的信息，请参阅 [接口一致性级别](../../../odbc/reference/develop-app/interface-conformance-levels.md)。  
  
 以下列表列出了符合 ISO 92 标准符合性级别的函数的 *FunctionId* 的有效值：  
  
|FunctionId 值|FunctionId 值|  
|----------|----------|  
|SQL_API_SQLALLOCHANDLE|SQL_API_SQLGETDESCFIELD|  
|SQL_API_SQLBINDCOL|SQL_API_SQLGETDESCREC|  
|SQL_API_SQLCANCEL|SQL_API_SQLGETDIAGFIELD|  
|SQL_API_SQLCLOSECURSOR|SQL_API_SQLGETDIAGREC|  
|SQL_API_SQLCOLATTRIBUTE|SQL_API_SQLGETENVATTR|  
|SQL_API_SQLCONNECT|SQL_API_SQLGETFUNCTIONS|  
|SQL_API_SQLCOPYDESC|SQL_API_SQLGETINFO|  
|SQL_API_SQLDATASOURCES|SQL_API_SQLGETSTMTATTR|  
|SQL_API_SQLDESCRIBECOL|SQL_API_SQLGETTYPEINFO|  
|SQL_API_SQLDISCONNECT|SQL_API_SQLNUMRESULTCOLS|  
|SQL_API_SQLDRIVERS|SQL_API_SQLPARAMDATA|  
|SQL_API_SQLENDTRAN|SQL_API_SQLPREPARE|  
|SQL_API_SQLEXECDIRECT|SQL_API_SQLPUTDATA|  
|SQL_API_SQLEXECUTE|SQL_API_SQLROWCOUNT|  
|SQL_API_SQLFETCH|SQL_API_SQLSETCONNECTATTR|  
|SQL_API_SQLFETCHSCROLL|SQL_API_SQLSETCURSORNAME|  
|SQL_API_SQLFREEHANDLE|SQL_API_SQLSETDESCFIELD|  
|SQL_API_SQLFREESTMT|SQL_API_SQLSETDESCREC|  
|SQL_API_SQLGETCONNECTATTR|SQL_API_SQLSETENVATTR|  
|SQL_API_SQLGETCURSORNAME|SQL_API_SQLSETSTMTATTR|  
|SQL_API_SQLGETDATA| |  
  
 以下列表列出了符合开放组标准符合性级别的函数的 *FunctionId* 的有效值：  
  
|FunctionId 值|FunctionId 值|  
|-|-|  
|SQL_API_SQLCOLUMNS|SQL_API_SQLSTATISTICS|  
|SQL_API_SQLSPECIALCOLUMNS|SQL_API_SQLTABLES|  
  
 下面是符合 ODBC 标准符合性级别的函数的 *FunctionId* 有效值列表。  
  
|FunctionId 值|FunctionId 值|  
|-|-|  
|SQL_API_SQLBINDPARAMETER|SQL_API_SQLNATIVESQL|  
|SQL_API_SQLBROWSECONNECT|SQL_API_SQLNUMPARAMS|  
|SQL_API_SQLBULKOPERATIONS [1]|SQL_API_SQLPRIMARYKEYS|  
|SQL_API_SQLCOLUMNPRIVILEGES|SQL_API_SQLPROCEDURECOLUMNS|  
|SQL_API_SQLDESCRIBEPARAM|SQL_API_SQLPROCEDURES|  
|SQL_API_SQLDRIVERCONNECT|SQL_API_SQLSETPOS|  
|SQL_API_SQLFOREIGNKEYS|SQL_API_SQLTABLEPRIVILEGES|  
|SQL_API_SQLMORERESULTS| |  
  
 [1] 使用 ODBC 2.x 驱动程序时，只有在满足以下两个条件时，才会返回 **SQLBulkOperations** ： ODBC 2.x *驱动程序支持* **SQLSetPos**，而信息类型 SQL_POS_OPERATIONS 返回设置的 SQL_POS_ADD *位。*  
  
 下面列出了 ODBC 3.8 或更高版本中引入的函数的 *FunctionId* 有效值：  
  
|FunctionId 值|  
|-|  
|SQL_API_SQLCANCELHANDLE [2]|  
  
 [2] 仅当驱动程序支持 **SQLCancel** 和 **SQLCancelHandle** 时， **SQLCancelHandle** 才会返回。 如果支持 **SQLCancel** ，但 **SQLCancelHandle** 不为，则应用程序仍可对语句句柄调用 **SQLCancelHandle** ，因为它将映射到 **SQLCancel**。  
  
## <a name="sql_func_exists-macro"></a>SQL_FUNC_EXISTS 宏  
 使用 SQL_API_ODBC3_ALL_FUNCTIONS 的 *FunctionID* 参数调用 **SQLGetFunctions** 后，SQL_FUNC_EXISTS (*SupportedPtr*， *FunctionID*) 宏用于确定 ODBC *1.x 或更* 早版本的函数的支持。 应用程序调用 SQL_FUNC_EXISTS，并 *将 SupportedPtr* 参数设置为传递到 *SQLGetFunctions* 的 *SupportedPtr* ，并将 *FunctionID* 参数设置为该函数的 **#define** 。 如果支持该函数，则 SQL_FUNC_EXISTS 返回 SQL_TRUE; 否则返回 SQL_FALSE。  
  
> [!NOTE]
>  当使用 ODBC 2.x 驱动程序时，ODBC 3.x *驱动程序* 管理器将为 **SQLAllocHandle** 和 **SQLFreeHandle** 返回 SQL_TRUE，因为 **SQLAllocHandle** 映射到 **SQLAllocEnv**、 **SQLAllocConnect** 或 **SQLAllocStmt**，并且 **SQLFreeHandle** 映射到 **SQLFreeEnv**、 **SQLFreeConnect** 或 **SQLFreeStmt**。 SQL_TRUE 但不支持将 **SQLAllocHandle** 或 **SQLFreeHandle** 与 SQL_HANDLE_DESC 的 *HandleType* 参数一起使用，因为在这种情况下，不存在要映射到的 ODBC *2.x 函数。*  
  
## <a name="code-example"></a>代码示例  
 以下三个示例演示应用程序如何使用 **SQLGetFunctions** 来确定驱动程序是否支持 **SQLTables**、 **SQLColumns** 和 **SQLStatistics**。 如果驱动程序不支持这些功能，应用程序将从驱动程序断开连接。 第一个示例对每个函数调用 **SQLGetFunctions** 一次。  
  
```cpp  
SQLUSMALLINT TablesExists, ColumnsExists, StatisticsExists;  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
  
retcodeTables = SQLGetFunctions(hdbc, SQL_API_SQLTABLES, &TablesExists);  
retcodeColumns = SQLGetFunctions(hdbc, SQL_API_SQLCOLUMNS, &ColumnsExists);  
retcodeStatistics = SQLGetFunctions(hdbc, SQL_API_SQLSTATISTICS, &StatisticsExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (retcodeTables == SQL_SUCCESS && TablesExists == SQL_TRUE &&   
retcodeColumns == SQL_SUCCESS && ColumnsExists == SQL_TRUE &&   
retcodeStatistics == SQL_SUCCESS && StatisticsExists == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 在第二个示例中，ODBC 3.x 应用程序调用 **SQLGetFunctions** ，并向其传递一个数组，其中 **SQLGetFunctions** 返回有关所有 ODBC 1.x 和更早函数的信息。  
  
```cpp  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[SQL_API_ODBC3_ALL_FUNCTIONS_SIZE];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ODBC3_ALL_FUNCTIONS, fExists);  
  
// SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver.  
if (reccode == SQL_SUCCESS &&   
SQL_FUNC_EXISTS(fExists, SQL_API_SQLTABLES) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLCOLUMNS) == SQL_TRUE &&  
   SQL_FUNC_EXISTS(fExists, SQL_API_SQLSTATISTICS) == SQL_TRUE)   
{  
  
   // Continue with application  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
 第三个示例是 ODBC 2.x 应用程序调用 **SQLGetFunctions** ，并向其传递一个100元素数组，其中 **SQLGetFunctions** 返回有关所有 ODBC 2.x 和更早函数的信息。  
  
```cpp  
#define FUNCTIONS 100  
  
RETCODE retcodeTables, retcodeColumns, retcodeStatistics  
SQLUSMALLINT fExists[FUNCTIONS];  
  
retcode = SQLGetFunctions(hdbc, SQL_API_ALL_FUNCTIONS, fExists);  
  
/* SQLGetFunctions is completed successfully and SQLTables, SQLColumns, and SQLStatistics are supported by the driver. */  
if (retcode == SQL_SUCCESS &&   
fExists[SQL_API_SQLTABLES] == SQL_TRUE &&  
   fExists[SQL_API_SQLCOLUMNS] == SQL_TRUE &&  
   fExists[SQL_API_SQLSTATISTICS] == SQL_TRUE)   
{  
  
   /* Continue with application */  
  
}  
  
SQLDisconnect(hdbc);  
```  
  
## <a name="related-functions"></a>相关函数  
  
|有关以下方面的信息|请参阅|  
|---------------------------|---------|  
|返回连接属性的设置|[SQLGetConnectAttr 函数](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|返回有关驱动程序或数据源的信息|[SQLGetInfo 函数](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|返回语句特性的设置|[SQLGetStmtAttr 函数](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
  
## <a name="see-also"></a>另请参阅  
 [ODBC API 参考](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [ODBC 头文件](../../../odbc/reference/install/odbc-header-files.md)
