---
description: 处理生成消息的语句
title: 处理生成消息的语句 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- PRINT statement
- messages [ODBC], statements generating messages
- statements [ODBC], message generation
- errors [ODBC], statements generating messages
- SQL Server Native Client ODBC driver, errors
- STATISTICS IO option
- STATISTICS TIME option
- DBCC statements
- SQLExecute function
- RAISERROR statement
- SQLGetDiagRec function
- ODBC error handling, statements generating messages
- SQLExecDirect function
ms.assetid: 672ebdc5-7fa1-4ceb-8d52-fd25ef646654
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c648e39186dada7527678f6a1c8efcd764d86355
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438450"
---
# <a name="processing-statements-that-generate-messages"></a>处理生成消息的语句
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[tsql](../../includes/tsql-md.md)] SET 语句选项 STATISTICS TIME 和 STATISTICS IO 用于获取有助于诊断长时间运行的查询的信息。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本还支持用于分析查询计划的 SHOWPLAN 选项。 ODBC 应用程序可通过执行以下语句设置这些选项：  
  
```  
SQLExecDirect(hstmt, "SET SHOWPLAN ON", SQL_NTS);  
SQLExecDirect(hstmt, "SET STATISTICS TIME ON", SQL_NTS90  
);  
SQLExecDirect(hstmt, "SET STATISTICS IO ON", SQL_NTS);  
```  
  
 当 SET STATISTICS TIME 或 SET 显示计划处于开启状态时， **SQLExecute** 和 **SQLExecDirect** 将返回 SQL_SUCCESS_WITH_INFO，此时，应用程序可以通过调用 **SQLGetDiagRec** 来检索显示计划或统计信息时间输出，直到返回 SQL_NO_DATA。 SHOWPLAN 数据的每一行均按以下格式返回：  
  
```  
szSqlState="01000", *pfNativeError=6223,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]   
              Table Scan"  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 版将 SHOWPLAN 选项替换为 SHOWPLAN_ALL 和 SHOWPLAN_TEXT，后两个选项均将输出作为结果集（而不是一组消息）返回。  
  
 STATISTICS TIME 的每一行均按以下格式返回：  
  
```  
szSqlState="01000", *pfNativeError= 3613,  
szErrorMsg="[Microsoft][SQL Server Native Client][SQL Server]  
   SQL Server Parse and Compile Time: cpu time = 0 ms."  
```  
  
 在到达结果集的末尾之前，SET STATISTICS IO 的输出不可用。 若要获取统计信息 IO 输出，应用程序会在 **SQLFetch** 或 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)返回 SQL_NO_DATA 时调用 **SQLGetDiagRec** 。 STATISTICS IO 的输出按以下格式返回：  
  
```  
szSqlState="01000", *pfNativeError= 3615,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Table: testshow  scan count 1,  logical reads: 1,  
   physical reads: 0."  
```  
  
## <a name="using-dbcc-statements"></a>使用 DBCC 语句  
 DBCC 语句将其数据作为消息（而不是结果集）返回。 **SQLExecDirect** 或 **SQLExecute** 返回 SQL_SUCCESS_WITH_INFO，应用程序将通过调用 **SQLGetDiagRec** 来检索输出，直到返回 SQL_NO_DATA。  
  
 例如，下面的语句返回 SQL_SUCCESS_WITH_INFO：  
  
```  
SQLExecDirect(hstmt, "DBCC CHECKTABLE(Authors)", SQL_NTS);  
```  
  
 对 **SQLGetDiagRec** 的调用返回：  
  
```  
szSqlState = "01000", *pfNativeError = 2536,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Checking authors"  
szSqlState = "01000", *pfNativeError = 2579,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   The total number of data pages in this table is 1."  
szSqlState = "01000", *pfNativeError = 7929,  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   Table has 23 data rows."  
szSqlState = "01000", *pfNativeError = 2528  
szErrorMsg="[Microsoft][ SQL Server Native Client][SQL Server]  
   DBCC execution completed. If DBCC printed error messages,  
   see your System Administrator."  
```  
  
## <a name="using-print-and-raiserror-statements"></a>使用 PRINT 和 RAISERROR 语句  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] PRINT 和 RAISERROR 语句还通过调用 **SQLGetDiagRec** 返回数据。 PRINT 语句使 SQL 语句执行返回 SQL_SUCCESS_WITH_INFO，并对 **SQLGetDiagRec** 的后续调用返回 *SQLState* 01000。 严重性为 10 或更低的 RAISERROR 在行为上与 PRINT 相同。 严重性为11或更高的 RAISERROR 导致 execute 返回 SQL_ERROR，对 **SQLGetDiagRec** 的后续调用会返回 *SQLState* 42000。 例如，下面的语句返回 SQL_SUCCESS_WITH_INFO：  
  
```  
SQLExecDirect (hstmt, "PRINT  'Some message' ", SQL_NTS);  
```  
  
 调用 **SQLGetDiagRec** 将返回：  
  
```  
szSQLState = "01000", *pfNative Error = 0,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Some message"  
```  
  
 下面的语句返回 SQL_SUCCESS_WITH_INFO：  
  
```  
SQLExecDirect (hstmt, "RAISERROR ('Sample error 1.', 10, -1)",  
   SQL_NTS)  
```  
  
 调用 **SQLGetDiagRec** 将返回：  
  
```  
szSQLState = "01000", *pfNative Error = 50000,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Sample error 1."  
```  
  
 下面的语句返回 SQL_ERROR：  
  
```  
SQLExecDirect (hstmt, "RAISERROR ('Sample error 2.', 11, -1)", SQL_NTS)  
```  
  
 调用 **SQLGetDiagRec** 将返回：  
  
```  
szSQLState = "42000", *pfNative Error = 50000,  
szErrorMsg= "[Microsoft] [SQL Server Native Client][SQL Server]  
   Sample error 2."  
```  
  
 在结果集中包含 PRINT 或 RAISERROR 语句的输出时，调用 **SQLGetDiagRec** 的时间是至关重要的。 若 **要检索** PRINT 或 RAISERROR 输出，必须在接收 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO 的语句之后立即调用。 当仅执行一个 SQL 语句时（如上述示例中所示），这非常简单。 在这些情况下，对 **SQLExecDirect** 或 **SQLExecute** 的调用将返回 SQL_ERROR 或 SQL_SUCCESS_WITH_INFO，然后可以调用 **SQLGetDiagRec** 。 当编写循环代码以处理一批 SQL 语句的输出或者当执行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程时，这要复杂一些。  
  
 在这种情况下，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 为在批处理或存储过程中执行的每个 SELECT 语句返回一个结果集。 如果批处理或过程包含 PRINT 或 RAISERROR 语句，它们的输出将与 SELECT 语句结果集交叉。 如果批处理或过程中的第一条语句是 PRINT 或 RAISERROR，则 **SQLExecute** 或 **SQLExecDirect** 返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR，应用程序需要调用 **SQLGetDiagRec** ，直到它返回 SQL_NO_DATA 检索打印或 RAISERROR 信息。  
  
 如果 PRINT 或 RAISERROR 语句出现在 (如 SELECT 语句) 的 SQL 语句之后，则在包含错误的结果集上 [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md)位置时，将返回 PRINT 或 RAISERROR 信息。 **SQLMoreResults** 根据消息的严重性返回 SQL_SUCCESS_WITH_INFO 或 SQL_ERROR。 消息是通过调用 **SQLGetDiagRec** 来检索的，直到返回 SQL_NO_DATA。  
  
## <a name="see-also"></a>另请参阅  
 [处理错误和消息](../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
  
  
