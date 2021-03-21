---
description: bcp_control
title: bcp_control |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_control
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_control function
ms.assetid: 32187282-1385-4c52-9134-09f061eb44f5
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 335dd32c4d3ba62c9aae09cc38adb4383d1f71fd
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104749577"
---
# <a name="bcp_control"></a>bcp_control
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  更改用于在文件和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之间进行大容量复制的各种控制参数的默认设置。  
  
## <a name="syntax"></a>语法  
  
```  
  
RETCODE bcp_control (  
        HDBC hdbc,  
        INT eOption,  
        void* iValue);  
```  
  
## <a name="arguments"></a>参数  
 *hdbc*  
 是启用大容量复制的 ODBC 连接句柄。  
  
 *eOption*  
 可以是下列值之一：  
  
 BCPABORT  
 停止正在进行的大容量复制操作。 从另一个线程调用 *eOption* 为 BCPABORT 的 **bcp_control** ，停止正在运行的大容量复制操作。 忽略 *iValue* 参数。  
  
 BCPBATCH  
 每批的行数。 默认值为 0，当提取数据时，该默认值表示表中的所有行；将数据复制到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 时，则表示用户数据文件中的所有行。 值小于 1 则将 BCPBATCH 重置为默认值。  
  
 BCPDELAYREADFMT  
 如果设置为 true，则布尔值将导致 [bcp_readfmt](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-readfmt.md) 在执行时读取。 如果为 false (默认) ，bcp_readfmt 将立即读取格式化文件。 如果 BCPDELAYREADFMT 为 true 并且您调用 bcp_columns 或 bcp_setcolfmt，则会发生序列错误。  
  
 如果在 `bcp_control(hdbc,` `, (void *)FALSE)` 调用 `bcp_control(hdbc,` BCPDELAYREADFMT 和 bcp_writefmt 后调用 BCPDELAYREADFMT `, (void *)TRUE)` ，也会发生序列错误。  
  
 有关详细信息，请参阅[元数据发现](../../relational-databases/native-client/features/metadata-discovery.md)。  
  
 BCPFILECP  
 *iValue* 包含数据文件的代码页的编号。 可以指定代码页的标号，例如 1252 或 850，或者采用以下值之一：  
  
 BCPFILE_ACP：文件中的数据位于客户端的 Microsoft Windows® 代码页中。  
  
 BCPFILE_OEMCP：文件中的数据位于客户端的 OEM 代码页中（默认值）。  
  
 BCPFILE_RAW：文件中的数据位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的代码页中。  
  
 BCPFILEFMT  
 数据文件格式的版本号。 这可能是 80 ( [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]) 、90 ( [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]) 、100 ( [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 或 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]) 、110 ( [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]) 或 120 () [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] 。 120 是默认值。 对于采用服务器早期版本所支持的格式的数据，该选项对导出和导入这样的数据非常有用。 例如，若要将从服务器中的文本列获取的数据导入 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 到或更高版本的服务器中的 **varchar (max)** 列 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，则应指定80。 同样，如果在从 **varchar (max)** 列导出数据时指定80，则将像保存文本列那样保存它 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ，并且可以将其导入到服务器的文本列 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。  
  
 BCPFIRST  
 要复制的文件或表的第一行数据。 默认值为 1；值小于 1 则将此选项重置为其默认值。  
  
 BCPFIRSTEX  
 对于 BCP out 操作，指定要复制到数据文件的数据库表的第一行。  
  
 对于 BCP in 操作，指定要复制到数据库表的数据文件的第一行。  
  
 *IValue* 参数应为包含值的已签名64位整数的地址。 可传递给 BCPFIRSTEX 的最大值为 2 ^ 63-1。  
  
 BCPFMTXML  
 指定生成的格式化文件应采用 XML 格式。 默认情况下将禁用此选项。  
  
 XML 格式化文件提供了更大的灵活性，但有一些限制。 例如，不能同时为字段指定前缀和终止符，这是在较旧的格式化文件中。  
  
> [!NOTE]  
>  只有当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 一起安装后，才支持 XML 格式化文件。  
  
 BCPHINTS  
 *iValue* 包含一个 SQLTCHAR 字符串指针。 寻址的字符串指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 大容量复制处理提示或返回结果集的 Transact-SQL 语句。 如果指定的 Transact-SQL 语句返回多个结果集，则忽略第一个结果集之后的所有结果集。 有关大容量复制处理提示的详细信息，请参阅 [Bcp 实用工具](../../tools/bcp-utility.md)。  
  
 BCPKEEPIDENTITY  
 当 *iValue* 为 TRUE 时，指定大容量复制函数插入为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用标识约束定义的列提供的数据值。 输入文件必须提供标识列的值。 如果未进行设置，则为插入的行生成新标识值。 忽略文件的标识列中所存在的任何数据。  
  
 BCPKEEPNULLS  
 指定是否会将文件中的空数据值转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的 NULL 值。 当 *iValue* 为 TRUE 时，空值将转换为表中的 NULL 值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 默认情况下会将空值转换为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中的列的默认值（如果存在默认值）。  
  
 BCPLAST  
 要复制的最后一行。 默认值为复制所有行；值小于 1 则将此选项重置为其默认值。  
  
 BCPLASTEX  
 对于 BCP out 操作，指定要复制到数据文件的数据库表的最后一行。  
  
 对于 BCP in 操作，指定要复制到数据库表的数据文件的最后一行。  
  
 *IValue* 参数应为包含值的已签名64位整数的地址。 可以传递到 BCPLASTEX 的最大值为 2^63-1。  
  
 BCPMAXERRS  
 大容量复制操作失败之前允许的错误数。 默认值为 10;如果值小于1，则将此选项重置为其默认值。 大容量复制将最大错误数限制为 65,535 个。 如果尝试将该选项设置为大于 65,535 的值，将导致该选项设置为 65,535。  
  
 BCPODBC  
 如果为 TRUE，则指定以字符格式保存的 **datetime** 和 **smalldatetime** 值将使用 ODBC 时间戳转义序列前缀和后缀。 BCPODBC 选项仅适用于 DB_OUT。  
  
 为 FALSE 时，表示1997年1月1日的 **日期时间** 值将转换为字符串： 1997-01-01 00：00：00.000。 如果为 TRUE，则将相同的 **日期时间** 值表示为： {ts ' 1997-01-01 00：00： 00.000 '}。  
  
 BCPROWCOUNT  
 返回当前（或上一次）BCP 操作所影响的行数。  
  
 BCPTEXTFILE  
 如果为 TRUE，则指定数据文件为文本文件，而非二进制文件。 如果该文件为文本文件，BCP 将通过检查数据文件的前两个字节中的 Unicode 字节标记来确定该文件是否是 Unicode 文件。  
  
 BCPUNICODEFILE  
 如果为 TRUE，则指定输入文件是 Unicode 文件。  
  
 *iValue*  
 指定的 *eOption* 的值。 *iValue* 是一个整数 (LONGLONG) 值强制转换为 void 指针，以允许将来扩展到64位值。  
  
## <a name="returns"></a>返回  
 SUCCEED 或 FAIL。  
  
## <a name="remarks"></a>备注  
 此函数设置用于大容量复制操作的各种控制参数，其中包括取消大容量复制之前允许的错误数、要从数据文件中复制的第一批和最后一批的行数和批大小。  
  
 从 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 大容量复制 SELECT 的结果集时，此函数还可用于指定 SELECT 语句。 将 *eOption* 设置为 BCPHINTS，并将 *iValue* 设置为具有指向包含 SELECT 语句的 SQLTCHAR 字符串的指针。  
  
 仅当在用户文件和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表之间进行复制时，这些控制参数才有意义。 控制参数设置对使用 bcp_sendrow 复制到的行没有影响 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-sendrow.md)  
  
## <a name="example"></a>示例  
  
```  
// Variables like henv not specified.  
SQLHDBC      hdbc;  
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
if (bcp_init(hdbc, _T("address"), _T("address.add"), _T("addr.err"),  
   DB_IN) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set the number of rows per batch.   
if (bcp_control(hdbc, BCPBATCH, (void*) 1000) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set file column count.   
if (bcp_columns(hdbc, 1) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Set the file format.   
if (bcp_colfmt(hdbc, 1, 0, 0, SQL_VARLEN_DATA, '\n', 1, 1)  
   == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Execute the bulk copy.   
if (bcp_exec(hdbc, &nRowsProcessed) == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
printf_s("%ld rows processed by bulk copy.", nRowsProcessed);  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [大容量复制函数](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
