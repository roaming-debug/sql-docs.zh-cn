---
description: bcp_writefmt
title: bcp_writefmt |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_writefmt
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_writefmt function
ms.assetid: cb4c1d37-667d-4bcd-b13c-eb638bcc9b69
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0f1288057763d60154016524dcd62302498b8c71
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483278"
---
# <a name="bcp_writefmt"></a>bcp_writefmt
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  创建一个格式化文件，它包含对当前大容量复制数据文件的格式的说明。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_writefmt (  
        HDBC hdbc,  
        LPCTSTR szFormatFile);  
```  
  
## <a name="arguments"></a>自变量  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *szFormatFile*  
 接收数据文件格式值的用户文件的路径和文件名。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 格式化文件指定大容量复制所创建的数据文件的数据格式。 对 [bcp_columns](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-columns.md) 和 [bcp_colfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-colfmt.md) 的调用定义数据文件的格式。 **bcp_writefmt** 在 *szformatfile 所* 引用的文件中保存此定义。 有关详细信息，请参阅 [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md)。  
  
 有关 **bcp** 数据格式文件的结构的详细信息，请参阅 [使用 Bcp 实用工具导入和导出大容量数据 &#40;SQL Server&#41;](../../relational-databases/import-export/import-and-export-bulk-data-by-using-the-bcp-utility-sql-server.md)。  
  
 若要加载已保存的格式化文件，请使用 [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md)。  
  
> [!NOTE]  
>  仅在版本7.0 和更高版本分发的 **bcp** 实用工具版本中支持 **bcp_writefmt** 生成的格式化文件 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
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
   _T("myErrors"),    DB_OUT) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
if (bcp_columns(hdbc, 3) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
bcp_colfmt(hdbc, 1, SQLCHARACTER, 0, SQL_VARLEN_DATA, '\t', 1, 1);  
bcp_colfmt(hdbc, 2, SQLCHARACTER, 0, SQL_VARLEN_DATA, '\t', 1, 2);  
bcp_colfmt(hdbc, 3, SQLCHARACTER, 0, SQL_VARLEN_DATA, '\t', 1, 3);  
  
if (bcp_writefmt(hdbc, _T("myFmtFile.fmt")) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
if (bcp_exec(hdbc, &nRowsProcessed) == SUCCEED)  
   {  
   printf_s("%ld rows copied from SQL Server\n", nRowsProcessed);  
   }  
  
// Carry on.  
```  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
