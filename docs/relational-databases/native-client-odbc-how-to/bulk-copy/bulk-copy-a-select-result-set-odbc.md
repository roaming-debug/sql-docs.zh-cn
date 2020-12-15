---
description: 大容量复制 SELECT 结果集 (ODBC)
title: 大容量复制 SELECT 结果集 (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- bulk copy [ODBC]
- bulk copy [ODBC], data files
- bulk copy [ODBC], about bulk copy
ms.assetid: 63d5a87b-4d5f-449b-8c77-9f9cc6b190d4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ad1dcb1db7fd976bd9f3ab8e640551afe2427372
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483248"
---
# <a name="bulk-copy-a-select-result-set-odbc"></a>大容量复制 SELECT 结果集 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  此示例显示如何使用大容量复制函数向外大容量复制 SELECT 语句的结果集。 此示例是面向 ODBC 3.0 版或更高版本开发的。  
  
> [!IMPORTANT]  
>  请尽可能使用 Windows 身份验证。 如果 Windows 身份验证不可用，请在运行时提示用户输入其凭据。 不要将凭据存储在一个文件中。 如果必须保存凭据，应当用 [Win32 crypto API](/windows/win32/seccrypto/cryptography-reference)（Win32 加密 API）加密它们。  
  
### <a name="to-bulk-copy-out-the-result-set-of-a-select-statement"></a>向外大容量复制 SELECT 语句的结果集  
  
1.  分配环境句柄和连接句柄。  
  
2.  设置 SQL_COPT_SS_BCP 和 SQL_BCP_ON 以启用大容量复制操作。  
  
3.  连接到 SQL Server。  
  
4.  调用 [bcp_init](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md) 以设置以下信息：  
  
    -   对于 *szTable* 参数，请指定 NULL。  
  
    -   接收结果集数据的数据文件的名称。  
  
    -   接收任何大容量复制错误消息的数据文件的名称（如果您不需要消息文件，请指定 NULL）。  
  
    -   复制方向：DB_OUT。  
  
5.  调用 [bcp_control](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)，将 eOption 设置为 BCPHINTS，并在 iValue 中放置一个指向包含 SELECT 语句的 SQLTCHAR 数组的指针。  
  
6.  调用 [bcp_exec](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-exec.md) 执行大容量复制操作。  

 按以上步骤操作时文件是以本机格式创建的。 您可以使用 [bcp_colfmt](../../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)将数据值转换为其他数据类型。 有关详细信息，请参阅 [创建大容量复制格式化文件 &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/bulk-copy/create-a-bulk-copy-format-file-odbc.md)。  
  
## <a name="example"></a>示例  
 需要一个名为 AdventureWorks 的 ODBC 数据源，其默认数据库是 AdventureWorks 示例数据库。  (可以从 [Microsoft SQL Server 示例和社区项目](https://go.microsoft.com/fwlink/?LinkID=85384) 主页下载 AdventureWorks 示例数据库。 ) 此数据源必须基于操作系统提供的 ODBC 驱动程序， (驱动程序名称为 "SQL Server" ) 。 如果您要将此示例构建为在 64 位操作系统上运行的 32 位应用程序并运行该示例，则必须使用 %windir%\SysWOW64\odbcad32.exe 中的 ODBC 管理器创建 ODBC 数据源。  
  
 此示例连接到您的计算机上默认的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例。 若要连接到命名实例，请更改 ODBC 数据源的定义以使用以下格式指定实例：server\namedinstance。 默认情况下，[!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)] 将安装在命名实例中。  
  
 使用 odbc32.lib 和 odbcbcp.lib 进行编译。  
  
```  
// compile with: odbc32.lib odbcbcp.lib  
#include <stdio.h>  
#include <string.h>  
#include <windows.h>  
#include <sql.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define MAXBUFLEN 256  
  
SQLHENV henv = SQL_NULL_HENV;  
HDBC hdbc1 = SQL_NULL_HDBC;  
  
void Cleanup() {  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
int main() {  
   RETCODE retcode;  
  
   // Bulk copy variables.  
   SDWORD cRows;  
   SQLTCHAR szBCPQuery[] = "SELECT BirthDate FROM HumanResources.Employee";  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle, set bulk copy mode, and then connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   retcode = SQLSetConnectAttr(hdbc1, SQL_COPT_SS_BCP, (void *)SQL_BCP_ON, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetConnectAttr(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample use Integrated Security, create SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"AdventureWorks", SQL_NTS, (UCHAR*)"", SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Initialize the bulk copy.  
   retcode = bcp_init(hdbc1, NULL, "BCPODBC.bcp", "BCPERROR.out", DB_OUT);  
   // The test is for the bulk copy return of SUCCEED, not the ODBC return of SQL_SUCCESS.  
   if ( (retcode != SUCCEED) ) {  
      printf("bcp_init(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Specify the query to use.  
   retcode = bcp_control(hdbc1, BCPHINTS, (void *)szBCPQuery);  
   if ( (retcode != SUCCEED) ) {  
      printf("bcp_control(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Execute the bulk copy.  
   retcode = bcp_exec(hdbc1, &cRows);  
   if ( (retcode != SUCCEED) ) {  
      printf("bcp_exec(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   printf("Number of rows bulk copied out = %d.\n", cRows);  
  
   // Cleanup  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server ODBC 驱动程序的大容量复制操作指南主题 &#40;ODBC&#41;](../../../relational-databases/native-client-odbc-how-to/bulk-copy/bulk-copying-with-the-sql-server-odbc-driver-how-to-topics-odbc.md)  
  
