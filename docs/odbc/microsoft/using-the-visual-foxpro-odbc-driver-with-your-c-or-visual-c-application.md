---
description: 将 Visual FoxPro ODBC 驱动程序与 C 或 Visual C++ 应用程序一起使用
title: 将 Visual FoxPro ODBC 驱动程序与 C 或 Visual C++ 应用程序一起使用 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- FoxPro ODBC driver [ODBC], C or C++ applications
- C++ applications [ODBC]
- Visual FoxPro ODBC driver [ODBC], C or C++ applications
- Visual FoxPro data [ODBC], C or C++ applications
- C applications [ODBC]
ms.assetid: beb11a68-849e-4fe0-b217-d3722b1b1389
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0aa73fdf7cf224dc1c7bff6ad162c5bb18e1d6b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070382"
---
# <a name="use-the-visual-foxpro-odbc-driver-with-your-c-or-visual-c-application"></a>将 Visual FoxPro ODBC 驱动程序与 C 或 Visual C++ 应用程序一起使用
C 或 c + + 应用程序通过将 [SQLExecute](../../odbc/microsoft/sqlexecute-visual-foxpro-odbc-driver.md) 或 [SQLExecDirect](../../odbc/microsoft/sqlexecdirect-visual-foxpro-odbc-driver.md) 语句发送到 Visual FoxPro 来与 visual foxpro 数据通信。 此语句可以包含以下内容：  
  
-   Visual FoxPro 语言的本机 SQL 语句，如 [DROP TABLE](../../odbc/microsoft/drop-table-command.md) 命令。  
  
-   [支持的 ODBC SQL 语法](../../odbc/microsoft/supported-odbc-sql-grammar-visual-foxpro-odbc-driver.md)。  
  
-   非 SQL Visual FoxPro 语言，如 [支持的 SET 命令](../../odbc/microsoft/supported-set-commands-visual-foxpro-odbc-driver.md)。  
  
 有关 SQL native to Visual FoxPro 的详细信息，请参阅 Visual FoxPro 文档。  
  
## <a name="example-using-the-visual-foxpro-odbc-driver-with-your-c-or-c-application"></a>示例：将 Visual FoxPro ODBC 驱动程序用于 C 或 c + + 应用程序  
 下面的示例使用 ODBC C API 检索名为 TasTrade 的 Microsoft® Visual FoxPro 示例数据库的 employee 表中的 "last_name" 字段中存储的数据。 此数据库随 Visual FoxPro 一起提供，默认情况下安装在以下位置：  
  
 `c:\vfp\samples\mainsamp\data\tastrade.dbc`  
  
 该示例一次显示一个姓氏，允许您单击消息框上的 "确定" 以查看下一个姓氏。 假设已将名为 Tastrade 的数据源设置为使用 Tastrade. dbc 数据库。  
  
> [!NOTE]  
>  应在所有 ODBC API 调用上执行错误检查;出于简洁起见，此示例不包括错误检查。  
  
```  
// FoxPro_ODBC_Driver_with_C.cpp  
// compile with: odbc32.lib user32.lib /c  
#include <windows.h>  
#include <sql.h>  
#include <sqlext.h>  
#include <stdio.h>  
#include <mbstring.h>  
  
#define MAX_DATA 100  
#define MYSQLSUCCESS(rc) ((rc==SQL_SUCCESS)||(rc==SQL_SUCCESS_WITH_INFO))  
  
class direxec {  
   RETCODE rc;        // ODBC return code  
   HENV henv;         // Environment     
   HDBC hdbc;         // Connection handle  
   HSTMT hstmt;       // Statement handle  
   unsigned char szData[MAX_DATA];   // Returned data storage  
   SDWORD cbData;     // Output length of data  
   unsigned char chr_ds_name[SQL_MAX_DSN_LENGTH];   // Data source name  
  
public:  
   direxec();           // Constructor  
   void sqlconn();      // Allocate env, stat, and conn  
   void sqlexec(unsigned char *);   // Execute SQL statement  
   void sqldisconn();   // Free pointers to env, stat, conn, and disconnect  
   void error_out();    // Displays errors  
};  
  
// Constructor initializes the string chr_ds_name with the data source name.  
direxec::direxec() {  
   _mbscpy_s(chr_ds_name, (const unsigned char *)"tastrade");  
}  
  
// Allocate environment handle, allocate connection handle,  
// connect to data source, and allocate statement handle.  
void direxec::sqlconn() {  
   SQLAllocEnv(&henv);  
   SQLAllocConnect(henv, &hdbc);  
   rc=SQLConnect(hdbc, chr_ds_name, SQL_NTS, NULL, 0, NULL, 0);  
  
   // Deallocate handles, display error message, and exit.  
   if (!MYSQLSUCCESS(rc)) {  
      SQLFreeEnv(henv);  
      SQLFreeConnect(hdbc);  
      error_out();  
      exit(-1);  
   }  
  
   rc = SQLAllocStmt(hdbc, &hstmt);  
}  
  
// Execute SQL command with SQLExecDirect() ODBC API.  
void direxec::sqlexec(unsigned char * cmdstr) {  
   rc = SQLExecDirect(hstmt, cmdstr, SQL_NTS);  
   if (!MYSQLSUCCESS(rc)) {   // Error  
      error_out();  
      // Deallocate handles and disconnect.  
      SQLFreeStmt(hstmt, SQL_DROP);  
      SQLDisconnect(hdbc);  
      SQLFreeConnect(hdbc);  
      SQLFreeEnv(henv);  
      exit(-1);  
   }  
   else {  
      for (rc = SQLFetch(hstmt) ; rc == SQL_SUCCESS; rc=SQLFetch(hstmt)) {  
         SQLGetData(hstmt, 1, SQL_C_CHAR, szData, sizeof(szData), &cbData);  
         // In this example, the data is returned in a messagebox for  
         // simplicity. However, normally the SQLBindCol() ODBC API could  
         // be called to bind individual data rows and assign for a rowset.  
         MessageBox(NULL, (const char *)szData, "ODBC", MB_OK);  
      }  
   }  
}  
  
// Free the statement handle, disconnect, free the connection handle, and  
// free the environment handle.  
void direxec::sqldisconn() {  
   SQLFreeStmt(hstmt, SQL_DROP);  
   SQLDisconnect(hdbc);  
   SQLFreeConnect(hdbc);  
   SQLFreeEnv(henv);  
}  
  
// Display error message in a message box that has an OK button.  
void direxec::error_out() {  
   unsigned char szSQLSTATE[10];  
   SDWORD nErr;  
   unsigned char msg[SQL_MAX_MESSAGE_LENGTH + 1];  
   SWORD cbmsg;  
  
   while(SQLError(0, 0, hstmt, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg) == SQL_SUCCESS) {  
      sprintf_s((char *)szData, MAX_DATA, "Error:\nSQLSTATE=%s, Native error=%ld, msg='%s'",   
         szSQLSTATE, nErr, msg);  
  
      MessageBox(NULL, (const char *)szData, "ODBC Error", MB_OK);  
   }  
}  
  
int main (){  
   // Declare an instance of the direxec object.  
   direxec x;  
  
   // Allocate handles and connect.  
   x.sqlconn();  
  
   // Execute SQL command "SELECT last_name FROM employee".  
   x.sqlexec((UCHAR FAR *)"SELECT last_name FROM employee");  
  
   // Free handles, and disconnect.  
   x.sqldisconn();  
  
   // Return success code; example executed successfully.  
   return (TRUE);  
}  
```
