---
description: bcp_readfmt
title: bcp_readfmt |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_readfmt
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_readfmt function
ms.assetid: 654001c8-ae9f-425c-b820-f0191bf89367
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1d365cf2e3120e1156267a49f0aec1607ce1efa3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483318"
---
# <a name="bcp_readfmt"></a>bcp_readfmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  从指定的格式文件中读取数据文件格式定义。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_readfmt (  
        HDBC hdbc,  
        LPCTSTR szFormatFile);  
```  
  
## <a name="arguments"></a>自变量  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *szFormatFile*  
 包含数据文件的格式值的文件的路径和文件名。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 **Bcp_readfmt** 读取格式值后，它将对 [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md)和 [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md)进行适当的调用。 您无需分析格式文件和进行这些调用。  
  
 若要保存格式化文件，请调用 [bcp_writefmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-writefmt.md)。 对 **bcp_readfmt** 的调用可以引用保存的格式。 有关详细信息，请参阅 [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md)。  
  
 此外，大容量复制实用工具 (**bcp**) 可以将用户定义的数据格式保存在可由 **bcp_readfmt** 引用的文件中。 有关 **bcp** 实用工具和 **bcp** 数据格式文件的结构的详细信息，请参阅 [批量导入和导出数据 &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)。  
  
 [Bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)的 *EOption* 参数的 **BCPDELAYREADFMT** 值会修改 bcp_readfmt 的行为。  
  
> [!NOTE]  
>  格式化文件必须由 **bcp** 实用工具版本4.2 或更高版本生成。  
  
## <a name="example"></a>示例  
  
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
if (bcp_init(hdbc, _T("myTable"), _T("myData.csv"),  
   _T("myErrors"),    DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
if (bcp_readfmt(hdbc, _T("myFmtFile.fmt")) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
if (bcp_exec(hdbc, &nRowsProcessed) == SUCCEED)  
   {  
   cout << nRowsProcessed << " rows copied to SQL Server\n";  
   }  
  
// Carry on.  
```  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
