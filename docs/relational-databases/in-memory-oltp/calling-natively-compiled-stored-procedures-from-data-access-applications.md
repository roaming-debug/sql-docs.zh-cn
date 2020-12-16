---
title: 本机编译存储过程 - 数据访问应用程序
description: 查找有关从数据访问应用程序调用本机编译存储过程的指南，以及使用 SQL Server Native Client ODBC 驱动程序的示例。
ms.custom: seo-dt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 9cf6c5ff-4548-401a-b3ec-084f47ff0eb8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 77c9d57563fb926a777a20edc89e8dbde7bf204d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465408"
---
# <a name="calling-natively-compiled-stored-procedures-from-data-access-applications"></a>从数据访问应用程序调用本机编译的存储过程

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

本主题提供有关从数据访问应用程序中调用本机编译的存储过程的指导。

## <a name="guidance-points"></a>指导内容

- 游标无法遍历本机编译存储过程。

- 不支持使用上下文连接从 CLR 模块调用本机编译的存储过程。

### <a name="sqlclient"></a>SqlClient

- 对于 SqlClient，准备好的执行与直接执行没有分别 。 使用带有 `CommandType = CommandType.StoredProcedure` 的 SqlCommand 执行存储过程。

- SqlClient 不支持准备好的 RPC 过程调用。

- SqlClient 不支持检索有关本机编译存储过程返回的结果集的纯架构信息（元数据发现）(`CommandType.SchemaOnly`)。
  - 请改为使用 [sp_describe_first_result_set (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)。

### <a name="sql-server-native-client"></a>SQL Server Native Client

- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 之前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 版本不支持检索有关本机编译存储过程返回的结果集的纯架构信息（元数据发现）。
- 请改为使用 [sp_describe_first_result_set (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)。

### <a name="odbc"></a>ODBC

以下建议适用于在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 中使用 ODBC 驱动程序调用本机编译的存储过程。

*调用一次：* 只调用存储过程一次的最高效方式是使用 **SQLExecDirect** 和 ODBC CALL 子句发出直接的 RPC 调用。 请勿使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] EXECUTE 语句。 如需多次调用存储过程，则准备好的执行最为高效。

*调用多次：* 多次调用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程的最高效方式是通过准备好的 RPC 过程调用执行。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 中使用 ODBC 驱动程序执行准备好的 RPC 调用的步骤是：

1. 打开到数据库的连接。
2. 使用 SQLBindParameter 绑定参数。
3. 使用 SQLPrepare 准备过程调用。
4. 使用 SQLExecute 多次执行存储过程。

#### <a name="c-code-for-odbc"></a>用于 ODBC 的 C 代码

下面的 C 代码片段演示了某个向订单添加行项目的存储过程的准备好的执行。 SQLPrepare 只调用了一次， 而 SQLExecute 调用了多次（每个过程执行调用一次）。

```cpp
// Bind parameters
// 1 - OrdNo
SQLRETURN returnCode = SQLBindParameter(
                     hstmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 10, 0,
                     &order.OrdNo, sizeof(SQLINTEGER), NULL);
if (returnCode != SQL_SUCCESS && returnCode != SQL_SUCCESS_WITH_INFO) {
   ODBCError(henv, hdbc, hstmt, NULL, true);
   exit(-1);
}

// 2, 3, 4 - ItemNo, ProdCode, Qty
...

// Prepare stored procedure
returnCode = SQLPrepare(hstmt, (SQLTCHAR *) _T("{call ItemInsert(?, ?, ?, ?)}"),
                        SQL_NTS);

for (unsigned int i = 0; i < order.ItemCount; i++) {
   ItemNo = order.ItemNo[i];
   ProdCode = order.ProdCode[i];
   Qty = order.Qty[i];

   // Execute stored procedure
   returnCode = SQLExecute(hstmt);
   if (returnCode != SQL_SUCCESS && returnCode != SQL_SUCCESS_WITH_INFO) {
      ODBCError(henv, hdbc, hstmt, NULL, true);
      exit(-1);
   }
}
```

## <a name="using-odbc-to-execute-a-natively-compiled-stored-procedure"></a>使用 ODBC 执行本机编译的存储过程

此示例显示如何使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驱动程序绑定参数和执行存储过程。 该示例编译为一个控制台应用程序，它使用直接执行来插入单个订单并使用准备的执行插入订单详细信息。

运行此示例：

1. 使用内存优化的数据文件组创建示例数据库。 有关如何使用内存优化的数据文件组创建数据库的信息，请参阅 [创建内存优化表和本机编译的存储过程](../../relational-databases/in-memory-oltp/creating-a-memory-optimized-table-and-a-natively-compiled-stored-procedure.md)。

2. 创建指向该数据库的名为 PrepExecSample 的 ODBC 数据源。 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 驱动程序。 您还可以修改该示例并使用 [Microsoft ODBC Driver for SQL Server](../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)。

3. 在示例数据库上运行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 脚本（下面）。

4. 编译并运行该示例。

5. 通过查询表的内容验证是否成功执行了程序：

    ```sql
    SELECT * FROM dbo.Ord;
    ```

    ```sql
    SELECT * FROM dbo.Item;
    ```

## <a name="preliminary-transact-sql"></a>初步 Transact-SQL

以下是创建内存优化的数据库对象的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 代码段。

```sql
IF EXISTS (SELECT * FROM SYS.OBJECTS WHERE OBJECT_ID=OBJECT_ID('dbo.OrderInsert'))
DROP PROCEDURE dbo.OrderInsert;  
GO
IF EXISTS (SELECT * FROM SYS.OBJECTS WHERE OBJECT_ID=OBJECT_ID('dbo.ItemInsert'))
DROP PROCEDURE dbo.ItemInsert;  
GO  
IF EXISTS (SELECT * FROM SYS.OBJECTS WHERE OBJECT_ID=OBJECT_ID('dbo.Ord'))
DROP TABLE dbo.Ord;  
GO  
IF EXISTS (SELECT * FROM SYS.OBJECTS WHERE OBJECT_ID=OBJECT_ID('dbo.Item'))
DROP TABLE dbo.Item;  
GO

CREATE TABLE dbo.Ord  
(  
   OrdNo INTEGER NOT NULL PRIMARY KEY NONCLUSTERED,  
   OrdDate DATETIME NOT NULL,   
   CustCode VARCHAR(5) NOT NULL)   
 WITH (MEMORY_OPTIMIZED=ON);  
GO  
  
CREATE TABLE dbo.Item  
(  
   OrdNo INTEGER NOT NULL,   
   ItemNo INTEGER NOT NULL,   
   ProdCode INTEGER NOT NULL,   
   Qty INTEGER NOT NULL,  
   CONSTRAINT PK_Item PRIMARY KEY NONCLUSTERED (OrdNo,ItemNo))  
   WITH (MEMORY_OPTIMIZED=ON);  
GO  
  
CREATE PROCEDURE dbo.OrderInsert(
    @OrdNo INTEGER, @CustCode VARCHAR(5))  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH  
   (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = 'english')  
  
  DECLARE @OrdDate datetime = GETDATE();  
  INSERT INTO dbo.Ord (OrdNo, CustCode, OrdDate)
  VALUES (@OrdNo, @CustCode, @OrdDate);
END;  
GO  
  
CREATE PROCEDURE dbo.ItemInsert(
    @OrdNo INTEGER, @ItemNo INTEGER, @ProdCode INTEGER, @Qty INTEGER)
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH  
   (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english')  
  
  INSERT INTO dbo.Item (OrdNo, ItemNo, ProdCode, Qty)
  VALUES (@OrdNo, @ItemNo, @ProdCode, @Qty)
END  
GO  
```

## <a name="c-code"></a>C 代码

以下是 C 代码段。

```cpp
// compile with: user32.lib odbc32.lib
#pragma once  
#define WIN32_LEAN_AND_MEAN  // Exclude rarely-used stuff from Windows headers.
#include <stdio.h>  
#include <stdlib.h>  
#include <tchar.h>  
#include <windows.h>  
#include "sql.h"  
#include "sqlext.h"  
#include "sqlncli.h"  
  
// cardinality of order item related array variables
#define ITEM_ARRAY_SIZE 20  
  
// struct to pass order entry data  
typedef struct OrdEntry_struct {  
   SQLINTEGER OrdNo;  
   SQLTCHAR CustCode[6];  
   SQLUINTEGER ItemCount;  
   SQLINTEGER ItemNo[ITEM_ARRAY_SIZE];  
   SQLINTEGER ProdCode[ITEM_ARRAY_SIZE];  
   SQLINTEGER Qty[ITEM_ARRAY_SIZE];  
} OrdEntryData;  
  
SQLHANDLE henv, hdbc, hstmt;  
  
void ODBCError(
      SQLHANDLE henv, SQLHANDLE hdbc,
      SQLHANDLE hstmt, SQLHANDLE hdesc,
      bool ShowError)
{  
   SQLRETURN r = 0;  
   SQLTCHAR szSqlState[6] = {0};  
   SQLINTEGER fNativeError = 0;  
   SQLTCHAR szErrorMsg[256] = {0};  
   SQLSMALLINT cbErrorMsgMax = sizeof(szErrorMsg) - 1;
   SQLSMALLINT cbErrorMsg = 0;  
   TCHAR text[1024] = {0}, title[256] = {0};  
  
   if (hdesc != NULL)  
      r = SQLGetDiagRec(SQL_HANDLE_DESC, hdesc, 1, szSqlState,
              &fNativeError, szErrorMsg, cbErrorMsgMax, &cbErrorMsg);
   else {  
      if (hstmt != NULL)  
         r = SQLGetDiagRec(SQL_HANDLE_STMT, hstmt, 1, szSqlState,
                 &fNativeError, szErrorMsg, cbErrorMsgMax, &cbErrorMsg);
      else {  
         if (hdbc != NULL)  
            r = SQLGetDiagRec(SQL_HANDLE_DBC, hdbc, 1, szSqlState,
                    &fNativeError, szErrorMsg, cbErrorMsgMax, &cbErrorMsg);
         else  
            r = SQLGetDiagRec(SQL_HANDLE_ENV, henv, 1, szSqlState,
                    &fNativeError, szErrorMsg, cbErrorMsgMax, &cbErrorMsg);
      }  
   }  
  
   if (ShowError) {  
      _sntprintf_s(title, _countof(title), _TRUNCATE, _T("ODBC Error %i"),
                      fNativeError);  
      _sntprintf_s(text, _countof(text), _TRUNCATE, _T("[%s] - %s"),
                      szSqlState, szErrorMsg);  
  
      MessageBox(NULL, (LPCTSTR) text, (LPCTSTR) _T("ODBC Error"), MB_OK);
   }  
}  
  
void connect() {  
   SQLRETURN r;  
  
   r = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &henv);  
  
   // This is an ODBC v3 application  
   r = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, 0);
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, NULL, NULL, NULL, true);  
      exit(-1);  
   }  
  
   r = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
  
   // Run in ANSI/implicit transaction mode  
   r = SQLSetConnectAttr(hdbc, SQL_ATTR_AUTOCOMMIT,
                          (SQLPOINTER) SQL_AUTOCOMMIT_OFF, SQL_IS_INTEGER);
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, NULL, NULL, NULL, true);  
      exit(-1);  
   }  
  
   TCHAR szConnStrIn[256] = _T("DSN=PrepExecSample");  
  
   r = SQLDriverConnect(hdbc, NULL, (SQLTCHAR *) szConnStrIn, SQL_NTS,
                          NULL, 0, NULL, SQL_DRIVER_NOPROMPT);
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, NULL, NULL, true);  
      exit(-1);  
   }  
}  
  
void setup_ODBC_basics() {  
   SQLRETURN r;  
  
   r = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);  
      exit(-1);  
   }  
}  
  
void OrdEntry(OrdEntryData& order) {  
   // Simple order entry  
   SQLRETURN r;  
  
   SQLINTEGER ItemNo, ProdCode, Qty;  
  
   // Bind parameters for the Order  
   // 1 - OrdNo input  
   r = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER,
                          0, 0, &order.OrdNo, sizeof(SQLINTEGER), NULL);
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // 2 - Custcode input  
   r = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT,SQL_C_TCHAR, SQL_VARCHAR, 5, 0,
                          &order.CustCode, sizeof(order.CustCode), NULL);
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // Insert the order  
   r = SQLExecDirect(hstmt, (SQLTCHAR *) _T("{call OrderInsert(?, ?)}"),SQL_NTS);
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // Flush results & reset hstmt  
   r = SQLMoreResults(hstmt);  
   if (r != SQL_NO_DATA) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   r = SQLFreeStmt(hstmt, SQL_RESET_PARAMS);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // Bind parameters for the Items  
   // 1 - OrdNo   
   r = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 10, 0,
                          &order.OrdNo, sizeof(SQLINTEGER), NULL);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // 2 - ItemNo   
   r = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 10, 0,
                          &ItemNo, sizeof(SQLINTEGER), NULL);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // 3 - ProdCode  
   r = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 10, 0,
                          &ProdCode, sizeof(SQLINTEGER), NULL);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // 4 - Qty  
   r = SQLBindParameter(hstmt, 4, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 10, 0,
                          &Qty, sizeof(SQLINTEGER), NULL);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // Prepare to insert items one at a time  
   r = SQLPrepare(hstmt, (SQLTCHAR *) _T("{call ItemInsert(?, ?, ?, ?)}"),SQL_NTS);
  
   for (unsigned int i = 0; i < order.ItemCount; i++) {  
  ItemNo = order.ItemNo[i];  
      ProdCode = order.ProdCode[i];  
      Qty = order.Qty[i];  
      r = SQLExecute(hstmt);  
      if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
         ODBCError(henv, hdbc, hstmt, NULL, true);   
         exit(-1);  
      }  
   }  
  
   // Flush results & reset hstmt  
   r = SQLMoreResults(hstmt);  
   if (r != SQL_NO_DATA) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   r = SQLFreeStmt(hstmt, SQL_RESET_PARAMS);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
  
   // Commit the transaction  
   r = SQLEndTran(SQL_HANDLE_DBC, hdbc, SQL_COMMIT);  
   if (r != SQL_SUCCESS && r != SQL_SUCCESS_WITH_INFO) {  
      ODBCError(henv, hdbc, hstmt, NULL, true);   
      exit(-1);  
   }  
}  
  
void testOrderEntry() {  
  
   OrdEntryData order;  
  
   order.OrdNo = 1;  
   _tcscpy_s((TCHAR *) order.CustCode, _countof(order.CustCode), _T("CUST1"));
   order.ItemNo[0] = 1;  
   order.ProdCode[0] = 10;  
   order.Qty[0] = 1;  
   order.ItemNo[1] = 2;  
   order.ProdCode[1] = 20;  
   order.Qty[1] = 2;  
   order.ItemNo[2] = 3;  
   order.ProdCode[2] = 30;  
   order.Qty[2] = 3;  
   order.ItemNo[3] = 4;  
   order.ProdCode[3] = 40;  
   order.Qty[3] = 4;  
   order.ItemCount = 4;  
  
   OrdEntry(order);  
}  
int _tmain() {  
   connect();  
   setup_ODBC_basics();  
  
   testOrderEntry();  
}  
```

## <a name="see-also"></a>另请参阅
[本机编译的存储过程](./a-guide-to-query-processing-for-memory-optimized-tables.md)