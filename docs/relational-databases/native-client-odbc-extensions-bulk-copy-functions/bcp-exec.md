---
description: bcp_exec
title: bcp_exec |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_exec
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_exec function
ms.assetid: b23ea2cc-8545-4873-b0c1-57e76b0a3a7b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06e9cdecd37e5fc1f78de6726384715ad7b8c9bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97406726"
---
# <a name="bcp_exec"></a>bcp_exec
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  执行数据库表和用户文件之间数据的完整大容量复制。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_exec (  
        HDBC hdbc,  
        LPDBINT pnRowsProcessed);  
```  
  
## <a name="arguments"></a>自变量  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *pnRowsProcessed*  
 指向 DBINT 的指针。 **Bcp_exec** 函数用成功复制的行数填充此 DBINT。 如果 *pnRowsProcessed* 为 NULL，则 **bcp_exec** 将忽略它。  
  
## <a name="returns"></a>返回  
 SUCCEED、SUCCEED_ASYNC 或 FAIL。 如果复制所有行， **bcp_exec** 函数将返回成功。 如果异步大容量复制操作仍未完成， **bcp_exec** 将返回 SUCCEED_ASYNC。 如果发生了完全失败，或者如果生成错误的行数达到使用 [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)为 BCPMAXERRS 指定的值，则 **bcp_exec** 返回失败。 BCPMAXERRS 默认为 10。 BCPMAXERRS 选项只影响从数据文件读取行（并且不是已发送到服务器的行）时提供程序检测到的语法错误。 服务器在检测到某一行有错误时将中止批处理。 检查 *pnRowsProcessed* 参数是否已成功复制的行数。  
  
## <a name="remarks"></a>备注  
 此函数将数据从用户文件复制到数据库表，或从用户文件复制到数据库表，具体取决于 [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md)中的 *eDirection* 参数的值。  
  
 调用 **bcp_exec** 之前，请使用有效的用户文件名调用 **bcp_init** 。 如果没有这样做，会导致错误。  
  
 **bcp_exec** 是在任意时间长度中可能未完成的唯一大容量复制函数。 因此，它是支持异步模式的唯一大容量复制函数。 若要设置异步模式，请在调用 **bcp_exec** 之前，使用 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md)将 SQL_ATTR_ASYNC_ENABLE 设置为 SQL_ASYNC_ENABLE_ON。 若要测试完成，请调用具有相同参数的 **bcp_exec** 。 如果大容量复制尚未完成， **bcp_exec** 将返回 SUCCEED_ASYNC。 它还会在 *pnRowsProcessed* 中返回已发送到服务器的行数的状态计数。 发送到服务器的行直到到达批的末尾时才会提交。  
  
 有关从开始大容量复制的重大更改的信息 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，请参阅 [&#40;ODBC&#41;执行大容量复制操作 ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)。  
  
## <a name="example"></a>示例  
 下面的示例演示如何使用 **bcp_exec**：  
  
```  
// Variables like henv not specified.  
HDBC      hdbc;  
DBINT      nRowsProcessed;  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...   
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source, return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bulk copy.   
if (bcp_init(hdbc, _T("pubs..authors"), _T("authors.sav"), NULL, DB_OUT)  
   == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Now, execute the bulk copy.   
if (bcp_exec(hdbc, &nRowsProcessed) == FAIL)  
   {  
   if (nRowsProcessed == -1)  
      {  
      printf_s("No rows processed on bulk copy execution.\n");  
      }  
   else  
      {  
      printf_s("Incomplete bulk copy.   Only %ld row%s copied.\n",  
         nRowsProcessed, (nRowsProcessed == 1) ? "": "s");  
      }  
   return;  
   }  
  
printf_s("%ld rows processed.\n", nRowsProcessed);  
  
// Carry on.  
```  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
