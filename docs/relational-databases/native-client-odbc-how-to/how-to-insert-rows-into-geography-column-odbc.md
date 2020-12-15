---
description: 如何：在 Geography 列中插入行 (ODBC)
title: 如何：将行插入到地域列 (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
ms.assetid: 0b6516f7-1fc0-4b01-a2d0-add0571070d5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7df48a706ab1a6983b1c1f32422c691f21a49fd8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473588"
---
# <a name="how-to-insert-rows-into-geography-column-odbc"></a>如何：在 Geography 列中插入行 (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  此示例使用两个不同的绑定（SQLCCHAR 和 SQLCBINARY）从 WellKnownBinary (WKB) 中向具有地理位置列的表中插入两行。 然后从该表中选择一行并使用 ::STAsText() 显示它。WKB 是 0x01010000000700ECFAD03A4C4001008000B5DF07C0，应用程序将以下内容显示到控制台：POINT(56.4595 -2.9842)。  
  
 此示例不需要 ODBC 数据源，但它默认情况下运行在 SQL Server 的本地实例上。  
  
 此示例不适用于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之前的任何 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 版本。  
  
 有关空间存储的详细信息，请参阅 [空间数据 &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)。  
  
## <a name="example"></a>示例  
 第一个 ( [!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表创建此示例使用的表。  
  
 使用 odbc32.lib 和 user32.lib 编译第二个 (C++) 代码列表。 请确保 INCLUDE 环境变量包含包含 sqlncli.msi 的目录。  
  
 如果您要将此示例构建为在 64 位操作系统上运行的 32 位应用程序并运行该示例，则必须使用 %windir%\SysWOW64\odbcad32.exe 中的 ODBC 管理器创建 ODBC 数据源。  
  
 此示例连接到您的计算机上默认的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 若要连接到命名实例，请更改 ODBC 数据源的定义以使用以下格式指定实例：server\namedinstance。 默认情况下，[!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 将安装在命名实例中。  
  
 第三个 ( [!INCLUDE[tsql](../../includes/tsql-md.md)]) 代码列表删除此示例使用的表。  
  
```sql
use tempdb  
GO  
  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'SpatialSample')  
   DROP TABLE SpatialSample  
  
CREATE TABLE SpatialSample (Name varchar(10), Geog Geography)  
GO  
```  
  
```cpp
// compile with: odbc32.lib user32.lib  
#include <windows.h>  
#include <Sqlext.h>  
#include <mbstring.h>  
#include "sqlncli.h"  
#include <string.h>  
#include <stdio.h>  
  
#define MAX_DATA 1024  
#define MYSQLSUCCESS(rc) ( (rc == SQL_SUCCESS) || (rc == SQL_SUCCESS_WITH_INFO) )  
  
SQLCHAR szDSN[] = "Driver={SQL Server Native Client 10.0};Server=.;Database=tempdb;Trusted_Connection=Yes;";  
  
class direxec {  
      RETCODE rc;   // ODBC return code  
      HENV henv;   // Environment  
      HDBC hdbc;   // Connection Handle  
      HSTMT hstmt;   // Statement Handle  
      SQLHDESC hdesc;   // Descriptor handle  
      SQLCHAR szData[MAX_DATA];   // Returned Data Storage  
      SDWORD cbData;   // Output Length of data   
  
      SQLCHAR szConnStrOut[MAX_DATA + 1];  
      SWORD swStrLen;  
  
public:  
      void sqlconn();   // Allocate env, stat and conn  
      void sqldisconn();   // Free pointers to env, stat, conn and disconnect  
      void error_out();   // Display errors  
      void check_rc(RETCODE rc);   // Checks for success of the return code  
      void SqlInsertFromChar();   // Insert a WKB in character form  
      void SqlInsertFromBinary();   // Insert a WKB in binary form   
      void SqlSelectGeogAsText(); // Retrieve the geography as Text.  
};   
  
// Allocate environment handles, connection handle, connect to data source, and allocate statement handle  
void direxec::sqlconn() {  
      // Allocate the environment handle  
      rc = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
      check_rc(rc);  
  
      // Set the ODBC version to version 3  
      rc = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
      check_rc(rc);  
  
      // Allocate the database connection handle  
      rc = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
      check_rc(rc);  
  
      // Connect to the database  
      rc = SQLDriverConnect(hdbc, NULL, szDSN, SQL_NTS, szConnStrOut, MAX_DATA, &swStrLen, SQL_DRIVER_NOPROMPT);  
      check_rc(rc);  
  
      // Allocate the statement handle  
      rc = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
      check_rc(rc);    
  
      // Allocate the descriptor handle  
      rc = rc = SQLAllocHandle(SQL_HANDLE_DESC, hdbc, &hdesc);  
      check_rc(rc);  
}   
  
// Display error message from the DiagRecord  
void direxec::error_out() {  
      // String to hold the SQL State  
      SQLCHAR szSQLSTATE[10];   
  
      // Error code  
      SDWORD nErr;  
  
      // The error message  
      SQLCHAR msg[SQL_MAX_MESSAGE_LENGTH + 1];  
  
      // Size of the message  
      SWORD cbmsg;  
  
      // If hstmt is not null use that for getting the DiagRec  
      if (hstmt)  
            rc = SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, 1, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg);  
      // else get the diag record from the env  
      else  
            rc = SQLGetDiagRec(SQL_HANDLE_ENV, henv, 1, szSQLSTATE, &nErr, msg, sizeof(msg), &cbmsg);  
  
      // If the rc is successful, show the message using a message box  
      if ( rc == SQL_SUCCESS) {  
            printf((char *)szData, "Error:\nSQLSTATE=%s,Native error=%ld, msg='%s'", szSQLSTATE, nErr, msg);  
            MessageBox(NULL, (const char *)szData, "ODBC Error", MB_OK);  
      }  
}  
  
// Checks the return code.  If failure, displays the error, free the memory and exits the program  
void direxec::check_rc(RETCODE rc) {  
      if (!MYSQLSUCCESS(rc)) {  
            error_out();  
            SQLFreeEnv(henv);  
            SQLFreeConnect(hdbc);  
            exit(-1);  
      }   
}  
  
void direxec::SqlInsertFromBinary() {     
      rc = SQLPrepare(hstmt, (SQLCHAR*) "INSERT INTO SpatialSample(Name,Geog) values('Sample1',Geography::STGeomFromWKB(?,4326))", SQL_NTS);  
      check_rc(rc);  
  
      SQLCHAR szBytes [] = "\x01\x01\x00\x00\x00\x07\x00\xEC\xFA\xD0\x3A\x4C\x40\x01\x00\x80\x00\xB5\xDF\x07\xC0";  
      SQLLEN iDataLength = sizeof(szBytes)-1;  
  
      rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_BINARY, SQL_VARBINARY, 100, 0, szBytes, sizeof(szBytes), &iDataLength);  
      check_rc(rc);  
  
      rc = SQLExecute(hstmt);  
      check_rc(rc);  
}  
  
void direxec::SqlInsertFromChar() {     
      rc = SQLPrepare(hstmt, (SQLCHAR*) "INSERT INTO SpatialSample(Name,Geog) values('Sample2',Geography::STGeomFromWKB(?,4326))", SQL_NTS);  
      check_rc(rc);  
  
      SQLCHAR szBytes [] = "01010000000700ECFAD03A4C4001008000B5DF07C0";  
  
      rc = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_VARBINARY, 100, 0, szBytes, sizeof(szBytes), NULL);  
      check_rc(rc);  
  
      rc = SQLExecute(hstmt);  
      check_rc(rc);  
}  
  
void direxec::SqlSelectGeogAsText() {  
      rc = SQLFreeStmt(hstmt, SQL_CLOSE);  
      check_rc(rc);   
  
      rc = SQLExecDirect(hstmt, (SQLCHAR*) "SELECT geog.STAsText() FROM SpatialSample", SQL_NTS);  
      check_rc(rc);   
  
      SQLCHAR rgcAsText[MAX_DATA];  
      SQLLEN cbAsText;   
  
      rc = SQLBindCol(hstmt, 1, SQL_C_CHAR, rgcAsText, sizeof(rgcAsText), &cbAsText);  
      check_rc(rc);  
  
      rc = SQLFetch(hstmt);  
      check_rc(rc);  
  
      rgcAsText[cbAsText] = '\0';  
      printf("%s\r\n", (LPSTR)rgcAsText);  
}   
  
int main() {  
      direxec x;  
  
      // Allocate handles, and connect.  
      x.sqlconn();    
  
      // Insert 2 samples into the table  
      x.SqlInsertFromChar();  
      x.SqlInsertFromBinary();  
  
      // Select 1 row from the table and display the geography as text  
      x.SqlSelectGeogAsText();  
}  
```  
  
```sql
use tempdb  
GO  
  
IF EXISTS (SELECT name FROM sysobjects WHERE name = 'SpatialSample')  
   DROP TABLE SpatialSample  
GO  
```  
  
  
