---
description: 处理 ODBC 错误 (ODBC)
title: 在 ODBC)  (处理 ODBC 错误 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- errors [ODBC]
ms.assetid: 66ab0762-79fe-4a31-b655-27dd215a0af7
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 42b33bd1c94d9cf4d99a7f9061517d5d3de39b3a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473518"
---
# <a name="process-odbc-errors-odbc"></a>处理 ODBC 错误 (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  可以使用两个 ODBC 函数调用来检索 ODBC 消息： [SQLGetDiagRec](../../odbc/reference/syntax/sqlgetdiagrec-function.md) 和 [SQLGetDiagField](../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md)。 若要获取 **SQLState**、**pfNative** 和 **ErrorMessage** 诊断字段中有关 ODBC 的主要信息，请调用 [SQLGetDiagRec](../../odbc/reference/syntax/sqlgetdiagrec-function.md)，直到其返回 SQL_NO_DATA 为止。 对于每条诊断记录，可以调用 [SQLGetDiagField](../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md) 来检索各个字段。 所有特定于驱动程序的字段都必须使用 **SQLGetDiagField** 来检索。  
  
 [SQLGetDiagRec](../../odbc/reference/syntax/sqlgetdiagrec-function.md) 和 [SQLGetDiagField](../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md) 通过 ODBC 驱动程序管理器进行处理，而不是通过单独的驱动程序进行处理。 在成功连接之前，ODBC 驱动程序管理器不会缓存特定于驱动程序的诊断字段。 在成功连接之前，无法针对特定于驱动程序的诊断字段调用 [SQLGetDiagField](../../relational-databases/native-client-odbc-api/sqlgetdiagfield.md)。 这包括 ODBC 连接命令，即使它们返回 SQL_SUCCESS_WITH_INFO 也是如此。 在进行下一次 ODBC 函数调用之前，特定于驱动程序的诊断字段不可用。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>说明  
 此示例显示一个简单的错误处理程序，它调用 [SQLGetDiagRec](../../odbc/reference/syntax/sqlgetdiagrec-function.md) 以获取标准 ODBC 信息。 随后测试是否存在有效连接，如果存在，则为特定于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ODBC 驱动程序的诊断字段调用 **SQLGetDiagField**。 IA64 平台不支持此示例。  
  
 此示例是面向 ODBC 3.0 版或更高版本开发的。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。 如果 Windows 身份验证不可用，请在运行时提示用户输入其凭据。 不要将凭据存储在一个文件中。 如果必须保存凭据，应当用 [Win32 crypto API](/windows/win32/seccrypto/cryptography-reference)（Win32 加密 API）加密它们。  
  
 需要一个名为 AdventureWorks 的 ODBC 数据源，其默认数据库是 AdventureWorks 示例数据库。  (可以从 [Microsoft SQL Server 示例和社区项目](https://go.microsoft.com/fwlink/?LinkID=85384) 主页下载 AdventureWorks 示例数据库。 ) 此数据源必须基于操作系统提供的 ODBC 驱动程序， (驱动程序名称为 "SQL Server" ) 。 如果您要将此示例构建为在 64 位操作系统上运行的 32 位应用程序并运行该示例，则必须使用 %windir%\SysWOW64\odbcad32.exe 中的 ODBC 管理器创建 ODBC 数据源。  
  
 此示例连接到您的计算机上默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 若要连接到命名实例，请更改 ODBC 数据源的定义以使用以下格式指定实例：server\namedinstance。 默认情况下，[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 将安装在命名实例中。  
  
 执行第一个 ( [!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表，以创建此示例使用的存储过程。  
  
 使用 odbc32.lib 编译第二个 (C++) 代码列表。 然后，执行该程序。  
  
 执行第三个 ( [!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表，以删除此示例使用的存储过程。  
  
### <a name="code"></a>代码  
  
```  
use AdventureWorks  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'BadOne')  
   DROP PROCEDURE BadOne  
  
Go  
  
CREATE PROCEDURE BadOne   
AS   
SELECT * FROM Purchasing.Vendor  
Go  
```  
  
### <a name="code"></a>代码  
  
```  
// compile with: odbc32.lib  
#include <stdio.h>  
#include <string.h>  
#include <windows.h>  
#include <sql.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define MAXBUFLEN 256  
  
SQLHENV henv = SQL_NULL_HENV;  
SQLHDBC hdbc1 = SQL_NULL_HDBC;       
SQLHSTMT hstmt1 = SQL_NULL_HSTMT;  
  
void ProcessLogMessages(SQLSMALLINT plm_handle_type, SQLHANDLE plm_handle, char *logstring, int ConnInd);  
  
void Cleanup() {  
   if (hstmt1 != SQL_NULL_HSTMT)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
int main() {  
   RETCODE retcode;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER)SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle and connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // This sample use Integrated Security. Please create the SQL Server   
   // DSN by using the Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"AdventureWorks", SQL_NTS, (UCHAR*)"",SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) &&  
      (retcode != SQL_SUCCESS_WITH_INFO) ) {  
         ProcessLogMessages(SQL_HANDLE_DBC, hdbc1, "SQLConnect() Failed\n\n", FALSE);  
         Cleanup();  
         return(9);  
   }  
   else {  
      ProcessLogMessages(SQL_HANDLE_DBC, hdbc1,  
         "\nConnect Successful\n\n", FALSE);  
   }  
  
   // Allocate statement handle, and then execute command.  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      ProcessLogMessages(SQL_HANDLE_DBC, hdbc1, "SQLAllocHandle(hstmt1) Failed\n\n", TRUE);  
      Cleanup();  
      return(9);  
   }  
  
   retcode = SQLExecDirect(hstmt1, (UCHAR*)"exec BadOne", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
         ProcessLogMessages(SQL_HANDLE_STMT, hstmt1, "SQLExecute() Failed\n\n", TRUE);  
         Cleanup();  
         return(9);  
   }  
  
   // Clear any result sets generated.  
   while ( ( retcode = SQLMoreResults(hstmt1) ) != SQL_NO_DATA )  
      ;  
  
   // Clean up.   
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
void ProcessLogMessages(SQLSMALLINT plm_handle_type, SQLHANDLE plm_handle, char *logstring, int ConnInd) {  
   RETCODE plm_retcode = SQL_SUCCESS;  
   UCHAR plm_szSqlState[MAXBUFLEN] = "", plm_szErrorMsg[MAXBUFLEN] = "";  
   SDWORD plm_pfNativeError = 0L;  
   SWORD plm_pcbErrorMsg = 0;  
   SQLSMALLINT plm_cRecNmbr = 1;  
   SDWORD plm_SS_MsgState = 0, plm_SS_Severity = 0;  
   SQLINTEGER plm_Rownumber = 0;  
   USHORT plm_SS_Line;  
   SQLSMALLINT plm_cbSS_Procname, plm_cbSS_Srvname;  
   SQLCHAR plm_SS_Procname[MAXNAME], plm_SS_Srvname[MAXNAME];  
  
   if (logstring)  
      printf(logstring);  
  
   while (plm_retcode != SQL_NO_DATA_FOUND) {  
      plm_retcode = SQLGetDiagRec(plm_handle_type, plm_handle, plm_cRecNmbr,   
                                  plm_szSqlState, &plm_pfNativeError, plm_szErrorMsg,   
                                  MAXBUFLEN - 1, &plm_pcbErrorMsg);  
  
      // Note that if the application has not yet made a successful connection,   
      // the SQLGetDiagField information has not yet been cached by ODBC Driver Manager and   
      // these calls to SQLGetDiagField will fail.  
      if (plm_retcode != SQL_NO_DATA_FOUND) {  
         if (ConnInd) {   
            plm_retcode = SQLGetDiagField( plm_handle_type, plm_handle, plm_cRecNmbr,  
                                                        SQL_DIAG_ROW_NUMBER, &plm_Rownumber,  
                                                        SQL_IS_INTEGER, NULL);  
  
            plm_retcode = SQLGetDiagField( plm_handle_type, plm_handle, plm_cRecNmbr,  
                                           SQL_DIAG_SS_LINE, &plm_SS_Line, SQL_IS_INTEGER, NULL);  
  
            plm_retcode = SQLGetDiagField( plm_handle_type, plm_handle, plm_cRecNmbr,   
                                           SQL_DIAG_SS_MSGSTATE, &plm_SS_MsgState,  
                                           SQL_IS_INTEGER, NULL);  
  
            plm_retcode = SQLGetDiagField( plm_handle_type, plm_handle, plm_cRecNmbr,  
                                           SQL_DIAG_SS_SEVERITY, &plm_SS_Severity,  
                                           SQL_IS_INTEGER, NULL);  
  
            plm_retcode = SQLGetDiagField( plm_handle_type, plm_handle, plm_cRecNmbr,  
                                           SQL_DIAG_SS_PROCNAME, &plm_SS_Procname,  
                                           sizeof(plm_SS_Procname), &plm_cbSS_Procname);  
  
            plm_retcode = SQLGetDiagField( plm_handle_type, plm_handle, plm_cRecNmbr,  
                                           SQL_DIAG_SS_SRVNAME, &plm_SS_Srvname,   
                                           sizeof(plm_SS_Srvname), &plm_cbSS_Srvname);  
         }  
  
         printf("szSqlState = %s\n", plm_szSqlState);  
         printf("pfNativeError = %d\n", plm_pfNativeError);  
         printf("szErrorMsg = %s\n", plm_szErrorMsg);  
         printf("pcbErrorMsg = %d\n\n", plm_pcbErrorMsg);  
  
         if (ConnInd) {  
            printf("ODBCRowNumber = %d\n", plm_Rownumber);  
            printf("SSrvrLine = %d\n", plm_Rownumber);  
            printf("SSrvrMsgState = %d\n", plm_SS_MsgState);  
            printf("SSrvrSeverity = %d\n", plm_SS_Severity);  
            printf("SSrvrProcname = %s\n", plm_SS_Procname);  
            printf("SSrvrSrvname = %s\n\n", plm_SS_Srvname);  
         }  
      }  
  
      plm_cRecNmbr++;   // Increment to next diagnostic record.  
   }  
}  
```  
  
### <a name="code"></a>代码  
  
```  
use AdventureWorks  
DROP PROCEDURE BadOne  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [ODBC 操作指南主题](../../relational-databases/native-client-odbc-how-to/odbc-how-to-topics.md)  
  
