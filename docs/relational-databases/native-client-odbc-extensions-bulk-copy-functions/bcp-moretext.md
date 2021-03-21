---
description: bcp_moretext
title: bcp_moretext |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_moretext
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_moretext function
ms.assetid: 23e98015-a8e4-4434-9b3f-9c7350cf965f
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 70a364306f2498e6a9f504bcbb2d1c119c746d9d
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749527"
---
# <a name="bcp_moretext"></a>bcp_moretext
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  将部分较长的可变长度数据类型值发送到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_moretext (  
        HDBC hdbc,  
        DBINT cbData,  
        LPCBYTE pData);  
```  
  
## <a name="arguments"></a>参数  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *cbData*  
 要从 *pData* 引用的数据复制到 SQL Server 的数据字节数。 SQL_NULL_DATA 的值指示为 NULL。  
  
 *pData*  
 指向要发送至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的受支持的较长可变长度数据块区的指针。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 此函数可与 [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) 和 [bcp_sendrow](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md) 结合使用，以便将长度可变的长数据值复制到多个较小的块区中 SQL Server。 **bcp_moretext** 可用于具有以下 SQL Server 数据类型的列： **text**、 **ntext**、 **image**、 **varchar (max)**、 **nvarchar (max)**、 **varbinary (max)**、用户定义类型 (UDT) 和 XML。 **bcp_moretext** 不支持数据转换，则提供的数据必须与目标列的数据类型匹配。  
  
 如果使用 **bcp_moretext** 支持的数据类型的非 NULL *pData* 参数调用 **bcp_bind** ， **bcp_sendrow** 将发送整个数据值，而不考虑长度。 但是，如果 **bcp_bind** 具有用于支持的数据类型的 NULL *pData* 参数，则 **bcp_moretext** 可用于在成功返回后立即从 **bcp_sendrow** 中复制数据，指示已处理包含数据的所有绑定列。  
  
 如果使用 **bcp_moretext** 在一行中发送一种受支持的数据类型列，则还必须使用它来发送该行中所有其他支持的数据类型列。 不可以跳过任何列。 支持的数据类型为 SQLTEXT、SQLNTEXT、SQLIMAGE、SQLUDT 和 SQLXML。 如果列分别为 varchar(max)、nvarchar(max) 或 varbinary(max)，则 SQLCHARACTER、SQLVARCHAR、SQNCHAR、SQLBINARY 和 SQLVARBINARY 也属于此类别。  
  
 调用 **bcp_bind** 或 [bcp_collen](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-collen.md) 会设置要复制到 SQL Server 列的所有数据部分的总长度。 尝试发送的字节数 SQL Server 超出 **bcp_bind** 或 **bcp_collen** 调用中指定的字节数。 例如，如果应用程序使用 **bcp_collen** 将 SQL Server **文本** 列的可用数据的长度设置为4500，则调用此错误 **bcp_moretext** 5 次，同时指示每次调用数据缓冲区的长度为1000字节长。  
  
 如果复制的行包含多个长、可变长度的列， **bcp_moretext** 首先会将其数据发送到最低的按序号编号列，后跟下一个最低的按序号编号列，依此类推。 正确设置所需的数据的总长度很重要。 无法在长度设置之外发出已通过大容量复制接收某列的所有数据的信号。  
  
 当使用 bcp_sendrow 和 bcp_moretext 将 **(的最大)** 值发送到服务器时，不需要调用 bcp_collen 来设置列长度。 相反，对于这些类型，将通过调用长度为零的 bcp_sendrow 终止值。  
  
 应用程序通常会在循环中调用 **bcp_sendrow** 和 **bcp_moretext** 以发送多行数据。 下面概述了如何为包含两个 **文本** 列的表执行此操作：  
  
```  
while (there are still rows to send)  
{  
bcp_sendrow(...);  
  
for (all the data in the first varbinary(max) column)  
bcp_moretext(...);  
bcp_moretext(hdbc, 0, NULL);  
  
for (all the data in the second varbinary(max) column)  
bcp_moretext(...);  
bcp_moretext(hdbc, 0, NULL);  
  
}  
  
```  
  
## <a name="example"></a>示例  
 此示例演示如何将 **bcp_moretext** 与 **bcp_bind** 和 **bcp_sendrow** 结合使用：  
  
```  
// Variables like henv not specified.  
HDBC      hdbc;  
DBINT      idRow = 5;  
char*      pPart1 = "This text value isn't very long,";  
char*      pPart2 = " but it's broken into three parts";  
char*      pPart3 = " anyhow.";  
DBINT      cbAllParts;  
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
if (bcp_init(hdbc, "comdb..articles", NULL, NULL, DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Bind program variables to table columns.   
if (bcp_bind(hdbc, (LPCBYTE) &idRow, 0, SQL_VARLEN_DATA, NULL, 0,  
   SQLINT4, 1)    == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
cbAllParts = (DBINT) (strnlen(pPart1, sizeof(pPart1) + 1) + strnlen(pPart2, sizeof(pPart2) + 1) + strnlen(pPart3, sizeof(pPart3) + 1));  
if (bcp_bind(hdbc, NULL, 0, cbAllParts, NULL, 0, SQLTEXT, 2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Send this row, with the text value broken into three chunks.   
if (bcp_sendrow(hdbc) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart1, sizeof(pPart1) + 1), pPart1) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart2, sizeof(pPart2) + 1), pPart2) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
if (bcp_moretext(hdbc, (DBINT) strnlen(pPart3, sizeof(pPart3) + 1), pPart3) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// All done. Get the number of rows processed (should be one).  
nRowsProcessed = bcp_done(hdbc);  
  
// Carry on.  
```  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
